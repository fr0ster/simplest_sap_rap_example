# Определения CDS Projection Definitions

## CDS Projection for view entity Orders
<a name="z##_i_order_"></a>
Z##_I_ORDER_####

```ABAP
@AccessControl.authorizationCheck: #NOT_REQUIRED

@EndUserText.label: 'Orders data'

@Metadata.allowExtensions: true

define view entity Z##_C_ORDER_####
  as projection on Z##_I_ORDER_####

{
  key ProdUuid,
  key MrktUuid,
  key OrderUuid,

      Quantity,
      DeliveryDate,

      @Semantics.amount.currencyCode: 'Amountcurr'
      Netamount,

      @Semantics.amount.currencyCode: 'Amountcurr'
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

## CDS Projection for view entity Market
<a name="z##_c_market_"></a>
Z##_C_MARKET_####

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
      _Orders : redirected to composition child Z##_C_ORDER_####,

      _Countries
}
```