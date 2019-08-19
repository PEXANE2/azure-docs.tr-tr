---
title: Azure veri fabrikasında kopyalama etkinliği | Microsoft Docs
description: Azure Data Factory kopyalama etkinliği, verileri desteklenen kaynak veri deposundan desteklenen bir havuz veri deposuna kopyalamak için kullanabileceğiniz öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: a8265496c475566ec7a87a19eab6d975838e9da4
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68966400"
---
# <a name="copy-activity-in-azure-data-factory"></a>Azure veri fabrikasında kopyalama etkinliği

## <a name="overview"></a>Genel Bakış

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-data-movement-activities.md)
> * [Geçerli sürüm](copy-activity-overview.md)

Azure Data Factory'de arasında veri depoları şirket içindeki ve buluttaki veri kopyalamak için kopyalama etkinliği'ni kullanabilirsiniz. Veriler kopyalandıktan sonra başka etkinlikler kullanılarak daha fazla dönüştürülebilir ve analiz edilebilir. Kopyalama etkinliği, dönüştürme ve iş zekası (BI) ve uygulama tüketimini analiz sonuçları yayımlamak için de kullanabilirsiniz.

![Kopyalama etkinliği rolü](media/copy-activity-overview/copy-activity.png)

Kopyalama Etkinliği yürütüldüğünde bir [Integration Runtime](concepts-integration-runtime.md). Farklı veri kopyalama senaryosunda, Integration Runtime farklı türleri yararlanılabilir olabilir:

* Her iki IP 'den internet üzerinden herkese açık olan veri depoları arasında veri kopyalarken kopyalama etkinliği güvenli, güvenilir, ölçeklenebilir ve [küresel olarak kullanılabilir](concepts-integration-runtime.md#integration-runtime-location) **Azure Integration Runtime**tarafından güçlenebilir.
* Bulunan şirket içi verileri kopyalama/veri depolarına veya ayarlamak gereken erişim denetimi (örneğin, Azure sanal ağı) içeren bir ağda olduğunda bir **tümleşik çalışma zamanı barındırabileceğiniz** veri kopyalama olanağı.

Tümleştirme çalışma zamanı her kaynak ve havuz veri deposu ile ilişkilendirilmesi gerekir. Hakkında ayrıntılı bilgi edinin. kopyalama etkinliği [kullanılacak IR'yi belirler](concepts-integration-runtime.md#determining-which-ir-to-use).

Kopyalama etkinliği, verileri bir kaynaktan havuza kopyalamak için aşağıdaki aşamalara üzerinden gider. Kopyalama etkinliği'ni destekleyen hizmet:

1. Bir kaynak veri deposundan veri okur.
2. Serileştirme/seri durumundan çıkarma, sıkıştırma/açma, sütun eşleme, vb. gerçekleştirir. Bunu, giriş veri kümesi, çıktı veri kümesi ve kopyalama etkinliği yapılandırmalarına göre bu işlemleri yapar.
3. Veri havuz/hedef veri deposuna yazar.

![Kopyalama Etkinliği’ne Genel Bakış](media/copy-activity-overview/copy-activity-overview.png)

## <a name="supported-data-stores-and-formats"></a>Desteklenen veri depoları ve biçimler

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

### <a name="supported-file-formats"></a>Desteklenen dosya biçimleri

Kopyalama etkinliği için kullanabileceğiniz **olarak dosya kopyalama-olan** içinde çalışması verilerin kopyalandığı verimli bir şekilde tüm serileştirme/seri kaldırma, iki dosya tabanlı veri depoları arasında.

Kopyalama etkinliği, belirtilen biçimlerdeki dosyaları okumayı ve yazmayı de destekler: **Text, JSON, avro, ORC ve Parquet**, dosyaları aşağıdaki codec bileşenleriyle sıkıştırma ve açma: **Gzip, söndür, bzip2 ve Zipsöndür**. Bkz: [desteklenen dosya ve sıkıştırma biçimleri](supported-file-formats-and-compression-codecs.md) ayrıntılarla.

Örneğin, aşağıdaki kopyalama etkinlikleri yapabilirsiniz:

* Verileri şirket içi SQL Server kopyalayın ve Parquet biçiminde Azure Data Lake Storage 2. yazın.
* Dosyaları (CSV) metin biçiminde şirket içi dosya sisteminden kopyalama ve Azure Blob Avro biçiminde yazmak.
* ZIP dosyalarını şirket içi dosya sisteminden kopyalayın ve Azure Data Lake Storage 2. ' ye açıp açın.
* Verileri Azure Blobundan GZip sıkıştırılmış metni (CSV) biçiminde kopyalayın ve Azure SQL veritabanı'na yazın.
* Serileştirme/seri durumdan çıkarma veya sıkıştırma/açma ile ilgili birçok durum daha vardır.

## <a name="supported-regions"></a>Desteklenen bölgeler

Kopyalama etkinliği'ni destekleyen hizmet genel olarak bölgelerinde kullanılabilir durumdadır ve coğrafyalar listelenen [Azure Integration Runtime konumları](concepts-integration-runtime.md#integration-runtime-location). Dünya çapında topolojisi genellikle bölgeler arası atlama önler verimli veri taşıma sağlar. Bkz: [bölgelere göre Hizmetler](https://azure.microsoft.com/regions/#services) Data Factory veri taşıma bir bölge ve kullanılabilirlik için.

## <a name="configuration"></a>Yapılandırma

Azure veri fabrikasında kopyalama etkinliği kullanmak için yapmanız:

1. **Kaynak veri deposu ve havuz veri deposu için bağlı hizmetler oluşturacaksınız.** Yapılandırma ve desteklenen özellikleri bağlayıcı makalenin "bağlı hizmet özellikleri" bölümüne bakın. Desteklenen bağlayıcı listesinde bulabilirsiniz [desteklenen veri depoları ve biçimler](#supported-data-stores-and-formats) bölümü.
2. **Kaynak ve havuz için veri kümeleri oluşturun.** Yapılandırma ve desteklenen özellikleri hakkında ' "veri kümesi özellikleri" başlıklı kaynak ve havuz bağlayıcı makalelerine bakın.
3. **Kopyalama etkinliği ile işlem hattı oluşturursunuz.** Sonraki bölümde, bir örnek sağlar.

### <a name="syntax"></a>Sözdizimi

Kopyalama etkinliği, aşağıdaki şablon desteklenen özelliklerin kapsamlı bir liste içerir. Senaryonuza uygun olanları belirtin.

```json
"activities":[
    {
        "name": "CopyActivityTemplate",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<source dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<sink dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>",
                <properties>
            },
            "sink": {
                "type": "<sink type>"
                <properties>
            },
            "translator":
            {
                "type": "TabularTranslator",
                "columnMappings": "<column mapping>"
            },
            "dataIntegrationUnits": <number>,
            "parallelCopies": <number>,
            "enableStaging": true/false,
            "stagingSettings": {
                <properties>
            },
            "enableSkipIncompatibleRow": true/false,
            "redirectIncompatibleRowSettings": {
                <properties>
            }
        }
    }
]
```

### <a name="syntax-details"></a>Söz dizimi ayrıntıları

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Copy etkinliğinin Type özelliği şu şekilde ayarlanmalıdır: **Copy** | Evet |
| inputs | Kaynak verileri hangi noktalara oluşturduğunuz veri kümesi belirtin. Kopyalama etkinliği, yalnızca tek bir giriş destekler. | Evet |
| outputs | Havuz veri hangi noktalara oluşturduğunuz veri kümesi belirtin. Kopyalama etkinliği, yalnızca tek bir çıktı destekler. | Evet |
| typeProperties | Kopyalama etkinliği yapılandırmak için özellikler grubu. | Evet |
| source | Kopyalama kaynağı türü ve karşılık gelen özelliklere veri almak nasıl belirtin.<br/><br/>"Kopyalama etkinliğinin özellikleri" bölümündeki bağlayıcı makalede listelenen daha ayrıntılı bilgi [desteklenen veri depoları ve biçimler](#supported-data-stores-and-formats). | Evet |
| sink | Kopyalama Havuz türü ve karşılık gelen özelliklere veri yazma konusunda belirtin.<br/><br/>"Kopyalama etkinliğinin özellikleri" bölümündeki bağlayıcı makalede listelenen daha ayrıntılı bilgi [desteklenen veri depoları ve biçimler](#supported-data-stores-and-formats). | Evet |
| translator | Kaynak havuzu için açıkça bir sütun eşlemelerini belirtin. Varsayılan kopyalama davranışı gereksinimi yerine getiremiyor uygulanır.<br/><br/>Ayrıntıları öğrenin [şema ve veri türü eşlemesi](copy-activity-schema-and-type-mapping.md). | Hayır |
| dataIntegrationUnits | ' In powerfulness belirtin [Azure Integration Runtime](concepts-integration-runtime.md) veri kopyalama olanağı. Eski veri taşıma birimleri (DMU) bulut olarak bilinir. <br/><br/>Ayrıntıları öğrenin [veri tümleştirme birimleri](copy-activity-performance.md#data-integration-units). | Hayır |
| parallelCopies | Kopyalama etkinliği, havuz için veri kaynağı ve veri yazma ait okunurken kullanılacak istediğiniz paralellik belirtin.<br/><br/>Ayrıntıları öğrenin [paralel kopyalama](copy-activity-performance.md#parallel-copy). | Hayır |
| enableStaging<br/>stagingSettings | Verileri kaynaktan havuza doğrudan kopyalamak yerine bir blob depolamada geçici verileri hazırlamak için seçin.<br/><br/>Yararlı senaryoları ve yapılandırma ayrıntılarını öğrenmek [kopyalama aşamalı](copy-activity-performance.md#staged-copy). | Hayır |
| enableskipıncompatiblerow<br/>redirectıncompatiblerowsettings| Havuz kaynaktan veri kopyalama sırasında uyumsuz satırların işlemek nasıl seçin.<br/><br/>Ayrıntıları öğrenin [hataya dayanıklılık](copy-activity-fault-tolerance.md). | Hayır |

## <a name="monitoring"></a>İzleme

Kopyalama etkinliği Azure Data Factory "Yazar ve İzleyici" kullanıcı arabiriminde veya programlama yoluyla çalıştırmasının izleyebilirsiniz. Ardından performans ve senaryonuz için kopyalama etkinliği'nin yapılandırılmasını karşılaştırabilirsiniz [Performans başvurusu](copy-activity-performance.md#performance-reference) şirket içi test.

### <a name="monitor-visually"></a>Görsel olarak izleme

Kopyalama etkinliği çalıştırmasının görsel olarak izlemek için veri fabrikanıza gidin -> **yazar ve İzleyici** -> **İzleyici sekmesi**, işlem hattı listesini çalıştırır bir"Etkinlikçalıştırmalarınıgörüntüle"bağlantısınabakın **Eylemler** sütun.

![İşlem hattı çalıştırmalarını izleme](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)

Bu işlem hattı çalıştırmasını etkinlikler listesini görmek için tıklayın. İçinde **eylemleri** sütun kopyalama etkinliği giriş, çıkış, (kopyalama etkinliği çalıştırma başarısız olursa) hataları ve ayrıntıları bağlantılar bulunur.

![Etkinlik çalıştırmalarını izleme](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

Tıklayın "**ayrıntıları**" altında bağlantı **eylemleri** kopyalama etkinliği'nin yürütme ayrıntıları ve performans özelliklerini görmek için. Bu havuz için kaynak, aktarım hızı, karşılık gelen süre ile geçtiği ve yapılandırmaları kopyalama senaryonuz için kullanılan adımları dahil olmak üzere birim/satır/dosyaları veri kopyalanan bilgiler gösterir.

>[!TIP]
>Bazı senaryolarda, kopyalama izleme sayfasının en üstünde "**performans ayarlama ipuçları**" ' nı da görürsünüz. bu durum, büyük/veya büyük/veya kopya aktarım hızını artırmak için bu şekilde nelerin değişeceklerini gösteren bir örnek görürsünüz. [](#performance-and-tuning)

**Örnek: Azure Data Lake Store için Amazon S3'ten kopyalama**
![İzleyici etkinlik çalışma ayrıntıları](./media/copy-activity-overview/monitor-activity-run-details-adls.png)

**Örnek: Azure SQL veritabanından Azure SQL veri ambarı'nı kullanarak kopyalama için hazırlanan kopyalama**
![İzleyici etkinlik çalışma ayrıntıları](./media/copy-activity-overview/monitor-activity-run-details-sql-dw.png)

### <a name="monitor-programmatically"></a>Program aracılığıyla izleyin

Kopyalama etkinliği yürütme ayrıntıları ve performans özellikleri de, etkinlik çalıştırma sonucu-> çıkışı bölümünde de döndürülür. Kapsamlı bir liste aşağıda verilmiştir; Yalnızca kopya senaryonuza uygun ayarlara gösterilir. Çalıştırılan Etkinlik izleme hakkında bilgi edinin [hızlı başlangıç bölümünde izleme](quickstart-create-data-factory-dot-net.md#monitor-a-pipeline-run).

| Özellik adı  | Açıklama | Birim |
|:--- |:--- |:--- |
| dataRead | Kaynaktan okunan veri boyutu | Int64 değeri **bayt** |
| dataWritten | Havuz için yazılan veri boyutu | Int64 değeri **bayt** |
| filesRead | Dosya depolama'yı veri kopyalama işlemi sırasında kopyalanan dosyaların sayısıdır. | Int64 değeri (birim) |
| filesWritten | Dosya depolama alanına veri kopyalama işlemi sırasında kopyalanan dosyaların sayısıdır. | Int64 değeri (birim) |
| Kaynakcepeakconnections | Kopyalama etkinliği çalışması sırasında kaynak veri deposuna kurulan en fazla eşzamanlı bağlantı sayısı. | Int64 değeri (birim) |
| sinkPeakConnections | Kopyalama etkinliği çalışması sırasında veri deposuna havuz için kurulan maksimum eşzamanlı bağlantı sayısı. | Int64 değeri (birim) |
| rowsRead | Kaynaktan okunan satır sayısı (ikili kopya için geçerli değildir). | Int64 değeri (birim) |
| rowsCopied | Havuza kopyalandığı satır sayısı (ikili kopya için geçerli değildir). | Int64 değeri (birim) |
| rowsSkipped | İki tanesinden uyumsuz satırların sayısı. True olarak Ayarla "Enableskipıncompatiblerow" tarafından özelliğini kapatabilirsiniz. | Int64 değeri (birim) |
| copyDuration | Kopyanın süresi. | Int32 değeri saniye |
| throughput | Verilerin aktarıldığı oran. | Kayan noktalı sayı olarak **KB/sn** |
| Kaynakcepeakconnections | Kopyalama sırasında kaynak veri deposuna kurulan en yüksek eşzamanlı bağlantı sayısı. | Int32 değeri |
| sinkPeakConnections| Kopyalama sırasında havuz veri deposuna kurulan en yüksek eşzamanlı bağlantı sayısı.| Int32 değeri |
| sqlDwPolyBase | PolyBase, SQL veri ambarı'na veri kopyalama işlemi sırasında kullanılıyorsa. | Boole |
| redshiftUnload | UNLOAD veri Redshift'ten kopyalarken kullanılıyorsa. | Boole |
| hdfsDistcp | Verileri HDFS kopyalarken DistCp kullanılıyorsa. | Boole |
| effectiveIntegrationRuntime | Etkinlik çalıştırma, biçiminde güçlendirmek için kullanılan tümleştirme Runtime(s) show `<IR name> (<region if it's Azure IR>)`. | Metin (dize) |
| usedDataIntegrationUnits | Kopyalama sırasında etkili veri tümleştirme birimi. | Int32 değeri |
| usedParallelCopies | Kopyalama sırasında etkili parallelCopies. | Int32 değeri |
| redirectRowPath | Blob depolamada Atlanan uyumsuz satırların günlük yolu "Redirectıncompatiblerowsettings" altında yapılandırın. Aşağıdaki örnekte bakın. | Metin (dize) |
| executionDetails | Kopyalama etkinliği, geçer aşamaları ve ilgili adımlarda, süre, kullanılan yapılandırmaları, vb. hakkında daha fazla bilgi. Bu bölümde, değişiklik gösterebileceği için ayrıştırılacak önermedi.<br/><br/>ADF Ayrıca, bölümündeki `detailedDurations`ilgili adımlarda harcanan ayrıntılı süreleri (saniye cinsinden) raporlar. Bu adımların süreleri özeldir ve yalnızca belirtilen kopyalama etkinliği çalıştırmasına uygulananlar görünür:<br/>- **Sıraya alma süresi** (`queuingDuration`): Kopyalama etkinliği tümleştirme çalışma zamanı üzerinde çalışmaya başlanana kadar geçen süre. Şirket içinde barındırılan IR kullanıyorsanız ve bu değer büyükse, IR kapasitesini ve kullanımını kontrol etmeyi ve iş yükünüze göre ölçeği artırma/genişletme seçeneğini önerin. <br/>- **Kopyalama öncesi betik süresi** (`preCopyScriptDuration`): IR ve kopyalama etkinliğinden itibaren kopyalama etkinliği arasındaki geçen süre, havuz veri deposunda kopyalama öncesi betiği yürütmeyi tamamlıyor. Kopyalama öncesi betiği yapılandırırken uygulayın. <br/>- **İlk bayta kadar süre** (`timeToFirstByte`): Önceki adımın bitişi ile kaynak veri deposundan ilk baytı alan IR arasındaki geçen süre. Dosya tabanlı olmayan kaynak için geçerlidir. Bu değer büyükse, sorguyu veya sunucuyu denetleyip iyileştirmek için öneri önerin.<br/>- **Aktarım süresi** (`transferDuration`): Önceki adımın sonu ile IR 'nin tüm verileri kaynaktan havuza aktarma arasındaki geçen süre. | Array |
| Perfönerisi | Performans ayarlama ipuçlarını kopyalayın. Ayrıntılar için [performans ve ayarlama](#performance-and-tuning) bölümüne bakın. | Array |

```json
"output": {
    "dataRead": 6198358,
    "dataWritten": 19169324,
    "filesRead": 1,
    "sourcePeakConnections": 1,
    "sinkPeakConnections": 2,
    "rowsRead": 39614,
    "rowsCopied": 39614,
    "copyDuration": 1325,
    "throughput": 4.568,
    "errors": [],
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)",
    "usedDataIntegrationUnits": 4,
    "usedParallelCopies": 1,
    "executionDetails": [
        {
            "source": {
                "type": "AzureBlobStorage"
            },
            "sink": {
                "type": "AzureSqlDatabase"
            },
            "status": "Succeeded",
            "start": "2019-08-06T01:01:36.7778286Z",
            "duration": 1325,
            "usedDataIntegrationUnits": 4,
            "usedParallelCopies": 1,
            "detailedDurations": {
                "queuingDuration": 2,
                "preCopyScriptDuration": 12,
                "transferDuration": 1311
            }
        }
    ],
    "perfRecommendation": [
        {
            "Tip": "Sink Azure SQL Database: The DTU utilization was high during the copy activity run. To achieve better performance, you are suggested to scale the database to a higher tier than the current 1600 DTUs.",
            "ReferUrl": "https://go.microsoft.com/fwlink/?linkid=2043368",
            "RuleName": "AzureDBTierUpgradePerfRecommendRule"
        }
    ]
}
```

## <a name="schema-and-data-type-mapping"></a>Şema ve veri türü eşlemesi

Bkz: [şema ve veri türü eşlemesi](copy-activity-schema-and-type-mapping.md), kopyalama etkinliği havuz için kaynak verilerinizi nasıl eşlendiğini açıklar.

## <a name="fault-tolerance"></a>Hataya dayanıklılık

Varsayılan olarak, kopyalama etkinliği verileri kopyalamayı durduruyor ve kaynak ve havuz arasında uyumsuz verilerle karşılaştığında bir hata döndürür. Açıkça atlayın ve uyumsuz satırları oturum ve yalnızca kopyalama başarılı olmak için bu uyumlu veri kopyalamak için yapılandırabilirsiniz. Bkz: [kopyalama etkinliği hataya dayanıklılık](copy-activity-fault-tolerance.md) hakkında daha fazla bilgi.

## <a name="performance-and-tuning"></a>Performans ve ayar

Bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](copy-activity-performance.md), Azure Data factory'deki veri taşıma (kopyalama etkinliği) performansını etkileyen önemli faktörlerin açıklar. Ayrıca, iç test sırasında gözlemlenen performans listeler ve kopyalama etkinliği performansı iyileştirmek için çeşitli yollar ele alınmaktadır.

Bazı durumlarda, ADF 'de bir kopyalama etkinliği yürüttüğünüzde, aşağıdaki örnekte gösterildiği gibi [kopyalama etkinliği izleme sayfasının](#monitor-visually) en üstünde "**performans ayarlama ipuçları**" nı doğrudan görürsünüz. Yalnızca verilen kopya çalıştırması için tanımlanan performans sorunlarını söyetmez, ancak kopyalama aktarım hızını artırmak için bu şekilde ne değişeceklerini de kılavuzluk eder. Performans ayarlama ipuçları şu anda, verileri Azure SQL veri ambarı 'na kopyalarken, veri deposu tarafındaki kaynak performans sorunu olduğunda Azure Cosmos DB RU veya Azure SQL VERITABANı DTU 'yu artırmak, gereksiz hazırlanmamış kaldırma Kopyala, vs. Performans ayarlama kuralları da kademeli olarak zenginleşmelidir.

**Örnek: performans ayarlama ipuçları ile Azure SQL DB 'ye kopyalama**

Bu örnekte, kopya çalıştırma sırasında, ADF Azure SQL DB 'nin yazma işlemlerini yavaşlatan yüksek bir DTU kullanımına ulaştığını fark eder, bu nedenle Azure SQL DB katmanını daha DTU ile artırma önerisi.

![Performansı ayarlama ipuçlarıyla izleme kopyalama](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

## <a name="incremental-copy"></a>Artımlı kopyalama
Data Factory, Delta verilerini bir kaynak veri deposundan bir havuz veri deposuna artımlı olarak kopyalamak için senaryoları destekler. Bkz: [öğretici: verileri artımlı olarak kopyalama](tutorial-incremental-copy-overview.md).

## <a name="read-and-write-partitioned-data"></a>Bölümlenmiş veri okuma ve yazma
Sürüm 1'de, Azure Data Factory SliceStart/SliceEnd/WindowStart/WindowEnd sistem değişkenlerini kullanarak bölümlenmiş verileri yazma veya okuma desteklenmiyor. Geçerli sürümde, parametrenin değeri bir işlem hattı parametresi ve tetikleyicinin başlangıç saati/zamanlanan saat'ı kullanarak bu davranışı elde edebilirsiniz. Daha fazla bilgi için [okumak veya yazmak nasıl veri bölümlenmiş](how-to-read-write-partitioned-data.md).

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki Hızlı Başlangıç kılavuzlarımız, öğreticilerimiz ve örneklerimizle bakın:

- [Verileri bir konumdan aynı Azure Blob depolama alanındaki başka bir konuma kopyalayın.](quickstart-create-data-factory-dot-net.md)
- [Verileri Azure Blob depolama alanından Azure SQL veritabanı'na kopyalayın.](tutorial-copy-data-dot-net.md)
- [Verileri şirket içi SQL Server'dan Azure'a kopyalama](tutorial-hybrid-copy-powershell.md)
