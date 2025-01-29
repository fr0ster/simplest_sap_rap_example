# Metadata Extension definitions

## Metadata Extension for root view entity Product
<a name="z##_c_product_"></a>
Z##_C_PRODUCT_####

```ABAP
@Metadata.layer: #CORE

@UI.headerInfo: { typeName: 'Kitchen Appliances',
                  typeNamePlural: 'Kitchen Appliances',
                  imageUrl: 'Imageurl',
                  title: { type: #STANDARD, label: 'Product', value: 'Prodid' },
                  description: { value: 'Pgname', type: #STANDARD } }

@UI.presentationVariant: [ { sortOrder: [ { by: 'Prodid', direction: #ASC },
                                          { by: 'PGNAME', direction: #ASC } ] } ]

annotate entity Z##_C_PRODUCT_####
    with

{
  // FACET SECTION
  @UI.facet: [ // Filter
               { id: 'idFilter',
                 purpose: #HEADER,
                 type: #DATAPOINT_REFERENCE,
                 targetQualifier: 'idFilter',
                 position: 10 },
               // Header
               { id: 'ProductHeaderPId',
                 purpose: #HEADER,
                 type: #DATAPOINT_REFERENCE,
                 targetQualifier: 'ProductId',
                 position: 20 },
                  //FldGroup1
               { id: 'idGeneral',
                 type: #COLLECTION,
                 targetQualifier: 'General',
                 label: 'General Information',
                 position: 10 },
               { id: 'idBasic',
                 type: #FIELDGROUP_REFERENCE,
                 label: 'Basic Data',
                 parentId: 'idGeneral',
                 targetQualifier: 'Basic',
                 position: 10 },
               { id: 'idSizeDim',
                 type: #FIELDGROUP_REFERENCE,
                 label: 'Size Dimensions',
                 parentId: 'idGeneral',
                 targetQualifier: 'Size',
                 position: 20 },
               { id: 'idPrice',
                 type: #FIELDGROUP_REFERENCE,
                 label: 'Price Details',
                 parentId: 'idGeneral',
                 targetQualifier: 'Price',
                 position: 30 },
                 //FldGroup2
               { id: 'idAdmGrp',
                 type: #COLLECTION,
                 label: 'Admin Information',
                 targetQualifier: 'Adm',
                 position: 20 },
               { id: 'idCreate',
                 type: #FIELDGROUP_REFERENCE,
                 label: 'Create Info',
                 parentId: 'idAdmGrp',
                 targetQualifier: 'Create',
                 position: 10 },
               { id: 'idChange',
                 type: #FIELDGROUP_REFERENCE,
                 label: 'Change Info',
                 parentId: 'idAdmGrp',
                 targetQualifier: 'Change',
                 position: 20 },
                 //FldGroup3
              {  id:                 'Market',
                 purpose:            #STANDARD,
                 type:               #LINEITEM_REFERENCE,
                 label:              'Market',
                 position:            30,
                 targetElement:      '_Market'
              } ]

  // ELEMENT LIST
  @UI.identification: [ { type: #FOR_ACTION, dataAction: 'nextPhase',  label: 'Next Phase',  position: 10 },
                        { type: #FOR_ACTION, dataAction: 'priorPhase', label: 'Prior Phase', position: 20 } ]

  @UI.lineItem: [ { type: #FOR_ACTION, dataAction: 'nextPhase',  label: 'Next Phase',  position: 10 },
                  { type: #FOR_ACTION, dataAction: 'priorPhase', label: 'Prior Phase', position: 20 } ]
  ProdUuid;

  @UI.dataPoint: { qualifier: 'ProductId', title: 'Product ID' }
  @UI.fieldGroup: [ { qualifier: 'Basic', position: 20 } ]
  @UI.identification: [ { position: 10, label: 'Product ID' } ]
  @UI.lineItem: [ { position: 20 } ]
  @UI.selectionField: [ { position: 20 } ]
  Prodid;

  @UI.fieldGroup: [ { qualifier: 'Basic', position: 30 } ]
  @UI.lineItem: [ { position: 30 } ]
  @UI.selectionField: [ { position: 40 } ]
  Pgid;

  @UI.hidden: true
  Pgname;

  @UI.fieldGroup: [ { qualifier: 'Basic', position: 40, criticality: 'PhaseCritically' } ]
  @UI.lineItem: [ { position: 40, criticality: 'PhaseCritically' } ]
  @UI.selectionField: [ { position: 30 } ]
  @UI.dataPoint: { qualifier: 'idFilter', title: 'Phase' }
  Phaseid;

  @UI.hidden: true
  PhaseCritically;

  @UI.fieldGroup: [ { qualifier: 'Size', position: 50 },
                    { position: 10, value: 'Height', type: #STANDARD, qualifier: 'AllMeasures' } ]
  Height;

  @UI.fieldGroup: [ { qualifier: 'Size', position: 60 },
                    { position: 20, value: 'Depth', type: #STANDARD, qualifier: 'AllMeasures' } ]
  Depth;

  @UI.fieldGroup: [ { qualifier: 'Size', position: 70 },
                    { position: 30, value: 'Width', type: #STANDARD, qualifier: 'AllMeasures' } ]
  Width;

  @UI.dataPoint: { qualifier: 'ProductNet', title: 'Net Price' }
  @UI.fieldGroup: [ { qualifier: 'Price', position: 90 } ]
  @UI.identification: [ { position: 60 } ]
  @UI.lineItem: [ { position: 50, criticality: 'PriceCriticality' } ]
  @UI.selectionField: [ { position: 90 } ]
  Price;

  @UI.hidden: true
  PriceCriticality;

  @UI.hidden: true
  Imageurl;

  @UI.fieldGroup: [ { qualifier: 'Create', position: 10 } ]
  CreatedBy;

  @UI.fieldGroup: [ { qualifier: 'Create', position: 20 } ]
  CreationTime;

  @UI.fieldGroup: [ { qualifier: 'Change', position: 10 } ]
  ChangedBy;

  @UI.fieldGroup: [ { qualifier: 'Change', position: 20 } ]
  ChangedTime;

  @UI.lineItem: [ { label: 'Size (HxDxW)', position: 70, type: #AS_FIELDGROUP, valueQualifier: 'AllMeasures' } ]
  Measure;
}
```

## Metadata Extension for root view entity Product
<a name="z##_c_market_"></a>
Z##_C_MARKET_####

```ABAP
@Metadata.layer: #CORE

@UI.headerInfo: { title: { label: 'Market', type: #STANDARD, value: 'Mrktid' },
                  description: { value: 'Country', type: #STANDARD },
                  imageUrl: 'Imageurl',
                  typeName: 'Market',
                  typeNamePlural: 'Markets' }

@UI.presentationVariant: [ { sortOrder: [ { by: 'Mrktid', direction: #ASC } ] } ]

annotate entity Z##_C_MARKET_#### with

{
  @UI.facet: [
---------------------------------------------------------------------
//                       Header Facet Annotations                        
              {
                 id:              'HeaderMrktid',
                 purpose:          #HEADER,
                 type:             #DATAPOINT_REFERENCE,
                 targetQualifier: 'Mrktid',
                 position: 10
              },

---------------------------------------------------------------------            
//                       Object Page Tabs                                               
              {
                 id:                 'GeneralInformation',
                 type:               #COLLECTION,
                 label:              'General Information',
                 position:           10
              },
              {
                 id:                 'AdminDataCollection',
                 type:               #COLLECTION,
                 label:              'Admin Data',
                 position:           20
              },
              {  id:                 'Order',
                 purpose:            #STANDARD,
                 type:               #LINEITEM_REFERENCE,
                 label:              'Orders',
                 position:            30,
                 targetElement:      '_Orders'
              },
 --------------------------------------------------------------------             
//                      Field Groups              

              {
                 id:                'BasicData',
                 purpose:           #STANDARD ,
                 parentId:          'GeneralInformation',
                 type:              #FIELDGROUP_REFERENCE,
                 label:             'Basic Data',
                 position:          10,
                 targetQualifier:   'BasicData'
              },
              {
                 id:                'MarketCharacteristics',
                 purpose:           #STANDARD ,
                 parentId:          'GeneralInformation',
                 type:              #FIELDGROUP_REFERENCE,
                 label:             'Market Characteristics',
                 position:          20,
                 targetQualifier:   'MarketCharacteristics'
              },
              {
                 id:                'AdminData',
                 purpose:           #STANDARD ,
                 parentId:          'AdminDataCollection',
                 type:              #FIELDGROUP_REFERENCE,
                 label:             'Admin Data',
                 position:          10,
                 targetQualifier:   'AdminData'
              }
            ]
  --------------------------------------------------------------------     

  @UI: { hidden: true }
  ProdUuid;

  @UI.hidden: true
  MrktUuid;

  -----------------------------
  @UI.dataPoint: { title: 'Market', qualifier: 'Mrktid' }
  @UI.fieldGroup: [ { position: 10, qualifier: 'BasicData' } ]
  @UI.identification: [ { position: 10, label: 'Market' } ]
  @UI.lineItem: [ { position: 30, importance: #HIGH } ]
  @UI.selectionField: [ { position: 10 } ]
  Mrktid;

  @UI.fieldGroup: [ { position: 25, qualifier: 'BasicData' } ]
  @UI.identification: [ { position: 25, label: 'Country ISO-Code' } ]
  Icocode;

  @UI.hidden: true
  Country;

  @UI.hidden: true
  Imageurl;


  @UI.fieldGroup: [ { position: 20, qualifier: 'MarketCharacteristics' } ]
  @UI.lineItem: [ { position: 60, importance: #MEDIUM } ]
  @UI.selectionField: [ { position: 30 } ]
  Startdate;

  @UI.fieldGroup: [ { position: 30, qualifier: 'MarketCharacteristics' } ]
  @UI.lineItem: [ { position: 70, importance: #MEDIUM } ]
  @UI.selectionField: [ { position: 35 } ]
  Enddate;

  ----------------------------      

  @UI.fieldGroup: [ { position: 10, qualifier: 'AdminData', label: 'Created by' } ]
  CreatedBy;

  @UI.fieldGroup: [ { position: 20, qualifier: 'AdminData' } ]
  CreationTime;

  @UI.fieldGroup: [ { position: 30, qualifier: 'AdminData', label: 'Changed by' } ]
  ChangedBy;

  @UI.fieldGroup: [ { position: 40, qualifier: 'AdminData' } ]
  ChangedTime ;
}
```

## Metadata Extension for root view entity Product
<a name="z##_c_order_"></a>
Z##_C_ORDER_####

```ABAP
@Metadata.layer: #CORE

@UI.headerInfo: { title: { label: 'Product', type: #STANDARD, value: 'ProductName' },
                  description: { value: 'ProductName', type: #STANDARD },
                  typeName: 'Order',
                  typeNamePlural: 'Orders' }

@UI.presentationVariant: [ { sortOrder: [ { by: 'ProductName', direction: #ASC } ] } ]

annotate entity Z##_C_ORDER_#### with

{
  @UI.facet: [
---------------------------------------------------------------------
//                       Header Facet Annotations                        
              {
                 id:              'HeaderGrossamount',
                 purpose:          #HEADER,
                 type:             #DATAPOINT_REFERENCE,
                 targetQualifier: 'Grossamount',
                 position: 20
              },

---------------------------------------------------------------------            
//                       Object Page Tabs                                               
              {
                 id:                 'GeneralInformation',
                 type:               #COLLECTION,
                 label:              'General Information',
                 position:           10
              },
              {
                 id:                 'BusinessPartnerTab',
                 type:               #COLLECTION,
                 label:              'Business Partner',
                 position:           15
              },
              {
                 id:                 'AdminDataCollection',
                 type:               #COLLECTION,
                 label:              'Admin Data',
                 position:           20
              },
 --------------------------------------------------------------------             
//                      Field Groups              

              {
                 id:                'BasicData',
                 purpose:           #STANDARD ,
                 parentId:          'GeneralInformation',
                 type:              #FIELDGROUP_REFERENCE,
                 label:             'Basic Data',
                 position:          10,
                 targetQualifier:   'BasicData'
              },
              {
                 id:                'BusinessPartner',
                 purpose:           #STANDARD ,
                 parentId:          'BusinessPartnerTab',
                 type:              #FIELDGROUP_REFERENCE,
                 label:             'Business Partner',
                 position:          20,
                 targetQualifier:   'BusinessPartner'
              },
              {
                 id:                'AdminData',
                 purpose:           #STANDARD ,
                 parentId:          'AdminDataCollection',
                 type:              #FIELDGROUP_REFERENCE,
                 label:             'Admin Data',
                 position:          10,
                 targetQualifier:   'AdminData'
              }
             ]
  --------------------------------------------------------------------   
  // Hidden

  @UI: { hidden: true }
  ProdUuid;

  @UI.hidden: true
  MrktUuid;

  @UI.hidden: true
  OrderUuid;
  -----------------------------  
  // BasicData  

  @UI.fieldGroup: [ { position: 5, qualifier: 'BasicData' } ]
  @UI.identification: [ { position: 5, label: 'Product Name' } ]
  @UI.lineItem: [ { position: 10, importance: #HIGH } ]
  ProductName;

  @UI.fieldGroup: [ { position: 20, qualifier: 'BasicData', label: 'Quantity' } ]
  @UI.lineItem: [ { position: 20, importance: #HIGH } ]
  Quantity;

  @UI.fieldGroup: [ { position: 40, qualifier: 'BasicData', label: 'DeliveryDate' } ]
  @UI.lineItem: [ { position: 40, importance: #HIGH } ]
  DeliveryDate;

  @UI.fieldGroup: [ { position: 50, qualifier: 'BasicData', label: 'Netamount' } ]
  @UI.lineItem: [ { position: 50, importance: #HIGH } ]
  Netamount;

  @UI.dataPoint: { title: 'Gross amount', qualifier: 'Grossamount' }
  @UI.fieldGroup: [ { position: 60, qualifier: 'BasicData', label: 'Grossamount' } ]
  @UI.lineItem: [ { position: 60, importance: #HIGH } ]
  Grossamount;

  -----------------------------
//AdminData  

  @UI.fieldGroup: [ { position: 10, label: 'Created By', qualifier: 'AdminData' } ]
  @UI.lineItem: [ { position: 80, label: 'Created By', importance: #LOW } ]
  CreatedBy;

  @UI.fieldGroup: [ { position: 20, qualifier: 'AdminData' } ]
  CreationTime;

  @UI.fieldGroup: [ { position: 30, label: 'Changed By', qualifier: 'AdminData' } ]
  @UI.lineItem: [ { position: 100, label: 'Changed By', importance: #LOW } ]
  ChangedBy;

  @UI.fieldGroup: [ { position: 40, qualifier: 'AdminData' } ]
  ChangedTime;
}
```