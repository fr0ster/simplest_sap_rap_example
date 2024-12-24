# Simplest SAP RAP Example

In this guide, we will build a simple SAP RAP application, progressively enhancing its functionality over several iterations. Each iteration focuses on specific tasks such as behavior implementation, data modeling, UI annotations, advanced features like Draft Handling, and OData services.

---
## [Iteration 1: Read-Only Application](./first_iteration/notes.md)
In the first iteration, we create the foundation of the application:
- **Data Model**:
 - Define the main **Business Object (BO)** with primary entities.
 - Create **basic associations** between entities.
- **Metadata Extension**:
 - Configure the **List Report** and **Object Page** with minimal annotations for navigation and display.
---
## [Iteration 2: Behavior Implementation](./second_iteration/notes.md)
In this iteration, we implement the core behavior of the business object:
- **Behavior Definition (BDEF)**:
 - Define the behavior type as **Managed**.
 - Enable standard transactional operations:
   - **Create**, **Update**, and **Delete** (CUD).
- **Behavior Implementation (Class)**:
 - Implement **validations** to ensure data consistency.
 - Add **determinations** for automatic updates of dependent fields.
---
## [Iteration 3: Advanced Data Model Refinements](./third_iteration/notes.md)
In this iteration, we refine and enhance the data model:
- Add **associations in the projection layer** for context-specific navigation.
- Introduce **calculated fields** for derived values where needed.
- Adjust existing associations for value help or dependent relationships.
---
## [Iteration 4: UI Enhancements and Annotations](./fourth_iteration/notes.md)
In this iteration, we improve the UI and add advanced annotations:
- Add **search functionality** to the **List Report**.
- Configure the **header section** in the **Object Page** to display key information.
- Use **FieldGroup** annotations to group fields logically on the **Object Page**.
- Introduce **Facets** for better layout organization.
- Define **field-level annotations** for labels, tooltips, and formatting.
- Configure dynamic visibility and field control for enhanced user experience.
---
## [Iteration 5: Draft Handling](./fifth_iteration/notes.md)
In this iteration, we enable **Draft Handling** to allow intermediate saving of changes:
- Configure the **Draft Behavior** in the Behavior Definition (BDEF).
- Implement logic to handle draft-specific actions, such as saving and discarding changes.
- Test the draft workflow in both the UI and backend.
---
## [Iteration 6: Side-by-Side Extension with New OData V4 Service](./sixth_iteration/notes.md)
In this iteration, we implement a **side-by-side extension** by creating a new **OData V4 service** based on an extended projection of the Business Object (BO):
- **OData V4 Service Binding**:
 - Create a new service for the extended projection of the BO.
 - Include necessary fields, actions, and associations.
- **UI V4 Annotations**:
 - Add annotations to define the list view, object page, and sections for the consuming application.
- **Integration**:
 - Ensure the new service integrates seamlessly with external applications.
---
### Application Evolution
1. The application starts as a **read-only solution** in Iteration 1.
2. Transactionality, Draft Handling, and UI enhancements are progressively added.
3. Finally, the **Side-by-Side Extension** (Iteration 6) enables external integrations with new functionalities via a dedicated OData V4 service.
By following these iterations, we transform the application into a fully functional SAP RAP solution, ready for advanced use cases like external service integration via OData.