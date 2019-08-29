---
title: Azure Data Factory ikili biçimi | Microsoft Docs
description: Bu konu başlığı altında, Azure Data Factory Ikili biçimi nasıl ele alınacağını açıklamaktadır.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: jingwang
ms.openlocfilehash: a3b3e4e873497b3b0f726d815a798919ace4f931
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68842549"
---
# <a name="binary-format-in-azure-data-factory"></a>Azure Data Factory ikili biçimi

İkili biçimi aşağıdaki bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [azure blob](connector-azure-blob-storage.md), [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [](connector-hdfs.md),, [http](connector-http.md)ve [SFTP](connector-sftp.md).

[Copy etkinliğinde](copy-activity-overview.md), [GetMetadata etkinliğinde](control-flow-get-metadata-activity.md)veya [Delete etkinliğinde](delete-activity.md)ikili veri kümesini kullanabilirsiniz. ADF, Ikili veri kümesi kullanırken dosya içeriğini ayrıştırmaz ancak olduğu gibi kabul etmez. 

>[!NOTE]
>Copy etkinliğinde binary veri kümesini kullanırken, yalnızca Ikili veri kümesinden Ikili veri kümesine kopyalayabilirsiniz.

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Ikili veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Veri kümesinin Type özelliği **binary**olarak ayarlanmalıdır. | Evet      |
| location         | Dosya (ler) in konum ayarları. Her dosya tabanlı bağlayıcının, altında `location`kendi konum türü ve desteklenen özellikleri vardır. **Bağlayıcı makalesi-> veri kümesi özellikleri bölümünde ayrıntılara bakın**. | Evet      |
| compression | Dosya sıkıştırmayı yapılandırmak için özellik grubu. Etkinlik yürütmesi sırasında sıkıştırma/açma işlemi yapmak istediğinizde bu bölümü yapılandırın. | Hayır |
| type | İkili dosyaları okumak/yazmak için kullanılan sıkıştırma codec bileşeni. <br>İzin verilen değerler **bzip2**, **gzip**, **söndür**, **zipsöndür**. dosyasını kaydederken kullanmak için. | Hayır       |
| level | Sıkıştırma oranı. Kopyalama etkinliği havuzunda veri kümesi kullanıldığında Uygula.<br>İzin verilen değerler **en iyi** veya **en hızlardır**.<br>- **En hızlı** Elde edilen dosya en iyi şekilde sıkıştırılmasa bile, sıkıştırma işleminin mümkün olduğunca hızlı bir şekilde tamamlanmalıdır.<br>- **En iyi**: İşlemin tamamlanmasını daha uzun sürse bile sıkıştırma işleminin en iyi şekilde sıkıştırılması gerekir. Daha fazla bilgi için [sıkıştırma düzeyi](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) konu. | Hayır       |

Aşağıda, Azure Blob depolamada bir Ikili veri kümesi örneği verilmiştir:

```json
{
    "name": "BinaryDataset",
    "properties": {
        "type": "Binary",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compression": {
                "type": "ZipDeflate"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Ikili kaynak ve havuz tarafından desteklenen özelliklerin bir listesini sağlar.

>[!NOTE]
>Copy etkinliğinde binary veri kümesini kullanırken, yalnızca Ikili veri kümesinden Ikili veri kümesine kopyalayabilirsiniz.

### <a name="binary-as-source"></a>Kaynak olarak ikili

Aşağıdaki özellikler, etkinlik ***\*kaynağını\**** kopyalama bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağının Type özelliği **Binarysource**olarak ayarlanmalıdır. | Evet      |
| storeSettings | Veri deposundan veri okuma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının, altında `storeSettings`kendi desteklenen okuma ayarları vardır. **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

### <a name="binary-as-sink"></a>Havuz olarak ikili

Aşağıdaki özellikler, etkinlik ***\*havuzunu\**** Kopyala bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağının Type özelliği **Binarysink**olarak ayarlanmalıdır. | Evet      |
| storeSettings | Veri deposuna veri yazma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen yazma ayarları vardır. **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
- [Etkinliği sil](delete-activity.md)
