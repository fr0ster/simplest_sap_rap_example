# Behaviour Definitions

## Behaviour Definitions for CDS Product BO Interface
<a name="z##_i_product_"></a>
Z##_I_PRODUCT_####

```ABAP
managed implementation in class zbp_##_i_product_#### unique;
strict ( 2 );

define behavior for Z##_I_PRODUCT_#### alias Product
persistent table z##_d_prod_####
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

  mapping for z##_d_prod_####
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

define behavior for Z##_I_MARKET_#### alias Market
persistent table z##_d_mrkt_####
lock dependent by _Product
authorization dependent by _Product
etag master LocalChangedTime
{
  update;
  delete;

  association _Product;

  field ( numbering : managed, readonly ) MrktUuid;
  field ( readonly ) ProdUuid;

  mapping for z##_d_mrkt_####
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
## Behaviour Definitions for CDS Product BO Projection Transaction Query
<a name="z##_c_product_"></a>
Z##_C_PRODUCT_####

```ABAP
projection;
strict ( 2 );

define behavior for Z##_C_PRODUCT_#### alias _Product
{
  use create;
  use update;
  use delete;

  use association _Market { create; }
}

define behavior for Z##_C_MARKET_#### alias _Market
{
  use update;
  use delete;

  use association _Product;
}
```