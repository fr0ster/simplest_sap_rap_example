# Table definition

## Table for view entity Critically levels

```ABAP
@EndUserText.label : 'Orders data'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table zok_d_order_0001 {

  key client         : abap.clnt not null;
  key prod_uuid      : sysuuid_x16 not null;
  key mrkt_uuid      : sysuuid_x16 not null;
  key order_uuid     : sysuuid_x16 not null;
  quantity           : abap.int2;
  delivery_date      : abap.dats;
  @Semantics.amount.currencyCode : 'zok_d_order_0001.amountcurr'
  netamount          : abap.curr(15,2);
  @Semantics.amount.currencyCode : 'zok_d_order_0001.amountcurr'
  grossamount        : abap.curr(15,2);
  amountcurr         : waers_curc;
  created_by         : abp_creation_user;
  creation_time      : abp_creation_tstmpl;
  changed_by         : abp_lastchange_user;
  changed_time       : abp_lastchange_tstmpl;
  local_changed_time : abp_locinst_lastchange_tstmpl;

}
```