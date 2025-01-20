# Other objects

## Consumer class without Outbound Service
<a name="z##_cl_consumer_without_"></a>

```ABAP
CLASS z##_cl_consumer_without_#### DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.

    INTERFACES if_oo_adt_classrun .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS z##_cl_consumer_without_#### IMPLEMENTATION.
  METHOD if_oo_adt_classrun~main.
    " TODO: variable is assigned but never used (ABAP cleaner)
    DATA lt_business_data TYPE TABLE OF z##_cm_product_market_####=>tys_i_draft_administrative_d_2.
    DATA lo_http_client   TYPE REF TO if_web_http_client.
    DATA lo_client_proxy  TYPE REF TO /iwbep/if_cp_client_proxy.
    DATA lo_request       TYPE REF TO /iwbep/if_cp_request_read_list.
    DATA lo_response      TYPE REF TO /iwbep/if_cp_response_read_lst.

*DATA:
* lo_filter_factory   TYPE REF TO /iwbep/if_cp_filter_factory,
* lo_filter_node_1    TYPE REF TO /iwbep/if_cp_filter_node,
* lo_filter_node_2    TYPE REF TO /iwbep/if_cp_filter_node,
* lo_filter_node_root TYPE REF TO /iwbep/if_cp_filter_node,
* lt_range_DRAFT_UUID TYPE RANGE OF sysuuid_x16,
* lt_range_DRAFT_ENTITY_TYPE TYPE RANGE OF <element_name>.

    TRY.
        " Create http client
*    DATA(lo_destination) = cl_http_destination_provider=>create_by_comm_arrangement(
*                                                 comm_scenario  = '<Comm Scenario>'
*                                                 comm_system_id = '<Comm System Id>'
*                                                 service_id     = '<Service Id>' ).
*    lo_http_client = cl_web_http_client_manager=>create_by_http_destination( lo_destination ).
    DATA(lo_destination) = cl_http_destination_provider=>create_by_url(
                               i_url = 'https://<skip>.abap.eu10.hana.ondemand.com'   ).
    lo_http_client = cl_web_http_client_manager=>create_by_http_destination( lo_destination ).
    lo_http_client->get_http_request( )->set_authorization_basic(
        i_username = 'test_user_####'
        i_password = '<skip>' ).
        lo_client_proxy = /iwbep/cl_cp_factory_remote=>create_v4_remote_proxy(
                              is_proxy_model_key       = VALUE #( repository_id       = 'DEFAULT'
                                                                  proxy_model_id      = 'Z##_CM_PRODUCT_MARKET_####'
                                                                  proxy_model_version = '0001' )
                              io_http_client           = lo_http_client
                              iv_relative_service_root = '/sap/opu/odata4/sap/zok_wapi_product_o4/srvd_a2x/sap/zok_wapi_product_0001/0001/' ).

        ASSERT lo_http_client IS BOUND.

        " Navigate to the resource and create a request for the read operation
        lo_request = lo_client_proxy->create_resource_for_entity_set( 'I_DRAFT_ADMINISTRATIVE_DAT' )->create_request_for_read( ).

        " Create the filter tree
*lo_filter_factory = lo_request->create_filter_factory( ).
*
*lo_filter_node_1  = lo_filter_factory->create_by_range( iv_property_path     = 'DRAFT_UUID'
*                                                        it_range             = lt_range_DRAFT_UUID ).
*lo_filter_node_2  = lo_filter_factory->create_by_range( iv_property_path     = 'DRAFT_ENTITY_TYPE'
*                                                        it_range             = lt_range_DRAFT_ENTITY_TYPE ).

*lo_filter_node_root = lo_filter_node_1->and( lo_filter_node_2 ).
*lo_request->set_filter( lo_filter_node_root ).

        lo_request->set_top( 50 )->set_skip( 0 ).

        " Execute the request and retrieve the business data
        lo_response = lo_request->execute( ).
        lo_response->get_business_data( IMPORTING et_business_data = lt_business_data ).

      CATCH /iwbep/cx_cp_remote INTO DATA(lx_remote). " TODO: variable is assigned but never used (ABAP cleaner)
        " Handle remote Exception
        " It contains details about the problems of your http(s) connection

      CATCH /iwbep/cx_gateway INTO DATA(lx_gateway). " TODO: variable is assigned but never used (ABAP cleaner)
        " Handle Exception

      CATCH cx_web_http_client_error INTO DATA(lx_web_http_client_error).
        " Handle Exception
        RAISE SHORTDUMP lx_web_http_client_error.

      CATCH cx_http_dest_provider_error.
        "handle exception
    ENDTRY.
  ENDMETHOD.
ENDCLASS.
```