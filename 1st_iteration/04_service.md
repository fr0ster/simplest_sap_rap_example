# Service definition

## CDS Interface for root view entity Product

```ABAP
@EndUserText.label: 'Product Service Definition'
define service Z##_PRODUCT_#### {
  expose Z##_C_PRODUCT_#### as Product;
  expose Z##_C_MARKET_####  as Market;
}
```