# CDS Interface definitions

## CDS Interface for root view entity Product
<a name="z##_i_product_"></a>
Z##_I_PRODUCT_####

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
  key prod_uuid          as ProdUuid,

      prodid             as Prodid,
      pgid               as Pgid,
      phaseid            as Phaseid,

      case phaseid
      when 1 then 1
      when 2 then 2
      when 3 then 3
      when 4 then 4
      else 0
      end                as PhaseCritically,

      @Semantics.amount.currencyCode: 'PriceCurrency'
      price              as Price,

      case
        when cast(price as abap.dec(15,2)) > _PriceCriticality.Treashhold1 then 1
        when cast(price as abap.dec(15,2)) > _PriceCriticality.Treashhold2 then 2
        when cast(price as abap.dec(15,2)) > _PriceCriticality.Treashhold3 then 3
        when cast(price as abap.dec(15,2)) > _PriceCriticality.Treashhold4 then 4
        else 0
      end                as PriceCriticality,

      price_currency     as PriceCurrency,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      height             as Height,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      depth              as Depth,

      @Semantics.quantity.unitOfMeasure: 'SizeUom'
      width              as Width,

      size_uom           as SizeUom,

      ''                 as Measure,

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

      /*Associations*/
      _Market,
      _PG,
      _PHASE,
      _UOM,
      _PriceCriticality
}
```