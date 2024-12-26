# Notes
## Third Iteration: Add third level of composition
In this iteration, we enhance the data model by introducing a **Criticality Levels** table and integrating it with the product data. Additionally, we update the Metadata Extension and projections to support new functionality, including criticality handling and UI improvements.

---
### Key Objectives:
1. Create a new **Orders Table** (**[Z##_D_ORDER_####](./00_tables.md#z##_d_order_)**) to store orders.
2. Develop a **CDS View Entity** (**[Z##_I_ORDER_####](./01_cds.md#z##_i_order_)**) for accessing the orders.
2. Develop a **CDS Projection** (**[Z##_C_ORDER_####](./02_cds.md#z##_c_order_)**) for accessing the orders.
3. Integrate the order data into the market entity.
5. Enhance Metadata Extensions.

---
### Pay Attention:
1. **Multilevel Composition**:
  - **Any** child entity should have assotiation to **root entity**. For example, **Orders** is **third** level and **have** assotiation to **Market**, but **Market** isn't **root** and for correct work of **Behavior Implementation** you should add assotiation to **Product**, **root** of owr **Busines Object**.
  ```ABAP
  define view entity ZOK_I_ORDER_0001
    as select from zok_d_order_0001

    association to parent ZOK_I_MARKET_0001 as _Market on $projection.ProdUuid = _Market.ProdUuid and $projection.MrktUuid = _Market.MrktUuid

    association [1..1] to ZOK_I_PRODUCT_0001 as _Product
      on $projection.ProdUuid = _Product.ProdUuid

{
    " Part of code was skipped
      /*Association*/
      _Market,
      _Product
}
  ```
  ```ABAP
  define view entity ZOK_C_ORDER_0001
  as projection on ZOK_I_ORDER_0001

{
    " Part of code was skipped
      /* Associations */
      _Market : redirected to parent ZOK_C_MARKET_0001,

      _Product : redirected to ZOK_C_PRODUCT_0001
}
  ```
  - Check, that all used assotiation define in BDEF for every entity. Root entity have assotiation to second level only, but other entity have assotiation to every used for navigatio level.
  ```ABAP
  managed implementation in class zbp_ok_i_product_#### unique;
  strict ( 2 );

  define behavior for Z##_I_PRODUCT_#### alias Product
  persistent table zok_d_prod_####
  lock master
  authorization master ( instance )
  etag master LocalChangedTime
  {
    " Part of code was skipped
    association _Market { create; }
    " Part of code was skipped
  }

  define behavior for Z##_I_MARKET_#### alias Market
  persistent table zok_d_mrkt_####
  lock dependent by _Product
  authorization dependent by _Product
  etag master LocalChangedTime
  {
    " Part of code was skipped
    association _Product;
    association _Orders { create; }
    " Part of code was skipped
  }

  define behavior for Z##_I_ORDER_#### alias Order
  persistent table zok_d_order_####
  lock dependent by _Product
  authorization dependent by _Product
  etag master LocalChangedTime
  {
    " Part of code was skipped
    association _Product;
    association _Market;
    " Part of code was skipped
  }
```

---
### Steps:
1. **[Create the Orders Table](./00_tables.md#z##_d_order_)**:
    - Define the table **[Z##_D_ORDER_####](./00_tables.md#z##_d_order_)** with the necessary fields.
3. **[Develop the Orders View Entity](./01_cds.md#z##_i_market_)**:
    - Create the view entity [Z##_I_ORDERS_####](./01_cds.md#z##_i_market_) to expose the criticality levels.
4. **[Integrate Orders into the Market Entity](./01_cds.md#z##_i_market_)**:
    - Add an **composition** in **[Z##_I_MARKET_####](./01_cds.md#z##_i_market_)** to the **[Z##_I_ORDER_####](./01_cds.md#z##_i_order_)** view.
    - Add an **assotiation to parent** in **[Z##_I_ORDER_####](./01_cds.md#z##_i_order_)** to the **[Z##_I_MARKET_####](./01_cds.md#z##_i_market_)** view.
    - Add a **redirect to child** in the **[Z##_C_MARKET_####](./01_cds.md#z##_i_market_)** to the **[Z##_C_ORDER_####](./01_cds.md#z##_i_order_)** view.
    - Add a **redirect to parent** in the **[Z##_C_ORDER_####](./02_cds.md#z##_c_order_)** to the **[Z##_C_MARKET_####](./02_cds.md#z##_c_market_)** view.
  ```ABAP
  define view entity Z##_I_MARKET_####
    as select from zok_d_mrkt_####

    " Part of code was skipped

    composition [0..*] of Z##_I_ORDER_#### as _Orders

  {
    " Part of code was skipped

        /*association*/
        _Product,
        _Countries,
        _Orders
  }
  ```
  - Activate both **Entity** **together**
5. **[Develop the Orders View Entity Projection](./02_cds.md#z##_i_order_)**:
  - Create the view entity **[Z##_C_ORDERS_####](./02_cds.md#z##_i_order_)** to expose the criticality levels.
6. **[Update the Product Projection](./02_cds.md#z##_c_market_)**:
  - Add an **redirect to child** to the **Order** entity in **[Z##_C_MARKET_####](./02_cds.md#z##_c_market_)** linking it to the orders.
  ```ABAP
  define view entity Z##_C_MARKET_####
    as projection on Z##_I_MARKET_####

  {
    " Part of code was skipped
        /* Associations */
        _Product : redirected to parent Z##_C_PRODUCT_####,
        _Orders : redirected to composition child Z##_C_ORDER_####,

        _Countries
  }
  ```
  - Activate both **projection** **together**
7. **[Add Metadata Extension for Orders](./03_metadata_extension.md#z##_c_market_)**:
  - Create a Metadata Extension for **[Z##_C_ORDER_####](./03_metadata_extension#z##_c_market_)** to configure its UI.
  - Add tab into Market Metadata Extension
  ```ABAP
  @Metadata.layer: #CORE

  annotate entity Z##_C_MARKET_####
      with

  {
    @UI.facet: [ { id: 'idMarket', type: #IDENTIFICATION_REFERENCE, label: 'Market Info', position: 10 },
                { id: 'idOrders',
                  type: #LINEITEM_REFERENCE,
                  label: 'Orders Info',
                  position: 20,
                  targetElement: '_Orders' } ]
    " Part of code was skipped
  }
  ```
8. **[Define the Behavior Definition (BDEF)](./06_behavior_implementation.md#z##_i_product_)**:
  - Define the behavior of the business object as **Managed**.
  - Add Bihavior Implementation Definition for Orders to use BDEF for Markets as template and correct it.
  - Generate class implementation over **Quick Fix** in **ADT**.
9. **[Add expose Z##_UI_PRODUCT_O2_#### for Orders into Service Definition ](./04_service.md)**
10. **Test the Behavior**:
  - Test the implemented behavior in the Fiori application or using the ABAP console.
  - Verify that the operations (Create, Update, Delete) function as expected.

---
### Final Steps:
- Activate all new and updated CDS objects.
- Test the updated service in the Fiori application to ensure the criticality handling works as expected.

---
### Possible Issues and Solutions:
#### 1. **CDS Views/Projectons Not Activating**
  **Problem**:
   - CDS views/projection fail to activate, particularly those with **composition associations**.
   **Solution**:
   - Activate all CDS views connected through composition **together**, especially:
     - When creating a new CDS view with a composition.
     - When modifying an existing composition that introduces or changes associations.
   - Always activate both the root view and any dependent views linked by the composition. Use the activation tool to ensure all dependencies are resolved.
#### 2. **Don't see Order tab**
  **Problem**:
   - On Object Page for Market you don't see Orders table.
   **Solution**:
   - Check Metadata Extension for Orders, there should be annotation for all needed fields.
   - Check Service Definition, there should be exposed Order Project.
#### 3. **Orders entity doesn't have transactional behavior**
  **Problem**:
   - don't see Create/Dlete buttons for Orders entity.
   **Solution**:
   - Check you modificated Interface Behaviour Implementation and Projection Behaviour Implementation exists and all objects are activated witout any error.

---
### Summary:
This iteration enhances the data model by introducing orders, integrating them into market data.