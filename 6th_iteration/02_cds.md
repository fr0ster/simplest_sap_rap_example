# CDS Projection Definitions

## CDS Projection Transacional Interface for root view entity Product
<a name="z##_ci_product_"></a>
Z##_CI_PRODUCT_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Product data'

@Metadata.allowExtensions: true

@Search.searchable: true

define root view entity Z##_CI_PRODUCT_####
  provider contract transactional_interface
  as projection on Z##_I_PRODUCT_####

{
  key ProdUuid,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_PRODUCT_VH_####', element: 'Prodid' } } ]
      Prodid,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_PG_VH_####', element: 'Pgid' } } ]
      @Search.defaultSearchElement: true
      Pgid,

      @Search.defaultSearchElement: true
      Price,

      PriceCriticality,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_PHASE_VH_####', element: 'Phaseid' } } ]
      @Search.defaultSearchElement: true
      Phaseid,

      PhaseCritically,
      Height,
      Depth,
      Width,

      @EndUserText.label: 'HxDxW'
      Measure,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_UOM_VH_####', element: 'Msehi' } } ]
      @EndUserText.label: 'Units'
      SizeUom,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_CURRENCY_VH_####', element: 'Currency' } } ]
      @Search.defaultSearchElement: true
      PriceCurrency,

      CreatedBy,
      CreationTime,
      ChangedBy,
      ChangedTime,
      LocalChangedTime,
      /* Associations */
      _Market : redirected to composition child Z##_CI_MARKET_####,

      _PG,
      _PHASE,
      _UOM,
      _PriceCriticality
}
```

## CDS Projection Transacional Query for root view entity Product
<a name="z##_c_product_"></a>
Z##_C_PRODUCT_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Product data'

@Metadata.allowExtensions: true

@Search.searchable: true

define root view entity Z##_C_PRODUCT_####
  provider contract transactional_query
  as projection on Z##_CI_PRODUCT_####

{
  key ProdUuid,

      Prodid,

      @ObjectModel.text.element: [ 'Pgname' ]
      Pgid,

      _PG.Pgname        as Pgname,
      _PG.Imageurl,

      @Search.defaultSearchElement: true
      Price,

      PriceCriticality,

      @ObjectModel.text.element: [ 'PhaseName' ]
      Phaseid,

      PhaseCritically,
      _PHASE.Phase      as PhaseName,
      Height,
      Depth,
      Width,
      Measure,
      SizeUom,
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
      _UOM,
      _PriceCriticality
}
```