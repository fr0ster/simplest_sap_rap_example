# Other objects

## Class for working with EML
<a name="z##_cl_eml_example_"></a>

```ABAP
CLASS z##_cl_eml_example_#### DEFINITION
  PUBLIC FINAL
  CREATE PUBLIC.

  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun.

  PRIVATE SECTION.
    METHODS deletes IMPORTING !out TYPE REF TO if_oo_adt_classrun_out.
    METHODS creates IMPORTING !out TYPE REF TO if_oo_adt_classrun_out.
    METHODS reads   IMPORTING !out TYPE REF TO if_oo_adt_classrun_out.
    METHODS updates IMPORTING !out TYPE REF TO if_oo_adt_classrun_out.
ENDCLASS.


CLASS z##_cl_eml_example_#### IMPLEMENTATION.
  METHOD deletes.
    DATA keys TYPE TABLE FOR DELETE z##_i_product_####.

    SELECT Prod_uuid FROM z##_d_prod_#### WHERE prodid = 'TEST-2500' INTO TABLE @DATA(lt_keys).
    keys = VALUE #( FOR ls_key IN lt_keys
                    ( %key-ProdUuid = ls_key-prod_uuid ) ).
    MODIFY ENTITIES OF z##_i_product_####
           ENTITY Product DELETE FROM keys
           " TODO: variable is assigned but never used (ABAP cleaner)
           FAILED DATA(failed_upd).
    COMMIT ENTITIES BEGIN
           RESPONSE OF z##_i_product_#### FAILED DATA(product_failed).
    IF product_failed IS NOT INITIAL.
      out->write( |ProdId TEST-2500 isn't existed| ).
    ENDIF.
    COMMIT ENTITIES END.
  ENDMETHOD.

  METHOD creates.
    MODIFY ENTITIES OF z##_i_product_####
           ENTITY Product CREATE FIELDS ( Prodid Pgid Width Depth Height Price PriceCurrency )
           WITH VALUE #( ( %cid          = 'cid1'
                           Pgid          = '1'
                           Prodid        = 'TEST-2500'
                           Width         = 10
                           Depth         = 10
                           Height        = 10
                           Price         = 300
                           PriceCurrency = 'EUR' ) )
           CREATE BY \_Market FIELDS ( Mrktid Startdate Enddate )
           WITH VALUE #( ( %cid_ref = 'cid1'
                           %target  = VALUE #( ( %cid      = 'cid2'
                                                 Mrktid    = '3'
                                                 Startdate = sy-datum
                                                 Enddate   = sy-datum + 366  ) ) ) )
           ENTITY Market CREATE BY \_Orders FIELDS ( Quantity DeliveryDate )
           WITH VALUE #( ( %cid_ref = 'cid2'
                           %target  = VALUE #( ( %cid         = 'cid3'
                                                 Quantity     = 100
                                                 DeliveryDate = sy-datum + 10 ) ) ) )
           " TODO: variable is assigned but never used (ABAP cleaner)
           MAPPED DATA(mapped_crt)
           " TODO: variable is assigned but never used (ABAP cleaner)
           REPORTED DATA(reported_crt)
           " TODO: variable is assigned but never used (ABAP cleaner)
           FAILED DATA(failed_crt).
    COMMIT ENTITIES BEGIN
           RESPONSE OF z##_i_product_#### REPORTED FINAL(reported) FAILED DATA(failed).
    IF failed-product IS NOT INITIAL.
      out->write( |ProdId TEST-2500 is existed| ).
    ENDIF.
    IF failed-market IS NOT INITIAL.
      out->write( |Market record epic fail| ).
    ENDIF.
    IF failed-order IS NOT INITIAL.
      LOOP AT failed-order INTO DATA(order_fail).
        out->write( |Order record epic fail { order_fail-%fail-cause }| ).
      ENDLOOP.
    ENDIF.
    COMMIT ENTITIES END.
  ENDMETHOD.

  METHOD reads.
    DATA keys        TYPE TABLE FOR READ IMPORT z##_i_product_####.
    DATA market_keys TYPE TABLE FOR READ IMPORT z##_i_market_####.

    SELECT Prod_uuid FROM z##_d_prod_#### WHERE prodid = 'TEST-2500' INTO TABLE @DATA(lt_keys).
    keys = VALUE #( FOR ls_key IN lt_keys
                    ( %key-ProdUuid = ls_key-prod_uuid ) ).
    READ ENTITIES OF z##_i_product_####
         ENTITY Product ALL FIELDS WITH keys RESULT DATA(product)
         ENTITY Product BY \_Market ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(market).
    market_keys = VALUE #( FOR ls_m_key IN market
                           ( CORRESPONDING #( ls_m_key ) ) ).
    out->write( product ).
    out->write( market ).
    READ ENTITIES OF z##_i_product_####
         ENTITY Market BY \_Orders ALL FIELDS WITH CORRESPONDING #( market_keys ) RESULT DATA(orders).
    out->write( orders ).
  ENDMETHOD.

  METHOD updates.
    DATA keys        TYPE TABLE FOR READ IMPORT z##_i_product_####.
    DATA market_keys TYPE TABLE FOR READ IMPORT z##_i_market_####.
    DATA order_keys  TYPE TABLE FOR READ IMPORT z##_i_order_####.

    SELECT Prod_uuid FROM z##_d_prod_#### WHERE prodid = 'TEST-2500' INTO TABLE @DATA(lt_keys).
    keys = VALUE #( FOR ls_key IN lt_keys
                    ( %key-ProdUuid = ls_key-prod_uuid ) ).
    READ ENTITIES OF z##_i_product_####
         ENTITY Product ALL FIELDS WITH keys RESULT DATA(product)
         ENTITY Product BY \_Market ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(market).
    market_keys = VALUE #( FOR ls_m_key IN market
                           ( CORRESPONDING #( ls_m_key ) ) ).
    READ ENTITIES OF z##_i_product_####
         ENTITY Market BY \_Orders ALL FIELDS WITH CORRESPONDING #( market_keys ) RESULT DATA(orders).
    order_keys = CORRESPONDING #( orders ).
    MODIFY ENTITIES OF z##_i_product_####
           ENTITY Order UPDATE FIELDS ( Quantity )
           WITH VALUE #( FOR key IN order_keys
                         ( %key-ProdUuid  = key-%key-ProdUuid
                           %key-MrktUuid  = key-%key-MrktUuid
                           %key-OrderUuid = key-%key-OrderUuid
                           Quantity       = 20 ) )
           " TODO: variable is assigned but never used (ABAP cleaner)
           FAILED DATA(failed_upd).
    COMMIT ENTITIES BEGIN
           RESPONSE OF z##_i_product_#### FAILED DATA(product_failed).
    IF product_failed IS NOT INITIAL.
      out->write( product_failed ).
    ENDIF.
    COMMIT ENTITIES END.
  ENDMETHOD.

  METHOD if_oo_adt_classrun~main.
    deletes( out ).
    creates( out ).
    " reads( out ).
    updates( out ).
    reads( out ).
  ENDMETHOD.
ENDCLASS.
```