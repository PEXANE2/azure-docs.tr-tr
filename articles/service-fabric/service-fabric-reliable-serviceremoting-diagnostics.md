---
title: Azure ServiceFabric tanılama ve izleme
description: Bu makalede, Hizmet Kumaşı Güvenilir ServiceRemoting çalışma zamanındaki performans izleme özellikleri açıklanmaktadır, örneğin, onun yaydığı performans sayaçları gibi.
author: suchiagicha
ms.topic: conceptual
ms.date: 06/29/2017
ms.author: pepogors
ms.openlocfilehash: 31095a619fc4d756fa4ef9c29691d1d511d59ece
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282282"
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-service-remoting"></a>Güvenilir Hizmet Remoting için tanılama ve performans izleme
Güvenilir ServiceRemoting çalışma zamanı [performans sayaçları](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx)yakar. Bunlar ServiceRemoting'in nasıl çalıştığına dair öngörüler sağlar ve sorun giderme ve performans izleme konusunda yardımcı olur.


## <a name="performance-counters"></a>Performans sayaçları
Güvenilir ServiceRemoting çalışma süresi aşağıdaki performans sayacı kategorilerini tanımlar:

| Kategori | Açıklama |
| --- | --- |
| Servis Kumaş Servisi |Azure Hizmet Kumaş Hizmet Remoting'ine özgü sayaçlar (örneğin, isteği işlemek için alınan ortalama süre |
| Servis Kumaş Servis Yöntemi |Hizmet Kumaş ı Remoting Service tarafından uygulanan yöntemlere özgü sayaçlar, örneğin, bir hizmet yönteminin ne sıklıkta çağrıldığı |

Önceki kategorilerin her birinde bir veya daha fazla sayaç vardır.

Windows işletim sisteminde varsayılan olarak kullanılabilen [Windows Performans İzleyicisi](https://technet.microsoft.com/library/cc749249.aspx) uygulaması, performans sayacı verilerini toplamak ve görüntülemek için kullanılabilir. [Azure Tanılama,](../cloud-services/cloud-services-dotnet-diagnostics.md) performans sayacı verilerini toplamak ve Azure tablolarına yüklemek için başka bir seçenektir.

### <a name="performance-counter-instance-names"></a>Performans sayacı örnek adları
Çok sayıda ServiceRemoting hizmetine veya bölümüne sahip bir kümenin çok sayıda performans sayacı örneği vardır. Performans sayacı örnek adları, performans sayacı örneğinin ilişkili olduğu belirli bölüm ve Hizmet yöntemini (varsa) tanımlamada yardımcı olabilir.

#### <a name="service-fabric-service-category"></a>Servis Kumaş Servisi kategorisi
Kategori `Service Fabric Service`için sayaç örnek adları aşağıdaki biçimdedir:

`ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*ServiceFabricPartitionID,* performans sayacı örneğinin ilişkili olduğu Hizmet Kumaşı bölümleme kimliğinin dize gösterimidir. Bölüm kimliği bir GUID'dir ve dize [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) gösterimi biçim belirteci "D" olan yöntem aracılığıyla oluşturulur.

*ServiceReplicaOrInstanceId,* performans sayacı örneğinin ilişkili olduğu Hizmet Dokusu Çoğaltma/Örnek Kimliği dize gösterimidir.

*ServiceRuntimeInternalID,* Kumaş Hizmeti çalışma süresi tarafından kendi iç kullanımı için oluşturulan 64 bit'lik bir tamsayı dize temsilidir. Bu, benzersizliğini sağlamak ve diğer performans sayacı örnek adlarıyla çakışmayı önlemek için performans sayacı örnek adına dahildir. Kullanıcılar performans sayacı örnek adının bu bölümünü yorumlamaya çalışmamalıdır.

Kategoriye ait `Service Fabric Service` bir sayaç için sayaç örneği adı örneği aşağıdaki leri verilmiştir:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046_5008379932`

Önceki örnekte, `2740af29-78aa-44bc-a20b-7e60fb783264` Hizmet Dokusu bölüm kimliği dize `635650083799324046` gösterimi, Çoğaltma/InstanceId `5008379932` dize gösterimidir ve çalışma zamanının iç kullanımı için oluşturulan 64 bitlik kimliktir.

#### <a name="service-fabric-service-method-category"></a>Servis Kumaş Servis Yöntemi kategorisi
Kategori `Service Fabric Service Method`için sayaç örnek adları aşağıdaki biçimdedir:

`MethodName_ServiceRuntimeMethodId_ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*MethodName,* performans sayacı örneğinin ilişkili olduğu hizmet yönteminin adıdır. Yöntem adının biçimi, Windows'daki performans sayacı örnek adlarının maksimum uzunluğuüzerindeki kısıtlamalarla adın okunabilirliğini dengeleyen Kumaş Hizmeti çalışma zamanındaki bazı mantığa göre belirlenir.

*ServiceRuntimeMethodId,* Kumaş Hizmeti çalışma süresi tarafından kendi iç kullanımı için oluşturulan 32 bit'lik bir tamsayı dize gösterimidir. Bu, benzersizliğini sağlamak ve diğer performans sayacı örnek adlarıyla çakışmayı önlemek için performans sayacı örnek adına dahildir. Kullanıcılar performans sayacı örnek adının bu bölümünü yorumlamaya çalışmamalıdır.

*ServiceFabricPartitionID,* performans sayacı örneğinin ilişkili olduğu Hizmet Kumaşı bölümleme kimliğinin dize gösterimidir. Bölüm kimliği bir GUID'dir ve dize [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) gösterimi biçim belirteci "D" olan yöntem aracılığıyla oluşturulur.

*ServiceReplicaOrInstanceId,* performans sayacı örneğinin ilişkili olduğu Hizmet Dokusu Çoğaltma/Örnek Kimliği dize gösterimidir.

*ServiceRuntimeInternalID,* Kumaş Hizmeti çalışma süresi tarafından kendi iç kullanımı için oluşturulan 64 bit'lik bir tamsayı dize temsilidir. Bu, benzersizliğini sağlamak ve diğer performans sayacı örnek adlarıyla çakışmayı önlemek için performans sayacı örnek adına dahildir. Kullanıcılar performans sayacı örnek adının bu bölümünü yorumlamaya çalışmamalıdır.

Kategoriye ait `Service Fabric Service Method` bir sayaç için sayaç örneği adı örneği aşağıdaki leri verilmiştir:

`ivoicemailboxservice.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486_5008380`

`ivoicemailboxservice.leavemessageasync` Önceki örnekte, yöntem adı, `2` çalışma zamanının iç kullanımı için oluşturulan 32 bit `89383d32-e57e-4a9b-a6ad-57c6792aa521` kimliktir, Hizmet Dokusu bölümleme`635650083804480486` kimliğinin dize gösterimidir, Hizmet `5008380` Kumaşı Çoğaltma/Örnek Kimliği'nin dize gösterimidir ve çalışma zamanının iç kullanımı için oluşturulan 64 bit kimliktir.

## <a name="list-of-performance-counters"></a>Performans sayaçları listesi
### <a name="service-method-performance-counters"></a>Servis yöntemi performans sayaçları

Güvenilir Hizmet çalışma zamanı, hizmet yöntemlerinin yürütülmesiyle ilgili aşağıdaki performans sayaçlarını yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Servis Kumaş Servis Yöntemi |Davetler/Sn |Hizmet yönteminin saniyede çağrılma sayısı |
| Servis Kumaş Servis Yöntemi |Çağırma başına ortalama milisaniye |Hizmet yöntemini milisaniye cinsinden yürütmek için alınan süre |
| Servis Kumaş Servis Yöntemi |Atılan özel durumlar/Sn |Hizmet yönteminin saniyede özel durum atma sayısı |

### <a name="service-request-processing-performance-counters"></a>Servis isteği işleme performans sayaçları
İstemci bir hizmet proxy nesnesi üzerinden bir yöntem çağırdığında, bir istek iletisi remoting hizmetine ağ üzerinden gönderiliyor sonuçlanır. Hizmet istek iletisini işler ve istemciye bir yanıt gönderir. Güvenilir ServiceRemoting çalışma zamanı, hizmet isteği işlemeyle ilgili aşağıdaki performans sayaçlarını yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Servis Kumaş Servisi |# bekleyen isteklerin |Hizmette işlenen istek sayısı |
| Servis Kumaş Servisi |İstek başına ortalama milisaniye |Bir isteği işlemek için hizmet tarafından alınan süre (milisaniye cinsinden) |
| Servis Kumaş Servisi |İstek deserialization için ortalama milisaniye |Hizmetten alındığında hizmet isteği iletisini deserialize etmek için alınan süre (milisaniye cinsinden) |
| Servis Kumaş Servisi |Yanıt serileştirme için ortalama milisaniye |Yanıt istemciye gönderilmeden önce servisteki servis yanıt iletisini seri hale getirmek için alınan süre (milisaniye cinsinden) |

## <a name="next-steps"></a>Sonraki adımlar
* [Örnek kod](https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0)
* [PerfView'deki EventSource sağlayıcıları](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
