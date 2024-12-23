# Simplest SAP RAP Example

---

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

## [Iteration 6: OData V4 Service for Side-by-Side Extension](./sixth_iteration/notes.md)

In the final iteration, we expose the application data through an **OData V4 service** to support a side-by-side extension:
- **OData Service Binding**:
 - Create an **OData V4 Service Binding** for the BO projection.
 - Configure the service to include only the necessary fields and associations.
- **Enable External Access**:
 - Mark the service for external use by activating it in the **Service Binding**.
 - Ensure that the service exposes the full transactional capabilities of the BO.
- **Extend the Business Object (Optional)**:
 - Add additional fields or calculated properties to the BO projection layer.
 - Ensure the extension is reflected in the OData service without affecting the core BO.

---

### Application Evolution
1. The application starts as a **read-only solution** in Iteration 1.
2. Transactionality, Draft Handling, and UI enhancements are progressively added.
3. Finally, the OData V4 service (Iteration 6) enables external integrations, making the application ready for side-by-side extensions.

By following these iterations, we transform the application into a fully functional SAP RAP solution, ready for advanced use cases like external service integration via OData.