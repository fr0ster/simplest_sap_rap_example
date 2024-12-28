# Metadata Extension definitions

## Metadata Extension for root view entity Market
<a name="z##_c_market_"></a>
Z##_C_MARKET_####

```ABAP
@Metadata.layer: #CORE

annotate entity Z##_C_MARKET_####
    with

{
  @UI.facet: [ { id: 'idMarket', type: #IDENTIFICATION_REFERENCE, label: 'Market Info', position: 10 },
               { id: 'idOrders',
                 type: #LINEITEM_REFERENCE,
                 label: 'Orders Info',
                 position: 20,
                 targetElement: '_Orders' } ]
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

## Metadata Extension for view entity Orders
<a name="z##_c_order_"></a>
Z##_C_ORDER_####

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