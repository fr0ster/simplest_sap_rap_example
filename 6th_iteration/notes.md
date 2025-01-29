# Notes

## Sixth Iteration: UI Enhancements and Annotations

In this iteration, we enhance the User interface.

---

### Key Objectives:
1. Add auxillary field to **[Data Model](./01_cds.md#z##_i_product_)** and **[Busines Model](./02_cds.md#z##_ci_product_)**. 
2. Enchance **[Metadata Extensions](./03_metadata_extestion.md)**.

---

### Steps:
1. **[Add auxillary field to Data Model](./01_cds.md#z##_i_product_)**:
    ```ABAP
    define root view entity Z##_I_PRODUCT_####
    as select from z##_d_prod_####

     " Part of code was skipped

    association [1..1] to Z##_I_CRITICALITY_LEVELS_#### as _PriceCriticality on _PriceCriticality.Id = 'PriceCritically'

    {
    key prod_uuid          as ProdUuid,
     " Part of code was skipped

        case phaseid
        when 1 then 1
        when 2 then 2
        when 3 then 3
        when 4 then 4
        else 0
        end                as PhaseCritically,

     " Part of code was skipped
    }
    ```

2. **[Add auxillary field to Projection Transactional Interface](./02_cds.md#z##_ci_product_)**:
    ```ABAP
    define root view entity Z##_CI_PRODUCT_####
    provider contract transactional_interface
    as projection on Z##_I_PRODUCT_####

    {
     " Part of code was skipped

        PhaseCritically,

     " Part of code was skipped
    }
    ```

3. **[Add auxillary field to Projection Transactional Query](./02_cds.md#z##_c_product_)**:
    ```ABAP
    define root view entity Z##_C_PRODUCT_####
    provider contract transactional_query
    as projection on Z##_CI_PRODUCT_####

    {
     " Part of code was skipped

        PhaseCritically,

     " Part of code was skipped
    }
    ```
4. **[Modificate Z##_C_PRODUCT_#### Metadata Extension](./03_metadata_extestion.md#z##_c_product_)**
    - Add headerinfo and presentationVariant
    ```ABAP
    @Metadata.layer: #CORE

    @UI.headerInfo: { typeName: 'Kitchen Appliances',
                    typeNamePlural: 'Kitchen Appliances',
                    imageUrl: 'Imageurl',
                    title: { type: #STANDARD, label: 'Product', value: 'Prodid' },
                    description: { value: 'Pgname', type: #STANDARD } }

    @UI.presentationVariant: [ { sortOrder: [ { by: 'Prodid', direction: #ASC },
                                            { by: 'PGNAME', direction: #ASC } ] } ]

    annotate entity Z##_C_PRODUCT_####
        with

    {
     " Part of code was skipped
    }
    ```
    - Add facets
5. **[Modificate Z##_C_MARKET_#### Metadata Extension](./03_metadata_extestion.md#z##_c_market_)**
6. **[Modificate Z##_C_ORDER_#### Metadata Extension](./03_metadata_extestion.md#z##_c_order_)**

---

### Final Steps:

- Activate all updated CDS objects.
- Test the updated services in the Fiori application to ensure proper integration and functionality.

---

### Possible Issues and Solutions:

---

### Summary:
This iteration enhances the UI.