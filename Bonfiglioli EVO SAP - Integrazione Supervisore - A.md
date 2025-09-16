# Bonfiglioli_EVO_SAP - Integrazione Supervisore - Allegato A

# Introduction

Il seguente documento costitisce un allegato tecnico al manuale funzionale Bonfiglioli_EVO_SAP Integrazione_Supervisore.docx.

Lo scopo di questa specifica è di fornire tutti i dettagli tecnici necessari al consumo dei servizi REST esposti da Digital Manufacturing da parte di servizi terzi presenti nell’ecosistema Bonfiglioli.

La numerazione dei capitoli specifici per ogni funzionalità/API, è la stessa del documento funzionale, in modo da avere una correlazione tra i due.

# Authentication

Di seguito sono elencati i dati per il flusso di autenticazione OAuth2 necessario per l’ottenimento di un JWT token da includere ed utilizzare nelle varie chiamate alla APIs REST di Digital Manufacturing

## Test Environment

> DM Tenant Base URL: https://api.eu20.hana.ondemand.com

<aside>
‼️

Questo url è da intendersi come la “radice” di ogni endpoint da chiamare per le specifiche APIs. Corrisponde nei vari paragrafi alla dicitura {baseUrl}

</aside>

> Access Token URL: https://bonfiglioli-sap-dm-test-mn0p51tk.authentication.eu20.hana.ondemand.com/oauth/token

> Client Id: sb-df8c469b-0625-4639-bc09-73608a87e9de!b108183|dmc-services-quality!b330

> Client Secret: 4ae30905-b9a0-4f51-a258-800aece41b41$LumCV3y70Wl74UjkZHh3BvwW8kP4l5yMQjzp4exYDb8=

## Production Environment

TBD

# 3.2 Orders

> Verb: GET

## Endpoint

<aside>
🌐

{baseUrl}/order/v1/orders?plant={plant}&order={order}

</aside>

## Parameters

- plant → codice identificativo del plant
- order → codice identificativo dell’ordine di cui si desiderano i dettagli

## Response Example

```json
{
  "plant": "EVO",
  "order": "12732532",
  "status": "RELEASABLE",
  "releaseStatus": "RELEASED",
  "executionStatus": "NOT_IN_EXECUTION",
  "orderType": "PRODUCTION",
  "orderCategory": "PRODUCTION_ORDER",
  "material": {
    "material": "XEA802720007",
    "version": "ERP001",
    "description": "C 12 2 F 11.9 S05 B5"
  },
  "bom": {
    "bom": "12732532-XEA802720007-1-1",
    "version": "ERP001",
    "bomType": "SHOPORDERBOM",
    "type": "SHOP_ORDER"
  },
  "routing": {
    "routing": "12732532",
    "version": "ERP001",
    "routingType": "SHOPORDER_SPECIFIC",
    "type": "SHOP_ORDER"
  },
  "productionQuantity": 30.0,
  "productionUnitOfMeasure": "ST",
  "productionUnitOfMeasureObject": {
    "uom": "PC",
    "internalUom": "ST",
    "shortText": "Piece",
    "longText": "Piece",
    "isPrimary": null
  },
  "buildQuantity": 30.0,
  "orderedQuantity": 30.0,
  "releasedQuantity": 30.0,
  "doneQuantity": 0.0,
  "doneQuantityInProductionUom": 0,
  "goodsReceiptQuantity": 0.0,
  "goodsReceiptQuantityInProductionUom": 0,
  "erpUnitOfMeasure": "ST",
  "baseUnitOfMeasureObject": {
    "uom": "PC",
    "internalUom": "ST",
    "shortText": "Piece",
    "longText": "Piece",
    "isPrimary": null
  },
  "batchNumber": "",
  "priority": 500,
  "plannedStartDate": "2025-09-11T22:00:00Z",
  "plannedCompletionDate": "2025-09-17T22:00:00Z",
  "scheduledStartDate": "2025-09-16T22:00:00Z",
  "scheduledCompletionDate": "2025-09-16T22:00:00Z",
  "sfcs": [
    "EVO254000015",
    "EVO254000016",
    "EVO254000017",
    "EVO254000018",
    "EVO254000019",
    "EVO254000020",
    "EVO254000021",
    "EVO254000022",
    "EVO254000023",
    "EVO254000024",
    "EVO254000025",
    "EVO254000026",
    "EVO254000027",
    "EVO254000028",
    "EVO254000029",
    "EVO254000030",
    "EVO254000031",
    "EVO254000032",
    "EVO254000033",
    "EVO254000034",
    "EVO254000035",
    "EVO254000036",
    "EVO254000037",
    "EVO254000038",
    "EVO254000039",
    "EVO254000040",
    "EVO254000041",
    "EVO254000042",
    "EVO254000043",
    "EVO254000044"
  ],
  "customValues": [
    {
      "attribute": "MANUFACTURING_ORDER_PARENT",
      "value": ""
    }
  ],
  "toleranceUnder": 0.0,
  "minDeliveryQuantity": 30.0,
  "toleranceOver": 0.0,
  "maxDeliveryQuantity": 30.0,
  "workCenters": [
    {
      "workCenterRef": "WorkCenterBO:EVO,496",
      "workCenter": "496",
      "description": "STGL C32 -->C11"
    },
    {
      "workCenterRef": "WorkCenterBO:EVO,S21",
      "workCenter": "S21",
      "description": "LINEA LAMMAS"
    }
  ],
  "productionVersion": "0001",
  "putawayStorageLocation": "EVO1",
  "erpRoutingGroup": "50328399",
  "warehouseNumber": ""
}
```

## Applications/Use Cases

- Conoscere i dati di testata di un ordine di produzione
- Conoscere i dati minimi delle entità associate all’ordine (bom, routing, ..) per ricavarne successivamente i dettagli
- Conoscere gli identificativi seriali di un determinato ordine

# 3.3 Routing & operations

> Verb: GET

## Endpoint

<aside>
🌐

{baseUrl}/routing/v1/routings?plant={plant}&routing={routing}&type={type}&version={version}

</aside>

## Parameters

- plant → codice identificativo del plant
- routing→ codice identificativo del ciclo
- type → tipologia del ciclo
- version → versione del ciclo

## Response Example

```json
[
  {
    "plant": "EVO",
    "routing": "12732532",
    "routingType": "SHOP_ORDER",
    "version": "ERP001",
    "currentVersion": true,
    "status": "RELEASABLE",
    "description": "XEA802720007",
    "relaxedFlow": false,
    "entryRoutingStepId": "0010",
    "quantityValidation": true,
    "automaticGoodsReceipt": false,
    "routingSteps": [
      {
        "stepId": "0010",
        "sequence": 10,
        "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
        "entry": true,
        "routingOperation": {
          "stepType": "NORMAL",
          "operationActivity": {
            "operationActivity": "12732532-0-0010",
            "version": "ERP001"
          },
          "baseQuantity": {
            "value": 1.0,
            "unitOfMeasure": {
              "internalUom": "ST"
            }
          },
          "weighRelevant": false,
          "automaticGoodsReceipt": false,
          "customValues": []
        },
        "workCenter": {
          "workCenter": "496"
        },
        "reportingStep": "0010",
        "controlKey": {
          "controlKey": "BP07"
        },
        "lastReportingStep": false,
        "rework": false,
        "queueDecisionType": "COMPLETING_OPERATOR",
        "erpSequence": "0",
        "nextStepList": ["0030"],
        "routingStepComponentList": [
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000612602727",
                "version": "ERP001"
              },
              "sequence": 170
            },
            "sequence": 1,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000710306120",
                "version": "ERP001"
              },
              "sequence": 200
            },
            "sequence": 2,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000710475018",
                "version": "ERP001"
              },
              "sequence": 210
            },
            "sequence": 3,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000712482001",
                "version": "ERP001"
              },
              "sequence": 240
            },
            "sequence": 4,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000714370001",
                "version": "ERP001"
              },
              "sequence": 250
            },
            "sequence": 5,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000718298009",
                "version": "ERP001"
              },
              "sequence": 270
            },
            "sequence": 6,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000710475025",
                "version": "ERP001"
              },
              "sequence": 330
            },
            "sequence": 7,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000710475043",
                "version": "ERP001"
              },
              "sequence": 10
            },
            "sequence": 8,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000610003858",
                "version": "ERP001"
              },
              "sequence": 20
            },
            "sequence": 9,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000612303190",
                "version": "ERP001"
              },
              "sequence": 40
            },
            "sequence": 10,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000612650416",
                "version": "ERP001"
              },
              "sequence": 50
            },
            "sequence": 11,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000613150760",
                "version": "ERP001"
              },
              "sequence": 60
            },
            "sequence": 12,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000616350570",
                "version": "ERP001"
              },
              "sequence": 70
            },
            "sequence": 13,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000710308190",
                "version": "ERP001"
              },
              "sequence": 80
            },
            "sequence": 14,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000712750070",
                "version": "ERP001"
              },
              "sequence": 90
            },
            "sequence": 15,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000712750071",
                "version": "ERP001"
              },
              "sequence": 100
            },
            "sequence": 16,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000712750072",
                "version": "ERP001"
              },
              "sequence": 110
            },
            "sequence": 17,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000712750074",
                "version": "ERP001"
              },
              "sequence": 120
            },
            "sequence": 18,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000718298012",
                "version": "ERP001"
              },
              "sequence": 130
            },
            "sequence": 19,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000718299027",
                "version": "ERP001"
              },
              "sequence": 140
            },
            "sequence": 20,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000718299041",
                "version": "ERP001"
              },
              "sequence": 150
            },
            "sequence": 21,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000710475018",
                "version": "ERP001"
              },
              "sequence": 160
            },
            "sequence": 22,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000710475060",
                "version": "ERP001"
              },
              "sequence": 30
            },
            "sequence": 23,
            "quantity": 30.0
          }
        ]
      },
      {
        "stepId": "0030",
        "sequence": 20,
        "description": "CHIUSURA + COLLAUDO",
        "entry": false,
        "routingOperation": {
          "stepType": "NORMAL",
          "operationActivity": {
            "operationActivity": "12732532-0-0030",
            "version": "ERP001"
          },
          "baseQuantity": {
            "value": 1.0,
            "unitOfMeasure": {
              "internalUom": "ST"
            }
          },
          "weighRelevant": false,
          "automaticGoodsReceipt": false,
          "customValues": []
        },
        "workCenter": {
          "workCenter": "S21"
        },
        "reportingStep": "0030",
        "controlKey": {
          "controlKey": "BP07"
        },
        "lastReportingStep": true,
        "rework": false,
        "queueDecisionType": "COMPLETING_OPERATOR",
        "erpSequence": "0",
        "routingStepComponentList": [
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000614600327",
                "version": "ERP001"
              },
              "sequence": 180
            },
            "sequence": 1,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000616302453",
                "version": "ERP001"
              },
              "sequence": 190
            },
            "sequence": 2,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000711353009",
                "version": "ERP001"
              },
              "sequence": 220
            },
            "sequence": 3,
            "quantity": 180.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000712407006",
                "version": "ERP001"
              },
              "sequence": 230
            },
            "sequence": 4,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000718001013",
                "version": "ERP001"
              },
              "sequence": 260
            },
            "sequence": 5,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000718307004",
                "version": "ERP001"
              },
              "sequence": 280
            },
            "sequence": 6,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000718401111",
                "version": "ERP001"
              },
              "sequence": 290
            },
            "sequence": 7,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000718401151",
                "version": "ERP001"
              },
              "sequence": 300
            },
            "sequence": 8,
            "quantity": 60.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000718407002",
                "version": "ERP001"
              },
              "sequence": 310
            },
            "sequence": 9,
            "quantity": 30.0
          },
          {
            "bomComponent": {
              "bom": {
                "bom": "12732532-XEA802720007-1-1",
                "version": "ERP001",
                "bomType": "SHOPORDERBOM",
                "plant": "EVO"
              },
              "material": {
                "plant": "EVO",
                "material": "000000000715700030",
                "version": "ERP001"
              },
              "sequence": 320
            },
            "sequence": 10,
            "quantity": 10.8
          }
        ]
      }
    ],
    "customValues": [],
    "routingOperationGroups": [
      {
        "routingOperationGroup": "12732532-0-0010",
        "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
        "routingOperationGroupSteps": [
          {
            "routingStep": {
              "stepId": "0010",
              "sequence": 10,
              "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
              "entry": true,
              "routingOperation": {
                "stepType": "NORMAL",
                "operationActivity": {
                  "operationActivity": "12732532-0-0010",
                  "version": "ERP001"
                },
                "baseQuantity": {
                  "value": 1.0,
                  "unitOfMeasure": {
                    "internalUom": "ST"
                  }
                },
                "weighRelevant": false,
                "automaticGoodsReceipt": false,
                "customValues": []
              },
              "workCenter": {
                "workCenter": "496"
              },
              "reportingStep": "0010",
              "controlKey": {
                "controlKey": "BP07"
              },
              "lastReportingStep": false,
              "rework": false,
              "queueDecisionType": "COMPLETING_OPERATOR",
              "erpSequence": "0",
              "nextStepList": ["0030"],
              "routingStepComponentList": [
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000612602727",
                      "version": "ERP001"
                    },
                    "sequence": 170
                  },
                  "sequence": 1,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000710306120",
                      "version": "ERP001"
                    },
                    "sequence": 200
                  },
                  "sequence": 2,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000710475018",
                      "version": "ERP001"
                    },
                    "sequence": 210
                  },
                  "sequence": 3,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000712482001",
                      "version": "ERP001"
                    },
                    "sequence": 240
                  },
                  "sequence": 4,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000714370001",
                      "version": "ERP001"
                    },
                    "sequence": 250
                  },
                  "sequence": 5,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000718298009",
                      "version": "ERP001"
                    },
                    "sequence": 270
                  },
                  "sequence": 6,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000710475025",
                      "version": "ERP001"
                    },
                    "sequence": 330
                  },
                  "sequence": 7,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000710475043",
                      "version": "ERP001"
                    },
                    "sequence": 10
                  },
                  "sequence": 8,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000610003858",
                      "version": "ERP001"
                    },
                    "sequence": 20
                  },
                  "sequence": 9,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000612303190",
                      "version": "ERP001"
                    },
                    "sequence": 40
                  },
                  "sequence": 10,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000612650416",
                      "version": "ERP001"
                    },
                    "sequence": 50
                  },
                  "sequence": 11,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000613150760",
                      "version": "ERP001"
                    },
                    "sequence": 60
                  },
                  "sequence": 12,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000616350570",
                      "version": "ERP001"
                    },
                    "sequence": 70
                  },
                  "sequence": 13,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000710308190",
                      "version": "ERP001"
                    },
                    "sequence": 80
                  },
                  "sequence": 14,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000712750070",
                      "version": "ERP001"
                    },
                    "sequence": 90
                  },
                  "sequence": 15,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000712750071",
                      "version": "ERP001"
                    },
                    "sequence": 100
                  },
                  "sequence": 16,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000712750072",
                      "version": "ERP001"
                    },
                    "sequence": 110
                  },
                  "sequence": 17,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000712750074",
                      "version": "ERP001"
                    },
                    "sequence": 120
                  },
                  "sequence": 18,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000718298012",
                      "version": "ERP001"
                    },
                    "sequence": 130
                  },
                  "sequence": 19,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000718299027",
                      "version": "ERP001"
                    },
                    "sequence": 140
                  },
                  "sequence": 20,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000718299041",
                      "version": "ERP001"
                    },
                    "sequence": 150
                  },
                  "sequence": 21,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000710475018",
                      "version": "ERP001"
                    },
                    "sequence": 160
                  },
                  "sequence": 22,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000710475060",
                      "version": "ERP001"
                    },
                    "sequence": 30
                  },
                  "sequence": 23,
                  "quantity": 30.0
                }
              ]
            }
          }
        ]
      },
      {
        "routingOperationGroup": "12732532-0-0030",
        "description": "CHIUSURA + COLLAUDO",
        "routingOperationGroupSteps": [
          {
            "routingStep": {
              "stepId": "0030",
              "sequence": 20,
              "description": "CHIUSURA + COLLAUDO",
              "entry": false,
              "routingOperation": {
                "stepType": "NORMAL",
                "operationActivity": {
                  "operationActivity": "12732532-0-0030",
                  "version": "ERP001"
                },
                "baseQuantity": {
                  "value": 1.0,
                  "unitOfMeasure": {
                    "internalUom": "ST"
                  }
                },
                "weighRelevant": false,
                "automaticGoodsReceipt": false,
                "customValues": []
              },
              "workCenter": {
                "workCenter": "S21"
              },
              "reportingStep": "0030",
              "controlKey": {
                "controlKey": "BP07"
              },
              "lastReportingStep": true,
              "rework": false,
              "queueDecisionType": "COMPLETING_OPERATOR",
              "erpSequence": "0",
              "routingStepComponentList": [
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000614600327",
                      "version": "ERP001"
                    },
                    "sequence": 180
                  },
                  "sequence": 1,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000616302453",
                      "version": "ERP001"
                    },
                    "sequence": 190
                  },
                  "sequence": 2,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000711353009",
                      "version": "ERP001"
                    },
                    "sequence": 220
                  },
                  "sequence": 3,
                  "quantity": 180.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000712407006",
                      "version": "ERP001"
                    },
                    "sequence": 230
                  },
                  "sequence": 4,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000718001013",
                      "version": "ERP001"
                    },
                    "sequence": 260
                  },
                  "sequence": 5,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000718307004",
                      "version": "ERP001"
                    },
                    "sequence": 280
                  },
                  "sequence": 6,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000718401111",
                      "version": "ERP001"
                    },
                    "sequence": 290
                  },
                  "sequence": 7,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000718401151",
                      "version": "ERP001"
                    },
                    "sequence": 300
                  },
                  "sequence": 8,
                  "quantity": 60.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000718407002",
                      "version": "ERP001"
                    },
                    "sequence": 310
                  },
                  "sequence": 9,
                  "quantity": 30.0
                },
                {
                  "bomComponent": {
                    "bom": {
                      "bom": "12732532-XEA802720007-1-1",
                      "version": "ERP001",
                      "bomType": "SHOPORDERBOM",
                      "plant": "EVO"
                    },
                    "material": {
                      "plant": "EVO",
                      "material": "000000000715700030",
                      "version": "ERP001"
                    },
                    "sequence": 320
                  },
                  "sequence": 10,
                  "quantity": 10.8
                }
              ]
            }
          }
        ]
      }
    ],
    "routingErpSequences": [],
    "modifiedDateTime": "2025-09-15T07:28:26.061Z",
    "createdDateTime": "2025-09-12T15:50:21.548Z"
  }
]
```

## Applications/Use Cases

- Conoscere il dettaglio delle operazioni di un determinato ciclo di un odp

# 3.4 Bill of Materials

> Verb: GET

## Endpoint

<aside>
🌐

{baseUrl}/bom/v1/boms?plant={plant}&bom={bom}&type={type}&version={version}&readReservationsAndBatch=true

</aside>

## Parameters

- plant → codice identificativo del plant
- bom→ codice identificativo della distinta base
- type → tipologia della distinta base
- version → versione della distinta base
- readReservationsAndBatch (optional) → true = ritorna (se manutenuti) eventuali dati relativi a batch/reservation number/warehouse

## Response Example

```json
[
  {
    "bom": "12732532-XEA802720007-1-1",
    "version": "ERP001",
    "bomType": "SHOP_ORDER",
    "plant": "EVO",
    "description": "XEA802720007",
    "status": "RELEASABLE",
    "currentVersion": true,
    "baseQuantity": 30.0,
    "baseUnitOfMeasure": "ST",
    "components": [
      {
        "sequence": 10,
        "erpSequence": 10,
        "material": {
          "plant": "EVO",
          "material": "000000000710475043",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0020",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 20,
        "erpSequence": 10,
        "material": {
          "plant": "EVO",
          "material": "000000000610003858",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0022",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 30,
        "erpSequence": 10,
        "material": {
          "plant": "EVO",
          "material": "000000000710475060",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0037",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 40,
        "erpSequence": 20,
        "material": {
          "plant": "EVO",
          "material": "000000000612303190",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0023",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 50,
        "erpSequence": 30,
        "material": {
          "plant": "EVO",
          "material": "000000000612650416",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0024",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 60,
        "erpSequence": 40,
        "material": {
          "plant": "EVO",
          "material": "000000000613150760",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0025",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 70,
        "erpSequence": 50,
        "material": {
          "plant": "EVO",
          "material": "000000000616350570",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0026",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 80,
        "erpSequence": 60,
        "material": {
          "plant": "EVO",
          "material": "000000000710308190",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0027",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 90,
        "erpSequence": 70,
        "material": {
          "plant": "EVO",
          "material": "000000000712750070",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0028",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 100,
        "erpSequence": 80,
        "material": {
          "plant": "EVO",
          "material": "000000000712750071",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0029",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 110,
        "erpSequence": 90,
        "material": {
          "plant": "EVO",
          "material": "000000000712750072",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0030",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 120,
        "erpSequence": 100,
        "material": {
          "plant": "EVO",
          "material": "000000000712750074",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0031",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 130,
        "erpSequence": 110,
        "material": {
          "plant": "EVO",
          "material": "000000000718298012",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0032",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 140,
        "erpSequence": 120,
        "material": {
          "plant": "EVO",
          "material": "000000000718299027",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0033",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 150,
        "erpSequence": 130,
        "material": {
          "plant": "EVO",
          "material": "000000000718299041",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0034",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 160,
        "erpSequence": 140,
        "material": {
          "plant": "EVO",
          "material": "000000000710475018",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0035",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 170,
        "erpSequence": 280,
        "material": {
          "plant": "EVO",
          "material": "000000000612602727",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0002",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 180,
        "erpSequence": 390,
        "material": {
          "plant": "EVO",
          "material": "000000000614600327",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0030"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0003",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 190,
        "erpSequence": 700,
        "material": {
          "plant": "EVO",
          "material": "000000000616302453",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0030"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0004",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 200,
        "erpSequence": 780,
        "material": {
          "plant": "EVO",
          "material": "000000000710306120",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0005",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 210,
        "erpSequence": 1050,
        "material": {
          "plant": "EVO",
          "material": "000000000710475018",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0006",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 220,
        "erpSequence": 1080,
        "material": {
          "plant": "EVO",
          "material": "000000000711353009",
          "version": "ERP001"
        },
        "quantity": 6.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0030"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0007",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 1.8e2,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 230,
        "erpSequence": 1210,
        "material": {
          "plant": "EVO",
          "material": "000000000712407006",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0030"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0008",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 240,
        "erpSequence": 1220,
        "material": {
          "plant": "EVO",
          "material": "000000000712482001",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0009",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 250,
        "erpSequence": 1380,
        "material": {
          "plant": "EVO",
          "material": "000000000714370001",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0010",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 260,
        "erpSequence": 1960,
        "material": {
          "plant": "EVO",
          "material": "000000000718001013",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0030"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0011",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 270,
        "erpSequence": 1980,
        "material": {
          "plant": "EVO",
          "material": "000000000718298009",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0012",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 280,
        "erpSequence": 2110,
        "material": {
          "plant": "EVO",
          "material": "000000000718307004",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0030"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0013",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 290,
        "erpSequence": 2170,
        "material": {
          "plant": "EVO",
          "material": "000000000718401111",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0030"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0014",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 300,
        "erpSequence": 2545,
        "material": {
          "plant": "EVO",
          "material": "000000000718401151",
          "version": "ERP001"
        },
        "quantity": 2.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0030"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0015",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 6e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 310,
        "erpSequence": 3600,
        "material": {
          "plant": "EVO",
          "material": "000000000718407002",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0030"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0016",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 320,
        "erpSequence": 6750,
        "material": {
          "plant": "EVO",
          "material": "000000000715700030",
          "version": "ERP001"
        },
        "quantity": 0.36,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0030"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "L",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0017",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 10.8,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      },
      {
        "sequence": 330,
        "erpSequence": 9996,
        "material": {
          "plant": "EVO",
          "material": "000000000710475025",
          "version": "ERP001"
        },
        "quantity": 1.0,
        "customValues": [],
        "assemblyOperationActivity": {
          "operationActivity": "12732532-0-0010"
        },
        "componentType": "NORMAL",
        "unitOfMeasure": "ST",
        "storageLocation": "EVO1",
        "alternativeItemGroup": "",
        "reservationItemNumber": "0018",
        "reservationOrderNumber": "0318017866",
        "alternates": [],
        "backflushEnabled": true,
        "totalQuantity": 3e1,
        "warehouseNumber": "",
        "assemblyQuantityAsRequired": false,
        "alternatesEnabled": false
      }
    ]
  }
]
```

## Applications/Use Cases

- Conoscere il dettaglio di una distinta base di un odp

# 3.5 Materials Classification

> Verb: PODT

## Endpoint

<aside>
🌐

{baseUrl}/classification/v1/read

</aside>

## Parameters

- plant → codice identificativo del plant
- objectKeys → identificativo del materiale/i di cui si desiderano le caratteristiche
- objectType → MATERIAL (hardcoding)+
- classType → tipologia classe, da definire se un parametro dinamico o un hardcoding a seconda delle configurazioni su S4
- classes → identificativo classe, da definire se un parametro dinamico o un hardcoding a seconda delle configurazioni su S4

```json
{
  "plant": "EVO",
  "objectKeys": ["JB00010153"],
  "objectType": "MATERIAL",
  "classType": "300",
  "classes": ["CLASS_M"]
}
```

## Response Example

```json
{
  "classificationAssignmentHeaders": [
    {
      "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
      "classType": "300",
      "classInternalId": "75adacc8-9ce2-4cb1-82c3-24b52ef1913f",
      "objectKey": "JB00010153",
      "objectType": "MATERIAL",
      "status": "1",
      "standardClass": "true",
      "assignmentCharacteristicValues": [
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "c0f8a92d-d391-4e57-9ef5-9b8f1237f3a4",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "M",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "6eaa90ac-39f1-4929-a0f2-239b773cf0d3",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "3LB",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "bec229f1-dd54-44f7-b1e9-c975cf15386e",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "4",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "296a1465-614f-4a00-a69f-5f38ee9ffe31",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "230/400-50",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "f9862b4d-934e-4fbd-b67d-0b0aea3cc61f",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "IP54",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "73c571cc-0083-4fb2-98b3-8d1d4c4a85cd",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "CLF",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "c979a882-bd5b-42df-ab5f-d0444b73602d",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "N",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "f6442315-1d87-4a9a-9b4d-592bcec544cf",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "40",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "8c861af5-9320-4530-8b42-93b5b74e2967",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "FD",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "c1ecdaf7-e115-4a85-8a38-c224be09277f",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "40",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "36bc13ba-9190-486d-9d5a-8a44d2789ac3",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "R",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "107349fc-6d54-4b7e-9737-a9886e5c2ab0",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "24",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "52a24767-e88e-4e97-8d24-0f4b5516e29f",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "SD",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "a1bff960-58fa-4274-a7f8-11e7ee972516",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "AA",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "affeadd4-7b37-4ada-97f8-648247c7026c",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "CUS",
          "fromValueUom": "",
          "deletionIndicator": "false"
        },
        {
          "assignmentId": "c537f0fb-a25f-417b-8d22-0b7ac5293df2",
          "charcInternalId": "b3904a2e-f9b4-4dd4-8c5d-45323abd65a5",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "K1",
          "fromValueUom": "",
          "deletionIndicator": "false"
        }
      ]
    },
    {
      "assignmentId": "5d2bb959-dbc6-4f93-9cbc-1f577f1d6276",
      "classType": "300",
      "classInternalId": "c9670132-da25-4f70-a77b-8fadf6d0197f",
      "objectKey": "JB00010153",
      "objectType": "MATERIAL",
      "status": "1",
      "standardClass": "true",
      "assignmentCharacteristicValues": [
        {
          "assignmentId": "5d2bb959-dbc6-4f93-9cbc-1f577f1d6276",
          "charcInternalId": "2bcaae12-f0be-48a8-994f-fcaff939c598",
          "valueCounter": "1",
          "classType": "300",
          "charcValue": "X_F",
          "fromValueUom": "",
          "deletionIndicator": "false"
        }
      ]
    }
  ],
  "classificationClasses": [
    {
      "classInternalId": "75adacc8-9ce2-4cb1-82c3-24b52ef1913f",
      "erpInternalId": "CLASS_M",
      "name": "CLASS_M",
      "type": "300",
      "status": 1,
      "group": "",
      "localClass": "false",
      "descriptionList": [
        {
          "classDescription": "CLASS_M",
          "language": "IT"
        }
      ],
      "characteristicList": [
        {
          "charcInternalId": "c0f8a92d-d391-4e57-9ef5-9b8f1237f3a4",
          "position": 1,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "6eaa90ac-39f1-4929-a0f2-239b773cf0d3",
          "position": 2,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "bec229f1-dd54-44f7-b1e9-c975cf15386e",
          "position": 3,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "296a1465-614f-4a00-a69f-5f38ee9ffe31",
          "position": 4,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "f9862b4d-934e-4fbd-b67d-0b0aea3cc61f",
          "position": 5,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "73c571cc-0083-4fb2-98b3-8d1d4c4a85cd",
          "position": 6,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "c979a882-bd5b-42df-ab5f-d0444b73602d",
          "position": 7,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "f6442315-1d87-4a9a-9b4d-592bcec544cf",
          "position": 8,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "8c861af5-9320-4530-8b42-93b5b74e2967",
          "position": 9,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "586d6290-5552-4b75-93d8-c272977cc72f",
          "position": 10,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "c1ecdaf7-e115-4a85-8a38-c224be09277f",
          "position": 11,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "36bc13ba-9190-486d-9d5a-8a44d2789ac3",
          "position": 12,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "421fd3f3-9c00-44b4-9e31-7caf52d11e6c",
          "position": 13,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "107349fc-6d54-4b7e-9737-a9886e5c2ab0",
          "position": 14,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "52a24767-e88e-4e97-8d24-0f4b5516e29f",
          "position": 15,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "be7c24fe-ba5a-456a-a9cb-44b7c3a1b6fb",
          "position": 16,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "da70c308-6a61-46c1-9f82-6ddd8a1dfd84",
          "position": 17,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "a1bff960-58fa-4274-a7f8-11e7ee972516",
          "position": 18,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "3882f1f7-a8ec-441b-b040-a14b3c563f4b",
          "position": 19,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "e497bfc2-9d45-48f8-bd03-79cdcd501662",
          "position": 20,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "bfca633f-5f0d-4f8f-bc50-5f06a4c83f10",
          "position": 21,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "552936d3-9094-4ea3-900a-6a3b74917438",
          "position": 22,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "6385d590-6111-4e5a-9bb3-f0f8e542e249",
          "position": 23,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "41f00423-3b3d-4cea-81e7-0bcade11647d",
          "position": 24,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "4b455a55-8d00-45ac-a8f3-fbdba0b698e5",
          "position": 25,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "24e26b8a-9dd1-4257-abbd-3a1dba248f00",
          "position": 26,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "02028eab-86ae-4e55-9326-406e5ea41522",
          "position": 27,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "a6714f1f-7a00-44c0-ac16-6601588b9088",
          "position": 28,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "0e74d361-1696-4b8f-b64f-75a03f9dbcfe",
          "position": 29,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "10321ee5-ec5a-4609-bdcf-e03cc87b1742",
          "position": 30,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "59a236e6-340e-4994-b516-41b2db40e74c",
          "position": 31,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "13281edc-2f23-4bfa-9f2d-0c870bcd0375",
          "position": 32,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "0231fec1-4a32-4607-a3f4-8ff1eac44e53",
          "position": 33,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "47299d04-a119-4c29-97f9-8f31053bd2fe",
          "position": 34,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "c5856935-967e-4569-b197-1eb6353777b3",
          "position": 35,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "ab718482-9abe-485a-baab-88194cab3f55",
          "position": 36,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "ec8ecaaa-8a51-4938-a90e-b2fbf9c559be",
          "position": 37,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "affeadd4-7b37-4ada-97f8-648247c7026c",
          "position": 38,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "3027e58d-1047-4c25-882a-5c5a0cb0c37b",
          "position": 39,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "1f5730e9-a897-4449-b649-bf40297cdd3b",
          "position": 40,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "b781ee2d-5de3-4400-bf2a-e509d1723cc5",
          "position": 41,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "9d4223a4-9bbc-41f2-88b8-876cad64fc64",
          "position": 42,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "0039e324-d363-432b-8df2-0435d9a63b54",
          "position": 43,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "8b195163-2077-4983-ad34-60afd9ea4633",
          "position": 44,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "b3904a2e-f9b4-4dd4-8c5d-45323abd65a5",
          "position": 45,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        },
        {
          "charcInternalId": "5c6069f6-fa44-487c-8b74-860dacbfd5fd",
          "position": 46,
          "validityStartDate": null,
          "validityEndDate": null,
          "deletionIndicator": "false"
        }
      ],
      "characteristicDetails": [
        {
          "charcInternalId": "586d6290-5552-4b75-93d8-c272977cc72f",
          "charcExternalId": "TC10202",
          "name": "TC10202",
          "positionNumber": 0,
          "modifiedDateTime": "2025-07-01T14:49:35.651+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "true",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "Gearbox speciality",
              "language": "EN"
            },
            {
              "characteristicDescription": "Gearbox speciality",
              "language": "IT"
            },
            {
              "characteristicDescription": "Gearbox speciality",
              "language": "DE"
            },
            {
              "characteristicDescription": "Gearbox speciality",
              "language": "SV"
            },
            {
              "characteristicDescription": "Gearbox speciality",
              "language": "ES"
            },
            {
              "characteristicDescription": "Gearbox speciality",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "c0f8a92d-d391-4e57-9ef5-9b8f1237f3a4",
          "charcExternalId": "M_S010",
          "name": "M_S010",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:21.991+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "PRODUCT SERIES",
              "language": "EN"
            },
            {
              "characteristicDescription": "GETRIEBETYP",
              "language": "DE"
            },
            {
              "characteristicDescription": "SERIES",
              "language": "FR"
            },
            {
              "characteristicDescription": "SERIE",
              "language": "IT"
            },
            {
              "characteristicDescription": "SERIE",
              "language": "ES"
            },
            {
              "characteristicDescription": "VÄXELTYP",
              "language": "SV"
            },
            {
              "characteristicDescription": "PRODUCT SERIES",
              "language": "ZH"
            },
            {
              "characteristicDescription": "SÉRIE DO PRODUTO",
              "language": "PT"
            },
            {
              "characteristicDescription": "ÜRÜN SERİLERİ",
              "language": "TR"
            }
          ]
        },
        {
          "charcInternalId": "6eaa90ac-39f1-4929-a0f2-239b773cf0d3",
          "charcExternalId": "M_S020",
          "name": "M_S020",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:22.097+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "FRAME SIZE",
              "language": "EN"
            },
            {
              "characteristicDescription": "GRANDEZZA",
              "language": "IT"
            },
            {
              "characteristicDescription": "GETRIEBEBAUGRÖSSE",
              "language": "DE"
            },
            {
              "characteristicDescription": "VÄXELSTORLEK",
              "language": "SV"
            },
            {
              "characteristicDescription": "TAMAÑO",
              "language": "ES"
            },
            {
              "characteristicDescription": "TAILLE",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "bec229f1-dd54-44f7-b1e9-c975cf15386e",
          "charcExternalId": "M_S200",
          "name": "M_S200",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:22.866+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "POLE NUMBER",
              "language": "EN"
            },
            {
              "characteristicDescription": "NUMERO POLI",
              "language": "IT"
            },
            {
              "characteristicDescription": "POLZAHL",
              "language": "DE"
            },
            {
              "characteristicDescription": "POLTAL",
              "language": "SV"
            },
            {
              "characteristicDescription": "NUMERO POLOS",
              "language": "ES"
            },
            {
              "characteristicDescription": "NOMBRE DE POLES",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "296a1465-614f-4a00-a69f-5f38ee9ffe31",
          "charcExternalId": "M_S210",
          "name": "M_S210",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:23.980+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "VOLTAGE-FREQUENCY",
              "language": "EN"
            },
            {
              "characteristicDescription": "TENSIONE-FREQUENZA",
              "language": "IT"
            },
            {
              "characteristicDescription": "SPANNUNG-FREQUENZ",
              "language": "DE"
            },
            {
              "characteristicDescription": "SPÄNNING-FREKVENS",
              "language": "SV"
            },
            {
              "characteristicDescription": "TENSION-FRECUENCIA",
              "language": "ES"
            },
            {
              "characteristicDescription": "TENSION - FREQUENCE",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "f9862b4d-934e-4fbd-b67d-0b0aea3cc61f",
          "charcExternalId": "M_S220",
          "name": "M_S220",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:24.060+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "DEGREE OF PROTECTION",
              "language": "EN"
            },
            {
              "characteristicDescription": "CLASSE DI PROTEZIONE",
              "language": "IT"
            },
            {
              "characteristicDescription": "SCHUTZKLASSE",
              "language": "DE"
            },
            {
              "characteristicDescription": "SKYDDSFORM",
              "language": "SV"
            },
            {
              "characteristicDescription": "CLASE DE PROTECCION",
              "language": "ES"
            },
            {
              "characteristicDescription": "CLASSE DE PROTECTION",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "73c571cc-0083-4fb2-98b3-8d1d4c4a85cd",
          "charcExternalId": "M_S230",
          "name": "M_S230",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:24.138+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "INSULATION CLASS",
              "language": "EN"
            },
            {
              "characteristicDescription": "CLASSE DI ISOLAMENTO",
              "language": "IT"
            },
            {
              "characteristicDescription": "ISOLATIONSKLASSE",
              "language": "DE"
            },
            {
              "characteristicDescription": "ISOLATIONSKLASS",
              "language": "SV"
            },
            {
              "characteristicDescription": "CLASE DE AISLAMIENTO",
              "language": "ES"
            },
            {
              "characteristicDescription": "CLASSE D'ISOLATION",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "f6442315-1d87-4a9a-9b4d-592bcec544cf",
          "charcExternalId": "M_S240",
          "name": "M_S240",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:28.986+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "MOUNTING",
              "language": "EN"
            },
            {
              "characteristicDescription": "FORMA COSTRUTTIVA",
              "language": "IT"
            },
            {
              "characteristicDescription": "BAUFORM",
              "language": "DE"
            },
            {
              "characteristicDescription": "BYGGFORM",
              "language": "SV"
            },
            {
              "characteristicDescription": "FORMA CONSTRUCTIVA",
              "language": "ES"
            },
            {
              "characteristicDescription": "FORME DE CONSTRUCTIO",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "c979a882-bd5b-42df-ab5f-d0444b73602d",
          "charcExternalId": "M_S250",
          "name": "M_S250",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:29.067+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "TERMINAL BOX",
              "language": "EN"
            },
            {
              "characteristicDescription": "POSIZ.MORSETTIERA",
              "language": "IT"
            },
            {
              "characteristicDescription": "KLEMMKASTENLAGE",
              "language": "DE"
            },
            {
              "characteristicDescription": "UTTAGSLÅDANS LÄGE",
              "language": "SV"
            },
            {
              "characteristicDescription": "POSIC. CAJA DE BORNE",
              "language": "ES"
            },
            {
              "characteristicDescription": "POSITION BOITE A BOR",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "8b195163-2077-4983-ad34-60afd9ea4633",
          "charcExternalId": "M_S297",
          "name": "M_S297",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:29.142+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "CON - CONNECTOR",
              "language": "EN"
            },
            {
              "characteristicDescription": "CON - CONNETTORI",
              "language": "IT"
            },
            {
              "characteristicDescription": "CON - ANSCHLUSS",
              "language": "DE"
            },
            {
              "characteristicDescription": "CON - CONNECTOR",
              "language": "SV"
            },
            {
              "characteristicDescription": "CON - CONECTOR",
              "language": "ES"
            },
            {
              "characteristicDescription": "CON - CONNECTEUR",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "8c861af5-9320-4530-8b42-93b5b74e2967",
          "charcExternalId": "M_S300",
          "name": "M_S300",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:29.222+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "BRAKE TYPE",
              "language": "EN"
            },
            {
              "characteristicDescription": "TIPO FRENO",
              "language": "IT"
            },
            {
              "characteristicDescription": "BREMSENTYP",
              "language": "DE"
            },
            {
              "characteristicDescription": "BROMS TYP",
              "language": "SV"
            },
            {
              "characteristicDescription": "TIPO FRENO",
              "language": "ES"
            },
            {
              "characteristicDescription": "TYPE FREIN",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "c1ecdaf7-e115-4a85-8a38-c224be09277f",
          "charcExternalId": "M_S310",
          "name": "M_S310",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:29.304+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "BRAKE TORQUE",
              "language": "EN"
            },
            {
              "characteristicDescription": "COPPIA FRENANTE",
              "language": "IT"
            },
            {
              "characteristicDescription": "BREMSMOMENT",
              "language": "DE"
            },
            {
              "characteristicDescription": "BROMSMOMENT",
              "language": "SV"
            },
            {
              "characteristicDescription": "PAREJA DE FRENO",
              "language": "ES"
            },
            {
              "characteristicDescription": "COUPLE DE FREINAGE",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "36bc13ba-9190-486d-9d5a-8a44d2789ac3",
          "charcExternalId": "M_S320",
          "name": "M_S320",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:29.378+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "BRAKE HAND RELEASE",
              "language": "EN"
            },
            {
              "characteristicDescription": "LEVA DI SBLOCCO",
              "language": "IT"
            },
            {
              "characteristicDescription": "BREMSENHANDLUFTUNG",
              "language": "DE"
            },
            {
              "characteristicDescription": "HANDLYFTDON",
              "language": "SV"
            },
            {
              "characteristicDescription": "PALANCA DE DESBLOQUE",
              "language": "ES"
            },
            {
              "characteristicDescription": "LEVIER DE DEBLOCAGE",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "421fd3f3-9c00-44b4-9e31-7caf52d11e6c",
          "charcExternalId": "M_S330",
          "name": "M_S330",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:29.458+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "RECTIFIER TYPE",
              "language": "EN"
            },
            {
              "characteristicDescription": "TIPO ALIMENTATORE",
              "language": "IT"
            },
            {
              "characteristicDescription": "GLEICHRICHTERTYP",
              "language": "DE"
            },
            {
              "characteristicDescription": "LIKRIKTARE TYP",
              "language": "SV"
            },
            {
              "characteristicDescription": "TIPO ALIMENTACION",
              "language": "ES"
            },
            {
              "characteristicDescription": "TYPE D'ALIMENTATEUR",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "107349fc-6d54-4b7e-9737-a9886e5c2ab0",
          "charcExternalId": "M_S340",
          "name": "M_S340",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:33.992+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "BRAKE SUPPLY-VOLTS",
              "language": "EN"
            },
            {
              "characteristicDescription": "ALIMENTAZIONE FRENO",
              "language": "IT"
            },
            {
              "characteristicDescription": "SPANNUNGS.DER BREMSE",
              "language": "DE"
            },
            {
              "characteristicDescription": "BROMSSPÄNNING",
              "language": "SV"
            },
            {
              "characteristicDescription": "ALIMENTACION FRENO",
              "language": "ES"
            },
            {
              "characteristicDescription": "ALIMENTATION DU FREI",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "52a24767-e88e-4e97-8d24-0f4b5516e29f",
          "charcExternalId": "M_S350",
          "name": "M_S350",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:34.073+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "SEPAR. BRAKE SUPPLY",
              "language": "EN"
            },
            {
              "characteristicDescription": "ALIMENTAZ.FRENO SEP.",
              "language": "IT"
            },
            {
              "characteristicDescription": "FREMDBREMSVERSORGUNG",
              "language": "DE"
            },
            {
              "characteristicDescription": "SEPARAT BROMSMATNING",
              "language": "SV"
            },
            {
              "characteristicDescription": "ALIMENTAC.FRENO SEP.",
              "language": "ES"
            },
            {
              "characteristicDescription": "ALIMENTATION FREIN S",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "be7c24fe-ba5a-456a-a9cb-44b7c3a1b6fb",
          "charcExternalId": "M_S355",
          "name": "M_S355",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:34.149+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "MSW - MICROSWITCH",
              "language": "EN"
            },
            {
              "characteristicDescription": "MSW - MICROSWITCH",
              "language": "IT"
            },
            {
              "characteristicDescription": "MSW - MICROSWITCH",
              "language": "DE"
            },
            {
              "characteristicDescription": "MSW - MICROSWITCH",
              "language": "SV"
            },
            {
              "characteristicDescription": "MSW - MICROSWITCH",
              "language": "ES"
            },
            {
              "characteristicDescription": "MSW - MICROSWITCH",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "b781ee2d-5de3-4400-bf2a-e509d1723cc5",
          "charcExternalId": "M_S385",
          "name": "M_S385",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:34.225+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "SERVICE",
              "language": "EN"
            },
            {
              "characteristicDescription": "SERVIZIO",
              "language": "IT"
            },
            {
              "characteristicDescription": "SERVICE",
              "language": "DE"
            },
            {
              "characteristicDescription": "SERVICE",
              "language": "SV"
            },
            {
              "characteristicDescription": "SERVICIO",
              "language": "ES"
            },
            {
              "characteristicDescription": "SERVICE",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "9d4223a4-9bbc-41f2-88b8-876cad64fc64",
          "charcExternalId": "M_S386",
          "name": "M_S386",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:34.301+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "CCC",
              "language": "EN"
            },
            {
              "characteristicDescription": "CCC",
              "language": "IT"
            },
            {
              "characteristicDescription": "CCC",
              "language": "DE"
            },
            {
              "characteristicDescription": "CCC",
              "language": "SV"
            },
            {
              "characteristicDescription": "CCC",
              "language": "ES"
            },
            {
              "characteristicDescription": "CCC",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "0039e324-d363-432b-8df2-0435d9a63b54",
          "charcExternalId": "M_S391",
          "name": "M_S391",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:34.380+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "IC - CABLES INPUT",
              "language": "EN"
            },
            {
              "characteristicDescription": "IC - INGRESSO CAVI",
              "language": "IT"
            },
            {
              "characteristicDescription": "IC - INPUT KABEL",
              "language": "DE"
            },
            {
              "characteristicDescription": "IC - CABLES INPUT",
              "language": "SV"
            },
            {
              "characteristicDescription": "IC - ENTRADA DE CABLE",
              "language": "ES"
            },
            {
              "characteristicDescription": "IC - CÂBLE D'ENTRÉE",
              "language": "FR"
            },
            {
              "characteristicDescription": "IC-KABLOLARIN GİRİŞİ",
              "language": "TR"
            }
          ]
        },
        {
          "charcInternalId": "b3904a2e-f9b4-4dd4-8c5d-45323abd65a5",
          "charcExternalId": "M_S392",
          "name": "M_S392",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:34.468+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "false",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "K1 -THERMISTOR KTY",
              "language": "EN"
            },
            {
              "characteristicDescription": "K1 - TERMISTORI KTY",
              "language": "IT"
            },
            {
              "characteristicDescription": "K1 - THERMISTOR KTY",
              "language": "DE"
            },
            {
              "characteristicDescription": "K1 -THERMISTOR KTY",
              "language": "SV"
            },
            {
              "characteristicDescription": "K1 - TERMISTORE KTY",
              "language": "ES"
            },
            {
              "characteristicDescription": "K1 - THERMISTANC KTY",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "da70c308-6a61-46c1-9f82-6ddd8a1dfd84",
          "charcExternalId": "M_S4001",
          "name": "M_S4001",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:34.541+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "ANTI RUN-BACK LEFT.",
              "language": "EN"
            },
            {
              "characteristicDescription": "AL - ANTIRETRO SINISTRO",
              "language": "IT"
            },
            {
              "characteristicDescription": "RUCKLAUFSPERRE LINKS",
              "language": "DE"
            },
            {
              "characteristicDescription": "BACKSPÄRR,ROT.VÄNST.",
              "language": "SV"
            },
            {
              "characteristicDescription": "ANTIRRETORNO IZQUIER",
              "language": "ES"
            },
            {
              "characteristicDescription": "ANTI RETOUR GAUCHE",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "a1bff960-58fa-4274-a7f8-11e7ee972516",
          "charcExternalId": "M_S40567",
          "name": "M_S40567",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:34.616+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "BRAKE RELEASE",
              "language": "EN"
            },
            {
              "characteristicDescription": "RILASCIO DEL FRENO",
              "language": "IT"
            },
            {
              "characteristicDescription": "BREMSENTRIEGELUNG",
              "language": "DE"
            },
            {
              "characteristicDescription": "BROMSSLÄPPNING",
              "language": "SV"
            },
            {
              "characteristicDescription": "LIBERACIÓN DEL FRENO",
              "language": "ES"
            },
            {
              "characteristicDescription": "DEBLOCAGE DES FREIN",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "3882f1f7-a8ec-441b-b040-a14b3c563f4b",
          "charcExternalId": "M_S410",
          "name": "M_S410",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:38.997+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "3 BIMETALLIC THERM.",
              "language": "EN"
            },
            {
              "characteristicDescription": "D3 - 3 SONDE TERMICHE",
              "language": "IT"
            },
            {
              "characteristicDescription": "MIT 3 BIMETALLEN(D3)",
              "language": "DE"
            },
            {
              "characteristicDescription": "3 TERMOSTATER",
              "language": "SV"
            },
            {
              "characteristicDescription": "3 SONDAS TERMICAS",
              "language": "ES"
            },
            {
              "characteristicDescription": "3 SONDES THERMIQUES",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "e497bfc2-9d45-48f8-bd03-79cdcd501662",
          "charcExternalId": "M_S420",
          "name": "M_S420",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:39.073+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "3 THERMISTORS",
              "language": "EN"
            },
            {
              "characteristicDescription": "E3 - 3 TERMISTORI",
              "language": "IT"
            },
            {
              "characteristicDescription": "MIT 3 THERMISTOREN",
              "language": "DE"
            },
            {
              "characteristicDescription": "3 TERMISTORER",
              "language": "SV"
            },
            {
              "characteristicDescription": "3 TERMISTORES",
              "language": "ES"
            },
            {
              "characteristicDescription": "3 THERMISTANCES",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "bfca633f-5f0d-4f8f-bc50-5f06a4c83f10",
          "charcExternalId": "M_S421",
          "name": "M_S421",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:39.147+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "6 THERMISTORS",
              "language": "EN"
            },
            {
              "characteristicDescription": "E6 - 6 TERMISTORI",
              "language": "IT"
            },
            {
              "characteristicDescription": "MIT 6 THERMISTOREN",
              "language": "DE"
            },
            {
              "characteristicDescription": "6 TERMISTORER",
              "language": "SV"
            },
            {
              "characteristicDescription": "6 TERMISTORES",
              "language": "ES"
            },
            {
              "characteristicDescription": "6 THERMISTANCES",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "552936d3-9094-4ea3-900a-6a3b74917438",
          "charcExternalId": "M_S430",
          "name": "M_S430",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:39.222+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "FLYWHEEL/SOFT START",
              "language": "EN"
            },
            {
              "characteristicDescription": "F1 - VOLANO X AVV.PROGR.",
              "language": "IT"
            },
            {
              "characteristicDescription": "SCHWUNGR.Z.SANF.ANF.",
              "language": "DE"
            },
            {
              "characteristicDescription": "SVÄNGHJUL",
              "language": "SV"
            },
            {
              "characteristicDescription": "VOLANTE X ARRANQUE P",
              "language": "ES"
            },
            {
              "characteristicDescription": "VOLANT POUR DEMARRAG",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "6385d590-6111-4e5a-9bb3-f0f8e542e249",
          "charcExternalId": "M_S440",
          "name": "M_S440",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:39.298+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "HEATERS",
              "language": "EN"
            },
            {
              "characteristicDescription": "H1 - RISCALD.ANTICONDENSA",
              "language": "IT"
            },
            {
              "characteristicDescription": "STILLSTANDSHEIZUNG",
              "language": "DE"
            },
            {
              "characteristicDescription": "VÄRMEBAND",
              "language": "SV"
            },
            {
              "characteristicDescription": "RECALENT. ANTICONDEN",
              "language": "ES"
            },
            {
              "characteristicDescription": "RECHAUFFAGE. ANTICON",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "41f00423-3b3d-4cea-81e7-0bcade11647d",
          "charcExternalId": "M_S441",
          "name": "M_S441",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:39.373+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "HEATERS 115V",
              "language": "EN"
            },
            {
              "characteristicDescription": "NH1 - RISCALD.ANTICON.115V",
              "language": "IT"
            },
            {
              "characteristicDescription": "HEIZUNG 115 V",
              "language": "DE"
            },
            {
              "characteristicDescription": "VÄRMEBAND 115V",
              "language": "SV"
            },
            {
              "characteristicDescription": "CALENTADOR 115V",
              "language": "ES"
            },
            {
              "characteristicDescription": "NH1 - RECHAUFFAGE. ANTICON",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "4b455a55-8d00-45ac-a8f3-fbdba0b698e5",
          "charcExternalId": "M_S445503",
          "name": "M_S445503",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:39.453+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "ENCODER",
              "language": "EN"
            },
            {
              "characteristicDescription": "ENCODER",
              "language": "IT"
            },
            {
              "characteristicDescription": "ENCODER",
              "language": "DE"
            },
            {
              "characteristicDescription": "ENCODER",
              "language": "SV"
            },
            {
              "characteristicDescription": "ENCODER",
              "language": "ES"
            },
            {
              "characteristicDescription": "ENCODER",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "24e26b8a-9dd1-4257-abbd-3a1dba248f00",
          "charcExternalId": "M_S450",
          "name": "M_S450",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:39.527+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "9 STUD TERM. BOARD",
              "language": "EN"
            },
            {
              "characteristicDescription": "M3 - MORSETT. 9 MORSETTI",
              "language": "IT"
            },
            {
              "characteristicDescription": "KLEMMB.M.9 ANSCHLÜS.",
              "language": "DE"
            },
            {
              "characteristicDescription": "9-POLIG KOPPL.PLINT",
              "language": "SV"
            },
            {
              "characteristicDescription": "CAJA DE BORNES 9 BOR",
              "language": "ES"
            },
            {
              "characteristicDescription": "BOITE A BORNES A 9 P",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "02028eab-86ae-4e55-9326-406e5ea41522",
          "charcExternalId": "M_S451",
          "name": "M_S451",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:44.004+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "BALANCE CLASS R",
              "language": "EN"
            },
            {
              "characteristicDescription": "RV - BILANCIAMEN.ROTORE R",
              "language": "IT"
            },
            {
              "characteristicDescription": "LÄUFER IN R AUSGEW.",
              "language": "DE"
            },
            {
              "characteristicDescription": "ROTOR BALANCING R",
              "language": "SV"
            },
            {
              "characteristicDescription": "BALANCE ROTOR R",
              "language": "ES"
            },
            {
              "characteristicDescription": "EQUILIBRAGE ROTOR CL",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "a6714f1f-7a00-44c0-ac16-6601588b9088",
          "charcExternalId": "M_S455",
          "name": "M_S455",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:44.081+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "CAPACITIVE FILTER",
              "language": "EN"
            },
            {
              "characteristicDescription": "CF - FILTRO CAPACITIVO",
              "language": "IT"
            },
            {
              "characteristicDescription": "KAPAZITIVER FILTER",
              "language": "DE"
            },
            {
              "characteristicDescription": "CAPACITIVE FILTER",
              "language": "SV"
            },
            {
              "characteristicDescription": "FILTRO DE CAPACIDAD",
              "language": "ES"
            },
            {
              "characteristicDescription": "FILTRE CAPACITIF",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "0e74d361-1696-4b8f-b64f-75a03f9dbcfe",
          "charcExternalId": "M_S460",
          "name": "M_S460",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:44.159+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "DOUBLE SHAFT EXTENS.",
              "language": "EN"
            },
            {
              "characteristicDescription": "PS - DOPPIA ESTREM.D'ALB.",
              "language": "IT"
            },
            {
              "characteristicDescription": "DOPPELWELLENENDE",
              "language": "DE"
            },
            {
              "characteristicDescription": "EXTRA UTGÅENDE AXEL",
              "language": "SV"
            },
            {
              "characteristicDescription": "DOBLE EXTREM. DE EJE",
              "language": "ES"
            },
            {
              "characteristicDescription": "DOUBLE EXTREMITE D'A",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "10321ee5-ec5a-4609-bdcf-e03cc87b1742",
          "charcExternalId": "M_S470",
          "name": "M_S470",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:44.240+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "DRIP COVER",
              "language": "EN"
            },
            {
              "characteristicDescription": "RC - TETTUCC. PARAPIOGGIA",
              "language": "IT"
            },
            {
              "characteristicDescription": "REGENSCHUTZDACH",
              "language": "DE"
            },
            {
              "characteristicDescription": "SKYDDSTAK",
              "language": "SV"
            },
            {
              "characteristicDescription": "PROTECCION LLUVIA",
              "language": "ES"
            },
            {
              "characteristicDescription": "CAPOT TOLE PARAPLUIE",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "59a236e6-340e-4994-b516-41b2db40e74c",
          "charcExternalId": "M_S475",
          "name": "M_S475",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:44.316+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "TEXTILE CANOPY",
              "language": "EN"
            },
            {
              "characteristicDescription": "TC - TETTUCCIO TESSILE",
              "language": "IT"
            },
            {
              "characteristicDescription": "TEXTILSCHUTZDACH",
              "language": "DE"
            },
            {
              "characteristicDescription": "SKYDDSTAK, TEXTIL",
              "language": "SV"
            },
            {
              "characteristicDescription": "PROTECCION TEXTIL",
              "language": "ES"
            },
            {
              "characteristicDescription": "CAPOT TEXTILE (ANTI-",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "13281edc-2f23-4bfa-9f2d-0c870bcd0375",
          "charcExternalId": "M_S4801",
          "name": "M_S4801",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:44.390+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "FORCED VENTILATION",
              "language": "EN"
            },
            {
              "characteristicDescription": "U1 - VENTILAZIONE FORZATA",
              "language": "IT"
            },
            {
              "characteristicDescription": "FREMDLÜFTER",
              "language": "DE"
            },
            {
              "characteristicDescription": "SEPARAT KYLFLÄKT",
              "language": "SV"
            },
            {
              "characteristicDescription": "VENTILACION FORZADA",
              "language": "ES"
            },
            {
              "characteristicDescription": "VENTILATION FORCEE",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "5c6069f6-fa44-487c-8b74-860dacbfd5fd",
          "charcExternalId": "M_S486",
          "name": "M_S486",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:44.470+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "EMC FILTER-CLASS B",
              "language": "EN"
            },
            {
              "characteristicDescription": "BF - FILTRO IN CLASSE B",
              "language": "IT"
            },
            {
              "characteristicDescription": "FILTER DER KLASSE B",
              "language": "DE"
            },
            {
              "characteristicDescription": "FILTER IN CLASS B",
              "language": "SV"
            },
            {
              "characteristicDescription": "FILTRO DE CLASE B",
              "language": "ES"
            },
            {
              "characteristicDescription": "FILTRE EN CLASSE B",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "47299d04-a119-4c29-97f9-8f31053bd2fe",
          "charcExternalId": "M_S487",
          "name": "M_S487",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:44.543+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "BRAKE CONTROL.MODULE",
              "language": "EN"
            },
            {
              "characteristicDescription": "BU - MODULO CONTROL.FRENO",
              "language": "IT"
            },
            {
              "characteristicDescription": "BREMSKONTROLLEINHEIT",
              "language": "DE"
            },
            {
              "characteristicDescription": "BROMSKONTROLL MODUL",
              "language": "SV"
            },
            {
              "characteristicDescription": "MODULO CONTROL.FRENO",
              "language": "ES"
            },
            {
              "characteristicDescription": "MODULE CONTROL FREIN",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "0231fec1-4a32-4607-a3f4-8ff1eac44e53",
          "charcExternalId": "M_S488",
          "name": "M_S488",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:44.616+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "BRAKE RESIST.MODULE",
              "language": "EN"
            },
            {
              "characteristicDescription": "BR - MODULO RESIST.FRENAT",
              "language": "IT"
            },
            {
              "characteristicDescription": "BR - FORMBLATT  AUSDAUER",
              "language": "DE"
            },
            {
              "characteristicDescription": "BROMSMOTSTÅND",
              "language": "SV"
            },
            {
              "characteristicDescription": "MODULO RESIST.FREN.",
              "language": "ES"
            },
            {
              "characteristicDescription": "MODULE RESIST.FREIN",
              "language": "FR"
            },
            {
              "characteristicDescription": "BRAKE RESIST.MODULE",
              "language": "ZH"
            },
            {
              "characteristicDescription": "Modulo resist freio",
              "language": "PT"
            },
            {
              "characteristicDescription": "FREN DİRENÇ MODÜLÜ",
              "language": "TR"
            }
          ]
        },
        {
          "charcInternalId": "c5856935-967e-4569-b197-1eb6353777b3",
          "charcExternalId": "M_S490",
          "name": "M_S490",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:44.723+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "50 HZ RATED OUTPUT",
              "language": "EN"
            },
            {
              "characteristicDescription": "PN - POTENZA A 50HZ",
              "language": "IT"
            },
            {
              "characteristicDescription": "LEISTUNG AUF 50 HZ",
              "language": "DE"
            },
            {
              "characteristicDescription": "SPÄNNING VID 50HZ",
              "language": "SV"
            },
            {
              "characteristicDescription": "POTENCIA A 50HZ",
              "language": "ES"
            },
            {
              "characteristicDescription": "PUISSANCE A 50HZ",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "ab718482-9abe-485a-baab-88194cab3f55",
          "charcExternalId": "M_S491",
          "name": "M_S491",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:44.799+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "60 HZ RATED OUTPUT",
              "language": "EN"
            },
            {
              "characteristicDescription": "PT - RITARGHETTARE",
              "language": "IT"
            },
            {
              "characteristicDescription": "60HZ AUF TYP.SCHIELD",
              "language": "DE"
            },
            {
              "characteristicDescription": "60HZ PÅ TYPSKYLT",
              "language": "SV"
            },
            {
              "characteristicDescription": "RETARJETAR",
              "language": "ES"
            },
            {
              "characteristicDescription": "REPLAQUER",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "ec8ecaaa-8a51-4938-a90e-b2fbf9c559be",
          "charcExternalId": "M_S495",
          "name": "M_S495",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:44.876+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "TROPICALISATION",
              "language": "EN"
            },
            {
              "characteristicDescription": "TP - TROPICALIZZATO",
              "language": "IT"
            },
            {
              "characteristicDescription": "TROPENGESCHÜTZT",
              "language": "DE"
            },
            {
              "characteristicDescription": "TROPIKISOLATION",
              "language": "SV"
            },
            {
              "characteristicDescription": "TROPICALIZADO",
              "language": "ES"
            },
            {
              "characteristicDescription": "TROPICALISE",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "affeadd4-7b37-4ada-97f8-648247c7026c",
          "charcExternalId": "M_S500",
          "name": "M_S500",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:44.953+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "CCSAUS CERTIFIED",
              "language": "EN"
            },
            {
              "characteristicDescription": "CUS - NORME CCSAUS",
              "language": "IT"
            },
            {
              "characteristicDescription": "CCSAUS AUSFÜHRUNG",
              "language": "DE"
            },
            {
              "characteristicDescription": "CCSAUS CERTIFIERAD",
              "language": "SV"
            },
            {
              "characteristicDescription": "NORMA CCSAUS",
              "language": "ES"
            },
            {
              "characteristicDescription": "CCSAUS NORMES",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "1f5730e9-a897-4449-b649-bf40297cdd3b",
          "charcExternalId": "M_S870",
          "name": "M_S870",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:49.008+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "CC - INSPECTION CERTIFIC.",
              "language": "EN"
            },
            {
              "characteristicDescription": "CC - CERTIF.DI COLLAUDO",
              "language": "IT"
            },
            {
              "characteristicDescription": "PRÜFBERICHT",
              "language": "DE"
            },
            {
              "characteristicDescription": "TESTRAPPORT",
              "language": "SV"
            },
            {
              "characteristicDescription": "CERTIF. DE VERIFICAC",
              "language": "ES"
            },
            {
              "characteristicDescription": "CERTIFICAT DE CONTRÔ",
              "language": "FR"
            }
          ]
        },
        {
          "charcInternalId": "3027e58d-1047-4c25-882a-5c5a0cb0c37b",
          "charcExternalId": "M_S877",
          "name": "M_S877",
          "positionNumber": 0,
          "modifiedDateTime": "2025-09-12T08:44:49.090+00:00",
          "status": "1",
          "dataType": "CHAR",
          "length": 30,
          "decimalPlaces": 0,
          "exponentValue": 0,
          "exponentFormat": 0,
          "isCaseSensitive": "false",
          "isRequired": "false",
          "isMultipleValuesAllowed": "false",
          "isValueIntervalAllowed": "false",
          "isAdditionalValueAllowed": "true",
          "isNegativeValueAllowed": "false",
          "characteristicDescriptionList": [
            {
              "characteristicDescription": "CONFORM. CERTIFIC. MOT.",
              "language": "EN"
            },
            {
              "characteristicDescription": "ACM - ATTEST. DI CONF.MOTORI",
              "language": "IT"
            },
            {
              "characteristicDescription": "KONF.BESCHEIN. MOT.",
              "language": "DE"
            },
            {
              "characteristicDescription": "CERTIFIKAT MOTORER",
              "language": "SV"
            },
            {
              "characteristicDescription": "CERTIF. DE CONFORM.MOT.",
              "language": "ES"
            },
            {
              "characteristicDescription": "ATTEST. DE CONFO. MOT.",
              "language": "FR"
            }
          ]
        }
      ]
    }
  ]
}
```

## Applications/Use Cases

- Conoscere le caratteristiche di classificazione di un determinato materiale

La valorizzazione di ciascuna caratteristica è da “costruire” mediante gli oggetti ritornati dalla API con questa logica:

- all’oggetto _classificationClasses / characteristicDetails_ sono elencati tutte le caratteristiche presenti nella classificazione. Rilevante è la proprietà _charcInternalId_, id interno a DM che identifica il parametro
- all’oggetto _classificationAssignmentHeaders / assignmentCharacteristicValues_ è possibile recuperare il valore associato ad ogni caratteristica mediante il _charcInternalId_ recuperato in precedenza

# 3.6 Shop Floor Controls

> Verb: GET

## Endpoint

<aside>
🌐

{baseUrl}/sfc/v1/sfcdetail?plant={plant}&sfc={sfc}

</aside>

## Parameters

- plant → codice identificativo del plant
- sfc → codice identificativo del seriale

## Response Example

```json
{
  "sfc": "EVO254000015",
  "material": {
    "material": "XEA802720007",
    "version": "ERP001",
    "description": "C 12 2 F 11.9 S05 B5"
  },
  "bom": {
    "bom": "12732532-XEA802720007-1-1",
    "version": "ERP001",
    "type": "SHOPORDERBOM"
  },
  "routing": {
    "routing": "12732532",
    "version": "ERP001",
    "type": "SHOPORDER_SPECIFIC"
  },
  "status": {
    "code": "403",
    "description": "ACTIVE"
  },
  "quantity": 1.0,
  "defaultBatchId": null,
  "steps": [
    {
      "stepId": "0010",
      "stepSequence": 10,
      "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
      "operation": {
        "operation": "12732532-0-0010",
        "version": "ERP001",
        "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID."
      },
      "stepRouting": {
        "routing": "12732532",
        "version": "ERP001",
        "type": "SHOPORDER_SPECIFIC"
      },
      "resource": "496",
      "quantityDone": 0.0,
      "quantityScrapped": 0.0,
      "quantityRejected": 0.0,
      "quantityInQueue": 0.0,
      "quantityInWork": 1.0,
      "quantityCompletePending": 0.0,
      "stepDone": false,
      "plannedWorkCenter": "496",
      "operationScheduledStartDate": "2025-09-16T22:00:00Z",
      "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
    },
    {
      "stepId": "0030",
      "stepSequence": 20,
      "description": "CHIUSURA + COLLAUDO",
      "operation": {
        "operation": "12732532-0-0030",
        "version": "ERP001",
        "description": "CHIUSURA + COLLAUDO"
      },
      "stepRouting": {
        "routing": "12732532",
        "version": "ERP001",
        "type": "SHOPORDER_SPECIFIC"
      },
      "resource": null,
      "quantityDone": 0.0,
      "quantityScrapped": 0.0,
      "quantityRejected": 0.0,
      "quantityInQueue": 0.0,
      "quantityInWork": 0,
      "quantityCompletePending": 0.0,
      "stepDone": false,
      "plannedWorkCenter": "S21",
      "operationScheduledStartDate": "2025-09-16T22:00:00Z",
      "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
    }
  ],
  "order": {
    "orderPlannedStartDateTime_Z": "2025-09-11T22:00:00Z",
    "orderPlannedStartDateTime": "2025-09-11T22:00:00.000+00:00",
    "orderPlannedCompleteDateTime_Z": "2025-09-17T22:00:00Z",
    "orderPlannedCompleteDateTime": "2025-09-17T22:00:00.000+00:00",
    "order": "12732532",
    "type": "PRODUCTION",
    "batchNumber": "",
    "orderStatus": "RELEASED",
    "orderCategory": "PRODUCTION_ORDER"
  }
}
```

## Applications/Use Cases

- Conoscere i dettagli di un determinato seriale
- Sapere l’avanzamento di un seriale nelle varie fasi di un ciclo di produzione attraverso i dettagli ritornati per ogni fase
  - quantityInQueue = 1 → seriale in coda ad all’operazione, in attesa di avvio lavorazione
  - quantityInWork = 1 → seriale in lavorazione nell’operazione, in attesa di completamento
  - quantityDone = 1 → seriale completato sull’operazione e dichiarato buono
  - quantityScrapped = 1 → seriale scartato sull’operazione e dichiarato scarto
  - stepDone = true → l’operazione è stata completata per il seriale in questione (buono o scarto)

# 3.7 Workcenters

> Verb: GET

## Endpoint

<aside>
🌐

{baseUrl}/workcenters?plant={plant}

</aside>

## Parameters

- plant → codice identificativo del plant

## Response Example

```json
[
    {
        "plant": "EVO",
        "workCenter": "496",
        "description": "STGL C32 -->C11",
        "isErp": true,
        "status": "ENABLED",
        "userAssignments": [],
        "canBeReleasedTo": true,
        "category": "NONE",
        "minNumberPeople": 0,
        "maxNumberPeople": 0,
        "productionSupplyArea": null,
        "createdOn": "2025-07-04T12:35:07Z",
        "modifiedOn": "2025-07-04T12:35:08Z",
        "members": [
            {
                "workCenter": null,
                "resource": {
                    "plant": "EVO",
                    "resource": "496"
                },
                "schedulingRelevant": false,
                "monitoringRelevant": false,
                "defaultMember": false
            }
        ],
        "oeeMembers": [],
        "customValues": []
    },
    ...
]

```

# 4.3 SFC Start (start operation)

> Verb: POST

## Endpoint

<aside>
🌐

{baseUrl}/sfc/v1/sfcs/start

</aside>

## Parameters

- plant → codice identificativo del plant
- operation → codice identificativo dell’operazione in avvio
- resource → centro di lavoro dove si sta avviando l’operazione
- quantity → quantità del seriale in avvio (sempre 1 nel caso della produzione assembly)
- sfcs → id del seriale in avviamento. È possibile indicare una serie di id nell’array per effettuare massivamente l’operazione di start su tutti i seriali specificati.

```json
{
  "plant": "EVO",
  "operation": "12732532-0-0030",
  "resource": "496",
  "quantity": "1",
  "sfcs": ["EVO254000015"]
}
```

## Response Example

```json
{
  "sfcs": ["EVO254000015"]
}
```

## Applications/Use Cases

- Avviare la lavorazione di uno o più seriali su una specifica operazione di un centro di lavoro

## Errors & Limitations

Il/i seriali che si desidera avviare devono essere “in coda” alla specifica operazione. Nel caso in cui si richiedesse l’avvio di un seriale che non si trovi in questa condizione, verranno ritornati errori del genere (400 bad request).

```json
{
  "error": {
    "message": "SFC EVO254000015 is not in the queue at operation 12732532-0-0010 instead it is in work at operation 12732532-0-0010. (Message 13961)",
    "causeMessage": null,
    "code": "13961",
    "correlationId": "a23663fe-e893-4f56-9c3f-70287bf314dc"
  }
}
```

Stesso errore viene ritornato se si cerca di avviare un seriale senza aver completato le fasi precedenti del ciclo.

# 4.4 SFC Complete (end operation)

> Verb: POST

## Endpoint

<aside>
🌐

{baseUrl}/sfc/v1/sfcs/complete

</aside>

## Parameters

- plant → codice identificativo del plant
- operation → codice identificativo dell’operazione in completamento
- resource → centro di lavoro dove si sta completando l’operazione
- quantity → quantità del seriale (sempre 1 nel caso della produzione assembly)
- sfcs → id del seriale in completamento. È possibile indicare una serie di id nell’array per effettuare massivamente l’operazione di completamento su tutti i seriali specificati.

```json
{
  "plant": "EVO",
  "operation": "12732532-0-0010",
  "resource": "496",
  "quantity": "1",
  "sfcs": ["EVO254000015"]
}
```

## Response Example

```json
{
  "success": true,
  "sfcs": ["EVO254000015"],
  "queuedOperations": ["12732532-0-0030"],
  "scrapped": false,
  "buyoffs": null,
  "doneStatuses": null
}
```

## Applications/Use Cases

- Completare la lavorazione di uno o più seriali su una specifica operazione di un centro di lavoro

## Errors & Limitations

Il/i seriali che si desidera completare devono essere “in lavorazione” alla specifica operazione. Nel caso in cui si richiedesse l’avvio di un seriale che non si trovi in questa condizione, verranno ritornati errori del genere (400 bad request).

```json
{
  "error": {
    "message": "SFC EVO254000015 isn't in work at operation 12732532-0-0010.",
    "causeMessage": null,
    "code": "15441",
    "correlationId": "2d31c1ec-0f62-4e61-a5e7-6e991eff6902"
  }
}
```

# 4.5 SFC Sign Off (pause operation)

> Verb: POST

## Endpoint

<aside>
🌐

{baseUrl}/sfc/v1/sfcs/signoff

</aside>

## Parameters

- plant → codice identificativo del plant
- operation → codice identificativo dell’operazione da mettere in pausa
- resource → centro di lavoro dove si sta lavorando
- sfcs → id del seriale da mettere in pausa. È possibile indicare una serie di id nell’array per effettuare massivamente l’operazione di pausa su tutti i seriali specificati.

## Response Example

```json
{
  "plant": "EVO",
  "sfcs": ["EVO254000015"],
  "operation": "12732532-0-0010",
  "resource": "496"
}
```

## Applications/Use Cases

- Mettere in pausa la lavorazione di uno o più seriali su una specifica operazione di un centro di lavoro

## Errors & Limitations

Il/i seriali che si desidera mettere in pausa devono essere “in lavorazione” alla specifica operazione. Nel caso in cui si richiedesse la pausa di un seriale che non si trovi in questa condizione, verranno ritornati errori del genere (400 bad request).

```json
{
  "error": {
    "message": "SFC EVO254000015 is not currently in work at any operation.",
    "causeMessage": null,
    "code": "13900",
    "correlationId": "9cef8e33-dfa0-4f2c-8b0f-76f8bfacbd24"
  }
}
```

# 4.6.1 Start Labor

> Verb: POST

## Endpoint

<aside>
🌐

{baseUrl}/timetracking/v1/indirect-labor/start

</aside>

## Parameters

- plant → codice identificativo del plant
- workcenter → centro di lavoro dove si sta lavorando
- resource → centro di lavoro dove si sta lavorando (su DM ogni workcenter avrà una sola risorsa figlio)
- userId → identificativo dell’operatore (badge)
- time (optional) → data/ora UTC dell’inizio movimento in formato ISO-8601 (es. 2022-03-30T10:08:07.304Z) Se non specificato viene assunto now come momento di aggrgazione
- activity → componente sulla quale consuntivare i tempi uomo (da definire la nomenclatura)

```json
{
  "plant": "EVO",
  "userId": "9999",
  "resource": "496",
  "workCenter": "496",
  "activity": "Production"
}
```

## Response Example

```json
{
  "id": "27bb8bdd-7fe3-4767-b91f-6781ed4e9a8b",
  "type": "INDIRECT_LABOR",
  "userId": "aimar@bonfiglioli.com",
  "plant": "EVO",
  "startTime": "2025-09-15T09:02:47Z",
  "endTime": null,
  "resource": "496",
  "workCenter": "496",
  "operation": null,
  "operationVersion": null,
  "stepId": null,
  "shopOrder": null,
  "sfc": null,
  "status": "NEW",
  "routing": null,
  "routingVersion": null,
  "routingType": null,
  "standardValue": null,
  "activity": "Production",
  "effectiveDuration": null,
  "numberOfOperators": 1,
  "adjustedDuration": null,
  "reportedDuration": null,
  "integrationStatus": null,
  "history": [],
  "effectiveLaborTime": null,
  "duration": null,
  "adjustedLaborTime": null
}
```

## Applications/Use Cases

- Effettuare l’aggrazione/labor on di uno specifico operatore su un centro di lavoro

## Errors & Limitations

Nel caso in cui l’operatore fosse già registrato sul centro di lavoro (o su un altro centro di lavoro) viene segnalato che esiste già un movimento aperto per l’employee con un errore 400 Bad Request.

```json
{
  "message": "User aimar@bonfiglioli.com is already tracking indirect labor.",
  "code": "timeTracking.alreadyStartedIndirect",
  "causeMessage": null,
  "correlationId": "64ba8700-a4ba-4a7e-b65c-9545bbe1dea0"
}
```

Sarà necessario esseguire il labor off per l’operatore (con relativa API) per riuscire a far partire il nuovo movimento.

# 4.6.2 Stop Labor

> Verb: POST

## Endpoint

<aside>
🌐

{baseUrl}/timetracking/v1/indirect-labor/stop

</aside>

## Parameters

- plant → codice identificativo del plant
- workcenter → centro di lavoro dove si sta lavorando
- resource → centro di lavoro dove si sta lavorando (su DM ogni workcenter avrà una sola risorsa figlio)
- userId → identificativo dell’operatore (capire se badge o user id)
- time (optional) → data/ora UTC della fine movimento in formato ISO-8601 (es. 2022-03-30T10:08:07.304Z). Se non specificato viene assunto now come momento di dis-aggrgazione
- activity → componente sulla quale consuntivare i tempi uomo (da definire la nomenclatura)

```json
{
  "plant": "EVO",
  "userId": "9999",
  "resource": "496",
  "workCenter": "496",
  "activity": "Production"
}
```

## Response Example

```json
{
  "id": "27bb8bdd-7fe3-4767-b91f-6781ed4e9a8b",
  "type": "INDIRECT_LABOR",
  "userId": "aimar@bonfiglioli.com",
  "plant": "EVO",
  "startTime": "2025-09-15T09:02:47Z",
  "endTime": "2025-09-15T09:08:10Z",
  "resource": "496",
  "workCenter": "496",
  "operation": null,
  "operationVersion": null,
  "stepId": null,
  "shopOrder": null,
  "sfc": null,
  "status": "NEW",
  "routing": null,
  "routingVersion": null,
  "routingType": null,
  "standardValue": null,
  "activity": "Production",
  "effectiveDuration": null,
  "numberOfOperators": 1,
  "adjustedDuration": null,
  "reportedDuration": null,
  "integrationStatus": null,
  "history": [],
  "effectiveLaborTime": null,
  "duration": "PT5M23S",
  "adjustedLaborTime": null
}
```

## Applications/Use Cases

- Effettuare la dis-aggrazione/labor off di uno specifico operatore su un centro di lavoro

## Errors & Limitations

Nel caso in cui l’operatore non fosse già registrato sul centro di lavoro viene segnalato che non esiste un movimento aperto per l’employee con un errore 400 Bad Request.

```json
{
  "message": "User aimar@bonfiglioli.com isn’t tracking indirect labor.",
  "code": "timeTracking.notStartedIndirect",
  "causeMessage": null,
  "correlationId": "cedd05df-a002-457c-a25f-7b8540b59819"
}
```

# **4.7.1 Parameters for a serial**

> Verb: GET

## Endpoint

<aside>
🌐

{baseUrl}/datacollection/v1/sfc/groups?plant=<plant>&workCenter=<workcenter>&resource=<resource>&sfc=<sfc>&operation=<operation>

</aside>

## Parameters

- plant → codice identificativo del plant
- workcenter → centro di lavoro
- resource → centro di lavoro
- sfc → id del seriale
- operation → codice identificativo dell’operazione in corso di lavorazione sul centro di lavoro

## Response Example

```json
[
  {
    "group": {
      "plant": "EVO",
      "group": "DC_ASSEMBLY_TEST",
      "version": "A",
      "current": true,
      "status": "RELEASABLE",
      "passFailGroup": false,
      "description": "Data Collection for testing of Assembly ",
      "allowMultipleCollection": false,
      "parameters": [
        {
          "parameterName": "FIXING",
          "description": "Fixing Force",
          "sequence": 10,
          "dcParameterType": "NUMBER",
          "dcParameterStatus": "ENABLED",
          "parameterPrompt": "",
          "overrideMinMax": false,
          "minValue": 10.0,
          "maxValue": 60.0,
          "unitOfMeasure": "N",
          "requiredDataEntries": 0,
          "targetValue": 35.0,
          "autoLogNc": false
        },
        {
          "parameterName": "CHILD_SERIAL",
          "description": "Child Serial",
          "sequence": 20,
          "dcParameterType": "TEXT",
          "dcParameterStatus": "ENABLED",
          "parameterPrompt": "",
          "overrideMinMax": false,
          "unitOfMeasure": "",
          "requiredDataEntries": 0,
          "autoLogNc": false
        },
        {
          "parameterName": "CHECKED",
          "description": "Serial Checked",
          "sequence": 30,
          "dcParameterType": "BOOLEAN",
          "dcParameterStatus": "ENABLED",
          "parameterPrompt": "",
          "overrideMinMax": false,
          "unitOfMeasure": "",
          "requiredDataEntries": 1,
          "trueValueName": "YES",
          "falseValueName": "NO",
          "defaultValueName": "FALSE_VALUE",
          "autoLogNc": false
        },
        {
          "parameterName": "COMPONENT_BATCH",
          "description": "Component Batch",
          "sequence": 40,
          "dcParameterType": "TEXT",
          "dcParameterStatus": "ENABLED",
          "parameterPrompt": "",
          "overrideMinMax": false,
          "unitOfMeasure": "",
          "requiredDataEntries": 0,
          "autoLogNc": false
        }
      ]
    },
    "sfcDataCollectionStatus": {
      "sfc": "EVO254000015",
      "timesProcessed": 0,
      "hasDataCollected": false,
      "measuredCount": 0
    },
    "allDataCollected": false,
    "step": {
      "stepId": "0030",
      "routing": {
        "routing": "12732532",
        "version": "ERP001",
        "type": "H",
        "plant": "EVO"
      }
    },
    "operation": {
      "operation": "12732532-0-0030",
      "version": "ERP001"
    },
    "shopOrder": "12732532",
    "resource": "S21",
    "workCenter": "S21"
  }
]
```

## Applications/Use Cases

- Conoscere i dettagli di testata della data collection group necessari per performare poi un log dei suoi parametri
- Conoscere i parametri data collection da campionare per un determinato seriale / operazione
- Sapere se sono stati registrati tutti i parametri richiesti (_allDataCollected == true_)
- Avere informazioni riguardo lo stato di processamento (oggetto _sfcDataCollectionStatus_)

## Errors & Limitations

Nel caso in cui non ci fossero parametri previsti per il seriale l’API risponde 200 OK con un array vuoto.

Nel caso invece si specificassero dei valori non validi a livello di centro di lavoro, operazione o seriale, l’API risponde con un errore 400 Bad Request

```json
{
  "message": "Operation 12732532-0-0020 and version EVO not found at plant {2}",
  "causeMessage": null,
  "code": "dc.operation.not.found",
  "correlationId": "970763b8-acee-4d58-85a9-97517ce343ea"
}
```

# **4.7.2 Sample parameters**

> Verb: POST

## Endpoint

<aside>
🌐

{baseUrl}/datacollection/v1/log

</aside>

## Parameters

- plant → codice identificativo del plant
- workcenter → codice identificativo del centro di lavoro
- resource → codice identificativo del centro di lavoro
- group → id del gruppo di appartenenza del parametro (da chiamata GET effettuata in precedenza)
- sfcs → id del seriale
- operation → id dell’operazione
- parameterValues → elenco dei parametri da loggare (uno solo se se ne effettua uno alla volta). Specificare name e value

```json
{
  "group": {
    "dcGroup": "DC_ASSEMBLY_TEST",
    "version": "A"
  },
  "operation": {
    "operation": "12732532-0-0010",
    "version": "ERP001"
  },
  "plant": "EVO",
  "workCenter": "496",
  "resource": "496",
  "sfcs": ["EVO254000015"],
  "parameterValues": [
    { "name": "FIXING", "value": "27.65" },
    { "name": "CHILD_SERIAL", "value": "TEST_1234" },
    { "name": "CHECKED", "value": "YES" },
    { "name": "COMPONENT_BATCH", "value": "20250915A" }
  ]
}
```

## Response Example

```json
{
  "success": true,
  "failedSfcs": [],
  "loggedNonConformantSfcs": [],
  "loggedNonConformanceParameters": []
}
```

## Applications/Use Cases

- Effettuare la registrazione di una serie di parametri di una determinata data collection (piano controllo qualità)

# 4.9 SFC in Work

> Verb: GET

## Endpoint

<aside>
🌐

{baseUrl}/sfc/v1/sfcsInWork?plant=<plant>&resource=<resource>

</aside>

## Parameters

- plant → codice identificativo del plant
- resource→ codice identificativo centro di lavoro

## Response Example

```json
[
  {
    "sfc": "EVO254000016",
    "operation": "12732532-0-0010"
  }
]
```

## Applications/Use Cases

- Conoscere il/i seriali in lavorazione su un determinato centro di lavoro

## Errors & Limitations

Nel caso in cui sul centro di lavoro non ci fossero seriali in lavorazione l’API risponde 200 OK con un array vuoto

# 4.10 Order Execution Information

> Verb: GET

## Endpoint

<aside>
🌐

{baseUrl}/sfc/v1/worklist/orders?plant=<plant>&filter.order=<order>&allSfcSteps=true

</aside>

## Parameters

- plant → codice identificativo del plant
- order→ codice identificativo ordine

## Response Example

```json
[
  {
    "orderPlannedStartDateTime_Z": "2025-09-11T22:00:00Z",
    "orderPlannedStartDateTime": "2025-09-11T22:00:00.000+00:00",
    "orderPlannedCompleteDateTime_Z": "2025-09-17T22:00:00Z",
    "orderPlannedCompleteDateTime": "2025-09-17T22:00:00.000+00:00",
    "order": "12732532",
    "type": "PRODUCTION",
    "orderStatus": "RELEASED",
    "priority": 500,
    "material": {
      "material": "XEA802720007",
      "version": "ERP001",
      "description": "C 12 2 F 11.9 S05 B5"
    },
    "bom": {
      "bom": "12732532-XEA802720007-1-1",
      "version": "ERP001",
      "type": "SHOPORDERBOM"
    },
    "buildQuantity": 30.0,
    "releasedQuantity": 30.0,
    "orderedQuantity": 30.0,
    "scrappedQuantity": 0.0,
    "doneQuantity": 0.0,
    "quantityRejected": 0.0,
    "quantityInQueue": 29.0,
    "quantityInWork": 1.0,
    "quantityCompletePending": 0.0,
    "customer": null,
    "customerOrder": null,
    "batchNumber": "",
    "erpOrder": "true",
    "erpProductionVersion": "0001",
    "erpUnitOfMeasure": "ST",
    "erpPutAwayStoreLoc": "EVO1",
    "scheduledResource": null,
    "orderCategory": "PRODUCTION_ORDER",
    "orderScheduledStartDateTime_Z": "2025-09-16T22:00:00Z",
    "orderScheduledStartDateTime": "2025-09-16T22:00:00.000+00:00",
    "orderScheduledCompleteDateTime_Z": "2025-09-16T22:00:00Z",
    "orderScheduledCompleteDateTime": "2025-09-16T22:00:00.000+00:00",
    "orderReleasedDateTime_Z": "2025-09-15T07:28:24Z",
    "orderReleasedDateTime": "2025-09-15T07:28:24.111+00:00",
    "orderActualStartDateTime_Z": "2025-09-15T08:21:13Z",
    "orderActualStartDateTime": "2025-09-15T08:21:13.124+00:00",
    "orderActualCompleteDateTime_Z": null,
    "orderActualCompleteDateTime": null,
    "customValues": [
      {
        "sourceType": "SHOP_ORDER",
        "attribute": "MANUFACTURING_ORDER_PARENT",
        "value": ""
      }
    ],
    "orderSfcs": [
      {
        "sfc": "EVO254000015",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "402",
          "description": "IN_QUEUE"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 1.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": true,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000016",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "403",
          "description": "ACTIVE"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 0.0,
        "quantityInWork": 1.0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": "496",
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 1.0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000017",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000018",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000019",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000020",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000021",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000022",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000023",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000024",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000025",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000026",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000027",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000028",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000029",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000030",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000031",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000032",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000033",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000034",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000035",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000036",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000037",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000038",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000039",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000040",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000041",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000042",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000043",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      },
      {
        "sfc": "EVO254000044",
        "order": "12732532",
        "orderScheduledStartDateTime": "2025-09-16T22:00:00Z",
        "orderScheduledCompleteDateTime": "2025-09-16T22:00:00Z",
        "material": {
          "material": "XEA802720007",
          "version": "ERP001",
          "description": "C 12 2 F 11.9 S05 B5"
        },
        "bom": {
          "bom": "12732532-XEA802720007-1-1",
          "version": "ERP001",
          "type": "SHOPORDERBOM"
        },
        "routing": {
          "routing": "12732532",
          "version": "ERP001",
          "type": "SHOPORDER_SPECIFIC"
        },
        "status": {
          "code": "401",
          "description": "NEW"
        },
        "priority": 500,
        "defaultBatchId": null,
        "quantity": 1.0,
        "quantityRejected": 0.0,
        "quantityInQueue": 1.0,
        "quantityInWork": 0,
        "quantityCompletePending": 0.0,
        "actualCompletionDateTime_Z": null,
        "actualCompletionDateTime": null,
        "steps": [
          {
            "stepId": "0010",
            "stepSequence": 10,
            "description": "MONTAGGIO PARTE LENTA + CORONA 1° RID.",
            "operation": {
              "operation": "12732532-0-0010",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 1.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "496",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          },
          {
            "stepId": "0030",
            "stepSequence": 20,
            "description": "CHIUSURA + COLLAUDO",
            "operation": {
              "operation": "12732532-0-0030",
              "version": "ERP001",
              "description": null
            },
            "stepRouting": {
              "routing": "12732532",
              "version": "ERP001",
              "type": "SHOPORDER_SPECIFIC"
            },
            "resource": null,
            "quantityDone": 0.0,
            "quantityScrapped": 0.0,
            "quantityRejected": 0.0,
            "quantityInQueue": 0.0,
            "quantityInWork": 0,
            "quantityCompletePending": 0.0,
            "stepDone": false,
            "plannedWorkCenter": "S21",
            "operationScheduledStartDate": "2025-09-16T22:00:00Z",
            "operationScheduledCompletionDate": "2025-09-16T22:00:00Z"
          }
        ]
      }
    ]
  }
]
```

## Applications/Use Cases

- Sapere lo stato di execution di ogni seriale di un determinato ordine di produzione
- Calcolare eventuali quantità di produzione di un ordine in base allo stato di avanzamento dei vari seriali figli
- Conoscere eventuali dati per controlli a livello di fase/quantità durante l’avanzamento produzione su supervisore

# 4.11.1 Create a Non Conformance

> Verb: POST

## Endpoint

<aside>
🌐

{baseUrl}/nonconformance/v1/log

</aside>

## Parameters

- plant → codice identificativo del plant
- code → identificativo del codice NC da aprire (capire chi manuteniene le causali)
- sfcs → id/ids dei seriali contenuto della non conformità
- resource → codice centro di lavoro
- workCenter → codice centro di lavoro
- routingStepId → identificativo dello step operazione
- quantity → quantità del seriale (sempre 1 per produzione discreta in assembly EVO)

```json
{
  "plant": "EVO",
  "code": "SEAL_FAULT",
  "sfcs": ["EVO254000017"],
  "resource": "496",
  "workCenter": "496",
  "routingStepId": "0010",
  "quantity": "1"
}
```

## Response Example

```json
{
  "ids": [
    {
      "sfc": "EVO254000017",
      "ids": ["8baa8850-a8ee-495d-a014-466e6bf1ee9d"]
    }
  ]
}
```

In ids ci sono gli identificativi dell’oggetto “non conformità” creato su Digital manufacturing attraverso il quale sarà poi possibile interagire con esso (valutare se memorizzarlo da qualche parte lato supervisore).

## Applications/Use Cases

- Aprire una nuova non conformità su un seriale per segnalare un difetto riscontrato e far partire il flusso di rilavorazione/riparazione. La NC creata avrà come stato OPEN.

# 4.11.2 Get Non Conformance by serial

> Verb: GET

## Endpoint

<aside>
🌐

{baseUrl}/nonconformance/v2/nonconformances?plant=<plant>&operationActivity=<operationActivity>&resource=<resource>&sfc=<sfc>

</aside>

## Parameters

- plant → codice identificativo del plant
- operationActivity → identificativo dell’operazione del ciclo nella quale è stata aperta la NC
- resource → codice centro di lavoro presso la quale è stata aperta la NC
- sfc → codice seriale

## Response Example

```json
[
  {
    "plant": "EVO",
    "incidentNumber": null,
    "id": "714e6157-8b07-44d7-acd1-8f1f604a0915",
    "code": {
      "plant": "EVO",
      "code": "CORONA_FAULT",
      "status": "ENABLED",
      "description": "Defect on the Corona assembly",
      "category": "FAILURE",
      "codeDef": {
        "severity": "MEDIUM",
        "dataType": "NONE",
        "dispositionRoutings": [],
        "secondaryCodes": [],
        "canBePrimaryCode": true,
        "createdDateTime": "2025-09-15T10:44:46.518Z",
        "modifiedDateTime": null,
        "autoCloseIncident": false
      },
      "groups": [
        {
          "modifiedDateTime": "2025-09-15T10:40:52.790Z",
          "createdDateTime": "2025-09-15T10:40:52.795Z",
          "plant": "EVO",
          "group": "NC_ASSEMBLY_TEST",
          "description": "Non Conformance for testing of Assembly",
          "validAtAllOperationActivities": true,
          "isEbr": false,
          "operationActivities": []
        }
      ],
      "customValues": [],
      "createdDateTime": "2025-09-15T10:44:46.360Z",
      "modifiedDateTime": "2025-09-15T10:44:46.567Z"
    },
    "sfc": "EVO254000017",
    "user": "dmc_services_user",
    "quantity": 1.0,
    "comments": null,
    "date": "2025-09-15T13:02:56.462Z",
    "resource": "496",
    "component": null,
    "category": "FAILURE",
    "state": "CLOSED",
    "modifiedDateTime": "2025-09-15T13:12:10.220Z",
    "createdDateTime": "2025-09-15T13:02:56.478Z",
    "routingStepId": "0010",
    "routing": {
      "plant": "EVO",
      "routing": "12732532",
      "type": "SHOPORDER_SPECIFIC",
      "version": "ERP001"
    },
    "material": {
      "plant": "EVO",
      "material": "XEA802720007",
      "version": "ERP001"
    },
    "operationActivity": {
      "plant": "EVO",
      "operationActivity": "12732532-0-0010",
      "version": "ERP001"
    },
    "fileIds": [],
    "workCenter": "496",
    "dataFields": []
  },
  {
    "plant": "EVO",
    "incidentNumber": null,
    "id": "8baa8850-a8ee-495d-a014-466e6bf1ee9d",
    "code": {
      "plant": "EVO",
      "code": "SEAL_FAULT",
      "status": "ENABLED",
      "description": "Defect on seal detected",
      "category": "FAILURE",
      "codeDef": {
        "severity": "MEDIUM",
        "dataType": "NONE",
        "dispositionRoutings": [],
        "secondaryCodes": [],
        "canBePrimaryCode": true,
        "createdDateTime": "2025-09-15T10:46:19.547Z",
        "modifiedDateTime": null,
        "autoCloseIncident": false
      },
      "groups": [
        {
          "modifiedDateTime": "2025-09-15T10:40:52.790Z",
          "createdDateTime": "2025-09-15T10:40:52.795Z",
          "plant": "EVO",
          "group": "NC_ASSEMBLY_TEST",
          "description": "Non Conformance for testing of Assembly",
          "validAtAllOperationActivities": true,
          "isEbr": false,
          "operationActivities": []
        }
      ],
      "customValues": [],
      "createdDateTime": "2025-09-15T10:46:19.508Z",
      "modifiedDateTime": "2025-09-15T10:46:19.565Z"
    },
    "sfc": "EVO254000017",
    "user": "dmc_services_user",
    "quantity": 1.0,
    "comments": null,
    "date": "2025-09-15T13:13:26.901Z",
    "resource": "496",
    "component": null,
    "category": "FAILURE",
    "state": "OPEN",
    "modifiedDateTime": "2025-09-15T13:13:26.901Z",
    "createdDateTime": "2025-09-15T13:13:26.913Z",
    "routingStepId": "0010",
    "routing": {
      "plant": "EVO",
      "routing": "12732532",
      "type": "SHOPORDER_SPECIFIC",
      "version": "ERP001"
    },
    "material": {
      "plant": "EVO",
      "material": "XEA802720007",
      "version": "ERP001"
    },
    "operationActivity": {
      "plant": "EVO",
      "operationActivity": "12732532-0-0010",
      "version": "ERP001"
    },
    "fileIds": [],
    "workCenter": "496",
    "dataFields": []
  }
]
```

Potrebbero esserci n non conformità create per uno stesso seriale. Il loro stato si può capire dalla property state (OPEN → non conformità ancora non risolta, aperta / CLOSED → non conformità gestita, chiusa).

## Applications/Use Cases

- Conoscere le informazioni delle non conformità (aperte e chiuse) per un determinato seriale

# 4.11.3 Check Non Conformance Open

> Verb: POST

## Endpoint

<aside>
🌐

{baseUrl}/nonconformance/v1/checkForOpen

</aside>

## Parameters

- plant → codice identificativo del plant
- sfcs → codice/i seriale
- bomLevel → 1 (hardcoding)

```json
{
  "sfcs": ["EVO254000017"],
  "bomLevel": 1,
  "plant": "EVO"
}
```

## Response Example

Nel caso in cui ci fosse una NC aperta per il/i seriali specificati, l’API ritorna 400 Bad request e il seguente payload

```json
{
  "code": "openNcCodes.found",
  "message": "Open nonconformance codes were found: SEAL_FAULT.",
  "correlationId": "1e581735-a7e6-4b4a-8708-545dfa10f9a9",
  "details": null
}
```

Nel caso in cui non ci fossero NC aperte (tutte gestite), allora ritorna 200 OK senza un payload.

## Applications/Use Cases

- Verificare se esiste una NC ancora aperta per un determinato seriale

# 4.11.4 Close a Non Conformance

> Verb: POST

## Endpoint

<aside>
🌐

{baseUrl}/nonconformance/v1/close

</aside>

## Parameters

- plant → codice identificativo del plant
- id → identificativo della non conformità ottenuto da chiamate precedenti

```json
{
  "id": "8baa8850-a8ee-495d-a014-466e6bf1ee9d",
  "plant": "EVO"
}
```

## Response Example

Nel caso in cui la NC fosse chiusa con successo l’API ritorna 200 OK.

Nel caso fossero stati specificati dei parametri non validi ritorna un errore 400 Bad Request.

## Applications/Use Cases

- Chiudere una specifica non conformità

# 4.11.5 Re-Open a Non Conformance

> Verb: POST

## Endpoint

<aside>
🌐

{baseUrl}/nonconformance/v1/open

</aside>

## Parameters

- plant → codice identificativo del plant
- id → identificativo della non conformità ottenuto da chiamate precedenti

```json
{
  "id": "8baa8850-a8ee-495d-a014-466e6bf1ee9d",
  "plant": "EVO"
}
```

## Response Example

Nel caso in cui la NC fosse aperta con successo l’API ritorna 200 OK.

Nel caso fossero stati specificati dei parametri non validi ritorna un errore 400 Bad Request.

## Applications/Use Cases

- Ri-aprire una non conformità chiusa per qualche ragione in precendenza

# 4.12.1 Machine Down

> Verb: POST

## Endpoint

<aside>
🌐

{baseUrl}/resource/v2/changestatus

</aside>

## Parameters

- plant → codice identificativo del plant
- resource → identificativo del centro di lavoro
- status → codice del tipo di fermo che si vuole aprire. Da capire come e se verranno categorizzati e gestiti i fermi
  - SCHEDULED_DOWN → fermo schedulato
  - UNSCHEDULED_DOWN → fermo non schedulato
- user → utenza tecnica di integrazione (da stabile poi quale) che effettua il cambio stato
- immediate → true (hardcoding)
- recordType → MANUAL (hardcoding)
- reasonCode (optional) → causale di fermo per causalizzare direttamente il downtime che si genera contestualmente

```json
{
  "plant": "EVO",
  "resource": "496",
  "status": "UNSCHEDULED_DOWN",
  "user": "paolo.aimar@sysconsgroup.com",
  "immediate": "true",
  "recordType": "MANUAL",
  "reasonCode": {
    "timeElement": "UNSCD_DOWN",
    "reasonCode1": "UN-MEC-FAULT"
  }
}
```

## Response Example

Nel caso in cui la il cambio stato viene effettuato con successo l’API ritorna 204 No Content.

Nel caso fossero stati specificati dei parametri non validi ritorna un errore 400 Bad Request.

## Applications/Use Cases

- Dichiarare un fermo su un determinato centro di lavoro che lo rende indisponibile

# 4.12.2 Machine productive

> Verb: POST

## Endpoint

<aside>
🌐

{baseUrl}/resource/v2/changestatus

</aside>

## Parameters

- plant → codice identificativo del plant
- resource → identificativo del centro di lavoro
- status → PRODUCTIVE (hardcoding)
- user → utenza tecnica di integrazione (da stabile poi quale) che effettua il cambio stato
- immediate → true (hardcoding)
- recordType → MANUAL (hardcoding)

```json
{
  "plant": "EVO",
  "resource": "496",
  "status": "PRODUCTIVE",
  "user": "paolo.aimar@sysconsgroup.com",
  "immediate": "true",
  "recordType": "MANUAL"
}
```

## Response Example

Nel caso in cui la il cambio stato viene effettuato con successo l’API ritorna 204 No Content.

Nel caso fossero stati specificati dei parametri non validi ritorna un errore 400 Bad Request.

## Applications/Use Cases

- Dichiarare un macchina operativa e chiudere un eventuale downtime aperto in precendenza
