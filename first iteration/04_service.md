# Service definition

## CDS Interface for root view entity Product

```ABAP
@EndUserText.label: 'Product Service Definition'
define service ZOK_PRODUCT_#### {
  expose ZOK_C_PRODUCT_#### as Product;
  expose ZOK_C_MARKET_####  as Market;
  expose ZOK_I_COUNTRY_####      as Country;
}
```