# Behaviour Implementations

## Behaviour Implementation for CDS  Product BO Interface
<a name="z##_i_product_"></a>

```ABAP
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
    WITH CORRESPONDING #( keys ) RESULT DATA(lt_orders)
    ENTITY Product FIELDS ( PriceCurrency )
    WITH CORRESPONDING #( keys ) RESULT DATA(lt_products).
    DELETE lt_orders WHERE Amountcurr IS NOT INITIAL.
    IF lt_orders IS INITIAL.
      RETURN.
    ENDIF.
    DELETE lt_products WHERE PriceCurrency IS NOT INITIAL.
    IF lt_products IS INITIAL.
      RETURN.
    ENDIF.
    DAtA(ls_product) = lt_products[ 1 ].
    MODIFY ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Order
           UPDATE FIELDS ( Amountcurr )
           WITH VALUE #( FOR product IN lt_orders
                         ( %tky = product-%tky Amountcurr = ls_product-PriceCurrency ) )
           REPORTED DATA(lt_update_reported).
    reported = CORRESPONDING #( DEEP lt_update_reported ).
  ENDMETHOD.

  METHOD checkGrossamount.
  ENDMETHOD.

  METHOD checkNetamount.
  ENDMETHOD.

  METHOD checkQuantity.
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
    DATA lv_timestamp type string.
    lv_timestamp = cl_abap_context_info=>get_system_date(  ) + 366.
    lv_timestamp = |{ lv_timestamp(6) }01|.
    MODIFY ENTITIES OF z##_i_product_#### IN LOCAL MODE
           ENTITY Market
           UPDATE FIELDS ( Enddate )
           WITH VALUE #( FOR product IN lt_markets
                         ( %tky = product-%tky Enddate = cl_abap_context_info=>get_system_date(  ) ) )
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
  ENDMETHOD.

  METHOD checkHeight.
  ENDMETHOD.

  METHOD checkWidth.
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

ENDCLASS.
```

## Behaviour Implementation for CDS Criticality Levels Interface
<a name="z##_i_criticality_levels_"></a>

```ABAP
CLASS lhc_Levels DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR CriticalityLevels RESULT result.

ENDCLASS.

CLASS lhc_Levels IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

ENDCLASS.
```