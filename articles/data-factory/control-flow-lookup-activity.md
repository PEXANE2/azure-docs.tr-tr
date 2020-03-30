---
title: Azure Veri Fabrikası'nda arama etkinliği
description: Harici bir kaynaktan bir değer aramak için Arama etkinliğini nasıl kullanacağınızı öğrenin. Bu çıktı, başarılı etkinliklerle daha da başvurulabilir.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/15/2018
ms.openlocfilehash: 08cc7ce8f306095a66bc0f8cf74dff8c8b551ecf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75440467"
---
# <a name="lookup-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda arama etkinliği

Arama etkinliği, Azure Veri Fabrikası tarafından desteklenen herhangi bir veri kaynağından bir veri kümesi alabilir. Aşağıdaki senaryoda kullanın:
- Nesne adını sert kodlamak yerine sonraki bir etkinlikte hangi nesnelerin üzerinde çalışacağını dinamik olarak belirleyin. Bazı nesne örnekleri dosya ve tablolardır.

Arama etkinliği, yapılandırma dosyasının veya tablonun içeriğini okur ve döndürür. Ayrıca, bir sorgu veya depolanan yordamyürütme sonucu döndürür. Arama etkinliğinden çıktı, singleton değeriyse sonraki bir kopyalama veya dönüştürme etkinliğinde kullanılabilir. Çıktı, bir dizi öznitelik varsa ForEach etkinliğinde kullanılabilir.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Aşağıdaki veri kaynakları Arama etkinliği için desteklenir. Arama etkinliği tarafından döndürülebilen en büyük satır sayısı 5.000'dir ve boyutu 2 MB'a kadardır. Şu anda, zaman aramadan önce Arama etkinliği için en uzun süre bir saattir.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores-for-lookup-activity.md)]

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "LookupActivity",
    "type": "Lookup",
    "typeProperties": {
        "source": {
            "type": "<source type>"
            <additional source specific properties (optional)>
        },
        "dataset": { 
            "referenceName": "<source dataset name>",
            "type": "DatasetReference"
        },
        "firstRowOnly": false
    }
}
```

## <a name="type-properties"></a>Tür özellikleri

Adı | Açıklama | Tür | Gerekli mi?
---- | ----------- | ---- | --------
Dataset | Arama için veri kümesi başvurusu sağlar. İlgili her bağlayıcı makalesinde **Dataset özellikleri** bölümünden ayrıntılar alın. | Anahtar/değer çifti | Evet
source | Veri kümesine özgü kaynak özellikleri içerir, Kopyalama Etkinliği kaynağıyla aynıdır. İlgili her bağlayıcı makalesinde **Etkinlik kopyalama özelliklerinden** ayrıntıları alın. | Anahtar/değer çifti | Evet
ilkSatırSadece | Yalnızca ilk satırı mı yoksa tüm satırları mı döndüreceklerini gösterir. | Boole | Hayır. Varsayılan değer: `true`.

> [!NOTE]
> 
> * **ByteArray türüne** sahip kaynak sütunlar desteklenmez.
> * **Yapı** veri kümesi tanımlarında desteklenmez. Metin biçimindeki dosyalar için sütun adını sağlamak için üstbilgi satırını kullanın.
> * Arama kaynağınız bir JSON dosyasıysa, JSON nesnesini yeniden şekillendirme `jsonPathDefinition` ayarı desteklenmez. Tüm nesneler geri alınacaktır.

## <a name="use-the-lookup-activity-result-in-a-subsequent-activity"></a>Sonraki bir etkinlikte Arama etkinliği sonucunu kullanma

Arama sonucu, etkinlik çalıştırma `output` sonucunun bölümünde döndürülür.

* **(varsayılan) `true` olarak ayarlandığında, `firstRowOnly` **çıktı biçimi aşağıdaki kodda gösterildiği gibi olur. Arama sonucu sabit `firstRow` bir anahtar altındadır. Sonraki etkinlikte sonucu kullanmak için, '' nin `@{activity('MyLookupActivity').output.firstRow.TableName}`deseni kullanın.

    ```json
    {
        "firstRow":
        {
            "Id": "1",
            "TableName" : "Table1"
        }
    }
    ```

* **Ne `firstRowOnly` zaman `false`ayarlanır **, çıkış biçimi aşağıdaki kodda gösterildiği gibi. Alan, `count` kaç kaydın döndürüldüğünü gösterir. Ayrıntılı değerler sabit `value` bir dizi altında görüntülenir. Böyle bir durumda, Arama etkinliği bir [Foreach etkinliği](control-flow-for-each-activity.md)tarafından takip edilir. Diziyi `value` ForEach etkinlik `items` alanına ' nın deseni kullanarak `@activity('MyLookupActivity').output.value`geçirirsiniz. Dizideki öğelere `value` erişmek için aşağıdaki sözdizimini kullanın: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. `@{activity('lookupActivity').output.value[0].tablename}` bunun bir örneğidir.

    ```json
    {
        "count": "2",
        "value": [
            {
                "Id": "1",
                "TableName" : "Table1"
            },
            {
                "Id": "2",
                "TableName" : "Table2"
            }
        ]
    } 
    ```

### <a name="copy-activity-example"></a>Etkinlik'i kopyala örneği
Bu örnekte, Etkinliği Kopyala, Azure SQL Veritabanı örneğinizdeki bir SQL tablosundaki verileri Azure Blob depolama alanına kopyalar. SQL tablosunun adı Blob depolamasındaki bir JSON dosyasında depolanır. Arama etkinliği çalışma zamanında tablo adını arar. JSON bu yaklaşım kullanılarak dinamik olarak değiştirilir. Ardışık hatları veya veri kümelerini yeniden dağıtmanız gerekmez. 

Bu örnek, yalnızca ilk satırı arar. Tüm satırları aramak ve ForEach etkinliğiyle sonuçları zincirlemek için, [Azure Veri Fabrikası'nı kullanarak birden çok tabloyu toplu olarak kopyala'daki](tutorial-bulk-copy.md)örnekleri görün.

### <a name="pipeline"></a>İşlem hattı
Bu ardışık işlem iki etkinlik içerir: Arama ve Kopya. 

- Arama etkinliği, Azure Blob depolama sındaki bir konuma atıfta bulunan **LookupDataset'i**kullanacak şekilde yapılandırılmıştır. Arama etkinliği, bu konumdaki bir JSON dosyasından SQL tablosunun adını okur. 
- Copy Activity, SQL tablosunun adı olan Arama etkinliğinin çıktısını kullanır. **SourceDataset'teki** **tabloAdı** özelliği, Arama etkinliğinden çıkan çıktıyı kullanacak şekilde yapılandırılır. Etkinlik kopyalarını SQL tablosundan Azure Blob depolamasındaki bir konuma kopyalar. Konum, **SinkDataset** özelliği tarafından belirtilir. 

```json
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "LookupActivity",
                "type": "Lookup",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "dataset": { 
                        "referenceName": "LookupDataset", 
                        "type": "DatasetReference" 
                    }
                }
            },
            {
                "name": "CopyActivity",
                "type": "Copy",
                "typeProperties": {
                    "source": { 
                        "type": "SqlSource", 
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.firstRow.tableName}" 
                    },
                    "sink": { 
                        "type": "BlobSink" 
                    }
                },                
                "dependsOn": [ 
                    { 
                        "activity": "LookupActivity", 
                        "dependencyConditions": [ "Succeeded" ] 
                    }
                 ],
                "inputs": [ 
                    { 
                        "referenceName": "SourceDataset", 
                        "type": "DatasetReference" 
                    } 
                ],
                "outputs": [ 
                    { 
                        "referenceName": "SinkDataset", 
                        "type": "DatasetReference" 
                    } 
                ]
            }
        ]
    }
}
```

### <a name="lookup-dataset"></a>Arama veri kümesi
**Arama** veri kümesi, **AzureStorageLinkedService** türütarafından belirtilen Azure Depolama arama klasöründeki **kaynak tablo.json** dosyasıdır. 

```json
{
    "name": "LookupDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "lookup",
            "fileName": "sourcetable.json",
            "format": {
                "type": "JsonFormat",
                "filePattern": "SetOfObjects"
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="source-dataset-for-copy-activity"></a>Kopyalama Etkinliği için **kaynak** veri kümesi
**Kaynak** veri kümesi, SQL tablosunun adı olan Arama etkinliğinin çıktısını kullanır. Etkinlik kopyalarını bu SQL tablosundan Azure Blob depolamasındaki bir konuma kopyalayın. Konum **lavabo** veri kümesi tarafından belirtilir. 

```json
{
    "name": "SourceDataset",
    "properties": {
        "type": "AzureSqlTable",
        "typeProperties":{
            "tableName": "@{activity('LookupActivity').output.firstRow.tableName}"
        },
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="sink-dataset-for-copy-activity"></a>Kopyalama Etkinliği için **veri kümesini batırın**
Etkinlik kopyalarını SQL tablosundan Azure Depolama'daki **csv** klasöründeki **filebylookup.csv** dosyasına kopyalar. Dosya **AzureStorageLinkedService** özelliği tarafından belirtilir. 

```json
{
    "name": "SinkDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "csv",
            "fileName": "filebylookup.csv",
            "format": {
                "type": "TextFormat"                                                                    
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti
Bu depolama hesabı, SQL tablolarının adlarını içeren JSON dosyasını içerir. 

```json
{
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<StorageAccountKey>"
        }
    },
        "name": "AzureStorageLinkedService"
}
```

### <a name="azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti
Bu Azure SQL Veritabanı örneği Blob depolamasına kopyalanacak verileri içerir. 

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Server=<server>;Initial Catalog=<database>;User ID=<user>;Password=<password>;"
        }
    }
}
```

### <a name="sourcetablejson"></a>sourcetable.json

#### <a name="set-of-objects"></a>Nesne kümesi

```json
{
  "Id": "1",
  "tableName": "Table1"
}
{
   "Id": "2",
  "tableName": "Table2"
}
```

#### <a name="array-of-objects"></a>Nesne dizisi

```json
[ 
    {
        "Id": "1",
        "tableName": "Table1"
    },
    {
        "Id": "2",
        "tableName": "Table2"
    }
]
```

## <a name="limitations-and-workarounds"></a>Sınırlamalar ve geçici geçici işler

Arama etkinliğinin bazı sınırlamaları ve önerilen geçici çözüm geçici çözüm.

| Sınırlama | Geçici çözüm |
|---|---|
| Arama etkinliği en fazla 5.000 satıra ve en fazla 2 MB boyutuna sahiptir. | Dış ardışık boru hattının iç bir ardışık alan üzerinde titreştiği ve en büyük satırları veya boyutu aşmayan verileri aldığı iki düzeyli bir ardışık hat lar tasarla. |
| | |

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası tarafından desteklenen diğer kontrol akışı etkinliklerine bakın: 

- [Boru Hattı etkinliğini yürütme](control-flow-execute-pipeline-activity.md)
- [ForEach etkinliği](control-flow-for-each-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
- [Web etkinliği](control-flow-web-activity.md)
