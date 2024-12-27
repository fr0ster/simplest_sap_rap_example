# Notes
## Fifth Iteration:  Business Model Enhancements
In this iteration, we enhance the busines model by introducing a **Value Helpers** in **Projection**.

---

### Key Objectives:
1. Add **Value Helpers** into **[Z##_C_PRODUCT_####](./02_cds.md#z##_d_product_)**.
2. Add **Value Helpers** into **[Z##_C_MARKET_####](./02_cds.md#z##_i_market_)**.
3. Add **Value Helpers** into **[Z##_C_ORDER_####](./02_cds.md#z##_c_order_)**.
4. Add **neaded fields** into **[SearchField Area](03_metadata_extension.md)** and enable standard searching functionality.

---

### Pay Attention:
1. **Value Helpers**:
   - Usually for using **Value Helpers** we use *@Consumption.valueHelpDefinition* annotation.
   ```ABAP
   define root view entity Z##_C_PRODUCT_####
   provider contract transactional_query
   as projection on Z##_I_PRODUCT_####

   {
         /* Part of code was skipped */
         @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_PRODUCT_VH_####', element: 'Prodid' } } ]
         @Search.defaultSearchElement: true
         Prodid,
         /* Part of code was skipped */
   }
   ```
2. **Standard Searchfield**:
   - For using **standard** we use *@Search.defaultSearchElement: true* annotation.
   ```ABAP
   define root view entity Z##_C_PRODUCT_####
   provider contract transactional_query
   as projection on Z##_I_PRODUCT_####

   {
         /* Part of code was skipped */
         @Search.defaultSearchElement: true
         Phaseid,
         /* Part of code was skipped */
   }
   ```
2. **Using text instead id**:
   - For using **text** instead **id** we use *@ObjectModel.text.element: [ `text field` ]* annotation.
   ```ABAP
   define root view entity Z##_C_PRODUCT_####
   provider contract transactional_query
   as projection on Z##_I_PRODUCT_####

   {
         /* Part of code was skipped */
         @Search.defaultSearchElement: true
         Phaseid,
         /* Part of code was skipped */
   }
   ```
---

### Steps:
1. **[Add a Value Helpers into Product](./02_cds.md#z##_c_product_)**:
   - Add annotation for  **[Z##_C_PRODUCT_####](./02_cds.md#z##_c_product_)** with the necessary fields.
   ```ABAP
   define root view entity Z##_C_PRODUCT_####
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

         /* Part of code was skipped */

         @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_PHASE_VH_####', element: 'Phaseid' } } ]
         @ObjectModel.text.element: [ 'PhaseName' ]
         @Search.defaultSearchElement: true
         Phaseid,

         _PHASE.Phase                as PhaseName,

         /* Part of code was skipped */

         @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_UOM_VH_####', element: 'Msehi' } } ]
         @EndUserText.label: 'Units'
         @Semantics.unitOfMeasure: true
         SizeUom,
         
         /* Part of code was skipped */

         @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_CURRENCY_VH_####', element: 'Currency' } } ]
         @Search.defaultSearchElement: true
         PriceCurrency,

         /* Part of code was skipped */
   }
   ```
2. **[Add a Value Helpers into Market](./02_cds.md#z##_c_market_)**:
   - Add annotation for  **[Z##_C_MARKET_####](./02_cds.md#z##_c_market_)** with the necessary fields.
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

         /* Part of code was skipped */

         _Countries.Country  as Country,

         /* Part of code was skipped */
   }
   ```
3. **[Add a Value Helpers into Order](./02_cds.md#z##_c_order_)**:
   - Add annotation for  **[Z##_C_ORDER_####](./02_cds.md#z##_c_order_)** with the necessary fields.
   ```ABAP
   define view entity Z##_C_ORDER_####
   as projection on Z##_I_ORDER_####

   {
         /* Part of code was skipped */

         @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_CURRENCY_VH_####', element: 'Currency' } } ]
         @Search.defaultSearchElement: true
         Amountcurr,

         /* Part of code was skipped */
   }
   ```
3. **[Add a searchfield](./03_metadata_extension.md)**:
   - Add a searchfield for **[Z##_C_PRODUCT_####](./03_metadata_extension.md#z##_c_product_)** with the necessary fields.
      ```ABAP
      @Metadata.layer: #CORE

      annotate entity Z##_C_PRODUCT_####
      with

      {
         /* Part of code was skipped */

            @UI.selectionField: [ { position: 10 } ]
            Prodid;

            @UI.selectionField: [ { position: 15 } ]
            Pgid;

            @UI.selectionField: [ { position: 25 } ]
            Phaseid;

         /* Part of code was skipped */
      }
      ```

---

### Final Steps:
- Activate all new and updated CDS objects.
- Test the updated service in the Fiori application to ensure the criticality handling works as expected.

---

### Possible Issues and Solutions:

---

### Summary:
This iteration enhances the data model by introducing orders and integrating them into the market data.