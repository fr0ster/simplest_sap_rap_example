# Developing a Comprehensive SAP RAP Application

This guide provides a systematic and iterative process for building a robust SAP RAP application. Each iteration builds upon the last, focusing on essential elements such as data modeling, behavior implementation, user interface design, and advanced functionalities like Draft Handling and OData services.

---

## [Iteration 1: Read-Only Application](./1st_iteration/notes.md)

The first iteration establishes the foundation of the application:

- **Data Model**:
  - Define the core **Business Object (BO)** and its primary entities.
  - Create **basic associations** to represent relationships between entities.
- **Metadata Extension**:
  - Configure the **List Report** and **Object Page** with minimal annotations for navigation and presentation.

---

## [Iteration 2: Behavior Implementation](./2ond_iteration/notes.md)

This iteration introduces transaction capabilities to the business object:

- **Behavior Definition (BDEF)**:
  - Configure the behavior as **Managed**.
  - Enable standard transactional operations, including **Create**, **Update**, and **Delete** (CUD).
- **Behavior Implementation (Class)**:
  - Add **validations** to ensure data consistency.
  - Implement **determinations** to automatically update related fields when necessary.

---

## [Iteration 3: Third-Level Composition](./3rd_iteration/notes.md)

This iteration extends the data model by incorporating hierarchical relationships:

- Introduce a **third-level entity** to manage Orders data.
- Establish a **new association** between the Market object and the new entity, integrating both interface and projection layers.

---

## [Iteration 4: Data Model Refinements](./4th_iteration/notes.md)

Refinements to the data model are the focus of this iteration:

- Add **associations** to connect Criticality Thresholds for calculating Criticality Levels via separate settings tables.
- Develop a service to manage Criticality Thresholds data.
- Include **calculated fields** to dynamically derive values based on existing data.
- Add **determinations** and **validations**.

---

## [Iteration 5: Business Model Enhancements](./5th_iteration/notes.md)

This iteration focuses on improving the business model:

- Extract **Projection Transactional Interface** from **Projection Transactional Query**.
- Enable **search functionality** within the **List Report**.
- Enhance the data model with ValueHelper annotations.
- Add **actions** at both the **Data Model level** and the **Business Model level**.
- Add **events** at the **Data Model level** and local handler for testing.

---

## [Iteration 6: UI Enhancements and Annotations](./6th_iteration/notes.md)

User interface improvements are prioritized in this iteration:

- Customize the **Object Page** header to display critical details.
- Utilize **FieldGroup** annotations to logically group related fields.
- Add more complex **Facets** for better organization and layout of the UI.
- Implement **field-level annotations** for labels, tooltips, and formatting.
- Configure **dynamic visibility** and field controls to enhance the user experience.

---

## [Iteration 7: Draft Handling](./7th_iteration/notes.md)

Draft Handling is introduced to manage intermediate data states:

- Enable **Draft Behavior** within the Behavior Definition (BDEF).
- Add logic to handle draft-specific actions like saving and discarding changes.
- Test the draft functionality across the user interface and backend.

---

## [Iteration 8: Extension](./8th_iteration/notes.md)

The eigth iteration integrates a side-by-side extension:
- Extension Market data table.
- Extension Market CDS Projection Transactional Query.
- Extension Market BDEF.

---

## [Iteration 9: Working of SAP RAP BO with EML](./9th_iteration/notes.md)

This iteration focuses on improving application performance and resolving issues:

- Creating.
- Reading.
- List reading.
- Updating.
- Deleting.

---

## [Iteration 10: Creating OData WebAPI service and consumer for them](./10th_iteration/notes.md)

This iteration focuses on improving application performance and resolving issues:


---

### Application Evolution

1. **Foundational Read-Only Setup**: Begin with a simple read-only application in Iteration 1.
2. **Transactional Features**: Iterations 2 through 5 enhance functionality with deeper data relationships and transaction support.
3. **UI Refinements and Drafts**: Iterations 6 and 7 improve user experience and introduce draft-saving capabilities.
4. **External Integration**: Iteration 8 completes the solution with advanced OData V4 extensions for broader system integration.
5. **Optimization and Debugging**: Iteration 9 ensures the application is robust, efficient, and scalable.

By following this step-by-step process, the application matures into a full-featured SAP RAP solution, capable of handling complex requirements and external integrations with ease.


### Useful links
- **Abap&BTP**
  1. [ABAP - Keyword Documentation](https://help.sap.com/doc/abapdocu_cp_index_htm/CLOUD/en-US/ABENABAP.html).
  2. [ABAP RESTful Programming Model](https://help.sap.com/docs/CP/c0d02c4330c34b3abca88bdd57eaccfc/3b77569ca8ee4226bdab4fcebd6f6ea6.html)
  3. [ABAP Cheat Sheets](https://github.com/SAP-samples/abap-cheat-sheets.git).
  4. [Troubleshooting Tools for RAP-based Apps](https://pages.community.sap.com/topics/abap-testing-analysis/troubleshooting).
  5. [ABAP Data Models](https://help.sap.com/docs/abap-cloud/abap-data-models/abap-data-models)

- **Fiori App**
  1. [Create a SAP Fiori App and Deploy it to SAP BTP, Cloud Foundry environment](https://developers.sap.com/tutorials/abap-environment-deploy-cf..html).
  2. [Create a SAP Fiori App and Deploy it to SAP BTP, ABAP Environment](https://developers.sap.com/tutorials/abap-environment-deploy-cf-production.html).
  3. [Create a SAP Fiori App in Visual Studio Code and Deploy it to SAP BTP, ABAP Environment](https://developers.sap.com/tutorials/abap-environment-vs-code.html).
  4. [SAP BTP ABAP Environment: Level Up](https://developers.sap.com/mission.abap-env-level-up.html).

- **SAP Hana**
  1. [Connect to SAP S/4HANA Cloud with SAP BTP, ABAP Environment](https://developers.sap.com/mission.abap-env-connect-s4hana.html).
  2. [Integrate the SAP BTP, ABAP environment with SAP S/4HANA Cloud, public edition](https://developers.sap.com/group.sap-btp-abap-s4hana-integrate.html).
  3. [hana-opensap-cloud-2020](https://github.com/SAP-samples/hana-opensap-cloud-2020).

- **Integration**
  1. [Expose an ABAP CDS View Externally Using a Communication Scenario](https://developers.sap.com/group.abap-env-first-app.html).
    - [Expose a Standard Core Data Service for ABAP Environment](https://developers.sap.com/tutorials/abap-environment-business-service-provisioning.html).
    - [Maintain a Communication Arrangement for Inbound Communication](https://developers.sap.com/tutorials/abap-environment-communication-arrangement.html).
  2. SAP Help Portal: [Service Consumption via Communication Arrangements](https://help.sap.com/docs/btp/sap-business-technology-platform/service-consumption-via-communication-arrangements).
  3. SAP Community blog post: [Service Consumption Model 2 for OData Client Proxy](https://blogs.sap.com/2023/11/06/service-consumption-model-2-for-odata-client-proxy/).

- **Tools&CLI**
  1. [Download and Start Using the btp CLI Client](https://help.sap.com/docs/btp/sap-business-technology-platform/download-and-start-using-btp-cli-client).
  2. [Get Started with the SAP BTP Command Line Interface (btp CLI)](https://developers.sap.com/tutorials/cp-sapcp-getstarted..html).
  3. [SAP BTP Command Line Setup and Cloud Foundry Setup](https://community.sap.com/t5/technology-blogs-by-members/sap-btp-command-line-setup-and-cloud-foundry-setup/ba-p/13587706).
  4. [Account Administration Using the SAP BTP Command Line Interface (btp CLI)](https://help.sap.com/docs/btp/sap-business-technology-platform/account-administration-using-sap-btp-command-line-interface-btp-cli).
  5. [Install the Cloud Foundry Command Line Interface (CLI)](https://developers.sap.com/tutorials/cp-cf-download-cli..html).
  6. [Cloud Foundry CLI](https://github.com/cloudfoundry/cli).

- **Best Practices**
  1. [SAP ABAP Programming Best Practices](https://www.linkedin.com/learning/sap-abap-programming-best-practices/).

- **[SAP Tutorials](https://developers.sap.com/tutorial-navigator..html)**
  - **General**
    1. [SAP BTP, ABAP environment](https://developers.sap.com/tutorial-navigator.html?tag=software-product%3Atechnology-platform%2Fsap-business-technology-platform%2Fsap-btp-abap-environment).
    2. [SAP Integration Suite](https://developers.sap.com/tutorial-navigator.html?tag=software-product%3Atechnology-platform%2Fsap-business-technology-platform%2Fsap-integration-suite).

  - **Fiori**
    1. [Welcome to the Developing and Extending SAP Fiori Elements Apps openSAP samples](https://github.com/SAP-samples/fiori-elements-opensap/tree/main).

  - **CAP**
    1. [Build a Business Application Using CAP for Java](https://developers.sap.com/mission.cap-java-app.html).
    2. [Combine CAP with SAP HANA Cloud to Create Full-Stack Applications](https://developers.sap.com/mission.hana-cloud-cap.html).

  - **SAP Hana**
    1. [Get Started with SAP HANA Cloud](https://developers.sap.com/mission.hana-cloud-get-started.html).
    2. [SAP HANA Cloud](https://developers.sap.com/tutorial-navigator.html?tag=software-product%3Atechnology-platform%2Fsap-hana-cloud%2Fsap-hana-cloud).
    3. [Use Clients to Query an SAP HANA Database](https://developers.sap.com/mission.hana-cloud-clients.html).
    4. [Python Analysis with Multi-Model Data in SAP HANA Cloud](https://developers.sap.com/group.hana-cloud-database-python-multi-model.html).

  - **Kyma Runtime**
    1. [Develop a Full-Stack Application in SAP BTP, Kyma Runtime](https://developers.sap.com/mission.cp-kyma-full-stack.html).
  
  - **Other topics**
    1. [Create an Application with the Luigi Micro Frontend Framework](https://developers.sap.com/group.luigi-app.html)