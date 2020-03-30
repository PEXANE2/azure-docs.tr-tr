---
title: Azure Veri Fabrikası'nda ORC biçimi
description: Bu konu, Azure Veri Fabrikası'nda ORC biçimiyle nasıl başa çıkılabildiğini açıklar.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: jingwang
ms.openlocfilehash: e104c4c8e976207859b75212d5406558f04c6377
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597499"
---
# <a name="orc-format-in-azure-data-factory"></a>Azure Veri Fabrikası'nda ORC biçimi

**ORC dosyalarını ayrışdırmak veya verileri ORC biçimine yazmak**istediğinizde bu makaleyi izleyin. 

ORC biçimi aşağıdaki bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Veri Gölü Depolama Gen1](connector-azure-data-lake-store.md), [Azure Veri Gölü Depolama Gen2](connector-azure-data-lake-storage.md), [Azure Dosya Depolama](connector-azure-file-storage.md), Dosya [Sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Bulut Depolama](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md), ve [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, ORC veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Veri kümesinin tür özelliği **Orc**olarak ayarlanmalıdır. | Evet      |
| location         | Dosya(lar) konum ayarları. Her dosya tabanlı bağlayıcının kendi konum türü ve `location`desteklenen özellikleri vardır. **Bağlayıcı makalesindeki ayrıntılara bakın -> Dataset özellikleri bölümünde.** | Evet      |

Aşağıda Azure Blob Depolama ORC veri kümesi bir örnektir:

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
* Sütun adındaki beyaz alan desteklenmez.
* ORC dosyası [sıkıştırmayla ilgili üç seçeneğe sahiptir](https://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY. Data Factory, bu sıkıştırma biçimlerinin herhangi birine sahip ORC dosyalarını okuyabilir. Verileri okumak için meta verilerdeki sıkıştırma kodlayıcısı/kod çözücüsünü kullanır. Ancak Data Factory bir ORC dosyasına yazarken varsayılan ORC değeri olan ZLIB seçeneğini kullanır. Şu anda bu davranışı geçersiz kılma seçeneği yoktur.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, ORC kaynağı ve lavabo tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="orc-as-source"></a>Kaynak olarak ORC

Aşağıdaki özellikler kopyalama etkinliği *** \*kaynak\* *** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinlik kaynağının tür özelliği **OrcSource**olarak ayarlanmalıdır. | Evet      |
| mağazaAyarları | Veri deposundan verilerin nasıl okunacaklarına ilişkin özellikler grubu. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen okuma ayarları vardır. **Bağlayıcı makaledeki ayrıntılara bakın -> Etkinlik özelliklerini kopyala bölümünden kopyalayın.** | Hayır       |

### <a name="orc-as-sink"></a>LAVABO OLARAK ORC

Aşağıdaki özellikler kopyalama etkinliği *** \*lavabo\* *** bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Kopyalama etkinlik kaynağının tür özelliği **OrcSink**olarak ayarlanmalıdır. | Evet      |
| mağazaAyarları | Veri deposuna nasıl veri yazılailgili bir grup özellik. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen yazma ayarları vardır. **Bağlayıcı makaledeki ayrıntılara bakın -> Etkinlik özelliklerini kopyala bölümünden kopyalayın.** | Hayır       |

## <a name="using-self-hosted-integration-runtime"></a>Kendi barındırılan Tümleştirme Çalışma Süresini Kullanma

> [!IMPORTANT]
> Kendi kendine barındırılan Tümleştirme Runtime tarafından yetkilendirilmiş kopya için, örneğin şirket içi ve bulut veri depoları arasında, ORC dosyalarını **olduğu gibi**kopyalamazsanız, IR makinenize **64 bit JRE 8 (Java Runtime Environment) veya OpenJDK** ve **Microsoft Visual C++ 2010 Yeniden Dağıtılabilir Paketi** yüklemeniz gerekir. Aşağıdaki paragrafı daha fazla ayrıntıyla birlikte kontrol edin.

ORC dosya serileştirme/deserialization ile Self barındırılan IR üzerinde çalışan kopya için, ADF *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* ilk olarak JRE için kayıt defterini *`JAVA_HOME`* kontrol ederek Java çalışma süresini bulur, eğer bulunmazsa, ikinci olarak OpenJDK için sistem değişkenini denetler.

- **JRE kullanmak için**: 64-bit IR 64-bit JRE gerektirir. [Buradan](https://go.microsoft.com/fwlink/?LinkId=808605)bulabilirsin.
- **OpenJDK kullanmak için**: IR sürüm 3.13'ten beri desteklenir. OpenJDK'nın diğer tüm gerekli montajlarını kendi kendine barındırılan IR makinesine paketleyip, sistem ortamını buna göre JAVA_HOME ayarlayın.
- **Visual C++ 2010 Yeniden Dağıtılabilir Paket yüklemek için**: Visual C++ 2010 Yeniden Dağıtılabilir Paket, kendi kendine barındırılan IR kurulumları ile yüklenmez. [Buradan](https://www.microsoft.com/download/details.aspx?id=14632)bulabilirsin.

> [!TIP]
> Kendi kendine barındırılan Tümleştirme Runtime kullanarak ORC formatına/orc formatına veri kopyalarsanız ve "Java, mesaj: **java.lang.OutOfMemoryError:Java yığın alanı"** çağırıldığında bir hata oluştu" diyerek hatayı vurursanız, jvm'nin bu tür kopyalamayı güçlendirmek için min/max heap boyutunu ayarlamak için kendi kendine barındırılan IR'yi barındıran makineye bir ortam değişkeni `_JAVA_OPTIONS` ekleyebilirsiniz, ardından ardışık ardışık ardışık lığı yeniden çalıştırabilirsiniz.

![Kendi kendine barındırılan IR'de JVM yığın boyutunu ayarlama](./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Örnek: değer `_JAVA_OPTIONS` `-Xms256m -Xmx16g`ile değişken ayarlayın. Bayrak, `Xms` java sanal makinesi (JVM) için ilk bellek `Xmx` ayırma havuzunu belirtirken, en yüksek bellek ayırma havuzunu belirtir. Bu, JVM'nin bellek `Xms` miktarıyla başlatılacağı ve maksimum `Xmx` bellek miktarı kullanabileceği anlamına gelir. Varsayılan olarak, ADF min 64 MB ve max 1G kullanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
