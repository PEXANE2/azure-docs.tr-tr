---
title: Kopyalama etkinliğini izle
description: Azure Data Factory kopyalama etkinliği yürütmesini izleme hakkında bilgi edinin.
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
ms.openlocfilehash: 6494352bf957af83b45488493bf12a094c730c09
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79125764"
---
# <a name="monitor-copy-activity"></a>Kopyalama etkinliğini izle

Bu makalede Azure Data Factory kopyalama etkinliği yürütmenin nasıl izleneceği özetlenmektedir. Kopyalama etkinliğine genel bir bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesinde oluşturulur.

## <a name="monitor-visually"></a>Görsel olarak izleme

Azure Data Factory bir işlem hattı oluşturup yayımladıktan sonra, bir tetikleyici ile ilişkilendirebilir veya bir geçici çalıştırmayı el ile başlatabilirsiniz. Azure Data Factory kullanıcı deneyiminde tüm işlem hattı çalıştırmalarının yerel olarak izlenmesini sağlayabilirsiniz. [Görsel olarak izleme Azure Data Factory](monitor-visually.md)genel olarak izleme Azure Data Factory hakkında bilgi edinin.

Kopyalama etkinliği çalıştırmasını izlemek için data factory **yazarıza & Monitor** Kullanıcı arabirimine gidin. **İzleyici** sekmesinde, işlem hattı çalıştırmalarının bir listesini görürsünüz, işlem hattı çalıştırmasında etkinlik çalıştırmaları listesine erişmek için işlem **hattı adı** bağlantısına tıklayın.

![Kopyalama etkinliği çalıştırmasını izle](./media/copy-activity-overview/monitor-pipeline-run.png)

Bu düzeyde, kopyalama etkinliği giriş, çıkış ve hata (kopyalama etkinliğinin başarısız olması durumunda), ayrıca süre/durum gibi istatistikler için bağlantılar bulabilirsiniz. Kopyalama etkinliği adının yanındaki **Ayrıntılar** düğmesine (eyegözlük) tıklamak, kopyalama etkinliği yürütmeyle ilgili ayrıntılı bilgiler verir. 

![Kopyalama etkinliği çalıştırmasını izle](./media/copy-activity-overview/monitor-copy-activity-run.png)

Bu grafik izleme görünümünde Azure Data Factory, veri okuma/yazma birimi, kaynaktan kopyalanan veri sayısı/satır sayısı ve kopyalama senaryonuz için uygulanan yapılandırma işlemleri, kopyalama etkinliğinin ilgili süreler ve Ayrıntılar ve daha fazlası dahil olmak üzere kopyalama etkinliği yürütme bilgilerini sunar. Olası her ölçüm ve ayrıntılı açıklaması için [Bu tabloya](#monitor-programmatically) bakın. 

Bazı senaryolarda, Data Factory bir kopyalama etkinliği çalıştırdığınızda örnekte gösterildiği gibi kopyalama etkinliği izleme görünümünün en üstünde **"performans ayarlama ipuçları"** görürsünüz. İpuçları, belirli bir kopya çalıştırması için ADF tarafından tanımlanan darboğazyı, kopyalama aktarım hızını artırmak için nelerin değişdiklerinin önerisiyle birlikte size bildirir. [Otomatik performans ayarlama ipuçları](copy-activity-performance-troubleshooting.md#performance-tuning-tips)hakkında daha fazla bilgi edinin.

En son **yürütme ayrıntıları ve süreleri** , kopyalama etkinliğinizin üzerinde geçtiği önemli adımları açıklar, bu, özellikle kopyalama performansının giderilmesi için kullanışlıdır. Kopya çalışmalarınızın performans sorunu en uzun süreye sahip bir süredir. Her aşamanın ne gösterdiği ve ayrıntılı sorun giderme kılavuzunu temsil etmesi için, [kopyalama etkinliği performansını sorun giderme](copy-activity-performance-troubleshooting.md) bölümüne bakın.

**Örnek: Amazon S3 öğesinden Azure Data Lake Storage 2. 'e Kopyala**

![Kopyalama etkinliği çalıştırma ayrıntılarını izle](./media/copy-activity-overview/monitor-copy-activity-run-details.png)

## <a name="monitor-programmatically"></a>Program aracılığıyla izleyin

Kopyalama etkinliği yürütme ayrıntıları ve performans özellikleri, Kullanıcı arabirimi izleme görünümünü oluşturmak için kullanılan **kopyalama etkinliği çalıştırma sonucu** > **çıktı** bölümünde de döndürülür. Aşağıda, döndürülebilecek özelliklerin tamamı listelenmiştir. Yalnızca kopyalama senaryonuz için geçerli olan özellikleri görürsünüz. Etkinlik çalışmalarının genel olarak nasıl izleneceği hakkında daha fazla bilgi için bkz. [programlı bir Azure Data Factory 'yi izleme](monitor-programmatically.md).

| Özellik adı  | Açıklama | Çıkışdaki birim |
|:--- |:--- |:--- |
| dataRead | Kaynaktaki okunan gerçek veri miktarı. | Int64 değeri, bayt cinsinden |
| dataWritten | Havuza yazılan/havuza kaydedilen verilerin gerçek bağlaması. Boyut, her veri deposunun verileri nasıl depoladığını birbirleriyle ilişkilendiren `dataRead` boyutundan farklı olabilir. | Int64 değeri, bayt cinsinden |
| filesRead | Dosya tabanlı kaynaktan okunan dosya sayısı. | Int64 değeri (birim) |
| filesWritten | Dosya tabanlı havuza yazılan/kaydedilen dosya sayısı. | Int64 değeri (birim) |
| Kaynakcepeakconnections | Kopyalama etkinliği sırasında kaynak veri deposuna kurulan en yüksek eşzamanlı bağlantı sayısı. | Int64 değeri (birim) |
| sinkPeakConnections | Kopyalama etkinliği sırasında havuz veri deposuna kurulan en yüksek eşzamanlı bağlantı sayısı. | Int64 değeri (birim) |
| rowsRead | Kaynaktan okunan satır sayısı (ikili kopya için geçerli değildir). | Int64 değeri (birim) |
| rowsCopied | Havuza kopyalanmış satır sayısı (ikili kopya için geçerli değildir). | Int64 değeri (birim) |
| rowsSkipped | Atlanan uyumsuz satır sayısı. `enableSkipIncompatibleRow` true olarak ayarlayarak, uyumsuz satırların atlanmasını etkinleştirebilirsiniz. | Int64 değeri (birim) |
| copyDuration | Kopya çalıştırmasının süresi. | Int32 değeri, saniye cinsinden |
| üretilen iş | Veri aktarımı oranı. | Kayan nokta sayısı, KBps cinsinden |
| Kaynakcepeakconnections | Kopyalama etkinliği sırasında kaynak veri deposuna kurulan en yüksek eşzamanlı bağlantı sayısı. | Int32 değeri (birim yok) |
| sinkPeakConnections| Kopyalama etkinliği sırasında havuz veri deposuna kurulan en yüksek eşzamanlı bağlantı sayısı.| Int32 değeri (birim yok) |
| sqlDwPolyBase | Veriler SQL veri ambarı 'na kopyalandığında PolyBase 'in kullanılıp kullanılmayacağını belirtir. | Boole |
| redshiftUnload | Redshift öğesinden veri kopyalanırken KALDıRMA kullanılıp kullanılmayacağını belirtir. | Boole |
| hdfsDistcp | Veriler, ne zaman bir sunucudan kopyalanırsa kullanılır. | Boole |
| effectiveIntegrationRuntime | Etkinlik çalıştırmasının gücü için kullanılan Integration Runtime (IR) veya çalışma zamanları `<IR name> (<region if it's Azure IR>)`biçiminde. | Metin (dize) |
| usedDataIntegrationUnits | Kopyalama sırasında etkili veri tümleştirme birimi. | Int32 değeri |
| usedParallelCopies | Kopyalama sırasında etkili parallelCopies. | Int32 değeri |
| redirectRowPath | `redirectIncompatibleRowSettings` özelliğinde yapılandırdığınız blob depolamada atlanan uyumsuz satırların yolu. Bkz. [hata toleransı](copy-activity-overview.md#fault-tolerance). | Metin (dize) |
| executionDetails | Kopyalama etkinliğinin aşamaları hakkında daha fazla ayrıntı ve ilgili adımlar, süreler, konfigürasyonlar vb. Bu bölümü değiştirebileceğinden, bu bölümü ayrıştırmayı önermiyoruz. Kopyalama performansını anlamanıza ve sorunlarını gidermenize nasıl yardımcı olduğunu daha iyi anlamak için, [görsel izleme](#monitor-visually) bölümüne bakın. | Dizi |
| Perfönerisi | Performans ayarlama ipuçlarını kopyalayın. Ayrıntılar için [performans ayarlama ipuçlarına](copy-activity-performance-troubleshooting.md#performance-tuning-tips) bakın. | Dizi |

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
Bir kopyalama etkinliği makalelere bakın:

\- [kopyalama etkinliğine genel bakış](copy-activity-overview.md)

\- [kopyalama etkinliği performansı](copy-activity-performance.md)