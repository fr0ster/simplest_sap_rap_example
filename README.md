# Simplest SAP RAP Example

This guide walks you through building a simple SAP RAP application, progressively enhancing its functionality over several iterations. Each iteration targets specific development aspects, including behavior implementation, data modeling, UI enhancements, advanced features like Draft Handling, and OData services.

---

## [Iteration 1: Read-Only Application](./first_iteration/notes.md)

The first iteration lays the foundation for the application:
- **Data Model**:
  - Define the main **Business Object (BO)** with primary entities.
  - Create **basic associations** between entities.
- **Metadata Extension**:
  - Configure the **List Report** and **Object Page** with minimal annotations for navigation and display.

---

## [Iteration 2: Behavior Implementation](./second_iteration/notes.md)

In this iteration, we enable transactional capabilities for the business object:
- **Behavior Definition (BDEF)**:
  - Set the behavior type as **Managed**.
  - Enable standard transactional operations:
    - **Create**, **Update**, and **Delete** (CUD).
- **Behavior Implementation (Class)**:
  - Add **validations** to ensure data consistency.
  - Implement **determinations** for automatic updates of dependent fields.

---

## [Iteration 3: Third-Level Composition](./third_iteration/notes.md)

This iteration focuses on extending the data model with deeper hierarchical relationships:
- Add an **entity at the third level of composition** to manage Orders data.
- Introduce a **new association** into the Market entity (interface and projection).

---

## [Iteration 4: Data Model Refinements](./fourth_iteration/notes.md)

Here, we enhance the data model with advanced refinements:
- Add **associations in the data model layer** for context-specific navigation.
- Introduce **calculated fields** for derived values where needed.
- Adjust existing associations for **value help** or dependent relationships.

---

## [Iteration 5: Business Model Enhancements](./fifth_iteration/notes.md)

This iteration refines the business model to add new functionalities:
- Add **associations in the business model layer** for enhanced context navigation.
- Introduce **charts** for market analytics and data visualization.

---

## [Iteration 6: UI Enhancements and Annotations](./sixth_iteration/notes.md)

In this iteration, we improve the user interface with advanced configurations:
- Add **search functionality** to the **List Report**.
- Configure the **header section** in the **Object Page** to display key details.
- Use **FieldGroup** annotations to logically group fields on the **Object Page**.
- Add **Facets** for better layout and organization.
- Define **field-level annotations** for labels, tooltips, and formatting.
- Configure **dynamic visibility** and field control for an improved user experience.

---

## [Iteration 7: Draft Handling](./seventh_iteration/notes.md)

This iteration enables **Draft Handling** for intermediate data saving:
- Configure the **Draft Behavior** in the Behavior Definition (BDEF).
- Add logic for handling draft-specific actions, such as saving and discarding changes.
- Test the draft workflow in both the UI and backend.

---

## [Iteration 8: Side-by-Side Extension with OData V4 Service](./eighth_iteration/notes.md)

In the final iteration, we implement a **side-by-side extension** with a new **OData V4 service**:
- **OData V4 Service Binding**:
  - Create a service for the extended projection of the BO.
  - Include necessary fields, actions, and associations.
- **UI V4 Annotations**:
  - Define annotations for the list view, object page, and sections in the consuming application.
- **Integration**:
  - Ensure seamless integration of the new service with external applications.

---

### Application Evolution

1. **Read-Only Foundation**: The application begins as a simple read-only solution in Iteration 1.
2. **Transactional Capabilities**: Iterations 2 through 5 add transactionality, deeper compositions, and advanced data relationships.
3. **UI and Draft Handling**: Iterations 6 and 7 refine the UI and introduce draft-handling features for intermediate saving.
4. **External Integration**: The final iteration enables side-by-side extensions with a robust OData V4 service.

By following these steps, the application evolves into a fully functional SAP RAP solution, capable of handling advanced use cases like external service integration.
