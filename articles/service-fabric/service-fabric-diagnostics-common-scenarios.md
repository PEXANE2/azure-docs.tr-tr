---
title: Azure Hizmet Kumaşı Sık Karşılaşılan Senaryoları Tanıla
description: Azure Service Fabric uygulamalarında sık karşılaşılan izleme ve tanılama senaryoları ile ilgili sorun giderme hakkında bilgi edinin.
ms.topic: article
ms.date: 02/25/2019
ms.openlocfilehash: 3c7f027bad71d48db5fba002f778f23db8225fa5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906955"
---
# <a name="diagnose-common-scenarios-with-service-fabric"></a>Service Fabric ile sık karşılaşılan senaryoları tanılama

Bu makalede, kullanıcıların Hizmet Kumaşı ile izleme ve tanılama alanında karşılaştıkları yaygın senaryolar gösterilmiştir. Sunulan senaryolar hizmet dokusunun 3 katmanının tümünü kapsar: Uygulama, Küme ve Altyapı. Her çözüm, her senaryoyu tamamlamak için Uygulama Öngörüleri ve Azure Monitör günlükleri, Azure izleme araçları kullanır. Her çözümdeki adımlar, kullanıcılara Hizmet Dokusu bağlamında Uygulama Öngörüleri ve Azure Monitör günlüklerinin nasıl kullanılacağı hakkında bir giriş sağlar.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites-and-recommendations"></a>Önkoşullar ve Öneriler

Bu makaledeki çözümler aşağıdaki araçları kullanır. Bunları ayarlamanızı ve yapılandırmanızı öneririz:

* [Hizmet Kumaşı ile Uygulama Öngörüleri](service-fabric-tutorial-monitoring-aspnet.md)
* [Kümenizde Azure Tanılama'yı etkinleştirme](service-fabric-diagnostics-event-aggregation-wad.md)
* [Log Analytics çalışma alanı ayarlama](service-fabric-diagnostics-oms-setup.md)
* [Performans Sayaçlarını izlemek için Günlük Analytics aracısı](service-fabric-diagnostics-oms-agent.md)

## <a name="how-can-i-see-unhandled-exceptions-in-my-application"></a>Uygulamamda işlenmemiş özel durumları nasıl görebiliyorum?

1. Uygulamanızın yapılandırıldıdığı Application Insights kaynağınıza gidin.
2. Sol üstteki *Arama'ya* tıklayın. Ardından bir sonraki panelde filtreyi tıklatın.

    ![AI Genel Bakış](media/service-fabric-diagnostics-common-scenarios/ai-search-filter.png)

3. Birçok olay türü (izlemeler, istekler, özel olaylar) görürsünüz. Filtreniz olarak "Özel Durum"u seçin.

    ![AI Filtre Listesi](media/service-fabric-diagnostics-common-scenarios/ai-filter-list.png)

    Listedeki bir özel durumu tıklatarak, Service Fabric Application Insights SDK kullanıyorsanız hizmet bağlamı da dahil olmak üzere daha fazla ayrıntıya bakabilirsiniz.

    ![AI Özel Durum](media/service-fabric-diagnostics-common-scenarios/ai-exception.png)

## <a name="how-do-i-view-which-http-calls-are-used-in-my-services"></a>Hizmetlerimde hangi HTTP çağrılarının kullanıldığını nasıl görüntüleyebilirim?

1. Aynı Application Insights kaynağında, özel durumlar yerine "isteklere" filtre uygulayabilir ve yapılan tüm istekleri görüntüleyebilirsiniz
2. Service Fabric Application Insights SDK'yı kullanıyorsanız, hizmetlerinizin birbirine bağlı görsel bir temsilini ve başarılı ve başarısız isteklerin sayısını görebilirsiniz. Soldaki "Uygulama Haritası"

    ![AI App](media/service-fabric-diagnostics-common-scenarios/app-map-blade.png) ![Harita Blade AI Uygulama Haritası](media/service-fabric-diagnostics-common-scenarios/app-map-new.png)

    Başvuru haritası hakkında daha fazla bilgi için [Başvuru Haritası belgelerini](../azure-monitor/app/app-map.md) ziyaret edin

## <a name="how-do-i-create-an-alert-when-a-node-goes-down"></a>Düğüm küçüldüğünde nasıl bir uyarı oluştururum?

1. Düğüm olayları Servis Kumaşı kümeniz tarafından izlenir. **ServiceFabric(NameofResourceGroup)** adlı Service Fabric Analytics çözüm kaynağına gidin
2. Bıçağın altındaki "Özet" başlıklı grafiğe tıklayın

    ![Azure Monitor günlükleri çözümü](media/service-fabric-diagnostics-common-scenarios/oms-solution-azure-portal.png)

3. Burada çeşitli ölçümleri görüntüleyen birçok grafik ve kutucuk bulunmaktadır. Grafiklerden birine tıkladığınızda sizi Günlük Arama'ya götürür. Burada herhangi bir küme olayları veya performans sayaçları için sorgulayabilirsiniz.
4. Aşağıdaki sorguyu girin. Bu olay iAnda'ları [Düğüm olayları referansbulunur](service-fabric-diagnostics-event-generation-operational.md#application-events)

    ```kusto
    ServiceFabricOperationalEvent
    | where EventID >= 25622 and EventID <= 25626
    ```

5. En üstteki "Yeni Uyarı Kuralı"nı tıklayın ve şimdi bu sorguya dayalı bir olay geldiğinde seçtiğiniz iletişim yönteminde bir uyarı alırsınız.

    ![Azure Monitör yeni uyarıyı kaydeder](media/service-fabric-diagnostics-common-scenarios/oms-create-alert.png)

## <a name="how-can-i-be-alerted-of-application-upgrade-rollbacks"></a>Uygulama yükseltme geri almalar konusunda nasıl uyarılabilirim?

1. Yükseltme geri dönüşleri için aşağıdaki sorguyu girmeden önce olduğu gibi aynı Günlük Arama penceresinde. Bu olay iT'leri [Uygulama olayları başvurusu](service-fabric-diagnostics-event-generation-operational.md#application-events) altında bulunur

    ```kusto
    ServiceFabricOperationalEvent
    | where EventID == 29623 or EventID == 29624
    ```

2. En üstteki "Yeni Uyarı Kuralı"nı tıklatın ve şimdi bu sorguya dayalı bir olay geldiğinde bir uyarı alırsınız.

## <a name="how-do-i-see-container-metrics"></a>Konteyner ölçümlerini nasıl görebiliyorum?

Tüm grafiklerle aynı görünümde, kapsayıcılarınızın performansı için bazı döşemeler görürsünüz. Bu döşemelerin doldurulması için Log Analytics Agent ve [Konteyner İzleme çözümüne](service-fabric-diagnostics-oms-containers.md) ihtiyacınız vardır.

![Log Analytics Konteyner Ölçümleri](media/service-fabric-diagnostics-common-scenarios/containermetrics.png)

>[!NOTE]
>Kabınızın **içinden** telemetri yapmak [için kaplar için Uygulama Öngörüleri nuget paketini](https://github.com/Microsoft/ApplicationInsights-servicefabric#microsoftapplicationinsightsservicefabric--for-service-fabric-lift-and-shift-scenarios)eklemeniz gerekir.

## <a name="how-can-i-monitor-performance-counters"></a>Performans sayaçlarını nasıl izleyebilirim?

1. Log Analytics aracısını kümenize ekledikten sonra, izlemek istediğiniz belirli performans sayaçlarını eklemeniz gerekir. Portaldaki Log Analytics çalışma alanının sayfasına gidin – çözümün sayfasından çalışma alanı sekmesi sol menüde yer almaktadır.

    ![Günlük Analitik Çalışma Alanı Sekmesi](media/service-fabric-diagnostics-common-scenarios/workspacetab.png)

2. Çalışma alanının sayfasına çıktıktan sonra, aynı sol menüdeki "Gelişmiş ayarlar"a tıklayın.

    ![Günlük Analitik Gelişmiş Ayarlar](media/service-fabric-diagnostics-common-scenarios/advancedsettingsoms.png)

3. Log Analytics aracısı aracılığıyla düğümlerinizden belirli sayaçları toplamaya başlamak için Windows Performans Sayaçları > (Linux makineleri için Linux performans sayaçları > Veri) tıklayın. Aşağıda sayaçların ekleyleyebileceği biçim örnekleri verilmiştir

   * `.NET CLR Memory(<ProcessNameHere>)\\# Total committed Bytes`
   * `Processor(_Total)\\% Processor Time`

     Hızlı başlangıçta, VotingData ve VotingWeb kullanılan işlem adlarıdır, bu nedenle bu sayaçları izleme

   * `.NET CLR Memory(VotingData)\\# Total committed Bytes`
   * `.NET CLR Memory(VotingWeb)\\# Total committed Bytes`

     ![Log Analytics Perf Sayaçları](media/service-fabric-diagnostics-common-scenarios/omsperfcounters.png)

4. Bu, altyapınızın iş yüklerinizi nasıl ele aldığını görmenizi ve kaynak kullanımına göre ilgili uyarıları ayarlamanızı sağlar. Örneğin - toplam İşlemci kullanımı %90'ın üzerinde veya %5'in altındaysa bir uyarı ayarlamak isteyebilirsiniz. Bunun için kullanacağınız sayaç adı "%İşlemci Zamanı" dır. Bunu, aşağıdaki sorgu için bir uyarı kuralı oluşturarak yapabilirsiniz:

    ```kusto
    Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" | where CounterValue >= 90 or CounterValue <= 5.
    ```

## <a name="how-do-i-track-performance-of-my-reliable-services-and-actors"></a>Güvenilir Hizmetlerimin ve Aktörlerimin performansını nasıl takip edebilirim?

Uygulamalarınızdaki Güvenilir Hizmetlerin veya Aktörlerin performansını izlemek için, Servis Kumaşı Aktörü, Aktör Yöntemi, Hizmet ve Hizmet Yöntemi sayaçlarını da toplamanız gerekir. Burada toplamak için güvenilir hizmet ve aktör performans sayaçları örnekleri

>[!NOTE]
>Service Fabric performans sayaçları şu anda Log Analytics aracısı tarafından toplanamaz, ancak [diğer tanılama çözümleri](service-fabric-diagnostics-partners.md) tarafından toplanabilir

* `Service Fabric Service(*)\\Average milliseconds per request`
* `Service Fabric Service Method(*)\\Invocations/Sec`
* `Service Fabric Actor(*)\\Average milliseconds per request`
* `Service Fabric Actor Method(*)\\Invocations/Sec`

Güvenilir [Hizmetler](service-fabric-reliable-serviceremoting-diagnostics.md) ve [Aktörler'deki](service-fabric-reliable-actors-diagnostics.md) performans sayaçlarının tam listesi için bu bağlantıları kontrol edin

## <a name="next-steps"></a>Sonraki adımlar

* [Ortak Kod Paketi Etkinleştirme Hatalarını Ara](./service-fabric-diagnostics-code-package-errors.md)
* Performans veya kullanımdaki değişikliklerden haberdar olmak için [AI'deki Uyarıları ayarlama](../azure-monitor/app/alerts.md)
* [Uygulama Öngörülerinde Akıllı Algılama,](../azure-monitor/app/proactive-diagnostics.md) olası performans sorunları konusunda sizi uyarmak için AI'ye gönderilen telemetrinin proaktif bir analizini gerçekleştirir
* Algılama ve tanılama konusunda [uyarı sağlayan](../log-analytics/log-analytics-alerts.md) Azure Monitor günlükleri hakkında daha fazla bilgi edinin.
* Şirket içi kümeler için Azure Monitor günlükleri, Azure Monitor günlüklerine veri göndermek için kullanılabilecek bir ağ geçidi (HTTP Forward Proxy) sunar. [Log Analytics ağ geçidini kullanarak Azure Monitor günlüklerine Internet erişimi olmayan bilgisayarları bağlama](../azure-monitor/platform/gateway.md) da bu konuda daha fazla bilgi edinin
* Azure Monitor [günlüklerinin](../log-analytics/log-analytics-log-searches.md) bir parçası olarak sunulan günlük arama ve sorgulama özelliklerine aşina olun
* Azure Monitor günlükleri ve neler sunduğunun daha ayrıntılı bir özetini alın, [Azure Monitor günlükleri nedir okuyun?](../operations-management-suite/operations-management-suite-overview.md)
