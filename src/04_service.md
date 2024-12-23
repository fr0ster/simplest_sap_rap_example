# Service definition

## CDS Interface for root view entity Product

```ABAP
@EndUserText.label: 'Product Service Definition'
define service Z_PRODUCT_#### {
  expose Z_C_PRODUCT_#### as Product;
  expose Z_C_MARKET_####  as Market;
}
```