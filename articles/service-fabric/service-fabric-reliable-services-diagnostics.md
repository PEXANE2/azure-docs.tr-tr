---
title: Azure Hizmet Kumaş Stateful Güvenilir Hizmetler tanılama
description: Azure Hizmet Kumaşında DevletE Uygun Güvenilir Hizmetler için tanılama işlevi
author: dkkapur
ms.topic: conceptual
ms.date: 8/24/2018
ms.author: dekapur
ms.openlocfilehash: 37162287e130b05dc41453c579b3a628ac878fca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282269"
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Durum Bilgisi Olan Reliable Services için tanılama işlevi
Azure Hizmet Kumaşı Özel Güvenilir Hizmetler StatefulServiceBase sınıfı, hizmeti hata ayıklamak, çalışma zamanının nasıl çalıştığına ilişkin öngörüler sağlamak ve sorun giderme yle ilgili yardımcı olmak için kullanılabilecek [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) olayları yayar.

## <a name="eventsource-events"></a>EventSource etkinlikleri
Stateful Reliable Services StatefulServiceBase sınıfının EventSource adı "Microsoft-ServiceFabric-Services" olur. Bu olay kaynağından gelen olaylar, hizmet [Visual Studio'da debugged](service-fabric-debugging-your-application.md)edilirken [Tanılama Olayları](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) penceresinde görünür.

EventSource etkinliklerinin toplanması ve/veya görüntülenmesinde yardımcı olan araçlara ve teknolojilere örnek olarak [PerfView,](https://www.microsoft.com/download/details.aspx?id=28567) [Azure Tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md)ve [Microsoft TraceEvent Kitaplığı](https://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent)verilebilir.

## <a name="events"></a>Olaylar
| Olay adı | Olay Kimliği | Düzey | Olay açıklaması |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Bilgilendirici |Hizmet RunAsync görevi başlatıldığında yayılan |
| StatefulRunAsyncİptal |2 |Bilgilendirici |Hizmet RunAsync görevi iptal edildiğinde yayılan |
| StatefulRunAsyncTamamlama |3 |Bilgilendirici |Hizmet RunAsync görevi tamamlandığında yayılan |
| StatefulRunAsyncSlowİptal |4 |Uyarı |Hizmet RunAsync görevi iptali tamamlamak için çok uzun sürdüğünde yayılan |
| StatefulRunAsyncFailure |5 |Hata |Hizmet RunAsync görevi bir özel durum attığında yayılan |

## <a name="interpret-events"></a>Olayları yorumlama
StatefulRunAsyncInvocation, StatefulRunAsyncCompletion ve StatefulRunAsyncCancellation olayları, bir hizmetin yaşam döngüsünü n yanı sıra bir hizmetin ne zaman başlayacağını, iptal ettiği veya bitirdiği zaman için zamanlamayı anlamak için hizmet yazarı için yararlıdır. Bu bilgiler, hizmet sorunlarını hata ayıklarken veya hizmet yaşam döngüsünü anlamakta yararlı olabilir.

Hizmet yazarları StatefulRunAsyncSlowCancellation ve StatefulRunAsyncFailure olaylarına çok dikkat etmelidirler, çünkü hizmetle ilgili sorunları gösterirler.

StatefulRunAsyncFailure hizmet RunAsync() görev bir özel durum atar zaman yayılan. Genellikle, atılan bir özel durum hizmetteki bir hatayı veya hatayı gösterir. Ayrıca, özel durum hizmetin başarısız olmasına neden olur, bu nedenle farklı bir düğüme taşınır. Bu işlem pahalı olabilir ve hizmet taşınırken gelen istekleri geciktirebilir. Hizmet yazarları özel durum nedenini belirlemeli ve mümkünse bunu azaltmalıdır.

StatefulRunAsyncSlowCancellation, RunAsync görevi için bir iptal isteği dört saniyeden uzun sürdüğünde yayılır. Bir hizmetin iptali tamamlaması çok uzun sürdüğünde, hizmetin başka bir düğümüzerinde hızla yeniden başlatılması yeteneğini etkiler. Bu senaryo, hizmetin genel kullanılabilirliğini etkileyebilir.

## <a name="performance-counters"></a>Performans sayaçları
Güvenilir Hizmetler çalışma süresi aşağıdaki performans sayacı kategorilerini tanımlar:

| Kategori | Açıklama |
| --- | --- |
| Servis Kumaş İşlemel Çoğaltıcı |Azure Hizmet Kumaş İşlemli Çoğaltıcı'ya özgü sayaçlar |
| Servis Kumaş TStore |Azure Hizmet Kumaşı TStore'a özgü sayaçlar |

Service Fabric Transactional Replicator, [Güvenilir Durum Yöneticisi](service-fabric-reliable-services-reliable-collections-internals.md) tarafından belirli bir [yineleme](service-fabric-concepts-replica-lifecycle.md)kümesi içindeki hareketleri çoğaltmak için kullanılır.

Service Fabric TStore, anahtar değer çiftlerini depolamak ve almak için [Güvenilir Koleksiyonlarda](service-fabric-reliable-services-reliable-collections-internals.md) kullanılan bir bileşendir.

Windows işletim sisteminde varsayılan olarak kullanılabilen [Windows Performans İzleyicisi](https://technet.microsoft.com/library/cc749249.aspx) uygulaması, performans sayacı verilerini toplamak ve görüntülemek için kullanılabilir. [Azure Tanılama,](../cloud-services/cloud-services-dotnet-diagnostics.md) performans sayacı verilerini toplamak ve Azure tablolarına yüklemek için başka bir seçenektir.

### <a name="performance-counter-instance-names"></a>Performans sayacı örnek adları
Çok sayıda güvenilir hizmet ekine veya güvenilir hizmet bölümüne sahip bir kümede çok sayıda işlem çoğaltıcı performans sayacı örneği olacaktır. Bu aynı zamanda TStore performans sayaçları için de geçerlidir, ancak aynı zamanda kullanılan Güvenilir Sözlükler ve Güvenilir Kuyrukların sayısıyla da çarpılır. Performans sayacı örnek adları, performans sayacı örneğinin ilişkili olduğu TStore durumunda belirli [bir bölümü,](service-fabric-concepts-partitioning.md)hizmet yinelemesini ve durum sağlayıcısını tanımlamaya yardımcı olabilir.

#### <a name="service-fabric-transactional-replicator-category"></a>Servis Kumaş İşlemel Çoğaltıcı kategorisi
Kategori `Service Fabric Transactional Replicator`için sayaç örnek adları aşağıdaki biçimdedir:

`ServiceFabricPartitionId:ServiceFabricReplicaId`

*ServiceFabricPartitionId,* performans sayacı örneğinin ilişkili olduğu Service Fabric partition ID'nin dize gösterimidir. Bölüm kimliği bir GUID'dir ve dize [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) gösterimi biçim belirtici "D" ile oluşturulur.

*ServiceFabricReplicaId,* güvenilir bir hizmetin belirli bir kopyasıyla ilişkili kimliktir. Çoğaltma kimliği, benzersizliğini sağlamak ve aynı bölüm tarafından oluşturulan diğer performans sayacı örnekleriile çakışmayı önlemek için performans sayacı örnek adına dahildir. Kopyaları ve güvenilir hizmetlerdeki rolleri hakkında daha fazla bilgi [burada](service-fabric-concepts-replica-lifecycle.md)bulabilirsiniz.

Aşağıdaki sayaç örneği adı, kategori altındaki `Service Fabric Transactional Replicator` bir sayaç için tipiktir:

`00d0126d-3e36-4d68-98da-cc4f7195d85e:131652217797162571`

Önceki örnekte, `00d0126d-3e36-4d68-98da-cc4f7195d85e` Hizmet Kumaşı bölümleme kimliğinin dize gösterimi ve `131652217797162571` çoğaltma kimliğidir.

#### <a name="service-fabric-tstore-category"></a>Servis Kumaş TStore kategorisi
Kategori `Service Fabric TStore`için sayaç örnek adları aşağıdaki biçimdedir:

`ServiceFabricPartitionId:ServiceFabricReplicaId:StateProviderId_PerformanceCounterInstanceDifferentiator_StateProviderName`

*ServiceFabricPartitionId,* performans sayacı örneğinin ilişkili olduğu Service Fabric partition ID'nin dize gösterimidir. Bölüm kimliği bir GUID'dir ve dize [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) gösterimi biçim belirtici "D" ile oluşturulur.

*ServiceFabricReplicaId,* güvenilir bir hizmetin belirli bir kopyasıyla ilişkili kimliktir. Çoğaltma kimliği, benzersizliğini sağlamak ve aynı bölüm tarafından oluşturulan diğer performans sayacı örnekleriile çakışmayı önlemek için performans sayacı örnek adına dahildir. Kopyaları ve güvenilir hizmetlerdeki rolleri hakkında daha fazla bilgi [burada](service-fabric-concepts-replica-lifecycle.md)bulabilirsiniz.

*StateProviderId,* güvenilir bir hizmet içinde bir devlet kuruluşuyla ilişkili kimliktir. Bir TStore'u diğerinden ayırt etmek için performans sayacı örnek adına devlet sağlayıcı kimliği dahildir.

*PerformanceCounterInstanceDifferentiator,* bir devlet sağlayıcısı içindeki performans sayacı örneğiyle ilişkili bir ayırt edici kimliktir. Bu ayırıcı, benzersizliğini sağlamak ve aynı durum sağlayıcısı tarafından oluşturulan diğer performans sayacı örnekleriile çakışmayı önlemek için performans sayacı örnek adına dahildir.

*StateProviderName,* güvenilir bir hizmet içinde bir devlet kuruluşuyla ilişkilendirilen addır. Kullanıcıların hangi durumu sağladığını kolayca belirlemesi için performans sayacı örnek adına devlet sağlayıcı adı dahildir.

Aşağıdaki sayaç örneği adı, kategori altındaki `Service Fabric TStore` bir sayaç için tipiktir:

`00d0126d-3e36-4d68-98da-cc4f7195d85e:131652217797162571:142652217797162571_1337_urn:MyReliableDictionary/dataStore`

Önceki `00d0126d-3e36-4d68-98da-cc4f7195d85e` örnekte, Hizmet Kumaşı bölümleme kimliğinin `131652217797162571` dize gösterimi, yineleme kimliğidir, `142652217797162571` devlet sağlayıcı kimliğidir ve `1337` performans sayacı örneği ayırt edicidir. `urn:MyReliableDictionary/dataStore`adlı `urn:MyReliableDictionary`koleksiyon için veri depolayan devlet sağlayıcısının adıdır.

### <a name="transactional-replicator-performance-counters"></a>İşlemsel Çoğalıcı performans sayaçları

Güvenilir Hizmetler çalışma `Service Fabric Transactional Replicator` süresi, kategori altında aşağıdaki etkinlikleri yayar

 Sayaç adı | Açıklama |
| --- | --- |
| Txn İşlemlerine Başla/sn | Saniyede oluşturulan yeni yazma hareketlerinin sayısı.|
| Txn İşlemleri/sn | Saniyede güvenilir koleksiyonlarda gerçekleştirilen ekleme/güncelleştirme/silme işlemlerinin sayısı.|
| Flush Bayt/sn'yi günlüğe kaydet | İşlem Çoğaltıcısı tarafından saniyede diske atılmakta olan bayt sayısı |
| Daraltılmış İşlemler/sn | İşlem Çoğaltıcısı tarafından azaltma nedeniyle her saniye reddedilen işlem sayısı. |
| Avg. İşlem ms/Commit | İşlem başına ortalama gecikme milisaniye cinsinden |
| Avg. Flush Gecikme (ms) | İşlem Çoğaltıcısı tarafından milisaniye cinsinden başlatılan disk yıkama işlemlerinin ortalama süresi |

### <a name="tstore-performance-counters"></a>TStore performans sayaçları

Güvenilir Hizmetler çalışma `Service Fabric TStore` süresi, kategori altında aşağıdaki etkinlikleri yayar

 Sayaç adı | Açıklama |
| --- | --- |
| Madde Sayısı | Mağazadaki öğe sayısı.|
| Disk Boyutu | Mağazaiçin denetim noktası dosyalarının baytlarda toplam disk boyutu.|
| Checkpoint Dosyası Yazma Bayt/sn | En son denetim noktası dosyası için saniyede yazılan bayt sayısı.|
| Disk Aktarım Bayt/sn Kopyala | Bir mağaza kopyası sırasında saniyede okunan (birincil yinelemede) veya yazılan (ikincil yinelemede) disk bayt sayısı.|

## <a name="next-steps"></a>Sonraki adımlar
[PerfView'deki EventSource sağlayıcıları](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
