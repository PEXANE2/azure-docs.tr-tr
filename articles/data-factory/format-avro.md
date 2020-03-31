---
title: Azure Veri Fabrikası'nda Avro formatı
description: Bu konu, Azure Veri Fabrikası'nda Avro biçimiyle nasıl başa çıkılabildiğini açıklar.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: jingwang
ms.openlocfilehash: 1717969aeb24a153f986c70ef60db1aac5c840fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78267783"
---
# <a name="avro-format-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Avro formatı

**Avro dosyalarını ayrışturmak veya verileri Avro formatına yazmak**istediğinizde bu makaleyi izleyin. 

Avro formatı aşağıdaki bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Veri Gölü Depolama Gen1](connector-azure-data-lake-store.md), [Azure Veri Gölü Depolama Gen2](connector-azure-data-lake-storage.md), [Azure Dosya Depolama](connector-azure-file-storage.md), Dosya [Sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Bulut Depolama](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md), ve [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde Avro veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Veri kümesinin tür özelliği **Avro**olarak ayarlanmalıdır. | Evet      |
| location         | Dosya(lar) konum ayarları. Her dosya tabanlı bağlayıcının kendi konum türü ve `location`desteklenen özellikleri vardır. **Bağlayıcı makalesindeki ayrıntılara bakın -> Dataset özellikleri bölümünde.** | Evet      |
| avroCompressionCodec | Avro dosyalarına yazarken kullanılacak sıkıştırma codec'i. Avro dosyalarından okurken, Veri Fabrikası dosya meta verilerini temel alan sıkıştırma kodlarını otomatik olarak belirler.<br>Desteklenen türleri "**none**" (varsayılan), "**deflate**", "**snappy**". Not şu anda Avro dosyalarını okurken/yazarken Snappy'yi desteklemez. | Hayır       |

> [!NOTE]
> Avro dosyaları için sütun adındaki beyaz boşluk desteklenmez.

Aşağıda Azure Blob Depolama Avro veri kümesi bir örnektir:

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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde Avro kaynağı ve lavabo tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="avro-as-source"></a>Kaynak olarak Avro

Aşağıdaki özellikler kopyalama etkinliği *** \*kaynak\* *** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinlik kaynağının türü özelliği **AvroSource**olarak ayarlanmalıdır. | Evet      |
| mağazaAyarları | Veri deposundan verilerin nasıl okunacaklarına ilişkin özellikler grubu. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen okuma ayarları vardır. **Bağlayıcı makaledeki ayrıntılara bakın -> Etkinlik özelliklerini kopyala bölümünden kopyalayın.** | Hayır       |

### <a name="avro-as-sink"></a>Lavabo olarak Avro

Aşağıdaki özellikler kopyalama etkinliği *** \*lavabo\* *** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinlik kaynağının türü özelliği **AvroSink**olarak ayarlanmalıdır. | Evet      |
| mağazaAyarları | Veri deposuna nasıl veri yazılailgili bir grup özellik. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen yazma ayarları vardır. **Bağlayıcı makaledeki ayrıntılara bakın -> Etkinlik özelliklerini kopyala bölümünden kopyalayın.** | Hayır       |

## <a name="data-type-support"></a>Veri türü desteği

### <a name="copy-activity"></a>Kopyalama etkinliği
Avro [karmaşık veri türleri](https://avro.apache.org/docs/current/spec.html#schema_complex) Kopyalama Etkinliğinde desteklenmez (kayıtlar, enumlar, diziler, haritalar, sendikalar ve sabit).

### <a name="data-flows"></a>Veri akışları
Veri akışlarında Avro dosyalarıyla çalışırken, karmaşık veri türlerini okuyabilir ve yazabilirsiniz, ancak önce veri kümesinden fiziksel şemayı temizlediğinizden emin olun. Veri akışlarında, mantıksal projeksiyonunuzu ayarlayabilir ve karmaşık yapılar olan sütunlar türetebilir, ardından bu alanları bir Avro dosyasıyla otomatik olarak eşleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
