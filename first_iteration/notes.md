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
  - Replace the `##` prefix in object names with any two meaningful characters of your choice (e.g., `##`).
  - Replace the `####` suffix in object names with four meaningful characters specific to your project (e.g., `####`).
2. **Package Consistency**:
  - All objects must be created within your own development package to ensure consistency and organization.
3. **Annotation for service fields**
  - For correct seting service field wich are neded for correct work of BO needed add to tables of entities filds with correct type and annotation
    - **Table definition**
    ```ABAP
      created_by         : abp_creation_user;
      creation_time      : abp_creation_tstmpl;
      changed_by         : abp_lastchange_user;
      changed_time       : abp_lastchange_tstmpl;
      local_changed_time : abp_locinst_lastchange_tstmpl;
    ```
    - **CDS definition**
    ```ABAP
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
4. **CDS Activation**:
  - CDS views that are connected via **composition associations** (e.g., `association [1..*]`) must be activated **together**.
  - This applies to:
    - **Creating new CDS views** that include compositions.
    - **Modifying existing compositions**, where associations are added or changed.
    - If one of the connected views is not activated, the entire activation process may fail.
  - Ensure all dependent CDS views are activated in bulk or in the correct sequence.
---
### Steps:
1. **[Create Tables](./00_tables.md)**:
  - Follow the instructions in [00_tables.md](./00_tables.md) to define the database tables required for the application.
2. **[Define CDS Interfaces](./01_cds.md)**:
  - Create CDS views for the **data model** as described in [01_cds.md](./01_cds.md).
3. **[Create CDS Projections](./02_cds.md)**:
  - Develop the CDS views for the **business model** as outlined in [02_cds.md](./02_cds.md).
  - Ensure that only necessary fields are exposed.
4. **[Add Metadata Extensions](./03_metadata_extension.md)**:
  - Use **Metadata Extensions** to configure annotations for the List Report and Object Page UI.
  - Refer to [03_metadata_extension.md](./03_metadata_extension.md) for details.
5. **[Create Service Definition](./04_service.md)**:
  - Define the service interface in [04_service.md](./04_service.md), specifying the entities and operations that should be available.
6. **[Generate Sample Data](./generator.md)**:
  - Implement a class for data generation as described in [generator.md](./generator.md).
  - Execute the class in the ABAP console using **F9** to populate the tables with sample data.
---
### The Final Step:
- **Create Service Binding (OData V2)**:
  - Bind the service definition to an OData V2 protocol for UI consumption.
  - Name the service binding as `Z##_UI_PRODUCT_O2_####`.
- Activate and publish the service.
- Test the service using the **Preview** button in the Service Binding editor.
---
### Possible Issues and Solutions:
#### 1. **CDS Views Not Activating**
  **Problem**:
  - CDS views fail to activate, particularly those with **composition associations**.
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
  - Example of **Metadata Extension**:
    ```abap
    @Metadata.layer: #CUSTOMER
    annotate view ZI_Product with {
      @UI: {
        lineItem: [
          { position: 10; label: 'Product ID'; },
          { position: 20; label: 'Product Name'; }
        ],
        identification: [
          { position: 10; label: 'Product Name'; }
        ]
      }
    };
    ```
---
### Summary:
This iteration sets up the foundational components of the application, including the database, data model, and service configuration. At the end of this iteration, you will have a fully functional **read-only application** that can be consumed through OData V2.