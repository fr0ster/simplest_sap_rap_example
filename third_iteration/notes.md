# Notes
## Third Iteration: Add third level of composition
In this iteration, we enhance the data model by introducing a **Criticality Levels** table and integrating it with the product data. Additionally, we update the Metadata Extension and projections to support new functionality, including criticality handling and UI improvements.

---
### Key Objectives:
1. Create a new **Orders Table** (**[Z##_D_ORDER_####](./00_tables.md#z##_d_order_)**) to store orders.
2. Develop a **CDS View Entity** ([Z##_I_ORDER_####](./01_cds.md#z##_i_order_)) for accessing the orders.
2. Develop a **CDS Projection** ([Z##_C_ORDER_####](./02_cds.md#z##_c_order_)) for accessing the orders.
3. Integrate the order data into the market entity:
  - Add an **composition** in [Z##_I_MARKET_####](./02_cds.md#z##_i_market_) to the [Z##_I_ORDER_####](./01_cds.md#z##_i_order_) view.
  - Add an **assotiation to parent** in [Z##_I_ORDER_####](./02_cds.md#z##_i_order_) to the [Z##_I_MARKET_####](./01_cds.md#z##_i_market_) view.
  - Add a **redirect to child** in the product projection ([Z##_I_MARKET_####](./02_cds.md#z##_i_market_)) to the [Z##_I_ORDER_####](./02_cds.md#z##_i_order_) view.
  - Add a **redirect to parent** in the product projection ([Z##_C_ORDER_####](./02_cds.md#z##_c_order_)) to the [Z##_C_MARKET_####](./02_cds.md#z##_c_market_) view.
5. Enhance Metadata Extensions:
  - Create a **Metadata Extension** for the [Z##_C_ORDER_####](./03_metadata_extension.md#z##_c_order_) entity.
  - Add tab into a **Metadata Extension** for the [Z##_C_MARKET_####](./03_metadata_extension.md#z##_c_market_) entity with association to [Z##_C_ORDER_####](./03_metadata_extension.md#z##_c_order_).

---
### Pay Attention:
1. **Multilevel Composition**:
  - **Any** child entity should have assotiation to **root entity**. For example, **Orders** is **third** level and **have** assotiation to **Market**, but **Market** isn't **root** and for correct work of **Behavior Implementation** you should add assotiation to **Product**, **root** of owr **Busines Object**.
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
1. **[Create the Orders Table](./00_tables.md)**:
  - Define the table `Z##_D_ORDER_####` with the necessary fields.
  - Refer to [00_tables.md](./00_tables.md) for details.
3. **[Develop the Orders View Entity](./01_cds.md)**:
  - Create the view entity `Z##_I_ORDERS_####` to expose the criticality levels.
  - Refer to [01_cds.md](./01_cds.md) for details.
4. **[Integrate Orders into the Market Entity](./02_cds.md)**:
  - Add an **composition** to the **Order** entity in `Z##_I_MARKET_####` linking it to the criticality levels.
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
9. **Test the Behavior**:
  - Test the implemented behavior in the Fiori application or using the ABAP console.
  - Verify that the operations (Create, Update, Delete) and validations/determinations function as expected.
---
### Final Steps:
- Activate all new and updated CDS objects.
- Test the updated service in the Fiori application to ensure the criticality handling works as expected.
---
### Summary:
This iteration enhances the data model by introducing orders, integrating them into market data.