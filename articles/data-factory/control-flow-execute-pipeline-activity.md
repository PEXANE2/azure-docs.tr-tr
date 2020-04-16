---
title: Azure Veri Fabrikası'nda Boru Hattı Etkinliğini Yürütme
description: Başka bir Veri Fabrikası ardışık hattından bir Veri Fabrikası ardışık hattını çağırmak için YürütEngel Hattı Etkinliğini nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 4bd667a2302136b5e12d2e4e548c9e8863715621
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415271"
---
# <a name="execute-pipeline-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Boru Hattı etkinliğini yürütme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

İşlem Hattı Yürütme etkinliği bir Data Factory işlem hattının başka bir işlem hattını çağırmasını sağlar.



## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "MyPipeline",
    "properties": {
        "activities": [
            {
                "name": "ExecutePipelineActivity",
                "type": "ExecutePipeline",
                "typeProperties": {
                    "parameters": {                        
                        "mySourceDatasetFolderPath": {
                            "value": "@pipeline().parameters.mySourceDatasetFolderPath",
                            "type": "Expression"
                        }
                    },
                    "pipeline": {
                        "referenceName": "<InvokedPipelineName>",
                        "type": "PipelineReference"
                    },
                    "waitOnCompletion": true
                 }
            }
        ],
        "parameters": [
            {
                "mySourceDatasetFolderPath": {
                    "type": "String"
                }
            }
        ]
    }
}
```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
ad | Yürütme boru hattı etkinliğinin adı. | Dize | Evet
type | Ayarlanmalıdır: **ExecutePipeline**. | Dize | Evet
Boru hattı | Bu ardışık ardışık ardışık ardışık çağrıldığı bağımlı boru hattı için boru hattı başvurusu. Bir ardışık kaynak başvuru nesnesinin iki özelliği vardır: **referenceName** ve **türü**. ReferenceName özelliği, başvuru ardışık adının belirtin. Tür özelliği PipelineReference olarak ayarlanmalıdır. | Boru HattıReferans | Evet
parametreler | Çağrılan boru hattına geçirilecek parametreler | Parametre adlarını bağımsız değişken değerleriyle eşleyen bir JSON nesnesi | Hayır
waitOnCompletion | Etkinlik yürütmesinin bağımlı ardışık hatlar yürütmesinin tamamlanmasını bekleyip beklemediğini tanımlar. Varsayılan değer false’tur. | Boole | Hayır

## <a name="sample"></a>Örnek
Bu senaryonun iki ardışık etkisi vardır:

- **Ana ardışık hat lar** - Bu ardışık işlem, çağrılan ardışık çağrıyapan bir Execute Pipeline etkinliği vardır. Ana boru hattı iki `masterSourceBlobContainer` `masterSinkBlobContainer`parametre alır: , .
- **Çağrılan ardışık işlem adı** - Bu ardışık işlem, bir Azure Blob kaynağından Azure Blob lavabosu verilerini kopyalayan bir Kopyalama etkinliği vardır. Çağrılan ardışık işlem iki `sourceBlobContainer` `sinkBlobContainer`parametre alır: , .

### <a name="master-pipeline-definition"></a>Ana boru hattı tanımı

```json
{
  "name": "masterPipeline",
  "properties": {
    "activities": [
      {
        "type": "ExecutePipeline",
        "typeProperties": {
          "pipeline": {
            "referenceName": "invokedPipeline",
            "type": "PipelineReference"
          },
          "parameters": {
            "sourceBlobContainer": {
              "value": "@pipeline().parameters.masterSourceBlobContainer",
              "type": "Expression"
            },
            "sinkBlobContainer": {
              "value": "@pipeline().parameters.masterSinkBlobContainer",
              "type": "Expression"
            }
          },
          "waitOnCompletion": true
        },
        "name": "MyExecutePipelineActivity"
      }
    ],
    "parameters": {
      "masterSourceBlobContainer": {
        "type": "String"
      },
      "masterSinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```

### <a name="invoked-pipeline-definition"></a>Çağrılan boru hattı tanımı

```json
{
  "name": "invokedPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "name": "CopyBlobtoBlob",
        "inputs": [
          {
            "referenceName": "SourceBlobDataset",
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sinkBlobDataset",
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```

**Bağlı hizmet**

```json
{
    "name": "BlobStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=*****;AccountKey=*****"
    }
  }
}
```

**Kaynak veri kümesi**
```json
{
    "name": "SourceBlobDataset",
    "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": {
        "value": "@pipeline().parameters.sourceBlobContainer",
        "type": "Expression"
      },
      "fileName": "salesforce.txt"
    },
    "linkedServiceName": {
      "referenceName": "BlobStorageLinkedService",
      "type": "LinkedServiceReference"
    }
  }
}
```

**Havuz veri kümesi**
```json
{
    "name": "sinkBlobDataset",
    "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": {
        "value": "@pipeline().parameters.sinkBlobContainer",
        "type": "Expression"
      }
    },
    "linkedServiceName": {
      "referenceName": "BlobStorageLinkedService",
      "type": "LinkedServiceReference"
    }
  }
}
```

### <a name="running-the-pipeline"></a>Boru hattını çalıştırma

Bu örnekte ana ardışık hattı çalıştırmak için, masterSourceBlobContainer ve masterSinkBlobContainer parametreleri için aşağıdaki değerler geçirilir: 

```json
{
  "masterSourceBlobContainer": "executetest",
  "masterSinkBlobContainer": "executesink"
}
```

Ana ardışık kaynak, bu değerleri aşağıdaki örnekte gösterildiği gibi çağrılan ardışık taşabına iletilir: 

```json
{
    "type": "ExecutePipeline",
    "typeProperties": {
      "pipeline": {
        "referenceName": "invokedPipeline",
        "type": "PipelineReference"
      },
      "parameters": {
        "sourceBlobContainer": {
          "value": "@pipeline().parameters.masterSourceBlobContainer",
          "type": "Expression"
        },
        "sinkBlobContainer": {
          "value": "@pipeline().parameters.masterSinkBlobContainer",
          "type": "Expression"
        }
      },

      ....
}

```
## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası tarafından desteklenen diğer kontrol akışı etkinliklerine bakın: 

- [Her Bir Etkinlik için](control-flow-for-each-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web Etkinliği](control-flow-web-activity.md)
