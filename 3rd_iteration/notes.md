# Notes
## Third Iteration: Add Third Level of Composition
In this iteration, we enhance the data model by introducing a **Criticality Levels** table and integrating it with the product data. Additionally, we update the Metadata Extension and projections to support new functionality, including criticality handling and UI improvements.

---

### Key Objectives:
1. Create a new **Orders Table** (**[Z##_D_ORDER_####](./00_tables.md#z##_d_order_)**) to store orders.
2. Develop a **CDS View Entity** (**[Z##_I_ORDER_####](./01_cds.md#z##_i_order_)**) for accessing the orders.
3. Develop a **CDS Projection** (**[Z##_C_ORDER_####](./02_cds.md#z##_c_order_)**) for accessing the orders.
4. Integrate the order data into the market entity.
5. Enhance Metadata Extensions.
6. Modificate **Behafior definition** for **Product BO**

---

### Pay Attention:
1. **Multilevel Composition**:
   - **Any** child entity should have an association to the **root entity**. For example, **Orders** is at the **third** level and has an association to **Market**, but **Market** is not the **root**. For correct functionality of the **Behavior Implementation**, you should add an association to **Product**, the **root** of our **Business Object**.
   ```ABAP
   define view entity Z##_I_ORDER_####
     as select from z##_d_order_####

     association to parent Z##_I_MARKET_#### as _Market on $projection.ProdUuid = _Market.ProdUuid and $projection.MrktUuid = _Market.MrktUuid

     association [1..1] to Z##_I_PRODUCT_#### as _Product
       on $projection.ProdUuid = _Product.ProdUuid

   {
       /* Part of code was skipped */
       _Market,
       _Product
   }
   ```
   ```ABAP
   define view entity Z##_C_ORDER_####
   as projection on Z##_I_ORDER_####

   {
       /* Part of code was skipped */
       _Market : redirected to parent Z##_C_MARKET_####,

       _Product : redirected to Z##_C_PRODUCT_####
   }
   ```
   - Ensure all associations are defined in the BDEF for every entity. The root entity has associations to the second level only, while other entities have associations to every level used for navigation.
   ```ABAP
   managed implementation in class zbp_##_i_product_#### unique;
   strict ( 2 );

   define behavior for Z##_I_PRODUCT_#### alias Product
   persistent table z##_d_prod_####
   lock master
   authorization master ( instance )
   etag master LocalChangedTime
   {
       /* Part of code was skipped */
       association _Market { create; }
       /* Part of code was skipped */
   }

   define behavior for Z##_I_MARKET_#### alias Market
   persistent table z##_d_mrkt_####
   lock dependent by _Product
   authorization dependent by _Product
   etag master LocalChangedTime
   {
       /* Part of code was skipped */
       association _Product;
       association _Orders { create; }
       /* Part of code was skipped */
   }

   define behavior for Z##_I_ORDER_#### alias Order
   persistent table z##_d_order_####
   lock dependent by _Product
   authorization dependent by _Product
   etag master LocalChangedTime
   {
       /* Part of code was skipped */
       association _Product;
       association _Market;
       /* Part of code was skipped */
   }
   ```

---

### Steps:
1. **[Create the Orders Table](./00_tables.md#z##_d_order_)**:
   - Define the table **[Z##_D_ORDER_####](./00_tables.md#z##_d_order_)** with the necessary fields.

2. **[Develop the Orders View Entity](./01_cds.md#z##_i_market_)**:
   - Create the view entity **[Z##_I_ORDERS_####](./01_cds.md#z##_i_market_)** to expose the criticality levels.

3. **[Integrate Orders into the Market Entity](./01_cds.md#z##_i_market_)**:
   - Add a **composition** in **[Z##_I_MARKET_####](./01_cds.md#z##_i_market_)** to the **[Z##_I_ORDER_####](./01_cds.md#z##_i_order_)** view.
   - Add an **association to parent** in **[Z##_I_ORDER_####](./01_cds.md#z##_i_order_)** to the **[Z##_I_MARKET_####](./01_cds.md#z##_i_market_)** view.
   - Add a **redirect to child** in **[Z##_C_MARKET_####](./01_cds.md#z##_i_market_)** to the **[Z##_C_ORDER_####](./01_cds.md#z##_i_order_)** view.
   - Add a **redirect to parent** in **[Z##_C_ORDER_####](./02_cds.md#z##_c_order_)** to the **[Z##_C_MARKET_####](./02_cds.md#z##_c_market_)** view.

4. **[Develop the Orders View Entity Projection](./02_cds.md#z##_i_order_)**:
   - Create the view entity **[Z##_C_ORDERS_####](./02_cds.md#z##_i_order_)** to expose the criticality levels.

5. **[Update the Product Projection](./02_cds.md#z##_c_market_)**:
   - Add a **redirect to child** to the **Order** entity in **[Z##_C_MARKET_####](./02_cds.md#z##_c_market_)** linking it to the orders.

6. **[Add Metadata Extension for Orders](./03_metadata_extension.md#z##_c_market_)**:
   - Create a Metadata Extension for **[Z##_C_ORDER_####](./03_metadata_extension.md#z##_c_market_)** to configure its UI.
   - Add a tab into the Market Metadata Extension.

7. **[Modificate the Behavior Definition (BDEF) Interface](./06_behavior_definition.md#z##_i_product_)**:
   - Define the behavior of the business object as **Managed**.
   - Add a Behavior Implementation Definition for Orders, using the BDEF for Markets as a template and correcting it.
   - Generate the class implementation using **Quick Fix** in **ADT**.

7. **[Modificate the Behavior Definition (BDEF) Transactional Query](./06_behavior_definition.md#z##_c_product_)**:
   - Define the behavior of the business object as **Managed**.
   - Add a Behavior Implementation Definition for Orders, using the BDEF for Markets as a template and correcting it.

8. **[Expose Z##_UI_PRODUCT_O2_#### for Orders in the Service Definition](./04_service.md)**.

9. **Test the Behavior**:
   - Test the implemented behavior in the Fiori application or using the ABAP console.
   - Verify that the operations (Create, Update, Delete) function as expected.

---

### Final Steps:
- Activate all new and updated CDS objects.
- Test the updated service in the Fiori application to ensure the criticality handling works as expected.

---

### Possible Issues and Solutions:
#### 1. **CDS Views/Projections Not Activating**
   **Problem**:
   - CDS views or projections fail to activate, particularly those with **composition associations**.
   **Solution**:
   - Activate all CDS views connected through composition **together**, especially:
     - When creating a new CDS view with a composition.
     - When modifying an existing composition that introduces or changes associations.
   - Always activate both the root view and any dependent views linked by the composition. Use the activation tool to ensure all dependencies are resolved.

#### 2. **Order Tab Not Visible**
   **Problem**:
   - The Orders table is not visible on the Object Page for Market.
   **Solution**:
   - Check the Metadata Extension for Orders, ensuring there are annotations for all necessary fields.
   - Check the Service Definition to ensure the Order Projection is exposed.

#### 3. **Orders Entity Missing Transactional Behavior**
   **Problem**:
   - The Create/Delete buttons are missing for the Orders entity.
   **Solution**:
   - Verify that the Interface Behavior Implementation and Projection Behavior Implementation exist and all objects are activated without errors.

---

### Summary:
This iteration enhances the data model by introducing orders and integrating them into the market data.