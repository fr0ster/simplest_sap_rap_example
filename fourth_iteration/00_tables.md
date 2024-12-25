# Table definition

## Table for view entity Critically levels
<a name="z##_d_crtly_"></a>
z##_d_crtly_####

```ABAP
@EndUserText.label : 'Criticality levels'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table z##_d_crtly_#### {

  key client         : abap.clnt not null;
  key id             : abap.char(40) not null;
  treashhold1        : abap.dec(15,4) not null;
  treashhold2        : abap.dec(15,4) not null;
  treashhold3        : abap.dec(15,4) not null;
  treashhold4        : abap.dec(15,4) not null;
  created_by         : abp_creation_user;
  creation_time      : abp_creation_tstmpl;
  changed_by         : abp_lastchange_user;
  changed_time       : abp_lastchange_tstmpl;
  local_changed_time : abp_locinst_lastchange_tstmpl;


}
```