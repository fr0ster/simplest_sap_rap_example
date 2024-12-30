# CDS, type and other extensions

## Market data Append structure
<a name="z##_s_ext_market_status_"></a>
Z##_S_EXT_MARKET_STATUS_####

```ABAP
@EndUserText.label : 'Extension include for Market Status'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
extend type z##_s_ext_incld_mrkt_#### with z##_s_ext_market_status_#### {

  zz_status_zmr : abap.char(5);

}
```

## CDS Interface for root view entity Market Extension
<a name="z##_i_ext_market_"></a>
Z##_I_EXT_MARKET_####

```ABAP
extend view entity Z##_I_MARKET_#### with {
   Market.zz_status_zmr
}
```

## CDS Projection Transacional Interface for root view entity Market Extension
<a name="z##_ci_ext_market_"></a>
Z##_CI_EXT_MARKET_####

```ABAP
extend view entity Z##_CI_MARKET_#### with {
   Market.zz_status_zmr
}
```

## CDS Projection Transacional Query for root view entity Market Extension
<a name="z##_c_ext_market_"></a>
Z##_C_EXT_MARKET_####

```ABAP
extend view entity Z##_C_MARKET_#### with {
   Market.zz_status_zmr
}
```

# Interface BDef extension (and BDef Projection Transactional Interface)
<a name="zbp_##_i_ext_product_####"></a>
ZBP_##_I_EXT_PRODUCT_####

```ABAP
extension using interface z##_ci_product_####
implementation in class zbp_##_i_ext_product_#### unique;

extend behavior for Market
{
    determination zz_setStatus on save { create; field zz_status_zmr; }
}
```

# BDef Projection Transactional Query
<a name="zbp_##_c_ext_product_####"></a>
ZBP_##_C_EXT_PRODUCT_####

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