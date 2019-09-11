---
title: Azure Data Factory JSON biçimi | Microsoft Docs
description: Bu konuda Azure Data Factory JSON biçimiyle nasıl ele alınacağını açıklamaktadır.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: jingwang
ms.openlocfilehash: 7ff164b378ac6981fcb9686d6264b0bcf50cfafb
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70814817"
---
# <a name="json-format-in-azure-data-factory"></a>Azure Data Factory JSON biçimi

**JSON dosyalarını ayrıştırmak veya VERILERI JSON biçimine yazmak**istediğinizde bu makaleye uyun. 

JSON biçimi aşağıdaki bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [azure blob](connector-azure-blob-storage.md), [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [,](connector-hdfs.md), [http](connector-http.md)ve [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, JSON veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Veri kümesinin Type özelliği **JSON**olarak ayarlanmalıdır. | Evet      |
| location         | Dosya (ler) in konum ayarları. Her dosya tabanlı bağlayıcının, altında `location`kendi konum türü ve desteklenen özellikleri vardır. **Bağlayıcı makalesi-> veri kümesi özellikleri bölümünde ayrıntılara bakın**. | Evet      |
| encodingName     | Test dosyalarını okumak/yazmak için kullanılan kodlama türü. <br>İzin verilen değerler şunlardır: "UTF-8", "UTF-16", "UTF-16IN", "UTF-32", "UTF-32TO", "US-ASCII", "UTF-7", "BIG5", "EUC-JP", "EUC-KR", "GB2312", "GB18030", "JOHAB", "SHIFT-JIS", "CP875", "CP866", "IBM00858", "IBM037", "IBM273", "IBM437", "IBM500", "IBM737", "IBM775", "IBM850" IBM852 "," IBM855 "," IBM857 "," IBM860 "," IBM861 "," IBM863 "," IBM864 "," IBM865 "," IBM869 "," IBM870 "," IBM01140 "," IBM01141 "," IBM01142 "," IBM01143 "," IBM01144 "," IBM01145 "," IBM01146 "," IBM01147 "," IBM01148 "," IBM01149 "," ISO-2022-JP "," ISO-2022-KR "," ISO-8859-1 "," ISO-8859-2 "," ISO-8859-3 "," ISO-8859-4 "," ISO-8859-5 "," ISO-8859-6 "," ISO-8859-7 "," ISO-8859-8 "," ISO-8859-9 "," ISO-8859-13 "," ISO-8859-15 "," WINDOWS-874 "," WINDOWS-1250 "," WINDOWS-1251 "," WINDOWS-1252 "," WINDOWS-1253 "," WINDOWS-1254 "," WINDOWS-1255 "," WINDOWS-1256 "," WINDOWS-1257 "," WINDOWS-1258 ".| Hayır       |
| compressionCodec | Metin dosyalarını okumak/yazmak için kullanılan sıkıştırma codec bileşeni. <br>İzin verilen değerler şunlardır **bzip2**, **gzip**, **söndür**, **zipsöndür**, **Snappy**veya **lz4**. dosyasını kaydederken kullanmak için. <br>Şu anda kopyalama etkinliği "Snappy" & "lz4" desteklemez. | Hayır       |
| compressionLevel | Sıkıştırma oranı. <br>İzin verilen değerler **en iyi** veya **en hızlardır**.<br>- **En hızlı** Elde edilen dosya en iyi şekilde sıkıştırılmasa bile, sıkıştırma işleminin mümkün olduğunca hızlı bir şekilde tamamlanmalıdır.<br>- **En iyi**: İşlemin tamamlanmasını daha uzun sürse bile sıkıştırma işleminin en iyi şekilde sıkıştırılması gerekir. Daha fazla bilgi için [sıkıştırma düzeyi](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) konu. | Hayır       |

Azure Blob depolamada JSON veri kümesinin bir örneği aşağıda verilmiştir:

```json
{
    "name": "JSONDataset",
    "properties": {
        "type": "Json",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compression": {
                "type": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, JSON kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="json-as-source"></a>Kaynak olarak JSON

Aşağıdaki özellikler, etkinlik ***\*kaynağını\**** kopyalama bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağının Type özelliği **Jsonsource**olarak ayarlanmalıdır. | Evet      |
| storeSettings | Veri deposundan veri okuma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının, altında `storeSettings`kendi desteklenen okuma ayarları vardır. **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

### <a name="json-as-sink"></a>Havuz olarak JSON

Aşağıdaki özellikler, etkinlik ***\*havuzunu\**** Kopyala bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağının Type özelliği **Jsonsink**olarak ayarlanmalıdır. | Evet      |
| formatSettings | Bir özellik grubu. Aşağıdaki **JSON yazma ayarları** tablosuna bakın. | Hayır       |
| storeSettings | Veri deposuna veri yazma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen yazma ayarları vardır. **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

Altında`formatSettings`desteklenen **JSON yazma ayarları** :

| Özellik      | Açıklama                                                  | Gerekli                                              |
| ------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| type          | FormatSettings türü **Jsonwritesetting**olarak ayarlanmalıdır. | Evet                                                   |
| filePattern |Her bir JSON dosyasında depolanan verilerin desenini belirtir. İzin verilen değerler: **setOfObjects** ve **arrayOfObjects**. **Varsayılan** değer **setOfObjects** olarak belirlenmiştir. Bu desenler hakkında ayrıntılı bilgi için bkz. [JSON dosyası desenleri](#json-file-patterns). |Hayır |

### <a name="json-file-patterns"></a>JSON dosyası desenleri

Kopyalama etkinliği, aşağıdaki JSON dosyası desenlerini otomatik olarak algılayabilir ve ayrıştırılabilir. 

- **1. Tür: setOfObjects**

    Her dosya tek bir nesne veya satırlara ayrılmış/bitiştirilmiş birden fazla nesne içerir. 
    Kopyalama etkinliği havuzunda bu seçenek seçildiğinde, kopyalama etkinliği her satır için her bir nesne (satır için ayrılmış) ile tek bir JSON dosyası üretir.

    * **tek nesne JSON örneği**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * **satırlara ayrılmış JSON örneği**

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * **bitiştirilmiş JSON örneği**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- **2. Tür: arrayOfObjects**

    Her dosya bir nesne dizisi içerir.

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
