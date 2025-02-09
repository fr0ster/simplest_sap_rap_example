
# CDS Interface Definitions

## CDS Interface for Root View Entity Product

<a name="z##_i_product_"></a>
### Z##_I_PRODUCT_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Product Interface'

define root view entity Z##_I_PRODUCT_####
  as select from z##_d_prod_####

  composition [0..*] of Z##_I_MARKET_####           as _Market
  association [1..1] to Z##_I_PG_VH_####            as _PG on $projection.Pgid = _PG.Pgid
  association [1..1] to Z##_I_PHASE_VH_####         as _PHASE on $projection.Phaseid = _PHASE.Phaseid
  association [0..1] to Z##_I_UOM_VH_####           as _UOM on $projection.SizeUom = _UOM.Msehi

{
  key prod_uuid       as ProdUuid,

      prodid          as Prodid,
      pgid            as Pgid,
      phaseid         as Phaseid,

      @Semantics.amount.currencyCode: 'PriceCurrency'
      price           as Price,

      price_currency  as PriceCurrency,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      height          as Height,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      depth           as Depth,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      width           as Width,

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

## CDS Interface for View Entity Market

<a name="z##_i_market_"></a>
### Z##_I_MARKET_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Market Interface'

define view entity Z##_I_MARKET_####
  as select from z##_d_mrkt_####

  association to parent Z##_I_PRODUCT_#### as _Product   on $projection.ProdUuid = _Product.ProdUuid
  association [1..1] to Z##_I_COUNTRY_#### as _Countries on $projection.Mrktid = _Countries.Id

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
      _Countries
}
```

## CDS Interface for View Entity Countries Dictionary

<a name="z##_i_country_"></a>
### Z##_I_COUNTRY_####

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

## CDS Interface for View Entity Country Value Helper

<a name="z##_i_country_vh_"></a>
### Z##_I_COUNTRY_VH_####

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

## CDS Interface for View Entity Products Value Helper

<a name="z##_i_product_vh_"></a>
### Z##_I_PRODUCT_VH_####

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

## CDS Interface for View Entity Currency Value Helper

<a name="z##_i_currency_vh_"></a>
### Z##_I_CURRENCY_VH_####

```ABAP
@AbapCatalog.viewEnhancementCategory: [ #NONE ]

@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Currency Search Help'

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

## CDS Interface for View Entity Product Group Value Helper

<a name="z##_i_pg_vh_"></a>
### Z##_I_PG_VH_####

```ABAP
@AbapCatalog.viewEnhancementCategory: [ #NONE ]

@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Product Group Search Help'


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

## CDS Interface for View Entity Phase Value Helper

<a name="z##_i_phase_vh_"></a>
### Z##_I_PHASE_VH_####

```ABAP
@AbapCatalog.viewEnhancementCategory: [ #NONE ]

@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Phase Search Help'


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

## CDS Interface for View Entity UOM Value Helper

<a name="z##_i_uom_vh_"></a>
### Z##_I_UOM_VH_####

```ABAP
@AbapCatalog.viewEnhancementCategory: [ #NONE ]

@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'UOM Search Help'


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