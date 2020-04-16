---
title: Azure Veri Fabrikası'nda ikili biçim
description: Bu konu, Azure Veri Fabrikası'nda İkili biçimle nasıl başa çıkılabildiğini açıklar.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: jingwang
ms.openlocfilehash: 4560560b3677030a66e277e96eb552d39f5c82c1
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416335"
---
# <a name="binary-format-in-azure-data-factory"></a>Azure Veri Fabrikası'nda ikili biçim
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

İkili biçim aşağıdaki bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Veri Gölü Depolama Gen1](connector-azure-data-lake-store.md), [Azure Veri Gölü Depolama Gen2](connector-azure-data-lake-storage.md), [Azure Dosya Depolama](connector-azure-file-storage.md), Dosya [Sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Bulut Depolama](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md), ve [SFTP](connector-sftp.md).

İkili veri kümesini [Kopyalama etkinliğinde,](copy-activity-overview.md) [GetMetadata etkinliğinde](control-flow-get-metadata-activity.md)veya [Etkinliği Sil'de](delete-activity.md)kullanabilirsiniz. İkili veri kümesini kullanırken, ADF dosya içeriğini ayrıştırmaz, olduğu gibi ele almaz. 

>[!NOTE]
>İkili veri kümesini kopyalama etkinliğinde kullanırken, yalnızca İkili veri kümesinden İkili veri kümesine kopyalayabilirsiniz.

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, İkili veri kümesi tarafından desteklenen özelliklerin bir listesi sağlar.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Veri kümesinin tür özelliği **İkili**olarak ayarlanmalıdır. | Evet      |
| location         | Dosya(lar) konum ayarları. Her dosya tabanlı bağlayıcının kendi konum türü ve `location`desteklenen özellikleri vardır. **Bağlayıcı makalesindeki ayrıntılara bakın -> Dataset özellikleri bölümünde.** | Evet      |
| sıkıştırma | Dosya sıkıştırma yapılandırmak için özellikleri grubu. Etkinlik yürütme sırasında sıkıştırma/dekompresyon yapmak istediğinizde bu bölümü yapılandırın. | Hayır |
| type | İkili dosyaları okumak/yazmak için kullanılan sıkıştırma codec'i. <br>İzin verilen değerler **bzip2,** **gzip**, **deflate**, **ZipDeflate**vardır. dosyayı kaydederken kullanmak için.<br>ZipDeflate dosya(lar) decompress ve dosya tabanlı lavabo veri deposuna yazmak için kopyalama etkinliği kullanırken not, dosyalar klasöre ayıklanır: `<path specified in dataset>/<folder named as source zip file>/`. | Hayır       |
| düzey | Sıkıştırma oranı. Veri kümesi Kopyalama etkinliği batarken kullanılırsa uygulayın.<br>İzin verilen değerler **En Uygun** veya **En Hızlı**değerlerdir.<br>- **En hızlı:** Elde edilen dosya en iyi şekilde sıkıştırılmamış olsa bile sıkıştırma işlemi mümkün olan en kısa sürede tamamlanmalıdır.<br>- **Optimal**: Sıkıştırma işlemi, operasyonun tamamlanması daha uzun sürse bile en iyi şekilde sıkıştırılmalıdır. Daha fazla bilgi için [Sıkıştırma Düzeyi](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) konusuna bakın. | Hayır       |

Aşağıda Azure Blob Depolama İkili veri kümesi bir örnektir:

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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, İkili kaynak ve lavabo tarafından desteklenen özelliklerin bir listesini sağlar.

>[!NOTE]
>İkili veri kümesini kopyalama etkinliğinde kullanırken, yalnızca İkili veri kümesinden İkili veri kümesine kopyalayabilirsiniz.

### <a name="binary-as-source"></a>Kaynak olarak ikili

Aşağıdaki özellikler kopyalama etkinliği *** \*kaynak\* *** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinlik kaynağının türü özelliği **BinarySource**olarak ayarlanmalıdır. | Evet      |
| mağazaAyarları | Veri deposundan verilerin nasıl okunacaklarına ilişkin özellikler grubu. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen okuma ayarları vardır. **Bağlayıcı makaledeki ayrıntılara bakın -> Etkinlik özelliklerini kopyala bölümünden kopyalayın.** | Hayır       |

### <a name="binary-as-sink"></a>Lavabo olarak ikili

Aşağıdaki özellikler kopyalama etkinliği *** \*lavabo\* *** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinlik kaynağının türü özelliği **BinarySink**olarak ayarlanmalıdır. | Evet      |
| mağazaAyarları | Veri deposuna nasıl veri yazılailgili bir grup özellik. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen yazma ayarları vardır. **Bağlayıcı makaledeki ayrıntılara bakın -> Etkinlik özelliklerini kopyala bölümünden kopyalayın.** | Hayır       |

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
- [Etkinliği silme](delete-activity.md)
