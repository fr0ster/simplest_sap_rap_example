# CDS Interface definitions

## CDS Interface for root view entity Product

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Product Interface'

define root view entity Z##_I_PRODUCT_####
  as select from z##_d_prod_####

  composition [0..*] of Z##_I_MARKET_####           as _Market
  association [1..1] to Z##_I_PG_VH_####            as _PG on $projection.Pgid = _PG.Pgid
  association [1..1] to Z##_I_PHASE_VH_####         as _PHASE on $projection.Phaseid = _PHASE.Phaseid
  association [0..1] to Z##_I_UOM_VH_####           as _UOM on $projection.SizeUom = _UOM.Msehi

  association [1..1] to Z##_I_CRITICALITY_LEVELS_#### as _PriceCriticality on _PriceCriticality.Id = 'PriceCritically'

{
  key prod_uuid       as ProdUuid,

      prodid          as Prodid,
      pgid            as Pgid,
      phaseid         as Phaseid,

      @Semantics.amount.currencyCode: 'PriceCurrency'
      price           as Price,

      case
        when cast(price as abap.dec(15,2)) > _PriceCriticality.Treashhold1 then 1
        when cast(price as abap.dec(15,2)) > _PriceCriticality.Treashhold2 then 2
        when cast(price as abap.dec(15,2)) > _PriceCriticality.Treashhold3 then 3
        when cast(price as abap.dec(15,2)) > _PriceCriticality.Treashhold4 then 4
        else 0
      end             as PriceCriticality,

      price_currency  as PriceCurrency,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      height          as Height,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      depth           as Depth,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      width           as Width,

      size_uom        as SizeUom,
      
      '' as Measure,

      @Semantics.user.createdBy: true
      created_by      as CreatedBy,

      @Semantics.systemDateTime.createdAt: true
      creation_time   as CreationTime,

      @Semantics.user.lastChangedBy: true
      changed_by      as ChangedBy,

      @Semantics.systemDateTime.lastChangedAt: true
      changed_time     as ChangedTime,

      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      local_changed_time as LocalChangedTime,

      /*Associations*/
      _Market,
      _PG,
      _PHASE,
      _UOM,
      _PriceCriticality
}
```

## CDS Interface for view entity Market

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

## CDS Interface for view entity Orders

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

## CDS Interface for view entity Countries dictionary

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Countries dict'

define view entity Z##_I_COUNTRY_####
  as select from z##_d_ctry_####

{
  key land     as Id,

      name     as Country,
      code     as Code,
      imageurl as Imageurl
}
```

## CDS Interface for view entity Country Value Helper

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Country Search Help'

@ObjectModel.resultSet.sizeCategory: #XS
@ObjectModel.usageType: { serviceQuality: #X, sizeCategory: #S, dataClass: #MIXED }

@Search.searchable: true

@UI.presentationVariant: [ { sortOrder: [ { by: 'Country', direction: #ASC } ] } ]

define view entity Z##_I_COUNTRY_VH_####
  as select from Z##_I_COUNTRY_####

{
      @ObjectModel.text.element: [ 'Country' ]
  key Id,

      @Search.defaultSearchElement: true
      Country as Country
}
```

## CDS Interface for view entity Products Value Helper

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Product ID Search Help'

@ObjectModel.resultSet.sizeCategory: #XS
@ObjectModel.usageType: { serviceQuality: #X, sizeCategory: #S, dataClass: #MIXED }

@Search.searchable: true

@UI.presentationVariant: [ { sortOrder: [ { by: 'ProdUuid', direction: #ASC } ] } ]

define view entity Z##_I_PRODUCT_VH_####
  as select distinct from z##_d_prod_####

{
      @UI.hidden: true
  key prod_uuid as ProdUuid,

      @Search.defaultSearchElement: true
      prodid    as Prodid
}
```

## CDS Interface for view entity Currency Value Helper

```ABAP
@AbapCatalog.viewEnhancementCategory: [ #NONE ]

@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Currency Interface'

@Metadata.ignorePropagatedAnnotations: true

@ObjectModel.resultSet.sizeCategory: #XS
@ObjectModel.usageType: { serviceQuality: #X, sizeCategory: #S, dataClass: #MIXED }

@Search.searchable: true
@UI.presentationVariant: [ { sortOrder: [ { by: 'Currency', direction: #ASC } ] } ]

define view entity Z##_I_CURRENCY_VH_####
  as select distinct from I_Currency

{
      @ObjectModel.text.element: [ 'CurrencyISOCode' ]
  key Currency,

      @Search.defaultSearchElement: true
      CurrencyISOCode
}
```

## CDS Interface for view entity Product Group Value Helper

```ABAP
@AbapCatalog.viewEnhancementCategory: [ #NONE ]

@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Product Group Interface'

@Metadata.ignorePropagatedAnnotations: true

@ObjectModel.resultSet.sizeCategory: #XS
@ObjectModel.usageType: { serviceQuality: #X, sizeCategory: #S, dataClass: #MIXED }

@Search.searchable: true

define view entity Z##_I_PG_VH_####
  as select from z##_d_pg_####

{
      @ObjectModel.text.element: [ 'Pgname' ]
  key pgid     as Pgid,

      @Search.defaultSearchElement: true
      pgname   as Pgname,

      imageurl as Imageurl
}
```

## CDS Interface for view entity Phase Value Helper

```ABAP
@AbapCatalog.viewEnhancementCategory: [ #NONE ]

@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Phase Interface'

@Metadata.ignorePropagatedAnnotations: true

@ObjectModel.resultSet.sizeCategory: #XS
@ObjectModel.usageType: { serviceQuality: #X, sizeCategory: #S, dataClass: #MIXED }

@Search.searchable: true
@UI.presentationVariant: [ { sortOrder: [ { by: 'phaseid', direction: #ASC } ] } ]

define view entity Z##_I_PHASE_VH_####
  as select from z##_d_phase_####

{
      @ObjectModel.text.element: [ 'Phase' ]
  key phaseid as Phaseid,

      @Search.defaultSearchElement: true
      phase   as Phase
}
```

## CDS Interface for view entity UOM Value Helper

```ABAP
@AbapCatalog.viewEnhancementCategory: [ #NONE ]

@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'VH UOM'

@Metadata.ignorePropagatedAnnotations: true

@ObjectModel.resultSet.sizeCategory: #XS
@ObjectModel.usageType: { serviceQuality: #X, sizeCategory: #S, dataClass: #MIXED }

@Search.searchable: true

@UI.presentationVariant: [ { sortOrder: [ { by: 'msehi', direction: #ASC } ] } ]

define view entity Z##_I_UOM_VH_####
  as select from z##_d_uom_####

{
      @ObjectModel.text.element: [ 'Msehi' ]
      @Search.defaultSearchElement: true
  key msehi   as Msehi,

      dimid   as Dimid,
      isocode as Isocode
}
```
## CDS Interface for root view entity Criticality Levels

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Criticality levels'

define root view entity Z##_I_CRITICALITY_LEVELS_####
  as select from z##_d_crtly_####

{
  key id          as Id,

      treashhold1 as Treashhold1,
      treashhold2 as Treashhold2,
      treashhold3 as Treashhold3,
      treashhold4 as Treashhold4,

      @Semantics.user.createdBy: true
      created_by      as CreatedBy,

      @Semantics.systemDateTime.createdAt: true
      creation_time   as CreationTime,

      @Semantics.user.lastChangedBy: true
      changed_by      as ChangedBy,

      @Semantics.systemDateTime.lastChangedAt: true
      changed_time     as ChangedTime,

      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      local_changed_time as LocalChangedTime
}
```