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
  as projection on Z##_I_PRODUCT_#### as Product

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
  as projection on Z##_CI_PRODUCT_#### as Product

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

## CDS Projection Transacional Interface for root view entity Market
<a name="z##_ci_market_"></a>
Z##_CI_MARKET_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Market data'

@Metadata.allowExtensions: true

@Search.searchable: true

define view entity Z##_CI_MARKET_####
  as projection on Z##_I_MARKET_#### as Market

{
  key ProdUuid,
  key MrktUuid,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_COUNTRY_VH_####', element: 'Id' } } ]
      @Search.defaultSearchElement: true
      Mrktid,

      Startdate,
      Enddate,
      CreatedBy,
      CreationTime,
      ChangedBy,
      ChangedTime,
      LocalChangedTime,

      /* Associations */
      _Product : redirected to parent Z##_CI_PRODUCT_####,
      _Orders : redirected to composition child Z##_CI_ORDER_####,

      _Countries
}
```

## CDS Projection Transacional Query for root view entity Market
<a name="z##_c_market_"></a>
Z##_C_MARKET_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Market data'

@Metadata.allowExtensions: true

@Search.searchable: true

define view entity Z##_C_MARKET_####
  as projection on Z##_CI_MARKET_#### as Market

{
  key ProdUuid,
  key MrktUuid,

      @ObjectModel.text.element: [ 'Country' ]
      Mrktid,

      _Countries.Country as Country,
      _Countries.Imageurl as Imageurl,
      _Countries.Code as Icocode,
      Startdate,
      Enddate,
      CreatedBy,
      CreationTime,
      ChangedBy,
      ChangedTime,
      LocalChangedTime,

      /* Associations */
      _Product : redirected to parent Z##_C_PRODUCT_####,
      _Orders : redirected to composition child Z##_C_ORDER_####,

      _Countries
}
```

## CDS Projection Transacional Interface for root view entity Order
<a name="z##_ci_order_"></a>
Z##_CI_ORDER_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Orders data'

@Metadata.allowExtensions: true

@Search.searchable: true

define view entity Z##_CI_ORDER_####
  as projection on Z##_I_ORDER_#### as Orders

{
  key ProdUuid,
  key MrktUuid,
  key OrderUuid,

      Quantity,
      DeliveryDate,
      Netamount,
      Grossamount,

      @Consumption.valueHelpDefinition: [ { entity: { name: 'Z##_I_CURRENCY_VH_####', element: 'Currency' } } ]
      @Search.defaultSearchElement: true
      Amountcurr,

      CreatedBy,
      CreationTime,
      ChangedBy,
      ChangedTime,
      LocalChangedTime,

      /* Associations */
      _Market : redirected to parent Z##_CI_MARKET_####,

      _Product : redirected to Z##_CI_PRODUCT_####
}
```

## CDS Projection Transacional Query for root view entity Order
<a name="z##_c_order_"></a>
Z##_C_ORDER_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Orders data'

@Metadata.allowExtensions: true

@Search.searchable: true

define view entity Z##_C_ORDER_####
  as projection on Z##_CI_ORDER_#### as Orders

{
  key ProdUuid,
  key MrktUuid,
  key OrderUuid,

      _Product._PG.Pgname as ProductName,

      Quantity,
      DeliveryDate,
      Netamount,
      Grossamount,
      Amountcurr,
      CreatedBy,
      CreationTime,
      ChangedBy,
      ChangedTime,
      LocalChangedTime,

      /* Associations */
      _Market : redirected to parent Z##_C_MARKET_####,

      _Product : redirected to Z##_C_PRODUCT_####
}
```