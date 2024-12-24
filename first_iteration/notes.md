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
  - Replace `####` in object names with four meaningful symbols of your choice (e.g., `Z_UI_PRDT`).
2. **Package Consistency**:
  - All objects must be created within your own development package to ensure consistency and organization.
3. **Modular Design**:
  - Follow the modular approach to separate concerns and ensure reusability.
---
### Steps:
1. **Create Tables**:
  - Follow the instructions in `00_tables.md` to define the database tables required for the application.
2. **Define CDS Interfaces**:
  - Create CDS views for the **data model** as described in `01_cds.md`.
3. **Create CDS Projections**:
  - Develop the CDS views for the **business model** as outlined in `02_cds.md`.
  - Ensure that only necessary fields are exposed.
4. **Add Metadata Extensions**:
  - Use **Metadata Extensions** to configure annotations for the List Report and Object Page UI.
  - Refer to `03_metadata_extension.md` for details.
5. **Create Service Definition**:
  - Define the service interface in `04_service.md`, specifying the entities and operations that should be available.
6. **Generate Sample Data**:
  - Implement a class for data generation as described in `generator.md`.
  - Execute the class in the ABAP console using **F9** to populate the tables with sample data.
---
### The Final Step:
- **Create Service Binding (OData V2)**:
 - Bind the service definition to an OData V2 protocol for UI consumption.
 - Name the service binding as `Z_UI_PRODUCT_O2_<your_suffix>`.
---
### Summary:
This iteration sets up the foundational components of the application, including the database, data model, and service configuration. At the end of this iteration, you will have a fully functional **read-only application** that can be consumed through OData V2.
has context menu