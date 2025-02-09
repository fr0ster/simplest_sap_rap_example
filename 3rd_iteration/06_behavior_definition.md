# Behaviour Definitions

## Behaviour Definition for CDS Criticality Levels BO Interface
<a name="z##_i_criticality_levels_"></a>
Z##_I_CRITICALITY_LEVELS_####

```ABAP
managed implementation in class zbp_##_i_crtlty_levels_#### unique;
strict ( 2 );

define behavior for Z##_I_CRITICALITY_LEVELS_#### alias CriticalityLevels
persistent table z##_d_crtly_####
lock master
authorization master ( instance )
etag master LocalChangedTime
{
  create;
  update;
  delete;

  field ( readonly ) Id;

  mapping for z##_d_crtly_####
    {
      Id               = id;
      Treashhold1      = treashhold1;
      Treashhold2      = treashhold2;
      Treashhold3      = treashhold3;
      Treashhold4      = treashhold4;
      CreatedBy        = created_by;
      CreationTime     = creation_time;
      ChangedBy        = changed_by;
      ChangedTime      = changed_time;
      LocalChangedTime = local_changed_time;
    }
}
```

## Behaviour Definition for CDS Product BO Interface
<a name="z##_i_product_"></a>
Z##_I_PRODUCT

```ABAP
managed;
strict ( 2 );
with draft;

define behavior for Z##_I_PRODUCT alias Product
implementation in class z##_cl_bp_i_product unique
persistent table z##_d_product
draft table z##_d_product_dt
lock master total etag ChangedTime
etag master LocalChangedTime
authorization master ( instance )
{
  create;
  update;
  delete;

  association _Market { create; with draft; }

  field ( numbering : managed, readonly ) ProdUUID;

  field ( readonly )
  Phaseid,
  PhaseCritically,
  PriceCriticality,
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
  SizeUomId,
  Price,
  PriceCurrency,
  Taxrate;

  determination set_first_pase on modify { field Phaseid; create; }

  validation validate_pg on save { field Pgid; }
  validation validate_prodid on save { field Prodid; }
  validation sizeValidation on save { field Height, Depth, Width; }

  draft action Edit;
  draft action Activate optimized;
  draft action Discard;
  draft action Resume;
  draft determine action Prepare
  {
    validation validate_pg;
    validation validate_prodid;
    validation sizeValidation;
    validation Market~validateMarket;
    validation Market~validateStartDate;
    validation Market~validateEndDate;
    validation Market~validateMarketDupl;
    validation Order~validateDeliveryDate;
  }

  factory action ( features : instance ) make_copy parameter z##_s_product [1];
  action ( features : instance ) move_to_next_phase result [1] $self;

  mapping for z##_d_product
    {
      ProdUUID         = prod_uuid;
      ProdID           = prodid;
      PgID             = pgid;
      PhaseID          = phaseid;
      Height           = height;
      Depth            = depth;
      Width            = width;
      SizeUomId        = size_uom;
      Price            = price;
      PriceCurrency    = price_currency;
      Taxrate          = taxrate;
      CreatedBy        = created_by;
      CreationTime     = creation_time;
      ChangedBy        = changed_by;
      ChangedTime      = changed_time;
      LocalChangedTime = local_changed_time;
    }
}

define behavior for Z##_I_MARKET alias Market
implementation in class z##_cl_bp_i_market unique
persistent table z##_d_prod_mrkt
draft table z##_d_market_dt
lock dependent by _Product
authorization dependent by _Product
etag master LocalChangedTime
{
  update;
  delete;

  association _Product { with draft; }
  association _Order { create; with draft; }

  field ( numbering : managed, readonly ) MrktUuid;
  field ( readonly )
  ProdUuid,
  ChangedBy,
  ChangedTime,
  CreationTime,
  CreatedBy;
  field ( mandatory ) Mrktid;
  field ( features : instance ) Status;

  action ( features : instance ) confirmMarketStatus result [1] $self;

  validation validateMarket on save { create; field Mrktid; }
  validation validateStartDate on save { create; field Startdate; }
  validation validateEndDate on save { create; field Enddate; }
  validation validateMarketDupl on save { create; field Mrktid; }

  mapping for z##_d_prod_mrkt
    {
      ProdUuid         = prod_uuid;
      MrktUuid         = mrkt_uuid;
      Mrktid           = mrktid;
      Startdate        = startdate;
      Enddate          = enddate;
      Status           = status;
      CreatedBy        = created_by;
      CreationTime     = creation_time;
      ChangedBy        = changed_by;
      ChangedTime      = changed_time;
      LocalChangedTime = local_changed_time;
    }
}

define behavior for Z##_I_MRKT_ORDER alias Order
implementation in class z##_cl_bp_i_order unique
persistent table z##_d_mrkt_order
draft table z##_d_order_dt
lock dependent by _Product
etag master LocalChangedTime
authorization dependent by _Product
{

  update;
  delete;

  field ( numbering : managed, readonly ) OrderUuid;
  field ( readonly ) ProdUuid, MrktUuid, Orderid;
  field ( readonly ) ChangedBy, CalendarYear, Netamount, Grossamount, Amountcurr, ChangedTime, CreationTime, CreatedBy;
  field ( mandatory ) Quantity, DeliveryDate;

  association _Product { with draft; }
  association _Market { with draft; }

  side effects
  {
    field Quantity affects field Netamount, field Grossamount;
    field DeliveryDate affects field CalendarYear;
  }

  determination calculateOrderID on modify { create; }
  determination calculateAmounts on modify { field DeliveryDate, Netamount, Grossamount, Amountcurr, CalendarYear, Quantity; }

  validation validateDeliveryDate on save { create; field DeliveryDate; }

  mapping for z##_d_mrkt_order
    {
      ProdUuid         = prod_uuid;
      MrktUuid         = mrkt_uuid;
      OrderUuid        = order_uuid;
      Orderid          = orderid;
      Quantity         = quantity;
      CalendarYear     = calendar_year;
      DeliveryDate     = delivery_date;
      Netamount        = netamount;
      Grossamount      = grossamount;
      Amountcurr       = amountcurr;
      CreatedBy        = created_by;
      CreationTime     = creation_time;
      ChangedBy        = changed_by;
      ChangedTime      = change_time;
      LocalChangedTime = local_changed_time;
    }

}
```

## Behaviour Definition for CDS Product BO Projection Transaction Query
<a name="z##c_product_"></a>
Z##_C_PRODUCT

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

