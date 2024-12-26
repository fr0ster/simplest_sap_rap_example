# Metadata Extension definitions

## Metadata Extension for root view entity Product

```ABAP
@Metadata.layer: #CORE

@UI.headerInfo: { typeName: 'Kitchen Appliances',
                  typeNamePlural: 'Kitchen Appliances',
                  title: { type: #STANDARD, label: 'Product', value: 'Prodid' },
                  description: { value: 'Pgname', type: #STANDARD } }

@UI.presentationVariant: [ { sortOrder: [ { by: 'Prodid', direction: #ASC },
                                          { by: 'PGNAME', direction: #ASC } ] } ]

annotate entity Z##_C_PRODUCT_####
    with

{
  @UI.facet: [
               // Filter
               { id: 'idFilter',
                 purpose: #HEADER,
                 type: #DATAPOINT_REFERENCE,
                 targetQualifier: 'idFilter',
                 position: 10 },
               //FldGroup1
               { id: 'idProduct',
                 purpose: #STANDARD,
                 type: #IDENTIFICATION_REFERENCE,
                 label: 'product Info',
                 position: 10 },
               { id: 'Market',
                 purpose: #STANDARD,
                 type: #LINEITEM_REFERENCE,
                 label: 'Market',
                 position: 30,
                 targetElement: '_Market' } ]
  // Elements
  @UI.identification: [ { type: #FOR_ACTION, dataAction: 'change_phase_id', label: 'Next Phase', position: 10 } ]
  @UI.lineItem: [ { type: #FOR_ACTION, dataAction: 'change_phase_id', label: 'Next Phase', position: 10 } ]
  @UI.hidden: true
  ProdUuid;

  @UI.identification: [ { position: 10, label: 'Prod Id' } ]
  @UI.lineItem: [ { position: 10 } ]
  @UI.selectionField: [ { position: 10 } ]
  @UI.textArrangement: #TEXT_ONLY
  Prodid;

  @UI.identification: [ { position: 20, label: 'ProdGroup Id' } ]
  @UI.lineItem: [ { position: 20 } ]
  @UI.selectionField: [ { position: 15 } ]
  @UI.textArrangement: #TEXT_ONLY
  Pgid;

  @UI.dataPoint: { qualifier: 'idFilter', title: 'PHASE' }
  @UI.identification: [ { position: 40 } ]
  @UI.lineItem: [ { position: 30 } ]
  @UI.selectionField: [ { position: 25 } ]
  @UI.textArrangement: #TEXT_ONLY
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

## Metadata Extension for view entity Market

```ABAP
@Metadata.layer: #CORE

annotate entity Z##_C_MARKET_####
    with

{
  @UI.facet: [ { id: 'idMarket', type: #IDENTIFICATION_REFERENCE, label: 'Market Info', position: 10 } ]
  @UI.hidden: true
  ProdUuid;

  @UI.hidden: true
  MrktUuid;

  @UI.identification: [ { position: 10, label: 'Market Id' } ]
  @UI.lineItem: [ { position: 10, type: #STANDARD } ]
  @UI.selectionField: [ { position: 10 } ]
  @UI.textArrangement: #TEXT_ONLY
  Mrktid;

  @UI.hidden: true
  ImageUrl;

  @UI.identification: [ { position: 20, label: 'Start date' } ]
  @UI.lineItem: [ { position: 20 } ]
  Startdate;

  @UI.identification: [ { position: 30, label: 'End date' } ]
  @UI.lineItem: [ { position: 30 } ]
  Enddate;

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

  /* Associations */
  @UI.hidden: true
  _Countries;

  @UI.hidden: true
  _Product;
}
```

## Metadata Extension for view entity Orders

```ABAP
@Metadata.layer: #CORE

annotate entity Z##_C_ORDER_####
    with

{
  @UI.facet: [ { id: 'idOrder', type: #IDENTIFICATION_REFERENCE, label: 'Order Info', position: 10 } ]
  @UI.hidden: true
  ProdUuid;

  @UI.hidden: true
  MrktUuid;

  @UI.hidden: true
  OrderUuid;

  @UI.identification: [ { position: 10, label: 'Quantity' } ]
  @UI.lineItem: [ { position: 10 } ]
  Quantity;

  @UI.identification: [ { position: 20, label: 'DeliveryDate' } ]
  @UI.lineItem: [ { position: 20 } ]
  DeliveryDate;

  @UI.identification: [ { position: 30, label: 'Netamount' } ]
  @UI.lineItem: [ { position: 30 } ]
  Netamount;

  @UI.identification: [ { position: 40, label: 'Grossamount' } ]
  @UI.lineItem: [ { position: 40 } ]
  Grossamount;

  @UI.identification: [ { position: 50, label: 'Amountcurr' } ]
  @UI.lineItem: [ { position: 50 } ]
  Amountcurr;

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

  /* Associations */
  @UI.hidden: true
  _Market;

  @UI.hidden: true
  _Product;
}
```

## Metadata Extension for view entity Critically Levels

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