# Notes

## Fifth Iteration: Business Model Enhancements

In this iteration, we enhance the business model by introducing **Value Helpers** in **Projection**.

---

### Key Objectives:
1. Create **Projection Transactional Interface**, change projected CDS in **Projection Transactional Query** to new **Projection Transactional Interface**.
2. Add **Value Helpers** into **[Z##_CI_PRODUCT_####](./02_cds.md#z##_ci_product_)**.
3. Add **Value Helpers** into **[Z##_CI_MARKET_####](./02_cds.md#z##_ci_market_)**.
4. Add **Value Helpers** into **[Z##_CI_ORDER_####](./02_cds.md#z##_ci_order_)**.
5. Add **needed fields** into the **SearchField Area** and enable standard search functionality in **[Metadata Extension](03_metadata_extension.md)**.
6. Add **actions**.

---

### Key Considerations:

1. **Value Helpers**:
   - Use the *@Consumption.valueHelpDefinition* annotation to define **Value Helpers**.
   ```ABAP
   define root view entity Z##_CI_PRODUCT_####
   provider contract transactional_query
   as projection on Z##_I_PRODUCT_####
   {
         /* Code snippet */
         @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_PRODUCT_VH_####', element: 'Prodid' } } ]
         @Search.defaultSearchElement: true
         Prodid,
         /* Code snippet */
   }
   ```

2. **Standard SearchField**:
   - Use *@Search.defaultSearchElement: true* for enabling default search functionality.
   ```ABAP
   define root view entity Z##_CI_PRODUCT_####
   provider contract transactional_query
   as projection on Z##_I_PRODUCT_####
   {
         /* Code snippet */
         @Search.defaultSearchElement: true
         Phaseid,
         /* Code snippet */
   }
   ```

3. **Text Instead of ID**:
   - Use *@ObjectModel.text.element* to display text fields instead of IDs.
   ```ABAP
   define root view entity Z##_CI_PRODUCT_####
   provider contract transactional_query
   as projection on Z##_I_PRODUCT_####
   {
         /* Code snippet */
         @ObjectModel.text.element: [ 'Pgname' ]
         @Search.defaultSearchElement: true
         Pgid,
         /* Code snippet */
   }
   ```

---

### Steps:

1. **Create Projection Transactional Interface**
   - **[Z##_CI_PRODUCT_####](./02_cds.md#z##_ci_product_)**
   - **[Z##_CI_MARKET_####](./02_cds.md#z##_ci_market_)**
   - **[Z##_CI_ORDER_####](./02_cds.md#z##_ci_order_)**

2. **Modify Projection Transactional Query**
   - **[Z##_C_PRODUCT_####](./02_cds.md#z##_c_product_)**
   - **[Z##_C_MARKET_####](./02_cds.md#z##_c_market_)**
   - **[Z##_C_ORDER_####](./02_cds.md#z##_c_order_)**

3. **Behavior Definition**
   - Create BDEF for **[Z##_CI_PRODUCT_####](./06_behavior_definition.md#z##_ci_product_)**

4. **[Add Value Helpers into Product](./02_cds.md#z##_ci_product_)**:
   - Annotate **[Z##_CI_PRODUCT_####](./02_cds.md#z##_ci_product_)** with the necessary fields.
   ```ABAP
   define root view entity Z##_CI_PRODUCT_####
   provider contract transactional_query
   as projection on Z##_I_PRODUCT_####
   {
         key ProdUuid,

         @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_PRODUCT_VH_####', element: 'Prodid' } } ]
         @Search.defaultSearchElement: true
         Prodid,

         @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_PG_VH_####', element: 'Pgid' } } ]
         @ObjectModel.text.element: [ 'Pgname' ]
         @Search.defaultSearchElement: true
         Pgid,

         _PG.Pgname                  as Pgname,

         @Search.defaultSearchElement: true
         @Semantics.amount.currencyCode: 'PriceCurrency'
         Price,

         @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_PHASE_VH_####', element: 'Phaseid' } } ]
         @ObjectModel.text.element: [ 'PhaseName' ]
         @Search.defaultSearchElement: true
         Phaseid,

         _PHASE.Phase                as PhaseName,

         @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_UOM_VH_####', element: 'Msehi' } } ]
         @EndUserText.label: 'Units'
         @Semantics.unitOfMeasure: true
         SizeUom,

         @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_CURRENCY_VH_####', element: 'Currency' } } ]
         @Search.defaultSearchElement: true
         PriceCurrency,
   }
   ```

5. **[Add Value Helpers into Market](./02_cds.md#z##_ci_market_)**:
   - Annotate **[Z##_CI_MARKET_####](./02_cds.md#z##_ci_market_)** with the necessary fields.
   ```ABAP
   define view entity Z##_C_MARKET_####
   as projection on Z##_I_MARKET_####
   {
         key ProdUuid,
         key MrktUuid,

         @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_COUNTRY_VH_####', element: 'Id' } } ]
         @ObjectModel.text.element: [ 'Country' ]
         @Search.defaultSearchElement: true
         Mrktid,

         _Countries.Country  as Country,
   }
   ```

6. **[Add Value Helpers into Order](./02_cds.md#z##_ci_order_)**:
   - Annotate **[Z##_CI_ORDER_####](./02_cds.md#z##_ci_order_)** with the necessary fields.
   ```ABAP
   define view entity Z##_C_ORDER_####
   as projection on Z##_I_ORDER_####
   {
         @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_CURRENCY_VH_####', element: 'Currency' } } ]
         @Search.defaultSearchElement: true
         Amountcurr,
   }
   ```

7. **[Add SearchField](./03_metadata_extension.md)**:
   - Add SearchField for **[Z##_C_PRODUCT_####](./03_metadata_extension.md#z##_c_product_)** with the necessary fields.
   ```ABAP
   @Metadata.layer: #CORE

   annotate entity Z##_C_PRODUCT_####
   with
   {
         @UI.selectionField: [ { position: 10 } ]
         Prodid;

         @UI.selectionField: [ { position: 15 } ]
         Pgid;

         @UI.selectionField: [ { position: 25 } ]
         Phaseid;
   }
   ```

8. **[Add action into Behavior Definition](./06_behavior_definition.md)**:
   - Add action for **[Z##_I_PRODUCT_####](./06_behavior_definition.md#z##_i_product_)**.
   ```ABAP
     " Part of code was skipped

      define behavior for Z##_I_PRODUCT_#### alias Product
      persistent table z##_d_prod_####
      lock master
      authorization master ( instance )
      etag master LocalChangedTime
      {
     " Part of code was skipped

      action nextPhase result [1] $self;

     " Part of code was skipped
      }
   ```
   - Add **[action implementation](./07_behavior_implementation.md#z##_i_product_)** over **Quick Fix**.
   ```ABAP
      METHOD nextPhase.
         READ ENTITIES OF z##_i_product_#### IN LOCAL MODE
               ENTITY Product
               FIELDS ( Phaseid )
               WITH CORRESPONDING #( keys )
               RESULT DATA(lt_product)
               FAILED failed.

         LOOP AT lt_product ASSIGNING FIELD-SYMBOL(<ls_product>).
            DATA(lv_phase_id) = phase_id-plan.
            CASE <ls_product>-Phaseid.
            WHEN phase_id-plan.
               lv_phase_id = phase_id-dev.
            WHEN phase_id-dev.
               lv_phase_id = phase_id-prod.
            WHEN phase_id-prod.
               lv_phase_id = phase_id-out.
            WHEN phase_id-out.
               lv_phase_id = phase_id-plan.
            WHEN OTHERS.
               lv_phase_id = phase_id-plan.
            ENDCASE.

            MODIFY ENTITIES OF z##_i_product_#### IN LOCAL MODE
                  ENTITY Product
                  UPDATE
                  FIELDS ( Phaseid )
                  WITH VALUE #( FOR key IN keys
                                 ( %tky    = key-%tky
                                 Phaseid = lv_phase_id ) )
                  FAILED   failed
                  REPORTED reported.
            READ ENTITIES OF z##_i_product_#### IN LOCAL MODE
               ENTITY Product
               ALL FIELDS WITH CORRESPONDING #( keys )
               RESULT DATA(lt_result).
            result = VALUE #( FOR ls_product_result IN lt_result
                              ( %tky   = ls_product_result-%tky
                              %param = ls_product_result ) ).
         ENDLOOP.
      ENDMETHOD.
   ```
   - Use action in CDS Projection BDEF [Transactional Interface](./06_behavior_definition.md) and Transactional Query
   ```ABAP
      interface;

      define behavior for Z##_C_PRODUCT_#### alias _Product
      {
     " Part of code was skipped

      use association _Market { create; }
      use action nextPhase;
      }
   ```
   ```ABAP
      projection;
      strict ( 2 );

      define behavior for Z##_C_PRODUCT_#### alias _Product
      {
     " Part of code was skipped

      use association _Market { create; }
      use action nextPhase;
      }
   ```
   - Create new action in **[Z##_C_PRODUCT_####](./06_behavior_definition.md#z##_c_product_)**.
   ```ABAP
      define behavior for Z##_C_PRODUCT_#### alias Product
      implementation in class zbp_##_c_product_#### unique
      {
     " Part of code was skipped
      use action nextPhase;
      action priorPhase result [1] $self;
      }
   ```
   - Create class implementation **zbp_##_c_product_####** and add **[action implementation](./07_behavior_implementation.md#z##_c_product_)** over **Quick Fix**.
   ```ABAP
      METHOD priorPhase.
         READ ENTITIES OF z##_c_product_#### IN LOCAL MODE
               ENTITY Product
               FIELDS ( Phaseid )
               WITH CORRESPONDING #( keys )
               RESULT DATA(lt_product)
               FAILED failed.

         LOOP AT lt_product ASSIGNING FIELD-SYMBOL(<ls_product>).
            DATA(lv_phase_id) = phase_id-plan.
            CASE <ls_product>-Phaseid.
            WHEN phase_id-plan.
               lv_phase_id = phase_id-out.
            WHEN phase_id-dev.
               lv_phase_id = phase_id-plan.
            WHEN phase_id-prod.
               lv_phase_id = phase_id-dev.
            WHEN phase_id-out.
               lv_phase_id = phase_id-prod.
            WHEN OTHERS.
               lv_phase_id = phase_id-plan.
            ENDCASE.

            MODIFY ENTITIES OF z##_c_product_#### IN LOCAL MODE
                  ENTITY Product
                  UPDATE
                  FIELDS ( Phaseid )
                  WITH VALUE #( FOR key IN keys
                                 ( %tky    = key-%tky
                                 Phaseid = lv_phase_id ) )
                  FAILED   failed
                  REPORTED reported.
            READ ENTITIES OF z##_c_product_#### IN LOCAL MODE
               ENTITY Product
               ALL FIELDS WITH CORRESPONDING #( keys )
               RESULT DATA(lt_result).
            result = VALUE #( FOR ls_product_result IN lt_result
                              ( %tky   = ls_product_result-%tky
                              %param = ls_product_result ) ).
         ENDLOOP.
      ENDMETHOD.
   ```
   - Add action by annotation into [Metadata Extensions](./03_metadata_extestion.md)
   ```ABAP
      @Metadata.layer: #CORE

      annotate entity Z##_C_PRODUCT_####
         with

      {
      " Part of code was skipped

         @UI.lineItem: [ { type: #FOR_ACTION, dataAction: 'nextPhase', label: 'Next Phase', position: 10 } ]
         @UI.identification: [ { type: #FOR_ACTION, dataAction: 'nextPhase', label: 'Next Phase', position: 10 } ]
         ProdUuid;

      " Part of code was skipped
      }
   ```

---

### Final Steps:

- Activate all updated CDS objects.
- Test the updated services in the Fiori application to ensure proper integration and functionality.

---

### Possible Issues and Solutions:

1. **Annotations Not Recognized**:
   - Verify syntax and ensure all referenced objects exist and are active.

2. **Search Functionality Not Working**:
   - Check if the fields have the *@Search.defaultSearchElement* annotation.

3. **Value Helper Not Displayed**:
   - Confirm the correctness of the entity name and element in the *@Consumption.valueHelpDefinition* annotation.

---

### Summary:
This iteration enhances the data model by integrating **Value Helpers**, refining search functionality, and improving the usability of the application with annotations.