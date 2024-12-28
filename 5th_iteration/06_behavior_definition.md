# Behaviour Definitions

## Behaviour Definition for CDS Product BO Projection Transaction Interface
<a name="z##_ci_product_"></a>
Z##_CI_PRODUCT_####

```ABAP
projection;
strict ( 2 );

define behavior for Z##_CI_PRODUCT_#### alias _Product
{
  use create;
  use update;
  use delete;

  use association _Market { create; }
}

define behavior for Z##_CI_MARKET_#### alias _Market
{
  use update;
  use delete;

  use association _Product;
  use association _Orders { create; }
}
define behavior for Z##_CI_ORDER_#### alias _Order
{
  use update;
  use delete;

  use association _Product;
  use association _Market;
}
```

## Behaviour Definition for CDS Product BO Projection Transaction Query
<a name="z##_c_product_"></a>
Z##_C_PRODUCT_####

```ABAP
projection;
strict ( 2 );

define behavior for Z##_C_PRODUCT_#### alias _Product
{
  use create;
  use update;
  use delete;

  use association _Market { create; }
}

define behavior for Z##_C_MARKET_#### alias _Market
{
  use update;
  use delete;

  use association _Product;
  use association _Orders { create; }
}
define behavior for Z##_C_ORDER_#### alias _Order
{
  use update;
  use delete;

  use association _Product;
  use association _Market;
}
```

