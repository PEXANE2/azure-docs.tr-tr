---
title: Azure Monitör'de klasik uyarı & izlemenin güncellenmesi
description: Daha önce Azure portalında Uyarılar (klasik) altında gösterilen klasik izleme hizmetlerinin ve işlevlerinin emekliye ayrılmasının açıklaması.
author: yanivlavi
services: azure-monitor
ms.topic: conceptual
ms.date: 2/7/2019
ms.author: yalavi
ms.subservice: alerts
ms.openlocfilehash: dec4d6824644cabf8b1872da207b8554fee0b3d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77659484"
---
# <a name="unified-alerting--monitoring-in-azure-monitor-replaces-classic-alerting--monitoring"></a>Azure Monitör'de birleşik uyarı & izleme, klasik uyarı & izlemenin yerini alır

Azure Monitor artık kaynaklar arasında 'Tek Metrik' ve 'Tek Uyarılar'ı destekleyen birleşik bir tam yığın izleme hizmeti haline gelmiştir; daha fazla bilgi için [yeni Azure Monitor'daki blog gönderimize](https://azure.microsoft.com/blog/new-full-stack-monitoring-capabilities-in-azure-monitor/)bakın. Yeni Azure izleme ve uyarı platformları, bulut bilgi işlemin artan genişliğine ayak uydurarak ve Microsoft Intelligent Cloud felsefesiile uyumlu olarak daha hızlı, daha akıllı ve genişletilebilir olacak şekilde oluşturulmuştur. 

Yeni Azure izleme ve uyarı platformu nun devreye alınmasıyla, Azure *uyarılarının klasik uyarıları* bölümünde barındırılan "klasik" izleme ve uyarı platformunun emekliye ayırılması, **Ağustos 2019'a kadar Azure genel bulutlarında küçümsülecektir.** [Azure Devlet bulutu](../../azure-government/documentation-government-welcome.md) ve [Azure China 21Vianet](https://docs.azure.cn/) etkilenmez.

> [!NOTE]
> Geçiş aracının kullanıma sunulmasındaki gecikme nedeniyle, klasik uyarı geçişi için emeklilik tarihi 30 Haziran 2019 tarihinden itibaren [31 Ağustos 2019'a uzatıldı.](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/)

 ![Azure portalında klasik uyarı](media/monitoring-classic-retirement/monitor-alert-screen2.png) 

Yeni platformda uyarılarınızı yeniden oluşturmaya başlamanızı ve uyarılarınızı yeniden oluşturmanızı öneririz. Çok sayıda uyarıya sahip müşteriler için, mevcut klasik uyarıları kesintiye veya ek maliyetler olmadan yeni uyarı sistemine taşımak için gönüllü bir [geçiş aracı](alerts-using-migration-tool.md) olan aşamalar halinde [kullanıma sunacağız.](alerts-understand-migration.md#rollout-phases)

> [!IMPORTANT]
> Etkinlik Günlüğü'nde oluşturulan Klasik Uyarı kuralları küçümsülmez veya geçirilmez. Etkinlik Günlüğü'nde oluşturulan tüm klasik uyarı kurallarına yeni Azure Monitörü - Uyarılar'dan erişilebilir ve olduğu gibi kullanılabilir. Daha fazla bilgi için bkz: [Azure Monitor kullanarak etkinlik günlüğü uyarıları oluştur, görüntüle ve yönet.](../../azure-monitor/platform/alerts-activity-log.md) Benzer şekilde, Hizmet Durumu Uyarılarına yeni Hizmet Durumu bölümünden erişilebilir ve olduğu gibi kullanılabilir. Ayrıntılar [için, hizmet durumu bildirimleri uyarılarına](../../azure-monitor/platform/alerts-activity-log-service-notifications.md)bakın.

## <a name="unified-metrics-and-alerts-in-application-insights"></a>Uygulama Öngörülerinde Birleşik Ölçümler ve Uyarılar

Azure Monitor'un yeni metrik platformu artık Application Insights'tan gelen izleme yi güçleyecek. Bu hareket, Uygulama Öngörüleri'nin Eylem Gruplarına bağlanarak önceki e-posta ve webhook aramalarından çok daha fazla seçenek sunacak olması anlamına gelir. Uyarılar artık Sesli Aramaları, Azure Fonksiyonlarını, Mantık Uygulamalarını, SMS'i ve ServiceNow ve Automation Runbooks gibi ITSM Araçlarını tetikleyebilir. Neredeyse gerçek zamanlı izleme ve uyarı yla, yeni platform Application Insights kullanıcılarının diğer Azure kaynakları arasında aynı teknoloji gücü nden yararlanmalarını ve Microsoft ürünlerinin izlenmesini desteklemesini sağlar.

Yeni birleştirilmiş Uygulama Öngörüleri için Uyarı:

- **Application Insights Platform ölçümleri** – Application Insights ürününden popüler önceden oluşturulmuş ölçümler sağlar. Daha fazla bilgi için, yeni Azure Monitörü'nde Uygulama Öngörüleri için Platform Ölçümleri'ni kullanma hakkındaki bu [makaleye](../../azure-monitor/app/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics)bakın.
- **Uygulama Öngörüleri Kullanılabilirliği ve Web testi** -web uygulamanızın veya sunucunuzun yanıt verme durumunu ve kullanılabilirliğini değerlendirme olanağı sağlar. Daha fazla bilgi için, [yeni Azure Monitöründe Kullanılabilirlik Testleri ve Uygulama Öngörüleri Için Uyarıları](../../azure-monitor/app/monitor-web-app-availability.md)kullanma hakkındaki bu makaleye bakın.
- **Uygulama Öngörüleri Özel ölçümleri** – izleme ve uyarılar için kendi ölçümlerini tanımlamanızı ve yayan ölçümler. Daha fazla bilgi için, [yeni Azure Monitörü'nde Uygulama Öngörüleri için Özel Metrik'i](../../azure-monitor/app/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation)kullanma hakkındaki bu makaleye bakın.
- **Uygulama Öngörüleri Hata Anomalileri (Smart Detection'ın bir parçası)** – web uygulamanız başarısız HTTP istekleri veya bağımlılık çağrıları oranında anormal bir artış görürse sizi neredeyse gerçek zamanlı olarak otomatik olarak haberdar eder. Daha fazla bilgi için, [Smart Detection - Failure Anomalies](https://docs.microsoft.com/azure/azure-monitor/app/proactive-failure-diagnostics)kullanarak bu makaleye bakın.

## <a name="unified-metrics-and-alerts-for-other-azure-resources"></a>Diğer Azure kaynakları için birleştirilmiş Ölçümler ve Uyarılar

Mart 2018'den bu yana, Azure kaynakları için yeni nesil uyarı ve çok boyutlu izleme kullanılabilirliği olmuştur. Şimdi yeni metrik platform ve uyarı neredeyse gerçek zamanlı yetenekleri ile daha hızlıdır. Daha da önemlisi, yeni metrik platform uyarıları, yeni platform belirli bir değer birleşimi, durum veya işlem için dilimleme ve filtre leme olanağı sağlayan boyutlar seçeneğini içerdiğinden, daha ayrıntılı ayrıntı sağlar. Yeni Azure Monitör'deki tüm uyarılar gibi, yeni metrik uyarılar da ActionGroups'un kullanımıyla daha genişletilebilir ve bildirimlerin SMS, Voice, Azure İşlevi, Otomasyon Runbook ve daha fazlasına e-posta veya webhook ötesine genişlemesine olanak tanır. Daha fazla bilgi için bkz: [Azure Monitor kullanarak metrik uyarıları oluştur, görüntüle ve yönet.](../../azure-monitor/platform/alerts-metric.md)
Azure kaynakları için daha yeni ölçümler şu şekilde kullanılabilir:

- **Azure Monitör Standart platform ölçümleri,** çeşitli Azure hizmetleri ve ürünlerinden önceden doldurulmuş popüler ölçümler sağlar. Daha fazla bilgi için Azure Monitor ve Azure [Monitor'daki Desteklenmiş ölçümler](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported) hakkındaki bu [makaleye](../../azure-monitor/platform/alerts-metric-overview.md#supported-resource-types-for-metric-alerts)bakın.
- Azure Tanılama aracısı da dahil olmak üzere kullanıcı odaklı kaynaklardan ölçümler sağlayan **Azure Monitor Özel ölçümleri.** Daha fazla bilgi için [Azure Monitor'da Özel ölçümler](../../azure-monitor/platform/metrics-custom-overview.md)hakkındaki bu makaleye bakın. Özel ölçümleri kullanarak, [Windows Azure Tanıaracı](../../azure-monitor/platform/collect-custom-metrics-guestos-resource-manager-vm.md) ve [InfluxData Telegraf aracısı](../../azure-monitor/platform/collect-custom-metrics-linux-telegraf.md)tarafından toplanan ölçümleri de yayımlayabilirsiniz.

## <a name="retirement-of-classic-monitoring-and-alerting-platform"></a>Klasik izleme ve uyarı platformunun emekliliği

Daha önce de belirtildiği gibi, Azure portalının [Uyarılar (klasik) bölümünden](../../azure-monitor/platform/alerts-classic.overview.md) şu anda kullanılabilir olan klasik izleme ve uyarı platformu, yeni sistemle değiştirildikleri göz önüne alındığında önümüzdeki aylarda kullanımdan kaldırılacaktır.
Eski klasik izleme ve uyarı 31 Ağustos 2019 tarihinde emekli olacak; ilgili API'lerin, Azure portal arabiriminin ve Hizmetlerin kapatılması da dahil olmak üzere. Özellikle, bu özellikler amortismana alınacaktır:

- Azure portalının [Uyarılar (klasik) bölümü](../../azure-monitor/platform/alerts-classic.overview.md) aracılığıyla şu anda kullanılabilen Azure kaynakları için eski (klasik) ölçümler ve uyarılar; [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules) kaynak olarak erişilebilir
- Uygulama Öngörüleri için eski (klasik) platform ve özel ölçümlerin yanı sıra Azure portalının [Uyarılar (klasik) bölümü](../../azure-monitor/platform/alerts-classic.overview.md) aracılığıyla şu anda kullanılabilen ve [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules) kaynağı olarak erişilebilen bu bilgiler hakkında uyarı
- Azure portalında [Uygulama Öngörüleri içinde Akıllı Algılama](../../azure-monitor/app/proactive-diagnostics.md) olarak şu anda kullanılabilen eski (klasik) Hata Anomalileri uyarısı; Azure portalının Uyarılar [(klasik) bölümünde](../../azure-monitor/platform/alerts-classic.overview.md) gösterilen yapılandırılmış uyarılarla

İlgili [API,](https://msdn.microsoft.com/library/azure/dn931945.aspx) [PowerShell,](../../azure-monitor/platform/alerts-classic-portal.md) [CLI,](../../azure-monitor/platform/alerts-classic-portal.md) [Azure portal sayfası](../../azure-monitor/platform/alerts-classic-portal.md)ve Kaynak [Şablonu](../../azure-monitor/platform/alerts-enable-template.md) dahil olmak üzere tüm klasik izleme ve uyarı sistemleri Ağustos 2019 sonuna kadar kullanılabilir kalacaktır. 

Ağustos 2019 sonunda Azure Monitor'da:

- Klasik izleme ve uyarı hizmeti kullanımdan kaldırılacak ve artık yeni uyarı kuralları nın oluşturulması için kullanılamaz.
- Ağustos 2019'dan sonra Uyarılar'da (klasik) var olmaya devam eden uyarı kuralları, bildirimleri yürütmeye ve çalıştırmaya devam eder, ancak değişiklik için kullanılamaz.
- Eylül 2019'dan itibaren, geçirilebilen klasik izleme & uyarı uyarılarındaki uyarı kuralları, Microsoft tarafından birkaç haftaya yayılan aşamalarda yeni Azure izleme platformundaki eşdeğerlerine otomatik olarak taşınacaktır. İşlem herhangi bir kesinti olmadan sorunsuz olacak ve müşteriler izleme kapsama hiçbir kaybı olacaktır.
- Yeni uyarı platformuna geçirilen uyarı kuralları, izleme kapsamını önceden olduğu gibi sağlar, ancak yeni yüklerle bildirim de olur. Klasik uyarı kuralıyla ilişkili herhangi bir e-posta adresi, webhook bitiş noktası veya mantık uygulaması bağlantısı geçirildiğinde ileriye taşınır, ancak yeni platformda uyarı yükü farklı olacağı için doğru şekilde olmayabilir.
- [Otomatik olarak geçirilemeyen](alerts-understand-migration.md#classic-alert-rules-that-will-not-be-migrated) ve kullanıcılardan el ile eylem gerektiren bazı klasik uyarı kuralları Haziran 2020'ye kadar çalışmaya devam edecektir.

> [!IMPORTANT]
> Microsoft Azure Monitor, klasik uyarı kurallarını yakında yeni [platforma gönüllü olarak geçirmek için](alerts-using-migration-tool.md) aşamalı olarak kullanıma sunuldu. Eylül 2019'dan itibaren hala var olan ve geçirilebilen tüm klasik uyarı kuralları için zorla çalıştırın. Müşterilerin, [uygulama istatistiklerindeki Birleşik Ölçümler ve Uyarılar'dan](#unified-metrics-and-alerts-in-application-insights) veya diğer Azure kaynakları için Birleşik Ölçümler ve [Uyarılar'dan](#unified-metrics-and-alerts-for-other-azure-resources)gelen yeni yükü işlemek için uyarlanmış olması için otomasyon tüketen klasik uyarı kuralı yükünün uyarlandığından emin olmak gerekir ( klasik uyarı kurallarının geçişinden sonra). Daha fazla bilgi [için bkz: klasik uyarı kuralı geçişine hazırlanın](alerts-prepare-migration.md)

Bu makale, yeni Azure izleme & uyarı işlevleriyle ilgili bağlantılar & ayrıntıların yanı sıra kullanıcılara yeni Azure Monitor platformunun benimsenmesinde yardımcı olacak araçların kullanılabilirliği yle sürekli olarak güncellenecektir.

## <a name="pricing-for-migrated-alert-rules"></a>Geçirilen Uyarı Kuralları için Fiyatlandırma

Azure Monitor [klasik uyarılarınızı](../../azure-monitor/platform/alerts-classic.overview.md) yeni uyarılar deneyimine geçirmenize yardımcı olmak için bir geçiş aracı kullanıma salıyoruz. Geçirilen uyarı kuralları ve ilgili geçiş edilen eylem grupları (e-posta, webhook veya LogicApp) ücretsiz olarak kalır. Eşik, toplama türünü ve toplama parçalı özelliğini de içeren klasik uyarılarda sahip olduğunuz işlevsellik, geçirilen uyarı kuralınızla ücretsiz olarak kullanılabilir olmaya devam eder. Ancak, geçirilen uyarı kuralını yeni uyarı platformu özelliklerinden, bildirimlerinden veya eylem türlerinden herhangi birini kullanmak üzere yeniden yönetirseniz, karşılık gelen bir ücret uygulanır. Uyarı kuralları ve bildirimler için fiyatlandırma hakkında daha fazla bilgi için Azure [Monitör fiyatlandırması'na](https://azure.microsoft.com/pricing/details/monitor/)bakın.

Uyarı kuralınız için ücret lendireceğiniz durumlara örnekler aşağıda verilmiştir:

- Yeni Azure Monitör platformunda, ücretsiz birimlerin ötesinde oluşturulan tüm yeni (geçirilmeyen) uyarı kuralı
- Azure Monitor tarafından dahil edilen ücretsiz birimlerin dışında alınan ve tutulan tüm veriler
- Application Insights tarafından yürütülen çoklu test web testleri
- Azure Monitor'da bulunan ücretsiz birimlerin ötesinde depolanan tüm özel ölçümler
- Sıklık, birden çok kaynak/boyut, [Dinamik Eşikler,](alerts-dynamic-thresholds.md)kaynak/sinyal değiştirme gibi yeni metrik uyarı özelliklerini kullanmak üzere düzenlenen tüm geçirilen uyarı kuralları.
- Yeni bildirimler kullanmak üzere düzenlenen tüm geçirilen eylem grupları veya SMS, Sesli Arama ve/veya ITSM tümleştirmesi gibi eylem türleri.

## <a name="next-steps"></a>Sonraki adımlar

* [Yeni birleştirilmiş Azure Monitörü](../../azure-monitor/overview.md)hakkında bilgi edinin.
* Yeni [Azure Uyarıları](../../azure-monitor/platform/alerts-overview.md)hakkında daha fazla bilgi edinin.
