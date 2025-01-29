# Notes

## Eighth Iteration: Extension

In this iteration, we enhance the user interface.

---

### Key Objectives:

1. Extend the Market data table.
2. Extend the Market CDS Projection Transactional Query.
3. Extend the Market BDEF.

---

### Pay Attention:

1. **To make our scenario more realistic, we make several assumptions:**
   
   - All modifications for adding extensibility are created in the **main package**.
   - All extensions are created in a **separate package**.

---

### Steps:

1. **[Create Dummy Append Structure for Product Data Table in Main Package](./00_tables.md#z##_s_ext_incl_prod_)**

```ABAP
@EndUserText.label : 'Extension Include'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.enhancement.fieldSuffix : 'ZPR'
@AbapCatalog.enhancement.quotaMaximumFields : 350
@AbapCatalog.enhancement.quotaMaximumBytes : 3500
define structure z##_s_ext_incld_prod_#### {
  dummy_field : abap.char(1);
}
```

```ABAP
@EndUserText.label : 'Product data'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
     " Part of code was skipped
define table z##_d_prod_#### {
     " Part of code was skipped
  include z##_s_ext_incld_prod_####;
}
```

```ABAP
@EndUserText.label : 'Draft table for entity Z##_I_PRODUCT_####'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
     " Part of code was skipped
define table z##_dt_prod_#### {
     " Part of code was skipped
  include z##_s_ext_incld_prod_####;
}
```

2. **[Create Dummy Append Structure for Market Data Table in Main Package](./00_tables.md#z##_s_ext_incl_mrkt_)**

```ABAP
@EndUserText.label : 'Extension Include'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.enhancement.fieldSuffix : 'ZMR'
@AbapCatalog.enhancement.quotaMaximumFields : 350
@AbapCatalog.enhancement.quotaMaximumBytes : 3500
define structure z##_s_ext_incld_mrkt_#### {
  dummy_field : abap.char(1);
}
```

```ABAP
@EndUserText.label : 'Markets data'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
     " Part of code was skipped
define table z##_d_mrkt_#### {
     " Part of code was skipped
  include z##_s_ext_incld_mrkt_####;
}
```

```ABAP
@EndUserText.label : 'Draft table for entity Z##_I_MARKET_####'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
     " Part of code was skipped
define table z##_dt_mrkt_#### {
     " Part of code was skipped
  include z##_s_ext_incld_mrkt_####;
}
```

3. **[Create Dummy Append Structure for Order Data Table in Main Package](./00_tables.md#z##_s_ext_incl_order_)**

```ABAP
@EndUserText.label : 'Extension Include'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.enhancement.fieldSuffix : 'ZOR'
@AbapCatalog.enhancement.quotaMaximumFields : 350
@AbapCatalog.enhancement.quotaMaximumBytes : 3500
define structure z##_s_ext_incld_order_#### {
  dummy_field : abap.char(1);
}
```

```ABAP
@EndUserText.label : 'Orders data'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
     " Part of code was skipped
define table z##_d_order_#### {
     " Part of code was skipped
  include z##_s_ext_incld_order_####;
}
```

```ABAP
@EndUserText.label : 'Draft table for entity Z##_I_ORDER_####'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
     " Part of code was skipped
define table z##_dt_ordr_#### {
     " Part of code was skipped
  include z##_s_ext_incld_order_####;
}
```

4. **[Create New Package for Extensions and Append Structure for Market Data Table](./08_extensions.md#z##_s_ext_market_status_)**

```ABAP
@EndUserText.label : 'Extension Include for Market Status'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
extend type z##_s_ext_incld_mrkt_#### with z##_s_ext_market_status_#### {
  zz_status_zmr : abap.char(5);
}
```

5. **Create Extensions for [CDS Interface](./06_behavior_definition.md#z##_i_ext_market_), [CDS Projection Transactional Interface](./06_behavior_definition.md#z##_ci_ext_market_), [CDS Projection Transactional Query](./06_behavior_definition.md#z##_c_ext_market_) Market Entity in Another Package**

```ABAP
extend view entity Z##_I_MARKET_#### with {
   Market.zz_status_zmr
}
```

```ABAP
extend view entity Z##_CI_MARKET_#### with {
   Market.zz_status_zmr
}
```

```ABAP
extend view entity Z##_C_MARKET_#### with {
   Market.zz_status_zmr
}
```

6. **[Add Extensibility to Behavior Definition for Interface](./06_behavior_definition.md#z##_i_product_)**

- **Header**

```ABAP
managed with additional save implementation in class zbp_##_i_product_#### unique;
strict ( 2 );
extensible
{
  with determinations on modify;
  with determinations on save;
  with validations on save;
  with additional save;
}
with draft;
```

- **Product Entity**

```ABAP
define behavior for Z##_I_PRODUCT_#### alias Product
     " Part of code was skipped
extensible
     " Part of code was skipped
{
     " Part of code was skipped
  draft determine action Prepare extensible
  {
     " Part of code was skipped
  }

     " Part of code was skipped

  mapping for z##_d_prod_#### corresponding extensible
    {
     " Part of code was skipped
    }
}
```

- **Market Entity**

```ABAP
define behavior for Z##_I_MARKET_#### alias Market
     " Part of code was skipped
extensible
     " Part of code was skipped
{
     " Part of code was skipped

  mapping for z##_d_mrkt_#### corresponding extensible
    {
     " Part of code was skipped
    }
}
```

- **Order Entity**

```ABAP
define behavior for Z##_I_ORDER_#### alias Order
     " Part of code was skipped
extensible
     " Part of code was skipped
{
     " Part of code was skipped

  mapping for z##_d_order_#### corresponding extensible
    {
     " Part of code was skipped
    }
}
```

7. **[Add Extensibility to Behavior Definition for Projection Transactional Interface](./06_behavior_definition.md#z##_ci_product_)**

- Header

```ABAP
interface;
extensible;
use draft;
use side effects;
```

8. **[Add Extensibility to Behavior Definition for Projection Transactional Query](./06_behavior_definition.md#z##_c_product_)**

- **Header**

```ABAP
projection implementation in class zbp_##_c_product_#### unique;
strict ( 2 );
extensible;
use draft;
use side effects;
```

- **Product Entity**

```ABAP
define behavior for Z##_C_PRODUCT_#### alias Product
extensible
{
     " Part of code was skipped
}
```

- **Market Entity**

```ABAP
define behavior for Z##_C_MARKET_#### alias Market
extensible
{
     " Part of code was skipped
}
```

- **Order Entity**

```ABAP
define behavior for Z##_C_ORDER_#### alias Order
extensible
{
     " Part of code was skipped
}
```

9. **Create Extension for Behavior Implementation in Another Package**

- **[Create Extension for CDS Interface, Here You Should Use CDS Projection Transactional Interface or Not](./08_extensions.md#z##_##_i_ext_product_)**

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

- **[Create Class Implementation for Extension](./08_extensions.md#zbp_##_i_ext_product_)**
Create global table, which are used for passing info from action to saver class
```ABAP
CLASS zbp_##_i_ext_product_#### DEFINITION PUBLIC ABSTRACT FINAL FOR BEHAVIOR OF z##_i_product_####.
  PUBLIC SECTION.
    CLASS-DATA mt_products TYPE TABLE OF sysuuid_x16.
ENDCLASS.
```
Create class for events raising
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
```
Create class for new determination
```ABAP
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
```
And finally class for new action
```ABAP
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
```

- **[Create Metadata Extension for Actions, Add Button](./08_extensions.md#z##_c_ext_product_me)**
Pay attention, annotation in Metadata Extension are stacked, so use correct **@Metadata.layer**, **#PARTNER** overwrite all annotation for field from **#CORE**
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

- **[Create Metadata Extension for New Fields](./08_extensions.md#z##_c_ext_market_me)**
Pay attention, annotation in Metadata Extension are stacked, so use correct **@Metadata.layer**, **#PARTNER** overwrite all annotation for field from **#CORE**
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

- **[Create Extension for CDS Projection Transactional Query](./08_extensions.md#z##_c_ext_product_)**

```ABAP
extension for projection;

extend behavior for Product
{
  use action zz_tester;
}

extend behavior for Market
{
}

extend behavior for Order
{
}
```

---

### Final Steps:

- Activate all updated BDEFs.
- Test the updated services in the Fiori application to ensure proper integration and functionality.

---

### Possible Issues and Solutions:

- [Insert potential issues and solutions]

---

### Summary:

This iteration enabled extensibility and created extensions.