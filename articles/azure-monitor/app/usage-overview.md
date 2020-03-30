---
title: Azure Uygulama Öngörüleri ile kullanım analizi | Microsoft dokümanlar
description: Kullanıcılarınızı ve uygulamanızla ne yaptıklarını anlayın.
ms.topic: conceptual
ms.date: 03/25/2019
ms.openlocfilehash: e964b1b5b9d5500f2d9f24ed765299389e6dbbb9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283965"
---
# <a name="usage-analysis-with-application-insights"></a>Application Insights ile kullanım analizi

Web veya mobil uygulamanızın hangi özellikleri en popüler? Kullanıcılarınız uygulamanızla hedeflerine uymurttmü? Belirli noktalarda vazgeçip daha sonra geri mi dönüyorlar?  [Azure Application Insights,](../../azure-monitor/app/app-insights-overview.md) insanların uygulamanızı nasıl kullandığına dair güçlü bilgiler edinmenize yardımcı olur. Uygulamanızı her güncellediğiniz de kullanıcılar için ne kadar iyi çalıştığını değerlendirebilirsiniz. Bu bilgiyle, bir sonraki geliştirme döngüleriniz hakkında veri odaklı kararlar verebilirsiniz.

## <a name="send-telemetry-from-your-app"></a>Uygulamanızdan telemetri gönderin

En iyi deneyim, Application Insights'ı hem uygulama sunucu kodunuza hem de web sayfalarınızda yükleyerek elde edilir. Uygulamanızın istemci ve sunucu bileşenleri telemetriyi analiz için Azure portalına geri gönderir.

1. **Sunucu kodu:** [ASP.NET,](../../azure-monitor/app/asp-net.md) [Azure,](../../azure-monitor/app/app-insights-overview.md) [Java,](../../azure-monitor/app/java-get-started.md) [Düğüm.js](../../azure-monitor/app/nodejs.md)veya [diğer](../../azure-monitor/app/platforms.md) uygulamalarınız için uygun modülü yükleyin.

    * *Sunucu kodu yüklemek istemiyor musunuz? [Bir Azure Uygulama Öngörüleri kaynağı oluşturmanıza devam edin.](../../azure-monitor/app/create-new-resource.md )*

2. **Web sayfası kodu:** Kapanışa ``</head>``kadar web sayfanıza aşağıdaki komut dosyasını ekleyin. Enstrümantasyon anahtarını Application Insights kaynağınız için uygun değerle değiştirin:
    
    ```html
    <script type="text/javascript">
    var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t}(
    {
      instrumentationKey:"INSTRUMENTATION_KEY"
    }
    );window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
    </script>
    ```

    Web sitelerini izlemek için daha gelişmiş yapılandırmalar öğrenmek için [JavaScript SDK başvuru makalesine](https://docs.microsoft.com/azure/azure-monitor/app/javascript)göz atın.

3. **Mobil uygulama kodu:** Uygulamanızdan olayları toplamak için App Center SDK'yı kullanın ve bu olayların kopyalarını [bu kılavuzu izleyerek](../../azure-monitor/learn/mobile-center-quickstart.md)analiz için Application Insights'a gönderin.

4. **Telemetri alın:** Projenizi hata ayıklama modunda birkaç dakika çalıştırın ve ardından Uygulama Öngörüleri'ndeki Genel Bakış bıçağındaki sonuçları arayın.

    Uygulamanızın performansını izlemek ve kullanıcılarınızın uygulamanızla ne yaptığını öğrenmek için uygulamanızı yayınlayın.

## <a name="include-user-and-session-id-in-your-telemetry"></a>Telemetrinize kullanıcı ve oturum kimliği ekleme
Zaman içinde kullanıcıları izlemek için Application Insights,onları tanımlamak için bir yol gerektirir. Etkinlikler aracı, kullanıcı kimliği veya oturum kimliği gerektirmeyen tek Kullanım aracıdır.

[Bu işlemi](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context)kullanarak kullanıcı ve oturum dislerini göndermeye başlayın.

## <a name="explore-usage-demographics-and-statistics"></a>Kullanım demografisini ve istatistikleri keşfedin
Kullanıcıların uygulamanızı ne zaman kullandığını, en çok hangi sayfaları ilgilendiklerini, kullanıcılarınızın nerede bulunduğunu, hangi tarayıcıları ve işletim sistemlerini kullandıklarını öğrenin. 

Kullanıcılar ve Oturumlar raporları verilerinizi sayfalara veya özel olaylara göre filtreler ve konum, ortam ve sayfa gibi özelliklere göre bölümlere ayırın. Kendi filtrelerinizi de ekleyebilirsiniz.

![Kullanıcılar](./media/usage-overview/users.png)  

Sağdaki öngörüler, veri kümesindeki ilginç desenlere işaret eder.  

* **Kullanıcılar** raporu, seçtiğiniz süreler içinde sayfalarınıza erişen benzersiz kullanıcıların sayısını sayar. Web uygulamaları için kullanıcılar çerezleri kullanarak sayılır. Birisi sitenize farklı tarayıcılar veya istemci makineleriyle erişir veya çerezlerini temizlerse, birden fazla kez sayılır.
* **Oturumlar** raporu, sitenize erişen kullanıcı oturumlarının sayısını sayar. Oturum, kullanıcı tarafından yarım saatten fazla bir süre yle sonlandırılan bir etkinlik dönemidir.

[Kullanıcılar, Oturumlar ve Etkinlikler araçları hakkında daha fazla şey](usage-segmentation.md)  

## <a name="retention---how-many-users-come-back"></a>Bekletme - kaç kullanıcı geri geliyor?

Bekletme, belirli bir zaman kovası sırasında bazı iş eylemlerini gerçekleştiren kullanıcı kohortlarına dayanarak, kullanıcılarınızın uygulamalarını kullanmak için ne sıklıkta geri döndüğünü anlamanıza yardımcı olur. 

- Belirli özelliklerin kullanıcıların diğerlerinden daha fazla geri gelmesine neden olduğunu anlama 
- Gerçek kullanıcı verilerine dayalı hipotezler oluşturma 
- Bekletme ürününüzün bir sorun olup olmadığını belirleme 

![Bekletme](./media/usage-overview/retention.png) 

Üstteki bekletme denetimleri, bekletmeyi hesaplamak için belirli olayları ve zaman aralığını tanımlamanıza olanak sağlar. Ortadaki grafik, genel bekletme yüzdesinin belirtilen zaman aralığına göre görsel bir gösterimini verir. Alttaki grafik, belirli bir zaman diliminde tek tek bekletme temsil eder. Bu ayrıntı düzeyi, kullanıcılarınızın ne yaptığını ve geri dönen kullanıcıları daha ayrıntılı bir ayrıntılandırmada nelerin etkileyebileceğini anlamanızı sağlar.  

[Bekletme aracı hakkında daha fazla şey](usage-retention.md)

## <a name="custom-business-events"></a>Özel iş etkinlikleri

Kullanıcıların uygulamanızla ne yaptıklarını net bir şekilde anlamak için, özel olayları günlüğe kaydetmek için kod satırları eklemek yararlıdır. Bu olaylar, belirli düğmeleri tıklatma gibi ayrıntılı kullanıcı eylemlerinden, satın alma veya oyun kazanma gibi daha önemli iş etkinliklerine kadar her şeyi izleyebilir. 

Bazı durumlarda, sayfa görünümleri yararlı olayları temsil edebilir, ancak genel olarak doğru değildir. Kullanıcı ürünü satın almadan bir ürün sayfası açabilir. 

Belirli iş etkinliklerinde, kullanıcılarınızın sitenizdeki ilerlemesini grafikleyebilirsiniz. Farklı seçenekler için tercihlerini ve nerede bıraktıklarını veya zorlukla karşı laçlarını öğrenebilirsiniz. Bu bilgiyle, geliştirme biriktirme listenizdeki öncelikler hakkında bilinçli kararlar verebilirsiniz.

Olaylar uygulamanın istemci tarafından günlüğe kaydedilebilir:

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

Veya sunucu tarafından:

```csharp
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

Bu olaylara özellik değerleri ekleyebilirsiniz, böylece portalda incelediğinizde olayları filtreleyebilir veya bölebilirsiniz. Ayrıca, her olaya, tek bir kullanıcının etkinlik sırasını izlemenize olanak tanıyan anonim kullanıcı kimliği gibi standart bir özellik kümesi eklenir.

[Özel olaylar](../../azure-monitor/app/api-custom-events-metrics.md#trackevent) ve [özellikler](../../azure-monitor/app/api-custom-events-metrics.md#properties)hakkında daha fazla bilgi edinin.

### <a name="slice-and-dice-events"></a>Dilim ve zar olayları

Kullanıcılar, Oturumlar ve Etkinlikler araçlarında, özel olayları kullanıcıya, etkinlik adına ve özelliklerine göre dilimleyebilir ve zarlayabilirsiniz.
![Kullanıcılar](./media/usage-overview/users.png)  
  
## <a name="design-the-telemetry-with-the-app"></a>Uygulama ile telemetri tasarla

Uygulamanızın her bir özelliğini tasarlarken, uygulamanızın başarısını kullanıcılarınızla nasıl ölçeceğinize göz önünde bulundurun. Hangi iş etkinliklerini kaydetmeniz gerektiğine karar verin ve bu etkinliklere yönelik izleme çağrılarını en baştan uygulamanıza kodlayın.

## <a name="a--b-testing"></a>A | B Testi
Bir özelliğin hangi varyantının daha başarılı olacağını bilmiyorsanız, her ikisini de serbest bırakarak her birini farklı kullanıcılar için erişilebilir hale getirin. Her birinin başarısını ölçün ve ardından birleşik bir sürüme geçin.

Bu teknik için, uygulamanızın her sürümü tarafından gönderilen tüm telemetrilere farklı özellik değerleri eklersiniz. Bunu etkin TelemetryContext'daki özellikleri tanımlayarak yapabilirsiniz. Bu varsayılan özellikler, uygulamanın gönderdiği her telemetri iletisine eklenir - sadece özel iletileriniz değil, standart telemetri de.

Application Insights portalında, farklı sürümleri karşılaştırmak için verilerinizi özellik değerlerine filtreuygulayın ve bölün.

Bunu yapmak için, [bir telemetri başharfi ayarlayın:](../../azure-monitor/app/api-filtering-sampling.md#addmodify-properties-itelemetryinitializer)

**ASP.NET uygulamaları**

```csharp
    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry item)
            {
                var itemProperties = item as ISupportProperties;
                if (itemProperties != null && !itemProperties.Properties.ContainsKey("AppVersion"))
                {
                    itemProperties.Properties["AppVersion"] = "v2.1";
                }
            }
    }
```

Global.asax.cs gibi web uygulaması baş harflerinde:

```csharp

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
         .Add(new MyTelemetryInitializer());
    }
```

**ASP.NET Core uygulamaları**

> [!NOTE]
> Kullanarak veya kullanarak `ApplicationInsights.config` `TelemetryConfiguration.Active` baş harf ekleme ASP.NET Core uygulamaları için geçerli değildir. 

[ASP.NET Core](asp-net-core.md#adding-telemetryinitializers) uygulamaları için, `TelemetryInitializer` aşağıda gösterildiği gibi Bağımlılık Enjeksiyonu kabına ekleyerek yeni bir ekleme yapılır. Bu sizin `Startup.cs` `ConfigureServices` sınıf yöntemi yapılır.

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;
 public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<ITelemetryInitializer, MyTelemetryInitializer>();
}
```

Tüm yeni TelemetryClients otomatik olarak belirttiğiniz özellik değerini ekleyin. Tek tek telemetri olayları varsayılan değerleri geçersiz kılabilir.

## <a name="next-steps"></a>Sonraki adımlar
   - [Kullanıcılar, Oturumlar, Etkinlikler](usage-segmentation.md)
   - [Huniler](usage-funnels.md)
   - [Bekletme](usage-retention.md)
   - [Kullanıcı Akışları](usage-flows.md)
   - [Çalışma Kitapları](../../azure-monitor/app/usage-workbooks.md)
   - [Kullanıcı bağlamı ekleme](usage-send-user-context.md)
