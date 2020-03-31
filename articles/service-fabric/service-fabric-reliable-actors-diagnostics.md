---
title: Aktörler teşhis ve izleme
description: Bu makalede, Hizmet Kumaşı Güvenilir Aktörler çalışma süresindeki tanılama ve performans izleme özellikleri açıklanmaktadır, bunlar arasında yayılan olaylar ve performans sayaçları da yer al.) yer al.)
author: abhishekram
ms.topic: conceptual
ms.date: 10/26/2017
ms.author: abhisram
ms.openlocfilehash: e6e9fb66368461e0d3ebdd2709f4ced0e796bea5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282334"
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Reliable Actors için tanılama ve performans izlemesi
Güvenilir Aktörler çalışma zamanı [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) olaylar ve [performans sayaçları](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx)yakar. Bunlar, çalışma zamanının nasıl çalıştığına ilişkin öngörüler sağlar ve sorun giderme ve performans izleme konusunda yardımcı olur.

## <a name="eventsource-events"></a>EventSource etkinlikleri
Güvenilir Aktörler çalışma zamanı için EventSource sağlayıcı adı "Microsoft-ServiceFabric-Actors". Bu olay kaynağından olaylar, aktör uygulaması [Visual Studio'da debugged](service-fabric-debugging-your-application.md)ediliyor [zaman Tanılama Olayları](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) penceresinde görünür.

EventSource etkinliklerinin toplanması ve/veya görüntülenmesinde yardımcı olan araçlara ve teknolojilere örnek olarak [PerfView,](https://www.microsoft.com/download/details.aspx?id=28567) [Azure Tanılama,](../cloud-services/cloud-services-dotnet-diagnostics.md) [Anlamsal Günlük leme](https://msdn.microsoft.com/library/dn774980.aspx)ve Microsoft [TraceEvent Kitaplığı](https://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent)verilebilir.

### <a name="keywords"></a>Anahtar Sözcükler
Güvenilir Aktörler EventSource'a ait tüm olaylar bir veya daha fazla anahtar kelimeyle ilişkilidir. Bu, toplanan olayların filtrelemesini sağlar. Aşağıdaki anahtar sözcük bitleri tanımlanır.

| Bit | Açıklama |
| --- | --- |
| 0x1 |Kumaş Aktörler çalışma zamanı çalışmasını özetleyen önemli olaylar kümesi. |
| 0x2 |Aktör yöntemi çağrılarını açıklayan olaylar kümesi. Daha fazla bilgi için [aktörler hakkındaki tanıtım konusuna](service-fabric-reliable-actors-introduction.md)bakın. |
| 0x4 |Aktör durumuyla ilgili olaylar kümesi. Daha fazla bilgi için [aktör devlet yönetimi](service-fabric-reliable-actors-state-management.md)konusuna bakın. |
| 0x8 |Aktörde sıra tabanlı eşzamanlılıkla ilgili olaylar kümesi. Daha fazla bilgi için [eşzamanlılık](service-fabric-reliable-actors-introduction.md#concurrency)konusuna bakın. |

## <a name="performance-counters"></a>Performans sayaçları
Güvenilir Aktörler çalışma süresi aşağıdaki performans sayacı kategorilerini tanımlar.

| Kategori | Açıklama |
| --- | --- |
| Servis Kumaş Aktör |Azure Hizmet Kumaşı aktörlerine özgü sayaçlar , örneğin aktör durumunu kurtarmak için gereken süre |
| Servis Kumaş Aktör Yöntemi |Service Fabric aktörleri tarafından uygulanan yöntemlere özgü sayaçlar, örneğin bir aktör yönteminin ne sıklıkta çağrıldığı |

Yukarıdaki kategorilerin her biri bir veya daha fazla sayaç vardır.

Windows işletim sisteminde varsayılan olarak kullanılabilen [Windows Performans İzleyicisi](https://technet.microsoft.com/library/cc749249.aspx) uygulaması, performans sayacı verilerini toplamak ve görüntülemek için kullanılabilir. [Azure Tanılama,](../cloud-services/cloud-services-dotnet-diagnostics.md) performans sayacı verilerini toplamak ve Azure tablolarına yüklemek için başka bir seçenektir.

### <a name="performance-counter-instance-names"></a>Performans sayacı örnek adları
Çok sayıda aktör hizmeti ne de aktör hizmeti bölümleri olan bir küme, çok sayıda aktör performans sayacı örneğine sahip olur. Performans sayacı örnek adları, performans sayacı örneğinin ilişkili olduğu belirli [bölüm](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) ve aktör yöntemini (varsa) tanımlamada yardımcı olabilir.

#### <a name="service-fabric-actor-category"></a>Servis Kumaş Oyuncu kategorisi
Kategori `Service Fabric Actor`için sayaç örnek adları aşağıdaki biçimdedir:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID,* performans sayacı örneğinin ilişkili olduğu Hizmet Kumaşı bölümleme kimliğinin dize gösterimidir. Bölüm kimliği bir GUID'dir ve dize [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) gösterimi biçim belirteci "D" olan yöntem aracılığıyla oluşturulur.

*ActorRuntimeInternalID,* Kumaş Aktörleri tarafından kendi iç kullanımı için çalışma zamanı tarafından oluşturulan 64 bit'lik bir tamsayı dize gösterimidir. Bu, benzersizliğini sağlamak ve diğer performans sayacı örnek adlarıyla çakışmayı önlemek için performans sayacı örnek adına dahildir. Kullanıcılar performans sayacı örnek adının bu bölümünü yorumlamaya çalışmamalıdır.

Kategoriye ait `Service Fabric Actor` bir sayaç için sayaç örneği adı örneği aşağıdaki leri verilmiştir:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

Yukarıdaki örnekte, `2740af29-78aa-44bc-a20b-7e60fb783264` Hizmet Kumaşı bölümleme kimliğinin `635650083799324046` dize gösterimi ve çalışma zamanının iç kullanımı için oluşturulan 64 bit lik kimliktir.

#### <a name="service-fabric-actor-method-category"></a>Servis Kumaş Oyuncu Yöntemi kategorisi
Kategori `Service Fabric Actor Method`için sayaç örnek adları aşağıdaki biçimdedir:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName,* performans sayacı örneğinin ilişkili olduğu aktör yönteminin adıdır. Yöntem adının biçimi, Windows'daki performans sayacı örnek adlarının maksimum uzunluğuüzerindeki kısıtlamalarla adın okunabilirliğini dengeleyen Kumaş Aktörleri çalışma zamanındaki bazı mantığa göre belirlenir.

*ActorsRuntimeMethodId,* Kumaş Aktörleri tarafından kendi iç kullanımı için oluşturulan 32 bit'lik bir tamsayı dize gösterimidir. Bu, benzersizliğini sağlamak ve diğer performans sayacı örnek adlarıyla çakışmayı önlemek için performans sayacı örnek adına dahildir. Kullanıcılar performans sayacı örnek adının bu bölümünü yorumlamaya çalışmamalıdır.

*ServiceFabricPartitionID,* performans sayacı örneğinin ilişkili olduğu Hizmet Kumaşı bölümleme kimliğinin dize gösterimidir. Bölüm kimliği bir GUID'dir ve dize [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) gösterimi biçim belirteci "D" olan yöntem aracılığıyla oluşturulur.

*ActorRuntimeInternalID,* Kumaş Aktörleri tarafından kendi iç kullanımı için çalışma zamanı tarafından oluşturulan 64 bit'lik bir tamsayı dize gösterimidir. Bu, benzersizliğini sağlamak ve diğer performans sayacı örnek adlarıyla çakışmayı önlemek için performans sayacı örnek adına dahildir. Kullanıcılar performans sayacı örnek adının bu bölümünü yorumlamaya çalışmamalıdır.

Kategoriye ait `Service Fabric Actor Method` bir sayaç için sayaç örneği adı örneği aşağıdaki leri verilmiştir:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

Yukarıdaki örnekte, `ivoicemailboxactor.leavemessageasync` yöntem adı, `2` çalışma zamanının iç kullanımı için oluşturulan 32 `89383d32-e57e-4a9b-a6ad-57c6792aa521` bit lik kimlik, Hizmet Dokusu bölüm `635650083804480486` kimliği dize gösterimi ve çalışma zamanının iç kullanımı için oluşturulan 64 bit lik kimliktir.

## <a name="list-of-events-and-performance-counters"></a>Olaylar ve performans sayaçları listesi
### <a name="actor-method-events-and-performance-counters"></a>Aktör yöntemi etkinlikleri ve performans sayaçları
Güvenilir Aktörler çalışma zamanı [aktör yöntemleri](service-fabric-reliable-actors-introduction.md)ile ilgili aşağıdaki olayları yayır.

| Olay adı | Olay Kimliği | Düzey | Anahtar kelime | Açıklama |
| --- | --- | --- | --- | --- |
| ActorMethodStart |7 |Ayrıntılı |0x2 |Aktörler çalışma zamanı bir aktör yöntemi çağırmak üzeredir. |
| ActorMethodStop |8 |Ayrıntılı |0x2 |Bir aktör yöntemi yürütmeyi tamamladı. Diğer bir tarihte, çalışma zamanının aktör yöntemine çağrı yaptığı eşzamanlı çağrı döndürülür ve aktör yöntemiyle döndürülen görev tamamlanır. |
| ActorMethodThrewException |9 |Uyarı |0x3 |Bir özel durum, bir aktör yönteminin yürütülmesi sırasında, çalışma zamanının aktör yöntemine eşzamanlı araması sırasında veya aktör yöntemi yle döndürülen görevin yürütülmesi sırasında atıldı. Bu olay, aktör kodunda incelenmesi gereken bir tür hata olduğunu gösterir. |

Güvenilir Aktörler runtime aktör yöntemlerinin yürütülmesi ile ilgili aşağıdaki performans sayaçları yayınlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Servis Kumaş Aktör Yöntemi |Davetler/Sn |Aktör hizmet yönteminin saniyede çağrılma sayısı |
| Servis Kumaş Aktör Yöntemi |Çağırma başına ortalama milisaniye |Aktör hizmeti yöntemini milisaniye cinsinden yürütmek için alınan süre |
| Servis Kumaş Aktör Yöntemi |Atılan özel durumlar/Sn |Aktör hizmet yönteminin saniyede bir özel durum fırlattık sayısı |

### <a name="concurrency-events-and-performance-counters"></a>Eşzamanlılık olayları ve performans sayaçları
Güvenilir Aktörler çalışma süresi [eşzamanlılık](service-fabric-reliable-actors-introduction.md#concurrency)ile ilgili aşağıdaki olayları yayır.

| Olay adı | Olay Kimliği | Düzey | Anahtar kelime | Açıklama |
| --- | --- | --- | --- | --- |
| AktörMeAramalarWaitingForLock |12 |Ayrıntılı |0x8 |Bu olay bir aktör her yeni dönüş başında yazılır. Sıra tabanlı eşzamanlılığı zorlayan aktör başına kilidi elde etmek için bekleyen aktör çağrılarının sayısını içerir. |

Güvenilir Aktörler çalışma zamanı eşzamanlılıkla ilgili aşağıdaki performans sayaçlarını yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Servis Kumaş Aktör |# aktör kilit bekleyen aktör aramaları |Sıra tabanlı eşzamanlılığı zorlayan aktör başına kilidi elde etmek için bekleyen aktör çağrılarının sayısı |
| Servis Kumaş Aktör |Kilit başına ortalama milisaniye bekleme |Sıra tabanlı eşzamanlılığı zorlayan aktör başına kilidi elde etmek için alınan süre (milisaniye cinsinden) |
| Servis Kumaş Aktör |Ortalama milisaniye aktör kilidi tutuldu |Aktör başına kilidin tutulduğu saat (milisaniye cinsinden) |

### <a name="actor-state-management-events-and-performance-counters"></a>Aktör devlet yönetimi etkinlikleri ve performans sayaçları
Güvenilir Aktörler çalışma zamanı [aktör devlet yönetimi](service-fabric-reliable-actors-state-management.md)ile ilgili aşağıdaki olayları yayır.

| Olay adı | Olay Kimliği | Düzey | Anahtar kelime | Açıklama |
| --- | --- | --- | --- | --- |
| AktörSaveStateStart |10 |Ayrıntılı |0x4 |Aktörler çalışma zamanı aktör devlet kaydetmek üzeredir. |
| AktörSaveStateStop |11 |Ayrıntılı |0x4 |Aktörler runtime aktör devlet tasarrufu bitirdi. |

Güvenilir Aktörler runtime aktör devlet yönetimi ile ilgili aşağıdaki performans sayaçları yayınlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Servis Kumaş Aktör |Kaydet durumu çalışması başına ortalama milisaniye |Milisaniye cinsinden aktör durumunu kurtarmak için alınan süre |
| Servis Kumaş Aktör |Yük durumu çalışması başına ortalama milisaniye |Aktör durumunu milisaniye cinsinden yüklemek için alınan süre |

### <a name="events-related-to-actor-replicas"></a>Aktör yinelemeleri ile ilgili olaylar
Güvenilir Aktörler çalışma zamanı [aktör yinelemeleri](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors)ile ilgili aşağıdaki olayları yayır.

| Olay adı | Olay Kimliği | Düzey | Anahtar kelime | Açıklama |
| --- | --- | --- | --- | --- |
| ReplicaChangeRoletoPrimary |1 |Bilgilendirici |0x1 |Aktör çoğaltma birincil rolü değiştirdi. Bu, bu bölümün aktörlerini bu yineleme içinde oluşturulacağı anlamına gelir. |
| ReplicaChangeRoleFromPrimary |2 |Bilgilendirici |0x1 |Aktör çoğaltma rolünü Birincil olmayan olarak değiştirdi. Bu, bu bölümün aktörlerini artık bu yineleme içinde oluşturulmayacak anlamına gelir. Bu yineleme içinde zaten oluşturulan aktörlere yeni istekler teslim edilecektir. Devam eden istekler tamamlandıktan sonra aktörler yok edilecek. |

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Aktör etkinleştirme ve devre dışı bırakma olayları ve performans sayaçları
Güvenilir Aktörler çalışma süresi [aktör etkinleştirme ve devre dışı bırakma](service-fabric-reliable-actors-lifecycle.md)ile ilgili aşağıdaki olayları yayır.

| Olay adı | Olay Kimliği | Düzey | Anahtar kelime | Açıklama |
| --- | --- | --- | --- | --- |
| AktörAktif |5 |Bilgilendirici |0x1 |Bir aktör etkinleştirildi. |
| Aktör Devre Dışı Bırakıldı |6 |Bilgilendirici |0x1 |Bir aktör devre dışı bırakıldı. |

Güvenilir Aktörler çalışma zamanı, aktör etkinleştirme ve devre dışı bırakma ile ilgili aşağıdaki performans sayaçlarını yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Servis Kumaş Aktör |Ortalama OnActivateAsync miliseconds |OnActivateAsync yöntemini milisaniye cinsinden yürütmek için alınan süre |

### <a name="actor-request-processing-performance-counters"></a>Aktör isteği işleme performans sayaçları
İstemci, bir aktör proxy nesnesi aracılığıyla bir yöntem çağırdığında, ağ üzerinden aktör hizmetine bir istek iletisi gönderilmesiyle sonuçlanır. Hizmet istek iletisini işler ve istemciye bir yanıt gönderir. Güvenilir Aktörler çalışma zamanı, aktör isteği işlemeyle ilgili aşağıdaki performans sayaçlarını yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Servis Kumaş Aktör |# bekleyen isteklerin |Hizmette işlenen istek sayısı |
| Servis Kumaş Aktör |İstek başına ortalama milisaniye |Bir isteği işlemek için hizmet tarafından alınan süre (milisaniye cinsinden) |
| Servis Kumaş Aktör |İstek deserialization için ortalama milisaniye |Hizmetten alındığında aktör istek iletisini deserialize etmek için (milisaniye cinsinden) alınan süre |
| Servis Kumaş Aktör |Yanıt serileştirme için ortalama milisaniye |Yanıt istemciye gönderilmeden önce hizmetteki aktör yanıt iletisini serihale getirmek için alınan süre (milisaniye cinsinden) |

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Aktörler Service Fabric platformını nasıl kullanır?](service-fabric-reliable-actors-platform.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Örnek kod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [PerfView'deki EventSource sağlayıcıları](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
