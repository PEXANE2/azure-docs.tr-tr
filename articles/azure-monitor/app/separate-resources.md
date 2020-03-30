---
title: Azure Uygulama Öngörülerinde telemetriyi ayırma
description: Geliştirme, test ve üretim pulları için farklı kaynaklara doğrudan telemetri.
ms.topic: conceptual
ms.date: 05/15/2017
ms.openlocfilehash: 3580d162f4b3955a04ffcd0f13933221bfef3b65
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671469"
---
# <a name="separating-telemetry-from-development-test-and-production"></a>Telemetriyi Geliştirme, Test ve Üretimden Ayırma

Bir web uygulamasının bir sonraki sürümünü geliştirirken, yeni sürümden ve zaten yayımlanmış sürümden [Application Insights](../../azure-monitor/app/app-insights-overview.md) telemetrisini karıştırmak istemezsinuz. Karışıklığı önlemek için, farklı geliştirme aşamalarından telemetriyi ayrı enstrümantasyon anahtarları (ituşları) ile Application Insights kaynaklarını ayırmak için gönderin. Bir sürüm bir aşamadan diğerine taşınırken enstrümantasyon anahtarını değiştirmeyi kolaylaştırmak için, yapılandırma dosyasıyerine ikey'i kodolarak ayarlamak yararlı olabilir. 

(Sisteminiz bir Azure Bulut Hizmetiise, [ayrı ikeys ayarlamanın başka bir yöntemi vardır.)](../../azure-monitor/app/cloudservices.md)

## <a name="about-resources-and-instrumentation-keys"></a>Kaynaklar ve enstrümantasyon tuşları hakkında

Web uygulamanız için Application Insights izleme yi ayarladığınızda, Microsoft Azure'da bir Application Insights *kaynağı* oluşturursunuz. Uygulamanızdan toplanan telemetriyi görmek ve analiz etmek için bu kaynağı Azure portalında açarsınız. Kaynak bir *enstrümantasyon anahtarı* (ikey) ile tanımlanır. Uygulamanızı izlemek için Application Insights paketini yüklediğinizde, telemetriyi nereye göndereceğini bilmesi için uygulama landırma tuşuyla yapılandırabilirsiniz.

Genellikle farklı senaryolarda ayrı kaynaklar veya tek bir paylaşılan kaynak kullanmayı seçersiniz:

* Farklı, bağımsız uygulamalar - Her uygulama için ayrı bir kaynak ve ikey kullanın.
* Tek bir iş uygulamasının birden çok bileşeni veya rolü - Tüm bileşen uygulamaları için tek bir [paylaşılan kaynak](../../azure-monitor/app/app-map.md) kullanın. Telemetri, cloud_RoleName özelliği tarafından filtrelenebilir veya segmente edilebilir.
* Geliştirme, Test ve Sürüm - Sistemin 'damga' veya üretim aşamasındaki sürümleri için ayrı bir kaynak ve ikey kullanın.
* A | B testi - Tek bir kaynak kullanın. Türevlerini tanımlayan telemetriye özellik eklemek için bir TelemetryInitializer oluşturun.


## <a name="dynamic-instrumentation-key"></a><a name="dynamic-ikey"></a>Dinamik enstrümantasyon anahtarı

Kod üretim aşamaları arasında ilerlerken ikey'i değiştirmeyi kolaylaştırmak için, yapılandırma dosyasında değil koda ayarlayın.

Anahtarı, ASP.NET hizmetinde global.aspx.cs gibi bir başlatma yönteminde ayarlayın:

*C #*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

Bu örnekte, farklı kaynakların ituşları web yapılandırma dosyasının farklı sürümlerine yerleştirilir. Sürüm komut dosyasının bir parçası olarak yapabileceğiniz web yapılandırma dosyasını değiştirme hedef kaynağı değiştirir.

### <a name="web-pages"></a>Web sayfaları
iKey, uygulamanızın web sayfalarında, [quickstart blade'den aldığınız komut dosyasında](../../azure-monitor/app/javascript.md)da kullanılır. Bunun yerine kelimenin tam anlamıyla komut dosyasına kodlama, sunucu durumundan oluşturun. Örneğin, bir ASP.NET uygulamasında:

*Razor içinde JavaScript*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a>Ek Uygulama Öngörüleri kaynakları oluşturun
Farklı uygulama bileşenleri için telemetriyi ayırmak veya aynı bileşenin farklı pulları (dev/test/üretim) için yeni bir Uygulama Öngörüleri kaynağı oluşturmanız gerekir.

[portal.azure.com,](https://portal.azure.com)bir Application Insights kaynağı ekleyin:

![Yeni, Application Insights öğesine tıklayın](./media/separate-resources/01-new.png)

* **Uygulama türü,** genel bakış bıçağında gördüklerinizi ve [metrik gezginde](../../azure-monitor/app/metrics-explorer.md)bulunan özellikleri etkiler. Uygulama türünü görmüyorsanız, web sayfaları için web türlerinden birini seçin.
* **Kaynak grubu,** [erişim denetimi](../../azure-monitor/app/resources-roles-access-control.md)gibi özellikleri yönetmek için bir kolaylıktır. Geliştirme, test ve üretim için ayrı kaynak grupları kullanabilirsiniz.
* **Abonelik,** Azure'daki ödeme hesabınızdır.
* **Konum,** verilerinizi sakladığımız yerdir. Şu anda değiştirilemez. 
* **Panoya ekle,** Azure Giriş sayfanızda kaynağınız için hızlı erişim döşemesi koyar. 

Kaynağı oluşturmak birkaç saniye sürer. Bittiğinde bir uyarı görürsünüz.

(Otomatik olarak bir kaynak oluşturmak için bir [PowerShell komut dosyası](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource#creating-a-resource-automatically) yazabilirsiniz.)

### <a name="getting-the-instrumentation-key"></a>Enstrümantasyon anahtarını alma
Enstrümantasyon anahtarı oluşturduğunuz kaynağı tanımlar. 

![Essentials'a tıklayın, Enstrümantasyon Tuşu, CTRL+C'yi tıklatın](./media/separate-resources/02-props.png)

Uygulamanızın veri göndereceği tüm kaynakların enstrümantasyon tuşlarına ihtiyacınız var.

## <a name="filter-on-build-number"></a>Yapı numarasına filtre uygulayın
Uygulamanızın yeni bir sürümünü yayımladığınızda, telemetriyi farklı yapılardan ayırmak isteyebilirsiniz.

Uygulama Sürümü özelliğini, [arama](../../azure-monitor/app/diagnostic-search.md) ve metrik [gezgin](../../azure-monitor/app/metrics-explorer.md) sonuçlarına filtre uygulayabilmeniz için ayarlayabilirsiniz.

![Özellik üzerinde filtreleme](./media/separate-resources/050-filter.png)

Uygulama Sürümü özelliğini ayarlamanın birkaç farklı yöntemi vardır.

* Doğrudan ayarlayın:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* Tüm TelemetriIstemci örneklerinin tutarlı bir şekilde ayarlandığından emin olmak için bu satırı bir [telemetri baş harfine](../../azure-monitor/app/api-custom-events-metrics.md#defaults) sarın.
* [ASP.NET] Sürümü ' `BuildInfo.config`de ayarlayın Web modülü sürümü BuildLabel düğümünden alır. Bu dosyayı projenize ekleyin ve Çözüm Gezgini'nde Her Zaman Kopyala özelliğini ayarlamayı unutmayın.

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
* [ASP.NET] MSBuild'te BuildInfo.config'i otomatik olarak oluşturun. Bunu yapmak için dosyanıza `.csproj` birkaç satır ekleyin:

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Bu, *ProjectName*adlı bir dosya oluşturur. BuildInfo.config. Yayımlama işlemi buildInfo.config olarak yeniden adlandırır.

    Yapı etiketi, Visual Studio ile oluşturduğunuzda bir yer tutucu (AutoGen_...) içerir. Ama MSBuild ile inşa edildiğinde, doğru sürüm numarası ile doldurulur.

    MSBuild'in sürüm numaraları oluşturmasına izin `1.0.*` vermek için, AssemblyReference.cs

## <a name="version-and-release-tracking"></a>Sürüm ve sürüm izleme
Uygulama sürümünü izlemek için `buildinfo.config` dosyasının Microsoft Build Engine işleminiz tarafından oluşturulduğundan emin olun. Dosyanıza `.csproj` şunları ekleyin:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Yapı bilgisi mevcut olduğunda Application Insights web modülü **Uygulama sürümünü** telemetrinin her bir öğesine bir özellik olarak ekler. Bu sayede, [tanılama aramaları](../../azure-monitor/app/diagnostic-search.md) gerçekleştirirken veya [ölçümleri keşfederken](../../azure-monitor/app/metrics-explorer.md) sürüme göre filtreleyebilirsiniz.

Ancak, yapı sürüm numarasının Visual Studio'daki geliştirici yapısı tarafından değil, yalnızca Microsoft Build Engine tarafından oluşturulduğuna dikkat edin.

### <a name="release-annotations"></a>Sürüm ek açıklamaları
Azure DevOps kullanıyorsanız, yeni bir sürüm yayınladığınızda grafiklerinize [bir ek açıklama işaretçisi](../../azure-monitor/app/annotations.md) ekleyebilirsiniz. Aşağıdaki görüntüde bu işaretin nasıl göründüğü gösterilmiştir.

![Bir grafikteki örnek sürüm ek açıklamasının ekran görüntüsü](media/separate-resources/release-annotation.png)
## <a name="next-steps"></a>Sonraki adımlar

* [Birden çok rol için paylaşılan kaynaklar](../../azure-monitor/app/app-map.md)
* [Create a Telemetry Initializer to distinguish A| B varyantları](../../azure-monitor/app/api-filtering-sampling.md#add-properties)
