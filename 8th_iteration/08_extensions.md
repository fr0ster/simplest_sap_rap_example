# CDS, type and other extensions

## Market data Append structure
<a name="z##_s_ext_market_status_"></a>
Z##_S_EXT_MARKET_STATUS_####

```ABAP
@EndUserText.label : 'Extension include for Market Status'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
extend type z##_s_ext_incld_mrkt_#### with z##_s_ext_market_status_#### {

  zz_status_zmr : abap.char(5);

}
```

## CDS Interface for root view entity Market Extension
<a name="z##_i_ext_market_"></a>
Z##_I_EXT_MARKET_####

```ABAP
extend view entity Z##_I_MARKET_#### with {
   Market.zz_status_zmr
}
```

## CDS Projection Transacional Interface for root view entity Market Extension
<a name="z##_ci_ext_market_"></a>
Z##_CI_EXT_MARKET_####

```ABAP
extend view entity Z##_CI_MARKET_#### with {
   Market.zz_status_zmr
}
```

## CDS Projection Transacional Query for root view entity Market Extension
<a name="z##_c_ext_market_"></a>
Z##_C_EXT_MARKET_####

```ABAP
extend view entity Z##_C_MARKET_#### with {
   Market.zz_status_zmr
}
```

# Interface BDef extension (and BDef Projection Transactional Interface)
<a name="z##_##_i_ext_product_"></a>
Z##_##_I_EXT_PRODUCT_####

```ABAP
extension using interface z##_ci_product_####
implementation in class zbp_##_i_ext_product_#### unique;

extend behavior for Product
with additional save
{
  action ( authorization : instance ) zz_tester result [1] $self;
  event zz_testEvent2 parameter Z##_A_PARAMS_####;
}

extend behavior for Market
{
  determination zz_setStatus on save { create; field zz_status_zmr; }
}
```

# Class implementation zbp_##_i_ext_product_####
<a name="zbp_##_i_ext_product_"></a>
ZBP_##_I_EXT_PRODUCT_####

```ABAP
CLASS zbp_##_i_ext_product_#### DEFINITION PUBLIC ABSTRACT FINAL FOR BEHAVIOR OF z##_i_product_####.
  PUBLIC SECTION.
    CLASS-DATA mt_products TYPE TABLE OF sysuuid_x16.
ENDCLASS.

CLASS zbp_##_i_ext_product_#### IMPLEMENTATION.
ENDCLASS.
```
```ABAP
CLASS lsc_z##_i_ext_product_#### DEFINITION INHERITING FROM cl_abap_behavior_saver.

  PROTECTED SECTION.

    METHODS save_modified REDEFINITION.

ENDCLASS.

CLASS lsc_z##_i_ext_product_#### IMPLEMENTATION.
  METHOD save_modified.
    CHECK zbp_##_i_ext_product_####=>mt_products IS NOT INITIAL.
    IF create-product IS NOT INITIAL.
      " Event defined in BDEF: event created;
      RAISE ENTITY EVENT z##_ci_product_####~zz_testEvent2
            FROM VALUE #( FOR <cr> IN create-product
                          ( %key   = VALUE #( ProdUuid = <cr>-ProdUuid )
                            %param = VALUE #( p_1 = '001' )  ) ).
    ELSEIF update-product IS NOT INITIAL.
      " Event defined in BDEF: event updated parameter some_abstract_entity;
      RAISE ENTITY EVENT z##_ci_product_####~zz_testEvent2
            FROM VALUE #( FOR <upd> IN update-product
                          ( %key   = VALUE #( ProdUuid = <upd>-ProdUuid )
                            %param = VALUE #( p_1 = '002' ) ) ).
    ELSEIF delete-product IS NOT INITIAL.
      " Event defined in BDEF: event deleted parameter some_abstract_entity;
      RAISE ENTITY EVENT z##_ci_product_####~zz_testEvent2
            FROM VALUE #( FOR <del> IN delete-product
                          ( %key   = VALUE #( ProdUuid = <del>-ProdUuid )
                            %param = VALUE #( p_1 = '003' ) ) ).
    ELSE.
      RAISE ENTITY EVENT z##_ci_product_####~zz_testEvent2
            FROM VALUE #( FOR lv_key IN zbp_##_i_ext_product_####=>mt_products[]
                          ( %key   = VALUE #( ProdUuid = lv_key )
                            %param = VALUE #( p_1 = '004' ) ) ).
    ENDIF.
    CLEAR zbp_##_i_ext_product_####=>mt_products[].
  ENDMETHOD.

ENDCLASS.

CLASS lhc_product DEFINITION INHERITING FROM cl_abap_behavior_handler.

  PRIVATE SECTION.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR Product RESULT result.

    METHODS zz_tester FOR MODIFY
      IMPORTING keys FOR ACTION Product~zz_tester RESULT result.

ENDCLASS.

CLASS lhc_product IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD zz_tester.
    READ ENTITIES OF z##_ci_product_#### IN LOCAL MODE
         ENTITY Product
         ALL FIELDS WITH CORRESPONDING #( keys )
         RESULT DATA(lt_result).
    zbp_##_i_ext_product_####=>mt_products[] = VALUE #( FOR ls_result IN lt_result
                                                        (  ls_result-ProdUuid ) ).
    result = VALUE #( FOR ls_product_result IN lt_result
                      ( %tky   = ls_product_result-%tky
                        %param = ls_product_result ) ).
  ENDMETHOD.

ENDCLASS.

CLASS lhc_market DEFINITION INHERITING FROM cl_abap_behavior_handler.

  PRIVATE SECTION.

    METHODS zz_setStatus FOR DETERMINE ON SAVE
      IMPORTING keys FOR Market~zz_setStatus.

ENDCLASS.

CLASS lhc_market IMPLEMENTATION.

  METHOD zz_setStatus.
    READ ENTITIES OF z##_i_product_#### ENTITY Market FIELDS ( zz_status_zmr ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_markets).
    DELETE lt_markets WHERE zz_status_zmr IS NOT INITIAL.
    IF lt_markets IS INITIAL.
      RETURN.
    ENDIF.
    MODIFY ENTITIES OF z##_i_product_####
           ENTITY Market
           UPDATE FIELDS ( zz_status_zmr )
           WITH VALUE #( FOR ls_market IN lt_markets
                         ( %tky = ls_market-%tky zz_status_zmr = '12345' ) )
           REPORTED DATA(lt_update_reported).
    reported = CORRESPONDING #( DEEP lt_update_reported ).
  ENDMETHOD.

ENDCLASS.

*"* use this source file for the definition and implementation of
*"* local helper classes, interface definitions and type
*"* declarations
```

# Test Event Handler for extension
<a name="z##_cl_ext_event_handler_"></a>
Z##_CL_EXT_EVENT_HANDLER_####

```ABAP
CLASS z##_cl_ext_event_handler_#### DEFINITION
  PUBLIC ABSTRACT FINAL
  FOR EVENTS OF z##_ci_product_####.

  PUBLIC SECTION.
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS z##_cl_ext_event_handler_#### IMPLEMENTATION.
ENDCLASS.
```
```ABAP
*"* use this source file for the definition and implementation of
*"* local helper classes, interface definitions and type
*"* declarations

CLASS lcl_local_event_consumption DEFINITION INHERITING FROM cl_abap_behavior_event_handler.
  PRIVATE SECTION.
    METHODS consume_event_1 FOR ENTITY EVENT IMPORTING params FOR Product~zz_testEvent2.
ENDCLASS.


CLASS lcl_local_event_consumption IMPLEMENTATION.
  METHOD consume_event_1.
    CHECK params IS NOT INITIAL.
  ENDMETHOD.
ENDCLASS.
```

# BDef Projection Transactional Query
<a name="z##_c_ext_product_"></a>
Z##_C_EXT_PRODUCT_####

```ABAP
extension for projection;

extend behavior for Product
{
}

extend behavior for Market
{
}

extend behavior for Order
{
}
```

# Metadata Estension for Z##_C_MARKET_#### 
<a name="z##_c_ext_market_me"></a>
Z##_C_EXT_MARKET_####

```ABAP
@Metadata.layer: #PARTNER

annotate entity Z##_C_MARKET_####
    with

{
  @UI.fieldGroup: [ { position: 35, qualifier: 'MarketCharacteristics', label: 'ZZ_STATUS' } ]
  @UI.lineItem: [ { position: 75, label: 'ZZ_STATUS' } ]
  zz_status_zmr;
}
```

# Metadata Estension for Z##_C_PRODUCT_#### 
<a name="z##_c_ext_product_me"></a>
Z##_C_EXT_PRODUCT_####

```ABAP
@Metadata.layer: #PARTNER

annotate entity Z##_C_PRODUCT_####
    with

{
  @UI.identification: [ { type: #FOR_ACTION, dataAction: 'nextPhase',  label: 'Next Phase',  position: 10 },
                        { type: #FOR_ACTION, dataAction: 'priorPhase', label: 'Prior Phase', position: 20 },
                        { type: #FOR_ACTION, dataAction: 'zz_tester',  label: 'Test Event',  position: 30 } ]

  @UI.lineItem: [ { type: #FOR_ACTION, dataAction: 'nextPhase',  label: 'Next Phase',  position: 10 },
                  { type: #FOR_ACTION, dataAction: 'priorPhase', label: 'Prior Phase', position: 20 },
                  { type: #FOR_ACTION, dataAction: 'zz_tester',  label: 'Test Event',  position: 30 } ]
  ProdUuid;
}
```