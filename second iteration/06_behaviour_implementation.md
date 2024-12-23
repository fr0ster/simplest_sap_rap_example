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
  field ( readonly : update ) Prodid;

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
    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR Product RESULT result.

ENDCLASS.


CLASS lhc_Product IMPLEMENTATION.
  METHOD get_instance_authorizations.
  ENDMETHOD.
ENDCLASS.
```