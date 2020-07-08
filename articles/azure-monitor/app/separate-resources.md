---
title: Application Insights dağıtımınızı tasarlama-bir çok kaynak karşılaştırması
description: Geliştirme, test ve üretim damgaları için farklı kaynaklara doğrudan telemetri.
ms.topic: conceptual
ms.date: 05/11/2020
ms.openlocfilehash: 187d84b29e42aa3264417dd66e66c3886b17e92a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83773690"
---
# <a name="how-many-application-insights-resources-should-i-deploy"></a>Kaç Application Insights kaynak dağıtmalıyım?

Bir Web uygulamasının sonraki sürümünü geliştirirken, yeni sürümden ve önceden yayınlanan sürümden [Application Insights](../../azure-monitor/app/app-insights-overview.md) telemetrisini karıştırmak istemezsiniz. Karışıklığın önüne geçmek için, farklı geliştirme aşamalarından Telemetriyi ayrı Application Insights kaynaklarına (ıkeys 'ler) göre ayrı olarak gönderin. Bir sürüm bir aşamadan diğerine geçiş yaparken izleme anahtarının değiştirilmesini kolaylaştırmak için, yapılandırma dosyası yerine koddaki Ikey ' i ayarlamak yararlı olabilir.

(Sisteminiz bir Azure bulut hizmeti ise [ayrı ıkeys 'leri ayarlamaya yönelik başka bir yöntem](../../azure-monitor/app/cloudservices.md)vardır.)

## <a name="about-resources-and-instrumentation-keys"></a>Kaynaklar ve izleme anahtarları hakkında

Web uygulamanız için Application Insights izlemeyi ayarlarken Microsoft Azure ' de bir Application Insights *kaynağı* oluşturursunuz. Uygulamanızdan toplanan Telemetriyi görmek ve analiz etmek için Azure portal bu kaynağı açarsınız. Kaynak bir *izleme anahtarı* (Ikey) ile tanımlanır. Uygulamanızı izlemek için Application Insights paketini yüklediğinizde, Telemetriyi nereye gönderileceğini bilmesi için izleme anahtarıyla yapılandırırsınız.

Her Application Insights kaynak, kullanıma hazır olan ölçümler ile gelir. Tek tek bileşenler aynı Application Insights kaynağına rapor alıyorsa, bu ölçümler Pano/Uyarı üzerinde anlamlı olmayabilir.

### <a name="when-to-use-a-single-application-insights-resource"></a>Tek bir Application Insights kaynağı ne zaman kullanılır?

-   Birlikte dağıtılan uygulama bileşenleri için. Genellikle aynı DevOps/Ise kullanıcıları tarafından yönetilen tek bir ekip tarafından geliştirilmiştir.
-   Bu, yanıt süreleri, panodaki hata oranları gibi ana performans göstergelerini (KPI 'Lar), varsayılan olarak tüm bunların tamamında (Ölçüm Gezgini deneyiminde rol adına göre segmentleyebilirsiniz) bir araya getirir.
-   Rol tabanlı Access Control (RBAC) uygulama bileşenleri arasında farklı bir şekilde yönetmeye gerek yoktur.
-   Bileşenler arasında farklı ölçüm uyarısı ölçütlerine gerek yoksa.
-   Sürekli dışarı aktarmaları bileşenler arasında farklı şekilde yönetmeniz gerekmiyorsa.
-   Faturalandırma/kotaları bileşenler arasında farklı şekilde yönetmeniz gerekmiyorsa.
-   Bir API anahtarının tüm bileşenlerdeki verilere erişimi olması normaldir. Ve 10 API anahtarı tüm bunlar genelinde gereksinimler için yeterlidir.
-   Tüm roller genelinde aynı akıllı algılama ve çalışma öğesi tümleştirme ayarlarına sahip olmak istiyorsanız.

### <a name="other-things-to-keep-in-mind"></a>Göz önünde bulundurmanız gereken diğer şeyler

-   Anlamlı değerlerin [Cloud_RoleName](https://docs.microsoft.com/azure/azure-monitor/app/app-map?tabs=net#set-cloud-role-name) özniteliğine ayarlandığından emin olmak için özel kod eklemeniz gerekebilir. Bu öznitelik için anlamlı değerler ayarlanmaksızın, Portal deneyimlerinden *hiçbiri* çalışmayacak.
- Service Fabric uygulamalar ve klasik bulut hizmetleri için SDK otomatik olarak Azure rol ortamından okur ve bunları ayarlar. Diğer tüm uygulama türleri için büyük olasılıkla bunu açıkça ayarlamanız gerekecektir.
-   Canlı ölçüm deneyimi, rol adına göre bölmeyi desteklemez.

## <a name="dynamic-instrumentation-key"></a><a name="dynamic-ikey"></a>Dinamik izleme anahtarı

Kod üretim aşamaları arasında taşındığı için Ikey 'in değiştirilmesini kolaylaştırmak için yapılandırma dosyası yerine kodu olarak ayarlayın.

Bir ASP.NET hizmetinde global.aspx.cs gibi bir başlatma yönteminde anahtarı ayarlayın:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

Bu örnekte, farklı kaynaklar için ıkeys 'ler Web yapılandırma dosyasının farklı sürümlerine yerleştirilir. Yayın betiğinin bir parçası olarak yapabileceğiniz Web yapılandırma dosyasını değiştirme-hedef kaynağı takas eder.

### <a name="web-pages"></a>Web sayfaları
Ikey, uygulamanızın Web sayfalarında [hızlı başlangıç bölmesinden aldığınız betikte](../../azure-monitor/app/javascript.md)de kullanılır. Komut dosyasına tam olarak kodlamak yerine sunucu durumundan oluşturun. Örneğin, bir ASP.NET uygulamasında:

*Razor 'de JavaScript*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a>Ek Application Insights kaynakları oluşturma

Bir Application Insights kaynağı oluşturmak için [kaynak oluşturma kılavuzunu](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource)izleyin.

### <a name="getting-the-instrumentation-key"></a>İzleme anahtarını alma
İzleme anahtarı, oluşturduğunuz kaynağı tanımlar.

Uygulamanızın veri göndereceği tüm kaynakların izleme anahtarlarına ihtiyacınız vardır.

## <a name="filter-on-build-number"></a>Derleme numarasını filtrele
Uygulamanızın yeni bir sürümünü yayımladığınızda, farklı yapılardan Telemetriyi ayırabilmek isteyeceksiniz.

Uygulama sürümü özelliğini, [arama](../../azure-monitor/app/diagnostic-search.md) ve [Ölçüm Gezgini](../../azure-monitor/platform/metrics-charts.md) sonuçlarını filtreleyebilmeniz için ayarlayabilirsiniz.

Uygulama sürümü özelliğini ayarlamanın birkaç farklı yöntemi vardır.

* Doğrudan ayarla:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* Tüm TelemetryClient örneklerinin tutarlı şekilde ayarlandığından emin olmak için bu satırı bir [telemetri başlatıcısında](../../azure-monitor/app/api-custom-events-metrics.md#defaults) sarın.
* [ASP.NET] Sürümünü ' de ayarlayın `BuildInfo.config` . Web modülü, BuildLabel düğümünden sürümü alacak. Bu dosyayı projenize ekleyin ve Çözüm Gezgini her zaman Kopyala özelliğini ayarlamayı unutmayın.

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] MSBuild 'te otomatik olarak BuildInfo.config oluşturun. Bunu yapmak için dosyanıza birkaç satır ekleyin `.csproj` :

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Bu, *yourprojectname*.BuildInfo.config adlı bir dosya oluşturur. Yayımla işlemi onu BuildInfo.config olarak yeniden adlandırır.

    Visual Studio ile derleme sırasında derleme etiketi bir yer tutucu (AutoGen_...) içerir. Ancak MSBuild ile birlikte kullanıldığında, doğru sürüm numarasıyla doldurulur.

    MSBuild 'in sürüm numaraları oluşturmasına izin vermek için AssemblyReference.cs içindeki gibi bir sürümü ayarlayın `1.0.*`

## <a name="version-and-release-tracking"></a>Sürüm ve sürüm izleme
Uygulama sürümünü izlemek için `buildinfo.config` dosyasının Microsoft Build Engine işleminiz tarafından oluşturulduğundan emin olun. `.csproj`Dosyanıza şunu ekleyin:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Yapı bilgisi mevcut olduğunda Application Insights web modülü **Uygulama sürümünü** telemetrinin her bir öğesine bir özellik olarak ekler. Bu sayede, [tanılama aramaları](../../azure-monitor/app/diagnostic-search.md) gerçekleştirirken veya [ölçümleri keşfederken](../../azure-monitor/platform/metrics-charts.md) sürüme göre filtreleyebilirsiniz.

Ancak, derleme sürüm numarasının Visual Studio 'daki geliştirici derlemesi tarafından değil, yalnızca Microsoft Build Engine tarafından oluşturulup oluşturulduğuna dikkat edin.

### <a name="release-annotations"></a>Sürüm ek açıklamaları
Azure DevOps kullanıyorsanız, yeni bir sürüm yayınlayışınızda grafiklerinize [ek açıklama işaretleyicisi](../../azure-monitor/app/annotations.md) ekleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Birden çok rol için paylaşılan kaynaklar](../../azure-monitor/app/app-map.md)
* [Ayırt etmek için telemetri başlatıcısı oluşturma | B çeşitleri](../../azure-monitor/app/api-filtering-sampling.md#add-properties)
