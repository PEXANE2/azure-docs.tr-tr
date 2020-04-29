---
title: Azure Data Factory ikili biçimi
description: Bu konu başlığı altında, Azure Data Factory Ikili biçimi nasıl ele alınacağını açıklamaktadır.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: jingwang
ms.openlocfilehash: 4560560b3677030a66e277e96eb552d39f5c82c1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81416335"
---
# <a name="binary-format-in-azure-data-factory"></a>Azure Data Factory ikili biçimi
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

İkili biçimi şu bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md) [,,](connector-hdfs.md), [http](connector-http.md)ve [SFTP](connector-sftp.md).

[Copy etkinliğinde](copy-activity-overview.md), [GetMetadata etkinliğinde](control-flow-get-metadata-activity.md)veya [Delete etkinliğinde](delete-activity.md)ikili veri kümesini kullanabilirsiniz. ADF, Ikili veri kümesi kullanırken dosya içeriğini ayrıştırmaz ancak olduğu gibi kabul etmez. 

>[!NOTE]
>Copy etkinliğinde binary veri kümesini kullanırken, yalnızca Ikili veri kümesinden Ikili veri kümesine kopyalayabilirsiniz.

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Ikili veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Veri kümesinin Type özelliği **binary**olarak ayarlanmalıdır. | Yes      |
| location         | Dosya (ler) in konum ayarları. Her dosya tabanlı bağlayıcının, altında `location`kendi konum türü ve desteklenen özellikleri vardır. **Bağlayıcı makalesi-> veri kümesi özellikleri bölümünde ayrıntılara bakın**. | Yes      |
| sıkıştırma | Dosya sıkıştırmayı yapılandırmak için özellik grubu. Etkinlik yürütmesi sırasında sıkıştırma/açma işlemi yapmak istediğinizde bu bölümü yapılandırın. | Hayır |
| type | İkili dosyaları okumak/yazmak için kullanılan sıkıştırma codec bileşeni. <br>İzin verilen değerler **bzip2**, **gzip**, **söndür**, **zipsöndür**. dosyasını kaydederken kullanmak için.<br>Bu dosyaları açmak ve dosya tabanlı havuz veri deposuna yazmak için kopyalama etkinliğini kullanırken, dosyalar klasörüne ayıklanacaktır: `<path specified in dataset>/<folder named as source zip file>/`. | Hayır       |
| düzey | Sıkıştırma oranı. Kopyalama etkinliği havuzunda veri kümesi kullanıldığında Uygula.<br>İzin verilen değerler **en iyi** veya **en hızlardır**.<br>- **En hızlı:** Elde edilen dosya en iyi şekilde sıkıştırılmasa bile, sıkıştırma işleminin mümkün olduğunca hızlı bir şekilde tamamlanmalıdır.<br>- **En iyi**: işlemin tamamlanmasını daha uzun sürse bile sıkıştırma işlemi en iyi şekilde sıkıştırılmalıdır. Daha fazla bilgi için bkz. [sıkıştırma düzeyi](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) konusu. | Hayır       |

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

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Ikili kaynak ve havuz tarafından desteklenen özelliklerin bir listesini sağlar.

>[!NOTE]
>Copy etkinliğinde binary veri kümesini kullanırken, yalnızca Ikili veri kümesinden Ikili veri kümesine kopyalayabilirsiniz.

### <a name="binary-as-source"></a>Kaynak olarak ikili

Aşağıdaki özellikler, etkinlik *** \*kaynağını\* *** kopyalama bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağının Type özelliği **Binarysource**olarak ayarlanmalıdır. | Yes      |
| storeSettings | Veri deposundan veri okuma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının, altında `storeSettings`kendi desteklenen okuma ayarları vardır. **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

### <a name="binary-as-sink"></a>Havuz olarak ikili

Aşağıdaki özellikler, etkinlik *** \*havuzunu\* *** Kopyala bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağının Type özelliği **Binarysink**olarak ayarlanmalıdır. | Yes      |
| storeSettings | Veri deposuna veri yazma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen yazma ayarları vardır. **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
- [Etkinliği sil](delete-activity.md)
