# Metadata Extension definitions

## Metadata Extension for root view entity Product
<a name="z##_c_product_"></a>
Z##_C_PRODUCT_####

```ABAP
@Metadata.layer: #CORE

annotate entity Z##_C_PRODUCT_####
    with

{
  @UI.facet: [
               { id: 'idProduct',
                 purpose: #STANDARD,
                 type: #IDENTIFICATION_REFERENCE,
                 label: 'General info',
                 position: 10 },
               { id: 'Market',
                 purpose: #STANDARD,
                 type: #LINEITEM_REFERENCE,
                 label: 'Market info',
                 position: 20,
                 targetElement: '_Market' } ]

  @UI.hidden: true
  ProdUuid;

  @UI.identification: [ { position: 10, label: 'Prod Id' } ]
  @UI.lineItem: [ { position: 10 } ]
  @UI.selectionField: [ { position: 10 } ]
  Prodid;

  @UI.identification: [ { position: 20, label: 'ProdGroup Id' } ]
  @UI.lineItem: [ { position: 20 } ]
  @UI.selectionField: [ { position: 15 } ]
  Pgid;

  @UI.dataPoint: { qualifier: 'idFilter', title: 'PHASE' }
  @UI.identification: [ { position: 40 } ]
  @UI.lineItem: [ { position: 30 } ]
  @UI.selectionField: [ { position: 25 } ]
  Phaseid;

  @UI.identification: [ { position: 30 } ]
  @UI.lineItem: [ { position: 40 } ]
  Height;

  @UI.identification: [ { position: 40 } ]
  @UI.lineItem: [ { position: 50 } ]
  Depth;

  @UI.identification: [ { position: 50 } ]
  @UI.lineItem: [ { position: 60 } ]
  Width;

  @UI.identification: [ { position: 60, label: 'Price' } ]
  @UI.lineItem: [ { position: 70 } ]
  Price;

  @UI.hidden: true
  CreatedBy;

  @UI.hidden: true
  CreationTime;

  @UI.hidden: true
  ChangedBy;

  @UI.hidden: true
  ChangedTime;

  @UI.hidden: true
  LocalChangedTime;
}
```
## Metadata Extension for view entity Critically Levels
<a name="z##_c_criticality_levels_"></a>
Z##_C_CRITICALITY_LEVELS_####

```ABAP
@Metadata.layer: #CORE

annotate entity Z##_C_CRITICALITY_LEVELS_####
    with

{
  @UI.facet: [ { id: 'idCriticalityLevels', type: #IDENTIFICATION_REFERENCE, label: 'Criticality levels', position: 10 } ]
  @UI.hidden: true
  Id;

  @UI.identification: [ { position: 10, label: 'Level 1' } ]
  @UI.lineItem: [ { position: 10 } ]
  Treashhold1;

  @UI.identification: [ { position: 20, label: 'Level 2' } ]
  @UI.lineItem: [ { position: 20 } ]
  Treashhold2;

  @UI.identification: [ { position: 30, label: 'Level 3' } ]
  @UI.lineItem: [ { position: 30 } ]
  Treashhold3;

  @UI.identification: [ { position: 40, label: 'Level 4' } ]
  @UI.lineItem: [ { position: 40 } ]
  Treashhold4;
}
```