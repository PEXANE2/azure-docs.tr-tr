---
title: ApplicationInsights.config referans - Azure | Microsoft Dokümanlar
description: Veri toplama modüllerini etkinleştirin veya devre dışı kullanabilirsiniz ve performans sayaçları ve diğer parametreler ekleyin.
ms.topic: conceptual
ms.date: 05/22/2019
ms.reviewer: olegan
ms.openlocfilehash: 460bd79e3a37c492301f7438112fef4487618fed
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80982098"
---
# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>ApplicationInsights.config veya .xml ile Application Insights SDK yapılandırma
Application Insights .NET SDK bir dizi NuGet paketinden oluşur. [Çekirdek paket](https://www.nuget.org/packages/Microsoft.ApplicationInsights) Uygulama Öngörüleri telemetri göndermek için API sağlar. [Ek paketler,](https://www.nuget.org/packages?q=Microsoft.ApplicationInsights) uygulamanızdan ve bağlamından telemetriyi otomatik olarak izlemek için telemetri *modülleri* ve *başlangıç layıcıları* sağlar. Yapılandırma dosyasını ayarlayarak, Telemetri Modüllerini ve baş harflerini etkinleştirebilir veya devre dışı bırakabilir ve bazıları için parametreler ayarlayabilirsiniz.

Yapılandırma dosyası, `ApplicationInsights.config` uygulamanızın türüne bağlı olarak adlandırılmış veya. `ApplicationInsights.xml` [SDK'nın çoğu sürümü yüklediğinizde][start]otomatik olarak projenize eklenir. Varsayılan olarak, Uygulama **Öngörüleri >'i Ekle'yi**destekleyen Visual Studio şablon projelerinden gelen otomatik deneyimi kullanırken, ApplicationInsights.config dosyası proje kök klasöründe oluşturulur ve uyulduğunda depo kutusuna kopyalanır. Ayrıca bir [IIS sunucusunda Status Monitor][redfield]tarafından bir web uygulamasına eklenir. Azure web sitesi [için uzantı](azure-web-apps.md) veya [Azure VM için uzantı ve sanal makine ölçeği kümesi](azure-vm-vmss-apps.md) kullanılıyorsa yapılandırma dosyası yoksayılır.

[Bir web sayfasında SDK'yı][client]kontrol etmek için eşdeğer bir dosya yok.

Bu belge, yapılandırma dosyasında gördüğünüz bölümleri, SDK bileşenlerini nasıl denetlediklerini ve hangi NuGet paketlerinin bu bileşenleri yüklediği açıklanır.

> [!NOTE]
> ApplicationInsights.config ve .xml talimatları .NET Core SDK için geçerli değildir. .NET Core uygulamalarını yapılandırmak için [bu](../../azure-monitor/app/asp-net-core.md) kılavuzu izleyin.

## <a name="telemetry-modules-aspnet"></a>Telemetri Modülleri (ASP.NET)
Her Telemetri Modülü belirli bir veri türünü toplar ve verileri göndermek için çekirdek API'yi kullanır. Modüller, .config dosyasına gerekli satırları da ekleyen farklı NuGet paketleri tarafından yüklenir.

Yapılandırma dosyasında her modül için bir düğüm vardır. Modülü devre dışı bırakın, düğümü silin veya silinebilir.

### <a name="dependency-tracking"></a>Bağımlılık İzleme
[Bağımlılık izleme,](../../azure-monitor/app/asp-net-dependencies.md) uygulamanızın veritabanlarına, dış hizmetlere ve veritabanlarına yaptığı aramalar hakkında telemetri toplar. Bu modülün bir IIS sunucusunda çalışmasına izin vermek için [Durum Monitörü'ne yüklemeniz][redfield]gerekir.

[TrackDependency API'yi](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)kullanarak kendi bağımlılık izleme kodunuzu da yazabilirsiniz.

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet paketi.

Bağımlılıklar, aracı tabanlı (kodsuz) ekleme yi kullanarak kodunuzu değiştirmeden otomatik olarak toplanabilir. Azure web uygulamalarında kullanmak için [Uygulama Öngörüleri uzantısını](azure-web-apps.md)etkinleştirin. Azure VM veya Azure sanal makine ölçeği kümesinde kullanmak [için VM ve sanal makine ölçeği kümesi için Uygulama İzleme uzantısını](azure-vm-vmss-apps.md)etkinleştirin.

### <a name="performance-collector"></a>Performans toplayıcısı
IIS yüklemelerinden CPU, bellek ve ağ yükü gibi [sistem performans sayaçlarını toplar.](../../azure-monitor/app/performance-counters.md) Kendiniz ayarladığınız performans sayaçları da dahil olmak üzere hangi sayaçların toplandığını belirtebilirsiniz.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet paketi.

### <a name="application-insights-diagnostics-telemetry"></a>Uygulama Öngörüleri Tanılama Telemetri
Application `DiagnosticsTelemetryModule` Insights enstrümantasyon kodunun kendisindeki raporlar hataları. Örneğin, kod performans sayaçlarına erişemiyorsa `ITelemetryInitializer` veya bir özel durum atarsa. Bu modül tarafından izlenen izleme telemetritanı [arama][diagnostic]görünür.

```
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package. If you only install this package, the ApplicationInsights.config file is not automatically created.
```

### <a name="developer-mode"></a>Geliştirici Modu
`DeveloperModeWithDebuggerAttachedTelemetryModule`Uygulama Öngörüleri'ni, `TelemetryChannel` uygulama işlemine bir hata ayıklama bağlı olduğunda, aynı anda bir telemetri öğesi olan verileri hemen göndermeye zorlar. Bu, uygulamanızın telemetriyi izlediği an ile Uygulama Öngörüleri portalında görüntülenme süresi arasındaki süreyi azaltır. CPU ve ağ bant genişliğinde önemli ek yükü neden olur.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Uygulama Öngörüleri Windows Server](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet paketi

### <a name="web-request-tracking"></a>Web İstek Takibi
HTTP isteklerinin [yanıt süresini ve sonuç kodunu](../../azure-monitor/app/asp-net.md) bildirir.

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet paketi

### <a name="exception-tracking"></a>Özel durum izleme
`ExceptionTrackingTelemetryModule`web uygulamanızdaki işlenmemiş özel durumları izler. Bkz. [Hatalar ve özel durumlar.][exceptions]

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet paketi
* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`- [gözlenmeyen görev özel durumlarını](https://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx)izler.
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`- çalışan rolleri, windows hizmetleri ve konsol uygulamaları için işlenmemiş özel durumları izler.
* [Uygulama Öngörüleri Windows Server](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet paketi.

### <a name="eventsource-tracking"></a>EventSource İzleme
`EventSourceTelemetryModule`Traces olarak Application Insights'a gönderilecek EventSource olaylarını yapılandırmanızı sağlar. EventSource olaylarını izleme hakkında daha fazla bilgi için, [Bkz. EventSource Etkinliklerini Kullanma.](../../azure-monitor/app/asp-net-trace-logs.md#use-eventsource-events)

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [Microsoft.ApplicationInsights.EventSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a>ETW Olay Takibi
`EtwCollectorTelemetryModule`ETW sağlayıcılarından gelen olayları, Traces olarak Uygulama Öngörüleri'ne gönderilmek üzere yapılandırmanıza olanak tanır. ETW olaylarını izleme hakkında daha fazla bilgi için [ETW Olaylarını Kullanma'ya](../../azure-monitor/app/asp-net-trace-logs.md#use-etw-events)bakın.

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [Microsoft.ApplicationInsights.EtwCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights
Microsoft.ApplicationInsights paketi [SDK'nın temel API'sini](https://msdn.microsoft.com/library/mt420197.aspx) sağlar. Diğer Telemetri Modülleri bunu kullanır ve kendi [telemetrinizi tanımlamak için de kullanabilirsiniz.](../../azure-monitor/app/api-custom-events-metrics.md)

* ApplicationInsights.config giriş yok.
* [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet paketi. Bu NuGet'i yüklerseniz,.config dosyası oluşturulamaz.

## <a name="telemetry-channel"></a>Telemetri Kanalı
[Telemetri kanalı,](telemetry-channels.md) telemetrinin arabelleğe alma ve Uygulama Öngörüleri hizmetine iletilmesini yönetir.

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`web uygulamaları için varsayılan kanaldır. Bellekteki verileri arabelleğe getirir ve daha güvenilir telemetri teslimi için yeniden deneme mekanizmaları ve yerel disk depolama kullanır.
* `Microsoft.ApplicationInsights.InMemoryChannel`başka bir kanal yapılandırılmamışsa kullanılan hafif bir telemetri kanalıdır. 

## <a name="telemetry-initializers-aspnet"></a>Telemetri Başharfleri (ASP.NET)
Telemetri Initializers telemetri her öğe ile birlikte gönderilen bağlam özellikleri ayarlayın.

Bağlam özelliklerini ayarlamak için [kendi baş harflerinizi yazabilirsiniz.](../../azure-monitor/app/api-filtering-sampling.md#add-properties)

Standart baş harflerin tümü Web veya WindowsServer NuGet paketleri tarafından ayarlanır:

* `AccountIdTelemetryInitializer`AccountId özelliğini ayarlar.
* `AuthenticatedUserIdTelemetryInitializer`AuthenticatedUserId özelliğini JavaScript SDK tarafından ayarlanan ayarlar.
* `AzureRoleEnvironmentTelemetryInitializer`Tüm `RoleName` telemetri `Device` öğeleri için bağlamın özelliklerini ve `RoleInstance` özelliklerini Azure çalışma zamanı ortamından çıkarılan bilgilerle güncelleştirir.
* `BuildInfoConfigComponentVersionTelemetryInitializer`MS `Version` Build tarafından `Component` üretilen dosyadan çıkarılan değerle tüm telemetri öğeleri için bağlamın özelliğini `BuildInfo.config` güncelleştirir.
* `ClientIpHeaderTelemetryInitializer`isteğin `X-Forwarded-For` HTTP `Location` üstbilgisini temel alan tüm telemetri öğelerinin bağlamının özelliğini güncelleştirir. `Ip`
* `DeviceTelemetryInitializer`tüm telemetri `Device` öğeleri için bağlamın aşağıdaki özelliklerini güncelleştirir.
  * `Type`"PC" olarak ayarlanır
  * `Id`web uygulamasının çalıştığı bilgisayarın alan adı olarak ayarlanır.
  * `OemName`WMI kullanılarak `Win32_ComputerSystem.Manufacturer` alandan çıkarılan değere ayarlanır.
  * `Model`WMI kullanılarak `Win32_ComputerSystem.Model` alandan çıkarılan değere ayarlanır.
  * `NetworkType`'den çıkarılan değere `NetworkInterface`ayarlanır.
  * `Language`adına `CurrentCulture`ayarlanır.
* `DomainNameRoleInstanceTelemetryInitializer``RoleInstance` web uygulamasının `Device` çalıştığı bilgisayarın etki alanı adı ile tüm telemetri öğeleri için bağlam Özelliğini güncelleştirir.
* `OperationNameTelemetryInitializer`http `Name` yöntemine `RequestTelemetry` dayalı `Name` tüm telemetri öğelerinin `Operation` bağlamının özelliğini ve özelliğini ve ASP.NET MVC denetleyicisinin adlarını ve isteği işlemek için çağrılan eylemi günceller.
* `OperationIdTelemetryInitializer`veya `OperationCorrelationTelemetryInitializer` bir `Operation.Id` isteği otomatik olarak oluşturulan `RequestTelemetry.Id`ile işlerken izlenen tüm telemetri öğelerinin bağlam özelliğini güncelleştirir.
* `SessionTelemetryInitializer`Kullanıcının `Id` `Session` tarayıcısında çalışan ApplicationInsights JavaScript enstrümantasyon `ai_session` kodu tarafından oluşturulan çerezden çıkarılan değerle tüm telemetri öğeleriiçin bağlamın özelliğini güncelleştirir.
* `SyntheticTelemetryInitializer`veya `SyntheticUserAgentTelemetryInitializer` kullanılabilirlik `Session`testi `Operation` veya arama motoru botu gibi sentetik bir kaynaktan gelen bir isteği işlerken izlenen tüm telemetri öğelerinin `User`, ve bağlam özelliklerini güncelleştirir. Varsayılan olarak, [Metrics Explorer](../../azure-monitor/app/metrics-explorer.md) sentetik telemetri görüntülemez.

    İsteklerin `<Filters>` özelliklerini tanımlayan küme.
* `UserTelemetryInitializer`Kullanıcının `Id` `AcquisitionDate` `User` tarayıcısında çalışan Application Insights JavaScript enstrümantasyon kodu tarafından oluşturulan `ai_user` çerezden çıkarılan değerlerle tüm telemetri öğeleri için bağlamın özelliklerini ve özelliklerini güncelleştirir.
* `WebTestTelemetryInitializer`[kullanılabilirlik testlerinden](../../azure-monitor/app/monitor-web-app-availability.md)gelen HTTP istekleri için kullanıcı kimliğini, oturum kimliğini ve sentetik kaynak özelliklerini ayarlar.
  İsteklerin `<Filters>` özelliklerini tanımlayan küme.

Service Fabric'te çalışan .NET uygulamaları için `Microsoft.ApplicationInsights.ServiceFabric` NuGet paketini ekleyebilirsiniz. Bu paket, `FabricTelemetryInitializer`telemetri öğelerine Service Fabric özelliklerini ekleyen bir , içerir. Daha fazla bilgi için, bu NuGet paketi tarafından eklenen özellikler hakkında [GitHub sayfasına](https://github.com/Microsoft/ApplicationInsights-ServiceFabric/blob/master/README.md) bakın.

## <a name="telemetry-processors-aspnet"></a>Telemetri İşlemcileri (ASP.NET)
Telemetri İşlemcileri, her telemetri öğesini SDK'dan portala gönderilmeden hemen önce filtreleyebilir ve değiştirebilir.

Kendi [Telemetri İşlemcilerinizi yazabilirsiniz.](../../azure-monitor/app/api-filtering-sampling.md#filtering)

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Adaptif örnekleme Telemetri İşlemcisi (2.0.0-beta3'ten itibaren)
Bu, varsayılan olarak etkindir. Uygulamanız çok fazla telemetri gönderiyorsa, bu işlemci bir kısmını kaldırır.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

Parametre, algoritmanın ulaşmaya çalıştığı hedefi sağlar. SDK'nın her örneği bağımsız olarak çalışır, bu nedenle sunucunuz birden fazla makineden oluşan bir kümeyse, telemetrinin gerçek hacmi buna göre çarpılır.

[Örnekleme hakkında daha fazla bilgi edinin.](../../azure-monitor/app/sampling.md)

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Sabit oranlı örnekleme Telemetri İşlemcisi (2.0.0-beta1'den itibaren)
Standart bir [örnekleme Telemetri İşlemcisi](../../azure-monitor/app/api-filtering-sampling.md) (2.0.1 itibaren):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```

## <a name="instrumentationkey"></a>EnstrümantasyonAnahtar
Bu, verilerinizin göründüğü Application Insights kaynağını belirler. Genellikle, uygulamalarınızın her biri için ayrı bir anahtarla ayrı bir kaynak oluşturursunuz.

Anahtarı dinamik olarak ayarlamak istiyorsanız (örneğin, uygulamanızdan farklı kaynaklara sonuç göndermek istiyorsanız- anahtarı yapılandırma dosyasından atlayabilir ve bunun yerine kodolarak ayarlayabilirsiniz.

Standart Telemetri Modülleri de dahil olmak üzere TelemetriIstemci'nin tüm örnekleri için anahtarı ayarlamak için. Bunu, ASP.NET bir hizmette global.aspx.cs gibi bir başlatma yönteminde yapın:

```csharp
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights;

    protected void Application_Start()
    {
        TelemetryConfiguration configuration = TelemetryConfiguration.CreateDefault();
        configuration.InstrumentationKey = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";
        var telemetryClient = new TelemetryClient(configuration);
   
```

Yalnızca farklı bir kaynağa belirli bir olay kümesi göndermek istiyorsanız, belirli bir Telemetri İstemci için anahtarı ayarlayabilirsiniz:

```csharp

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

Yeni bir anahtar edinmek [için, Application Insights portalında yeni bir kaynak oluşturun.][new]



## <a name="applicationid-provider"></a>ApplicationId Sağlayıcısı

_v2.6.0'dan itibaren kullanılabilir_

Bu sağlayıcının amacı, Bir Enstrümantasyon Anahtarına dayalı bir Uygulama Kimliği aramaktır. Uygulama Kimliği RequestTelemetry ve DependencyTelemetry'ye dahildir ve Portaldaki Korelasyon'u belirlemek için kullanılır.

Bu kod veya `TelemetryConfiguration.ApplicationIdProvider` config ayarı tarafından kullanılabilir.

### <a name="interface-iapplicationidprovider"></a>Arayüz: IApplicationIdProvider

```csharp
public interface IApplicationIdProvider
{
    bool TryGetApplicationId(string instrumentationKey, out string applicationId);
}
```


[Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) sdk iki uygulama sağlar: `ApplicationInsightsApplicationIdProvider` `DictionaryApplicationIdProvider`ve .

### <a name="applicationinsightsapplicationidprovider"></a>UygulamaInsightsApplicationIdProvider

Bu, Profil API'mizin etrafında bir ambalaj. İstekleri ve önbellek sonuçlarını daraltır.

[Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) veya [Microsoft.ApplicationInsights.Web'i](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) yüklediğinizde bu sağlayıcı config dosyanıza eklenir

Bu sınıfın isteğe `ProfileQueryEndpoint`bağlı bir özelliği vardır.
Varsayılan olarak bu `https://dc.services.visualstudio.com/api/profiles/{0}/appId`ayarlanır.
Bu yapılandırma için bir proxy yapılandırmanız gerekiyorsa, temel adresi ve "/api/profiles/{0}/appId" dahil olmak üzere proxy oluşturmanızı öneririz. ' '{0}' ' nin istek başına çalışma zamanında Enstrümantasyon Tuşu ile değiştirilir.

#### <a name="example-configuration-via-applicationinsightsconfig"></a>ApplicationInsights.config ile Örnek Yapılandırma:
```xml
<ApplicationInsights>
    ...
    <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
        <ProfileQueryEndpoint>https://dc.services.visualstudio.com/api/profiles/{0}/appId</ProfileQueryEndpoint>
    </ApplicationIdProvider>
    ...
</ApplicationInsights>
```

#### <a name="example-configuration-via-code"></a>Kod üzerinden Örnek Yapılandırma:
```csharp
TelemetryConfiguration.Active.ApplicationIdProvider = new ApplicationInsightsApplicationIdProvider();
```

### <a name="dictionaryapplicationidprovider"></a>SözlükApplicationIdProvider

Bu, yapılandırılmış Enstrümantasyon Anahtarı / Uygulama Kimliği çiftleri üzerinde duracaktır statik bir sağlayıcıdır.

Bu sınıfın, `Defined`Uygulama Kimliği çiftleri için Enstrümantasyon Anahtarının> bir Sözlük<dize, string olan bir özelliği vardır.

Bu sınıf, yapılandırmanızda bulunmayan bir Enstrümantasyon Anahtarı istendiğinde kullanmak üzere başka bir sağlayıcıyı yapılandırmak için kullanılabilecek isteğe bağlı bir özelliğe `Next` sahiptir.

#### <a name="example-configuration-via-applicationinsightsconfig"></a>ApplicationInsights.config ile Örnek Yapılandırma:
```xml
<ApplicationInsights>
    ...
    <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.DictionaryApplicationIdProvider, Microsoft.ApplicationInsights">
        <Defined>
            <Type key="InstrumentationKey_1" value="ApplicationId_1"/>
            <Type key="InstrumentationKey_2" value="ApplicationId_2"/>
        </Defined>
        <Next Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights" />
    </ApplicationIdProvider>
    ...
</ApplicationInsights>
```

#### <a name="example-configuration-via-code"></a>Kod üzerinden Örnek Yapılandırma:
```csharp
TelemetryConfiguration.Active.ApplicationIdProvider = new DictionaryApplicationIdProvider{
 Defined = new Dictionary<string, string>
    {
        {"InstrumentationKey_1", "ApplicationId_1"},
        {"InstrumentationKey_2", "ApplicationId_2"}
    }
};
```




## <a name="next-steps"></a>Sonraki adımlar
[API hakkında daha fazla bilgi edinin.][api]

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[client]: ../../azure-monitor/app/javascript.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[exceptions]: ../../azure-monitor/app/asp-net-exceptions.md
[netlogs]: ../../azure-monitor/app/asp-net-trace-logs.md
[new]: ../../azure-monitor/app/create-new-resource.md 
[redfield]: ../../azure-monitor/app/monitor-performance-live-website-now.md
[start]: ../../azure-monitor/app/app-insights-overview.md
