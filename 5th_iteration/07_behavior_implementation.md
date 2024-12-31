# Behaviour Implementations

## Behaviour Implementation for CDS  Product BO Interface
<a name="z##_i_product_"></a>

```ABAP
CLASS lsc_z##_i_product_#### DEFINITION INHERITING FROM cl_abap_behavior_saver.

  PROTECTED SECTION.

    METHODS save_modified REDEFINITION.

ENDCLASS.

CLASS lsc_z##_i_product_#### IMPLEMENTATION.
  METHOD save_modified.
    IF create-product IS NOT INITIAL.
      " Event defined in BDEF: event created;
      RAISE ENTITY EVENT z##_i_product_####~testEvent
            FROM VALUE #( FOR <cr> IN create-product
                          ( %key   = VALUE #( ProdUuid = <cr>-ProdUuid )
                            %param = VALUE #( p_1 = '001' )  ) ).
    ENDIF.

    IF update-product IS NOT INITIAL.
      " Event defined in BDEF: event updated parameter some_abstract_entity;
      RAISE ENTITY EVENT z##_i_product_####~testEvent
            FROM VALUE #( FOR <upd> IN update-product
                          ( %key   = VALUE #( ProdUuid = <upd>-ProdUuid )
                            %param = VALUE #( p_1 = '001' ) ) ).
    ENDIF.

    IF delete-product IS NOT INITIAL.
      " Event defined in BDEF: event deleted parameter some_abstract_entity;
      RAISE ENTITY EVENT z##_i_product_####~testEvent
            FROM VALUE #( FOR <del> IN delete-product
                          ( %key   = VALUE #( ProdUuid = <del>-ProdUuid )
                            %param = VALUE #( p_1 = '001' ) ) ).
    ENDIF.
  ENDMETHOD.

ENDCLASS.

CLASS lhc_order DEFINITION INHERITING FROM cl_abap_behavior_handler.

  PRIVATE SECTION.

    METHODS setAmountcurr FOR DETERMINE ON SAVE
      IMPORTING keys FOR Order~setAmountcurr.

    METHODS checkGrossamount FOR VALIDATE ON SAVE
      IMPORTING keys FOR Order~checkGrossamount.

    METHODS checkNetamount FOR VALIDATE ON SAVE
      IMPORTING keys FOR Order~checkNetamount.

    METHODS checkQuantity FOR VALIDATE ON SAVE
      IMPORTING keys FOR Order~checkQuantity.

ENDCLASS.

CLASS lhc_order IMPLEMENTATION.

  METHOD setAmountcurr.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE
    ENTITY Order FIELDS ( Amountcurr )
    WITH CORRESPONDING #( keys ) RESULT DATA(lt_orders).
    DELETE lt_orders WHERE Amountcurr IS NOT INITIAL.
    IF lt_orders IS INITIAL.
      RETURN.
    ENDIF.
    MODIFY ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Order
           UPDATE FIELDS ( Amountcurr )
           WITH VALUE #( FOR product IN lt_orders
                         ( %tky = product-%tky Amountcurr = 'USD' ) )
           REPORTED DATA(lt_update_reported).
    reported = CORRESPONDING #( DEEP lt_update_reported ).
  ENDMETHOD.

  METHOD checkGrossamount.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE
         ENTITY Market
         FIELDS ( Startdate
                  Enddate ) WITH CORRESPONDING #( keys )
         RESULT DATA(lt_marketdate).

    LOOP AT lt_marketdate ASSIGNING FIELD-SYMBOL(<ls_marketdate>).
      READ ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Market BY \_Orders
           FIELDS ( Grossamount ) WITH VALUE #( ( %tky = <ls_marketdate>-%tky )  )
           RESULT DATA(lt_orderdate).

      LOOP AT lt_orderdate ASSIGNING FIELD-SYMBOL(<ls_orderdate>).
        APPEND VALUE #( %tky        = <ls_orderdate>-%tky
                        %state_area = 'CHECKQGROSSAMOUNT' )
               TO reported-order.

        IF     <ls_orderdate>-Grossamount >= 0.
          CONTINUE.
        ENDIF.

        APPEND VALUE #( %tky = <ls_orderdate>-%tky ) TO failed-order.

        APPEND VALUE #( %tky                  = <ls_orderdate>-%tky
                        %state_area           = 'CHECKQGROSSAMOUNT'
                        %msg                  = new_message_with_text( severity = if_abap_behv_message=>severity-error text = 'Grossamount less than zero' )
                        %element-Grossamount = if_abap_behv=>mk-on )
               TO reported-order.
      ENDLOOP.
    ENDLOOP.
  ENDMETHOD.

  METHOD checkNetamount.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE
         ENTITY Market
         FIELDS ( Startdate
                  Enddate ) WITH CORRESPONDING #( keys )
         RESULT DATA(lt_marketdate).

    LOOP AT lt_marketdate ASSIGNING FIELD-SYMBOL(<ls_marketdate>).
      READ ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Market BY \_Orders
           FIELDS ( Netamount ) WITH VALUE #( ( %tky = <ls_marketdate>-%tky )  )
           RESULT DATA(lt_orderdate).

      LOOP AT lt_orderdate ASSIGNING FIELD-SYMBOL(<ls_orderdate>).
        APPEND VALUE #( %tky        = <ls_orderdate>-%tky
                        %state_area = 'CHECKNETAMOUNT' )
               TO reported-order.

        IF     <ls_orderdate>-Netamount >= 0.
          CONTINUE.
        ENDIF.

        APPEND VALUE #( %tky = <ls_orderdate>-%tky ) TO failed-order.

        APPEND VALUE #( %tky                  = <ls_orderdate>-%tky
                        %state_area           = 'CHECKNETAMOUNT'
                        %msg                  = new_message_with_text( severity = if_abap_behv_message=>severity-error text = 'Netamount less than zero' )
                        %element-Netamount = if_abap_behv=>mk-on )
               TO reported-order.
      ENDLOOP.
    ENDLOOP.
  ENDMETHOD.

  METHOD checkQuantity.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE
         ENTITY Market
         FIELDS ( Startdate
                  Enddate ) WITH CORRESPONDING #( keys )
         RESULT DATA(lt_marketdate).

    LOOP AT lt_marketdate ASSIGNING FIELD-SYMBOL(<ls_marketdate>).
      READ ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Market BY \_Orders
           FIELDS ( Quantity ) WITH VALUE #( ( %tky = <ls_marketdate>-%tky )  )
           RESULT DATA(lt_orderdate).

      LOOP AT lt_orderdate ASSIGNING FIELD-SYMBOL(<ls_orderdate>).
        APPEND VALUE #( %tky        = <ls_orderdate>-%tky
                        %state_area = 'CHECKQUANTITY' )
               TO reported-order.

        IF <ls_orderdate>-Quantity >= 0.
          CONTINUE.
        ENDIF.

        APPEND VALUE #( %tky = <ls_orderdate>-%tky ) TO failed-order.

        APPEND VALUE #( %tky              = <ls_orderdate>-%tky
                        %state_area       = 'CHECKQUANTITY'
                        %msg              = new_message_with_text( severity = if_abap_behv_message=>severity-error
                                                                   text     = 'Quantity less than zero' )
                        %element-Quantity = if_abap_behv=>mk-on )
               TO reported-order.
      ENDLOOP.
    ENDLOOP.
  ENDMETHOD.

ENDCLASS.

CLASS lhc_market DEFINITION INHERITING FROM cl_abap_behavior_handler.

  PRIVATE SECTION.

    METHODS setEnddate FOR DETERMINE ON SAVE
      IMPORTING keys FOR Market~setEnddate.

    METHODS setStartdate FOR DETERMINE ON SAVE
      IMPORTING keys FOR Market~setStartdate.

ENDCLASS.

CLASS lhc_market IMPLEMENTATION.

  METHOD setEnddate.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE ENTITY Market FIELDS ( Enddate ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_markets).
    DELETE lt_markets WHERE Enddate IS NOT INITIAL.
    IF lt_markets IS INITIAL.
      RETURN.
    ENDIF.
    MODIFY ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Market
           UPDATE FIELDS ( Enddate )
           WITH VALUE #( FOR product IN lt_markets
                         ( %tky = product-%tky Enddate = cl_abap_context_info=>get_system_date(  ) + 366 ) )
           REPORTED DATA(lt_update_reported).
    reported = CORRESPONDING #( DEEP lt_update_reported ).
  ENDMETHOD.

  METHOD setStartdate.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE ENTITY Market FIELDS ( Startdate ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_markets).
    DELETE lt_markets WHERE Startdate IS NOT INITIAL.
    IF lt_markets IS INITIAL.
      RETURN.
    ENDIF.
    MODIFY ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Market
           UPDATE FIELDS ( Startdate )
           WITH VALUE #( FOR product IN lt_markets
                         ( %tky = product-%tky Startdate = cl_abap_context_info=>get_system_date(  ) ) )
           REPORTED DATA(lt_update_reported).
    reported = CORRESPONDING #( DEEP lt_update_reported ).
  ENDMETHOD.

ENDCLASS.


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
    METHODS setdepth FOR DETERMINE ON SAVE
      IMPORTING keys FOR product~setdepth.

    METHODS setheight FOR DETERMINE ON SAVE
      IMPORTING keys FOR product~setheight.

    METHODS setwidth FOR DETERMINE ON SAVE
      IMPORTING keys FOR product~setwidth.

    METHODS checkdepth FOR VALIDATE ON SAVE
      IMPORTING keys FOR product~checkdepth.

    METHODS checkheight FOR VALIDATE ON SAVE
      IMPORTING keys FOR product~checkheight.

    METHODS checkwidth FOR VALIDATE ON SAVE
      IMPORTING keys FOR product~checkwidth.

    METHODS setsizeuom FOR DETERMINE ON SAVE
      IMPORTING keys FOR product~setsizeuom.

    METHODS setpricecurrency FOR DETERMINE ON SAVE
      IMPORTING keys FOR product~setpricecurrency.

    METHODS setphaseid FOR DETERMINE ON SAVE
      IMPORTING keys FOR product~setphaseid.

    METHODS nextphase FOR MODIFY
      IMPORTING keys FOR ACTION product~nextphase RESULT result.

    METHODS getNextPhase IMPORTING i_phaseid type int1 RETURNING VALUE(r_out) type int1.
ENDCLASS.


CLASS lhc_Product IMPLEMENTATION.
  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD setDepth.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE ENTITY Product FIELDS ( Depth ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_products).
    DELETE lt_products WHERE Depth IS NOT INITIAL.
    IF lt_products IS INITIAL.
      RETURN.
    ENDIF.
    MODIFY ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Product
           UPDATE FIELDS ( Depth )
           WITH VALUE #( FOR product IN lt_products
                         ( %tky = product-%tky Depth = '10' ) )
           REPORTED DATA(lt_update_reported).
    reported = CORRESPONDING #( DEEP lt_update_reported ).
  ENDMETHOD.

  METHOD setHeight.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE ENTITY Product FIELDS ( Height ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_products).
    DELETE lt_products WHERE Height IS NOT INITIAL.
    IF lt_products IS INITIAL.
      RETURN.
    ENDIF.
    MODIFY ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Product
           UPDATE FIELDS ( Height )
           WITH VALUE #( FOR product IN lt_products
                         ( %tky = product-%tky Height = '10' ) )
           REPORTED DATA(lt_update_reported).
    reported = CORRESPONDING #( DEEP lt_update_reported ).
  ENDMETHOD.

  METHOD setWidth.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE ENTITY Product FIELDS ( Width ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_products).
    DELETE lt_products WHERE Width IS NOT INITIAL.
    IF lt_products IS INITIAL.
      RETURN.
    ENDIF.
    MODIFY ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Product
           UPDATE FIELDS ( Width )
           WITH VALUE #( FOR product IN lt_products
                         ( %tky = product-%tky Width = '10' ) )
           REPORTED DATA(lt_update_reported).
    reported = CORRESPONDING #( DEEP lt_update_reported ).
  ENDMETHOD.

  METHOD checkDepth.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE ENTITY Product FIELDS ( Depth ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_products).

    LOOP AT lt_products INTO DATA(ls_product).
      APPEND VALUE #( %tky        = ls_product-%tky
                      %state_area = 'CHECKDEPTH' ) TO reported-product.
      IF ls_product-Depth >= 0.
        CONTINUE.
      ENDIF.

      APPEND VALUE #( %tky = ls_product-%tky ) TO failed-product.

      APPEND VALUE #( %tky          = ls_product-%tky
                      %state_area   = 'CHECKDEPTH'
                      %msg          = new_message_with_text( severity = if_abap_behv_message=>severity-error text = 'Depth less than zero' )
                      %element-Pgid = if_abap_behv=>mk-on )
             TO reported-product.
    ENDLOOP.
  ENDMETHOD.

  METHOD checkHeight.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE ENTITY Product FIELDS ( Height ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_products).

    LOOP AT lt_products INTO DATA(ls_product).
      APPEND VALUE #( %tky        = ls_product-%tky
                      %state_area = 'CHECKHEIGHT' ) TO reported-product.
      IF ls_product-Height >= 0.
        CONTINUE.
      ENDIF.

      APPEND VALUE #( %tky = ls_product-%tky ) TO failed-product.

      APPEND VALUE #( %tky          = ls_product-%tky
                      %state_area   = 'CHECKHEIGHT'
                      %msg          = new_message_with_text( severity = if_abap_behv_message=>severity-error text = 'Height less than zero' )
                      %element-Pgid = if_abap_behv=>mk-on )
             TO reported-product.
    ENDLOOP.
  ENDMETHOD.

  METHOD checkWidth.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE ENTITY Product FIELDS ( Width ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_products).

    LOOP AT lt_products INTO DATA(ls_product).
      APPEND VALUE #( %tky        = ls_product-%tky
                      %state_area = 'CHECKQWIDTH' ) TO reported-product.
      IF ls_product-Width >= 0.
        CONTINUE.
      ENDIF.

      APPEND VALUE #( %tky = ls_product-%tky ) TO failed-product.

      APPEND VALUE #( %tky          = ls_product-%tky
                      %state_area   = 'CHECKQWIDTH'
                      %msg          = new_message_with_text( severity = if_abap_behv_message=>severity-error text = 'Width less than zero' )
                      %element-Pgid = if_abap_behv=>mk-on )
             TO reported-product.
    ENDLOOP.
  ENDMETHOD.

  METHOD setSizeUom.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE ENTITY Product FIELDS ( SizeUom ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_products).
    DELETE lt_products WHERE SizeUom IS NOT INITIAL.
    IF lt_products IS INITIAL.
      RETURN.
    ENDIF.
    MODIFY ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Product
           UPDATE FIELDS ( SizeUom )
           WITH VALUE #( FOR product IN lt_products
                         ( %tky = product-%tky SizeUom = 'CM' ) )
           REPORTED DATA(lt_update_reported).
    reported = CORRESPONDING #( DEEP lt_update_reported ).
  ENDMETHOD.

  METHOD setPriceCurrency.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE ENTITY Product FIELDS ( PriceCurrency ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_products).
    DELETE lt_products WHERE PriceCurrency IS NOT INITIAL.
    IF lt_products IS INITIAL.
      RETURN.
    ENDIF.
    MODIFY ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Product
           UPDATE FIELDS ( PriceCurrency )
           WITH VALUE #( FOR product IN lt_products
                         ( %tky = product-%tky PriceCurrency = 'USD' ) )
           REPORTED DATA(lt_update_reported).
    reported = CORRESPONDING #( DEEP lt_update_reported ).
  ENDMETHOD.

  METHOD setPhaseid.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE ENTITY Product FIELDS ( Phaseid ) WITH CORRESPONDING #( keys ) RESULT DATA(lt_products).
    DELETE lt_products WHERE Phaseid IS NOT INITIAL.
    IF lt_products IS INITIAL.
      RETURN.
    ENDIF.
    MODIFY ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Product
           UPDATE FIELDS ( Phaseid )
           WITH VALUE #( FOR product IN lt_products
                         ( %tky = product-%tky Phaseid = '1' ) )
           REPORTED DATA(lt_update_reported).
    reported = CORRESPONDING #( DEEP lt_update_reported ).
  ENDMETHOD.

  METHOD nextPhase.
    READ ENTITIES OF z##_i_product_#### IN LOCAL MODE
         ENTITY Product
         FIELDS ( Phaseid ) WITH CORRESPONDING #( keys )
         RESULT DATA(lt_result).
    MODIFY ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Product
           UPDATE
           FIELDS ( Phaseid )
           WITH VALUE #( FOR ls_result IN lt_result
                         ( %tky    = ls_result-%tky
                           Phaseid = getnextphase( ls_result-Phaseid ) ) )
           FAILED   failed
           REPORTED reported.
    result = VALUE #( FOR ls_product_result IN lt_result
                      ( %tky   = ls_product_result-%tky
                        %param = ls_product_result ) ).
  ENDMETHOD.

  METHOD getnextphase.
    CASE i_phaseid.
      WHEN phase_id-plan.
        r_out = phase_id-dev.
      WHEN phase_id-dev.
        r_out = phase_id-prod.
      WHEN phase_id-prod.
        r_out = phase_id-out.
      WHEN phase_id-out.
        r_out = phase_id-plan.
      WHEN OTHERS.
        r_out = phase_id-plan.
    ENDCASE.
  ENDMETHOD.

ENDCLASS.
```

## Behaviour Implementation for CDS Product Projection Transactional Query
<a name="z##_c_product_"></a>

```ABAP
CLASS lhc_product DEFINITION INHERITING FROM cl_abap_behavior_handler.

  PRIVATE SECTION.
    CONSTANTS:
      BEGIN OF phase_id,
        plan TYPE int1 VALUE 1,
        dev  TYPE int1 VALUE 2,
        prod TYPE int1 VALUE 3,
        out  TYPE int1 VALUE 4,
      END OF phase_id.

    METHODS priorPhase FOR MODIFY
      IMPORTING keys FOR ACTION Product~priorPhase RESULT result.

    METHODS getPriorPhase IMPORTING i_phaseid type int1 RETURNING VALUE(r_out) type int1.

ENDCLASS.

CLASS lhc_product IMPLEMENTATION.
  METHOD priorPhase.
    READ ENTITIES OF z##_c_product_#### IN LOCAL MODE
         ENTITY Product
         ALL FIELDS WITH CORRESPONDING #( keys )
         RESULT DATA(lt_result).
    MODIFY ENTITIES OF z##_c_product_#### IN LOCAL MODE
           ENTITY Product
           UPDATE
           FIELDS ( Phaseid )
           WITH VALUE #( FOR ls_result IN lt_result
                         ( %tky    = ls_result-%tky
                           Phaseid = getpriorphase( ls_result-Phaseid ) ) )
           FAILED   failed
           REPORTED reported.
    result = VALUE #( FOR ls_product_result IN lt_result
                      ( %tky   = ls_product_result-%tky
                        %param = ls_product_result ) ).
  ENDMETHOD.

  METHOD getpriorphase.
    CASE i_phaseid.
      WHEN phase_id-plan.
        r_out = phase_id-out.
      WHEN phase_id-dev.
        r_out = phase_id-plan.
      WHEN phase_id-prod.
        r_out = phase_id-dev.
      WHEN phase_id-out.
        r_out = phase_id-prod.
      WHEN OTHERS.
        r_out = phase_id-plan.
    ENDCASE.
  ENDMETHOD.

ENDCLASS.

*"* use this source file for the definition and implementation of
*"* local helper classes, interface definitions and type
*"* declarations
```
## Behaviour Implementation for CDS Product Projection Transactional Query
<a name="z##_cl_event_handler_"></a>
Z##CL_EVENT_HANDLER_####

```ABAP
CLASS z##_cl_event_handler_#### DEFINITION
  PUBLIC
  ABSTRACT
  FINAL
  FOR EVENTS OF Z##_I_PRODUCT_#### .

  PUBLIC SECTION.
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS z##_cl_event_handler_#### IMPLEMENTATION.
ENDCLASS.
```
```ABAP
CLASS lcl_local_event_consumption DEFINITION INHERITING FROM cl_abap_behavior_event_handler.
  PRIVATE SECTION.
    METHODS consume_event_1 FOR ENTITY EVENT IMPORTING params FOR Product~testEvent.
ENDCLASS.


CLASS lcl_local_event_consumption IMPLEMENTATION.
  METHOD consume_event_1.
    CHECK params IS NOT INITIAL.
  ENDMETHOD.
ENDCLASS.
```