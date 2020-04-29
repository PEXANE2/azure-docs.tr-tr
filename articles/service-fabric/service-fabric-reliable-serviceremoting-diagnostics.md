---
title: Azure ServiceFabric tanılama ve izleme
description: Bu makalede, Service Fabric güvenilir ServiceRemoting çalışma zamanının, BT tarafından yayılan performans sayaçları gibi performans izleme özellikleri açıklanmaktadır.
author: suchiagicha
ms.topic: conceptual
ms.date: 06/29/2017
ms.author: pepogors
ms.openlocfilehash: 31095a619fc4d756fa4ef9c29691d1d511d59ece
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79282282"
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-service-remoting"></a>Güvenilir hizmet uzaktan Iletişimi için tanılama ve performans izleme
Güvenilir ServiceRemoting çalışma zamanı, [performans sayaçlarını](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx)yayar. Bunlar ServiceRemoting 'in nasıl çalıştığı hakkında Öngörüler ve sorun giderme ve performans izleme konularında yardım sağlar.


## <a name="performance-counters"></a>Performans sayaçları
Güvenilir ServiceRemoting çalışma zamanı, aşağıdaki performans sayacı kategorilerini tanımlar:

| Kategori | Açıklama |
| --- | --- |
| Service Fabric hizmeti |Azure Service Fabric Service Remoting 'e özgü sayaçlar, örneğin, isteği işlemek için geçen ortalama süre |
| Service Fabric hizmeti yöntemi |Service Fabric Remoting hizmeti tarafından uygulanan yöntemlere özgü sayaçlar, örneğin, bir hizmet yönteminin ne sıklıkla çağrıldığı. |

Yukarıdaki kategorilerin her birinde bir veya daha fazla sayaç bulunur.

Windows işletim sisteminde varsayılan olarak kullanılabilir olan [Windows Performans İzleyicisi](https://technet.microsoft.com/library/cc749249.aspx) uygulaması, performans sayacı verilerini toplamak ve görüntülemek için kullanılabilir. [Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md) , performans sayacı verilerini toplamaya ve Azure tablolarına yüklemeye yönelik başka bir seçenektir.

### <a name="performance-counter-instance-names"></a>Performans sayacı örnek adları
Çok sayıda Serviceretıting hizmeti veya bölümü olan bir küme, çok sayıda performans sayacı örneğine sahiptir. Performans sayacı örneği adları, performans sayacı örneğinin ilişkilendirildiği belirli bölümü ve hizmet yöntemini (varsa) tanımlamaya yardımcı olabilir.

#### <a name="service-fabric-service-category"></a>Service Fabric hizmet kategorisi
Kategori `Service Fabric Service`için, sayaç örneği adları aşağıdaki biçimdedir:

`ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*Servicefabricpartitionıd* , performans sayacı örneğinin ilişkilendirildiği SERVICE fabrıc bölüm kimliğinin dize gösterimidir. Bölüm KIMLIĞI bir GUID 'dir ve dize temsili, Biçim belirleyicisi "D" olan [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) yöntem aracılığıyla oluşturulur.

*Servicerepereporınstanceıd* , performans sayacı örneğinin Ilişkilendirildiği Service Fabric çoğaltma/örnek kimliğinin dize gösterimidir.

*Serviceruntimeınternalıd* , iç kullanımı Için yapı hizmeti çalışma zamanı tarafından oluşturulan 64 bitlik bir tamsayının dize gösterimidir. Bu, benzersizlik sağlamak ve diğer performans sayacı örneği adlarıyla çakışmamak için performans sayacı örneği adına dahildir. Kullanıcılar, performans sayacı örneği adının bu bölümünü yorumlama denememelidir.

Aşağıda, `Service Fabric Service` kategoriye ait bir sayaca yönelik sayaç örneği adının bir örneği verilmiştir:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046_5008379932`

Önceki örnekte, `2740af29-78aa-44bc-a20b-7e60fb783264` SERVICE fabrıc bölüm kimliğinin dize gösterimidir, `635650083799324046` Replica/InstanceId 'nin dize gösterimidir ve `5008379932` çalışma zamanının Iç kullanımı için oluşturulan 64 bitlik kimliğidir.

#### <a name="service-fabric-service-method-category"></a>Service Fabric hizmet yöntemi kategorisi
Kategori `Service Fabric Service Method`için, sayaç örneği adları aşağıdaki biçimdedir:

`MethodName_ServiceRuntimeMethodId_ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*MethodName* , performans sayacı örneğinin ilişkilendirildiği hizmet yönteminin adıdır. Yöntem adının biçimi, ad okunabilirliğini Windows üzerinde performans sayacı örnek adlarının en fazla uzunluğu olan kısıtlamalarla dengeleyen yapı hizmeti çalışma zamanındaki bazı mantığa göre belirlenir.

*Serviceruntimemethodıd* , iç kullanımı Için yapı hizmeti çalışma zamanı tarafından oluşturulan 32 bitlik bir tamsayının dize gösterimidir. Bu, benzersizlik sağlamak ve diğer performans sayacı örneği adlarıyla çakışmamak için performans sayacı örneği adına dahildir. Kullanıcılar, performans sayacı örneği adının bu bölümünü yorumlama denememelidir.

*Servicefabricpartitionıd* , performans sayacı örneğinin ilişkilendirildiği SERVICE fabrıc bölüm kimliğinin dize gösterimidir. Bölüm KIMLIĞI bir GUID 'dir ve dize temsili, Biçim belirleyicisi "D" olan [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) yöntem aracılığıyla oluşturulur.

*Servicerepereporınstanceıd* , performans sayacı örneğinin Ilişkilendirildiği Service Fabric çoğaltma/örnek kimliğinin dize gösterimidir.

*Serviceruntimeınternalıd* , iç kullanımı Için yapı hizmeti çalışma zamanı tarafından oluşturulan 64 bitlik bir tamsayının dize gösterimidir. Bu, benzersizlik sağlamak ve diğer performans sayacı örneği adlarıyla çakışmamak için performans sayacı örneği adına dahildir. Kullanıcılar, performans sayacı örneği adının bu bölümünü yorumlama denememelidir.

Aşağıda, `Service Fabric Service Method` kategoriye ait bir sayaca yönelik sayaç örneği adının bir örneği verilmiştir:

`ivoicemailboxservice.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486_5008380`

`ivoicemailboxservice.leavemessageasync` Yukarıdaki örnekte, yöntem adı, `2` çalışma zamanının iç kullanımı `89383d32-e57e-4a9b-a6ad-57c6792aa521` için oluşturulan 32 bitlik kimlik, Service Fabric bölüm kimliğinin`635650083804480486` dize gösterimi, Service Fabric çoğaltması/örnek kimliğinin dize gösterimidir ve `5008380` çalışma zamanının iç kullanımı için oluşturulan 64 bit kimliğidir.

## <a name="list-of-performance-counters"></a>Performans sayaçları listesi
### <a name="service-method-performance-counters"></a>Hizmet yöntemi performans sayaçları

Güvenilir hizmet çalışma zamanı, hizmet yöntemlerinin yürütülmesi ile ilgili aşağıdaki performans sayaçlarını yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric hizmeti yöntemi |Saniyedeki çağırma sayısı |Hizmet yönteminin saniye başına çağrılme sayısı |
| Service Fabric hizmeti yöntemi |Çağrı başına ortalama milisaniye |Hizmet yönteminin yürütülmesi için harcanan süre (milisaniye) |
| Service Fabric hizmeti yöntemi |Oluşturulan özel durumlar/sn |Hizmet yönteminin saniye başına özel durum kaç kez aldığı |

### <a name="service-request-processing-performance-counters"></a>Hizmet isteği işleme performans sayaçları
Bir istemci bir hizmet proxy nesnesi aracılığıyla bir yöntemi çağırdığında, ağ üzerinden uzaktan iletişim hizmetine bir istek iletisi gönderilmesine neden olur. Hizmet, istek iletisini işler ve istemciye geri yanıt gönderir. Güvenilir ServiceRemoting çalışma zamanı, hizmet isteği işlemeyle ilgili aşağıdaki performans sayaçlarını yayınlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric hizmeti |bekleyen istek sayısı |Hizmette işlenmekte olan istek sayısı |
| Service Fabric hizmeti |İstek başına ortalama milisaniye |Bir isteği işlemek için hizmete göre geçen süre (milisaniye cinsinden) |
| Service Fabric hizmeti |İstek seri durumundan çıkarma için ortalama milisaniye |Hizmette alındığı sırada hizmet isteği iletisinin serisini kaldırmak için geçen süre (milisaniye cinsinden) |
| Service Fabric hizmeti |Yanıt serileştirme ortalama milisaniyesi |Yanıt istemciye gönderilmeden önce hizmette hizmet yanıt iletisini serileştirmek için geçen süre (milisaniye cinsinden) |

## <a name="next-steps"></a>Sonraki adımlar
* [Örnek kod](https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0)
* [PerfView 'da EventSource sağlayıcıları](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
