---
title: Azure Veri Fabrikası'nda Her Etkinlik
description: For Each Activity, ardışık ardınızdaki yinelenen bir kontrol akışını tanımlar. Bir koleksiyon üzerinde tekrarı yapmak ve belirtilen etkinlikleri yürütmek için kullanılır.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/23/2019
ms.openlocfilehash: 71528385563b29ce70edf396434be0174beac105
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804852"
---
# <a name="foreach-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Her Etkinlik
ForEach Etkinliği, ardışık ardınızdaki yinelenen bir kontrol akışını tanımlar. Bu etkinlik bir koleksiyon üzerinde yinelemek için kullanılır ve bir döngüde belirtilen etkinlikleri yürütür. Bu etkinliğin döngü uygulaması, programlama dillerindeki Foreach döngü yapısına benzer.

## <a name="syntax"></a>Sözdizimi
Özellikler daha sonra bu makalede açıklanmıştır. Öğeler özelliği koleksiyondur ve koleksiyondaki her öğe aşağıdaki `@item()` sözdiziminde gösterildiği gibi kullanılarak anılır:  

```json
{  
   "name":"MyForEachActivityName",
   "type":"ForEach",
   "typeProperties":{  
      "isSequential":"true",
        "items": {
            "value": "@pipeline().parameters.mySinkDatasetFolderPathCollection",
            "type": "Expression"
        },
      "activities":[  
         {  
            "name":"MyCopyActivity",
            "type":"Copy",
            "typeProperties":{  
               ...
            },
            "inputs":[  
               {  
                  "referenceName":"MyDataset",
                  "type":"DatasetReference",
                  "parameters":{  
                     "MyFolderPath":"@pipeline().parameters.mySourceDatasetFolderPath"
                  }
               }
            ],
            "outputs":[  
               {  
                  "referenceName":"MyDataset",
                  "type":"DatasetReference",
                  "parameters":{  
                     "MyFolderPath":"@item()"
                  }
               }
            ]
         }
      ]
   }
}

```

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
ad | Her biri için etkinliğin adı. | Dize | Evet
type | **ForEach** olarak ayarlanmalıdır | Dize | Evet
isSequential | Döngünün sırayla mı yoksa paralel olarak mı yürütülmesi gerektiğini belirtir.  En fazla 20 döngü yinelemesi paralel olarak aynı anda yürütülebilir). Örneğin, 10 farklı kaynağa sahip bir kopyalama etkinliği üzerinde bir ForEach etkinliğini yinelerseniz ve **isSequential** false olarak ayarlanmış veri kümelerini batırınsa, tüm kopyalar aynı anda yürütülür. Varsayılan, False'dur. <br/><br/> "isSequential" False olarak ayarlanmışsa, birden çok yürütülebilir çalıştırmak için doğru bir yapılandırma olduğundan emin olun. Aksi takdirde, bu özellik yazma çakışmaları tahakkuk önlemek için dikkatli kullanılmalıdır. Daha fazla bilgi için [Paralel yürütme](#parallel-execution) bölümüne bakın. | Boole | Hayır. Varsayılan, False'dur.
toplu Say | Paralel yürütme sayısını denetlemek için kullanılacak toplu iş sayısı (isSequential false olarak ayarlandığında). Bu üst eşzamanlılık sınırıdır, ancak her biri için etkinlik her zaman bu sayıda yürütülmez | Üstsayı (maksimum 50) | Hayır. Varsayılan değer 20'dir.
Öğeler | Üzerinde yinelenecek bir JSON Dizisini döndüren bir ifade. | İfade (JSON Dizisini döndürür) | Evet
Etkinlikler | Yürütülecek faaliyetler. | Etkinlikler Listesi | Evet

## <a name="parallel-execution"></a>Paralel yürütme
**isSequential** yanlış olarak ayarlanmışsa, etkinlik en fazla 20 eşzamanlı yinelemeye paralel olarak yinelenir. Bu ayar dikkatli kullanılmalıdır. Eşzamanlı yinelemeler aynı klasöre ancak farklı dosyalara yazıyorsa, bu yaklaşım iyidir. Eşzamanlı yinelemeler aynı anda aynı dosyaya yazıyorsa, bu yaklaşım büyük olasılıkla bir hataya neden olur. 

## <a name="iteration-expression-language"></a>Yineleme ifade dili
ForEach etkinliğinde, özellik **öğeleri**için üzerinde yinelenecek bir dizi sağlayın." ForEach etkinliğinde tek bir numaralandırma üzerinde yinelemek için kullanın. `@item()` Örneğin, **öğeler** bir diziyse: [1, 2, `@item()` 3], ilk yinelemede 1, ikinci yinelemede 2 ve üçüncü yinelemede 3 döndürür.

## <a name="iterating-over-a-single-activity"></a>Tek bir etkinlik üzerinde yinelenme
**Senaryo:** Azure Blob'daki aynı kaynak dosyadan Azure Blob'daki birden çok hedef dosyasına kopyalayın.

### <a name="pipeline-definition"></a>Boru hattı tanımı

```json
{
    "name": "<MyForEachPipeline>",
    "properties": {
        "activities": [
            {
                "name": "<MyForEachActivity>",
                "type": "ForEach",
                "typeProperties": {
                    "isSequential": "true",
                    "items": {
                        "value": "@pipeline().parameters.mySinkDatasetFolderPath",
                        "type": "Expression"
                    },
                    "activities": [
                        {
                            "name": "MyCopyActivity",
                            "type": "Copy",
                            "typeProperties": {
                                "source": {
                                    "type": "BlobSource",
                                    "recursive": "false"
                                },
                                "sink": {
                                    "type": "BlobSink",
                                    "copyBehavior": "PreserveHierarchy"
                                }
                            },
                            "inputs": [
                                {
                                    "referenceName": "<MyDataset>",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "MyFolderPath": "@pipeline().parameters.mySourceDatasetFolderPath"
                                    }
                                }
                            ],
                            "outputs": [
                                {
                                    "referenceName": "MyDataset",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "MyFolderPath": "@item()"
                                    }
                                }
                            ]
                        }
                    ]
                }
            }
        ],
        "parameters": {
            "mySourceDatasetFolderPath": {
                "type": "String"
            },
            "mySinkDatasetFolderPath": {
                "type": "String"
            }
        }
    }
}

```

### <a name="blob-dataset-definition"></a>Blob dataset tanımı

```json
{  
   "name":"<MyDataset>",
   "properties":{  
      "type":"AzureBlob",
      "typeProperties":{  
         "folderPath":{  
            "value":"@dataset().MyFolderPath",
            "type":"Expression"
         }
      },
      "linkedServiceName":{  
         "referenceName":"StorageLinkedService",
         "type":"LinkedServiceReference"
      },
      "parameters":{  
         "MyFolderPath":{  
            "type":"String"
         }
      }
   }
}

```

### <a name="run-parameter-values"></a>Parametre değerlerini çalıştırma

```json
{
    "mySourceDatasetFolderPath": "input/",
    "mySinkDatasetFolderPath": [ "outputs/file1", "outputs/file2" ]
}

```

## <a name="iterate-over-multiple-activities"></a>Birden çok etkinlik üzerinde yineleyin
Bir ForEach etkinliğinde birden çok etkinliği (örneğin kopyalama ve web etkinlikleri) üzerinden yinelemek mümkündür. Bu senaryoda, birden çok aktiviteyi ayrı bir ardışık ardışık ardışık ardışık alana soyutlamanızı öneririz. Daha sonra, birden çok etkinlikle ayrı bir ardışık çağrıda bulunmak için ForEach etkinliğiyle birlikte ardışık ardışık [ardışık ardışık ardışık işetkinliğini](control-flow-execute-pipeline-activity.md) kullanabilirsiniz. 


### <a name="syntax"></a>Sözdizimi

```json
{
  "name": "masterPipeline",
  "properties": {
    "activities": [
      {
        "type": "ForEach",
        "name": "<MyForEachMultipleActivities>"
        "typeProperties": {
          "isSequential": true,
          "items": {
            ...
          },
          "activities": [
            {
              "type": "ExecutePipeline",
              "name": "<MyInnerPipeline>"
              "typeProperties": {
                "pipeline": {
                  "referenceName": "<copyHttpPipeline>",
                  "type": "PipelineReference"
                },
                "parameters": {
                  ...
                },
                "waitOnCompletion": true
              }
            }
          ]
        }
      }
    ],
    "parameters": {
      ...
    }
  }
}

```

### <a name="example"></a>Örnek
**Senaryo:** Execute Pipeline etkinliği ile foreach etkinliği içinde bir InnerPipeline üzerinden titre. Şema tanımları parametreli iç boru hattı kopyaları.

#### <a name="master-pipeline-definition"></a>Ana Boru Hattı tanımı

```json
{
  "name": "masterPipeline",
  "properties": {
    "activities": [
      {
        "type": "ForEach",
        "name": "MyForEachActivity",
        "typeProperties": {
          "isSequential": true,
          "items": {
            "value": "@pipeline().parameters.inputtables",
            "type": "Expression"
          },
          "activities": [
            {
              "type": "ExecutePipeline",
              "typeProperties": {
                "pipeline": {
                  "referenceName": "InnerCopyPipeline",
                  "type": "PipelineReference"
                },
                "parameters": {
                  "sourceTableName": {
                    "value": "@item().SourceTable",
                    "type": "Expression"
                  },
                  "sourceTableStructure": {
                    "value": "@item().SourceTableStructure",
                    "type": "Expression"
                  },
                  "sinkTableName": {
                    "value": "@item().DestTable",
                    "type": "Expression"
                  },
                  "sinkTableStructure": {
                    "value": "@item().DestTableStructure",
                    "type": "Expression"
                  }
                },
                "waitOnCompletion": true
              },
              "name": "ExecuteCopyPipeline"
            }
          ]
        }
      }
    ],
    "parameters": {
      "inputtables": {
        "type": "Array"
      }
    }
  }
}

```

#### <a name="inner-pipeline-definition"></a>İç boru hattı tanımı

```json
{
  "name": "InnerCopyPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            }
          },
          "sink": {
            "type": "SqlSink"
          }
        },
        "name": "CopyActivity",
        "inputs": [
          {
            "referenceName": "sqlSourceDataset",
            "parameters": {
              "SqlTableName": {
                "value": "@pipeline().parameters.sourceTableName",
                "type": "Expression"
              },
              "SqlTableStructure": {
                "value": "@pipeline().parameters.sourceTableStructure",
                "type": "Expression"
              }
            },
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sqlSinkDataset",
            "parameters": {
              "SqlTableName": {
                "value": "@pipeline().parameters.sinkTableName",
                "type": "Expression"
              },
              "SqlTableStructure": {
                "value": "@pipeline().parameters.sinkTableStructure",
                "type": "Expression"
              }
            },
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceTableName": {
        "type": "String"
      },
      "sourceTableStructure": {
        "type": "String"
      },
      "sinkTableName": {
        "type": "String"
      },
      "sinkTableStructure": {
        "type": "String"
      }
    }
  }
}

```

#### <a name="source-dataset-definition"></a>Kaynak veri kümesi tanımı

```json
{
  "name": "sqlSourceDataset",
  "properties": {
    "type": "SqlServerTable",
    "typeProperties": {
      "tableName": {
        "value": "@dataset().SqlTableName",
        "type": "Expression"
      }
    },
    "structure": {
      "value": "@dataset().SqlTableStructure",
      "type": "Expression"
    },
    "linkedServiceName": {
      "referenceName": "sqlserverLS",
      "type": "LinkedServiceReference"
    },
    "parameters": {
      "SqlTableName": {
        "type": "String"
      },
      "SqlTableStructure": {
        "type": "String"
      }
    }
  }
}

```

#### <a name="sink-dataset-definition"></a>Lavabo dataset tanımı

```json
{
  "name": "sqlSinkDataSet",
  "properties": {
    "type": "AzureSqlTable",
    "typeProperties": {
      "tableName": {
        "value": "@dataset().SqlTableName",
        "type": "Expression"
      }
    },
    "structure": {
      "value": "@dataset().SqlTableStructure",
      "type": "Expression"
    },
    "linkedServiceName": {
      "referenceName": "azureSqlLS",
      "type": "LinkedServiceReference"
    },
    "parameters": {
      "SqlTableName": {
        "type": "String"
      },
      "SqlTableStructure": {
        "type": "String"
      }
    }
  }
}

```

#### <a name="master-pipeline-parameters"></a>Ana boru hattı parametreleri
```json
{
    "inputtables": [
        {
            "SourceTable": "department",
            "SourceTableStructure": [
              {
                "name": "departmentid",
                "type": "int"
              },
              {
                "name": "departmentname",
                "type": "string"
              }
            ],
            "DestTable": "department2",
            "DestTableStructure": [
              {
                "name": "departmentid",
                "type": "int"
              },
              {
                "name": "departmentname",
                "type": "string"
              }
            ]
        }
    ]
    
}
```

## <a name="aggregating-outputs"></a>Çıktıları toplama

__Foreach__ etkinliğin çıktılarını toplamak için lütfen _Değişkenler_ ve _Ek Değişken_ etkinliğini yararlanın.

İlk olarak, `array` ardışık hatlar bir _değişken_ bildirin. Ardından, __her bir döngü__ içinde _Append Değişken_ etkinliği çağırın. Daha sonra, dizinizden toplama yı alabilirsiniz.

## <a name="limitations-and-workarounds"></a>Sınırlamalar ve geçici geçici işler

ForEach etkinliğinin bazı sınırlamaları ve önerilen geçici çözüm lerle ilgili bazı sınırlamalar aşağıda ve dailmektedir.

| Sınırlama | Geçici çözüm |
|---|---|
| Başka bir ForEach döngüsü (veya Bir Until döngüsü) içine bir ForEach döngü nest olamaz. | ForEach döngüsüne sahip dış boru hattının iç içe geçen döngüyle iç hat lar üzerinde gerçekleştiği iki düzeyli bir boru hattı tasarla. |
| ForEach etkinliği paralel `batchCount` işleme için en fazla 50 ve en fazla 100.000 öğeye sahiptir. | ForEach aktivitesi ile dış boru hattının bir iç boru hattı üzerinde gerçekleştiği iki seviyeli bir boru hattı tasarla. |
| | |

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası tarafından desteklenen diğer kontrol akışı etkinliklerine bakın: 

- [İşlem Hattı Çalıştırma Etkinliği](control-flow-execute-pipeline-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Web Etkinliği](control-flow-web-activity.md)
