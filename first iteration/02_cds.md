# Определения CDS Projection

## CDS Projection for root view entity Product

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Product Projection'

@Metadata.allowExtensions: true
@Metadata.ignorePropagatedAnnotations: true

@Search.searchable: true

define root view entity ZOK_C_PRODUCT_####
  provider contract transactional_query
  as projection on ZOK_I_PRODUCT_####

{
  key ProdUuid,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'ZOK_I_PRODUCT_VH_####', element: 'Prodid' } } ]
      @Search.defaultSearchElement: true
      Prodid,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'ZOK_I_PG_VH_####', element: 'Pgid' } } ]
      @ObjectModel.text.element: [ 'Pgname' ]
      @Search.defaultSearchElement: true
      Pgid,

      _PG.Pgname        as Pgname,

      @Search.defaultSearchElement: true
      @Semantics.amount.currencyCode: 'PriceCurrency'
      Price,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'ZOK_I_PHASE_VH_####', element: 'Phaseid' } } ]
      @ObjectModel.text.element: [ 'PhaseName' ]
      @Search.defaultSearchElement: true
      Phaseid,

      _PHASE.Phase      as PhaseName,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      Height,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      Depth,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      Width,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'ZOK_I_UOM_VH_####', element: 'Msehi' } } ]
      @EndUserText.label: 'Units'
      @ObjectModel.text.element: [ 'DimName' ]
      @Search.defaultSearchElement: true
      @Semantics.unitOfMeasure: true
      SizeUom,

      _UOM.Isocode      as DimName,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'ZOK_I_CURRENCY_VH_####', element: 'Currency' } } ]
      @Search.defaultSearchElement: true
      PriceCurrency,

      CreatedBy,
      CreationTime,
      ChangedBy,
      ChangedTime,
      LocalChangedTime,
      /* Associations */
      _Market : redirected to composition child ZOK_C_MARKET_1234,
      _PG,
      _PHASE,
      _UOM
}
```
## CDS Projection for view entity Market

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Market Projection'

@Metadata.allowExtensions: true
@Metadata.ignorePropagatedAnnotations: true

@Search.searchable: true

define view entity ZOK_C_MARKET_####
  as projection on ZOK_I_MARKET_####

{
  key ProdUuid,
  key MrktUuid,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'ZOK_I_COUNTRY_VH_####', element: 'Mrktid' } } ]
      @ObjectModel.text.element: [ 'Country' ]
      @Search.defaultSearchElement: true
      Mrktid,

      _Countries.Code     as Code,
      _Countries.Mrktname as Country,
      _Countries.Imageurl as ImageUrl,
      Startdate,
      Enddate,
      CreatedBy,
      CreationTime,
      ChangedBy,
      ChangedTime,
      LocalChangedTime,
      /* Associations */
      _Product : redirected to parent ZOK_C_PRODUCT_1234,

      _Countries
}
```