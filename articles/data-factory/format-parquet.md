---
title: Azure Veri Fabrikası'nda parke formatı
description: Bu konu, Azure Veri Fabrikası'nda Parke formatıyla nasıl başa çıkılabildiğini açıklar.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 223b1b996b82acaa753eb55723e251dc5901bbec
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417717"
---
# <a name="parquet-format-in-azure-data-factory"></a>Azure Veri Fabrikası'nda parke formatı
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

**Parke dosyalarını ayrışdırmak veya verileri Parke formatına yazmak**istediğinizde bu makaleyi takip edin. 

Parke formatı aşağıdaki bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Veri Gölü Depolama Gen1](connector-azure-data-lake-store.md), [Azure Veri Gölü Depolama Gen2](connector-azure-data-lake-storage.md), [Azure Dosya Depolama](connector-azure-file-storage.md), Dosya [Sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Bulut Depolama](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md), ve [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde Parke veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Veri kümesinin tür özelliği **Parke**olarak ayarlanmalıdır. | Evet      |
| location         | Dosya(lar) konum ayarları. Her dosya tabanlı bağlayıcının kendi konum türü ve `location`desteklenen özellikleri vardır. **Bağlayıcı makalesindeki ayrıntılara bakın -> Dataset özellikleri bölümünde.** | Evet      |
| sıkıştırmaCodec | Parke dosyalarına yazarken kullanılacak sıkıştırma codec'i. Parke dosyalarından okurken, Veri Fabrikaları dosya meta verilerini temel alan sıkıştırma kodlarını otomatik olarak belirler.<br>Desteklenen türleri "**yok**", "**gzip**", "**snappy**" (varsayılan) ve "**lzo**". Not şu anda Parke dosyalarını okurken/yazarken Kopyalama etkinliği LZO'u desteklemez. | Hayır       |

> [!NOTE]
> Parke dosyaları için sütun adındaki beyaz boşluk desteklenmez.

Aşağıda Azure Blob Depolama Parke veri kümesi bir örnektir:

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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde Parke kaynağı ve lavabo tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="parquet-as-source"></a>Kaynak olarak parke

Aşağıdaki özellikler kopyalama etkinliği *** \*kaynak\* *** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinlik kaynağının türü özelliği **ParkquetSource**olarak ayarlanmalıdır. | Evet      |
| mağazaAyarları | Veri deposundan verilerin nasıl okunacaklarına ilişkin özellikler grubu. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen okuma ayarları vardır. **Bağlayıcı makaledeki ayrıntılara bakın -> Etkinlik özelliklerini kopyala bölümünden kopyalayın.** | Hayır       |

### <a name="parquet-as-sink"></a>Lavabo olarak parke

Aşağıdaki özellikler kopyalama etkinliği *** \*lavabo\* *** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinlik kaynağının türü özelliği **ParkquetSink**olarak ayarlanmalıdır. | Evet      |
| mağazaAyarları | Veri deposuna nasıl veri yazılailgili bir grup özellik. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen yazma ayarları vardır. **Bağlayıcı makaledeki ayrıntılara bakın -> Etkinlik özelliklerini kopyala bölümünden kopyalayın.** | Hayır       |

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Veri akışını eşlemede [kaynak dönüşümü](data-flow-source.md) ve [lavabo dönüşümünden](data-flow-sink.md) ayrıntıları öğrenin.

## <a name="data-type-support"></a>Veri türü desteği

Parke kompleksi veri türleri şu anda desteklenmez (örn. MAP, LIST, STRUCT).

## <a name="using-self-hosted-integration-runtime"></a>Kendi barındırılan Tümleştirme Çalışma Süresini Kullanma

> [!IMPORTANT]
> Kendi kendine barındırılan Tümleştirme Runtime tarafından yetkilendirilmiş kopya için, örneğin şirket içi ve bulut veri depoları arasında, Parke dosyalarını **olduğu gibi**kopyalamazsanız, **64 bit JRE 8 (Java Runtime Environment) veya OpenJDK** ve **Microsoft Visual C++ 2010 Yeniden Dağıtılabilir Paketi** IR makinenize yüklemeniz gerekir. Aşağıdaki paragrafı daha fazla ayrıntıyla birlikte kontrol edin.

Parke dosyası serileştirme/deserialization ile Self barındırılan IR üzerinde çalışan kopya için, ADF *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* ilk olarak JRE için kayıt defterikontrol *`JAVA_HOME`* ederek Java çalışma süresini bulur, eğer bulunmazsa, ikinci olarak OpenJDK için sistem değişkenini denetler.

- **JRE kullanmak için**: 64-bit IR 64-bit JRE gerektirir. [Buradan](https://go.microsoft.com/fwlink/?LinkId=808605)bulabilirsin.
- **OpenJDK kullanmak için**: IR sürüm 3.13'ten beri desteklenir. OpenJDK'nın diğer tüm gerekli montajlarını kendi kendine barındırılan IR makinesine paketleyip, sistem ortamını buna göre JAVA_HOME ayarlayın.
- **Visual C++ 2010 Yeniden Dağıtılabilir Paket yüklemek için**: Visual C++ 2010 Yeniden Dağıtılabilir Paket, kendi kendine barındırılan IR kurulumları ile yüklenmez. [Buradan](https://www.microsoft.com/download/details.aspx?id=14632)bulabilirsin.

> [!TIP]
> Kendi kendine barındırılan Tümleştirme Runtime kullanarak Parke formatına/parke formatından verileri kopyalarsanız ve "Java, mesaj: **java.lang.OutOfMemoryError:Java yığın alanı"** çağırıldığında bir hata oluştu" diyerek hatayı vurursanız, JVM'nin bu tür kopyalamayı güçlendirmek için min/max heap boyutunu ayarlamak için kendi kendine barındırılan IR'yi barındıran makineye bir ortam değişkeni `_JAVA_OPTIONS` ekleyebilirsiniz ve ardından ardışık ardışık hattı yeniden çalıştırabilirsiniz.

![Kendi kendine barındırılan IR'de JVM yığın boyutunu ayarlama](./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Örnek: değer `_JAVA_OPTIONS` `-Xms256m -Xmx16g`ile değişken ayarlayın. Bayrak, `Xms` java sanal makinesi (JVM) için ilk bellek `Xmx` ayırma havuzunu belirtirken, en yüksek bellek ayırma havuzunu belirtir. Bu, JVM'nin bellek `Xms` miktarıyla başlatılacağı ve maksimum `Xmx` bellek miktarı kullanabileceği anlamına gelir. Varsayılan olarak, ADF min 64 MB ve max 1G kullanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
