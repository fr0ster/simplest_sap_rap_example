# Notes

## Eigth Iteration: Side-by-Side Extension

In this iteration, we enhance the User interface.

---

### Key Objectives:

1. Extension Market data table.
2. Extension Market CDS Projection Transactional Query.
2. Extension Market BDEF.

---

### Pay Attention:

1. **To make our scenario more realistic, we make several assumptions**:
  
  - All modifications for adding extensibility are created in the **main packag**e.
  - All extensions are created in a **separate package**.

---

### Steps:

1. **[Create Dummy append Structure for Product data table in main package](./00_tables.md#z##_s_ext_incl_prod_)**

```ABAP
@EndUserText.label : 'Extension Include'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.enhancement.fieldSuffix : 'ZPR'
@AbapCatalog.enhancement.quotaMaximumFields : 350
@AbapCatalog.enhancement.quotaMaximumBytes : 3500
define structure z##_s_ext_incld_prod_#### {

  dummy_field : abap.char(1);

}
```

```ABAP
@EndUserText.label : 'Product data'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
     " Part of code was skipped
define table z##_d_mrkt_#### {

     " Part of code was skipped
  include z##_s_ext_incld_prod_####;

}
```

```ABAP
@EndUserText.label : 'Draft table for entity Z##_I_PRODUCT_####'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
     " Part of code was skipped
define table z##_dt_prod_#### {

     " Part of code was skipped
  include z##_s_ext_incld_prod_####;

}
```

2. **[Create Dummy append Structure for Market data table in main package](./00_tables.md#z##_s_ext_incl_mrkt_)**

```ABAP
@EndUserText.label : 'Extension Include'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.enhancement.fieldSuffix : 'ZMR'
@AbapCatalog.enhancement.quotaMaximumFields : 350
@AbapCatalog.enhancement.quotaMaximumBytes : 3500
define structure z##_s_ext_incld_mrkt_#### {

  dummy_field : abap.char(1);

}
```

```ABAP
@EndUserText.label : 'Markets data'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
     " Part of code was skipped
define table z##_d_mrkt_#### {

     " Part of code was skipped
  include z##_s_ext_incld_mrkt_####;

}
```

```ABAP
@EndUserText.label : 'Draft table for entity Z##_I_MARKET_####'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
     " Part of code was skipped
define table z##_dt_mrkt_#### {

     " Part of code was skipped
  include z##_s_ext_incld_mrkt_####;

}
```

3. **[Create Dummy append Structure for Order data table in main package](./00_tables.md#z##_s_ext_incl_order_)**

```ABAP
@EndUserText.label : 'Extension Include'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.enhancement.fieldSuffix : 'ZOR'
@AbapCatalog.enhancement.quotaMaximumFields : 350
@AbapCatalog.enhancement.quotaMaximumBytes : 3500
define structure z##_s_ext_incld_order_#### {

  dummy_field : abap.char(1);

}
```

```ABAP
@EndUserText.label : 'Orders data'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
     " Part of code was skipped
define table z##_d_order_#### {

     " Part of code was skipped
  include z##_s_ext_incld_order_####;

}
```

```ABAP
@EndUserText.label : 'Draft table for entity Z##_I_ORDER_####'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
     " Part of code was skipped
define table z##_dt_ordr_#### {

     " Part of code was skipped
  include z##_s_ext_incld_order_####;

}
```

4. **[Create new package for Extension and create there Append structure for Market data table in other package](./08_extensions.md#z##_s_ext_market_status_)**

```ABAP
@EndUserText.label : 'Extension include for Market Status'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
extend type z##_s_ext_incld_mrkt_#### with z##_s_ext_market_status_#### {

  zz_status_zmr : abap.char(5);

}
```

5. **Create extension for [CDS Interface](./06_behavior_definition.md#z##_i_ext_market_), [CDS Projection Transactional Interface](./06_behavior_definition.md#z##_ci_ext_market_), [CDS projection transaqtionsl Query](./06_behavior_definition.md#z##_c_ext_market_) Market entity in other package**

```ABAP
extend view entity Z##_I_MARKET_#### with {
   Market.zz_status_zmr
}
```

```ABAP
extend view entity Z##_CI_MARKET_#### with {
   Market.zz_status_zmr
}
```

```ABAP
extend view entity Z##_C_MARKET_#### with {
   Market.zz_status_zmr
}
```

6. **[Add extensible to Behavior definition for interface](./06_behavior_definition.md#z##_i_product_)**

- **Header**

```ABAP
managed with additional save implementation in class zbp_##_i_product_#### unique;
strict ( 2 );
extensible
{
  with determinations on modify;
  with determinations on save;
  with validations on save;
  with additional save;
}
with draft;
```

- **Product entity**

```ABAP
define behavior for Z##_I_PRODUCT_#### alias Product
     " Part of code was skipped
extensible
     " Part of code was skipped
{
     " Part of code was skipped
  draft determine action Prepare extensible
  {
     " Part of code was skipped
  }

     " Part of code was skipped

  mapping for z##_d_prod_#### corresponding extensible
    {
     " Part of code was skipped
    }
}
```

- **Market entity**

```ABAP
define behavior for Z##_I_MARKET_#### alias Market
     " Part of code was skipped
extensible
     " Part of code was skipped
{
     " Part of code was skipped

  mapping for z##_d_mrkt_#### corresponding extensible
    {
     " Part of code was skipped
    }
}
```

- **Order entity**

```ABAP
define behavior for Z##_I_ORDER_#### alias Order
     " Part of code was skipped
extensible
     " Part of code was skipped
{
     " Part of code was skipped

  mapping for z##_d_order_#### corresponding extensible
    {
     " Part of code was skipped
    }
}
```

7. **[Add extensible to Behavior definition for Projection Transactional Interface](./06_behavior_definition.md#z##_ci_product_)**

- Header

```ABAP
interface;
extensible;
use draft;
use side effects;
```

8. **[Add extensible to Behavior definition for Projection Transactional Query](./06_behavior_definition.md#z##_c_product_)**

- **Header**

```ABAP
projection implementation in class zbp_##_c_product_#### unique;
strict ( 2 );
extensible;
use draft;
use side effects;
```

-** Product entity**

```ABAP
define behavior for Z##_C_PRODUCT_#### alias Product
extensible
{
     " Part of code was skipped
}
```

- **Market entity**

```ABAP
define behavior for Z##_C_MARKET_#### alias Market
extensible
{
     " Part of code was skipped
}
```

- **Order entity**

```ABAP
define behavior for Z##_C_ORDER_#### alias Order
extensible
{
     " Part of code was skipped
}
```

8. **Create extension for Behaviour Implementation in other package**

- **[Create extension for CDS Interface, here you should use CDS Projection Transactional Interface or not.

  But we can't create extension for CDS Projection Transactional Interface only(./08_extensions.md#zbp_##_i_ext_product_####)]**

  ```ABAP
  extension using interface z##_ci_product_####
  implementation in class zbp_##_i_ext_product_#### unique;

  extend behavior for Market
  {
      determination zz_setStatus on save { create; field zz_status_zmr; }
  }
  ```

- [Create extension for CDS Projection Transactional Query](./08_extensions.md#zbp_##_c_ext_product_####)

  ```ABAP
  extension for projection;

  extend behavior for Product
  {
  }

  extend behavior for Market
  {
  }

  extend behavior for Order
  {
  }
  ```

---

### Final Steps:

- Activate all updated BDEFs.

- Test the updated services in the Fiori application to ensure proper integration and functionality.

---

### Possible Issues and Solutions:

---

### Summary:

This iteration enabled extensibility and created extensions.