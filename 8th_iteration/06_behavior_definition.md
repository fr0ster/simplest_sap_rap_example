# Behaviour Definitions

## Behaviour Definition for CDS Product BO Projection Transaction Interface
<a name="z##_c_product_"></a>
Z##_C_PRODUCT_####

```ABAP
projection implementation in class zbp_##_c_product_#### unique;
strict ( 2 );
use draft;

define behavior for Z##_C_PRODUCT_#### alias Product
{
  use create;
  use update;
  use delete;

  use action Prepare;
  use action Edit;
  use action Activate;
  use action Discard;
  use action Resume;

  use association _Market { create; with draft; }
  use action nextPhase;
  action priorPhase result [1] $self;
}

define behavior for Z##_C_MARKET_#### alias Market
{
  use update;
  use delete;

  use association _Product {  with draft; }
  use association _Orders { create; with draft; }
}

define behavior for Z##_C_ORDER_#### alias Order
{
  use update;
  use delete;

  use association _Product {  with draft; }
  use association _Market {  with draft; }
}
```

## Behaviour Definition for CDS Product BO Projection Transaction Interface
<a name="z##_ci_product_"></a>
Z##_CI_PRODUCT_####

```ABAP
interface;
use draft;
use side effects;

define behavior for Z##_CI_PRODUCT_#### alias Product
{
  use create;
  use update;
  use delete;

  use action Prepare;
  use action Edit;
  use action Activate;
  use action Discard;
  use action Resume;

  use association _Market { create; with draft; }
  use action nextPhase;
}

define behavior for Z##_CI_MARKET_#### alias Market
{
  use update;
  use delete;

  use association _Product {  with draft; }
  use association _Orders { create; with draft; }
}

define behavior for Z##_CI_ORDER_#### alias Order
{
  use update;
  use delete;

  use association _Product {  with draft; }
  use association _Market {  with draft; }
}
```

## Behaviour Definition for CDS Product BO Interface
<a name="z##_i_product_"></a>
Z##_I_PRODUCT

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

define behavior for Z##_I_PRODUCT_#### alias Product
persistent table z##_d_prod_####
draft table z##_dt_prod_####
extensible
lock master
total etag LocalChangedTime
authorization master ( instance )
etag master LocalChangedTime
{
  create;
  update;
  delete;

  association _Market { create; with draft; }

  field ( numbering : managed, readonly ) ProdUuid;
  field ( suppress )
  PriceCriticality,
  PhaseCritically,
  Measure;
  field ( readonly )
  Phaseid,
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

  draft action Edit;
  draft action Activate optimized;
  draft action Discard;
  draft action Resume;
  draft determine action Prepare extensible
  {
    validation checkHeight;
    validation checkDepth;
    validation checkWidth;
    validation Order~checkQuantity;
    validation Order~checkNetamount;
    validation Order~checkGrossamount;
  }

  action nextPhase result [1] $self;

  event testEvent parameter Z##_A_PARAMS_####;

  mapping for z##_d_prod_#### corresponding extensible
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
draft table z##_dt_mrkt_####
extensible
lock dependent by _Product
authorization dependent by _Product
etag master LocalChangedTime
{
  update;
  delete;

  association _Product { with draft; }
  association _Orders { create; with draft; }

  field ( numbering : managed, readonly ) MrktUuid;
  field ( readonly ) ProdUuid;

  determination setStartdate on save { create; field Startdate; }
  determination setEnddate on save { create; field Enddate; }

  mapping for z##_d_mrkt_#### corresponding extensible
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
draft table z##_dt_ordr_####
extensible
lock dependent by _Product
authorization dependent by _Product
etag master LocalChangedTime
{
  update;
  delete;

  association _Product { with draft; }
  association _Market { with draft; }

  field ( numbering : managed, readonly ) OrderUuid;
  field ( readonly ) MrktUuid;
  field ( readonly ) ProdUuid;

  determination setAmountcurr on save { create; field Amountcurr; }

  validation checkQuantity on save { field Quantity; }
  validation checkNetamount on save { field Netamount; }
  validation checkGrossamount on save { field Grossamount; }

  mapping for z##_d_order_#### corresponding extensible
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