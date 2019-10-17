---
title: Azure Data Factory | için Parquet biçimi | Microsoft Docs
description: Bu konu, Azure Data Factory içinde Parquet biçimiyle nasıl başa çıkılacağını açıklamaktadır.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 92950a79be43bc656d50e2ced106a2fdb47f53bf
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387718"
---
# <a name="parquet-format-in-azure-data-factory"></a>Azure Data Factory içinde Parquet biçimi

**Parquet dosyalarını ayrıştırmak veya verileri Parquet biçimine yazmak**istediğinizde bu makaleyi izleyin. 

Parquet biçimi şu bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Bulut depolama](connector-google-cloud-storage.md), [,](connector-hdfs.md), [http](connector-http.md)ve [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Parquet veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik         | Açıklama                                                  | Gereklidir |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Veri kümesinin Type özelliği **Parquet**olarak ayarlanmalıdır. | Yes      |
| location         | Dosya (ler) in konum ayarları. Her dosya tabanlı bağlayıcının kendi konum türü ve `location` altında desteklenen özellikleri vardır. **Bağlayıcı makalesi-> veri kümesi özellikleri bölümünde ayrıntılara bakın**. | Yes      |
| compressionCodec | Parquet dosyalarına yazılırken kullanılacak sıkıştırma codec bileşeni. Parquet dosyalarından okurken, dosya meta verileri temelinde sıkıştırma codec bileşenini otomatik olarak belirleme Data Factory.<br>Desteklenen türler şunlardır "**none**", "**gzip**", "**Snappy**" (varsayılan) ve "**LZO**". Şu anda kopyalama etkinliği LZO 'yi desteklemez. | Hayır       |

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

Kopyalama etkinliği ***\*kaynak @ no__t-2*** bölümünde aşağıdaki özellikler desteklenir.

| Özellik      | Açıklama                                                  | Gereklidir |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağının Type özelliği **Parquetsource**olarak ayarlanmalıdır. | Yes      |
| storeSettings | Veri deposundan veri okuma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının, `storeSettings` altında kendi desteklenen okuma ayarları vardır. **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

### <a name="parquet-as-sink"></a>Havuz olarak Parquet

Kopyalama etkinliği ***\*sink @ no__t-2*** bölümünde aşağıdaki özellikler desteklenir.

| Özellik      | Açıklama                                                  | Gereklidir |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinliği kaynağının Type özelliği **Parquetsink**olarak ayarlanmalıdır. | Yes      |
| storeSettings | Veri deposuna veri yazma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının, `storeSettings` altında kendi desteklenen yazma ayarları vardır. **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | Hayır       |

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Eşleme veri akışındaki [kaynak dönüşümden](data-flow-source.md) ve [Havuz dönüşümünde](data-flow-sink.md) ayrıntıları öğrenin.

## <a name="data-type-support"></a>Veri türü desteği

Parquet karmaşık veri türleri şu anda desteklenmiyor (örn. MAP, LIST, STRUCT).

## <a name="using-self-hosted-integration-runtime"></a>Şirket içinde barındırılan Integration Runtime kullanma

> [!IMPORTANT]
> Şirket içinde barındırılan Integration Runtime, örneğin şirket içi ve bulut veri depoları arasında eşit olarak kopyalanmıyorsa, Parquet dosyalarını **olduğu gibi**KOPYALAMADıYSANıZ, ır makinenize **64 bit JRE 8 (Java Runtime Environment) veya OpenJDK** yüklemeniz gerekir. Daha ayrıntılı bilgi için aşağıdaki paragrafa bakın.

Iquet dosya serileştirme/seri hale getirme ile şirket içinde barındırılan IR üzerinde çalışan kopyalama için, ADF, bir JRE için kayıt defteri *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* ' i seçerek Java çalışma zamanını bulur. bu bulunamazsa, OpenJDK için sistem değişkeni *`JAVA_HOME`* ' ü seçerek denetleyin

- **JRE 'yi kullanmak için**: 64 bit ır, 64 BIT JRE gerektirir. [Buradan](https://go.microsoft.com/fwlink/?LinkId=808605)bulabilirsiniz.
- **OpenJDK 'yi kullanmak için**: ır sürüm 3,13 ' den itibaren desteklenmektedir. JVM. dll dosyasını, OpenJDK 'nin diğer tüm gerekli Derlemeleriyle şirket içinde barındırılan IR makinesine paketleyin ve sistem ortam değişkenini JAVA_HOME göre ayarlayın.

> [!TIP]
> Şirket içinde barındırılan Integration Runtime kullanarak Parquet biçimine/kaynağından veri kopyalarsanız ve "Java çağrılırken bir hata oluştu, ileti: **Java. lang. OutOfMemoryError: Java yığın alanı**" olarak bir ortam @no__t değişkeni ekleyebilirsiniz. Bu kopyayı güçlendiren JVM için Min/Max yığın boyutunu ayarlamak üzere şirket içinde barındırılan IR 'yi barındıran makine işlem hattını yeniden çalıştırın.

![Şirket içinde barındırılan IR 'de JVM yığın boyutunu ayarlama](./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Örnek: `_JAVA_OPTIONS` değişkenini `-Xms256m -Xmx16g` değeriyle ayarlayın. @No__t-0 bayrağı, bir Java Sanal Makinesi (JVM) için ilk bellek ayırma havuzunu belirtir, ancak `Xmx` en fazla bellek ayırma havuzunu belirtir. Bu, JVM 'nin `Xms` bellek miktarıyla başlatılabileceği ve en fazla @no__t 1 bellek miktarını kullanabilecek anlamına gelir. ADF, varsayılan olarak en az 64MB ve en fazla 1G kullanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
