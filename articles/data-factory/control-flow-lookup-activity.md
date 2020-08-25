---
title: Azure Data Factory 'de arama etkinliği
description: Bir dış kaynaktan bir değer aramak için arama etkinliğini nasıl kullanacağınızı öğrenin. Bu çıkışa, izleyen etkinlikler tarafından daha fazla başvuru yapılabilir.
services: data-factory
documentationcenter: ''
author: linda33wj
ms.author: jingwang
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/24/2020
ms.openlocfilehash: 7a0b4e52d729c3f13d5ac425627970d67b87979e
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88795890"
---
# <a name="lookup-activity-in-azure-data-factory"></a>Azure Data Factory 'de arama etkinliği

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Arama etkinliği, Azure Data Factory tarafından desteklenen herhangi bir veri kaynağından bir veri kümesi alabilir. Aşağıdaki senaryoda kullanın:
- Nesne adını sabit kodlamak yerine, sonraki bir etkinlikte hangi nesnelerin üzerinde çalışacağını dinamik olarak belirleme. Bazı nesne örnekleri dosya ve tablolardır.

Arama etkinliği, bir yapılandırma dosyasının veya tablosunun içeriğini okur ve döndürür. Ayrıca, bir sorgu veya saklı yordam yürütmenin sonucunu döndürür. Arama etkinliğinin çıktısı, tek bir değer ise sonraki bir kopya veya dönüştürme etkinliğinde kullanılabilir. Çıktı bir öznitelik dizisi ise, ForEach etkinliğinde kullanılabilir.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Aşağıdaki veri kaynakları arama etkinliği için desteklenir. Arama etkinliği tarafından döndürülebilecek en büyük satır sayısı, en fazla 2 MB boyutunda 5.000. Şu anda, zaman aşımından önce arama etkinliğinin en uzun süresi bir saattir.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores-for-lookup-activity.md)]

## <a name="syntax"></a>Syntax

```json
{
    "name":"LookupActivity",
    "type":"Lookup",
    "typeProperties":{
        "source":{
            "type":"<source type>"
        },
        "dataset":{
            "referenceName":"<source dataset name>",
            "type":"DatasetReference"
        },
        "firstRowOnly":<true or false>
    }
}
```

## <a name="type-properties"></a>Tür özellikleri

Ad | Açıklama | Tür | Gerekli mi?
---- | ----------- | ---- | --------
veri kümesi | Arama için veri kümesi başvurusu sağlar. İlgili her bağlayıcı makalesinin **veri kümesi özellikleri** bölümünden Ayrıntılar alın. | Anahtar/değer çifti | Yes
kaynak | Kopyalama etkinliği kaynağıyla aynı olan veri kümesine özgü kaynak özelliklerini içerir. İlgili her bağlayıcı makalesinin **etkinlik özelliklerini kopyalama** bölümünden Ayrıntılar alın. | Anahtar/değer çifti | Yes
yalnızca FirstRow | Yalnızca ilk satırın mi yoksa tüm satırların mi döndürülüp döndürülmeyeceğini gösterir. | Boole | Hayır. Varsayılan değer: `true`.

> [!NOTE]
> 
> * **ByteArray** türüne sahip kaynak sütunları desteklenmez.
> * **Yapı** veri kümesi tanımlarında desteklenmiyor. Metin biçimli dosyalar için, sütun adını sağlamak üzere üst bilgi satırını kullanın.
> * Arama kaynağınız bir JSON dosyası ise, `jsonPathDefinition` JSON nesnesini yeniden şekillendirme ayarı desteklenmez. Tüm nesneler alınacaktır.

## <a name="use-the-lookup-activity-result"></a>Arama etkinliği sonucunu kullanın

Arama sonucu, `output` etkinlik çalıştırma sonucunun bölümünde döndürülür.

* **, `firstRowOnly` `true` (Varsayılan) olarak ayarlandığında**, çıkış biçimi aşağıdaki kodda gösterildiği gibidir. Arama sonucu sabit bir `firstRow` anahtar altında. Sonraki etkinlikteki sonucu kullanmak için, öğesinin stilini kullanın  `@{activity('LookupActivity').output.firstRow.table` .

    ```json
    {
        "firstRow":
        {
            "Id": "1",
            "schema":"dbo",
            "table":"Table1"
        }
    }
    ```

* **, `firstRowOnly` Olarak `false` ayarlandığında **, çıkış biçimi aşağıdaki kodda gösterildiği gibidir. Bir `count` alan, kaç kaydın döndürüleceğini gösterir. Ayrıntılı değerler sabit bir dizi altında görüntülenir `value` . Böyle bir durumda, arama etkinliğinin ardından bir [foreach etkinliği](control-flow-for-each-activity.md)gelir. `value`Dizisini ForEach etkinlik `items` alanına geçirin `@activity('MyLookupActivity').output.value` . Dizideki öğelere erişmek için `value` şu sözdizimini kullanın: `@{activity('lookupActivity').output.value[zero based index].propertyname}` . `@{activity('lookupActivity').output.value[0].schema}` bunun bir örneğidir.

    ```json
    {
        "count": "2",
        "value": [
            {
                "Id": "1",
                "schema":"dbo",
                "table":"Table1"
            },
            {
                "Id": "2",
                "schema":"dbo",
                "table":"Table2"
            }
        ]
    } 
    ```

## <a name="example"></a>Örnek

Bu örnekte, işlem hattı iki etkinlik içerir: **arama** ve **kopyalama**. Kopyalama etkinliği, verileri Azure SQL veritabanı örneğiniz içindeki bir SQL tablosundan Azure Blob depolama alanına kopyalar. SQL tablosunun adı, blob depolamada bir JSON dosyasında depolanır. Arama etkinliği çalışma zamanında tablo adını arar. JSON bu yaklaşım kullanılarak dinamik olarak değiştirilir. İşlem hatlarını veya veri kümelerini yeniden dağıtmanız gerekmez. 

Bu örnek yalnızca ilk satır için arama gösterir. Tüm satırları aramak ve sonuçları ForEach etkinliğiyle zincirlemek için [Azure Data Factory kullanarak birden çok tabloyu toplu olarak kopyalama](tutorial-bulk-copy.md)içindeki örneklere bakın.


### <a name="pipeline"></a>İşlem hattı

- Arama etkinliği, Azure Blob depolama alanındaki bir konuma başvuran **Lookupdataset**öğesini kullanacak şekilde yapılandırılmıştır. Arama etkinliği, SQL tablosunun adını bu konumdaki bir JSON dosyasından okur. 
- Kopyalama etkinliği, SQL tablosunun adı olan arama etkinliğinin çıkışını kullanır. **SourceDataset** 'teki **TableName** özelliği, arama etkinliğinin çıktısını kullanacak şekilde yapılandırılmıştır. Kopyalama etkinliği, verileri SQL tablosundan Azure Blob depolama alanındaki bir konuma kopyalar. Konum **Sinkdataset** özelliği tarafından belirtilir. 

```json
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "LookupActivity",
                "type": "Lookup",
                "dependsOn": [],
                "policy": {
                    "timeout": "7.00:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "userProperties": [],
                "typeProperties": {
                    "source": {
                        "type": "JsonSource",
                        "storeSettings": {
                            "type": "AzureBlobStorageReadSettings",
                            "recursive": true
                        },
                        "formatSettings": {
                            "type": "JsonReadSettings"
                        }
                    },
                    "dataset": {
                        "referenceName": "LookupDataset",
                        "type": "DatasetReference"
                    },
                    "firstRowOnly": true
                }
            },
            {
                "name": "CopyActivity",
                "type": "Copy",
                "dependsOn": [
                    {
                        "activity": "LookupActivity",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "policy": {
                    "timeout": "7.00:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "userProperties": [],
                "typeProperties": {
                    "source": {
                        "type": "AzureSqlSource",
                        "sqlReaderQuery": {
                            "value": "select * from [@{activity('LookupActivity').output.firstRow.schema}].[@{activity('LookupActivity').output.firstRow.table}]",
                            "type": "Expression"
                        },
                        "queryTimeout": "02:00:00",
                        "partitionOption": "None"
                    },
                    "sink": {
                        "type": "DelimitedTextSink",
                        "storeSettings": {
                            "type": "AzureBlobStorageWriteSettings"
                        },
                        "formatSettings": {
                            "type": "DelimitedTextWriteSettings",
                            "quoteAllText": true,
                            "fileExtension": ".txt"
                        }
                    },
                    "enableStaging": false,
                    "translator": {
                        "type": "TabularTranslator",
                        "typeConversion": true,
                        "typeConversionSettings": {
                            "allowDataTruncation": true,
                            "treatBooleanAsNumber": false
                        }
                    }
                },
                "inputs": [
                    {
                        "referenceName": "SourceDataset",
                        "type": "DatasetReference",
                        "parameters": {
                            "schemaName": {
                                "value": "@activity('LookupActivity').output.firstRow.schema",
                                "type": "Expression"
                            },
                            "tableName": {
                                "value": "@activity('LookupActivity').output.firstRow.table",
                                "type": "Expression"
                            }
                        }
                    }
                ],
                "outputs": [
                    {
                        "referenceName": "SinkDataset",
                        "type": "DatasetReference",
                        "parameters": {
                            "schema": {
                                "value": "@activity('LookupActivity').output.firstRow.schema",
                                "type": "Expression"
                            },
                            "table": {
                                "value": "@activity('LookupActivity').output.firstRow.table",
                                "type": "Expression"
                            }
                        }
                    }
                ]
            }
        ],
        "annotations": [],
        "lastPublishTime": "2020-08-17T10:48:25Z"
    }
}
```

### <a name="lookup-dataset"></a>Arama veri kümesi

**Arama** veri kümesi, **AzureBlobStorageLinkedService** türü tarafından belirtilen Azure depolama Arama klasöründeki **sourcetable.js** dosyası. 

```json
{
    "name": "LookupDataset",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureBlobStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "annotations": [],
        "type": "Json",
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "fileName": "sourcetable.json",
                "container": "lookup"
            }
        }
    }
}
```

### <a name="source-dataset-for-copy-activity"></a>Kopyalama etkinliği için **kaynak** veri kümesi

**Kaynak** veri KÜMESI, SQL tablosunun adı olan arama etkinliğinin çıkışını kullanır. Kopyalama etkinliği, verileri bu SQL tablosundan Azure Blob depolama alanındaki bir konuma kopyalar. Konum, **Havuz** veri kümesi tarafından belirtilir. 

```json
{
    "name": "SourceDataset",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureSqlDatabase",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "schemaName": {
                "type": "string"
            },
            "tableName": {
                "type": "string"
            }
        },
        "annotations": [],
        "type": "AzureSqlTable",
        "schema": [],
        "typeProperties": {
            "schema": {
                "value": "@dataset().schemaName",
                "type": "Expression"
            },
            "table": {
                "value": "@dataset().tableName",
                "type": "Expression"
            }
        }
    }
}
```

### <a name="sink-dataset-for-copy-activity"></a>Kopyalama etkinliği için **Havuz** veri kümesi

Kopyalama etkinliği, verileri SQL tablosundan Azure Storage 'daki **CSV** klasöründe **filebylookup.csv** dosyasına kopyalar. Dosya, **AzureBlobStorageLinkedService** özelliği tarafından belirtilir. 

```json
{
    "name": "SinkDataset",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureBlobStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "schema": {
                "type": "string"
            },
            "table": {
                "type": "string"
            }
        },
        "annotations": [],
        "type": "DelimitedText",
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "fileName": {
                    "value": "@{dataset().schema}_@{dataset().table}.csv",
                    "type": "Expression"
                },
                "container": "csv"
            },
            "columnDelimiter": ",",
            "escapeChar": "\\",
            "quoteChar": "\""
        },
        "schema": []
    }
}
```

### <a name="sourcetablejson"></a>Üzerinde sourcetable.js

Dosyasında **sourcetable.js** için aşağıdaki iki tür biçimi kullanabilirsiniz.

#### <a name="set-of-objects"></a>Nesne kümesi

```json
{
   "Id":"1",
   "schema":"dbo",
   "table":"Table1"
}
{
   "Id":"2",
   "schema":"dbo",
   "table":"Table2"
}
```

#### <a name="array-of-objects"></a>Nesne dizisi

```json
[ 
    {
        "Id": "1",
        "schema":"dbo",
        "table":"Table1"
    },
    {
        "Id": "2",
        "schema":"dbo",
        "table":"Table2"
    }
]
```

## <a name="limitations-and-workarounds"></a>Sınırlamalar ve geçici çözümler

Arama etkinliğinin ve önerilen geçici çözümlerin bazı sınırlamaları aşağıda verilmiştir.

| Sınırlama | Geçici çözüm |
|---|---|
| Arama etkinliğinin en fazla 5.000 satırı ve en fazla 2 MB boyutu vardır. | Dış işlem hattının, en fazla satır veya boyutu aşmayacak verileri alan bir iç işlem hattı üzerinden yineleyen iki düzeyli bir işlem hattı tasarlayın. |
| | |

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen diğer denetim akışı etkinliklerini görün: 

- [İşlem hattı yürütme etkinliği](control-flow-execute-pipeline-activity.md)
- [ForEach etkinliği](control-flow-for-each-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
