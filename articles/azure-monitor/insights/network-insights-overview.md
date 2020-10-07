---
title: Ağlar için Azure Izleyici (Önizleme)
description: Hiçbir yapılandırma olmadan dağıtılan tüm ağ kaynakları için sistem durumu ve ölçümlerinin kapsamlı bir görünümünü sağlayan ağ için Azure Izleyici 'ye hızlı bir genel bakış.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/24/2020
ms.openlocfilehash: 5f076f477c36f96d1807ce7071720225a6df8e03
ms.sourcegitcommit: 23aa0cf152b8f04a294c3fca56f7ae3ba562d272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91803815"
---
# <a name="azure-monitor-for-networks-preview"></a>Ağlar için Azure Izleyici (Önizleme)
Ağ için Azure Izleyici, herhangi bir yapılandırma olmadan dağıtılan tüm ağ kaynakları için [sistem durumu](https://docs.microsoft.com/azure/service-health/resource-health-checks-resource-types) ve [ölçümlerinin](../platform/metrics-supported.md) kapsamlı bir görünümünü sağlar.  Ayrıca [Bağlantı İzleyicisi](../../network-watcher/connection-monitor-preview.md), [ağ güvenlik grupları (NSG 'ler) için akış günlüğü](../../network-watcher/network-watcher-nsg-flow-logging-overview.md), [Trafik Analizi](../../network-watcher/traffic-analytics.md)ve diğer ağ [Tanılama](../../network-watcher/network-watcher-monitoring-overview.md#diagnostics) özellikleri gibi tüm ağ izleme özelliklerine erişim sağlar.

Ağ için Azure Izleyici, izlemenin aşağıdaki temel bileşenleri etrafında yapılandırılır:
- [Ağ durumu ve ölçümleri](#networkhealth)
- [Bağlantı](#connectivity)
- [Trafik](#traffic)
- [Tanılama araç seti](#diagnostictoolkit)

## <a name="network-health-and-metrics"></a><a name="networkhealth"></a>Ağ durumu ve ölçümleri

Ağlara yönelik Azure Izleyici **'ye genel bakış** sayfası, ağ kaynaklarınızın envanterini görselleştirmenin yanı sıra kaynak sistem durumu ve uyarılarla ilgili daha kolay bir yol sağlar. Bu, dört temel işlevsel alana bölünmüştür-arama ve filtreleme, Kaynak Durumu ve ölçümler, uyarılar. ve bağımlılık görünümü

![Genel Bakış sayfası](media/network-insights-overview/overview.png)

### <a name="search-and-filtering"></a>Arama ve filtreleme
Kaynak durumu ve Uyarılar görünümü, **abonelik**, **kaynak grubu** ve **kaynak türü**gibi filtreler kullanılarak özelleştirilebilir. Arama kutusu, kaynak özellikleri arasında arama yapma yeteneği sağlar.

Arama kutusu, kaynakları ve ilişkili kaynakları aramak için kullanılabilir. Örneğin, genel IP bir Application Gateway ilişkilendirilir. Genel IP DNS adını aramak, hem genel IP hem de ilişkili Application Gateway belirler.

![Ağlar için Azure Izleyici-arama](media/network-insights-overview/search.png)


### <a name="resource-health-and-metric"></a>Kaynak Durumu ve metrik
Her kutucuk, kaynak sistem durumu ile birlikte seçilen tüm aboneliklerde dağıtılan örneklerin sayısı ile bir kaynak türünü temsil eder. Aşağıdaki örnekte, 45 ER ve VPN bağlantısı dağıtıldı, 44 sağlıklı ve 1 kullanılamıyor.

![Ağlar için Azure Izleyici-kaynak durumu](media/network-insights-overview/resource-health.png)

Kullanılamaz duruma ve VPN bağlantılarına tıkladığınızda bir ölçüm görünümü başlatılır. 

![Ağlar için Azure Izleyici-Ölçüm görünümü](media/network-insights-overview/metric-view.png)

Kılavuz görünümündeki her bir öğeye tıklayabilirsiniz. Bu bağlantının kaynak sistem durumuna yeniden yönlendirmek için sistem durumu simgesine tıklayın. Bu bağlantı için sırasıyla uyarılar ve ölçümler sayfasına yeniden yönlendirmek üzere Uyarılar ' a tıklayın. 

### <a name="alerts"></a>Uyarılar
Sağdaki **Uyarılar** Kılavuzu, tüm abonelikler genelinde seçili kaynaklar için oluşturulan tüm uyarıların bir görünümünü sağlar. Ayrıntılı uyarılar sayfasına gitmek için uyarı sayılarına tıklayın.

### <a name="dependency-view"></a>Bağımlılık görünümü
**Bağımlılık** görünümü, kaynağın nasıl yapılandırıldığını görselleştirmenize yardımcı olur. Şu anda bağımlılık görünümü artık Application Gateway, sanal WAN ve Load Balancer için desteklenmektedir. Örneğin, Application Gateway durumunda bağımlılık görünümüne, ölçümler kılavuz görünümündeki Application Gateway kaynak adına tıklanarak erişilebilir. Bu ayrıca sanal WAN ve Load Balancer için de geçerlidir.

![Ağlar için Azure Izleyici-Application Gateway görünümü](media/network-insights-overview/application-gateway.png)

Application Gateway için **bağımlılık** görünümü, ön uç IP 'lerinin dinleyicileri, kuralları ve arka uç havuzunu nasıl bağladığınıza ilişkin basitleştirilmiş bir görünüm sağlar. Bağlanan kenarlar renk kodludur ve arka uç havuzu sistem durumuna göre ek ayrıntılar sağlar. Görünüm Ayrıca, sanal makine ölçek kümesi ve VM örnekleri gibi ilgili tüm arka uç havuzlarının Application Gateway ölçümlerinin ve ölçümlerinin ayrıntılı bir görünümünü sağlar.

![Ağlar için Azure Izleyici-bağımlılık görünümü](media/network-insights-overview/dependency-view.png)

Bağımlılık grafiği yapılandırma ayarlarına kolay gezinme sağlar. Diğer işlevlere erişmek için bir arka uç havuzuna sağ tıklayın. Örneğin, arka uç havuzu bir sanal makine ise, bağlantı sorunlarını belirlemek için VM öngörülerine ve ağ Izleyicisi bağlantısına doğrudan erişebilirsiniz.

![Ağlar için Azure Izleyici-bağımlılık Görünüm menüsü](media/network-insights-overview/dependency-view-menu.png)

Bağımlılık görünümündeki arama ve filtre çubuğu, grafikte arama yapmanın kolay bir yolunu sağlar. Örneğin, aşağıdaki örnekte *appgwtestrule* araması, grafik görünümünü *appgwtestrule*ile bağlantılı tüm düğümlere daraltacaktır.

![Ağlar için Azure Izleyici-arama örneği](media/network-insights-overview/search-example.png)

Farklı filtreler, belirli bir yol ve duruma göre daraltmak için yardım sağlar. Örneğin, durumun *sağlıksız*olduğu tüm kenarları göstermek için **sistem** durumu açılan listesinden yalnızca *sağlıksız* ' ı seçin.

Application Gateway, tüm arka uç havuzu kaynakları ve ön uç IP 'Leri için ayrıntılı ölçümleri olan önceden yapılandırılmış bir çalışma kitabı başlatmak üzere **ayrıntılı Ölçüm görünümü** ' ne tıklayın. 

## <a name="connectivity"></a><a name="connectivity"></a>Bağlantı

Bağlantı **sekmesi,** seçili abonelik kümesi Için bağlantı Izleyicisi ve [Bağlantı İzleyicisi (Önizleme)](../../network-watcher/connection-monitor-preview.md) kullanılarak yapılandırılan tüm testleri görselleştirmek için kolay bir yol sağlar.

![Ağlar için Azure Izleyici 'deki bağlantı sekmesi](media/network-insights-overview/azure-monitor-for-networks-connectivity-tab.png)

Testler, kaynak ve hedef kutucuklarına göre gruplandırılır ve her test için ulaşılabilirlik durumunu görüntüler. Erişilebilir ayarlar, başarısız olan denetimleri (%) temel alarak ulaşılabilirlik ölçütlerini yapılandırmak için kolay bir erişim sağlar. ve RTT (MS). Değerler ayarlandıktan sonra her test için durum seçim ölçütlerine göre güncelleştirilir.

![Ağlar için Azure Izleyici 'de bağlantı testleri](media/network-insights-overview/azure-monitor-for-networks-connectivity-tests.png)

Herhangi bir kaynak veya hedef Kutucuğa tıkladığınızda Ölçüm görünümü başlatılır.

![Ağlar için Azure Izleyici 'de bağlantı ölçümleri](media/network-insights-overview/azure-monitor-for-networks-connectivity-metrics.png)


Kılavuz görünümündeki her bir öğeye tıklayabilirsiniz. Atlama topolojisi ve bağlantı sorunlarını etkileyen bağlantıyı görüntülemek için **Bağlantı İzleyicisi** portalı sayfasına yeniden yönlendirmek Için **ulaşılabilirlik** simgesine tıklayın. Uyarı ' a **tıklayarak uyarılara yeniden** yönlendirin ve seçilen bağlantı izleyicisinin ölçüm sayfasına yeniden yönlendirmek Için **% başarısız olan yüzde/gidiş dönüş süresinin başarısız olup olmadığını kontrol** edin.

Sağdaki **Uyarılar**   Kılavuzu, tüm abonelikler genelinde yapılandırılmış bağlantı testleri için oluşturulan tüm uyarıların bir görünümünü sağlar. Ayrıntılı uyarılar sayfasına gitmek için uyarı sayılarına tıklayın.

## <a name="traffic"></a><a name="traffic"></a>Trafik
Trafik sekmesi, [NSG akış günlükleri](../../network-watcher/network-watcher-nsg-flow-logging-overview.md) için yapılandırılmış tüm NSG 'ler ve seçili abonelik kümesi için [Trafik Analizi](../../network-watcher/traffic-analytics.md) ve konumlara göre gruplanmış olarak erişim sağlar. Bu sekmede sunulan arama işlevselliği, Aranan IP adresi için yapılandırılan NSG 'leri tanımlamaya izin vermez. Ortamınızda herhangi bir IP adresini arayabilir ve döşeli bölgesel görünüm, NSG akış günlükleri ve trafik analizi yapılandırma durumu ile birlikte tüm NSG 'leri görüntüler.

![Ağlar için Azure Izleyici 'de trafik görünümü](media/network-insights-overview/azure-monitor-for-networks-traffic-view.png)

Herhangi bir bölge kutucuğuna tıkladığınızda, NSG akış günlüklerini görüntüleme ve yapılandırma konusunda kolay bir kılavuz görünümü başlatılır ve Trafik Analizi.  

![Ağlar için Azure Izleyici 'de trafik bölgesi görünümü](media/network-insights-overview/azure-monitor-for-networks-traffic-region-view.png)

Kılavuz görünümündeki her bir öğeye tıklayabilirsiniz. NSG akış günlüğünü ve Trafik Analizi yapılandırmasını düzenlemek için yapılandırma durumu ' na tıklayın. Seçili NSG için yapılandırılmış trafik uyarılarına yeniden yönlendirmek için uyarılar 'a tıklayın. Benzer şekilde, çalışma alanına tıklayarak Trafik Analizi görünümüne gidebilirsiniz.  

Sağdaki **Uyarılar**   Kılavuzu tüm aboneliklerdeki tüm trafik analizi çalışma alanı tabanlı uyarıların bir görünümünü sağlar. Ayrıntılı uyarılar sayfasına gitmek için uyarı sayılarına tıklayın.

## <a name="diagnostic-toolkit"></a><a name="diagnostictoolkit"></a> Tanılama araç seti
Tanılama araç seti, ağ sorunlarını gidermek için kullanılabilen tüm tanılama özelliklerine erişim sağlar. Bu açılan listeden, [paket yakalama](../../network-watcher/network-watcher-packet-capture-overview.md), [VPN sorunlarını giderme](../../network-watcher/network-watcher-troubleshoot-overview.md), [bağlantı sorunlarını giderme](../../network-watcher/network-watcher-connectivity-overview.md), [sonraki atlama](../../network-watcher/network-watcher-next-hop-overview.md) ve [IP akışı doğrulama](../../network-watcher/network-watcher-ip-flow-verify-overview.md)gibi özelliklere erişebilirsiniz.

![Tanılama araç seti sekmesi](media/network-insights-overview/azure-monitor-for-networks-diagnostic-toolkit.png)

## <a name="troubleshooting"></a>Sorun giderme 

Genel sorun giderme kılavuzu için, adanmış çalışma kitabı tabanlı Öngörüler [sorun giderme makalesine](troubleshoot-workbooks.md)bakın.

Bu bölüm, ağlar için Azure Izleyicisini kullanırken karşılaşabileceğiniz bazı yaygın sorunların tanılanması ve sorun gidermede size yardımcı olur. Belirli sorununuzla ilgili bilgileri bulmak için aşağıdaki listeyi kullanın.

### <a name="resolving-performance-issues-or-failures"></a>Performans sorunlarını veya başarısızlıklarını çözme

Ağlarda Azure Izleyici ile belirttiğiniz ağla ilgili sorunları gidermeye yardımcı olmak için, hatalı çalışan kaynağın sorun giderme belgelerine bakın. Sorun giderme-yüksek kullanılan hizmetler için bağlantı bağlantıları aşağıda listelenmiştir.
* Sanal ağ (VNET)
* Application Gateway
* VPN Gateway
* ExpressRoute 
* Load Balancer 

### <a name="why-dont-i-see-the-resources-from-all-the-subscriptions-i-have-selected"></a>Neden seçili olan tüm aboneliklerden kaynakları görmüyorum

Ağ öngörüleri aynı anda yalnızca 5 abonelikteki kaynakları gösterebilir. 

### <a name="i-want-to-make-changes-or-add-additional-visualizations-to-network-insights-how-do-i-do-so"></a>Ağ öngörülerine değişiklik yapmak veya ek görselleştirmeler eklemek istiyorum, bunu nasıl yapabilirim?

Değişiklik yapmak için, çalışma kitabını değiştirmek üzere "düzenleme modunu" seçin, sonra çalışmanızı belirlenen bir aboneliğe ve kaynak grubuna bağlı yeni bir çalışma kitabı olarak kaydedebilirsiniz.

### <a name="what-is-the-time-grain-once-we-pin-any-part-of-the-workbooks"></a>Çalışma kitaplarının herhangi bir bölümünü sabitledikten sonra zaman çizgisi nedir?

"Auto" zaman dilimi ' ni kullanıyoruz, bu nedenle hangi zaman aralığının seçili olduğuna bağlıdır.

### <a name="what-is-the-time-range-when-any-part-of-the-workbook-is-pinned"></a>Çalışma kitabının herhangi bir bölümünün sabitlendiği zaman aralığı nedir?

Zaman aralığı, pano ayarlarına bağlı olarak değişir.

### <a name="what-if-i-want-to-see-other-data-or-make-my-own-visualizations-how-can-i-make-changes-to-the-network-insights"></a>Diğer verileri görmek veya kendi görselleştirmelerimi yapmak istersem ne yapmalıyım? Ağ öngörülerine nasıl değişiklik yapabilirim?

Herhangi bir kenar panelinde ve ayrıntılı ölçüm görünümünde gördüğünüz çalışma kitabını düzenleme modunun kullanımı ile düzenleyebilir ve ardından çalışmalarınızı tüm yeni değişikliklerinizin bulunduğu yeni bir çalışma kitabı olarak kaydedebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- Ağ izleme hakkında daha fazla bilgi edinmek için [Azure Ağ İzleyicisi nedir?](../../network-watcher/network-watcher-monitoring-overview.md).
- Çalışma kitaplarının desteklemek için tasarlandıkları senaryoları, mevcut raporların yeni nasıl yazılacağını ve özelleştirildiğini ve [Azure izleyici çalışma kitaplarını kullanarak etkileşimli raporlar oluşturma](../platform/workbooks-overview.md)konusunu gözden geçirin.
