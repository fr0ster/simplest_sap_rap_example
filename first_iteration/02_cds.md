# Определения CDS Projection

## CDS Projection for root view entity Product

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Product data'

@Metadata.allowExtensions: true
@Metadata.ignorePropagatedAnnotations: true

@Search.searchable: true

define root view entity Z##_C_PRODUCT_####
  provider contract transactional_query
  as projection on Z##_I_PRODUCT_####

{
  key ProdUuid,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_PRODUCT_VH_####', element: 'Prodid' } } ]
      @Search.defaultSearchElement: true
      Prodid,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_PG_VH_####', element: 'Pgid' } } ]
      @ObjectModel.text.element: [ 'Pgname' ]
      @Search.defaultSearchElement: true
      Pgid,

      _PG.Pgname        as Pgname,

      @Search.defaultSearchElement: true
      @Semantics.amount.currencyCode: 'PriceCurrency'
      Price,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_PHASE_VH_####', element: 'Phaseid' } } ]
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

      @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_UOM_VH_####', element: 'Msehi' } } ]
      @EndUserText.label: 'Units'
      @ObjectModel.text.element: [ 'DimName' ]
      @Search.defaultSearchElement: true
      @Semantics.unitOfMeasure: true
      SizeUom,

      _UOM.Isocode      as DimName,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_CURRENCY_VH_####', element: 'Currency' } } ]
      @Search.defaultSearchElement: true
      PriceCurrency,

      CreatedBy,
      CreationTime,
      ChangedBy,
      ChangedTime,
      LocalChangedTime,
      /* Associations */
      _Market : redirected to composition child Z##_C_MARKET_####,
      _PG,
      _PHASE,
      _UOM
}
```
## CDS Projection for view entity Market

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Market data'

@Metadata.allowExtensions: true
@Metadata.ignorePropagatedAnnotations: true

@Search.searchable: true

define view entity Z##_C_MARKET_####
  as projection on Z##_I_MARKET_####

{
  key ProdUuid,
  key MrktUuid,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_COUNTRY_VH_####', element: 'Id' } } ]
      @ObjectModel.text.element: [ 'Country' ]
      @Search.defaultSearchElement: true
      Mrktid,

      _Countries.Code     as Code,
      _Countries.Country  as Country,
      _Countries.Imageurl as ImageUrl,
      Startdate,
      Enddate,
      CreatedBy,
      CreationTime,
      ChangedBy,
      ChangedTime,
      LocalChangedTime,
      /* Associations */
      _Product : redirected to parent Z##_C_PRODUCT_####,

      _Countries
}
```