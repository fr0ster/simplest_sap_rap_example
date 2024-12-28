# Notes
## Fourth Iteration: Enhancing the Data Model and Metadata Extensions
In this iteration, we enhance the data model by introducing a **Criticality Levels** table and integrating it with the product data. Additionally, we update the Metadata Extension and projections to support new functionality, including criticality handling and UI improvements.

---

### Key Objectives:
1. Create a new **[Criticality Levels Table](./00_tables.md)** to store criticality levels.
2. Develop a **[CDS View Entity](./01_cds.md#z##_i_criticality_levels_)** for accessing the criticality levels.
3. Develop a **[CDS Projection](./02_cds.md#z##_c_criticality_levels_)** for accessing the criticality levels.
4. Integrate the criticality data into the product entity.
5. Update the **Product Projection**.
6. Enhance Metadata Extensions.
7. Create a Service for modifying **Criticality Levels** data.
8. Create **validations** and **determinations** for **CDS Product BO Interface**

---

### Steps:
1. **[Create the Criticality Levels Table](./00_tables.md#z##_d_crtly_)**:
   - Define the table **[Z##_CRTLY_####](./00_tables.md#z##_d_crtly_)** with the necessary fields.

2. **[Update the Class for Data Generation](./05_generator.md#z##_cl_generate_data_)**:
   - Add code into **[Z##_CL_GENERATE_DATA_####](./05_generator.md#z##_cl_generate_data_)** for recreating settings.
   ```ABAP
     DATA lt_critically TYPE TABLE OF z##_d_crtly_####.

     " Price Critically
     " fill internal table (itab)
     lt_critically = VALUE #( id = 'PriceCritically' ( treashhold4 = 700 treashhold3 = 1200 treashhold2 = 1800 treashhold1 = 2000 ) ).

     " Delete the possible entries in the database table - in case it was already filled
     DELETE FROM z##_d_crtly_####1.
     " insert the new table entries
     INSERT z##_d_crtly_#### FROM TABLE @lt_critically.

     " check the result
     SELECT * FROM z##_d_crtly_#### INTO TABLE @lt_critically.
     out->write( sy-dbcnt ).
     out->write( 'critically data inserted successfully!' ).
   ```

3. **[Develop the Criticality Levels View Entity](./01_cds.md#z##_i_criticality_levels_)**:
   - Create the view entity **[Z##_I_CRITICALITY_LEVELS_####](./01_cds.md#z##_i_criticality_levels_)** to expose the criticality levels.

4. **[Integrate Criticality into the Product Entity](./02_cds.md)**:
   - Add an association to the product entity **[Z##_I_PRODUCT_####](./02_cds.md#z##_c_product_)** linking it to the criticality levels.
   - Add a calculated field `PriceCriticality` to evaluate product price against criticality thresholds.

   ```ABAP
   define root view entity Z##_I_PRODUCT_####
   as select from z##_d_prod_####

   " Part of code was skipped
   association to Z##_I_CRITICALITY_LEVELS_#### as _PriceCriticality on _PriceCriticality.Id = 'PriceCritically'

   {
     " Part of code was skipped
         @Semantics.amount.currencyCode: 'PriceCurrency'
         price              as Price,

         case
           when cast(price as abap.dec(15,2)) > _PriceCriticality.Treashhold1 then 1
           when cast(price as abap.dec(15,2)) > _PriceCriticality.Treashhold2 then 2
           when cast(price as abap.dec(15,2)) > _PriceCriticality.Treashhold3 then 3
           when cast(price as abap.dec(15,2)) > _PriceCriticality.Treashhold4 then 4
           else 0
         end                as PriceCriticality,

         price_currency     as PriceCurrency,
     " Part of code was skipped

         /*Associations*/
     " Part of code was skipped
         _PriceCriticality
   }
   ```

5. **[Update the Product Projection](./02_cds.md#z##_c_product_)**:
   - Add the calculated field `PriceCriticality` to the product projection.
   - Update UI annotations to link the criticality logic to the `Price` field.
   ```ABAP
   define root view entity Z##_C_PRODUCT_####
   provider contract transactional_query
   as projection on Z##_I_PRODUCT_####

   {
     " Part of code was skipped
         PriceCriticality,
     " Part of code was skipped
         _PG,
         _PHASE,
         _UOM,
         _PriceCriticality
   }
   ```

6. **[Update the Product Metadata Extension](./03_metadata_extestion.md#z##_c_product_)**:
   - Use the calculated field `PriceCriticality` in Criticality for annotation.
   ```ABAP
   @Metadata.layer: #CORE

   annotate entity Z##_C_PRODUCT_####
      with

   {
      " Part of code was skipped
   @UI.identification: [ { position: 60, label: 'Price', criticality: 'PriceCriticality' } ]
   @UI.lineItem: [ { position: 70, criticality: 'PriceCriticality' } ]
   Price;
      " Part of code was skipped
   }
   ```

7. **[Develop the Criticality Levels View Entity Projection](./02_cds.md#z##_i_criticality_levels_)**:
   - Create the view entity **[Z##_C_CRITICALITY_LEVELS_####](./02_cds.md#z##_i_criticality_levels_)** to expose the criticality levels.

8. **[Add Metadata Extension for Criticality Levels](./03_metadata_extestion.md#z##_i_criticality_levels_)**:
   - Create a Metadata Extension for [Z##_C_CRITICALITY_LEVELS_####](./03_metadata_extestion.md#z##_i_criticality_levels_) to configure its UI.

9. **[Define the Behavior Definition (BDEF)](./06_behavior_implementation.md#z##_i_criticality_levels_)**:
   - Define the behavior of the business object as **Managed**.
   - Enable standard transactional operations.
   - Generate class implementation over **Quick Fix** in **ADT**.

10. **[Define Projection of the Behavior Definition (BDEF)](./06_behavior_implementation.md#z##_i_criticality_levels_)**:
   - Set alias.
   - Refer to **[06_behavior_implementation.md](./06_behavior_implementation.md#z##_i_criticality_levels_)** for details.
   - Generate class implementation over **Quick Fix** in **ADT**.

11. **[Expose Criticality Levels Project as Service](./04_service.md)**:
    - Add expose **[Z##_C_CRITICALITY_LEVELS_####](./04_service.md)** as PriceCritically into Service Definition.

12. **[Create validations and determinations Definition ](./06_behavior_definition.md#z##_i_product_)**
   - Add validation and determination into **[Behaviour Definition for CDS Product BO Interface](./06_behavior_definition.md#z##_i_product_)**
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

      determination setPhaseid on save { create; field Phaseid; }
      determination setPriceCurrency on save { create; field PriceCurrency; }
      determination setHeight on save { create; field Height; }
      determination setDepth on save { create; field Depth; }
      determination setWidth on save { create; field Width; }
      determination setSizeUom on save { create; field SizeUom; }
      validation checkHeight on save { field Height; }
      validation checkDepth on save { field Depth; }
      validation checkWidth on save { field Width; }

       /* Part of code was skipped */
      }

      define behavior for Z##_I_MARKET_#### alias Market
      persistent table z##_d_mrkt_####
      lock dependent by _Product
      authorization dependent by _Product
      etag master LocalChangedTime
      {
       /* Part of code was skipped */
       
      determination setStartdate on save { create; field Startdate; }
      determination setEnddate on save { create; field Enddate; }

       /* Part of code was skipped */
      }

      define behavior for Z##_I_ORDER_#### alias Order
      persistent table z##_d_order_####
      lock dependent by _Product
      authorization dependent by _Product
      etag master LocalChangedTime
      {
       /* Part of code was skipped */

      determination setAmountcurr on save { create; field Amountcurr; }
      validation checkQuantity on save { field Quantity; }
      validation checkNetamount on save { field Netamount; }
      validation checkGrossamount on save { field Grossamount; }

       /* Part of code was skipped */
      }
   ```
   - Create implementation of **validations and determinations** in **[Class Behavior Implementation](./07_behavior_implementation.md#z##_i_product_)**
   - Pay attention, **%state_area** it's name of validation, **%element-Netamount = if_abap_behv=>mk-on** it's checked field.
   ```ABAP
           APPEND VALUE #( %tky        = <ls_orderdate>-%tky
                        %state_area = 'CHECKNETAMOUNT' )
               TO reported-order.

        IF     <ls_orderdate>-Netamount >= 0.
          CONTINUE.
        ENDIF.

        APPEND VALUE #( %tky = <ls_orderdate>-%tky ) TO failed-order.

        APPEND VALUE #( %tky                  = <ls_orderdate>-%tky
                        %state_area           = 'CHECKNETAMOUNT'
                        %msg                  = new_message_with_text( severity = if_abap_behv_message=>severity-error text = 'Netamount less than zero' )
                        %element-Netamount = if_abap_behv=>mk-on )
               TO reported-order.
   ```

---

### Final Steps:
- Activate all new and updated CDS objects.
- Test the updated service in the Fiori application to ensure the criticality handling works as expected.

---

### Possible Issues and Solutions:
#### 1. **Columns Missing in Preview**
   **Problem**:
   - CDS views/projection fail to activate, particularly those with **composition associations**.
   **Solution**:
   - Activate all CDS views connected through composition **together**, especially:
     - When creating a new CDS view with a composition.
     - When modifying an existing composition that introduces or changes associations.
   - Always activate both the root view and any dependent views linked by the composition. Use the activation tool to ensure all dependencies are resolved.

#### 2. **Criticality Service Preview Doesn't Show**
   **Problem**:
   - You see a blank page in the Criticality Levels Service preview.
   **Solution**:
   - Check the Metadata Extension for Orders, ensuring there are annotations for all necessary fields.
   - Check the Service Definition to ensure the Order Projection is exposed.

#### 3. **Orders Entity Missing Transactional Behavior**
   **Problem**:
   - The Create/Delete buttons are missing for the Orders entity.
   **Solution**:
   - Ensure there exists a BDEF for Interface and Projection and class behavior implementation.
   - Verify that the Interface Behavior Implementation and Projection Behavior Implementation exist and all objects are activated without errors.

---

### Summary:
This iteration enhances the data model by introducing criticality levels, integrating them into product data, and updating the UI to reflect criticality-based calculations. The new tab for managing criticality levels provides flexibility for maintaining the criticality values directly in the Fiori application.