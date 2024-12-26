
# CDS Interface Definitions

## CDS Interface for Root View Entity Product

<a name="z##_i_product_"></a>
### Z##_I_PRODUCT_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Product data'
@Metadata.ignorePropagatedAnnotations: true

define root view entity Z##_I_PRODUCT_####
  as select from z##_d_prod_####

  composition [0..*] of Z##_I_MARKET_#### as _Market
  association [1..1] to Z##_I_PG_VH_#### as _PG on $projection.Pgid = _PG.Pgid
  association [1..1] to Z##_I_PHASE_VH_#### as _PHASE on $projection.Phaseid = _PHASE.Phaseid
  association [0..1] to Z##_I_UOM_VH_#### as _UOM on $projection.SizeUom = _UOM.Msehi

{
  key prod_uuid as ProdUuid,
  ...
}
```

## CDS Interface for View Entity Market

<a name="z##_i_market_"></a>
### Z##_I_MARKET_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Market data'
@Metadata.ignorePropagatedAnnotations: true

define view entity Z##_I_MARKET_####
  as select from z##_d_mrkt_####
{
  key prod_uuid as ProdUuid,
  key mrkt_uuid as MrktUuid,
  ...
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
  key land as Id,
  ...
}
```

## CDS Interface for View Entity Country Value Helper

<a name="z##_i_country_vh_"></a>
### Z##_I_COUNTRY_VH_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Country Search Help'

define view entity Z##_I_COUNTRY_VH_####
  as select from Z##_I_COUNTRY_####
{
  key Id,
  ...
}
```

## CDS Interface for View Entity Products Value Helper

<a name="z##_i_product_vh_"></a>
### Z##_I_PRODUCT_VH_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Product ID Search Help'

define view entity Z##_I_PRODUCT_VH_####
  as select distinct from z##_d_prod_####
{
  key prod_uuid as ProdUuid,
  ...
}
```

## CDS Interface for View Entity Currency Value Helper

<a name="z##_i_currency_vh_"></a>
### Z##_I_CURRENCY_VH_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Currency Search Help'

define view entity Z##_I_CURRENCY_VH_####
  as select distinct from I_Currency
{
  key Currency,
  ...
}
```

## CDS Interface for View Entity Product Group Value Helper

<a name="z##_i_pg_vh_"></a>
### Z##_I_PG_VH_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Product Group Search Help'

define view entity Z##_I_PG_VH_####
  as select from z##_d_pg_####
{
  key pgid as Pgid,
  ...
}
```

## CDS Interface for View Entity Phase Value Helper

<a name="z##_i_phase_vh_"></a>
### Z##_I_PHASE_VH_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Phase Search Help'

define view entity Z##_I_PHASE_VH_####
  as select from z##_d_phase_####
{
  key phaseid as Phaseid,
  ...
}
```

## CDS Interface for View Entity UOM Value Helper

<a name="z##_i_uom_vh_"></a>
### Z##_I_UOM_VH_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'UOM Search Help'

define view entity Z##_I_UOM_VH_####
  as select from z##_d_uom_####
{
  key msehi as Msehi,
  ...
}
```