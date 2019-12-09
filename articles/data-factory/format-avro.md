---
title: Azure Data Factory avro biçimi
description: Bu konu, Azure Data Factory ' de avro biçimi ile nasıl başa çıkılacağını açıklamaktadır.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: jingwang
ms.openlocfilehash: 9e962a0e76cdc0d51a87df3c33927c34db991fc7
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74927406"
---
# <a name="avro-format-in-azure-data-factory"></a>Azure Data Factory avro biçimi

**Avro dosyalarını ayrıştırmak veya verileri avro biçimine yazmak**istediğinizde bu makaleye uyun. 

Avro biçimi şu bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md) [,,](connector-hdfs.md), [http](connector-http.md)ve [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, avro veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik         | Açıklama                                                  | Gereklidir |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Veri kümesinin Type özelliği **avro**olarak ayarlanmalıdır. | Yes      |
| location         | Dosya (ler) in konum ayarları. Her dosya tabanlı bağlayıcının kendi konum türü ve `location`altında desteklenen özellikleri vardır. **Bağlayıcı makalesi-> veri kümesi özellikleri bölümünde ayrıntılara bakın**. | Yes      |
| avroCompressionCodec | Avro dosyalarına yazılırken kullanılacak sıkıştırma codec bileşeni. Avro dosyalarından okurken, dosya meta verileri temelinde sıkıştırma codec bileşenini otomatik olarak belirleme Data Factory.<br>Desteklenen türler şunlardır "**none**" (varsayılan), "**söndür**", "**Snappy**". | Hayır       |

> [!NOTE]
> Sütun adında boşluk, avro dosyaları için desteklenmez.

Aşağıda, Azure Blob depolamada bir avro veri kümesi örneği verilmiştir:

```json
{
    "name": "AvroDataset",
    "properties": {
        "type": "Avro",
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
            "avroCompressionCodec": "snappy"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, avro kaynak ve havuz tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="avro-as-source"></a>Kaynak olarak avro

Aşağıdaki özellikler, kopyalama etkinliği ***\*kaynak\**** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gereklidir |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağının Type özelliği **Avrosource**olarak ayarlanmalıdır. | Yes      |
| storeSettings | Veri deposundan veri okuma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının, `storeSettings`altında kendi desteklenen okuma ayarları vardır. **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

### <a name="avro-as-sink"></a>Avro as havuzu

Aşağıdaki özellikler, kopyalama etkinliği ***\*havuzu\**** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gereklidir |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağının Type özelliği **Avrosink**olarak ayarlanmalıdır. | Yes      |
| storeSettings | Veri deposuna veri yazma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının `storeSettings`altında kendi desteklenen yazma ayarları vardır. **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

## <a name="data-type-support"></a>Veri türü desteği

Avro [karmaşık veri türleri](https://avro.apache.org/docs/current/spec.html#schema_complex) desteklenmez (kayıtlar, numaralandırmalar, diziler, Haritalar, birleşimler ve sabit).

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
