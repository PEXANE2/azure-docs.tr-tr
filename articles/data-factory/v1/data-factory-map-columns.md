---
title: Azure Veri Fabrikası'nda veri kümesi sütunlarını eşleme
description: Kaynak sütunları hedef sütunlara nasıl eşlendireceklerini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 6eb7012e28319ee6cc86de5ee56090743d681068
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74923880"
---
# <a name="map-source-dataset-columns-to-destination-dataset-columns"></a>Hedef veri kümesi sütunlarına kaynak veri kümesi sütunlarını eşle
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. 

Sütun eşleme, kaynak tablo haritasının "yapısında" belirtilen sütunların lavabo tablosunun "yapısı"nda belirtilen sütunlara nasıl belirtildiğini belirtmek için kullanılabilir. **SütunEşleme** özelliği, Kopyalama etkinliğinin **typeProperties** bölümünde kullanılabilir.

Sütun eşleme aşağıdaki senaryoları destekler:

* Kaynak veri kümesi yapısındaki tüm sütunlar, lavabo veri kümesi yapısındaki tüm sütunlara eşlenir.
* Kaynak veri kümesi yapısındaki sütunların bir alt kümesi, lavabo veri kümesi yapısındaki tüm sütunlara eşlenir.

Bir özel durumla sonuçlanan hata koşulları şunlardır:

* Lavabo tablosunun "yapısı"nda eşlemede belirtilenden daha az sütun veya daha fazla sütun.
* Yinelenen eşleme.
* SQL sorgu sonucu eşlemede belirtilen bir sütun adı yok.

> [!NOTE]
> Aşağıdaki örnekler Azure SQL ve Azure Blob içindir, ancak dikdörtgen veri kümelerini destekleyen tüm veri depoları için geçerlidir. İlgili veri kaynağındaki verilere işaret etmek için örneklerdeki veri kümesini ve bağlantılı hizmet tanımlarını ayarlayın.

## <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Örnek 1 – Azure SQL'den Azure blob'una sütun eşleme
Bu örnekte, giriş tablosunun bir yapısı vardır ve Azure SQL veritabanındaki bir SQL tablosunu işaret etmiştir.

```json
{
    "name": "AzureSQLInput",
    "properties": {
        "structure": 
         [
           { "name": "userid"},
           { "name": "name"},
           { "name": "group"}
         ],
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Bu örnekte, çıktı tablosunun bir yapısı vardır ve Azure blob depolamasındaki bir blob'a işaret ediyor.

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
         "structure": 
          [
                { "name": "myuserid"},
                { "name": "myname" },
                { "name": "mygroup"}
          ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Aşağıdaki JSON, bir ardışık ardışık ardışık ardışık alanda bir kopyalama etkinliği tanımlar. Kaynaktan sütunlar **Çevirmen** özelliğini kullanarak lavabodaki**sütunlara (sütun Eşlemeler)** eşlenmiştir.

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "Copy",
    "inputs":  [ { "name": "AzureSQLInput"  } ],
    "outputs":  [ { "name": "AzureBlobOutput" } ],
    "typeProperties":    {
        "source":
        {
            "type": "SqlSource"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
        }
    },
   "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
**Sütun eşleme akışı:**

![Sütun eşleme akışı](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Örnek 2 – Azure SQL'den Azure blob'una SQL sorgusu ile sütun eşleme
Bu örnekte, "yapı" bölümünde ki tablo adını ve sütun adlarını belirtmek yerine Azure SQL'den veri ayıklamak için bir SQL sorgusu kullanılır. 

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "CopyActivity",
    "inputs":  [ { "name": " AzureSQLInput"  } ],
    "outputs":  [ { "name": " AzureBlobOutput" } ],
    "typeProperties":
    {
        "source":
        {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "Translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
        }
    },
    "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
Bu durumda, sorgu sonuçları ilk olarak kaynağın "yapısı"nda belirtilen sütunlara eşlenir. Daha sonra, kaynak "yapı" sütunları sütun Haritalama belirtilen kurallar ile lavabo "yapı" sütunlar eşlenir.  Sorgunun kaynağın "yapısında" belirtilenlerden iki daha fazla sütun olmak üzere 5 sütun döndürtdöndüğünü varsayalım.

**Sütun eşleme akışı**

![Sütun eşleme akışı-2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama Etkinliği'ni kullanma yla ilgili bir öğretici için makaleye bakın: 

- [Blob Depolama'dan SQL Veritabanına veri kopyalama](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
