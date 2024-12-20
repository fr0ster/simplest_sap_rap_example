# CDS Interface definition

## CDS Interface for root view entity Product

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Product Interface'

@Metadata.ignorePropagatedAnnotations: true

define root view entity ZOK_I_PRODUCT_####
  as select from zok_d_product_####

  composition [0..*] of ZOK_I_MARKET_1234 as _Market

  association [1..1] to ZOK_I_PG_VH_####                  as _PG on $projection.Pgid = _PG.Pgid
  association [1..1] to ZOK_I_PHASE_VH_####               as _PHASE on $projection.Phaseid = _PHASE.Phaseid
  association [0..1] to ZOK_I_UOM_VH_####                 as _UOM on $projection.SizeUom = _UOM.Msehi

{
  key prod_uuid          as ProdUuid,

      prodid             as Prodid,
      pgid               as Pgid,
      phaseid            as Phaseid,

      @Semantics.amount.currencyCode: 'PriceCurrency'
      price              as Price,

      price_currency     as PriceCurrency,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      height             as Height,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      depth              as Depth,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      width              as Width,

      size_uom           as SizeUom,
      created_by         as CreatedBy,
      creation_time      as CreationTime,
      changed_by         as ChangedBy,
      changed_time       as ChangedTime,
      local_changed_time as LocalChangedTime,

      /*Associations*/
      _Market,
      _PG,
      _PHASE,
      _UOM
}
```

## CDS Interface for view entity Market

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Market Interface'
@Metadata.ignorePropagatedAnnotations: true
define view entity ZOK_I_MARKET_#### as select from zok_d_prod_mrkt
association to parent ZOK_I_PRODUCT_#### as _Product
    on $projection.ProdUuid = _Product.ProdUuid
    association [1..1] to ZOK_I_COUNTRY_#### as _Countries on $projection.Mrktid = _Countries.Mrktid
{
    key prod_uuid as ProdUuid,
    key mrkt_uuid as MrktUuid,
    mrktid as Mrktid,
    startdate as Startdate,
    enddate as Enddate,
    created_by as CreatedBy,
    creation_time as CreationTime,
    changed_by as ChangedBy,
    changed_time as ChangedTime,
    local_changed_time as LocalChangedTime,
    _Product,
    _Countries
}
```

## CDS Interface for view entity Markets dictionary

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Markets data'

define view entity ZOK_I_COUNTRY_####
  as select from zok_d_market_####

{
  key mrktid   as Mrktid,

      mrktname as Mrktname,
      code     as Code,
      imageurl as Imageurl
}
```

## CDS Interface for view entity Country Value Helper

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Country VH'

@ObjectModel.resultSet.sizeCategory: #XS
@ObjectModel.usageType: { serviceQuality: #X, sizeCategory: #S, dataClass: #MIXED }

@Search.searchable: true

@UI.presentationVariant: [ { sortOrder: [ { by: 'Country', direction: #ASC } ] } ]

define view entity ZOK_I_COUNTRY_VH_####
  as select from ZOK_I_COUNTRY_####

{
      @ObjectModel.text.element: [ 'Country' ]
  key Mrktid,

      @Search.defaultSearchElement: true
      Mrktname as Country
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

define view entity ZOK_I_PRODUCT_VH_####
  as select distinct from zok_d_product_####

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

define view entity ZOK_I_CURRENCY_VH_####
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

define view entity ZOK_I_PG_VH_####
  as select from zok_d_prod_grp_####

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

define view entity ZOK_I_PHASE_VH_####
  as select from zpip_d_phase_####

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

define view entity ZOK_I_UOM_VH_####
  as select from zok_d_uom_####

{
      @ObjectModel.text.element: [ 'Msehi' ]
      @Search.defaultSearchElement: true
  key msehi   as Msehi,

      dimid   as Dimid,
      isocode as Isocode
}
```