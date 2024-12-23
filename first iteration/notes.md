# Notes
## First iteration

Here we should create back-end objects for Fiori read-only application:
- Tables
- Data models (CDS Interface)
- Business model (CDS projection)
- Service Definition
- Service Binding OData V2
 
 This is for the future simplest application.

 ### Pay attention:
 1. Instead `####` in object names you should use any fourth symbols of your choice
 2. All objects should be created in your own package

### Steps
1. Create tables as describe in 00_tables.md
2. Create CDS Interfaces as describe in 01_cds.md
3. Create CDS Projections as describe in 02_cds.md
4. Create Metadata Extensions as describe in 03_metadata_extension.md
5. Create Service Definition as describe in 04_service.md
6. Create class for data generation as describe in generator.md and execute in console over F9

### the final step:
- Create Service Binding for ODataV2 UI based on your Service Definition
- Name it Z_UI_PRODUCT_O2_<your_suffix>


Here you should create read-only application