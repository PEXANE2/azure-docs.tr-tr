---
title: Kopyalama etkinliğini izleme
description: Azure Veri Fabrikası'nda kopyalama etkinliği yürütmesini nasıl izleyeceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: jingwang
ms.openlocfilehash: 47824095e892ca3c919d2d871feb612758ab2308
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417853"
---
# <a name="monitor-copy-activity"></a>Kopyalama etkinliğini izleme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Azure Veri Fabrikası'nda kopyalama etkinliği yürütmenasıl izlenir. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="monitor-visually"></a>Görsel olarak izleme

Azure Veri Fabrikası'nda bir ardışık hatlar oluşturup yayımladıktan sonra, bir tetikleyiciyle ilişkilendirebilir veya geçici bir çalıştırmabaşlatabilirsiniz. Tüm ardışık hatlar çalışırınızı Azure Veri Fabrikası kullanıcı deneyiminde yerel olarak izleyebilirsiniz. Azure Veri Fabrikası izleme hakkında genel [olarak Görsel olarak azure veri fabrikasını görüntüleyebilirsiniz.](monitor-visually.md)

Kopyalama etkinliğini izlemek için veri fabrikanızın **Yazar & Monitor** Kullanıcı UI adresine gidin. **Monitör** sekmesinde, ardışık hatlar çalışır listesi görürsünüz, ardışık ardışık ardışık çalışma daki etkinlik çalışır listesine erişmek için **ardışık ad** bağlantısını tıklatın.

![Kopyalama etkinliğini izleme](./media/copy-activity-overview/monitor-pipeline-run.png)

Bu düzeyde, etkinlik girdisini, çıktısını ve hatalarını kopyalamabağlantıları (Kopyalama etkinliği çalıştırılmazsa) ve süre/durum gibi istatistikleri görebilirsiniz. Kopyalama etkinliği adının yanındaki **Ayrıntılar** düğmesini (gözlük) tıklattığınızda, kopyalama etkinliği yürütmeniz hakkında ayrıntılı bilgi verecektir. 

![Kopyalama etkinliğini izleme](./media/copy-activity-overview/monitor-copy-activity-run.png)

Bu grafik izleme görünümünde, Azure Veri Fabrikası size veri okuma/yazma hacmi, kaynaktan batmaya kopyalanan dosya/satır sayısı, iş hacmi, kopyalama senaryonuz için uygulanan yapılandırmalar, kopyalama etkinliğinin ilgili süreler ve ayrıntılarla birlikte geçtiği adımlar ve daha fazlası dahil olmak üzere kopyalama etkinliği yürütme bilgilerini sunar. Her olası metrik ve ayrıntılı açıklamasında [bu tabloya](#monitor-programmatically) bakın. 

Bazı senaryolarda, Veri Fabrikası'nda bir Kopyalama etkinliği çalıştırdığınızda, örnekte gösterildiği gibi kopya etkinliği izleme görünümünün üst kısmında **"Performans alamı ipuçları"** görürsünüz. İpuçları, belirli kopyalama çalışması için ADF tarafından tanımlanan darboğaz ve kopyalama iş masını artırmak için neleri değiştireceğiniz konusunda önerilerde bulunmak. [Otomatik performans atonlama ipuçları](copy-activity-performance-troubleshooting.md#performance-tuning-tips)hakkında daha fazla bilgi edinin.

Alttaki **yürütme ayrıntıları ve süreleri,** kopyalama etkinliğinizin geçtiği ve özellikle kopya performansını gidermede yararlı olan önemli adımları açıklar. Kopya çalıştırmanızın darboğazı en uzun süreye sahip olandır. Her aşamanın temsil için kopya etkinlik performansını ve ayrıntılı sorun giderme yönündeki [Sorun Giderme etkinlik](copy-activity-performance-troubleshooting.md) performansına bakın.

**Örnek: Amazon S3'ten Azure Veri Gölü Depolama Gen2'ye kopyala**

![Kopyalama etkinliği çalıştırma ayrıntılarını izleme](./media/copy-activity-overview/monitor-copy-activity-run-details.png)

## <a name="monitor-programmatically"></a>Programlı olarak izleyin

Kopyalama etkinliği yürütme ayrıntıları ve performans özellikleri, Kullanıcı Bulma Birimi izleme görünümünü işlemek için kullanılan **Kopyalama Etkinliği çalıştırsonuç** > **Çıktısı** bölümünde de döndürülür. Aşağıda döndürülebilecek özelliklerin tam listesi verilmiştir. Yalnızca kopyalama senaryonuz için geçerli olan özellikleri görürsünüz. Genel olarak etkinlik çalışır nasıl izlenir hakkında bilgi [için](monitor-programmatically.md)bkz.

| Özellik adı  | Açıklama | Çıktıdaki birim |
|:--- |:--- |:--- |
| dataRead | Gerçek veri miktarı kaynaktan okunur. | Int64 değeri, bayt |
| dataWritten | Lavaboya yazılmış/taahhüt edilen verilerin gerçek montajı. Her veri deposunun `dataRead` verileri nasıl depoladığını ilişkilendiren boyut, boyutundan farklı olabilir. | Int64 değeri, bayt |
| dosyalarOkundu | Dosya tabanlı kaynaktan okunan dosya sayısı. | Int64 değeri (birim yok) |
| dosyalarYazılı | Dosya tabanlı lavaboya yazılan/taahhüt edilen dosya sayısı. | Int64 değeri (birim yok) |
| sourcePeakConnections | Kopyalama etkinliği sırasında kaynak veri deposuna kurulan eşzamanlı bağlantıların en yüksek sayısı. | Int64 değeri (birim yok) |
| sinkPeakConnections | Kopya etkinliği çalışması sırasında lavabo veri deposuna kurulan eşzamanlı bağlantıların en yüksek sayısı. | Int64 değeri (birim yok) |
| satırokusOku | Kaynaktan okunan satır sayısı (ikili kopya için geçerli değildir). | Int64 değeri (birim yok) |
| satırkopyalanmış | Batmak için kopyalanan satır sayısı (ikili kopya için geçerli değildir). | Int64 değeri (birim yok) |
| satırlarAtlanır | Atlanan uyumsuz satır sayısı. Doğru ayarlayarak `enableSkipIncompatibleRow` uyumsuz satırların atlanmasını etkinleştirebilirsiniz. | Int64 değeri (birim yok) |
| copyDuration | Kopyalama çalışmasının süresi. | Int32 değeri, saniye cinsinden |
| aktarım hızı | Veri aktarım hızı. | Kayan nokta sayısı, KBps |
| sourcePeakConnections | Kopyalama etkinliği sırasında kaynak veri deposuna kurulan eşzamanlı bağlantıların en yüksek sayısı. | Int32 değeri (birim yok) |
| sinkPeakConnections| Kopya etkinliği çalışması sırasında lavabo veri deposuna kurulan eşzamanlı bağlantıların en yüksek sayısı.| Int32 değeri (birim yok) |
| sqlDwPolyBase | Veriler SQL Veri Ambarı'na kopyalandığında PolyBase'in kullanılıp kullanılmadığı. | Boole |
| redshiftUnload | Veriler Redshift'ten kopyalandığında UNLOAD'ın kullanılıp kullanılmadığı. | Boole |
| hdfsDistcp | Veriler HDFS'den kopyalandığında DistCp'nin kullanılıp kullanılmadığı. | Boole |
| etkiliIntegrationRuntime | Tümleştirme çalışma zamanı (IR) veya etkinlik çalışmasını sağlamak için `<IR name> (<region if it's Azure IR>)`kullanılan çalışma süreleri biçiminde. | Metin (dize) |
| usedDataIntegrationUnits | Kopyalama sırasında etkili Veri Tümleştirme Birimleri. | Int32 değeri |
| kullanılan ParallelCopys | Kopya sırasında etkili parallelCopys. | Int32 değeri |
| redirectRowPath | Özellikte yapılandırdığınız blob depolamasında atlanan uyumsuz satırların `redirectIncompatibleRowSettings` günlüğüne giden yol. Bkz. [Hata toleransı.](copy-activity-overview.md#fault-tolerance) | Metin (dize) |
| yürütmeAyrıntılar | Kopyalama etkinliğinin geçtiği aşamalar ve ilgili adımlar, süreler, yapılandırmalar ve benzeri ayrıntılar hakkında daha fazla bilgi. Bu bölümü ayrışdırmanızı önermiyoruz, çünkü bu bölüm değişebilir. Kopya performansını anlamanıza ve sorun gidermenize nasıl yardımcı olduğunu daha iyi anlamak için [görsel olarak Monitör](#monitor-visually) bölümüne bakın. | Dizi |
| perfTavsiye | Performans atonlama ipuçlarını kopyalayın. Ayrıntılar için [Performans alamı ipuçlarına](copy-activity-performance-troubleshooting.md#performance-tuning-tips) bakın. | Dizi |

**Örnek:**

```json
"output": {
    "dataRead": 1180089300500,
    "dataWritten": 1180089300500,
    "filesRead": 110,
    "filesWritten": 110,
    "sourcePeakConnections": 640,
    "sinkPeakConnections": 1024,
    "copyDuration": 388,
    "throughput": 2970183,
    "errors": [],
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)",
    "usedDataIntegrationUnits": 128,
    "billingReference": "{\"activityType\":\"DataMovement\",\"billableDuration\":[{\"Managed\":11.733333333333336}]}",
    "usedParallelCopies": 64,
    "executionDetails": [
        {
            "source": {
                "type": "AmazonS3"
            },
            "sink": {
                "type": "AzureBlobFS",
                "region": "East US",
                "throttlingErrors": 6
            },
            "status": "Succeeded",
            "start": "2020-03-04T02:13:25.1454206Z",
            "duration": 388,
            "usedDataIntegrationUnits": 128,
            "usedParallelCopies": 64,
            "profile": {
                "queue": {
                    "status": "Completed",
                    "duration": 2
                },
                "transfer": {
                    "status": "Completed",
                    "duration": 386,
                    "details": {
                        "listingSource": {
                            "type": "AmazonS3",
                            "workingDuration": 0
                        },
                        "readingFromSource": {
                            "type": "AmazonS3",
                            "workingDuration": 301
                        },
                        "writingToSink": {
                            "type": "AzureBlobFS",
                            "workingDuration": 335
                        }
                    }
                }
            },
            "detailedDurations": {
                "queuingDuration": 2,
                "transferDuration": 386
            }
        }
    ],
    "perfRecommendation": [
        {
            "Tip": "6 write operations were throttled by the sink data store. To achieve better performance, you are suggested to check and increase the allowed request rate for Azure Data Lake Storage Gen2, or reduce the number of concurrent copy runs and other data access, or reduce the DIU or parallel copy.",
            "ReferUrl": "https://go.microsoft.com/fwlink/?linkid=2102534 ",
            "RuleName": "ReduceThrottlingErrorPerfRecommendationRule"
        }
    ],
    "durationInQueue": {
        "integrationRuntimeQueue": 0
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Diğer Kopyalama Etkinliği makalelerini görün:

\- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)

\- [Kopyalama etkinliği performansı](copy-activity-performance.md)