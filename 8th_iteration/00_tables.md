
# Table Definitions

## Dummy extended Structure for Product data enhancement

<a name="z##_s_ext_incl_prod_"></a>
### Z##_S_EXT_INCL_MARKET_####

```ABAP
@EndUserText.label : 'Extension Include'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.enhancement.fieldSuffix : 'ZPR'
@AbapCatalog.enhancement.quotaMaximumFields : 350
@AbapCatalog.enhancement.quotaMaximumBytes : 3500
define structure z##_s_ext_incl_prod_#### {

  dummy_field : abap.char(1);

}
```

## Table for View Entity Product Data

<a name="z##_d_mrkt_"></a>
### Z##_D_MRKT_####

```ABAP
@EndUserText.label : 'Product data'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table z##_d_prod_#### {

  key client         : abap.clnt not null;
  key prod_uuid      : sysuuid_x16 not null;
  prodid             : abap.char(15);
  pgid               : abap.char(3);
  phaseid            : abap.int1;
  @Semantics.amount.currencyCode : 'z##_d_prod_####.price_currency'
  price              : abap.curr(15,2);
  price_currency     : waers_curc;
  height             : abap.dec(20,2);
  depth              : abap.dec(20,2);
  width              : abap.dec(20,2);
  size_uom           : msehi;
  created_by         : abp_creation_user;
  creation_time      : abp_creation_tstmpl;
  changed_by         : abp_lastchange_user;
  changed_time       : abp_lastchange_tstmpl;
  local_changed_time : abp_locinst_lastchange_tstmpl;
  include z##_s_ext_incld_prod_####;

}
```

## Dummy extended Structure for Market data enhancement

<a name="z##_s_ext_incl_mrkt_"></a>
### Z##_S_EXT_INCL_MRKT_####

```ABAP
@EndUserText.label : 'Extension Include'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.enhancement.fieldSuffix : 'ZMR'
@AbapCatalog.enhancement.quotaMaximumFields : 350
@AbapCatalog.enhancement.quotaMaximumBytes : 3500
define structure z##_s_ext_incl_mrkt_#### {

  dummy_field : abap.char(1);

}
```

## Table for View Entity Markets Data

<a name="z##_d_mrkt_"></a>
### Z##_D_MRKT_####

```ABAP
@EndUserText.label : 'Markets data'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table z##_d_mrkt_#### {

  key client         : abap.clnt not null;
  key prod_uuid      : sysuuid_x16 not null;
  key mrkt_uuid      : sysuuid_x16 not null;
  mrktid             : abap.int1;
  startdate          : abap.dats;
  enddate            : abap.dats;
  created_by         : abp_creation_user;
  creation_time      : abp_creation_tstmpl;
  changed_by         : abp_lastchange_user;
  changed_time       : abp_lastchange_tstmpl;
  local_changed_time : abp_locinst_lastchange_tstmpl;
  include z##_s_ext_incl_market_####;

}
```

## Dummy extended Structure for Orders data enhancement

<a name="z##_s_ext_incl_order_"></a>
### Z##_S_EXT_INCL_ORDER_####

```ABAP
@EndUserText.label : 'Extension Include'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.enhancement.fieldSuffix : 'ZOR'
@AbapCatalog.enhancement.quotaMaximumFields : 350
@AbapCatalog.enhancement.quotaMaximumBytes : 3500
define structure z##_s_ext_incl_order_#### {

  dummy_field : abap.char(1);

}
```

## Table for View Entity Orders Data

<a name="z##_d_mrkt_"></a>
### Z##_D_MRKT_####

```ABAP
@EndUserText.label : 'Markets data'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table z##_d_mrkt_#### {

  key client         : abap.clnt not null;
  key prod_uuid      : sysuuid_x16 not null;
  key mrkt_uuid      : sysuuid_x16 not null;
  mrktid             : abap.int1;
  startdate          : abap.dats;
  enddate            : abap.dats;
  created_by         : abp_creation_user;
  creation_time      : abp_creation_tstmpl;
  changed_by         : abp_lastchange_user;
  changed_time       : abp_lastchange_tstmpl;
  local_changed_time : abp_locinst_lastchange_tstmpl;
  include z##_s_ext_incl_market_####;

}
```

## Append structure for Market Status

<a name="z##_s_ext_market_status_"></a>
### Z##_S_EXT_MARKET_STATUS_####

```ABAP
@EndUserText.label : 'Extension include for Market Status'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
extend type z##_s_ext_incl_market_#### with z##_s_ext_market_status_#### {

  zz_status_zmr : abap.char(5);

}
```
