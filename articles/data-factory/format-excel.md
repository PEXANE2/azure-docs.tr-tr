---
title: Azure Data Factory Excel biçimi
description: Bu konu başlığı altında, Excel biçimi Azure Data Factory nasıl ele alınacağını açıklamaktadır.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/10/2020
ms.author: jingwang
ms.openlocfilehash: 8b4876377501209e19ac496d605d228208d2323d
ms.sourcegitcommit: eeba08c8eaa1d724635dcf3a5e931993c848c633
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84670925"
---
# <a name="excel-format-in-azure-data-factory"></a>Azure Data Factory Excel biçimi
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

**Excel dosyalarını ayrıştırmak**istediğinizde bu makaleye uyun. Azure Data Factory hem ". xls" hem de ". xlsx" destekler.

Excel biçimi şu bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md) [,,](connector-hdfs.md), [http](connector-http.md)ve [SFTP](connector-sftp.md). Kaynak olarak desteklenir, ancak havuz değildir.

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Excel veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| tür             | Veri kümesinin Type özelliği **Excel**olarak ayarlanmalıdır.   | Yes      |
| location         | Dosya (ler) in konum ayarları. Her dosya tabanlı bağlayıcının, altında kendi konum türü ve desteklenen özellikleri vardır `location` . | Yes      |
| sheetname ayar çağırın        | Verilerin okunacağı Excel çalışma sayfası adı.                       | Yes      |
| aralık            | Seçmeli verileri bulmak için belirtilen çalışma sayfasındaki hücre aralığı (örneğin, `A3:H5` a3 'Den H5 'e), ( `A3` a3 hücresinden başlayarak bir tablo), `A3:A3` (tek hücreli). Belirtilmemişse, ADF tüm çalışma sayfasından tablo olarak okur. | No       |
| firstRowAsHeader | Belirtilen çalışma sayfasındaki/aralıktaki ilk satırın, sütun adlarıyla üst bilgi satırı olarak değerlendirilip değerlendirilmeyeceğini belirtir.<br>İzin verilen değerler **true** ve **false** (varsayılan) şeklindedir. | No       |
| nullValue        | Null değerin dize gösterimini belirtir. <br>Varsayılan değer boş bir **dizedir**. | No       |
| compressionCodec | Excel dosyalarını okumak için kullanılan sıkıştırma codec bileşeni. <br>İzin verilen değerler şunlardır **bzip2**, **gzip**, **söndür**, **zipsöndür**, **Snappy**veya **lz4**. Varsayılan değer sıkıştırılmaz. <br>**Şu anda** kopyalama etkinliği "Snappy" & "lz4" desteklemez ve eşleme veri akışı "zipsöndür" seçeneğini desteklemez. <br>Bu **dosyaları açmak ve dosya tabanlı** havuz veri deposuna yazmak için kopyalama **etkinliğini kullanırken dosyalar** klasörüne ayıklanır: `<path specified in dataset>/<folder named as source zip file>/` . | No       |
| compressionLevel | Sıkıştırma oranı. <br>İzin verilen değerler **en iyi** veya **en hızlardır**.<br>- **En hızlı:** Elde edilen dosya en iyi şekilde sıkıştırılmasa bile, sıkıştırma işleminin mümkün olduğunca hızlı bir şekilde tamamlanmalıdır.<br>- **En iyi**: işlemin tamamlanmasını daha uzun sürse bile sıkıştırma işlemi en iyi şekilde sıkıştırılmalıdır. Daha fazla bilgi için bkz. [sıkıştırma düzeyi](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) konusu. | No       |

Azure Blob depolamada Excel veri kümesinin bir örneği aşağıda verilmiştir:

```json
{
    "name": "ExcelDataset",
    "properties": {
        "type": "Excel",
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
            "sheetName": "MyWorksheet",
            "range": "A3:H5",
            "firstRowAsHeader": true
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Excel kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="excel-as-source"></a>Kaynak olarak Excel 

Aşağıdaki özellikler, etkinlik *** \* kaynağını \* *** kopyalama bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| tür          | Kopyalama etkinliği kaynağının Type özelliği **ExcelSource**olarak ayarlanmalıdır. | Yes      |
| storeSettings | Veri deposundan veri okuma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının, altında kendi desteklenen okuma ayarları vardır `storeSettings` . | Hayır       |

```json
"activities": [
    {
        "name": "CopyFromExcel",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "ExcelSource",
                "storeSettings": {
                    "type": "AzureBlobStorageReadSettings",
                    "recursive": true
                }
            },
            ...
        }
        ...
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
