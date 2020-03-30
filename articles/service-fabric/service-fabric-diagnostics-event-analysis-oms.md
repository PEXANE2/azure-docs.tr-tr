---
title: Azure Monitör günlükleri ile Azure Hizmet Kumaş Olay Analizi
description: Azure Hizmet Dokusu kümelerinin izlenmesi ve teşhisi için Azure Monitor günlüklerini kullanarak olayları görselleştirme ve çözümleme hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: srrengar
ms.openlocfilehash: 40dd930aa21e3056d5ecc908359215d6874ed8ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464745"
---
# <a name="event-analysis-and-visualization-with-azure-monitor-logs"></a>Azure Monitor günlükleri ile olay analizi ve görselleştirme
 Azure Monitor günlükleri bulutta barındırılan uygulama ve hizmetlerden telemetri toplar ve analiz eder ve bunların kullanılabilirliğini ve performansını en üst düzeye çıkarmanıza yardımcı olacak analiz araçları sağlar. Bu makalede, öngörüler elde etmek ve kümenizde neler olup bittiğini gidermek için Azure Monitor günlüklerinde sorguların nasıl çalıştırılacaklandığı özetlenmektedir. Aşağıdaki sık sorulan sorular ele alınmıştır:

* Sağlık olaylarını nasıl giderebilirim?
* Bir düğümün ne zaman batacağını nasıl bileceğim?
* Uygulamamın hizmetlerinin başlatıldığında veya durdurulup durdurulmadığını nasıl anlarım?

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="overview-of-the-log-analytics-workspace"></a>Log Analytics çalışma alanına genel bakış

>[!NOTE] 
>Tanılama depolama küme oluşturma zamanında varsayılan olarak etkin olsa da, tanılama depolama okuması için Log Analytics çalışma alanını ayarlamanız gerekir.

Azure Monitor günlükleri, Azure depolama tablosu veya aracı dahil olmak üzere yönetilen kaynaklardan veri toplar ve merkezi bir depoda saklar. Veriler daha sonra analiz, uyarı ve görselleştirme veya daha fazla dışa aktarma için kullanılabilir. Azure Monitor günlükleri olayları, performans verilerini veya diğer özel verileri destekler. Verilerin Azure Monitor günlüklerine aktığından emin olmak [için depolamadaki olaylardan okunacak bir Log Analytics çalışma alanı oluşturmak için](service-fabric-diagnostics-oms-setup.md) [tanılama uzantısını toplu olaylara](service-fabric-diagnostics-event-aggregation-wad.md) ve adımlara yapılandırma adımlarını göz atın.

Veriler Azure Monitor günlükleri tarafından alındıktan sonra, Azure'da, çeşitli senaryolara göre özelleştirilmiş, gelen verileri izlemek için önceden paketlenmiş çözümler veya operasyonel panolar olan çeşitli *İzleme Çözümleri* bulunur. Bunlar arasında *Service Fabric Analytics* çözümü ve Service Fabric kümelerini kullanırken tanılama ve izlemeyle en alakalı iki çözüm yer alan *Kapsayıcılar* çözümü yer almaktadır. Bu makalede, çalışma alanı yla oluşturulan Hizmet Kumaş Analizi çözümlerinin nasıl kullanılacağı açıklanmaktadır.

## <a name="access-the-service-fabric-analytics-solution"></a>Service Fabric Analytics çözümüne erişin

Azure [Portalı'nda,](https://portal.azure.com)Service Fabric Analytics çözümlerini oluşturduğunuz kaynak grubuna gidin.

Kaynak **ServiceFabric\<adını seçinOfOMSWorkspace\>**.

, `Summary`Hizmet Kumaşı için bir tane de dahil olmak üzere, etkinleştirilen çözümlerin her biri için grafik şeklinde kutucuklar görürsünüz. Service **Fabric** Analytics çözümüne devam etmek için Service Fabric grafiğini tıklatın.

![Servis Kumaş çözümü](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_summary.PNG)

Aşağıdaki resimde Service Fabric Analytics çözümünün ana sayfası gösterilmektedir. Bu ana sayfa, kümenizde neler olup bittiğine dair anlık görüntü sağlar.

![Servis Kumaş çözümü](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_solution.PNG)

 Küme oluşturma üzerine tanılamayı etkinleştirdiyseniz, 

* [Servis Kumaş küme etkinlikleri](service-fabric-diagnostics-event-generation-operational.md)
* [Güvenilir Aktörler programlama modeli olaylar](service-fabric-reliable-actors-diagnostics.md)
* [Güvenilir Hizmetler programlama modeli olaylar](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>Kutunun dışında Hizmet Kumaş olaylara ek olarak, daha ayrıntılı sistem olayları [tanılama uzantısı config güncelleyerek](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations)toplanabilir.

## <a name="view-service-fabric-events-including-actions-on-nodes"></a>Düğümlerle ilgili eylemler de dahil olmak üzere Hizmet Kumaş Etkinliklerini görüntüleyin

Service Fabric Analytics sayfasında, **Servis Kumaş Etkinlikleri**için grafiğe tıklayın.

![Servis Kumaş Çözüm Operasyonel Kanal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events_selection.png)

Listedeki olayları görüntülemek için **Liste'yi** tıklatın. Buraya geldikten sonra toplanan tüm sistem olaylarını göreceksiniz. Başvuru için bunlar Azure Depolama hesabındaki **WADServiceFabricSystemEventsTable'dan** gelir ve benzer şekilde bir sonraki gördüğünüz güvenilir hizmetler ve aktörler etkinlikleri de ilgili tablolardan dır.
    
![Operasyonel Kanalı Sorgula](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events.png)

Alternatif olarak, soldaki büyüteç tıklayın ve aradığınızı bulmak için Kusto sorgu dilini kullanabilirsiniz. Örneğin, kümedeki düğümler üzerinde yapılan tüm eylemleri bulmak için aşağıdaki sorguyu kullanabilirsiniz. Aşağıda kullanılan olay işlleri [operasyonel kanal olayları](service-fabric-diagnostics-event-generation-operational.md)referansında bulunur.

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

Belirli düğümler (Bilgisayar) sistem hizmeti (TaskName) gibi daha birçok alanda sorgu yapabilirsiniz.

## <a name="view-service-fabric-reliable-service-and-actor-events"></a>View Servis Kumaş Güvenilir Hizmet ve Aktör olaylar

Hizmet Kumaş Analizi sayfasında, **Güvenilir Hizmetler**için grafiği tıklatın.

![Servis Kumaş Çözümü Güvenilir Hizmetler](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_services_events_selection.png)

Listedeki olayları görüntülemek için **Liste'yi** tıklatın. Burada güvenilir hizmetlerden olayları görebilirsiniz. Hizmet runasync başlatıldığında ve genellikle dağıtımlar ve yükseltmeleri olur tamamlandığında için farklı olaylar görebilirsiniz. 

![Güvenilir Hizmetleri Sorgula](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_service_events.png)

Güvenilir aktör olayları benzer bir şekilde izlenebilir. Güvenilir aktörler için daha ayrıntılı olayları yapılandırmak `scheduledTransferKeywordFilter` için tanı uzantısı için config'dekini değiştirmeniz gerekir (aşağıda gösterilmiştir). Bunlar için değerler ayrıntıları [güvenilir aktörler olaylar referans](service-fabric-reliable-actors-diagnostics.md#keywords)bulunmaktadır.

```json
"EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
```

Kusto sorgu dili güçlüdür. Çalıştırabileceğiniz bir diğer değerli sorgu, hangi düğümlerin en çok olay ürettiğini bulmaktır. Aşağıdaki ekran görüntüsündeki sorgu, service Fabric operasyonel olayları belirli hizmet ve düğümle biraraya getirir.

![Düğüm Başına Olayları Sorgula](media/service-fabric-diagnostics-event-analysis-oms/oms_kusto_query.png)

## <a name="next-steps"></a>Sonraki adımlar

* Altyapı izlemeyi, yani performans sayaçlarını etkinleştirmek için [Log Analytics aracısını eklemeye](service-fabric-diagnostics-oms-agent.md)gidin. Aracı performans sayaçları toplar ve bunları varolan çalışma alanınıza ekler.
* Şirket içi kümeler için Azure Monitor günlükleri, Azure Monitor günlüklerine veri göndermek için kullanılabilecek bir Ağ Geçidi (HTTP İleri Proxy) sunar. Bu konuda daha fazla bilgi edinin, [Log Analytics ağ geçidini kullanarak Azure Monitor günlüklerine Internet erişimi olmayan bilgisayarları bağlama'da](../azure-monitor/platform/gateway.md).
* Algılama ve tanılamada yardımcı olmak için [otomatik uyarıyı](../log-analytics/log-analytics-alerts.md) yapılandırın.
* Azure Monitor günlüklerinin bir parçası olarak sunulan [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) özelliklerine alışın.
* Azure Monitor günlükleri ve neler sunduğunun daha ayrıntılı bir özetini alın, [Azure Monitor günlükleri nedir?](../operations-management-suite/operations-management-suite-overview.md)
