# CDS Projection Definitions

## CDS Projection Transacional Query for Root View Entity Product

<a name="z##_c_product_"></a>
### Z##_C_PRODUCT_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Product data'

@Metadata.allowExtensions: true

define root view entity Z##_C_PRODUCT_####
  provider contract transactional_query
  as projection on Z##_I_PRODUCT_####

{
  key ProdUuid,

      Prodid,
      Pgid,
      _PG.Pgname        as Pgname,
      Price,
      Phaseid,
      _PHASE.Phase      as PhaseName,
      Height,
      Depth,
      Width,
      SizeUom,
      _UOM.Isocode      as DimName,
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

## CDS Projection for View Entity Market
<a name="z##_c_market_"></a>
### Z##_C_MARKET_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Market data'

@Metadata.allowExtensions: true

define view entity Z##_C_MARKET_####
  as projection on Z##_I_MARKET_####

{
  key ProdUuid,
  key MrktUuid,

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