# CDS Projection Definitions

## CDS Projection for view entity Criticality Levels
<a name="z##_c_criticality_levels_"></a>
Z##_C_CRITICALITY_LEVELS_####

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
  as projection on Z##_I_PRODUCT_####

{
  key ProdUuid,

      Prodid,
      Pgid,
      _PG.Pgname                  as Pgname,
      Price,

      PriceCriticality,
      Phaseid,
      _PHASE.Phase                as PhaseName,
      Height,
      Depth,
      Width,
      SizeUom,
      _UOM.Isocode                as DimName,
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