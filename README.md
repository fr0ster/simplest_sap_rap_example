# Developing a Comprehensive SAP RAP Application

This guide provides a systematic and iterative process for building a robust SAP RAP application. Each iteration builds upon the last, focusing on essential elements such as data modeling, behavior implementation, user interface design, and advanced functionalities like Draft Handling and OData services.

---

## [Iteration 1: Read-Only Application](./1st_iteration/notes.md)

The first iteration establishes the foundation of the application:

- **Define Business Object (BO)** with primary entities.
- **Create basic associations** to represent relationships.
- **Configure List Report and Object Page** with minimal annotations.

---

## [Iteration 2: Behavior Implementation](./2ond_iteration/notes.md)

This iteration introduces transaction capabilities:

- **Define behavior (BDEF)** as Managed with Create, Update, Delete (CUD).
- **Implement validations and determinations** to ensure consistency and auto-update fields.

---

## [Iteration 3: Third-Level Composition](./3rd_iteration/notes.md)

Extending data model with hierarchical relationships:

- **Introduce third-level entity** to manage Orders.
- **Establish new associations** between Market object and new entity.

---

## [Iteration 4: Data Model Refinements](./4th_iteration/notes.md)

Refining the data model:

- **Add associations** for Criticality Thresholds.
- **Develop a service** to manage Thresholds data.
- **Implement calculated fields** and additional validations.

---

## [Iteration 5: Business Model Enhancements](./5th_iteration/notes.md)

Enhancing business model:

- **Extract Projection Transactional Interface**.
- **Enable search functionality** in List Report.
- **Add actions and events** at different model levels.

---

## [Iteration 6: UI Enhancements and Annotations](./6th_iteration/notes.md)

Improving user experience:

- **Customize Object Page header**.
- **Group fields** with FieldGroup annotations.
- **Enhance UI structure** with facets and formatting.

---

## [Iteration 7: Draft Handling](./7th_iteration/notes.md)

Implementing draft functionality:

- **Enable Draft Behavior** in BDEF.
- **Implement draft actions** like save and discard.

---

## [Iteration 8: Extension](./8th_iteration/notes.md)

Enhancing extensibility:

- **Extend Market data table, CDS projection, and BDEF**.

---

## [Iteration 9: Working of SAP RAP BO with EML](./9th_iteration/notes.md)

Ensuring performance and issue resolution:

- **CRUD operations** using EML.

---

## [Iteration 10: Creating OData WebAPI service and consumer for them](./10th_iteration/notes.md)

Finalizing application with API exposure:

- **Create OData service**.
- **Implement consumer** to interact with the service.

---

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

- **SAP CDS**

  1. [CDS : How to define Hierarchy View](https://community.sap.com/t5/technology-blogs-by-sap/cds-how-to-define-hierarchy-view/ba-p/13758059).

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

- **SAP Kyme**

  1. [Access a Kyma Instance Using kubectl](https://help.sap.com/docs/btp/sap-business-technology-platform/access-kyma-instance-using-kubectl).
  2. [Develop a Full-Stack Application in SAP BTP, Kyma Runtime](https://developers.sap.com/mission.cp-kyma-full-stack.html).
  3. [Consume SAP BTP Services In SAP Kyma](https://developers.sap.com/tutorials/btp-kyma-extension.html).
  4. [Use Redis in SAP BTP, Kyma Runtime to Store and Retrieve Data](https://developers.sap.com/tutorials/cp-kyma-redis-function.html).

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
