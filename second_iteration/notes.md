# Notes
## Second Iteration

In this iteration, we will implement the core **business behavior** for the application. The goal is to enhance the Business Object (BO) by enabling transactional operations and ensuring proper backend logic for data integrity and consistency.

---
### Objectives:
- Implement **Managed Behavior** for the Business Object (BO).
- Add support for transactional operations:
 - **Create**
 - **Update**
 - **Delete**
- Define and implement **determinations** and **validations**.
---
### Pay Attention:
1. **Determinations** and **validations**:
  - These are created for the **data model** to ensure data consistency and automatically adjust values where required.
2. **Actions** and **associations**:
  - These are defined for the **business model** (CDS projection) to provide additional functionality and navigation options.
  - Ensure you **use `action` and `association`** explicitly in the projection (`use action` / `use association`) for behavior implementation. Without this, actions and associations will not be functional.
3. Use meaningful names for all objects, adhering to naming conventions in your project.
4. Place all objects in the same package as the ones from the first iteration.
5. Replace `<####>` in object names with a unique identifier or suffix of your choice.
---
### Steps:
1. Implement the **Behavior Definition (BDEF)**:
  - Follow the instructions in `06_behavior_implementation.md` to define the BO as **Managed**.
  - Enable transactional operations:
    - `create`
    - `update`
    - `delete`.
2. Implement the **Behavior Implementation (Class)**:
  - Add logic for:
    - **Validations**: Ensure data meets specific requirements before saving.
    - **Determinations**: Automatically calculate or adjust dependent field values.
3. Adjust the **CDS Projection** for Behavior Implementation:
  - Open the **CDS projection view** of your BO.
  - Explicitly include `use action` and `use association` for each relevant action or association in the projection. For example:
    ```abap
    define root view entity ZBO_Product as projection on ZI_Product {
      key ProductID,
      ProductName,
      Category,
      -- Include actions and associations for behavior
      use action Create,
      use action Delete,
      use association _Category
    }
    ```
  - This ensures the behavior is active in the projection and can be consumed in the service.
4. Test the Behavior Implementation:
  - Verify the transactional capabilities of the BO using tools like SAP Gateway or backend test utilities.
  - Ensure that validations and determinations function correctly during operations.
---
### The Final Step:
- Activate the behavior and ensure it is integrated with your existing service and metadata.
---
This iteration completes the implementation of transactional behavior, making the Business Object capable of handling data creation, updates, and deletion effectively.
has context menu