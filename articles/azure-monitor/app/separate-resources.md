---
title: Azure Application Insights telemetri ayırma
description: Geliştirme, test ve üretim damgaları için farklı kaynaklara doğrudan telemetri.
ms.topic: conceptual
ms.date: 05/15/2017
ms.openlocfilehash: 565d51751ad50479f4e227b6855ac63b80bd949e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81536786"
---
# <a name="separating-telemetry-from-development-test-and-production"></a>Geliştirme, test ve üretimden telemetri ayırma

Bir Web uygulamasının sonraki sürümünü geliştirirken, yeni sürümden ve önceden yayınlanan sürümden [Application Insights](../../azure-monitor/app/app-insights-overview.md) telemetrisini karıştırmak istemezsiniz. Karışıklığın önüne geçmek için, farklı geliştirme aşamalarından Telemetriyi ayrı Application Insights kaynaklarına (ıkeys 'ler) göre ayrı olarak gönderin. Bir sürüm bir aşamadan diğerine geçiş yaparken izleme anahtarının değiştirilmesini kolaylaştırmak için, yapılandırma dosyası yerine koddaki Ikey ' i ayarlamak yararlı olabilir. 

(Sisteminiz bir Azure bulut hizmeti ise [ayrı ıkeys 'leri ayarlamaya yönelik başka bir yöntem](../../azure-monitor/app/cloudservices.md)vardır.)

## <a name="about-resources-and-instrumentation-keys"></a>Kaynaklar ve izleme anahtarları hakkında

Web uygulamanız için Application Insights izlemeyi ayarlarken Microsoft Azure ' de bir Application Insights *kaynağı* oluşturursunuz. Uygulamanızdan toplanan Telemetriyi görmek ve analiz etmek için Azure portal bu kaynağı açarsınız. Kaynak bir *izleme anahtarı* (Ikey) ile tanımlanır. Uygulamanızı izlemek için Application Insights paketini yüklediğinizde, Telemetriyi nereye gönderileceğini bilmesi için izleme anahtarıyla yapılandırırsınız.

Genellikle farklı senaryolarda ayrı kaynaklar veya tek bir paylaşılan kaynak kullanmayı tercih edersiniz:

* Farklı, bağımsız uygulamalar-her uygulama için ayrı bir kaynak ve Ikey kullanın.
* Birden çok bileşen veya bir iş uygulaması rolü-tüm bileşen uygulamaları için [tek bir paylaşılan kaynak](../../azure-monitor/app/app-map.md) kullanın. Telemetri cloud_RoleName özelliğine göre filtrelenebilir veya kesimlenebilir.
* Geliştirme, test ve yayın-' damga ' veya üretim aşamasında sistemin sürümleri için ayrı bir kaynak ve Ikey kullanın.
* A | B testi-tek bir kaynak kullanın. Telemetrileri tanımlayan bir özelliği bir özellik eklemek için Telemetryınitializer oluşturun.


## <a name="dynamic-instrumentation-key"></a><a name="dynamic-ikey"></a>Dinamik izleme anahtarı

Kod üretim aşamaları arasında taşındığı için Ikey 'in değiştirilmesini kolaylaştırmak için yapılandırma dosyası yerine kodu olarak ayarlayın.

Bir ASP.NET hizmetinde global.aspx.cs gibi bir başlatma yönteminde anahtarı ayarlayın:

*, #*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

Bu örnekte, farklı kaynaklar için ıkeys 'ler Web yapılandırma dosyasının farklı sürümlerine yerleştirilir. Yayın betiğinin bir parçası olarak yapabileceğiniz Web yapılandırma dosyasını değiştirme-hedef kaynağı takas eder.

### <a name="web-pages"></a>Web sayfaları
Ikey, uygulamanızın Web sayfalarında [hızlı başlangıç dikey penceresinden aldığınız betikte](../../azure-monitor/app/javascript.md)de kullanılır. Komut dosyasına tam olarak kodlamak yerine sunucu durumundan oluşturun. Örneğin, bir ASP.NET uygulamasında:

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
Farklı uygulama bileşenlerine veya aynı bileşenin farklı damgalar (geliştirme ve test/üretim) için telemetri ayırmak üzere yeni bir Application Insights kaynağı oluşturmanız gerekir.

[Portal.Azure.com](https://portal.azure.com)bir Application Insights kaynağı ekleyin:

![Yeni, Application Insights öğesine tıklayın](./media/separate-resources/01-new.png)

* **Uygulama türü** , genel bakış dikey penceresinde gördüklerinizi ve [Ölçüm Gezgini](../../azure-monitor/platform/metrics-charts.md)'nde kullanılabilen özellikleri etkiler. Uygulama türünü görmüyorsanız, Web sayfaları için Web türlerinden birini seçin.
* **Kaynak grubu** , [Access Control](../../azure-monitor/app/resources-roles-access-control.md)gibi özellikleri yönetmeye kolaylık vardır. Geliştirme, test ve üretim için ayrı kaynak grupları kullanabilirsiniz.
* **Abonelik** , Azure 'daki ödeme hesabıdır.
* **Konum** , verilerinizi nerede tutacağız. Şu anda değiştirilemez. 
* **Panoya Ekle** , Azure giriş sayfanıza kaynağınız için hızlı erişim kutucuğu koyar. 

Kaynağı oluşturmak birkaç saniye sürer. İşiniz bittiğinde bir uyarı görürsünüz.

(Bir kaynağı otomatik olarak oluşturmak için bir [PowerShell betiği](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource#creating-a-resource-automatically) yazabilirsiniz.)

### <a name="getting-the-instrumentation-key"></a>İzleme anahtarını alma
İzleme anahtarı, oluşturduğunuz kaynağı tanımlar. 

![Temel bileşenler ' e tıklayın, Izleme tuşuna tıklayın, CTRL + C](./media/separate-resources/02-props.png)

Uygulamanızın veri göndereceği tüm kaynakların izleme anahtarlarına ihtiyacınız vardır.

## <a name="filter-on-build-number"></a>Derleme numarasını filtrele
Uygulamanızın yeni bir sürümünü yayımladığınızda, farklı yapılardan Telemetriyi ayırabilmek isteyeceksiniz.

Uygulama sürümü özelliğini, [arama](../../azure-monitor/app/diagnostic-search.md) ve [Ölçüm Gezgini](../../azure-monitor/platform/metrics-charts.md) sonuçlarını filtreleyebilmeniz için ayarlayabilirsiniz.

![Bir özellik üzerinde filtreleme](./media/separate-resources/050-filter.png)

Uygulama sürümü özelliğini ayarlamanın birkaç farklı yöntemi vardır.

* Doğrudan ayarla:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* Tüm TelemetryClient örneklerinin tutarlı şekilde ayarlandığından emin olmak için bu satırı bir [telemetri başlatıcısında](../../azure-monitor/app/api-custom-events-metrics.md#defaults) sarın.
* [ASP.NET] Sürümünü ' de `BuildInfo.config`ayarlayın. Web modülü, BuildLabel düğümünden sürümü alacak. Bu dosyayı projenize ekleyin ve Çözüm Gezgini her zaman Kopyala özelliğini ayarlamayı unutmayın.

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
* [ASP.NET] MSBuild 'te Builınfo. config dosyasını otomatik olarak oluşturun. Bunu yapmak için `.csproj` dosyanıza birkaç satır ekleyin:

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Bu, *yourprojectname*adlı bir dosya oluşturur. Builınfo. config. Yayımla işlemi onu Builınfo. config olarak yeniden adlandırır.

    Visual Studio ile derleme sırasında derleme etiketi bir yer tutucu (AutoGen_...) içerir. Ancak MSBuild ile birlikte kullanıldığında, doğru sürüm numarasıyla doldurulur.

    MSBuild 'in sürüm numaraları oluşturmasına izin vermek için AssemblyReference.cs içindeki gibi `1.0.*` bir sürümü ayarlayın

## <a name="version-and-release-tracking"></a>Sürüm ve sürüm izleme
Uygulama sürümünü izlemek için `buildinfo.config` dosyasının Microsoft Build Engine işleminiz tarafından oluşturulduğundan emin olun. `.csproj` Dosyanıza şunu ekleyin:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Yapı bilgisi mevcut olduğunda Application Insights web modülü **Uygulama sürümünü** telemetrinin her bir öğesine bir özellik olarak ekler. Bu sayede, [tanılama aramaları](../../azure-monitor/app/diagnostic-search.md) gerçekleştirirken veya [ölçümleri keşfederken](../../azure-monitor/platform/metrics-charts.md) sürüme göre filtreleyebilirsiniz.

Ancak, derleme sürüm numarasının Visual Studio 'daki geliştirici derlemesi tarafından değil, yalnızca Microsoft Build Engine tarafından oluşturulup oluşturulduğuna dikkat edin.

### <a name="release-annotations"></a>Sürüm ek açıklamaları
Azure DevOps kullanıyorsanız, yeni bir sürüm yayınlayışınızda grafiklerinize [ek açıklama işaretleyicisi](../../azure-monitor/app/annotations.md) ekleyebilirsiniz. Aşağıdaki görüntüde bu işaretin nasıl göründüğü gösterilmiştir.

![Bir grafikteki örnek sürüm ek açıklamasının ekran görüntüsü](media/separate-resources/release-annotation.png)
## <a name="next-steps"></a>Sonraki adımlar

* [Birden çok rol için paylaşılan kaynaklar](../../azure-monitor/app/app-map.md)
* [Ayırt etmek için telemetri başlatıcısı oluşturma | B çeşitleri](../../azure-monitor/app/api-filtering-sampling.md#add-properties)
