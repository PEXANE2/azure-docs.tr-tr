---
title: Azure Monitör belgelerindeki yenilikler
description: Azure Monitör belgelerinde her ay güncelleştirilen önemli güncelleştirmeler.
ms.subservice: ''
ms.topic: overview
author: bwren
ms.author: bwren
ms.date: 03/05/2020
ms.openlocfilehash: c29790035ec4e971957784e826a1e8bd8e0c9329
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79500506"
---
# <a name="whats-new-in-azure-monitor-documentation"></a>Azure Monitör belgelerindeki yenilikler nelerdir?
Bu makalede, yeni veya önemli ölçüde güncelleştirilen Azure Monitor makaleleri listelenmektedir. Her ayın ilk haftasında bir önceki aya ait makale güncelleştirmelerini eklemek üzere yenilenir.

## <a name="february-2020"></a>Şubat 2020

### <a name="agents"></a>Aracılar
Tanılama uzantısı içeriğinin yeniden yazılmasının bir parçası olarak birden çok güncelleştirme.

- [Azure izleme aracılarına genel bakış](platform/agents-overview.md) - Her aracının benzersiz özelliklerini daha iyi netleştirmek için tabloları yeniden yapılandırıldı.
- [Azure Diagnostics uzantısına genel bakış](platform/diagnostics-extension-overview.md) - Yeniden yazmayı tamamlayın.
- [Azure Monitor'daki etkinlikler için IIS için blob depolama](platform/diagnostics-extension-logs.md) ve tablo depolama yı kullanın - Güncelleme ve netlik için genel yeniden yazın.
- [Windows Azure tanılama uzantısını (WAD) yükleyin ve yapılandırın](platform/diagnostics-extension-windows-install.md) - Yeni makale. 
- [Windows tanılama uzantısı şeması](platform/diagnostics-extension-schema-windows.md) - Yeniden düzenlendi.
- [Windows Azure tanılama uzantısından Azure Etkinlik Hub'larına veri gönderme](platform/diagnostics-extension-stream-event-hubs.md) - Tamamen yeniden yazıldı ve güncelleştirildi.
- [Tanılama verilerini Azure Depolama'da depolayın ve görüntüleyin](/azure/cloud-services/diagnostics-extension-to-storage) - Tamamen yeniden yazıldı ve güncelleştirildi.
- [Windows için Log Analytics sanal makine uzantısı](../virtual-machines/extensions/oms-windows.md) - Log Analytics aracısı ile ilişkiyi daha iyi açıklar.
- [Linux için Azure Monitor sanal makine uzantısı](../virtual-machines/extensions/oms-linux.md) - Log Analytics aracısıyla ilişkiyi daha iyi açıklar.




### <a name="application-insights"></a>Application Insights
- [Azure Application Insights'ta bağlantı dizeleri](app/sdk-connection-string.md) - Yeni makale.

### <a name="insights-and-solutions"></a>Öngörüler ve çözümler

#### <a name="azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici
- [Azure Etkin Dizini Azure Kubernetes Hizmetiyle Tümleştir](../aks/azure-ad-integration.md) - Kapsayıcılar için Azure Monitor'u desteklemek için RBAC özellikli kümeyi desteklemek için bir istemci uygulaması oluşturmak için not eklendi.

#### <a name="azure-monitor-for-vms"></a>VM'ler için Azure İzleyici
- [Sanal Taşıtlar için Azure Monitor (GA) sık sorulan sorular](insights/vminsights-ga-release-faq.md) - Performans verilerinin depolanmasına göre değişiklik yapın.

#### <a name="office-365"></a>Office 365
- [Azure'da Office 365 yönetim çözümü](insights/solution-office-365.md) - Amortisman tarihi güncelleştirildi.


### <a name="logs"></a>Günlükler
- [Azure Monitor'da günlük sorgularını optimize edin](log-query/query-optimization.md) - Yeni makale.
- [Azure Monitör Günlükleri için kullanımı ve maliyetleri yönetin](platform/manage-cost-storage.md) - Kullanımınızı anlamanıza yardımcı olmak için geliştirilmiş örnek sorgular.

### <a name="metrics"></a>Ölçümler
- [Azure Monitor platform ölçümleri Tanılama Ayarları ile dışa aktarılabilir](platform/metrics-supported-export-diagnostic-settings.md) - Nulls ve sıfır değerleri için davranışa değişiklik bölümü eklendi.


### <a name="visualizations"></a>Görsel öğeler
Çalışma kitapları dönüşüm kılavuzugörüntülemek için tasarımcı için birden çok yeni makale.

- [Çalışma kitapları geçiş kılavuzuna Azure Monitor görünüm tasarımcısı](platform/view-designer-conversion-overview.md) - Yeni makale.
- [Azure Monitor görünüm tasarımcısından çalışma kitapları dönüştürme seçeneklerine](platform/view-designer-conversion-options.md) - Yeni makale.
- [Azure Monitor görünüm tasarımcısından çalışma kitapları döşeme dönüşümlerine -](platform/view-designer-conversion-tiles.md) Yeni makale.
- [Azure Monitor görünüm tasarımcısından çalışma kitapları dönüşüm özetine ve erişime](platform/view-designer-conversion-access.md) - Yeni makale.
- [Azure Monitor görünüm tasarımcısından çalışma kitaplarına dönüşüm yaygın görevler](platform/view-designer-conversion-tasks.md) - Yeni makale.
- [Azure Monitor görünüm tasarımcısından çalışma kitapları dönüştürme örneklerine](platform/view-designer-conversion-examples.md) - Yeni makale.




## <a name="january-2020"></a>Ocak 2020

### <a name="general"></a>Genel
- [Azure Monitor tarafından neler izlenir?](monitor-reference.md) - Yeni makale.

### <a name="agents"></a>Aracılar
- [Azure Log Analytics aracısıyla günlük verileri toplayın](platform/log-analytics-agent.md) - Güncelleştirilmiş ağ güvenlik duvarı gereksinimleri tablosu.


### <a name="alerts"></a>Uyarılar
- [Azure portalında eylem grupları oluşturun ve yönetin](platform/action-groups.md) - Artık gerekli olmayan v2 işlevleri için kaldırıldı ayarı.
- [Kaynak Yöneticisi şablonuyla bir metrik uyarı oluşturun](platform/alerts-metric-create-templates.md) - *YoksayveriÖnce* parametresi için örnek eklendi.  Çok ölçüt kuralları yla ilgili kısıtlamalar eklendi.
- [Log Analytics Alert REST API](platform/api-alerts.md) kullanarak - JSON örneği düzeltildi.


### <a name="application-insights"></a>Application Insights
- [Application Insights ve Log Analytics tarafından kullanılan IP adresleri](app/ip-addresses.md) - Azure Ağ Güvenlik Grupları'nı kullanarak trafiğe izin vermek için gelen bağlantı noktası kuralının nasıl eklenilebildiğini içeren Kullanılabilirlik testi bölümü güncelleştirildi.
- [Azure Application Insights Profiler ile ilgili sorun giderme](app/profiler-troubleshooting.md) - Genel sorun giderme güncelleştirildi.
- [Azure Uygulama Öngörüleri'nde Telemetri örneklemesi](app/sampling.md) - Müşteri geri bildirimlerine dayalı olarak okunabilirliği artırmak için güncelleştirildi ve yeniden yapılandırıldı.


### <a name="data-security"></a>Veri güvenliği
- [Azure Monitor müşteri tarafından yönetilen anahtar yapılandırması](platform/customer-managed-keys.md) - Yeni makale.

### <a name="insights-and-solutions"></a>Öngörüler ve çözümler

#### <a name="azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici
- [Kapsayıcılar aracısı veri toplama için Azure Monitörünü yapılandırın](insights/container-insights-agent-config.md) - Azure Red Hat OpenShift'te aracıyı yükseltmek için ayrıntılar eklendi ve aracıyı yükseltme yöntemlerini ayırt etmek için ek bilgiler eklendi.
- [Kapsayıcılar için Azure Monitor için performans uyarıları oluşturun](insights/container-insights-alerts.md) - Çalışma alanı bağlamı uyarılarını kullanarak çalışma alanında depolanan performans verileri hakkında uyarı oluşturmak için gözden geçirilmiş bilgiler ve güncelleştirilmiş adımlar.
- [Kapsayıcılar için Azure Monitor ile Kubernetes izleme](insights/container-insights-analyze.md) - Windows Kubernetes kümelerinin desteğiyle ilgili hem genel bakış makalesini hem de analiz makalesini güncelleştirin.
- [Azure Kırmızı Şapka OpenShift kümelerini kapsayıcılar için Azure Monitor ile yapılandırın](insights/container-insights-azure-redhat-setup.md) - Azure Red Hat OpenShift'te aracıyı yükseltmek için ayrıntılar eklendi ve aracıyı yükseltme yöntemlerini ayırt etmek için ek bilgiler eklendi.
- [Karma Kubernetes kümelerini kapsayıcılar için Azure Monitor ile yapılandırın](insights/container-insights-hybrid-setup.md) - Kubelet'in cAdvisor'ı ile güvenli bağlantı noktası:10250 için ek desteği yansıtacak şekilde güncellendi.
- [Kapsayıcılar aracısı için Azure Monitörü nasıl yönetilir](insights/container-insights-manage-agent.md) - Azure Red Hat OpenShift ile diğer Kubernetes kümeleri türlerine kıyasla davranış ve metrik kazıma ile ilgili güncelleştirilmiş ayrıntılar.
- [Prometheus Tümleştirme kapsayıcıları için Azure Monitörünü yapılandırın](insights/container-insights-prometheus-integration.md) - Azure Red Hat OpenShift ile diğer Kubernetes kümeleri türlerine kıyasla davranış ve metrik kazıma config ile ilgili güncelleştirilmiş ayrıntılar.
- [Azure Monitor'u ölçümler için kapsayıcılar için güncelleştirme](insights/container-insights-update-metrics.md) - Azure Red Hat OpenShift ile diğer Kubernetes kümeleri türlerine kıyasla davranış ve metrik kazıma ile ilgili güncelleştirilmiş ayrıntılar.


#### <a name="azure-monitor-for-vms"></a>VM'ler için Azure İzleyici
- [VM'ler için Azure Monitor (GA) sık sorulan sorular](insights/vminsights-ga-release-faq.md) - Çalışma alanını ve aracıları yeni sürüme yükseltme hakkında bilgi eklendi.

#### <a name="office-365"></a>Office 365
- [Azure'da Office 365 yönetim çözümü](insights/solution-office-365.md) - Azure Sentinel'de Office 365 çözümüne geçişle ilgili ayrıntılar ve SSS eklendi. Onboarding bölümü kaldırıldı.



### <a name="logs"></a>Günlükler
- [Azure Monitor'da Günlük Analizi çalışma alanlarını yönetin](platform/manage-access.md) - Eylemleri Değil Güncelleştirmeleri.
- [Azure Monitör Günlükleri için kullanımı ve maliyetleri yönetin](platform/manage-cost-storage.md) - Fiyatlandırma Modeli bölümünde veri hacminin hesaplanmasına açıklık eklendi.
- [Günlük Analizi Çalışma Alanı Oluşturmak ve Yapılandırmak için Azure Kaynak Yöneticisi şablonlarını kullanın](platform/template-workspace-configuration.md) - Yeni fiyatlandırma katmanlarıyla güncelleştirilmiş şablon.


### <a name="platform-logs"></a>Platform günlükleri
- [Tanı ayarlarıyla Azure Etkinlik günlüğünü topla- Azure Monitor](platform/diagnostic-settings-legacy.md) - Değiştirilen özellikler hakkında ek bilgiler.
- [Azure Etkinlik Günlüğü'ne dışa aktarma](platform/activity-log-export.md) - UI değişiklikleri için güncelleştirildi. 





## <a name="december-2019"></a>Aralık 2019

### <a name="agents"></a>Aracılar
- [Linux bilgisayarlarını Azure Monitor'a bağlayın](platform/agent-linux.md) - Yeni makale.

### <a name="alerts"></a>Uyarılar
- [Kaynak Yöneticisi şablonuyla bir metrik uyarısı oluşturun](platform/alerts-metric-create-templates.md) - Özel metrik için örnek eklendi.
- [Azure Monitörü'nde Dinamik Eşiklere Sahip Uyarılar Oluşturma](platform/alerts-dynamic-thresholds.md) - Dinamik eşik grafiklerini yorumlama bölümü eklendi.
- [Azure'da uyarı ve bildirim izleme](platform/alerts-overview.md) ye genel bakış - Güncelleştirilmiş Kaynak Grafiği sorgusu.
- [Azure Monitor'da metrik uyarılar için desteklenen kaynaklar](platform/alerts-metric-near-real-time.md) - Desteklenen ölçümlerve boyutlara güncelleştirin.
- [Eski Log Analytics uyarılarından API'yi yeni Azure Uyarıları API'sine geçirin](platform/alerts-log-api-switch.md) - Değiştirilmiş uyarı adına not eklendi.
- [Azure Monitor'da metrik uyarıların nasıl çalıştığını anlayın.](platform/alerts-metric-overview.md) - Ölçekte izleme için desteklenen kaynak türleri eklendi.

### <a name="application-insights"></a>Application Insights
- [İşçi Hizmeti uygulamaları için Uygulama Öngörüleri (HTTP dışı uygulamalar)](app/worker-service.md) - C# koduna varsayılan günlük düzeyi eklendi. Güncelleştirilmiş paket başvuru sürümü.
- [ApplicationInsights.config reference - Azure](app/configuration-with-applicationinsights-config.md) - Güncelleştirilmiş örnek kod.
- [PowerShell ile Azure Uygulama Öngörülerini Otomatikleştir](app/powershell.md) - Kaynak Yöneticisi şablonuna güncelleştirin.
- [Azure Monitör Uygulama Öngörüleri NuGet paketleri](app/nuget.md) - Güncelleştirilmiş paket sürümleri.
- [Yeni bir Azure Uygulama Öngörüleri kaynağı oluşturun](app/create-new-resource.md) - Genel olarak benzersiz bir ada eklenen not.
- [Canlı Ölçümler Akışı - Azure Uygulama Öngörüleri](app/live-stream.md) - Güncelleştirilmiş ASP.NET Core SDK sürüm gereksinimi ile tanı.
- [Uygulama Öngörüleri'ndeki etkinlik sayaçları](app/eventcounters.md) - Kategori ve tabloyu özel Ölçümlere güncelledi.
- [Azure Application Insights'ta Java izleme günlüklerini keşfedin](app/java-trace-logs.md) - Java aracısı günlüğekaydetme eşiği için yapılandırma eklendi.
- [Application Insights ve Log Analytics tarafından kullanılan IP adresleri](app/ip-addresses.md) - Canlı Ölçümler Akışı için güncelleştirilmiş IP adresleri.
- [Azure uygulama hizmetlerinin performansını izleyin](app/azure-web-apps.md) - Core 3.0 ASP.NET için destek eklendi. 
- [Azure Monitor ile Python uygulamalarını izleyin (önizleme)](app/opencensus-python.md) - Azure'a OpenCensus Python şema eşlemesi için açıklama eklendi. Şema izle
- [Azure Uygulama Öngörüleri için sürüm notları](app/release-notes.md) - Eski sürümler için notlar eklendi.
- [Azure Uygulama Öngörüleri'ndeki Telemetri kanalları](app/telemetry-channels.md) - Bağlantının kaybedildiği uzun süre boyunca atılan veriler için güncelleştirilmiş süre.
- [Azure Application Insights'ta Telemetri örneklemesi](app/sampling.md) - Özel TelemetryInitializer için düzeltilmiş kod parçacığı.
- [Java web projesinde Uygulama Öngörüleri](app/java-troubleshoot.md) Sorun Giderme - JDK 9'da bağımlılık toplamayı desteklememe bildirimi kaldırıldı.

### <a name="insights-and-solutions"></a>Öngörüler ve çözümler
- [Kapsayıcılar için Azure Monitörü Sık Sorulan Sorular](insights/container-insights-faq.md) - Resim ve Ad alanlarına soru eklendi.
- [Azure Monitor'da Azure SQL Analytics çözümü](insights/azure-sql.md) - Güncelleştirilmiş Veritabanı Yönetilen Örnek desteğini bekler.
- [Kapsayıcılar aracısı veri toplama için Azure Monitörünü yapılandırın](insights/container-insights-agent-config.md) - enrich_container_logs için ayar eklendi.
- [Karma Kubernetes kümelerini kapsayıcılar için Azure Monitörü ile yapılandırın](insights/container-insights-hybrid-setup.md) - Sorun giderme bölümü eklendi.
- [Azure Monitor ile Etkin Dizin çoğaltma durumunu izleyin](insights/ad-replication-status.md) - .NET Framework ön koşulu güncelleştirildi.
- [Azure'da Ağ Performans İzleme çözümü](insights/network-performance-monitor.md) - Desteklenen bölgeler eklendi.
- Azure Monitor - .NET Framework ön koşulu [güncelleştirilmiş olarak Active Directory ortamınızı optimize edin.](insights/ad-assessment.md)
- Azure Monitor - .NET Framework ön koşulu [güncelleştirilmiş olarak SQL Server ortamınızı optimize edin.](insights/sql-assessment.md)
- Azure Log Analytics - .NET Framework ön koşulu [güncelleştirildirse Sistem Merkezi Operasyon Yöneticisi ortamınızı optimize edin.](insights/scom-assessment.md)
- [Azure Log Analytics'te BT Hizmet Yönetimi Bağlayıcısı ile desteklenen bağlantılar](platform/itsmc-connections.md) - Müşteri kimliği ve istemci sırrıön koşula New York eklendi.

### <a name="logs"></a>Günlükler
- [Azure Günlük Analizi çalışma alanını silme ve kurtarma](platform/delete-workspace.md) - PowerShell yöntemi eklendi.
- [Azure Monitör Günlükleri dağıtımınızı tasarlama](platform/design-logs-deployment.md) - Çalışma alanı için yutma oranı artırıldı.

### <a name="metrics"></a>Ölçümler
- [Azure Monitor platform ölçümleri Tanı ayarları ile dışa aktarılabilir](platform/metrics-supported-export-diagnostic-settings.md) - Yeni makale.

### <a name="platform-logs"></a>Platform günlükleri
Tanılama ayarlarını kullanarak etkinlik günlüğünü yapılandırmak için yeni bir özelliğe dayalı olarak platform günlükleri için içeriğin yeniden yapılandırılmasının bir parçası olarak güncelleştirilen birden çok makale.

- [Azure kaynak günlüğünü depolama hesabına arşivleme](platform/resource-logs-collect-storage.md)
- [Azure Etkinlik Günlüğü etkinlik şeması](platform/activity-log-schema.md)
- [Azure Monitör hizmet sınırları](service-limits.md)
- [Log Analytics çalışma alanında Azure etkinlik günlüklerini toplama ve analiz](platform/activity-log-collect.md)
- [Tanı lama ayarlarıyla Azure Etkinlik günlüğünü topla (önizleme) - Azure Monitor](platform/diagnostic-settings-legacy.md)
- [Azure kiracılarında Bir Günlük Analizi çalışma alanında Azure Etkinliği günlüklerini topla](platform/activity-log-collect-tenants.md)
- [Log Analytics çalışma alanında Azure kaynak günlüklerini toplama](platform/resource-logs-collect-workspace.md)
- [Kaynak Yöneticisi şablonu kullanarak Azure'da tanılama ayarı oluşturma](platform/diagnostic-settings-template.md)
- [Azure'da günlükleri ve ölçümleri toplamak için tanılama ayarı oluşturma](platform/diagnostic-settings.md)
- [Azure Etkinlik Günlüğünü Dışa Aktarma](platform/activity-log-export.md)
- [Azure platform günlüklerine genel bakış](platform/platform-logs-overview.md)
- [Azure izleme verilerini etkinlik merkezine aktarın](platform/stream-monitoring-data-event-hubs.md)
- [Azure platform günlüğünü bir etkinlik merkezine aktarın](platform/resource-logs-stream-event-hubs.md)

### <a name="quickstarts-and-tutorials"></a>Hızlı başlangıçlar ve öğreticiler

- [Azure Monitor'da bir metrik grafiği oluşturun](learn/tutorial-metrics-explorer.md) - Yeni makale.
- [Azure Kaynağından kaynak günlükleri toplayın ve Azure Monitor ile analiz edin](learn/tutorial-resource-logs.md) - Yeni makale.
- [Azure Monitor ile azure kaynağını izleyin](learn/quick-monitor-azure-resource.md) - Yeni makale.
   
## <a name="next-steps"></a>Sonraki adımlar

- Azure Monitör belgelerine katkıda bulunmak istiyorsanız, [Dokümanlar Katılımcı Kılavuzu'na](https://docs.microsoft.com/contribute/)bakın.