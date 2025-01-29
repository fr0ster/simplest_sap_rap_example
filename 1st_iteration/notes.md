# Notes

## First Iteration: Creating a Fiori Read-Only Application

In this iteration, we will create back-end objects for a **Fiori read-only application**, focusing on the following key components:
- **Tables**: To store data for the application.
- **Data models (CDS Interface)**: To define the underlying structure of the data.
- **Business model (CDS Projection)**: To expose specific data for the application.
- **Service Definition**: To define the service interface.
- **Service Binding (OData V2)**: To make the service available for consumption.

---

### Pay Attention:
1. **Object Naming**:
   - Replace the `##` prefix in object names with any two meaningful characters of your choice (e.g., `Z_`).
   - Replace the `####` suffix in object names with four meaningful characters specific to your project (e.g., `_PROD`).

2. **Package Consistency**:
   - All objects must be created within your own development package to ensure consistency and organization.
   - If you are going to use **AbapGit**, create **Service Binding** in a separate package, outside the **main package**.

3. **Annotations for Service Fields**:
   - To ensure proper operation of the Business Object (BO), include service-related fields in the tables with the appropriate data types and annotations:
     - **Table Definition**:
       ```abap
       created_by         : abp_creation_user;
       creation_time      : abp_creation_tstmpl;
       changed_by         : abp_lastchange_user;
       changed_time       : abp_lastchange_tstmpl;
       local_changed_time : abp_locinst_lastchange_tstmpl;
       ```
     - **CDS Definition**:
       ```abap
       @Semantics.user.createdBy: true
       created_by      as CreatedBy,

       @Semantics.systemDateTime.createdAt: true
       creation_time   as CreationTime,

       @Semantics.user.lastChangedBy: true
       changed_by      as ChangedBy,

       @Semantics.systemDateTime.lastChangedAt: true
       changed_time     as ChangedTime,

       @Semantics.systemDateTime.localInstanceLastChangedAt: true
       local_changed_time as LocalChangedTime,
       ```

4. **Currency fields** in **Metadata Extension**:
   - The annotation **@Semantics.amount.currencyCode** causes the currency code to appear next to the field to which the annotation is applied. However, if the annotation **@UI.hidden: true** is applied to the field with the currency code, it will not appear next to the field annotated with **@Semantics.amount.currencyCode**.

5. **Projection type**:
   - Use the **Projection** type in the wizard.
   - Add **provider contract transactional_query** to the definition via **Quick Fix** or manually, as an empty provider contract is now obsolete.

6. **CDS Activation**:
   - CDS views that are connected via **composition associations** (e.g., `association [1..*]`) must be activated **together**. This applies to:
     - **Creating new CDS views** that include compositions.
     - **Modifying existing compositions**, where associations are added or changed.
   - If one of the connected views is not activated, the entire activation process may fail. Ensure all dependent CDS views are activated in bulk or in the correct sequence.

---

### Steps:

1. **[Create Tables](./00_tables.md)**:
   - Define the database tables required for the application.
      - **[z##_d_prod_####](./00_tables.md#z##_d_prod_)**
      - **[z##_d_mrkt_####](./00_tables.md#z##_d_mrkt_)**
      - **[z##_d_ctry_####](./00_tables.md#z##_d_ctry_)**
      - **[z##_d_pg_####](./00_tables.md#z##_d_pg_)**
      - **[z##_d_phase_####](./00_tables.md#z##_d_phase_)**
      - **[z##_d_uom_####](./00_tables.md#z##_d_uom_)**

2. **[Define CDS Interfaces](./01_cds.md)**:
   - Create CDS views for the **data model**.
      - **[Z##_I_PRODUCT_####](./01_cds.md#Z##_I_PRODUCT_)**
      - **[Z##_I_MARKET_####](./01_cds.md#Z##_I_MARKET_)**
      - **[Z##_I_COUNTRY_####](./01_cds.md#Z##_I_COUNTRY_)**
      - **[Z##_I_COUNTRY_VH_####](./01_cds.md#Z##_I_COUNTRY_VH_)**
      - **[Z##_I_PRODUCT_VH_####](./01_cds.md#Z##_I_PRODUCT_VH_)**
      - **[Z##_I_CURRENCY_VH_####](./01_cds.md#Z##_I_CURRENCY_VH_)**
      - **[Z##_I_PG_VH_####](./01_cds.md#Z##_I_PG_VH_)**
      - **[Z##_I_PHASE_VH_####](./01_cds.md#Z##_I_PHASE_VH_)**
      - **[Z##_I_UOM_VH_####](./01_cds.md#Z##_I_UOM_VH_)**

3. **[Create CDS Projections](./02_cds.md)**:
   - Develop the CDS views for the **business model**.
      - **[Z##_C_PRODUCT_####](./02_cds.md#Z##_C_PRODUCT_)**
      - **[Z##_C_MARKET_####](./02_cds.md#Z##_C_MARKET_)**

4. **[Add Metadata Extensions](./03_metadata_extension.md)**:
   - Use **Metadata Extensions** to configure annotations for the List Report and Object Page UI.
      - **[Z##_C_PRODUCT_####](./03_metadata_extension.md#Z##_C_PRODUCT_)**
      - **[Z##_C_MARKET_####](./03_metadata_extension.md#Z##_C_MARKET_)**

5. **[Create Service Definition](./04_service.md)**:
   - Define the service interface specifying the entities and operations.
   - Refer to [04_service.md](./04_service.md) for details.

6. **[Generate Sample Data](./05_generator.md)**:
   - Implement a class for data generation.
   - Execute the class in the ABAP console using **F9** to populate the tables with sample data.
   - Refer to [generator.md](./05_generator.md) for details.

---

### The Final Step:
- **Create Service Binding (OData V2)**:
   - Bind the service definition to an OData V2 protocol for UI consumption.
   - Name the service binding as `Z##_UI_PRODUCT_O2_####`.
   - Activate and publish the service.
   - Test the service using the **Preview** button in the Service Binding editor.

---

### Possible Issues and Solutions:

#### 1. **CDS Views/Projections Not Activating**
   **Problem**:
   - CDS views/projection fail to activate, particularly those with **composition associations**.
   **Solution**:
   - Activate all CDS views connected through composition **together**, especially:
     - When creating a new CDS view with a composition.
     - When modifying an existing composition that introduces or changes associations.
   - Always activate both the root view and any dependent views linked by the composition. Use the activation tool to ensure all dependencies are resolved.

#### 2. **Columns Missing in Preview**
   **Problem**:
   - The preview in the Fiori List Report does not show columns or data properly.
   - Metadata is missing, or UI annotations are not defined for the required fields.
   **Solution**:
   - Verify that the **Metadata Extension** includes proper UI annotations for the fields:
     - Use `@UI.lineItem` for fields to be displayed in the list view.
     - Use `@UI.identification` for fields to appear in the object page header.
   - Refer to [03_metadata_extension.md](./03_metadata_extension.md) for examples.

---

### Summary:
This iteration sets up the foundational components of the application, including the database, data model, and service configuration. At the end of this iteration, you will have a fully functional **read-only application** that can be consumed through OData V2.