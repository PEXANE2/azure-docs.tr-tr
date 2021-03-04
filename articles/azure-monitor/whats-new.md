---
title: Azure Izleyici belgelerindeki yenilikler
description: Her ay Azure Izleyici belgelerinin önemli güncelleştirmeleri güncelleştirildi.
ms.topic: overview
author: bwren
ms.author: bwren
ms.date: 02/10/2021
ms.openlocfilehash: dd6c44587ce3f4e2b5de940ef831a20a4079c4ef
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102051931"
---
# <a name="whats-new-in-azure-monitor-documentation"></a>Azure Izleyici belgelerindeki yenilikler nelerdir?

Bu makalede, yeni veya önemli ölçüde güncelleştirilmiş Azure Izleyici makaleleri listelenmektedir. Önceki aydan makale güncelleştirmelerini dahil etmek için her ayın ilk haftası yenilenir.

## <a name="january-2021"></a>Ocak 2021 

### <a name="general"></a>Genel 
- [Azure IZLEYICI SSS](faq.md) -Application Insights için cihaz bilgilerine giriş eklendi.
### <a name="agents"></a>Aracılar  
- [Analiz Azure Izleyici günlükleri Için olay izleme (ETW) olayları toplanıyor](./agents/data-sources-event-tracing-windows.md) -yeni makale.
- [Azure izleyici 'de (Önizleme) veri toplama kuralları](./agents/data-collection-rule-overview.md) -POWERSHELL ve CLI örneklerine bağlantılar eklendi.

### <a name="alerts"></a>Uyarılar  
- [Güvenli dışarı aktarma Ile ITSM araçlarına bağlanmak Için Azure 'ı yapılandırın](./alerts/itsm-connector-secure-webhook-connections-azure-configuration.md) -yeni makale.
- [ISMC panosundaki bağlayıcı durumu hataları](./alerts/itsmc-dashboard-errors.md) -yeni makale.
- [ISMC panosunu kullanarak hataları araştırın](./alerts/itsmc-dashboard.md) -yeni makale.
- [Azure ölçüm uyarılarında sorun giderme](./alerts/alerts-troubleshoot-metric.md) -dinamik eşiklere eklenen bölümler.
- BT Hizmet Yönetimi Bağlayıcısı-yeni makaledeki [sorunları giderin](./alerts/itsmc-troubleshoot-overview.md) .

### <a name="application-insights"></a>Application Insights
- [Azure Application Insights telemetri bağıntısı](app/correlation.md) , bir modül bir modül başka bir modül tarafından OpenCensus Python 'da çağırdığında izleme bağıntısı
- [Web sayfaları için Application Insights](app/javascript.md) -yeni makale.
- [Application Insights JAVASCRIPT SDK-yeni makale Için analiz otomatik toplama eklentisi ' ne tıklayın](app/javascript-click-analytics-plugin.md) .
- [Kod değişikliği olmadan uygulamalarınızı izleyin-Azure izleyici için otomatik izleme Application Insights](app/codeless-overview.md) Python sütunu eklendi.
- [JavaScript SDK 'sı için Application Insights eklentisi](app/javascript-react-plugin.md) -yeni makale.
- [Telemetri işlemcileri (Önizleme)-Java ile yeniden yazan Azure izleyici Application Insights](app/java-standalone-telemetry-processors.md) .
- [Azure Application Insights Ile Kullanım Analizi](app/usage-overview.md) -yeni makale.
- [Web uygulaması sorunlarını bulmak Için Azure izleyici 'de uygulama değişikliği analizini kullanın](app/change-analysis.md) -eklenen hata iletileri.


### <a name="insights"></a>Insights    
- [Azure Için Azure izleyici Veri Gezgini (Önizleme)](insights/data-explorer.md) -yeni makale.

### <a name="logs"></a>Günlükler    
- [Azure izleyici müşteri tarafından yönetilen anahtar](./logs/customer-managed-keys.md) -Kullanıcı tarafından atanan yönetilen kimliği tanıtın.
- [Azure izleyici, ayrılmış kümeleri günlüğe kaydeder](./logs/logs-dedicated-clusters.md) -güncelleştirilmiş yanıt kodu.
- [Çapraz hizmet sorgusu-Azure izleyici ve azure Veri Gezgini (Önizleme)](/azure/azure-monitor/platform/azure-data-explorer-monitor-cross-service-query) -yeni makale.

### <a name="metrics"></a>Ölçümler
- [Azure Izleyici ölçüm ölçümleri toplama ve açıklanacak](./essentials/metrics-aggregation-explained.md) yeni makale.

### <a name="platform-logs"></a>Platform günlükleri
- [Azure Izleyici kaynak günlüğü desteklenen hizmetler ve Kategoriler](./essentials/resource-logs-categories.md) -yeni makale.

### <a name="visualizations"></a>Görsel öğeler
- [Azure izleyici çalışma kitapları veri kaynakları](./visualize/workbooks-data-sources.md) -birleştirme ve değişiklik Analizi eklendi.


## <a name="december-2020"></a>Aralık 2020

### <a name="general"></a>Genel
- [Azure izleyici müşterinin yönettiği anahtar tarafından yönetilen](logs/customer-managed-keys.md) hata iletileri.
- [Azure izleyici ile tümleştirilen Iş ortakları](partners.md) Olay Hub 'ı tümleştirmede eklendi bölümü.

### <a name="agents"></a>Aracılar
- [Azure izleyici kullanarak azure Veri Gezgini çapraz kaynak sorgulama](logs/azure-monitor-data-explorer-proxy.md) -yeni makale.
- [Azure izleme aracılarına genel bakış](agents/agents-overview.md) -Oracle 8 desteği eklendi.

### <a name="alerts"></a>Uyarılar
- [Azure ölçüm uyarıları sorunlarını giderme](alerts/alerts-troubleshoot-metric.md) -dinamik eşikler için sorun giderme eklendi.
- [BT Hizmet Yönetimi Bağlayıcısı Log Analytics](alerts/itsmc-definition.md) -yeni makale.
- [BT hizmet yönetimi Bağlayıcısı genel bakış](alerts/itsmc-overview.md) -yeniden yapılandırılmış sorun giderme bilgileri.
- BT Hizmet Yönetimi Bağlayıcısı-yeni makale [Ile Cherwell bağlayın](alerts/itsmc-connections-cherwell.md) .
- BT Hizmet Yönetimi Bağlayıcısı-yeni makale [Ile Provance 'e bağlanın](alerts/itsmc-connections-provance.md) .
- [SCSM 'yi BT hizmet yönetimi Bağlayıcısı](alerts/itsmc-connections-scsm.md) -yeni makale ile bağlayın.
- [ServiceNow 'ı BT hizmet yönetimi Bağlayıcısı](alerts/itsmc-connections-servicenow.md) -yeni makale ile bağlayın.
- [ServiceNow eşitleme sorunları](alerts/itsmc-resync-servicenow.md) -yeniden yapılandırılmış sorun giderme bilgileri el ile nasıl düzeltilir.




### <a name="application-insights"></a>Application Insights
- [JavaScript Web Apps Için Azure Application Insights](app/javascript.md) -bağlantı dizesi kurulumu eklendi.
- [Azure Application Insights standart ölçümleri](app/standard-metrics.md) -yeni makale.
- [Azure izleyici Application Insights Java](app/java-in-process-agent.md) -uygulamanızdan Özel telemetri gönderme hakkında ek bilgiler.
- Telemetri Application Insights tarafından eklenen Tanılama ayarları tabanlı dışarı aktarma için [sürekli dışarı aktarma](app/export-telemetry.md) .
- [Azure işlevleri 'nde .net ve .NET Core uygulamaları için Snapshot Debugger etkinleştirin](app/snapshot-debugger-function-app.md) -yeni makale.
- [Application Insights tarafından kullanılan IP adresleri ve](app/ip-addresses.md) Azure kamu için log ANALYTICS eklenen IP adresleri.
- Tanılama Hizmetleri site uzantısının durum sayfasında [Azure Application Insights Profiler ile ilgili bilgiler ile ilgili sorunları giderin](app/profiler-troubleshooting.md) .
- [Azure Application Insights kullanılabilirlik testlerinizde sorun giderin](app/troubleshoot-availability.md) -ping testleri için sorun gidermeye yönelik güncelleştirmeler.
- [Java Için Azure izleyici Application Insights sorunlarını giderme](app/java-standalone-troubleshoot.md) -yeni makale.

### <a name="containers"></a>Kapsayıcılar
- [Kapsayıcı öngörülerine Ilişkin raporlar](insights/container-insights-reports.md) -yeni makale.

### <a name="logs"></a>Günlükler
- [Azure Izleyici adanmış kümeleri günlüğe kaydeder](logs/logs-dedicated-clusters.md) -otomatik komutlar, bağlantısı kesme ve kaldırma yöntemleri ve sorun giderme için yöntemler eklenmiştir.
- [Azure izleyici ve azure Veri Gezgini (Önizleme) arasında çapraz hizmet sorgusu](logs/azure-data-explorer-monitor-cross-service-query.md) -yeni makale.
- [Azure izleyici 'de (Önizleme) çalışma alanı verilerini dışarı aktarma Log Analytics,](logs/logs-data-export.md) ARM şablonları eklendi.

### <a name="metrics"></a>Ölçümler
- [Azure Ölçüm Gezgini gelişmiş özellikler](essentials/metrics-charts.md) -kaynak kapsamı seçiciyle ilgili bilgiler eklendi.
- [Ölçüm Gezgini birden çok kaynak görüntüleme](essentials/metrics-dynamic-scope.md) -yeni makale.

### <a name="networks"></a>Ağlar
- Azure [izleyici 'de Azure ağ analizi çözümü](insights/azure-networking-analytics.md) -Network Insights çalışma kitabıyla ilgili bilgiler eklendi.

### <a name="virtual-machines"></a>Sanal Makineler
- [Karma ortam Için Azure Izleyicisini etkinleştirin](vm/vminsights-enable-hybrid.md) -bağımlılık aracısının yeni sürümü.


### <a name="visualizations"></a>Görsel öğeler
- [Azure izleyici çalışma kitabı Haritası görselleştirmeleri](visualize/workbooks-map-visualizations.md) -yeni makale.
- [Azure Izleyici çalışma kitapları kendi depolama alanınızı getirir](visualize/workbooks-bring-your-own-storage.md) -yeni makale.


## <a name="november-2020"></a>Kasım 2020

### <a name="general"></a>Genel
- [Azure İzleyici hizmeti limitleri](service-limits.md) -Azure Arc desteği için güncelleştirildi.

### <a name="agents"></a>Aracılar
- Azure [izleme aracılarına genel bakış](agents/agents-overview.md) -Azure Arc desteği için güncelleştirildi.
- [Azure izleyici aracısını yükler](agents/azure-monitor-agent-install.md) -yeni makale.
- [Azure izleyici aracısına genel bakış](agents/azure-monitor-agent-overview.md) -Azure Arc desteği için güncelleştirildi.
- [Aracılar için Kaynak Yöneticisi şablonu örnekleri](agents/resource-manager-agent.md) -Azure Arc desteği için güncelleştirildi.

### <a name="alerts"></a>Uyarılar
- Web kancaları için Azure portal tarafından eklenen kaynak IP adreslerinde [eylem grupları oluşturun ve yönetin](alerts/action-groups.md) .

### <a name="application-insights"></a>Application Insights
- [Java kodsuz kullanacaksınız uygulama izleme Azure izleyici Application Insights](app/java-in-process-agent.md) -eklenen yapılandırma örneği.
- Yanıt verme kancalarını kullanarak [Application Insights JavaScript SDK 'sı](app/javascript-react-plugin.md) tarafından eklenen bölüme tepki verir.
- [Application Insights Java 2. x SDK 'Dan yükseltme](app/java-standalone-upgrade-from-2x.md) -yeni makale.
- [Microsoft. ApplicationInsights. SnapshotCollector Için sürüm notları](app/snapshot-collector-release-notes.md) -yeni makale.

### <a name="autoscale"></a>Otomatik Ölçeklendirme
- Otomatik ölçeklendirmeyi farklı bir bölgeye taşımaya yönelik [Azure 'da otomatik ölçeklendirme ile çalışmaya](autoscale/autoscale-get-started.md) başlayın.

### <a name="data-collection"></a>Veri toplama
- [Azure izleyici Aracısı (Önizleme) için veri toplamayı yapılandırma](agents/data-collection-rule-azure-monitor-agent.md) -Azure Arc desteği için güncelleştirildi.
- [Azure izleyici 'de veri toplama kuralları (Önizleme)](agents/data-collection-rule-overview.md) -Azure Arc desteği için güncelleştirildi.
- [Veri toplama kuralları için şablon örnekleri Kaynak Yöneticisi](agents/resource-manager-data-collection-rules.md) -yeni makale.

### <a name="insights-and-solutions"></a>Öngörüler ve çözümler
- ServiceNow 'a bağlanma üzerine [güvenli dışarı aktarma ile eklenen bölümü kullanarak Azure 'U ITSM araçlarına bağlayın](alerts/it-service-management-connector-secure-webhook-connections.md) .

### <a name="logs"></a>Günlükler
- [Log Analytics ve Excel 'ı tümleştirin](logs/log-excel.md) -yeni makale.
- [Log Analytics Data Security](logs/data-security.md) -ek güvenlik özelliklerine eklenen bölüm.
- Power BI-yeni makale [ile Log Analytics tümleştirme](logs/log-powerbi.md) .
- [Azure izleyici günlük kayıtlarında standart sütunlar](logs/log-standard-columns.md) -_SubscriptionId sütunu eklendi.

Günlük sorgusu içeriğini yeniden yapılandırmak için yeni ve güncelleştirilmiş makaleler.

- [Log Analytics öğreticisi](logs/log-analytics-tutorial.md)
- [Azure Izleyici 'de günlük sorguları](logs/log-query-overview.md)
- [Azure Izleyici 'de Log Analytics genel bakış](logs/log-analytics-overview.md)
- [Azure Veri Gezgini ve Azure Izleyici sorguları için örnekler](/azure/data-explorer/kusto/query/samples?pivots=azuremonitor)
- [Öğretici: Azure Veri Gezgini ve Azure Izleyici 'de kusto sorguları kullanma](/azure/data-explorer/kusto/query/tutorial?pivots=azuremonitor)



### <a name="virtual-machines"></a>Sanal makineler

- [VM Insights 'a genel bakış 'ı etkinleştirin](vm/vminsights-enable-overview.md) -desteklenen bölgeler eklendi.

VM öngörüleri Konuk durumu (Önizleme) için yeni makaleler

- [VM öngörüleri Konuk durumu (Önizleme)](vm/vminsights-health-overview.md)
- [VM öngörüleri Konuk sistem durumu uyarıları (Önizleme)](vm/vminsights-health-alerts.md)
- [VM öngörüleri Konuk sistem durumu 'nda izlemeyi yapılandırma (Önizleme)](vm/vminsights-health-configure.md)
- [Veri toplama kurallarını kullanarak VM öngörüleri Konuk durumunda izlemeyi yapılandırma (Önizleme)](vm/vminsights-health-configure-dcr.md)
- [VM öngörüleri Konuk durumunu etkinleştirme (Önizleme)](vm/vminsights-health-enable.md)
- [VM Insights Konuk durumu sorunlarını giderme (Önizleme)](vm/vminsights-health-troubleshoot.md)





## <a name="october-2020"></a>Ekim 2020

### <a name="general"></a>Genel
- [Azure İzleyici API 'si emekli](logs/operationalinsights-api-retirement.md) -yeni makale.

### <a name="agents"></a>Aracılar
- Aracılarda [Azure izleyici tarafından](monitor-reference.md) eklenen bölümde izlenen bölüm.

### <a name="alerts"></a>Uyarılar
- Hizmet etiketindeki Azure portal tarafından eklenen bölümde [eylem grupları oluşturun ve yönetin](alerts/action-groups.md) .
- [Ölçüm uyarıları için Kaynak Yöneticisi şablonu örnekleri](alerts/resource-manager-alerts-metric.md) -eklenen içerik eşleştirme parametresi ve test konumları.
- [Azure ölçüm uyarıları sorunlarını giderme](alerts/alerts-troubleshoot-metric.md) -kural yapılandırması için en iyi yöntem eklendi.

### <a name="application-insights"></a>Application Insights
- [Application Insights JavaScript SDK 'sı Için angular eklentisi](app/javascript-angular-plugin.md) -yeni makale.
- [ASP.NET Core uygulamalar Için Azure Application Insights](app/asp-net-core.md) -ıllogger GÜNLÜKLERI hakkında SSS eklendi.
- [Azure Application Insights ile ASP.NET için Izlemeyi yapılandırma](app/asp-net.md) -makale yeniden yazıldı.
- [Azure Application Insights 'de günlük tabanlı ve önceden toplanmış ölçümler](app/pre-aggregated-metrics-log-metrics.md) önceden toplanmış ölçümler içeren tablolara eklendi.
- Konum popülasyon etiketlerindeki [Web sitesi tarafından eklenen herhangi bir bölümün kullanılabilirliğini ve yanıt hızını izleyin](app/monitor-web-app-availability.md) .
- [Java uygulamalarını her yerde izleyin-Azure izleyici Application Insights](app/java-standalone-config.md) eklenen yapılandırma örneği.
- [Java uygulamalarını her yerde izleyin-Azure izleyici Application Insights](app/java-standalone-telemetry-processors.md) -yeni makale.
- Web uygulaması sorunları-sanal makinelere ve etkinlik günlüğüne eklenen bölümleri [bulmak Için Azure izleyici 'de uygulama değişikliği analizini kullanın](app/change-analysis.md) .
  
### <a name="autoscale"></a>Otomatik Ölçeklendirme
- Otomatik ölçeklendirmeyi farklı bir bölgeye taşımaya yönelik [Azure 'da otomatik ölçeklendirme ile çalışmaya](autoscale/autoscale-get-started.md) başlayın.

### <a name="containers"></a>Kapsayıcılar
- [Container Insights Ile BD Izlemesini yapılandırma](containers/container-insights-persistent-volumes.md) -yeni makale.
- [Kapsayıcı öngörüleri aracısını yönetme](containers/container-insights-manage-agent.md) -Azure Arc etkin Kubernetes kümesi için destek eklendi.
- [Kapsayıcı öngörülerine yönelik ölçüm uyarıları](containers/container-insights-metric-alerts.md) -Azure Arc etkinleştirilmiş Kubernetes kümesi için destek eklendi.

### <a name="insights-and-solutions"></a>Öngörüler ve çözümler
- [BT hizmet yönetimi Bağlayıcısı-Azure izleyici 'de güvenli dışarı aktarma](alerts/it-service-management-connector-secure-webhook-connections.md) bölümünde ServiceNow üzerinde bölüm eklendi.

### <a name="logs"></a>Günlükler
- [Logic App-yeni makale kullanarak Log Analytics çalışma alanındaki verileri Azure depolama 'Ya arşivleme](logs/logs-export-logic-app.md) .
- [Azure izleyici 'de (Önizleme) çalışma alanı verilerini dışarı aktarma Log Analytics](logs/logs-data-export.md) -Olay Hub 'ı için REST isteği için örnek gövde eklendi.
- Azure [Izleyici günlükleri için kullanımı ve maliyetleri yönetme](logs/manage-cost-storage.md) -Azure izleyici günlükleri ve Azure Güvenlik Merkezi faturalaması arasındaki ilişkiyle ilgili bilgiler eklendi. Düğüm başına fiyatlandırma katmanı kullanılıyorsa düğüm sayıları için sorgu eklendi. 
- [Azure izleyici 'de Log Analytics çalışma alanının sistem durumunu izleme](logs/monitor-workspace.md) -yeni makale.
- Azure [Veri Gezgini (Önizleme) kullanarak Azure izleyici 'de verileri sorgulama](logs/azure-data-explorer-monitor-proxy.md) -yeni makale.
- Azure [Veri Gezgini (Önizleme) kullanarak Azure izleyici 'den aktarılmış verileri sorgulama](logs/azure-data-explorer-query-storage.md) -yeni makale.

### <a name="networks"></a>Ağlar
- [Ağlar Için Azure Izleyici önizlemesi](insights/network-insights-overview.md) -sorun giderme bölümü. Bağlantı üzerine bölüm eklendi.

### <a name="platform-logs"></a>Platform günlükleri
- [Azure etkinlik günlüğü olay şeması](essentials/activity-log-schema.md) -önem düzeylerinin açıklaması eklendi.

### <a name="virtual-machines"></a>Sanal makineler
- [VM'ler için Azure izleyici analiz değişikliği](vm/vminsights-change-analysis.md) -yeni makale.
- [VM'ler için Azure izleyici genel bakışı etkinleştir](vm/vminsights-enable-overview.md) -desteklenen bölgeler eklendi.
- [Ölçümler Için kapsayıcı öngörülerini güncelleştirme](containers/container-insights-update-metrics.md) Azure Arc etkinleştirilmiş Kubernetes kümesi için destek eklendi.



## <a name="september-2020"></a>Eylül 2020

### <a name="general"></a>Genel
- [Azure IZLEYICI SSS](faq.md) -opentelemetri üzerine bölüm eklendi.

### <a name="agents"></a>Aracılar
- [Azure izleyici aracısına genel bakış](agents/azure-monitor-agent-overview.md) -yeni aracıya geçiş için karar etmenleri eklenmiştir.
- [Azure izleme aracılarına genel bakış](agents/agents-overview.md) -Windows 10 için destek eklendi.

### <a name="alerts"></a>Uyarılar
- [Azure Resource Manager şablonuyla bir günlük uyarısı oluşturun](alerts/alerts-log-create-templates.md) -yeni makale.
- [Azure ölçüm uyarıları sorunlarını giderme](alerts/alerts-troubleshoot-metric.md) -bir ölçüm uyarısı KURALı için ARM şablonunu dışarı aktarma bölümü eklendi.

### <a name="application-insights"></a>Application Insights
- [Yeni bir Azure Izleyici oluşturun Application Insights çalışma alanı tabanlı kaynak](app/create-workspace-resource.md) kaldırıldı önizleme belirtimi.
- [Azure Application Insights veri saklama ve depolama](app/data-retention-privacy.md) , Mac ve Linux veri kaybı koruması için yeni destek için Ayrıntılar eklendi.
- [Application Insights,](app/eventcounters.md) varsayılan olarak toplanan sayaçlara eklenen notta olay sayaçları.
- [Azure Application Insights ile önceden toplanmış ve önceden toplanmış ölçümler,](app/pre-aggregated-metrics-log-metrics.md) önizleme atamasıdır.
- [Bir Azure izleyici Application Insights klasik kaynağını çalışma alanı tabanlı bir kaynağa geçirme](app/convert-classic-resource.md) -yeni makale.
- [Tüm ortamlarda Java uygulamalarını izleme-Azure izleyici Application Insights](app/java-in-process-agent.md) -aracının yeni önizleme sürümü için güncelleştirildi.
- [Azure Application Insights ile ASP.NET için Web App Analytics 'ı ayarlama](app/asp-net.md) -makale yeniden yazıldı.
- [Azure Application Insights telemetri kanalları](app/telemetry-channels.md) -Mac ve Linux veri kaybı koruması için yeni destek için Ayrıntılar eklendi.
- [Azure Application Insights Snapshot Debugger sorunlarını](app/snapshot-debugger-troubleshoot.md) giderme-Snapshot Debugger sorun GIDERME için SSL bölümü eklendi.
- Web uygulaması sorunlarını, eklenen sanal makine ve etkinlik günlüğünü [bulmak Için Azure izleyici 'de uygulama değişikliği analizini kullanın](app/change-analysis.md) .


### <a name="containers"></a>Kapsayıcılar
- [Azure Arc etkin Kubernetes kümesini kapsayıcı öngörüleri Ile yapılandırma](containers/container-insights-enable-arc-enabled-clusters.md) -hizmet sorumlusu kullanılarak izlemeyi etkinleştirme Kılavuzu eklendi.
- [Dağıtım & kapsayıcı öngörüleri Ile HPA ölçümleri](containers/container-insights-deployment-hpa-metrics.md) -yeni makale.

### <a name="insights-and-solutions"></a>Öngörüler ve çözümler
- Reda tarafından kaldırılan önizleme ataması [için Azure önbelleği Için Azure izleyici](insights/redis-cache-insights-overview.md) .
- [Ağlar Için Azure izleyici (Önizleme)](insights/network-insights-overview.md) -bağlantı ve trafik bölümleri eklendi.
- [BT hizmet yönetimi Bağlayıcısı-Azure izleyici 'de güvenli dışarı aktarma](alerts/it-service-management-connector-secure-webhook-connections.md) -yeni makale.
- [Azure izleyici 'de BT hizmet yönetimi Bağlayıcısı](alerts/itsmc-connections.md) -Cherwell ve Provance ISM tümleştirmelerine göz önünde.
- Key Vault kaldırılan önizleme belirtimi [Için Azure izleyici ile Key Vault izleyin](insights/key-vault-insights-overview.md) .

### <a name="logs"></a>Günlükler
- [Azure izleyici günlük sorgularıyla denetim sorguları](logs/query-audit.md) -yeni makale.
- [Azure izleyici müşteri tarafından yönetilen, anahtar](logs/customer-managed-keys.md) eklenen müşteri Kasası.
- [Azure Izleyici ayrılmış kümeleri günlüğe kaydeder](logs/logs-dedicated-clusters.md) -yeni makale.
- [Azure Izleyici günlükleri dağıtımını tasarlama](logs/design-logs-deployment.md) -güncelleştirilmiş ölçek ve Alım birimi hız limiti bölümü.
- [Azure izleyici Log Analytics günlük sorgu kapsamı](logs/scope.md) -çalışma alanı tabanlı uygulamaları dahil etmek için güncelleştirmeler.
- [Azure izleyici 'de Günlükler](logs/data-platform-logs.md) -çalışma alanı tabanlı uygulamaları dahil etmek için güncelleştirmeler.
- [Azure izleyici günlük kayıtlarında standart sütunlar](logs/log-standard-columns.md) -çalışma alanı tabanlı uygulamaları dahil etmek için güncelleştirmeler.
- [Azure İzleyici hizmeti sınırları](service-limits.md) -Kullanıcı sorgu azaltma için sınırlamalar güncelleştirildi.
- [Azure izleyici 'de müşteri tarafından yönetilen depolama hesaplarını kullanma Log Analytics](logs/private-storage.md) -makaleye yeniden yazıldı.
- [Azure Log Analytics 'de verileri görüntüleme ve çözümleme](./logs/data-platform-logs.md) -çalışma alanı tabanlı uygulamaları dahil etmek için güncelleştirmeler.


### <a name="platform-logs"></a>Platform günlükleri
- [Azure etkinlik günlüğü olay şeması-Azure izleyici](essentials/activity-log-schema.md) -önem düzeyleri eklendi.
- [Tanılama ayarları için şablon örnekleri Kaynak Yöneticisi](essentials/resource-manager-diagnostic-settings.md) -Azure depolama hesabı için örnek eklendi.

### <a name="visualizations"></a>Görsel öğeler
- [Azure izleyici çalışma kitabı grafik görselleştirmeleri](visualize/workbooks-chart-visualizations.md) -yeni makale.
- [Azure izleyici çalışma kitabı bileşik çubuk Oluşturucusu](visualize/workbooks-composite-bar.md) -yeni makale.
- [Azure izleyici çalışma kitabı grafik görselleştirmeleri](visualize/workbooks-graph-visualizations.md) -yeni makale.
- [Azure izleyici çalışma kitabı kılavuz görselleştirmeleri](visualize/workbooks-grid-visualizations.md) -yeni makale.
- [Azure izleyici çalışma kitabı Honey çağrısıyla görselleştirmeleri](visualize/workbooks-honey-comb.md) -yeni makale.
- [Azure izleyici çalışma kitabı metin görselleştirmeleri](visualize/workbooks-text-visualizations.md) -yeni makale.
- [Azure izleyici çalışma kitabı kutucuk görselleştirmeleri](visualize/workbooks-tile-visualizations.md) -yeni makale.
- [Azure izleyici çalışma kitabı ağaç görselleştirmeleri](visualize/workbooks-tree-visualizations.md) -yeni makale.




## <a name="august-2020"></a>Ağustos 2020

### <a name="general"></a>Genel

- [Azure izleyici tarafından izlenen](monitor-reference.md) -Azure izleyici aracısını içerecek şekilde güncelleştirildi.


### <a name="agents"></a>Aracılar
- [Azure izleyici aracısına genel bakış](agents/azure-monitor-agent-overview.md) -yeni makale.
- [Karma ortam Için Azure Izleyicisini etkinleştirin](vm/vminsights-enable-hybrid.md) -bağımlılık Aracısı sürümü güncelleştirildi.
- [Azure izleme aracılarına genel bakış](agents/agents-overview.md) -Azure izleyici Aracısı ve birleştirilmiş işletim sistemi desteği tablosu eklendi.


#### <a name="new-and-updated-articles-from-restructure-of-agent-content"></a>Aracı içeriğini yeniden yapılandırmak için yeni ve güncelleştirilmiş makaleler
- [VM öngörülerini etkinleştir genel bakış](vm/vminsights-enable-overview.md)
- [Linux bilgisayarlarına Log Analytics aracısını yükleme](agents/agent-linux.md)
- [Windows bilgisayarlarına Log Analytics aracısını yükleme](agents/agent-windows.md)
- [Log Analytics aracısına genel bakış](agents/log-analytics-agent.md)

### <a name="application-insights"></a>Application Insights
- [JavaScript Web Apps Için Azure Application Insights](app/javascript.md) -istemci sunucu bağıntısını ve CORS bağıntısını yapılandırmayı açıklığa kavuşturan bölüm eklendi.
- Çalışma alanı tabanlı uygulamalar tarafından sunulan [Yeni bir Azure izleyici Application Insights çalışma alanı tabanlı, kaynak](app/create-workspace-resource.md) tarafından eklenen yetenekler oluşturun.
- [Application Insights ve Log Analytics tarafından](app/ip-addresses.md) güncelleştirilmiş IP adresleri, canlı ölçümler akışı için kullanılır.
- Desteklenen özel telemetri için [Azure izleyici Application Insights eklenen tablo aracılığıyla Java uygulamalarını izleyin](app/java-in-process-agent.md) .
- [Application Insights JavaScript SDK 'sı Için yerel](app/javascript-react-native-plugin.md) yanıt verme eklentisi-yeni makale.
- [JavaScript SDK 'sı için Application Insights eklentisi](app/javascript-react-plugin.md) -yeni makale.
- [Application Insights izleme-yeni makale Ile Azure işlev uygulamaları oluşturmaya yönelik Kaynak Yöneticisi Şablon örneği](app/resource-manager-function-app.md) .
- [Application Insights izleme-yeni makale Ile Azure Uygulama Hizmetleri Web uygulamaları oluşturmaya yönelik Kaynak Yöneticisi şablon örnekleri](app/resource-manager-web-app.md) .
- [Azure Application Insights eklenen video Ile Kullanım Analizi](app/usage-overview.md) .

### <a name="autoscale"></a>Otomatik Ölçeklendirme
- App Service sağlıklı örneklere yönlendirmeye yönelik [Azure 'da otomatik ölçeklendirme ile çalışmaya](autoscale/autoscale-get-started.md) başlayın.

### <a name="data-collection"></a>Veri toplama
- [Azure izleyici Aracısı (Önizleme) için veri toplamayı yapılandırma](agents/data-collection-rule-azure-monitor-agent.md) -yeni makale.
- [Azure izleyici 'de veri toplama kuralları (Önizleme)](agents/data-collection-rule-overview.md) -yeni makale.


### <a name="containers"></a>Kapsayıcılar
- [Dağıtım & kapsayıcı öngörüleri Ile HPA ölçümleri](containers/container-insights-deployment-hpa-metrics.md) -yeni makale.

### <a name="insights"></a>Insights
- [Azure izleyici 'de çözümleri izleme](insights/solutions.md) -yeni kullanıcı arabirimi için güncelleştirildi.
- [Azure 'da ağ performansı İzleyicisi çözüm,](insights/network-performance-monitor.md) desteklenen çalışma alanı bölgelerini ekledi.


### <a name="logs"></a>Günlükler
- [Azure IZLEYICI SSS](faq.md) -çalışma alanından veri silmeye yönelik giriş eklendi. 502 ve 503 yanıtlarına giriş eklendi.
  - [Azure Izleyici günlükleri dağıtımını tasarlama](logs/design-logs-deployment.md) -alma birimi hız sınırı bölümündeki güncelleştirmeler.
- [Azure Izleyici günlükleri için kullanımı ve maliyetleri yönetme](logs/manage-cost-storage.md) -kullanım sorguları, daha verimli sorgu biçimine karşı güncelleştirildi.
- [Azure izleyici 'de günlük sorgularını iyileştirme](logs/query-optimization.md) -performans göstergelerine belirli değerler ekledi.
- [Tanılama ayarları için şablon örnekleri Kaynak Yöneticisi](essentials/resource-manager-diagnostic-settings.md) -günlük sorgusu denetim günlükleri için örnek eklendi.


### <a name="platform-logs"></a>Platform günlükleri
- [Platform günlüklerini ve ölçümleri, Tanılama ayarları için farklı hedeflerle eklenen bölgesel gereksinime göndermek için Tanılama ayarları oluşturun](essentials/diagnostic-settings.md) .

### <a name="visualizations"></a>Görsel öğeler
- [Azure Izleyici çalışma kitaplarına genel bakış](visualize/workbooks-overview.md) -video eklendi.
- Bir [Azure çalışma kitabı şablonunu başka bir bölgeye taşıma](visualize/workbook-templates-move-region.md) -yeni makale.
- [Bir Azure çalışma kitabını başka bir bölgeye taşıma](visualize/workbooks-move-region.md) -yeni makale.



## <a name="july-2020"></a>Temmuz 2020

### <a name="general"></a>Genel
- [Azure Izleyicisini dağıtma](deploy-scale.md) -VM öngörüleri ekleme içeriğini yeniden yapılandırabilir.
- [Azure özel bağlantısı 'Nı kullanarak ağları sınırlı Azure izleyici 'ye güvenli](logs/private-link-security.md) bir şekilde bağlayın.

### <a name="alerts"></a>Uyarılar
- [Azure izleyici uyarıları Için eylem kuralları](alerts/alerts-action-rules.md) -CLI işlemleri eklendi.
- Azure portal, Kullanıcı arabirimindeki değişiklikleri yansıtacak şekilde güncellenen [eylem grupları oluşturun ve yönetin](alerts/action-groups.md) .
- [Azure izleyici 'de örnek sorgular Log Analytics](logs/example-queries.md) -yeni makale.
- Uyarı kuralı kotasında Azure Izleyici tarafından eklenen bölümde [günlük uyarılarına sorun giderme](alerts/alerts-troubleshoot-log.md) .
- [Azure ölçüm uyarıları sorunlarını giderme](alerts/alerts-troubleshoot-metric.md) -henüz yayınlanmayan özel bir ölçümde uyarı kuralında bölüm eklendi.
- [Ölçüm uyarılarının Azure Izleyici 'de nasıl çalıştığını anlayın.](alerts/alerts-metric-overview.md) -Toplama ayrıntı düzeyi seçme önerisi eklendi.

### <a name="application-insights"></a>Application Insights
- [Azure Web App uzantısı Için sürüm notları-Application Insights](app/web-app-extension-release-notes.md) -yeni makale.
- [Application Insights kaynakları için şablon örnekleri Kaynak Yöneticisi](app/resource-manager-app-resource.md) -yeni makale.
- Azure App Service ASP.NET Core uygulamalar için Profil Oluşturucu çalıştıran hata hakkında [Azure Application Insights Profiler ile ilgili sorunları giderin](app/profiler-troubleshooting.md) . 

### <a name="containers"></a>Kapsayıcılar
- [Kapsayıcı öngörülerinin günlük uyarıları](containers/container-insights-log-alerts.md) -yeni makale.
- [Kapsayıcı öngörülerinin ölçüm uyarıları](containers/container-insights-metric-alerts.md) -yeni makale.

### <a name="logs"></a>Günlükler
- [Azure izleyici müşterinin yönettiği anahtar tarafından yönetilen](logs/customer-managed-keys.md) hata iletisi ve bölüm CMK yapılandırması.
- [Azure IZLEYICI http veri toplayıcı API 'si](logs/data-collector-api.md) -Python 3 örneği eklendi.
- Alt sorgular kullanılırken birden çok veri taramasını önleme başlıklı bölümde [Azure izleyici 'de günlük sorgularını en iyileştirin](logs/query-optimization.md) .
- [Öğretici: Log Analytics sorguları ile çalışmaya](./logs/log-analytics-tutorial.md) başlayın-video eklendi.

### <a name="platform-logs"></a>Platform günlükleri
- [Platform günlüklerini ve ölçümlerini farklı hedeflerle eklenen videoya göndermek için Tanılama ayarları oluşturun](essentials/diagnostic-settings.md) .
- [Azure izleyici için Kaynak Yöneticisi şablon örnekleri](/resource-manager-samples.md) -Günlükler hedef türü kullanılarak ARM örneği eklendi. 

### <a name="solutions"></a>Çözümler
- [Azure izleyici 'de çözümleri izleme](insights/solutions.md) -CLI işlemlerinde eklendi.
- [Azure 'Da Office 365 yönetim çözümü](insights/solution-office-365.md) -devre dışı bırakma tarihi değiştirildi.

### <a name="virtual-machines"></a>Sanal makineler

VM Insights içeriğini yeniden yapılandırmak için yeni ve güncelleştirilmiş makaleler

- [VM öngörüleri nedir?](vm/vminsights-overview.md)
- [VM öngörüleri için Log Analytics çalışma alanını yapılandırma](vm/vminsights-configure-workspace.md)
- [Linux bilgisayarlarını Azure Izleyici 'ye bağlama](agents/agent-linux.md)
- [Karma ortam için Azure Izleyicisini etkinleştirme](vm/vminsights-enable-hybrid.md)
- [Azure portal tek bir sanal makine veya sanal makine ölçek kümesi için Azure Izleyicisini etkinleştirin](vm/vminsights-enable-portal.md)
- [Azure Ilkesini kullanarak VM öngörülerini etkinleştirme](./vm/vminsights-enable-policy.md)
- [VM öngörülerini etkinleştir genel bakış](vm/vminsights-enable-overview.md)
- [PowerShell kullanarak VM öngörülerini etkinleştirme](vm/vminsights-enable-powershell.md)
- [Kaynak Yöneticisi şablonları kullanarak VM öngörülerini etkinleştirme](vm/vminsights-enable-resource-manager.md)
- [PowerShell veya şablonlar ile VM öngörülerini etkinleştirme](./vm/vminsights-enable-powershell.md)


### <a name="visualizations"></a>Görsel öğeler
- [Log Analytics Pano görselleştirmelerinizi yükseltme](logs/dashboard-upgrade.md) -yenileme oranı güncelleştirildi.
- [Azure izleyici 'den verileri görselleştirme](visualizations.md) -eklenen video.


## <a name="june-2020"></a>Haziran 2020

### <a name="general"></a>Genel
- [Azure Izleyiciyi dağıtma](deploy-scale.md) -yeni makale.
- [Azure izleyici müşteri tarafından yönetilen anahtar](logs/customer-managed-keys.md) -güncelleştirilmiş billingtype özelliği. PowerShell komutları eklendi.

### <a name="agents"></a>Aracılar
- [Log Analytics aracısına genel bakış](agents/log-analytics-agent.md) -Python 2 gereksinimi eklendi.

### <a name="alerts"></a>Uyarılar
- [Hedef kaynakları farklı bir Azure bölgesine hareket ettirildiğinde uyarı kurallarını veya eylem kurallarını güncelleştirme](alerts/alerts-resource-move.md) -yeni makale.
- [Azure ölçüm uyarıları sorunlarını giderme](alerts/alerts-troubleshoot-metric.md) -yeni makale.
- [Azure izleyici 'de günlük uyarıları sorunlarını giderme](alerts/alerts-troubleshoot-metric.md) -yeni makale.
  
### <a name="application-insights"></a>Application Insights
- [JavaScript Web Apps Için Azure Application Insights](app/javascript.md) -JavaScript SDK 'sı bölümüne güncelleştirin. Yükleme başarısızlıklarını raporlamak için kod parçacığı güncelleştirildi.
- [Profil oluşturucu & Snapshot Debugger-yeni makale için KCG 'leri (kendi depolama alanınızı getirin) yapılandırma](app/profiler-bring-your-own-storage.md) .
- [Azure 'da, Opencensus Python Ile gelen Istek izleme Application Insights](app/opencensus-python-request.md) , opencensus için günlük kaydı ve yapılandırma güncelleştirildi.
- [Azure Application Insights ile canlı bir ASP.NET Web uygulamasını izleme](app/monitor-performance-live-website-now.md) -durum İzleyicisi v1 için kullanım dışı bırakma tarihi güncelleştirildi.
- [Azure Application Insights ile Node.js hizmetleri izleme](app/nodejs.md) -birden çok güncelleştirme, sanal SÜRÜMLERDEN ve SDK yapılandırmasından geçiş de dahil olmak üzere
- Azure izleyici dışarı aktarımlarını yapılandırma bölümüne [Azure izleyici (Önizleme) Ile Python uygulamalarını izleyin](app/opencensus-python.md) .
- [Kod değişikliği olmadan uygulamalarınızı izleyin-Azure izleyici için otomatik izleme Application Insights](app/codeless-overview.md) -yeni makale.
- [JavaScript Web uygulamaları IÇIN SDK yükleme hatası giderme](app/javascript-sdk-load-failure.md) -yeni makale.

### <a name="containers"></a>Kapsayıcılar
- Yay etkin Kubernetes için [karma Kubernetes kümesi](containers/container-insights-optout-hybrid.md) ile eklenen bölümü izlemeyi durdurma.
- [Azure Arc etkin Kubernetes kümesini kapsayıcı öngörüleri Ile yapılandırma](containers/container-insights-enable-arc-enabled-clusters.md) -yeni makale.
- [Kapsayıcı öngörüleri Ile Azure Red Hat OpenShift v4. x yapılandırma](containers/container-insights-azure-redhat4-setup.md) önkoşulları güncelleştirildi.
- [Container Insights canlı verilerini ayarlama (Önizleme)](containers/container-insights-livedata-setup.md) -Azure ABD kamu 'da kullanılamayan özellik hakkında not kaldırıldı.

### <a name="insights"></a>Insights
- [SSS-Azure 'da ağ performansı İzleyicisi çözüm](insights/network-performance-monitor-faq.md) -ExpressRoute IZLEYICISI için SSS eklendi.

### <a name="logs"></a>Günlükler
- [Azure Log Analytics çalışma alanını silme ve kurtarma](logs/delete-workspace.md) -PowerShell komutu eklendi. Sorun giderme güncelleştirildi.
- Azure RBAC bölümünde izin verilmeyen tablolar için [Azure izleyici 'de Log Analytics çalışma alanlarını yönetin](logs/manage-access.md) .
- [Azure Izleyici günlükleri için kullanımı ve maliyetleri yönetme](logs/manage-cost-storage.md) -veri boyutunun hesaplanmasıyla ilgili ek ayrıntılar. Veri birimi uyarılarını yapılandırma güncelleştirildi. Azure Sentinel tarafından toplanan Güvenlik verileriyle ilgili ayrıntılar. Veri ucunun açıklaması.
- [Azure Izleyici günlüklerini Azure Logic Apps ve Power otomatikleştir tarafından eklenen bağlayıcı limitleriyle kullanın](logs/logicapp-flow-connector.md) .

### <a name="metrics"></a>Ölçümler
- [Azure izleyici kaynak türüne göre desteklenen ölçümleri](essentials/metrics-supported.md) SQL Server.


### <a name="platform-logs"></a>Platform günlükleri

- [Tanılama ayarları için şablon örnekleri Kaynak Yöneticisi](essentials/resource-manager-diagnostic-settings.md) -etkinlik günlüğü tanılaması için düzelme ayarı.
- [Azure Portal yeni makale kullanarak Log Analytics çalışma alanına Azure etkinlik günlüğü gönderin](essentials/quick-collect-activity-log-portal.md) .
- [Azure Resource Manager şablonu kullanarak Log Analytics çalışma alanına Azure etkinlik günlüğü gönderin](essentials/quick-collect-activity-log-arm.md) -yeni makale.

Platform günlüğü içeriğinin yeniden ve konsolidasyonundan yeni ve güncelleştirilmiş makaleler

- [Azure Kaynak günlüklerini depolama hesabına arşivleme](./essentials/resource-logs.md#send-to-azure-storage)
- [Azure etkinlik günlüğü olay şeması](essentials/activity-log-schema.md)
- [Azure etkinlik günlüğü](essentials/activity-log.md)
- [Azure Izleyici CLı örnekleri](/cli-samples.md)
- [Azure Izleyici PowerShell örnekleri](/powershell-samples.md)
- [Azure İzleme REST API'si adım adım kılavuzu](essentials/rest-api-walkthrough.md)
- [Azure kaynak günlüğü desteklenen Hizmetleri ve şemaları](./essentials/resource-logs-schema.md)
- [Azure kaynak günlükleri](essentials/resource-logs.md)
- [Azure Izleyici 'de Azure etkinlik günlüğünü toplayın ve çözümleyin](./essentials/activity-log.md)
- [Log Analytics çalışma alanında Azure Kaynak günlüklerini toplayın](./essentials/resource-logs.md#send-to-log-analytics-workspace)
- [Platform günlüklerini ve ölçümlerini farklı hedeflere göndermek için tanılama ayarlarını oluşturma](essentials/diagnostic-settings.md)
- [Azure etkinlik günlüğünü dışarı aktarma](./essentials/activity-log.md#legacy-collection-methods)
- [Azure platform günlüklerine genel bakış](essentials/platform-logs-overview.md)
- [Azure platformu günlüklerini bir olay hub 'ına akış](./essentials/resource-logs.md#send-to-azure-event-hubs)
- [Azure Izleyici 'de Azure etkinlik günlüğü olaylarını görüntüleme](./essentials/activity-log.md#view-the-activity-log)

### <a name="virtual-machines"></a>Sanal makineler
- [Azure Portal 'de VM öngörülerini etkinleştirin](./vm/vminsights-enable-portal.md) -Azure Arc 'ı içerecek şekilde güncelleştirildi.
- [VM öngörülerini etkinleştirin](vm/vminsights-enable-overview.md) -Azure yay içerecek şekilde güncelleştirildi.
- [VM öngörüleri nedir?](vm/vminsights-overview.md) -Azure yayı içerecek şekilde güncelleştirildi.


### <a name="visualizations"></a>Görsel öğeler
- [Azure izleyici çalışma kitapları veri kaynakları](visualize/workbooks-data-sources.md) -uyarılar ve özel uç noktalar bölümü eklendi.
- [Azure izleyici çalışma kitabı tabanlı Öngörüler sorunlarını giderme](insights/troubleshoot-workbooks.md) -yeni makale.
- [Log Analytics Pano görselleştirmelerinizi yükseltme](logs/dashboard-upgrade.md) -yeni makale.



## <a name="may-2020"></a>Mayıs 2020

### <a name="general"></a>Genel

- [Azure izleyici hakkında SSS](faq.md) -ölçümler için bölüm eklendi.
- [Azure izleyici müşteri tarafından yönetilen anahtar](logs/customer-managed-keys.md) -genel kullanıma hazırlık için çeşitli değişiklikler.
- [Azure izleyici Için yerleşik ilke tanımları](./policy-reference.md) -yeni makale.
- [Günlük alımı Için müşterinin sahip olduğu depolama hesapları](logs/private-storage.md) -yeni makale.
- [Azure Izleyici günlükleri için kullanımı ve maliyetleri yönetme](logs/manage-cost-storage.md) -küme orantılı faturalandırması eklendi.
- [Ağları Azure izleyici 'ye güvenli bir şekilde bağlamak Için Azure özel bağlantısı 'Nı kullanın](logs/private-link-security.md) -yeni makale.


#### <a name="new-resource-manager-template-samples"></a>Yeni Kaynak Yöneticisi şablonu örnekleri 
- [Azure Izleyici için Kaynak Yöneticisi şablonu örnekleri](/resource-manager-samples.md)
- [Eylem grupları için şablon örnekleri Kaynak Yöneticisi](alerts/resource-manager-action-groups.md)
- [Aracılar için Kaynak Yöneticisi şablonu örnekleri](agents/resource-manager-agent.md)
- [Kapsayıcı öngörüleri için Kaynak Yöneticisi şablon örnekleri](containers/resource-manager-container-insights.md)
- [VM öngörüleri için şablon örnekleri Kaynak Yöneticisi](vm/resource-manager-vminsights.md)
- [Tanılama ayarları için şablon örnekleri Kaynak Yöneticisi](essentials/resource-manager-diagnostic-settings.md)
- [Log Analytics çalışma alanları için şablon örnekleri Kaynak Yöneticisi](logs/resource-manager-workspace.md)
- [Günlük sorguları için şablon örnekleri Kaynak Yöneticisi](logs/resource-manager-log-queries.md)
- [Günlük sorgusu uyarı kuralları için Kaynak Yöneticisi şablonu örnekleri](alerts/resource-manager-alerts-log.md)
- [Ölçüm uyarı kuralları için Kaynak Yöneticisi şablonu örnekleri](alerts/resource-manager-alerts-metric.md)
- [Çalışma kitapları için şablon Kaynak Yöneticisi örnekleri](visualize/resource-manager-workbooks.md)

### <a name="agents"></a>Aracılar
- [Windows Azure tanılama uzantısı 'nı (WAD) yükleyip yapılandırma](agents/diagnostics-extension-windows-install.md) tanılamayı yapılandırma hakkında ayrıntılı bilgi.
- [Log Analytics aracısına genel bakış](agents/log-analytics-agent.md) -desteklenen Linux sürümleri eklendi.

### <a name="application-insights"></a>Application Insights

- [Azure işlevleri 'nde çalışan uygulamaları Application Insights-Azure izleyici](app/monitor-functions.md) -yeni makale ile izleyin.
- [Azure Application Insights ile Node.js hizmetleri izleme](app/nodejs.md) -önceki sürümlerden geçiş hakkında yeni bölüm içeren genel güncelleştirmeler.
- Web kancaları ve ABD kamu için [Application Insights ve Log Analytics tarafından eklenen IP adresleri tarafından kullanılan IP adresleri](app/ip-addresses.md) .
- [Azure Kubernetes Service (AKS) üzerindeki uygulamaları Application Insights-Azure izleyici](app/kubernetes-codeless.md) -yeni makale ile izleyin.
- DotNet-Trace ile günlükleri toplama konusunda .NET 'e eklenen bölüm [için veri Application Insights sorun giderme](app/asp-net-troubleshoot-no-data.md) .
- Web uygulaması sorunlarını (değişiklik Analizi özelliğinde birden çok güncelleştirme) [bulmak Için Azure izleyici 'de uygulama değişikliği analizini kullanın](app/change-analysis.md) .

#### <a name="new-and-updated-articles-for-preview-of-workspace-based-applications"></a>Çalışma alanı tabanlı uygulamaların önizlemesi için yeni ve güncelleştirilmiş makaleler
- [Azure Izleyici Application Insights çalışma alanı tabanlı kaynak şeması](app/apm-tables.md)
- [Yeni bir Azure Izleyici Application Insights çalışma alanı tabanlı kaynak oluşturma](app/create-workspace-resource.md)
- [Azure Izleyici günlük sorgularındaki App () ifadesi](logs/app-expression.md)
- [Azure Izleyici 'de günlük sorgu kapsamı Log Analytics](logs/scope.md)
- [Azure Izleyici ile kaynaklar arasında sorgu](logs/cross-workspace-query.md)
- [Azure Izleyici günlük kayıtlarında standart özellikler](./logs/log-standard-columns.md)
- [Azure Izleyici günlüklerinin yapısı](./logs/data-platform-logs.md)





### <a name="containers"></a>Kapsayıcılar
- [Kapsayıcı öngörülerini etkinleştirme](containers/container-insights-onboard.md) -güvenlik duvarı yapılandırma tablosu güncelleştirildi.
- [Ölçümler Için kapsayıcı öngörülerini güncelleştirme](containers/container-insights-update-metrics.md) -ölçümleri toplamak için yönetilen kimlikleri kullanmaya yönelik güncelleştirme.
- [Kapsayıcı öngörüleri Için izleme maliyeti](containers/container-insights-cost.md) -yeni makale.
- [Container Insights canlı verilerini ayarlama (Önizleme)](containers/container-insights-livedata-setup.md) -yeni küme rolü bağlama desteği.

### <a name="insights"></a>Insights
- [Redsıs için Azure önbelleği Için Azure izleyici (Önizleme)](insights/redis-cache-insights-overview.md) -yeni makale.
- [Key Vault Için Azure izleyici ile Key Vault izleyin (Önizleme)](./insights/key-vault-insights-overview.md) -yeni makale.

### <a name="logs"></a>Günlükler
- PowerShell tarafından eklenen sorun giderme bölümü [ile Log Analytics yapılandırma & oluşturun](logs/powershell-workspace-configuration.md) .
- Azure portal eklenen sorun giderme bölümünde [bir Log Analytics çalışma alanı oluşturun](logs/quick-create-workspace.md) .
- Azure CLı ile eklenen sorun giderme bölümünü [kullanarak bir Log Analytics çalışma alanı oluşturun](logs/quick-create-workspace-cli.md) .
- [Azure Log Analytics çalışma alanını silme ve kurtarma](logs/delete-workspace.md) -silinmiş bir çalışma alanını kurtarmayla ilgili bilgiler güncelleştirildi.
- [Azure izleyici günlük sorguları](logs/functions.md) -kaldırılan işlevler diğer işlevleri içermeyen işlevler hakkında not.
- [Azure Izleyici günlüklerinin yapısı](./logs/data-platform-logs.md) -Application Insights tablo için açıklanan özellik açıklamaları.
- [Azure Izleyici günlüklerini Azure Logic Apps ve gelişmiş otomatikleştir ile](logs/logicapp-flow-connector.md) eklenen sınırlar bölümü ile kullanın.
- [Log Analytics çalışma alanı eklenen sorun giderme bölümü oluşturmak ve yapılandırmak Için PowerShell 'ı kullanın](logs/powershell-workspace-configuration.md) .


### <a name="metrics"></a>Ölçümler
- [Azure izleyici kaynak türüne göre desteklenen ölçümler](essentials/metrics-supported.md) -açıklanan Konuk ölçümleri ve ölçüm yönlendirmesi. 

### <a name="solutions"></a>Çözümler
- Desteklenen sürümlere Windows Server 2019 ' i eklenen [Azure izleyici ile Active Directory ortamınızı iyileştirin](insights/ad-assessment.md) .
- Desteklenen SQL Server sürümlerine eklenen [SQL Server ortamınızı Azure izleyici Ile iyileştirin](insights/sql-assessment.md) .


### <a name="virtual-machines"></a>Sanal makineler
- [VM Insights 'a genel bakış 'ı etkinleştirin](vm/vminsights-enable-overview.md) -Ubuntu Server 'ın desteklenen sürümlerine eklenir. Log Analytics çalışma alanı için desteklenen bölgeler eklendi.
- Kullanılabilir ölçümler için VM Öngörüler 'e eklenen sınırlamalar bölümü [ile performansı grafik olarak belirleme](vm/vminsights-performance.md) .

### <a name="visualizations"></a>Görsel öğeler
- [Azure izleyici çalışma kitapları ve Azure Resource Manager şablonları](visualize/workbooks-automate.md) -çalışma kitabı şablonu dağıtmak için Kaynak Yöneticisi Güncelleştirmesi eklendi.
- [Azure Izleyici çalışma kitapları grupları](./visualize/workbooks-groups.md) -yeni makale.
- [Azure Izleyici çalışma kitapları-JSON verilerini JSONPath Ile Dönüştür](visualize/workbooks-jsonpath.md) -yeni makale.


## <a name="april-2020"></a>Nisan 2020

### <a name="general"></a>Genel

- [Azure izleyici müşteri tarafından yönetilen](logs/customer-managed-keys.md) ve zaman uyumsuz işlemlerde anahtar eklendi bölümü
- [Azure izleyici 'de Log Analytics çalışma alanlarını yönetme](logs/manage-access.md) -özel Günlükler bölümlerinde güncelleştirildi.

### <a name="alerts"></a>Uyarılar

- [Azure izleyici uyarıları Için eylem kuralları](alerts/alerts-action-rules.md) -eklenen video.
- Azure tarafından eklenen videoda [uyarı ve bildirim Izlemeye genel bakış](alerts/alerts-overview.md) .

### <a name="application-insights"></a>Application Insights

- [Azure Application Insights 'de uygulama eşlemesi](app/app-map.md) -Java Aracısı için bulut rol adları yapılandırması eklendi.
- [Azure Application Insights .net ARACıSı API başvurusu](app/status-monitor-v2-api-reference.md) -birleştirilmiş API başvurusu.
- App Insights ve Log Analytics API 'Leri, eylem grubu Web kancaları ve Azure ABD kamu için [Application Insights ve Log Analytics tarafından GÜNCELLEŞTIRILMIŞ IP adresleri tarafından kullanılan IP adresleri](app/ip-addresses.md) .
- [Java uygulamalarını her yerde izleyin](app/java-standalone-config.md) -yeni makale.
- [Java uygulamalarını herhangi bir ortamda izleyin](app/java-in-process-agent.md) -yeni makale.
- [Tüm ortamlarda çalışan Java uygulamalarını izleme](app/java-standalone-arguments.md) -yeni makale.
- [Şirket içinde çalışan Java uygulamalarını izleme](app/java-on-premises.md) -yeni makale.
- [Visual Studio 'da Application Insights kaldırın](app/remove-application-insights.md) -yeni makale.
- [Azure Application Insights telemetri örnekleme](app/sampling.md) -Python 'da sabit hızlı örnekteki düzeltme.

### <a name="containers"></a>Kapsayıcılar

- [Azure Red Hat OpenShift v4. x 'ı Container Insights Ile yapılandırma](containers/container-insights-azure-redhat4-setup.md) -yeni makale.
- [ServiceNow eşitleme sorunlarını el ile çözme](alerts/itsmc-resync-servicenow.md) -yeni makale.
- [Azure ve Red Hat OpenShift v4 kümenizi izlemeyi durdurma](containers/container-insights-optout-openshift-v4.md) -yeni makale.
- [Azure Red Hat OpenShift v3 kümenizi izlemeyi durdurma](containers/container-insights-optout-openshift-v3.md) -yeni makale.
- [Karma Kubernetes kümenizi izlemeyi durdurma](containers/container-insights-optout-hybrid.md) -yeni makale.

### <a name="insights"></a>Insights

- [Azure anahtar kasalarını, Anahtar kasaları (Önizleme) Için Azure izleyici Ile izleyin](insights/key-vault-insights-overview.md) -yeni makale.

### <a name="logs"></a>Günlükler

- [Azure İzleyici hizmeti sınırları](service-limits.md) -Kullanıcı sorgu azaltma eklendi.
- [Azure Izleyici günlükleri için kullanımı ve maliyetleri yönetme](logs/manage-cost-storage.md) -günlük kümeleri için faturalama eklendi. Düğüm başına bir fiyatlandırma katmanına kadar olan müşterilere, GB başına veya kapasite Ayırma katmanına geçiş yapılıp yapılmayacağını belirleme olanağı tanımak için kusto sorgusu eklendi.

### <a name="metrics"></a>Ölçümler

- [Azure Ölçüm Gezgini](essentials/metrics-charts.md) ile eklenen toplama bölümünün gelişmiş özellikleri.

### <a name="workbooks"></a>Çalışma Kitapları

- [Azure Izleyici çalışma kitapları ve Azure Resource Manager şablonları](visualize/workbooks-automate.md) -çalışma kitabı şablonu dağıtmak için Kaynak Yöneticisi şablonu eklendi.

## <a name="march-2020"></a>Mart 2020

### <a name="general"></a>Genel

- [Azure izleyicisine genel bakış](overview.md) -Azure izleyici 'ye genel bakış videosu eklendi.
- [Azure izleyici müşteri tarafından yönetilen anahtar yapılandırması](logs/customer-managed-keys.md) -Genel güncelleştirmeler.
- [Azure izleyici veri başvurusu](/azure/azure-monitor/reference/) -yeni site.

### <a name="alerts"></a>Uyarılar

- [Azure izleyici 'de etkinlik günlüğü uyarıları oluşturma, görüntüleme ve yönetme](alerts/alerts-activity-log.md) -Kaynak Yöneticisi şablonunun ek açıklaması.
- [Ölçüm uyarılarının Azure Izleyici 'de nasıl çalıştığını anlayın.](alerts/alerts-metric-overview.md) -Kamu desteği için güncelleştirildi.
- [Azure izleyici uyarıları ve bildirimleri sorunlarını giderme](alerts/alerts-troubleshoot.md) -yeni makale.

### <a name="application-insights"></a>Application Insights

- PowerShell tarafından eklenen ARMClient örnekleri [Ile Azure Application Insights otomatikleştirin](app/powershell.md) .
- [Application Insights 'ten Telemetriyi sürekli dışa aktarma](app/export-telemetry.md) -dışarı aktarma yapısının ayrıntılarını içeren tablo ekleyin.
- Azure App Service eklenen Kaynak Yöneticisi Şablon örneğinde [.NET uygulamaları için Snapshot Debugger etkinleştirin](app/snapshot-debugger-appservice.md) .
- [Azure Application Insights için kullanımı ve maliyetleri,](app/pricing.md) veri Cap uyarısıyla eklenen bilgileri yönetin.
- [Azure izleyici (Önizleme) Ile Python uygulamalarını izleme](app/opencensus-python.md) -standart ölçümler eklendi.
- [JavaScript uygulamaları Için kaynak eşleme desteği-Azure izleyici Application Insights](app/source-map-support.md) -yeni makale.

### <a name="containers"></a>Kapsayıcılar

- [Azure IZLEYICI SSS](faq.md) -kapsayıcı öngörüleri için güncelleştirme.
- [Kapsayıcı öngörüleri Ile GPU Izlemesini yapılandırma](containers/container-insights-gpu-monitoring.md) -yeni makale.

### <a name="insights"></a>Insights

- [Azure 'Da Office 365 yönetim çözümü](insights/solution-office-365.md) -kullanımdan kaldırma tarihi güncelleştirildi.

### <a name="logs"></a>Günlükler

- [Azure izleyici 'de](logs/query-optimization.md) , XML ve JSON AYRıŞTıRMA için CPU koşulunda günlük sorgularını iyileştirin.
- [Azure Log Analytics çalışma alanını silme ve kurtarma](logs/delete-workspace.md) -sorun giderme eklendi.
- [Azure Izleyici günlüklerini Azure Logic Apps ve Power otomatikleştirmede kullanın](logs/logicapp-flow-connector.md) -yeni Azure izleyici Bağlayıcısı için güncelleştirildi.

### <a name="metrics"></a>Ölçümler

- [Disk ölçümleri Azure Portal](essentials/portal-disk-metrics-deprecation.md) -yeni makalede kullanımdan kaldırıldı.
- [Öğretici-Azure izleyici 'de bir ölçüm grafiği oluşturma](essentials/tutorial-metrics-explorer.md) -video eklendi.

### <a name="platform-logs"></a>Platform günlükleri

- Azure [izleyici 'de Azure etkinlik günlüğü toplayın ve çözümleyin](./essentials/activity-log.md) -tanılama ayarlarıyla etkinlik günlüğünü toplamayı daha iyi açıklamak için yeniden yazın.

### <a name="virtual-machines"></a>Sanal makineler

- Azure [izleyici Ile Azure sanal makinelerini izleme](vm/monitor-vm-azure.md) -yeni makale.
- [Hızlı başlangıç: Azure izleyici Ile Azure sanal makinelerini izleme](vm/quick-monitor-azure-vm.md) -VM öngörüleri eklemek için güncelleştirildi.
- [VM öngörülerinin uyarıları](vm/vminsights-alerts.md) -yeni makale.
- [VM öngörülerini etkinleştirin](vm/vminsights-enable-overview.md) -aracı indirme bağlantıları güncelleştirildi.

VM öngörülerinin genel kullanılabilirliği için genel güncelleştirmeler

- [VM öngörüleri nedir?](vm/vminsights-overview.md)
- [VM öngörüleri (GA) sık sorulan sorular](vm/vminsights-ga-release-faq.md) 
- [Azure Ilkesini kullanarak VM öngörülerini etkinleştirme](./vm/vminsights-enable-policy.md) 
- [VM öngörüleri ile performansı grafikle](vm/vminsights-performance.md)
- [VM öngörülerine ait günlükleri sorgulama](vm/vminsights-log-search.md)
- [VM öngörüleri ile uygulama bağımlılıklarını görüntüleme](vm/vminsights-maps.md) 

### <a name="visualizations"></a>Görsel öğeler

- [Azure izleyici 'den verileri görselleştirme](visualizations.md) -Görünüm Tasarımcısı 'nın planlanmış kullanım dışı bırakılmasıyla güncelleştirildi.

## <a name="february-2020"></a>Şubat 2020

### <a name="agents"></a>Aracılar

Tanılama uzantısı içeriğinin yeniden yazma parçası olarak birden çok güncelleştirme.

- Her bir aracının benzersiz özelliklerini daha iyi netleştirmek için [Azure izleme aracıları](agents/agents-overview.md) -yeniden yapılandırılmış tablolara genel bakış.
- [Azure tanılama uzantıya genel bakış](agents/diagnostics-extension-overview.md) -yeniden yazmayı doldurun.
- [Azure izleyici 'de olaylar IÇIN IIS ve tablo depolaması için BLOB depolama kullanın](agents/diagnostics-extension-logs.md) -güncelleştirme ve açıklık için genel yeniden yazma.
- [Windows Azure tanılama uzantısı 'nı (WAD) yükleyip yapılandırın](agents/diagnostics-extension-windows-install.md) -yeni makale. 
- [Windows Tanılama uzantısı şeması](agents/diagnostics-extension-schema-windows.md) -yeniden düzenlendi.
- [Windows Azure tanılama uzantısı 'Ndan azure Event Hubs 'a veri gönderin](agents/diagnostics-extension-stream-event-hubs.md) -tamamen yeniden yazıldı ve güncelleştirilir.
- [Azure depolama 'da tanılama verilerini depolama ve görüntüleme](../cloud-services/diagnostics-extension-to-storage.md) -tamamen yeniden yazıldı ve güncelleştirildi.
- [Windows için sanal makine uzantısı Log Analytics](../virtual-machines/extensions/oms-windows.md) Log Analytics aracısıyla daha iyi açıklığa kavuştura ilişkisi.
- [Linux Için Azure izleyici sanal makine uzantısı](../virtual-machines/extensions/oms-linux.md) -Log Analytics aracısıyla daha iyi bir ilişki ilişkisi.

### <a name="application-insights"></a>Application Insights

- [Azure Application Insights bağlantı dizeleri](app/sdk-connection-string.md) -yeni makale.

### <a name="insights-and-solutions"></a>Öngörüler ve çözümler

#### <a name="container-insights"></a>Kapsayıcı öngörüleri

- [Azure Kubernetes hizmeti 'nin,](../aks/azure-ad-integration-cli.md) kapsayıcı öngörülerini desteklemek Için KUBERNETES RBAC özellikli kümeyi desteklemesi için bir istemci uygulaması oluşturmak üzere bir istemci uygulaması oluşturmaya yönelik olarak Azure Active Directory tümleştirin.

#### <a name="vm-insights"></a>VM öngörüleri

- [VM öngörüleri (GA) sık sorulan sorular](vm/vminsights-ga-release-faq.md) -performans verilerinin nasıl depolandığını değiştirin.

#### <a name="office-365"></a>Office 365

- [Azure 'Da Office 365 yönetim çözümü](insights/solution-office-365.md) -kullanımdan kaldırma tarihi güncelleştirildi.


### <a name="logs"></a>Günlükler

- [Azure izleyici 'de günlük sorgularını iyileştirme](logs/query-optimization.md) -yeni makale.
- [Azure Izleyici günlükleri için kullanımı ve maliyetleri yönetme](logs/manage-cost-storage.md) -kullanımınızın anlaşılmasına yardımcı olmak için geliştirilmiş örnek sorgular.

### <a name="metrics"></a>Ölçümler

- [Azure izleyici platformu ölçümleri, Tanılama ayarları aracılığıyla dışarı aktarılabilir](essentials/metrics-supported-export-diagnostic-settings.md) -null ve sıfır değerleri için davranış değişikliğine eklenen bölüm.

### <a name="visualizations"></a>Görsel öğeler

Görünüm Tasarımcısı 'na çalışma kitapları dönüştürme kılavuzuna yönelik birden çok yeni makale.

- [Azure izleyici Görünüm Tasarımcısı çalışma kitapları geçiş kılavuzu](visualize/view-designer-conversion-overview.md) -yeni makale.
- [Azure izleyici Görünüm Tasarımcısı çalışma kitapları dönüştürme seçenekleri](visualize/view-designer-conversion-options.md) -yeni makale.
- [Azure izleyici Görünüm Tasarımcısı çalışma kitapları kutucuk dönüştürmeleri](visualize/view-designer-conversion-tiles.md) -yeni makale.
- [Azure izleyici Görünüm Tasarımcısı çalışma kitapları dönüştürme özeti ve erişim](visualize/view-designer-conversion-access.md) -yeni makale.
- [Azure izleyici Görünüm Tasarımcısı çalışma kitaplarını dönüştürme ortak görevler](visualize/view-designer-conversion-tasks.md) -yeni makale.
- [Azure izleyici Görünüm Tasarımcısı çalışma kitapları dönüştürme örnekleri](visualize/view-designer-conversion-examples.md) -yeni makale.

## <a name="january-2020"></a>Ocak 2020

### <a name="general"></a>Genel

- [Azure Izleyici ile neler izlenir?](monitor-reference.md) -Yeni makale.

### <a name="agents"></a>Aracılar

- [Azure Log Analytics Agent ile günlük verilerini toplayın](agents/log-analytics-agent.md) -ağ güvenlik duvarı gereksinimleri tablosu güncelleştirildi.

### <a name="alerts"></a>Uyarılar

- Artık gerekli olmayan v2 işlevleri için kaldırılan [Azure Portal ayarında eylem grupları oluşturun ve yönetin](alerts/action-groups.md) .
- *Ignoredatabefore* parametresi için [Kaynak Yöneticisi şablonla eklenen bir örnek içeren bir ölçüm uyarısı oluşturun](alerts/alerts-metric-create-templates.md) .  Çok ölçütlü kurallarla ilgili kısıtlamalar eklendi.
- [Log Analytics uyarı REST API kullanma](alerts/api-alerts.md) -JSON örneği düzeltildi.

### <a name="application-insights"></a>Application Insights

- [Application Insights ve Log Analytics tarafından kullanılan IP adresleri](app/ip-addresses.md) -Azure ağ güvenlik grupları kullanılarak trafiğe izin vermek için bir gelen bağlantı noktası kuralı ekleme ile kullanılabilirlik testi bölümü güncelleştirildi.
- [Azure Application Insights Profiler ile ilgili sorunları giderin](app/profiler-troubleshooting.md) -güncelleştirilmiş genel sorun giderme.
- [Azure Application Insights telemetri örnekleme](app/sampling.md) -müşteri geri bildirimlerine göre okunabilirliğini geliştirmek için güncelleştirildi ve yeniden yapılandırılır.

### <a name="data-security"></a>Veri güvenliği

- [Azure izleyici müşteri tarafından yönetilen anahtar yapılandırması](logs/customer-managed-keys.md) -yeni makale.

### <a name="insights-and-solutions"></a>Öngörüler ve çözümler

#### <a name="container-insights"></a>Kapsayıcı öngörüleri

- Kapsayıcı içgörüler [Aracısı veri toplamayı yapılandırma](containers/container-insights-agent-config.md) -Azure Red Hat OpenShift üzerinde aracı yükseltme ayrıntıları eklendi ve aracıyı yükseltme yöntemlerini ayırt etmek için ek bilgiler eklendi.
- [Kapsayıcı öngörüleri için performans uyarıları oluşturun](./containers/container-insights-log-alerts.md) -çalışma alanı bağlamı uyarılarını kullanarak çalışma alanında depolanan performans verilerinde uyarı oluşturmaya yönelik bilgiler ve güncelleştirilmiş adımlar.
- [Container Insights Ile Kubernetes izleme](containers/container-insights-analyze.md) -hem genel bakış makalesi hem de Windows Kubernetes kümelerinin desteğiyle ilgili çözümleme makalesi güncelleştirildi.
- Azure [Red Hat OpenShift kümelerini kapsayıcı öngörüleri Ile yapılandırın](containers/container-insights-azure-redhat-setup.md) -Azure Red Hat OpenShift üzerinde aracı yükseltme ayrıntıları eklendi ve aracı yükseltme yöntemlerini ayırmak için ek bilgiler eklendi.
- [Karma Kubernetes kümelerini kapsayıcı](containers/container-insights-hybrid-setup.md) Içgörüler ile yapılandırın-, Kubelet 'In Cadvizörü ile 10250 güvenli bağlantı noktası için ek desteği yansıtacak şekilde güncelleştirildi.
- [Container Insights Aracısı 'nı yönetme](containers/container-insights-manage-agent.md) -Azure Red Hat OpenShift ile ölçüm ve yapılandırma ile ilgili güncelleştirilmiş ayrıntılar, diğer Kubernetes kümeleriyle karşılaştırılır.
- [Kapsayıcı öngörülerini yapılandırma](containers/container-insights-prometheus-integration.md) -Azure Red Hat OpenShift ile ölçüm ve yapılandırma ile ilgili güncelleştirilmiş ayrıntılar, diğer Kubernetes kümelerine kıyasla güncelleştirildi.
- Ölçüm [öngörülerini güncelleştirme](containers/container-insights-update-metrics.md) , Azure Red Hat OpenShift ile ölçüm koruması ile ilgili ölçüm ve yapılandırma ile ilgili, diğer Kubernetes kümelerinin türlerine kıyasla güncelleştirilmiş ayrıntılar.

#### <a name="vm-insights"></a>VM öngörüleri

- [VM öngörüleri (GA) sık sorulan sorular](vm/vminsights-ga-release-faq.md) -çalışma alanı ve aracıları yeni sürüme yükseltme hakkında bilgiler eklendi.

#### <a name="office-365"></a>Office 365

- Azure ['da office 365 yönetim çözümü](insights/solution-office-365.md) -Azure 'da Office 365 çözümüne geçiş konusunda ayrıntılar ve SSS eklendi. Ekleme bölümü kaldırıldı.

### <a name="logs"></a>Günlükler

- [Azure izleyici 'de Log Analytics çalışma alanlarını yönetme](logs/manage-access.md) -eylemlere yönelik güncelleştirmeler.
- [Azure Izleyici günlükleri için kullanımı ve maliyetleri yönetme](logs/manage-cost-storage.md) -fiyatlandırma modeli bölümünde veri hacmi hesaplamasına açıklama eklendi.
- Yeni fiyatlandırma katmanlarına sahip [Log Analytics çalışma alanı-güncelleştirilmiş şablon oluşturmak ve yapılandırmak için Azure Resource Manager şablonlarını kullanın](./logs/resource-manager-workspace.md) .

### <a name="platform-logs"></a>Platform günlükleri

- [Tanılama ayarlarıyla Azure etkinlik günlüğü toplama-Azure izleyici](./essentials/activity-log.md) -değiştirilen özelliklerle ilgili ek bilgiler.
- [Azure etkinlik günlüğünü dışarı aktarma](./essentials/activity-log.md#legacy-collection-methods) -UI değişiklikleri için güncelleştirildi. 

## <a name="december-2019"></a>Aralık 2019

### <a name="agents"></a>Aracılar

- [Linux bilgisayarları Azure izleyici 'ye bağlama](agents/agent-linux.md) -yeni makale.

### <a name="alerts"></a>Uyarılar

- Özel ölçüm için [Kaynak Yöneticisi şablonla eklenen bir örnek içeren bir ölçüm uyarısı oluşturun](alerts/alerts-metric-create-templates.md) .
- [Azure izleyici 'de dinamik eşiklerle uyarı oluşturma](alerts/alerts-dynamic-thresholds.md) -dinamik eşik grafiklerini yorumlama bölümünde bölüm eklendi.
- Azure tarafından güncelleştirilmiş kaynak grafiği sorgusunda [uyarı ve bildirim Izlemeye genel bakış](alerts/alerts-overview.md) .
- [Azure izleyici 'de ölçüm uyarıları Için desteklenen kaynaklar](alerts/alerts-metric-near-real-time.md) -ölçüm ve boyutların güncelleştirilmesi desteklenir.
- [Eski Log Analytics Uyarıları API 'sinden yeni Azure uyarıları API 'si](alerts/alerts-log-api-switch.md) ile değiştirilen uyarı adı üzerine geçiş yapın.
- [Ölçüm uyarılarının Azure Izleyici 'de nasıl çalıştığını anlayın.](alerts/alerts-metric-overview.md) -İzleme için desteklenen kaynak türleri ölçekli olarak eklendi.

### <a name="application-insights"></a>Application Insights

- [Çalışan hizmeti uygulamaları için Application Insights (http olmayan uygulamalar)](app/worker-service.md) -C# koduna varsayılan günlük kaydı düzeyi eklendi. Paket başvurusu sürümü güncelleştirildi.
- [ApplicationInsights.config başvurusu-Azure](app/configuration-with-applicationinsights-config.md) -güncelleştirilmiş örnek kod.
- [Azure Application Insights 'Yi PowerShell Ile otomatikleştirin](app/powershell.md) -Kaynak Yöneticisi şablona güncelleştirin.
- [Yeni bir Azure Application Insights kaynağı oluşturma](app/create-new-resource.md) -genel benzersiz ada Note eklendi.
- [Canlı ölçüm akışı Ile tanılama-Azure Application Insights](app/live-stream.md) -GÜNCELLEŞTIRILMIŞ ASP.NET Core SDK sürümü gereksinimi.
- Application Insights-güncelleştirilmiş kategori ve tablodaki [olay sayaçları](app/eventcounters.md) customölçümler.
- Java aracı günlüğü eşiği için [Azure Application Insights tarafından eklenen Java izleme günlüklerini keşfet](app/java-trace-logs.md) .
- Canlı Ölçüm Akışı için [Application Insights ve Log Analytics tarafından güncellenen IP adresleri tarafından kullanılan IP adresleri](app/ip-addresses.md) .
- [Azure App Services performansını izleme](app/azure-web-apps.md) ASP.NET Core 3,0 için destek eklendi. 
- [Azure izleyici (Önizleme) Ile Python uygulamalarını izleme](app/opencensus-python.md) -Azure 'A OpenCensus Python Şeması eşleme açıklaması eklendi. Şemayı izle
- [Azure Application Insights Için sürüm notları](app/release-notes.md) -eski sürümler için not eklendi.
- [Azure Application Insights telemetri kanalları](app/telemetry-channels.md) -kayıp bağlantının genişletilmiş süresi boyunca atılan veriler için güncelleştirilmiş süre.
- [Azure Application Insights telemetri örnekleme](app/sampling.md) -özel telemetryınitializer için kod parçacığı düzeltildi.
- [Java Web projesinde Application Insights sorunlarını giderme](app/java-troubleshoot.md) -JDK 9 ' da bağımlılık toplamayı desteklememe hakkında bir bildirimde sorun giderme.

### <a name="insights-and-solutions"></a>Öngörüler ve çözümler

- [Kapsayıcı öngörüleri hakkında sık sorulan sorular](./faq.md) -görüntü ve ad alanlarına soru eklendi.
- [Azure izleyici 'de Azure SQL Analytics çözümü](insights/azure-sql.md) -güncelleştirilmiş veritabanı yönetilen örnek desteğini bekler.
- Kapsayıcı içgörüler [Aracısı veri toplamayı yapılandır](containers/container-insights-agent-config.md) -enrich_container_logs ayarı eklendi.
- [Kapsayıcı öngörüleri Ile karma Kubernetes kümelerini yapılandırma](containers/container-insights-hybrid-setup.md) sorun giderme bölümü.
- [Azure izleyici ile Active Directory çoğaltma durumunu izleme](insights/ad-replication-status.md) .NET Framework önkoşul güncelleştirildi.
- [Azure 'da ağ performansı İzleyicisi çözüm,](insights/network-performance-monitor.md) desteklenen bölgelerde eklendi.
- [Active Directory ortamınızı Azure izleyici Ile iyileştirin](insights/ad-assessment.md) .NET Framework önkoşul güncelleştirildi.
- [SQL Server ortamınızı Azure izleyici Ile iyileştirin](insights/sql-assessment.md) .NET Framework önkoşul güncelleştirildi.
- [System Center Operations Manager ortamınızı Azure Log Analytics iyileştirin](insights/scom-assessment.md) .NET Framework önkoşul güncelleştirildi.
- [Azure Log Analytics BT hizmet yönetimi Bağlayıcısı desteklenen bağlantılar](alerts/itsmc-connections.md) -önkoşul istemci kimliği ve istemci gizli anahtarı 'Na New York eklendi.

### <a name="logs"></a>Günlükler

- [Azure Log Analytics çalışma alanı eklenen PowerShell metodunu silme ve kurtarma](logs/delete-workspace.md) .
- [Azure Izleyici günlüklerinizi tasarlama](logs/design-logs-deployment.md) bir çalışma alanı için alım oranını artırdık.

### <a name="metrics"></a>Ölçümler

- [Azure izleyici platformu ölçümleri, Tanılama ayarları aracılığıyla dışarı aktarılabilir](essentials/metrics-supported-export-diagnostic-settings.md) -yeni makale.

### <a name="platform-logs"></a>Platform günlükleri

Tanılama ayarları kullanılarak etkinlik günlüğünü yapılandırmaya yönelik yeni özelliğe dayalı olarak, platform günlükleri için içeriğin yeniden yapılandırmasının bir parçası olarak güncelleştirilmiş birden çok makale güncelleştirildi.

- [Azure Kaynak günlüklerini depolama hesabına arşivleme](./essentials/resource-logs.md#send-to-azure-storage)
- [Azure etkinlik günlüğü olay şeması](essentials/activity-log-schema.md)
- [Azure Izleyici hizmeti sınırları](service-limits.md)
- [Log Analytics çalışma alanında Azure etkinlik günlüklerini toplayın ve çözümleyin](./essentials/activity-log.md)
- [Tanılama ayarları (Önizleme) ile Azure etkinlik günlüğü toplama-Azure Izleyici](./essentials/activity-log.md)
- [Azure etkinlik günlüklerini Azure kiracılarının tamamında bir Log Analytics çalışma alanında toplayın](./essentials/activity-log.md)
- [Log Analytics çalışma alanında Azure Kaynak günlüklerini toplayın](./essentials/resource-logs.md#send-to-log-analytics-workspace)
- [Kaynak Yöneticisi şablonu kullanarak Azure 'da tanılama ayarı oluşturma](./essentials/resource-manager-diagnostic-settings.md)
- [Azure 'da günlüklerin ve ölçümlerin toplanması için tanılama ayarı oluştur](essentials/diagnostic-settings.md)
- [Azure etkinlik günlüğünü dışarı aktarma](./essentials/activity-log.md#legacy-collection-methods)
- [Azure platform günlüklerine genel bakış](essentials/platform-logs-overview.md)
- [Azure izleme verilerini Olay Hub 'ına akış](essentials/stream-monitoring-data-event-hubs.md)
- [Azure platformu günlüklerini bir olay hub 'ına akış](./essentials/resource-logs.md#send-to-azure-event-hubs)

### <a name="quickstarts-and-tutorials"></a>Hızlı başlangıçlar ve öğreticiler

- [Azure izleyici 'de bir ölçüm grafiği oluşturun](essentials/tutorial-metrics-explorer.md) -yeni makale.
- [Azure kaynağından kaynak günlüklerini toplayın ve Azure izleyici ile çözümleyin](essentials/tutorial-resource-logs.md) -yeni makale.
- Azure [izleyici Ile Azure kaynağını izleme](essentials/quick-monitor-azure-resource.md) -yeni makale.
   
## <a name="next-steps"></a>Sonraki adımlar

- Azure Izleyici belgelerine katkıda bulunmak istiyorsanız [docs katılımcısı Kılavuzu](/contribute/)' na bakın.