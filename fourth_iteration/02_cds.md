# Определения CDS Projection

## CDS Projection for view entity Criticality Levels

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Criticality levels'

@Metadata.allowExtensions: true

define root view entity Z##_C_CRITICALITY_LEVELS_####
  provider contract transactional_query
  as projection on Z##_I_CRITICALITY_LEVELS_####

{
  key Id,

      Treashhold1,
      Treashhold2,
      Treashhold3,
      Treashhold4
}
```

## CDS Projection for root view entity Product

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Product data'

@Metadata.allowExtensions: true
@Metadata.ignorePropagatedAnnotations: true

@Search.searchable: true

define root view entity ZOK_C_PRODUCT_0001
  provider contract transactional_query
  as projection on ZOK_I_PRODUCT_0001

{
  key ProdUuid,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'ZOK_I_PRODUCT_VH_0001', element: 'Prodid' } } ]
      @Search.defaultSearchElement: true
      Prodid,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'ZOK_I_PG_VH_0001', element: 'Pgid' } } ]
      @ObjectModel.text.element: [ 'Pgname' ]
      @Search.defaultSearchElement: true
      Pgid,

      _PG.Pgname                  as Pgname,

      @Search.defaultSearchElement: true
      @Semantics.amount.currencyCode: 'PriceCurrency'
      Price,

      PriceCriticality,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'ZOK_I_PHASE_VH_0001', element: 'Phaseid' } } ]
      @ObjectModel.text.element: [ 'PhaseName' ]
      @Search.defaultSearchElement: true
      Phaseid,

      _PHASE.Phase                as PhaseName,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      Height,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      Depth,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      Width,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'ZOK_I_UOM_VH_0001', element: 'Msehi' } } ]
      @EndUserText.label: 'Units'
      @ObjectModel.text.element: [ 'DimName' ]
      @Search.defaultSearchElement: true
      @Semantics.unitOfMeasure: true
      SizeUom,

      _UOM.Isocode                as DimName,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'ZOK_I_CURRENCY_VH_0001', element: 'Currency' } } ]
      @Search.defaultSearchElement: true
      PriceCurrency,

      CreatedBy,
      CreationTime,
      ChangedBy,
      ChangedTime,
      LocalChangedTime,
      /* Associations */
      _Market : redirected to composition child ZOK_C_MARKET_0001,

      _PG,
      _PHASE,
      _UOM,
      _PriceCriticality
}

```