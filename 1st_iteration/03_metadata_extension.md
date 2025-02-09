# Metadata Extension definitions

## Metadata Extension for root view entity Product

<a name="z##_c_product_"></a>

### Z##_C_PRODUCT_####

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
                 position: 30,
                 targetElement: '_Market' } ]

  @UI.hidden: true
  ProdUuid;

  @UI.identification: [ { position: 10, label: 'Prod Id' } ]
  @UI.lineItem: [ { position: 10 } ]
  Prodid;

  @UI.identification: [ { position: 20, label: 'ProdGroup Id' } ]
  @UI.lineItem: [ { position: 20 } ]
  Pgid;

  @UI.dataPoint: { qualifier: 'idFilter', title: 'PHASE' }
  @UI.identification: [ { position: 40 } ]
  @UI.lineItem: [ { position: 30 } ]
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

<a name="z##_c_market_"></a>

### Z##_C_MARKET_####

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