# Notes

## Tenth Iteration: Creating OData WebAPI service and consumer for them

In this iteration, we create Inbound Service, Outbound Service and two Consumer.
First wil work without Outbound Service, second will do it with Outbound Service.
Pay attention, Inbound and Outbound Services should be created in different system for more realities.

---

### Key Objectives:

1. Create Inbound Service (optional).
2. Create consumer without Outbound Service.
3. Create consumer with Outbound Service.

---

### Pay Attention:

---

### Steps:

1. **Create Inbound Service**

- Next steps you should do in ADT
  - Create separated Abap Package for your Inbound Service
  ![inbound_package](./image/inbound_package.png)
  - Create Service Binding
  ![inbound_binding](./image/inbound_service_binding_1.png)
  ![inbound_binding](./image/inbound_service_binding_2.png)
  - Create Communication Scenario
  ![inbound_communication](./image/inbound_communication_scenario_1.png)
  ![inbound_communication](./image/inbound_communication_scenario_2.png)
  ![inbound_communication](./image/inbound_communication_scenario_3.png)
  ![inbound_communication](./image/inbound_communication_scenario_4.png)
- Next steps you should do in Abap Instance UI in Administration tab, so it's impossible on trial account :(
  - Create Communication User
  ![inbound_user](./image/inbound_user_1.png)
  ![inbound_user](./image/inbound_user_2.png)
  ![inbound_user](./image/inbound_user_3.png)
  - Create Communication System
  ![inbound_system](./image/inbound_system_1.png)
  ![inbound_system](./image/inbound_system_2.png)
  ![inbound_system](./image/inbound_system_3.png)
  ![inbound_system](./image/inbound_system_4.png)
  ![inbound_system](./image/inbound_system_5.png)
  ![inbound_system](./image/inbound_system_5.png)
  ![inbound_system](./image/inbound_system_7.png)
  - Create Communication Arrangement
  ![inbound_arrangement](./image/inbound_arrangement_1.png)
  ![inbound_arrangement](./image/inbound_arrangement_2.png)
  ![inbound_arrangement](./image/inbound_arrangement_3.png)
  ![inbound_arrangement](./image/inbound_arrangement_4.png)
  - Download Service Metadata File
  ![inbound_arrangement](./image/inbound_arrangement_4.png)

2. **Create consumer without Outbound Service**

  - Create separated Abap Package for your Consumer
  ![outbound_package](./image/outbound_package.png)
  - Download Service Metadata File
  ![oubound_arrangement](./image/inbound_arrangement_4.png)
  - Create Service Consumtion Model
  ![outbound_model](./image/outbound_model_1.png)
  ![outbound_model](./image/outbound_model_2.png)
  ![outbound_model](./image/outbound_model_3.png)
  - [Create class Consumer](./09_others.md#z##_cl_consumer_without_)
  ![outbound_class_without_service](./image/outbound_class_without_service_1.png)
  ![outbound_class_without_service](./image/outbound_class_without_service_2.png)
  ```ABAP
          " Create http client
*    DATA(lo_destination) = cl_http_destination_provider=>create_by_comm_arrangement(
*                                                 comm_scenario  = '<Comm Scenario>'
*                                                 comm_system_id = '<Comm System Id>'
*                                                 service_id     = '<Service Id>' ).
    DATA(lo_destination) = cl_http_destination_provider=>create_by_url(
                               i_url = 'https://<skip>.abap.eu10.hana.ondemand.com'   ).
    lo_http_client = cl_web_http_client_manager=>create_by_http_destination( lo_destination ).
    lo_http_client->get_http_request( )->set_authorization_basic(
        i_username = 'test_user_####'
        i_password = '<skip>' ).
  ```

  Useful links
  [Implement an Outbound Service Call in SAP BTP, ABAP environment for an OData Service via Service Consumption Model](https://developers.sap.com/tutorials/abap-environment-business-partner-outbound-call.html)

3. **Create consumer with Outbound Service**

  - Create Outbound Service
  ![outbound_service](./image/outbound_service_1.png)
  ![outbound_service](./image/outbound_service_2.png)
  - Create Communication Scenario
  ![outbound_communication](./image/outbound_communication_scenario_1.png)
  ![outbound_communication](./image/outbound_communication_scenario_2.png)
  ![outbound_communication](./image/outbound_communication_scenario_3.png)
- Next steps you should do in Abap Instance UI in Administration tab, so it's impossible on trial account :(
  - Create Communication System
  ![outbound_system](./image/outbound_system_1.png)
  ![outbound_system](./image/outbound_system_2.png)
  ![outbound_system](./image/outbound_system_3.png)
  ![outbound_system](./image/outbound_system_4.png)
  - Create Communication Arrangement
  ![inbound_arrangement](./image/outbound_arrangement_1.png)
  ![inbound_arrangement](./image/outbound_arrangement_2.png)
  ![inbound_arrangement](./image/outbound_arrangement_3.png)
  - **[Download metadata](https://sapes5.sapdevcenter.com/sap/opu/odata/sap/ZE2E100_SOL_2_SRV/$metadata)**
   [Metadata](./ZE2E100_SOL_2_SRV.xml)
  - Create Service Consumtion Model
  ![outbound_model](./image/outbound_model_with_service_1.png)
  - Create class Consumer
  ![outbound_class_with_service](./image/outbound_class_with_service_1.png)
  ![outbound_class_with_service](./image/outbound_class_with_service_2.png)
   ```ABAP
        " Create http client
        DATA(lo_destination) = cl_http_destination_provider=>create_by_comm_arrangement(
                                   comm_scenario  = 'Z####_CS_BUSINESS_PARTNER_####'
                                   comm_system_id = 'Z####_BP_COMM_SYS_####'
                                   service_id     = 'Z####_BUSINESS_PARTNER_####_REST' ).
        lo_http_client = cl_web_http_client_manager=>create_by_http_destination( lo_destination ).
        lo_client_proxy = /iwbep/cl_cp_factory_remote=>create_v2_remote_proxy(
                              is_proxy_model_key       = VALUE #( repository_id       = 'DEFAULT'
                                                                  proxy_model_id      = 'Z####_CS_BUSINESS_PARTNER_####'
                                                                  proxy_model_version = '####' )
                              io_http_client           = lo_http_client
                              iv_relative_service_root = '' ).
  ```
## Useful links
  [Implement an Outbound Service Call in SAP BTP, ABAP environment for an OData Service via Service Consumption Model](https://developers.sap.com/tutorials/abap-environment-business-partner-outbound-call.html)

---

### Final Steps:

- Activate all created objects.
- Execute class as Console Application by F9.

---

### Possible Issues and Solutions:

- [Insert potential issues and solutions]

---

### Summary:

This code explaines how work with SAP RAP BO.

### Useful links:
- [Connect to SAP S/4HANA Cloud with SAP BTP, ABAP Environment](https://developers.sap.com/mission.abap-env-connect-s4hana.html).
- [Integrate the SAP BTP, ABAP environment with SAP S/4HANA Cloud, public edition](https://developers.sap.com/group.sap-btp-abap-s4hana-integrate.html).
- [Get Data from a Remote System Using an OData Service](https://developers.sap.com/mission.abap-env-connect-2-environments.html).
  - [Service Consumption via Communication Arrangements](https://help.sap.com/docs/btp/sap-business-technology-platform/service-consumption-via-communication-arrangements).
  - [Service Consumption Model 2 for OData Client Proxy](https://blogs.sap.com/2023/11/06/service-consumption-model-2-for-odata-client-proxy/).
- [Maintain a Communication Arrangement for Inbound Communication](https://developers.sap.com/tutorials/abap-environment-communication-arrangement.html).
- [Learn How to Modernize RFC Receiver Communications into API-Based Protocols in Cloud Integration](https://developers.sap.com/tutorials/modernize-rfc-receiver.html).