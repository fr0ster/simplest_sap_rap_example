# Notes

## Fifth Iteration: Business Model Enhancements

In this iteration, we enhance the business model by introducing **Value Helpers** in **Projection**.

---

### Key Objectives:
1. Create **Projection Transacional Interface**, change projected CDS in **Projection Transacional Query** to new **Projection Transacional Interface**. 
2. Add **Value Helpers** into **[Z##_CI_PRODUCT_####](./02_cds.md#z##_ci_product_)**.
3. Add **Value Helpers** into **[Z##_CI_MARKET_####](./02_cds.md#z##_ci_market_)**.
4. Add **Value Helpers** into **[Z##_CI_ORDER_####](./02_cds.md#z##_ci_order_)**.
5. Add **needed fields** into the **SearchField Area** and enable standard search functionality in **[Metadata Extension](03_metadata_extension.md)**.

---

### Key Considerations:

1. **Value Helpers**:
   - Use the *@Consumption.valueHelpDefinition* annotation to define **Value Helpers**.
   ```ABAP
   define root view entity Z##_C_PRODUCT_####
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
   define root view entity Z##_C_PRODUCT_####
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
   define root view entity Z##_C_PRODUCT_####
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

1. **Create Projection Transacional Interface**
   - **[Z##_CI_PRODUCT_####](./02_cds.md#z##_ci_product_)**
   - **[Z##_CI_MARKET_####](./02_cds.md#z##_ci_market_)**
   - **[Z##_CI_ORDER_####](./02_cds.md#z##_ci_order_)**

2. **Modificate Projection Transacional Query**
   - **[Z##_CI_PRODUCT_####](./02_cds.md#z##_c_product_)**
   - **[Z##_CI_PRODUCT_####](./02_cds.md#z##_c_market_)**
   - **[Z##_CI_PRODUCT_####](./02_cds.md#z##_c_order_)**

3. **Behavior Definition**
   - Create BDEF for **[Z##_CI_PRODUCT_####](./06_behavior_definition.md#z##_ci_product_)**
   - Modificate BDEF for **[Z##_C_PRODUCT_####](./06_behavior_definition.md#z##_c_product_)**

1. **[Add Value Helpers into Product](./02_cds.md#z##_ci_product_)**:
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

2. **[Add Value Helpers into Market](./02_cds.md#z##_ci_market_)**:
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

3. **[Add Value Helpers into Order](./02_cds.md#z##_ci_order_)**:
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

4. **[Add SearchField](./03_metadata_extension.md)**:
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