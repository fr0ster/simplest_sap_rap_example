# CDS Interface definition

## CDS Interface for view entity Orders
<a name="z##_i_order_"></a>
Z##_I_ORDER_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Orders data'

@Metadata.ignorePropagatedAnnotations: true

define view entity Z##_I_ORDER_####
  as select from z##_d_order_####

  association to parent Z##_I_MARKET_#### as _Market on $projection.ProdUuid = _Market.ProdUuid and $projection.MrktUuid = _Market.MrktUuid

  association [1..1] to Z##_I_PRODUCT_#### as _Product
    on $projection.ProdUuid = _Product.ProdUuid

{
  key prod_uuid          as ProdUuid,
  key mrkt_uuid          as MrktUuid,
  key order_uuid         as OrderUuid,

      quantity           as Quantity,
      delivery_date      as DeliveryDate,

      @Semantics.amount.currencyCode: 'Amountcurr'
      netamount          as Netamount,

      @Semantics.amount.currencyCode: 'Amountcurr'
      grossamount        as Grossamount,

      amountcurr         as Amountcurr,

      @Semantics.user.createdBy: true
      created_by         as CreatedBy,

      @Semantics.systemDateTime.createdAt: true
      creation_time      as CreationTime,

      @Semantics.user.lastChangedBy: true
      changed_by         as ChangedBy,

      @Semantics.systemDateTime.lastChangedAt: true
      changed_time       as ChangedTime,

      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      local_changed_time as LocalChangedTime,

      /*Association*/
      _Market,
      _Product
}
```

## CDS Interface for root view entity Market
<a name="z##_i_market_"></a>
Z##_I_MARKET_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Market Interface'

define view entity Z##_I_MARKET_####
  as select from z##_d_mrkt_####

  association to parent Z##_I_PRODUCT_#### as _Product   on $projection.ProdUuid = _Product.ProdUuid
  association [1..1] to Z##_I_COUNTRY_#### as _Countries on $projection.Mrktid = _Countries.Id

  composition [0..*] of Z##_I_ORDER_#### as _Orders

{
  key prod_uuid          as ProdUuid,
  key mrkt_uuid          as MrktUuid,

      mrktid             as Mrktid,
      startdate          as Startdate,
      enddate            as Enddate,

      @Semantics.user.createdBy: true
      created_by         as CreatedBy,

      @Semantics.systemDateTime.createdAt: true
      creation_time      as CreationTime,

      @Semantics.user.lastChangedBy: true
      changed_by         as ChangedBy,

      @Semantics.systemDateTime.lastChangedAt: true
      changed_time       as ChangedTime,

      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      local_changed_time as LocalChangedTime,

      /*association*/
      _Product,
      _Countries,
      _Orders
}
```