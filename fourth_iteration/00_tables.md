# Table definition

## Table for view entity Critically levels

```ABAP
@EndUserText.label : 'Criticality levels'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table z##_d_crtly_#### {

  key client  : abap.clnt not null;
  key id      : abap.char(40) not null;
  treashhold1 : abap.dec(15,4) not null;
  treashhold2 : abap.dec(15,4) not null;
  treashhold3 : abap.dec(15,4) not null;
  treashhold4 : abap.dec(15,4) not null;

}
```