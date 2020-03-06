---
title: Azure Kamu İzleme ve Yönetim | Microsoft Docs
description: Bu, Azure Kamu için uygulama geliştirmeye yönelik özellikler ve yönergeler için bir karşılaştırma sağlar.
services: azure-government
cloud: gov
author: gsacavdm
ms.assetid: 4b7720c1-699e-432b-9246-6e49fb77f497
ms.service: azure-government
ms.topic: article
ms.workload: azure-government
ms.date: 12/11/2019
ms.author: gsacavdm
ms.openlocfilehash: b6f395964c286aca1bc8a1c190edeea00ba6e15c
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78362266"
---
# <a name="azure-government-monitoring--management"></a>Azure Kamu İzleme ve Yönetim
Bu makalede, Azure Kamu ortamı için izleme ve Yönetim Hizmetleri çeşitlemeleri ve konuları özetlenmektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="advisor"></a>Advisor
Danışman, Azure Kamu 'da genel kullanıma sunulmuştur.

Daha fazla bilgi için bkz. [danışman ortak belgeleri](../advisor/advisor-overview.md).

### <a name="variations"></a>Farklılıkları
Aşağıdaki danışman önerileri Azure Kamu 'da Şu anda kullanılamıyor:

* Yüksek Kullanılabilirlik
  * VPN ağ geçidinizi bağlantı esnekliği için etkin-etkin olarak yapılandırma
  * Azure sorunları sizi etkiliyorsa bildirim almak için Azure hizmet durumu uyarıları oluşturun
  * Dayanıklılık için Traffic Manager uç noktalarını yapılandırma
  * Azure depolama hesabınız için geçici silme kullanın
* Performans
  * Performansı ve güvenilirliği App Service geliştirme
  * Traffic Manager profilinizde etkin hale geçen DNS süresini, sağlıklı uç noktalara daha hızlı yük devretmek için azaltın
  * SQL veri ambarı performansını iyileştirme
  * Premium Depolamayı kullanma
  * Depolama hesabınızı Azure Resource Manager geçirin
* Maliyet
  * Kullandıkça Öde maliyetlerinden tasarruf etmek için ayrılmış sanal makine örnekleri satın alın
  * Sağlaması kaldırılan ExpressRoute devreleri ortadan kaldırın
  * Boşta olan sanal ağ geçitlerini silin veya yeniden yapılandırın

Az kullanılan sanal makineleri doğru boyuta getirmek veya kapatmanız önerilir, Azure Kamu 'da aşağıdaki gibidir:

Danışman, sanal makine kullanımınızı 7 gün boyunca izler ve düşük kullanım sanal makinelerini belirler. CPU kullanımı %5 veya daha az olursa ve ağ kullanımı %2 ' den küçük veya geçerli iş yükünün daha küçük bir sanal makine boyutuyla ele alınabiliyorsa, sanal makineler düşük kullanım olarak değerlendirilir. Aşırı kullanılan sanal makineleri tanımlamaya daha Agresif olmak istiyorsanız, CPU kullanım kuralını abonelik başına temelinde ayarlayabilirsiniz.

## <a name="automation"></a>Otomasyon
Otomasyon, Azure Kamu 'da genel kullanıma sunulmuştur.

Daha fazla bilgi için bkz. [Otomasyon ortak belgeleri](../automation/automation-intro.md).

## <a name="azure-migrate"></a>Azure Geçişi

Azure geçişi, Azure Kamu 'da genel kullanıma sunulmuştur.

Daha fazla bilgi için bkz. [Azure geçiş belgeleri](../migrate/migrate-overview.md).

### <a name="variations"></a>Farklılıkları
Aşağıdaki Azure geçiş özellikleri şu anda Azure Kamu 'da kullanılamaz:

* Azure geçişi, Azure Kamu 'da Şu anda kullanılamayan bağımlılık görselleştirmesi için Hizmet Eşlemesi bağımlı olduğundan, bağımlılık görselleştirme işlevselliği Azure Kamu 'da kullanılamaz.
* Azure Kamu için yalnızca hedef bölgeler olarak ve Azure Kamu tekliflerini kullanarak değerlendirme oluşturabilirsiniz.

## <a name="backup"></a>Backup
Yedekleme, Azure Kamu 'da genel kullanıma sunulmuştur.

Daha fazla bilgi için bkz. [Azure Kamu yedekleme](documentation-government-services-backup.md).

## <a name="policy"></a>İlke
İlke, Azure Kamu 'da genel kullanıma sunulmuştur.

Daha fazla bilgi için bkz. [Azure İlkesi](../governance/policy/overview.md).

## <a name="site-recovery"></a>Site Recovery
Azure Site Recovery, Azure Kamu 'da genel kullanıma sunulmuştur.

Daha fazla bilgi için bkz. [Ticari belgeler Site Recovery](../site-recovery/site-recovery-overview.md).

### <a name="variations"></a>Farklılıkları
Aşağıdaki Site Recovery özellikleri Azure Kamu 'da Şu anda kullanılamıyor:
* E-posta ile bildirim

| Site Recovery | Klasik | Resource Manager |
| --- | --- | --- |
| VMware/fiziksel  | GA | GA |
| Hyper-V | GA | GA |
| Siteden siteye | GA | GA |

Site Recovery için aşağıdaki URL 'Ler Azure Kamu 'da farklıdır:

| Azure genel | Azure Kamu | Notlar |
| --- | --- | --- |
| \*.hypervrecoverymanager.windowsazure.com | \*. hypervrecoverymanager.windowsazure.us | Site Recovery hizmetine erişim |
| \*.backup.windowsazure.com  | \*. backup.windowsazure.us | Koruma hizmetine erişim |
| \*.blob.core.windows.net | \*. blob.core.usgovcloudapi.net | VM anlık görüntülerini depolamak için |
| https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi | https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi | MySQL 'i indirmek için |

## <a name="monitor"></a>İzleme
Azure Izleyici, Azure Kamu 'da genel kullanıma sunulmuştur.

Daha fazla bilgi için bkz. [ticari belgeleri izleme](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview).

### <a name="variations"></a>Farklılıkları
Aşağıdaki bölümlerde Azure Kamu 'da Azure Izleyici özelliklerinin ayrıntıları ve geçici çözümleri listelenmiştir:

#### <a name="action-groups"></a>Eylem Grupları
İşlem grupları, Azure Kamu 'da ticari Azure 'dan farklı bir farklılık olmadan genel kullanıma sunulmuştur.   

#### <a name="activity-log-alerts"></a>Etkinlik Günlüğü Uyarıları
Etkinlik günlüğü uyarıları, Azure Kamu 'da ticari Azure 'dan farklı bir farklılık olmadan genel kullanıma sunulmuştur.

#### <a name="alerts-experience"></a>Uyarı deneyimi
Birleşik uyarılar kullanıcı arabirimi deneyimi, Azure Kamu 'daki ölçüm ve günlük uyarıları için kullanılabilir.

#### <a name="autoscale"></a>Otomatik Ölçeklendirme
Otomatik ölçeklendirme, Azure Kamu 'da genel kullanıma sunulmuştur.

Ayarları belirtmek için PowerShell/ARM/REST çağrılarını kullanıyorsanız, otomatik ölçeklendirmeyi "konum" olarak "USGov Virginia" veya "USGov Iowa" olarak ayarlayın. Otomatik ölçeklendirme tarafından hedeflenen kaynak herhangi bir bölgede bulunabilir. Ayarın bir örneği aşağıda verilmiştir:

```powershell
$rule1 = New-AzAutoscaleRule -MetricName "Requests" -MetricResourceId "/subscriptions/S1/resourceGroups/RG1/providers/Microsoft.Web/sites/WebSite1" -Operator GreaterThan -MetricStatistic Average -Threshold 10 -TimeGrain 00:01:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue "1"
$rule2 = New-AzAutoscaleRule -MetricName "Requests" -MetricResourceId "/subscriptions/S1/resourceGroups/RG1/providers/Microsoft.Web/sites/WebSite1" -Operator GreaterThan -MetricStatistic Average -Threshold 10 -TimeGrain 00:01:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue "2"
$profile1 = New-AzAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1, $rule2 -Name "MyProfile"
$webhook_scale = New-AzAutoscaleWebhook -ServiceUri https://example.com?mytoken=mytokenvalue
$notification1= New-AzAutoscaleNotification -CustomEmails myname@company.com -SendEmailToSubscriptionAdministrator -SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
Add-AzAutoscaleSetting -Location "USGov Virginia" -Name "MyScaleVMSSSetting" -ResourceGroup sdubeys-usgv -TargetResourceId /subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Web/serverFarms/ServerFarm1 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Kaynaklarınızın otomatik ölçeklendirmeyi uygulamayı ilgileniyorsanız, ayarları belirtmek için PowerShell/ARM/REST çağrılarını kullanın.

PowerShell kullanma hakkında daha fazla bilgi için bkz. [ortak belgeler](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples#create-and-manage-autoscale-settings).

#### <a name="metrics"></a>Ölçümler
Ölçümler, Azure Kamu 'da genel kullanıma sunulmuştur. Ancak, çok boyutlu ölçümler yalnızca REST API aracılığıyla desteklenir. [Çok boyutlu ölçümleri gösterme](../azure-monitor/platform/metrics-charts.md) özelliği, Azure Kamu Portalı ' nda önizleme aşamasındadır.

#### <a name="metric-alerts"></a>Ölçüm Uyarıları
Ölçüm uyarılarının ilk üretimi, hem Azure Kamu hem de ticari Azure 'da genel kullanıma sunulmuştur. İlk nesil *Uyarılar (klasik)* olarak adlandırılır. Ölçüm uyarılarının ikinci nesli ( [birleşik uyarılar deneyimi](../azure-monitor/platform/alerts-overview.md)olarak da anılır), [genel buluta kıyasla](../azure-monitor/platform/alerts-metric-near-real-time.md)daha düşük bir kaynak sağlayıcıları kümesi de mevcuttur. Zamana göre daha fazla eklenecektir. 

İkinci nesil uyarılar deneyiminde Şu anda desteklenen kaynaklar şunlardır:
- Microsoft.ApiManagement/service
- Microsoft. Cache/redsıs
- Microsoft.Compute/virtualMachines
- Microsoft. Dbformyısql/sunucuları
- Microsoft. DBforPostgreSQL/sunucuları
- Microsoft. Dbformarıdb/sunucular
- Microsoft.Devices/ıothubs
- Microsoft. EventGrid/Domains
- Microsoft. EventGrid/konuları
- Microsoft. EventHub/kümeler
- Microsoft.EventHub/namespaces
- Microsoft. Insights/bileşenler
- Microsoft. Network/dnsZones
- Microsoft.Network/loadBalancers
- Microsoft. Network/Natgateway 'ler
- Microsoft. Network/privateEndpoints
- Microsoft. Network/privateLinkServices
- Microsoft.Network/trafficManagerProfiles
- Microsoft. Operationalınsights/çalışma alanları
- Microsoft. Powerbiadanmış/kapasiteler
- Microsoft. Relay/Namespace
- Microsoft.ServiceBus/namespaces
- Microsoft. SQL/ManagedInstances
- Microsoft.Sql/servers/databases
- Microsoft.Sql/servers/elasticPools
- Microsoft.Storage/storageAccounts
- Microsoft.Storage/storageAccounts/blobServices
- Microsoft. Storage/storageAccounts/fileServices
- Microsoft.Storage/storageAccounts/queueServices
- Microsoft.Storage/storageAccounts/tableServices
- Microsoft. Web/hostingEnvironments/multiRolePools
- Microsoft. Web/hostingEnvironments/workerPools
- Microsoft.Web/serverfarms
- Microsoft.Web/sites
- Microsoft.Web/sites/slots

Henüz ikinci sayıda uyarının oluşturulmasında kullanılamayan kaynaklar için [Klasik uyarıları](../azure-monitor/platform/alerts-classic.overview.md) kullanmaya devam edebilirsiniz. 

Ölçüm uyarıları oluşturmak için PowerShell/ARM/REST çağrılarını kullanırken, ölçüm uyarısının "konum" değerini "USGov Virginia" veya "USGov Iowa" olarak ayarlamanız gerekir. Ayarın bir örneği aşağıda verilmiştir:

```powershell
$actionEmail = New-AzAlertRuleEmail -CustomEmail myname@company.com
$actionWebhook = New-AzAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
Add-AzMetricAlertRule -Name vmcpu_gt_1 -Location "USGov Virginia" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

PowerShell kullanma hakkında daha fazla bilgi için bkz. [ortak belgeler](../azure-monitor/platform/powershell-quickstart-samples.md).

## <a name="application-insights"></a>Application Insights

> [!NOTE]
> Azure Uygulama Hizmetleri için codeless Aracısı/uzantısı tabanlı izleme **Şu anda desteklenmiyor**. Bu işlevsellik kullanılabilir hale geldiğinde, bu makale güncelleştirilir.

Bu bölümde, Azure Kamu 'da Application Insights kullanmak için gereken ek yapılandırma açıklanmaktadır. Azure Izleyici hakkında daha fazla bilgi edinmek ve [tüm belgeleri](https://docs.microsoft.com/azure/azure-monitor/overview)kullanıma Application Insights.

### <a name="enable-application-insights-for-aspnet--aspnet-core-with-visual-studio"></a>Visual Studio ile ASP.NET & ASP.NET Core için Application Insights etkinleştirme

Şu anda Azure Kamu müşterileri için, Visual Studio 'da geleneksel **uygulama öngörüleri telemetrisi Ekle** düğmesi aracılığıyla Application Insights etkinleştirmenin tek yolu, küçük bir el ile geçici çözüm gerektirir. İlişkili sorunu yaşayan müşteriler _, "Bu hesapla Ilişkilendirilmiş Azure aboneliği_ yok _" veya "seçili abonelik_ , `microsoft.insights` kaynak sağlayıcısı abonelik için kayıtlı durumuna sahip olsa bile Application Insights desteklemez. Bu sorunu azaltmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Visual Studio 'Yu [Azure Kamu bulutunu hedefleyecek](https://docs.microsoft.com/azure/azure-government/documentation-government-get-started-connect-with-vs)şekilde değiştirin.

2. AzureGraphApiVersion için Kullanıcı ortam değişkenini şu şekilde oluşturun (veya zaten var olan bir küme): (bir kullanıcı ortam değişkeni oluşturmak Için, **gelişmiş** > **ortam değişkenlerine** > **sistem** > **Gelişmiş sistem ayarları** ' **na > gidin** .)

    `Variable name: AzureGraphApiVersion` `Variable value: 2014-04-01`

3. Proje türüne bağlı olarak, [ASP.net](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#net-with-applicationinsightsconfig) veya [ASP.NET Core](#aspnet-core) için uygun Application Insights SDK uç noktası değişiklikleri yapın.

### <a name="snapshot-debugger"></a>Anlık Görüntü Hata Ayıklayıcı

Snapshot Debugger artık Azure Kamu müşterileri tarafından kullanılabilir. Snapshot Debugger kullanmak için, [Snapshot Collector sürüm 1.3.5](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector/1.3.5-pre-1906.403) veya sonraki bir sürümü kullandığınızdan emin olmanız gerekir. Ardından standart [Snapshot Debugger belgelerini](https://docs.microsoft.com/azure/azure-monitor/app/snapshot-debugger)izlemeniz yeterlidir.

### <a name="sdk-endpoint-modifications"></a>SDK uç noktası değişiklikleri

Application Insights verilerini Azure Kamu bölgesine göndermek için Application Insights SDK 'Ları tarafından kullanılan varsayılan uç nokta adreslerini değiştirmeniz gerekir. Her SDK biraz farklı değişiklik gerektirir.

### <a name="net-with-applicationinsightsconfig"></a>ApplicationInsights. config ile .NET

> [!NOTE]
> Her bir SDK yükseltmesi gerçekleştirildiğinde ApplicationInsights. config dosyası otomatik olarak üzerine yazılır. SDK yükseltmesini gerçekleştirdikten sonra bölgeye özgü uç nokta değerlerini yeniden girdiğinizden emin olun.

```xml
<ApplicationInsights>
  ...
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <QuickPulseServiceEndpoint>https://quickpulse.applicationinsights.us/QuickPulseService.svc</QuickPulseServiceEndpoint>
    </Add>
  </TelemetryModules>
    ...
  <TelemetryChannel>
     <EndpointAddress>https://dc.applicationinsights.us/v2/track</EndpointAddress>
  </TelemetryChannel>
  ...
  <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
    <ProfileQueryEndpoint>https://dc.applicationinsights.us/api/profiles/{0}/appId</ProfileQueryEndpoint>
  </ApplicationIdProvider>
  ...
</ApplicationInsights>
```

### <a name="aspnet-core"></a>ASP.NET Çekirdeği

Ana uç noktayı ayarlamak için projenizdeki appSettings. json dosyasını aşağıdaki şekilde değiştirin:

```json
"ApplicationInsights": {
    "InstrumentationKey": "instrumentationkey",
    "TelemetryChannel": {
      "EndpointAddress": "https://dc.applicationinsights.us/v2/track"
    }
  }
```

Canlı ölçümler ve profil sorgu uç noktası değerleri yalnızca kod aracılığıyla ayarlanabilir. Tüm uç nokta değerlerinin varsayılan değerlerini kod aracılığıyla geçersiz kılmak için, `Startup.cs` dosyasının `ConfigureServices` yönteminde aşağıdaki değişiklikleri yapın:

```csharp
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel; //place at top of Startup.cs file

   services.ConfigureTelemetryModule<QuickPulseTelemetryModule>((module, o) => module.QuickPulseServiceEndpoint="https://quickpulse.applicationinsights.us/QuickPulseService.svc");

   services.AddSingleton<IApplicationIdProvider, ApplicationInsightsApplicationIdProvider>(_ => new ApplicationInsightsApplicationIdProvider() { ProfileQueryEndpoint = "https://dc.applicationinsights.us/api/profiles/{0}/appId" });

   services.AddSingleton<ITelemetryChannel>(_ => new ServerTelemetryChannel() { EndpointAddress = "https://dc.applicationinsights.us/v2/track" });

    //place in ConfigureServices method. If present, place this prior to   services.AddApplicationInsightsTelemetry("instrumentation key");
```

### <a name="azure-functions"></a>Azure İşlevleri

Lütfen aşağıdaki paketleri Işlev projenize yüklersiniz:

- Microsoft. ApplicationInsights sürümü 2.10.0
- Microsoft. ApplicationInsights. PerfCounterCollector sürüm 2.10.0
- Microsoft. ApplicationInsights. WindowsServer. TelemetryChannel sürüm 2.10.0

Ayrıca, Işlev uygulamanız için başlangıç kodunu ekleyin (veya değiştirin):

```csharp
[assembly: FunctionsStartup(typeof(Example.Startup))]
namespace Example
{
  class Startup : FunctionsStartup
  {
      public override void Configure(IFunctionsHostBuilder builder)
      {
          var quickPulseFactory = builder.Services.FirstOrDefault(sd => sd.ServiceType == typeof(ITelemetryModule) && 
                                               sd.ImplementationType == typeof(QuickPulseTelemetryModule));
          if (quickPulseFactory != null)
          {
              builder.Services.Remove(quickPulseFactory);
          }

          var appIdFactory = builder.Services.FirstOrDefault(sd => sd.ServiceType == typeof(IApplicationIdProvider));
          if (appIdFactory != null)
          {
              builder.Services.Remove(appIdFactory);
          }

          var channelFactory = builder.Services.FirstOrDefault(sd => sd.ServiceType == typeof(ITelemetryChannel));
          if (channelFactory != null)
          {
              builder.Services.Remove(channelFactory);
          }

          builder.Services.AddSingleton<ITelemetryModule, QuickPulseTelemetryModule>(_ =>
              new QuickPulseTelemetryModule
              {
                  QuickPulseServiceEndpoint = "https://quickpulse.applicationinsights.us/QuickPulseService.svc"
              });

          builder.Services.AddSingleton<IApplicationIdProvider, ApplicationInsightsApplicationIdProvider>(_ => new ApplicationInsightsApplicationIdProvider() { ProfileQueryEndpoint = "https://dc.applicationinsights.us/api/profiles/{0}/appId" });

          builder.Services.AddSingleton<ITelemetryChannel>(_ => new ServerTelemetryChannel() { EndpointAddress = "https://dc.applicationinsights.us/v2/track" });
      }
  }
}
```

### <a name="java"></a>Java

ApplicationInsights. xml dosyasını değiştirerek varsayılan uç nokta adresini değiştirin.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>ffffeeee-dddd-cccc-bbbb-aaaa99998888</InstrumentationKey>
  <TelemetryModules>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
  </TelemetryModules>
  <TelemetryInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>
  </TelemetryInitializers>
  <!--Add the following Channel value to modify the Endpoint address-->
  <Channel type="com.microsoft.applicationinsights.channel.concrete.inprocess.InProcessTelemetryChannel">
  <EndpointAddress>https://dc.applicationinsights.us/v2/track</EndpointAddress>
  </Channel>
</ApplicationInsights>
```

### <a name="spring-boot"></a>Spring Boot

`application.properties` dosyasını değiştirin ve şunu ekleyin:

```yaml
azure.application-insights.channel.in-process.endpoint-address= https://dc.applicationinsights.us/v2/track
```

### <a name="nodejs"></a>Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY');
appInsights.defaultClient.config.endpointUrl = "https://dc.applicationinsights.us/v2/track"; // ingestion
appInsights.defaultClient.config.profileQueryEndpoint = "https://dc.applicationinsights.us/api/profiles/{0}/appId"; // appid/profile lookup
appInsights.defaultClient.config.quickPulseHost = "https://quickpulse.applicationinsights.us/QuickPulseService.svc"; //live metrics
appInsights.Configuration.start();
```

Uç noktalar, ortam değişkenleri aracılığıyla da yapılandırılabilir:

```
Instrumentation Key: "APPINSIGHTS_INSTRUMENTATIONKEY"
Profile Endpoint: "https://dc.applicationinsights.us/api/profiles/{0}/appId"
Live Metrics Endpoint: "https://quickpulse.applicationinsights.us/QuickPulseService.svc"
```

### <a name="javascript"></a>JavaScript

```javascript
<script type="text/javascript">
   var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){
      function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/next/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t
   }({
      instrumentationKey:"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx"
      endpointUrl: "https://dc.applicationinsights.us/v2/track"
   });

   window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

### <a name="firewall-exceptions"></a>Güvenlik Duvarı özel durumları

Azure Application Insights hizmeti, birkaç IP adresi kullanır. İzlemekte olduğunuz uygulama bir güvenlik duvarının arkasında barındırılıyorsa, bu adresleri bilmeniz gerekebilir.

> [!NOTE]
> Bu adresler statik olsa da, bunları zaman zaman değiştirmek zorunda olduğumuz olasıdır. Tüm Application Insights trafiği, gelen güvenlik duvarı kuralları gerektiren kullanılabilirlik izleme ve Web kancaları dışında giden trafiği temsil eder.

### <a name="outgoing-ports"></a>Giden bağlantı noktaları
Application Insights SDK ve/veya Durum İzleyicisi portala veri göndermesini sağlamak için sunucunuzun güvenlik duvarında bazı giden bağlantı noktalarını açmanız gerekir:

| Amaç | URL'si | IP | Bağlantı Noktaları |
| --- | --- | --- | --- |
| Telemetri | dc.applicationinsights.us | 23.97.4.113 | 443 |

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri
Azure Izleyici günlükleri Azure Kamu 'da genel kullanıma sunulmuştur.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

### <a name="variations"></a>Farklılıkları

* Azure Kamu 'da bulunan çözümler şunlardır:
  * [Ağ performansı İzleyicisi (NPM)](https://blogs.msdn.microsoft.com/azuregov/2017/09/05/network-performance-monitor-general-availability/) -NPM, genel ve hibrit bulut ortamları için bulut tabanlı bir ağ izleme çözümüdür. Kuruluşlar şirket içi ve bulut ortamlarında ağ kullanılabilirliğini izlemek için NPM 'yi kullanır.  Uç nokta Izleyicisi-NPM 'nin bir alt yeteneği, uygulamalara yönelik ağ bağlantısını izler.

Aşağıdaki Azure Izleyici günlükleri özellikleri ve çözümleri Şu anda Azure Kamu 'da mevcut değildir.

* Microsoft Azure ' de önizlemede olan çözümler şunlar da dahildir:
  * Hizmet Eşlemesi
  * Windows 10 Upgrade Analytics çözümü
  * Application Insights çözümü
  * Azure ağ güvenlik grubu analizi çözümü
  * Azure Otomasyonu analiz çözümü
  * Key Vault Analytics çözümü
* Şirket içi yazılımda güncelleştirme gerektiren çözümler ve özellikler şunlar da dahildir:
  * Surface Hub çözümü
* Genel Azure 'da önizlemede olan ve aşağıdakiler dahil olmak üzere Özellikler:
  * Power BI verileri dışarı aktarma
* Azure ölçümleri ve Azure tanılama

Azure Izleme günlüklerinin URL 'Leri Azure Kamu 'da farklıdır:

| Azure genel | Azure Kamu | Notlar |
| --- | --- | --- |
| mms.microsoft.com |oms.microsoft.us |Log Analytics çalışma alanları portalı |
| çalışma alanı *kimliği*. ods.opinsights.Azure.com |çalışma alanı *kimliği*. ods.opinsights.Azure.us |[Veri Toplayıcı API’si](../azure-monitor/platform/data-collector-api.md) |
| \*.ods.opinsights.azure.com |\*. ods.opinsights.azure.us |Aracı iletişimi- [Güvenlik Duvarı ayarlarını yapılandırma](../log-analytics/log-analytics-proxy-firewall.md) |
| \*.oms.opinsights.azure.com |\*. oms.opinsights.azure.us |Aracı iletişimi- [Güvenlik Duvarı ayarlarını yapılandırma](../log-analytics/log-analytics-proxy-firewall.md) |
| \*.blob.core.windows.net |\*. blob.core.usgovcloudapi.net |Aracı iletişimi- [Güvenlik Duvarı ayarlarını yapılandırma](../log-analytics/log-analytics-proxy-firewall.md) |
| portal.loganalytics.io |portal.loganalytics.us |Gelişmiş analiz portalı- [Güvenlik Duvarı ayarlarını yapılandırma](../azure-monitor/log-query/portals.md) |
| api.loganalytics.io |api.loganalytics.us |Gelişmiş analiz portalı- [Güvenlik Duvarı ayarlarını yapılandırma](../azure-monitor/log-query/portals.md) |
| docs.loganalytics.io |docs.loganalytics.us |Gelişmiş analiz portalı- [Güvenlik Duvarı ayarlarını yapılandırma](../azure-monitor/log-query/portals.md) |
| \*.azure-automation.net |\*. azure-automation.us |Azure Otomasyonu- [Güvenlik Duvarı ayarlarını yapılandırma](../azure-monitor/platform/log-analytics-agent.md#network-firewall-requirements) |
| Yok | *. usgovtrafficmanager.net | Azure Traffic Manager- [Güvenlik Duvarı ayarlarını yapılandırma](../azure-monitor/platform/log-analytics-agent.md#network-firewall-requirements) |

Aşağıdaki Azure Izleyici günlükleri özellikleri, Azure Kamu 'da farklı davranır:

* System Center Operations Manager yönetim grubunuzu Azure Izleyici günlüklerine bağlamak için, güncelleştirilmiş yönetim paketlerini indirip içeri aktarmanız gerekir.
  + System Center Operations Manager 2016
    1. [2016 System Center Operations Manager Için güncelleştirme paketi 2](https://support.microsoft.com/help/3209591)' i yükler.
    2. Güncelleştirme paketi 2 ' nin bir parçası olarak dahil edilen yönetim paketlerini Operations Manager içine aktarın. Bir diskten bir yönetim paketini içeri aktarma hakkında daha fazla bilgi için bkz. [Operations Manager Management Pack 'ı Içeri aktarma](https://technet.microsoft.com/library/hh212691.aspx).
    3. Operations Manager Azure Izleyici günlüklerine bağlamak için, [Azure izleyici günlüklerine bağlanma Operations Manager](../azure-monitor/platform/om-agents.md)içindeki adımları izleyin.
  + System Center Operations Manager 2012 R2 UR3 (veya üzeri)/Operations Manager 2012 SP1 UR7 SÜRÜMLERIYLE (veya üzeri)
    1. [Güncelleştirilmiş yönetim paketlerini](https://go.microsoft.com/fwlink/?LinkId=828749)indirin ve kaydedin.
    2. İndirdiğiniz dosyayı ayıklayın.
    3. Yönetim paketlerini Operations Manager içine aktarın. Bir diskten bir yönetim paketini içeri aktarma hakkında daha fazla bilgi için bkz. [Operations Manager Management Pack 'ı Içeri aktarma](https://technet.microsoft.com/library/hh212691.aspx).
    4. Operations Manager Azure Izleyici günlüklerine bağlamak için, [Azure izleyici günlüklerine bağlanma Operations Manager](../azure-monitor/platform/om-agents.md)içindeki adımları izleyin.

* Configuration Manager bilgisayardan bilgisayar gruplarını kullanma hakkında daha fazla bilgi için bkz. [Azure izleyiciyi Configuration Manager bağlama](../azure-monitor/platform/collect-sccm.md).

### <a name="frequently-asked-questions"></a>Sık sorulan sorular
* Microsoft Azure Azure Izleyici günlüklerinden Azure Kamu 'ya veri geçirebilir miyim?
  * Hayır. Microsoft Azure verileri veya çalışma alanınızı Azure Kamu 'ya taşımak mümkün değildir.
* Operations Management Suite portalından Microsoft Azure ve Azure Kamu çalışma alanları arasında geçiş yapabilir miyim?
  * Hayır. Microsoft Azure ve Azure Kamu portalları için portallar ayrıdır ve bilgi paylaşmazlar.

Daha fazla bilgi için bkz. [Azure izleyici günlükleri genel belgeleri](../log-analytics/log-analytics-overview.md).

## <a name="scheduler"></a>Scheduler
Bu hizmet ve nasıl kullanılacağı hakkında bilgi için bkz. [Azure Scheduler belgeleri](../scheduler/index.md).

## <a name="azure-portal"></a>Azure portalı
Azure Kamu portalına [buradan](https://portal.azure.us)erişebilirsiniz.

## <a name="azure-resource-manager"></a>Azure Resource Manager
Bu hizmet ve nasıl kullanılacağı hakkında bilgi için bkz. [Azure Resource Manager belgeleri](../azure-resource-manager/management/overview.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Kamu bloguna](https://blogs.msdn.microsoft.com/azuregov/) abone olma
* [Azure-gov](https://stackoverflow.com/questions/tagged/azure-gov) kullanarak Stack Overflow hakkında yardım alın
* [Azure Kamu geri bildirim Forumu](https://feedback.azure.com/forums/558487-azure-government) aracılığıyla geri bildirimde bulunun veya yeni özellikler isteyin
