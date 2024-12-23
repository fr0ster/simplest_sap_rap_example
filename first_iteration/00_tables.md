# Table definition

## Table for root view entity Product

```ABAP
@EndUserText.label : 'Product data'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
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

}
```

## Table for view entity Markets data

```ABAP
@EndUserText.label : 'Markets data'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
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

}
```

## Table for view entity Countries dictionary

```ABAP
@EndUserText.label : 'Countries dict'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table z##_d_ctry_#### {

  key client : abap.clnt not null;
  key land   : abap.int1 not null;
  name       : abap.char(40);
  code       : abap.lang;
  imageurl   : abap.char(200);

}
```

## Table for view entity Product Group dictionary

```ABAP
@EndUserText.label : 'Product group dict'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table zok_d_pg_0001 {

  key client : abap.clnt not null;
  key pgid   : abap.char(3) not null;
  pgname     : abap.char(30);
  imageurl   : abap.char(200);

}
```

## Table for view entity Phases dictionary

```ABAP
@EndUserText.label : 'Phases dict'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #ALLOWED
define table z##_d_phase_#### {

  key client  : abap.clnt not null;
  key phaseid : abap.int1 not null;
  phase       : abap.char(5);

}
```

## Table for view entity UOM dictionary

```ABAP
@EndUserText.label : 'UOM dict'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table z##_d_uom_#### {

  key client : abap.clnt not null;
  key msehi  : msehi not null;
  dimid      : abap.char(6);
  isocode    : abap.char(3);

}
```