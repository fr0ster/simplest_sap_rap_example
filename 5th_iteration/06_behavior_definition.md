# Behaviour Definitions

## Behaviour Definition for CDS Product BO Projection Transaction Interface
<a name="z##_c_product_"></a>
Z##_C_PRODUCT_####

```ABAP
projection;
strict ( 2 );

define behavior for Z##_CI_PRODUCT_#### alias _Product
{
  use create;
  use update;
  use delete;

  use association _Market { create; }
}

define behavior for Z##_CI_MARKET_#### alias _Market
{
  use update;
  use delete;

  use association _Product;
  use association _Orders { create; }
}
define behavior for Z##_CI_ORDER_#### alias _Order
{
  use update;
  use delete;

  use association _Product;
  use association _Market;
}
```

## Behaviour Definition for CDS Product BO Projection Transaction Interface
<a name="z##_ci_product_"></a>
Z##_CI_PRODUCT_####

```ABAP
projection;
strict ( 2 );

define behavior for Z##_C_PRODUCT_#### alias _Product
{
  use create;
  use update;
  use delete;

  use association _Market { create; }
  use action nextPhase;
}

define behavior for Z##_C_MARKET_#### alias _Market
{
  use update;
  use delete;

  use association _Product;
  use association _Orders { create; }
}
define behavior for Z##_C_ORDER_#### alias _Order
{
  use update;
  use delete;

  use association _Product;
  use association _Market;
}
```

## Behaviour Definition for CDS Product BO Interface
<a name="z##_i_product_"></a>
Z##_I_PRODUCT

```ABAP
managed with additional save implementation in class zbp_##_i_product_#### unique;
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
  PriceCriticality,
  Measure,
  CreationTime,
  CreatedBy,
  ChangedTime,
  ChangedBy,
  LocalChangedTime;
  field ( readonly : update ) Prodid;

  determination setPhaseid on save { create; field Phaseid; }
  determination setPriceCurrency on save { create; field PriceCurrency; }
  determination setHeight on save { create; field Height; }
  determination setDepth on save { create; field Depth; }
  determination setWidth on save { create; field Width; }
  determination setSizeUom on save { create; field SizeUom; }

  validation checkHeight on save { field Height; }
  validation checkDepth on save { field Depth; }
  validation checkWidth on save { field Width; }

  action nextPhase result [1] $self;


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
  association _Orders { create; }

  field ( numbering : managed, readonly ) MrktUuid;
  field ( readonly ) ProdUuid;

  determination setStartdate on save { create; field Startdate; }
  determination setEnddate on save { create; field Enddate; }

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

define behavior for Z##_I_ORDER_#### alias Order
persistent table z##_d_order_####
lock dependent by _Product
authorization dependent by _Product
etag master LocalChangedTime
{
  update;
  delete;

  association _Product;
  association _Market;

  field ( numbering : managed, readonly ) OrderUuid;
  field ( readonly ) MrktUuid;
  field ( readonly ) ProdUuid;

  determination setAmountcurr on save { create; field Amountcurr; }
  validation checkQuantity on save { field Quantity; }
  validation checkNetamount on save { field Netamount; }
  validation checkGrossamount on save { field Grossamount; }

  mapping for z##_d_order_####
    {
      OrderUuid        = order_uuid;
      ProdUuid         = prod_uuid;
      MrktUuid         = mrkt_uuid;
      Quantity         = quantity;
      DeliveryDate     = delivery_date;
      Netamount        = netamount;
      Grossamount      = grossamount;
      Amountcurr       = amountcurr;
      CreatedBy        = created_by;
      CreationTime     = creation_time;
      ChangedBy        = changed_by;
      ChangedTime      = changed_time;
      LocalChangedTime = local_changed_time;
    }
}
```