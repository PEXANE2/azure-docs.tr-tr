---
title: Azure Data Factory içinde Parquet biçimi
description: Bu konu, Azure Data Factory içinde Parquet biçimiyle nasıl başa çıkılacağını açıklamaktadır.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 223b1b996b82acaa753eb55723e251dc5901bbec
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81417717"
---
# <a name="parquet-format-in-azure-data-factory"></a>Azure Data Factory içinde Parquet biçimi
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

**Parquet dosyalarını ayrıştırmak veya verileri Parquet biçimine yazmak**istediğinizde bu makaleyi izleyin. 

Parquet biçimi şu bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md) [,,](connector-hdfs.md), [http](connector-http.md)ve [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Parquet veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Veri kümesinin Type özelliği **Parquet**olarak ayarlanmalıdır. | Yes      |
| location         | Dosya (ler) in konum ayarları. Her dosya tabanlı bağlayıcının, altında `location`kendi konum türü ve desteklenen özellikleri vardır. **Bağlayıcı makalesi-> veri kümesi özellikleri bölümünde ayrıntılara bakın**. | Yes      |
| compressionCodec | Parquet dosyalarına yazılırken kullanılacak sıkıştırma codec bileşeni. Parquet dosyalarından okurken, veri fabrikaları dosya meta verilerine göre otomatik olarak sıkıştırma codec bileşenini otomatik olarak belirlenir.<br>Desteklenen türler şunlardır "**none**", "**gzip**", "**Snappy**" (varsayılan) ve "**LZO**". Şu anda kopyalama etkinliği, okuma/yazma Parquet dosyalarını okurken LZO 'yi desteklemez. | Hayır       |

> [!NOTE]
> Parquet dosyaları için sütun adında boşluk desteklenmez.

Azure Blob depolamada bir Parquet veri kümesinin örneği aşağıda verilmiştir:

```json
{
    "name": "ParquetDataset",
    "properties": {
        "type": "Parquet",
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
            "compressionCodec": "snappy"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Parquet kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="parquet-as-source"></a>Kaynak olarak Parquet

Aşağıdaki özellikler, etkinlik *** \*kaynağını\* *** kopyalama bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağının Type özelliği **Parquetsource**olarak ayarlanmalıdır. | Yes      |
| storeSettings | Veri deposundan veri okuma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının, altında `storeSettings`kendi desteklenen okuma ayarları vardır. **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

### <a name="parquet-as-sink"></a>Havuz olarak Parquet

Aşağıdaki özellikler, etkinlik *** \*havuzunu\* *** Kopyala bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağının Type özelliği **Parquetsink**olarak ayarlanmalıdır. | Yes      |
| storeSettings | Veri deposuna veri yazma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen yazma ayarları vardır. **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Eşleme veri akışındaki [kaynak dönüşümden](data-flow-source.md) ve [Havuz dönüşümünde](data-flow-sink.md) ayrıntıları öğrenin.

## <a name="data-type-support"></a>Veri türü desteği

Parquet karmaşık veri türleri şu anda desteklenmiyor (örn. MAP, LIST, STRUCT).

## <a name="using-self-hosted-integration-runtime"></a>Şirket içinde barındırılan Integration Runtime kullanma

> [!IMPORTANT]
> Şirket içinde barındırılan Integration Runtime, örneğin şirket içi ve bulut veri depoları arasında eşit olarak kopyalanmıyorsa, Parquet dosyalarını **olduğu gibi**KOPYALAMADıYSANıZ, ır makinenizde **64 bit JRE 8 (Java Runtime Environment) veya OpenJDK** ve **Microsoft Visual C++ 2010 yeniden dağıtılabilir paketini** yüklemeniz gerekir. Daha ayrıntılı bilgi edinmek için aşağıdaki paragrafı inceleyin.

Iquet dosya serileştirme/seri hale getirme ile şirket içinde barındırılan IR üzerinde çalışan kopya için, ADF, bir JRE için kayıt defterini *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* denetleyerek, OpenJDK için sistem değişkenini *`JAVA_HOME`* belirterek, Java çalışma zamanını konumlandırır.

- **JRE 'yi kullanmak için**: 64 bit ır, 64 BIT JRE gerektirir. [Buradan](https://go.microsoft.com/fwlink/?LinkId=808605)bulabilirsiniz.
- **OpenJDK 'yi kullanmak için**: ır sürüm 3,13 ' den itibaren desteklenmektedir. JVM. dll dosyasını, OpenJDK 'nin diğer tüm gerekli Derlemeleriyle şirket içinde barındırılan IR makinesine paketleyin ve sistem ortam değişkeni JAVA_HOME uygun şekilde ayarlayın.
- **Visual C++ 2010 yeniden dağıtılabilir paketini yüklemek için**: Visual C++ 2010 yeniden dağıtılabilir paketi, şirket IÇINDE barındırılan IR yüklemeleri ile birlikte yüklenmez. [Buradan](https://www.microsoft.com/download/details.aspx?id=14632)bulabilirsiniz.

> [!TIP]
> Şirket içinde barındırılan Integration Runtime kullanarak Parquet biçimine/kaynağından veri kopyalarsanız ve "Java çağrılırken bir hata oluştu" hatası ile karşılaşırsanız, ileti: **Java. lang. OutOfMemoryError: Java yığın alanı**", bu kopyayı GÜÇLENDIREN JVM için Min/ `_JAVA_OPTIONS` Max yığın boyutunu ayarlamak üzere şirket içinde barındırılan IR barındıran makineye bir ortam değişkeni ekleyebilirsiniz, sonra işlem hattını yeniden çalıştırın.

![Şirket içinde barındırılan IR 'de JVM yığın boyutunu ayarlama](./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Örnek: değişkeni `_JAVA_OPTIONS` değeri `-Xms256m -Xmx16g`ile ayarlayın. Bayrak `Xms` , bir Java sanal makinesi (JVM) için ilk bellek ayırma havuzunu belirtir, ancak `Xmx` en fazla bellek ayırma havuzunu belirtir. Bu, JVM 'nin bellek miktarı ile `Xms` başlatıldığı ve en fazla bellek `Xmx` miktarını kullanabileceği anlamına gelir. ADF, varsayılan olarak en az 64 MB ve en fazla 1G kullanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
