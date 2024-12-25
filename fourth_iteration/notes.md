# Notes
## Fourth Iteration: Enhancing the Data Model and Metadata Extensions
In this iteration, we enhance the data model by introducing a **Criticality Levels** table and integrating it with the product data. Additionally, we update the Metadata Extension and projections to support new functionality, including criticality handling and UI improvements.

---
### Key Objectives:
1. Create a new **Criticality Levels Table** ([`Z##_D_CRTLY_####`](./00_tables.md)) to store criticality levels.
2. Develop a **CDS View Entity** ([`Z##_I_CRITICALITY_LEVELS_####`](./01_cds.md)) for accessing the criticality levels.
2. Develop a **CDS Projection** ([`Z##_C_CRITICALITY_LEVELS_####`](./02_cds.md)) for accessing the criticality levels.
3. Integrate the criticality data into the product entity:
  - Add an **association** in [`Z##_I_PRODUCT_####`](./02_cds.md) to the [`Z##_I_CRITICALITY_LEVELS_####`](./01_cds.md) view.
  - Calculate a new field (`PriceCriticality`) in the product entity based on the criticality levels.
4. Update the **Product Projection**:
  - Expose the new `PriceCriticality` field in the projection ([`Z##_I_PRODUCT_PROJ_####`](./03_metadata_extestion.md)).
  - Use the `PriceCriticality` field in **UI annotations** (`lineItem` and `identification`) to enhance the display of the `Price` field.
5. Enhance Metadata Extensions:
  - Add a **Metadata Extension** for the [`Z##_C_CRITICALITY_LEVELS_####`](./03_metadata_extestion.md) entity.
  - Add a **new tab** in the product projection ([`Z##_I_PRODUCT_PROJ_####`](./03_metadata_extestion.md)) for managing criticality levels.

---
### Steps:
1. **[Create the Criticality Levels Table](./00_tables.md)**:
  - Define the table `Z##_CRTLY_####` with the necessary fields.
  - Refer to [00_tables.md](./00_tables.md) for details.
2. **[Update the class for data generation](./05_generator.md)**:
  - Add cod into `Z##_CL_GENERATE_DATA_####` for recreating settings.
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
  - Refer to [05_generator.md](./05_generator.md) for details.
3. **[Develop the Criticality Levels View Entity](./01_cds.md)**:
  - Create the view entity `Z##_I_CRITICALITY_LEVELS_####` to expose the criticality levels.
  - Refer to [01_cds.md](./01_cds.md) for details.
4. **[Integrate Criticality into the Product Entity](./02_cds.md)**:
  - Add an association to the product entity `Z##_I_PRODUCT_####` linking it to the criticality levels.
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
  - Refer to [01_cds.md](./01_cds.md) for details.
5. **[Update the Product Projection](./02_cds.md)**:
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
  - Refer to [02_cds.md](./02_cds.md) for details.
6. **[Develop the Criticality Levels View Entity Projection](./02_cds.md)**:
  - Create the view entity `Z##_C_CRITICALITY_LEVELS_####` to expose the criticality levels.
  - Refer to [02_cds.md](./02_cds.md) for details.
7. **[Add Metadata Extension for Criticality Levels](./03_metadata_extestion.md)**:
  - Create a Metadata Extension for `Z##_C_CRITICALITY_LEVELS_####` to configure its UI.
  - Refer to [03_metadata_extestion.md](./03_metadata_extestion.md) for details.
8. **[Define the Behavior Definition (BDEF)](./06_behavior_implementation.md)**:
  - Define the behavior of the business object as **Managed**.
  - Enable standard transactional operations.
  - Refer to [06_behavior_implementation.md](./06_behavior_definition.md) for details.
  - Generate class implementation over **Quick Fix** in **ADT**.
8. **[Define Projection of the Behavior Definition (BDEF)](./06_behavior_implementation.md)**:
  - Set alias.
  - Refer to [06_behavior_implementation.md](./06_behavior_definition.md) for details.
  - Generate class implementation over **Quick Fix** in **ADT**.
9. **Test the Behavior**:
  - Test the implemented behavior in the Fiori application or using the ABAP console.
  - Verify that the operations (Create, Update, Delete) and validations/determinations function as expected.
---
### Final Steps:
- Activate all new and updated CDS objects.
- Test the updated service in the Fiori application to ensure the criticality handling works as expected.
---
### Summary:
This iteration enhances the data model by introducing criticality levels, integrating them into product data, and updating the UI to reflect criticality-based calculations. The new tab for managing criticality levels provides flexibility for maintaining the criticality values directly in the Fiori application.