# Behaviour Implementation definition

## Behaviour Implementation for CDS Interface

```ABAP
managed implementation in class zbp_ok_i_product_#### unique;
strict ( 2 );

define behavior for Z_I_PRODUCT_#### alias Product
persistent table z_d_product
lock master
authorization master ( instance )
etag master LocalChangedTime
{
  create;
  update;
  delete;

  association _Market { create; }

  field ( numbering : managed, readonly ) ProdUuid;
  field ( readonly )
  Phaseid,
  CreationTime,
  CreatedBy,
  ChangedTime,
  ChangedBy,
  LocalChangedTime;
  field ( features : instance )
  Prodid,
  Pgid,
  Height,
  Depth,
  Width,
  SizeUom,
  Price,
  PriceCurrency;

  determination set_phase_id on save { field Phaseid; create; }
  validation check_prod_id on save { field Prodid; }
  action change_phase_id result [1] $self;

  mapping for z_d_product
    {
      ProdUUID         = prod_uuid;
      ProdID           = prodid;
      PgID             = pgid;
      Phaseid          = phaseid;
      Height           = height;
      Depth            = depth;
      Width            = width;
      SizeUom          = size_uom;
      Price            = price;
      PriceCurrency    = price_currency;
      CreatedBy        = created_by;
      CreationTime     = creation_time;
      ChangedBy        = changed_by;
      ChangedTime      = changed_time;
      LocalChangedTime = local_changed_time;
    }
}

define behavior for Z_I_MARKET_#### alias Market
persistent table z_d_prod_mrkt
lock dependent by _Product
authorization dependent by _Product
etag master LocalChangedTime
{
  update;
  delete;

  association _Product;

  field ( numbering : managed, readonly ) MrktUuid;
  field ( readonly ) ProdUuid;

  mapping for z_d_prod_mrkt
    {
      ProdUuid         = prod_uuid;
      MrktUuid         = mrkt_uuid;
      Mrktid           = mrktid;
      Startdate        = startdate;
      Enddate          = enddate;
      CreatedBy        = created_by;
      CreationTime     = creation_time;
      ChangedBy        = changed_by;
      ChangedTime      = changed_time;
      LocalChangedTime = local_changed_time;
    }
}
```
## Behaviour Implementation for CDS Projection

```ABAP
projection;
strict ( 2 );

define behavior for Z_C_PRODUCT_#### alias _Product
{
  use create;
  use update;
  use delete;

  use association _Market { create; }

  use action change_phase_id;
}

define behavior for Z_C_MARKET_#### alias _Market
{
  use update;
  use delete;

  use association _Product;
}
```
## Behaviour Implementation Class for Interface

```ABAP
CLASS lhc_Product DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.
    CONSTANTS:
      BEGIN OF phase_id,
        plan TYPE int1 VALUE 1,
        dev  TYPE int1 VALUE 2,
        prod TYPE int1 VALUE 3,
        out  TYPE int1 VALUE 4,
      END OF phase_id.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR Product RESULT result.
    METHODS get_instance_features FOR INSTANCE FEATURES
      IMPORTING keys REQUEST requested_features FOR Product RESULT result.
    METHODS set_phase_id FOR DETERMINE ON SAVE
      IMPORTING keys FOR Product~set_phase_id.
    METHODS check_prod_id FOR VALIDATE ON SAVE
      IMPORTING keys FOR Product~check_prod_id.
    METHODS change_phase_id FOR MODIFY
      IMPORTING keys FOR ACTION Product~change_phase_id RESULT result.

ENDCLASS.


CLASS lhc_Product IMPLEMENTATION.
  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD get_instance_features.
    READ ENTITIES OF z_i_product_#### IN LOCAL MODE
         ENTITY Product
         FIELDS ( Prodid
                  Phaseid ) WITH CORRESPONDING #( keys )
         RESULT DATA(lt_phases)
*         ENTITY Product BY \_Market
*         FIELDS ( ProdUuid MrktUuid Enddate )
*         WITH CORRESPONDING #( keys )
*         RESULT DATA(lt_market)
         FAILED failed.
    result = VALUE #( FOR ls_phase IN lt_phases
                      ( %tky                           = ls_phase-%tky
                        %features-%field-Prodid        = COND #( WHEN ls_phase-Phaseid = phase_id-plan THEN
                                                                   if_abap_behv=>fc-f-mandatory
                                                                 WHEN ls_phase-Phaseid = phase_id-dev THEN
                                                                   if_abap_behv=>fc-f-read_only
                                                                 WHEN ls_phase-Phaseid = phase_id-prod THEN
                                                                   if_abap_behv=>fc-f-read_only
                                                                 ELSE
                                                                   if_abap_behv=>fc-f-read_only )
                        %features-%field-Pgid          = COND #( WHEN ls_phase-Phaseid = phase_id-plan THEN
                                                                   if_abap_behv=>fc-f-mandatory
                                                                 WHEN ls_phase-Phaseid = phase_id-dev THEN
                                                                   if_abap_behv=>fc-f-read_only
                                                                 WHEN ls_phase-Phaseid = phase_id-prod THEN
                                                                   if_abap_behv=>fc-f-read_only
                                                                 ELSE
                                                                   if_abap_behv=>fc-f-read_only )
                        %features-%field-Height        = COND #( WHEN ls_phase-Phaseid = phase_id-plan THEN
                                                                   if_abap_behv=>fc-f-unrestricted
                                                                 WHEN ls_phase-Phaseid = phase_id-dev THEN
                                                                   if_abap_behv=>fc-f-mandatory
                                                                 WHEN ls_phase-Phaseid = phase_id-prod THEN
                                                                   if_abap_behv=>fc-f-read_only
                                                                 ELSE
                                                                   if_abap_behv=>fc-f-read_only )
                        %features-%field-Depth         = COND #( WHEN ls_phase-Phaseid = phase_id-plan THEN
                                                                   if_abap_behv=>fc-f-unrestricted
                                                                 WHEN ls_phase-Phaseid = phase_id-dev THEN
                                                                   if_abap_behv=>fc-f-mandatory
                                                                 WHEN ls_phase-Phaseid = phase_id-prod THEN
                                                                   if_abap_behv=>fc-f-read_only
                                                                 ELSE
                                                                   if_abap_behv=>fc-f-read_only )
                        %features-%field-Width         = COND #( WHEN ls_phase-Phaseid = phase_id-plan THEN
                                                                   if_abap_behv=>fc-f-unrestricted
                                                                 WHEN ls_phase-Phaseid = phase_id-dev THEN
                                                                   if_abap_behv=>fc-f-mandatory
                                                                 WHEN ls_phase-Phaseid = phase_id-prod THEN
                                                                   if_abap_behv=>fc-f-read_only
                                                                 ELSE
                                                                   if_abap_behv=>fc-f-read_only )
                        %features-%field-SizeUom       = COND #( WHEN ls_phase-Phaseid = phase_id-plan THEN
                                                                   if_abap_behv=>fc-f-unrestricted
                                                                 WHEN ls_phase-Phaseid = phase_id-dev THEN
                                                                   if_abap_behv=>fc-f-mandatory
                                                                 WHEN ls_phase-Phaseid = phase_id-prod THEN
                                                                   if_abap_behv=>fc-f-read_only
                                                                 ELSE
                                                                   if_abap_behv=>fc-f-read_only )
                        %features-%field-Price         = COND #( WHEN ls_phase-Phaseid = phase_id-plan THEN
                                                                   if_abap_behv=>fc-f-unrestricted
                                                                 WHEN ls_phase-Phaseid = phase_id-dev THEN
                                                                   if_abap_behv=>fc-f-mandatory
                                                                 WHEN ls_phase-Phaseid = phase_id-prod THEN
                                                                   if_abap_behv=>fc-f-read_only
                                                                 ELSE
                                                                   if_abap_behv=>fc-f-read_only )
                        %features-%field-PriceCurrency = COND #( WHEN ls_phase-Phaseid = phase_id-plan THEN
                                                                   if_abap_behv=>fc-f-unrestricted
                                                                 WHEN ls_phase-Phaseid = phase_id-dev THEN
                                                                   if_abap_behv=>fc-f-mandatory
                                                                 WHEN ls_phase-Phaseid = phase_id-prod THEN
                                                                   if_abap_behv=>fc-f-read_only
                                                                 ELSE
                                                                   if_abap_behv=>fc-f-read_only ) ) ).
  ENDMETHOD.

  METHOD set_phase_id.
    READ ENTITIES OF z_i_product_#### IN LOCAL MODE ENTITY Product FIELDS ( Phaseid ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_products).
    DELETE lt_products WHERE Phaseid IS NOT INITIAL.
    IF lt_products IS INITIAL.
      RETURN.
    ENDIF.
    MODIFY ENTITIES OF z_i_product_#### IN LOCAL MODE
           ENTITY Product
           UPDATE FIELDS ( Phaseid )
           WITH VALUE #( FOR product IN lt_products
                         ( %tky = product-%tky Phaseid = '1' ) )
           REPORTED DATA(lt_update_reported).
    reported = CORRESPONDING #( DEEP lt_update_reported ).
  ENDMETHOD.

  METHOD check_prod_id.
    READ ENTITIES OF z_i_product_#### IN LOCAL MODE ENTITY Product FIELDS ( Prodid ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_products).

    DATA lt_product TYPE SORTED TABLE OF z_i_product_#### WITH UNIQUE KEY Prodid.

    lt_product = CORRESPONDING #( lt_products DISCARDING DUPLICATES MAPPING prodid = Prodid EXCEPT * ).
    DELETE lt_product WHERE prodid IS INITIAL.

    IF lt_product IS NOT INITIAL.
      " Check if ID exist
      SELECT FROM z_d_product
        FIELDS prodid
        FOR ALL ENTRIES IN @lt_product
        WHERE prodid = @lt_product-prodid
        INTO TABLE @DATA(lt_product_db).
    ENDIF.

    " Raise msg for non existing and initial Prodid
    LOOP AT lt_products INTO DATA(ls_product).
      APPEND VALUE #( %tky        = ls_product-%tky
                      %state_area = 'VALIDATE_PRODID' ) TO reported-product.
      IF ls_product-Prodid IS NOT INITIAL AND NOT line_exists( lt_product_db[ prodid = ls_product-Prodid ] ).
        CONTINUE.
      ENDIF.

      APPEND VALUE #( %tky = ls_product-%tky ) TO failed-product.

      APPEND VALUE #( %tky            = ls_product-%tky
                      %state_area     = 'VALIDATE_PRODID'
                      %msg            = COND #( WHEN ls_product-Prodid IS NOT INITIAL
                                                THEN NEW z_cx_product(
                                                             io_severity = if_abap_behv_message=>severity-error
                                                             iv_textid   = z_cx_product=>prodid_is_duplicated
                                                             iv_prodid   = ls_product-Prodid )
                                                ELSE NEW z_cx_product(
                                                             io_severity = if_abap_behv_message=>severity-error
                                                             iv_textid   = z_cx_product=>prodid_is_empty ) )
                      %element-Prodid = if_abap_behv=>mk-on )
             TO reported-product.
    ENDLOOP.
  ENDMETHOD.

  METHOD change_phase_id.
    READ ENTITIES OF z_i_product_#### IN LOCAL MODE
         ENTITY Product
         FIELDS ( ProdUuid Prodid Phaseid )
         WITH CORRESPONDING #( keys )
         RESULT DATA(lt_product)
         ENTITY Product BY \_Market
         FIELDS ( ProdUuid MrktUuid Enddate )
         WITH CORRESPONDING #( keys )
         " TODO: variable is assigned but never used (ABAP cleaner)
         RESULT DATA(lt_market)
         FAILED failed.
    " ---
    LOOP AT lt_product ASSIGNING FIELD-SYMBOL(<ls_product>).
      " -----
      IF <ls_product>-Phaseid = phase_id-plan.
        DATA(lv_next) = phase_id-dev.
      ELSEIF <ls_product>-Phaseid = phase_id-dev.
        lv_next = phase_id-prod.
      ELSEIF <ls_product>-Phaseid = phase_id-prod.
        lv_next = phase_id-out.
      ELSEIF <ls_product>-Phaseid = phase_id-out.
        lv_next = phase_id-plan.
      ELSE.
        lv_next = phase_id-plan.
      ENDIF.
      " -----
      MODIFY ENTITIES OF z_i_product_#### IN LOCAL MODE
             ENTITY Product
             UPDATE
             FIELDS ( Phaseid )
             WITH VALUE #( FOR key IN keys
                           ( %tky    = key-%tky
                             Phaseid = lv_next ) )
             FAILED   failed
             REPORTED reported.
      READ ENTITIES OF z_i_product_#### IN LOCAL MODE
           ENTITY Product
           ALL FIELDS WITH CORRESPONDING #( keys )
           RESULT DATA(ls_result).
      result = VALUE #( FOR ls_product_result IN ls_result
                        ( %tky   = ls_product_result-%tky
                          %param = ls_product_result ) ).
    ENDLOOP.
    " ---
  ENDMETHOD.
ENDCLASS.
```