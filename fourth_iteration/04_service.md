# Service definition

## CDS Interface for root view entity Product

```ABAP
@EndUserText.label: 'Product Service Definition'
define service Z##_PRODUCT_#### {
  expose Z##_C_PRODUCT_####            as Product;
  expose Z##_C_MARKET_####             as Market;
  expose Z##_C_ORDER_####              as Orders;
  expose Z##_C_CRITICALITY_LEVELS_#### as PriceCritically;
}
```