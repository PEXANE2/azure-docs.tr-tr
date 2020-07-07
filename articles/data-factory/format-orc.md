---
title: Azure Data Factory için ORC biçimi
description: Bu konu, Azure Data Factory ' de ORC biçimi ile nasıl başa çıkılacağını açıklamaktadır.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: jingwang
ms.openlocfilehash: 9b68d3724c6390fc5d30745924451e27ef9855b3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81417734"
---
# <a name="orc-format-in-azure-data-factory"></a>Azure Data Factory için ORC biçimi

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

**Orc dosyalarını ayrıştırmak veya VERILERI orc biçimine yazmak**istediğinizde bu makaleye uyun. 

ORC biçimi şu bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md) [,,,](connector-hdfs.md) [http](connector-http.md)ve [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm ORC veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| tür             | Veri kümesinin Type özelliği **orc**olarak ayarlanmalıdır. | Yes      |
| location         | Dosya (ler) in konum ayarları. Her dosya tabanlı bağlayıcının, altında kendi konum türü ve desteklenen özellikleri vardır `location` . **Bağlayıcı makalesi-> veri kümesi özellikleri bölümünde ayrıntılara bakın**. | Yes      |

Azure Blob depolamada ORC veri kümesinin bir örneği aşağıda verilmiştir:

```json
{
    "name": "ORCDataset",
    "properties": {
        "type": "Orc",
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
            }
        }
    }
}
```

Aşağıdaki noktalara dikkat edin:

* Karmaşık veri türleri desteklenmez (STRUCT, MAP, LIST, UNION).
* Sütun adında boşluk desteklenmiyor.
* ORC dosyası [sıkıştırmayla ilgili üç seçeneğe sahiptir](https://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY. Data Factory, bu sıkıştırma biçimlerinin herhangi birine sahip ORC dosyalarını okuyabilir. Verileri okumak için meta verilerdeki sıkıştırma kodlayıcısı/kod çözücüsünü kullanır. Ancak Data Factory bir ORC dosyasına yazarken varsayılan ORC değeri olan ZLIB seçeneğini kullanır. Şu anda bu davranışı geçersiz kılma seçeneği yoktur.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, ORC kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="orc-as-source"></a>ORC as kaynağı

Aşağıdaki özellikler, etkinlik *** \* kaynağını \* *** kopyalama bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| tür          | Kopyalama etkinliği kaynağının Type özelliği **Orcsource**olarak ayarlanmalıdır. | Yes      |
| storeSettings | Veri deposundan veri okuma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının, altında kendi desteklenen okuma ayarları vardır `storeSettings` . **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | No       |

### <a name="orc-as-sink"></a>Havuz olarak ORC

Aşağıdaki özellikler, etkinlik *** \* havuzunu \* *** Kopyala bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| tür          | Kopyalama etkinliği kaynağının Type özelliği **Orcsink**olarak ayarlanmalıdır. | Yes      |
| storeSettings | Veri deposuna veri yazma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının altında kendi desteklenen yazma ayarları vardır `storeSettings` . **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın**. | No       |

## <a name="using-self-hosted-integration-runtime"></a>Şirket içinde barındırılan Integration Runtime kullanma

> [!IMPORTANT]
> Şirket içinde barındırılan Integration Runtime, örneğin şirket içi ve bulut veri depoları arasında geçiş için, ORC dosyalarını **olduğu gibi**KOPYALAMADıYSANıZ, ır makinenizde **64 bit JRE 8 (Java Runtime Environment) veya OpenJDK** ve **Microsoft Visual C++ 2010 yeniden dağıtılabilir paketini** yüklemeniz gerekir. Daha ayrıntılı bilgi edinmek için aşağıdaki paragrafı inceleyin.

ORC dosya serileştirme/seri hale getirme ile şirket içinde barındırılan IR üzerinde çalışan kopyalama için, ADF, Visual x + s için kayıt defterini denetleyerek, *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* OpenJDK için sistem değişkenini belirterek, Java çalışma zamanını bulur *`JAVA_HOME`* .

- **JRE 'yi kullanmak için**: 64 bit ır, 64 BIT JRE gerektirir. [Buradan](https://go.microsoft.com/fwlink/?LinkId=808605)bulabilirsiniz.
- **OpenJDK 'yi kullanmak için**: ır sürüm 3,13 ' den itibaren desteklenmektedir. OpenJDK 'nin diğer tüm gerekli Derlemeleriyle jvm.dll, şirket içinde barındırılan IR makinesine paketleyin ve sistem ortam değişkeni JAVA_HOME uygun şekilde ayarlayın.
- **Visual C++ 2010 yeniden dağıtılabilir paketini yüklemek için**: Visual C++ 2010 yeniden dağıtılabilir paketi, şirket IÇINDE barındırılan IR yüklemeleri ile birlikte yüklenmez. [Buradan](https://www.microsoft.com/download/details.aspx?id=14632)bulabilirsiniz.

> [!TIP]
> Şirket içinde barındırılan Integration Runtime kullanarak ORC formatına/biçiminden veri kopyalarsanız ve "Java çağrılırken bir hata oluştu" hatası ile karşılaşırsanız, ileti: **Java. lang. OutOfMemoryError: Java yığın alanı**", `_JAVA_OPTIONS` Bu kopyayı güçlendiren JVM için Min/Max yığın boyutunu ayarlamak üzere ŞIRKET içinde barındırılan IR barındıran makineye bir ortam değişkeni ekleyebilirsiniz, sonra işlem hattını yeniden çalıştırın.

![Şirket içinde barındırılan IR 'de JVM yığın boyutunu ayarlama](./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Örnek: değişkeni `_JAVA_OPTIONS` değeri ile ayarlayın `-Xms256m -Xmx16g` . Bayrak, `Xms` bir Java sanal makinesi (JVM) için ilk bellek ayırma havuzunu belirtir, ancak `Xmx` en fazla bellek ayırma havuzunu belirtir. Bu, JVM 'nin bellek miktarı ile başlatıldığı `Xms` ve en fazla bellek miktarını kullanabileceği anlamına gelir `Xmx` . ADF, varsayılan olarak en az 64 MB ve en fazla 1G kullanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
