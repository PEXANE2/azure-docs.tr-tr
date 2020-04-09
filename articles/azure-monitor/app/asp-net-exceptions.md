---
title: Azure Uygulama Öngörüleri ile hataları ve özel durumları tanılama
description: İstek telemetrisiyle birlikte ASP.NET uygulamalardan istisnaları yakalayın.
ms.topic: conceptual
ms.date: 07/11/2019
ms.openlocfilehash: 9f24f09e7d2ef0a3e5f3a8f6546a9115118473ab
ms.sourcegitcommit: df8b2c04ae4fc466b9875c7a2520da14beace222
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80892351"
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a>Application Insights ile web uygulamalarınızda özel durumları tanılama
Canlı web uygulamanızdaki istisnalar [Application Insights](../../azure-monitor/app/app-insights-overview.md)tarafından raporlanır. Başarısız istekleri, nedenleri hızlı bir şekilde tanılayabilmeniz için hem istemcide hem de sunucudaki özel durumlar ve diğer olaylarla ilişkilendirebilirsiniz.

## <a name="set-up-exception-reporting"></a>Özel durum raporlamasını ayarlama
* Sunucu uygulamanızdan bildirilen özel durumlar için:
  * Azure web uygulamaları: [Uygulama Öngörüleri Uzantısı](../../azure-monitor/app/azure-web-apps.md) ekle
  * Azure VM ve Azure sanal makine ölçeği, IIS tarafından barındırılan uygulamaları ayarlar: [Uygulama İzleme Uzantısını](../../azure-monitor/app/azure-vm-vmss-apps.md) ekleyin
  * [Uygulama Öngörüleri SDK'yı](../../azure-monitor/app/asp-net.md) uygulama kodunuza yükleyin veya
  * IIS web sunucuları: [Uygulama Öngörüleri Aracısı](../../azure-monitor/app/monitor-performance-live-website-now.md)Çalıştır ; Veya
  * Java web uygulamaları: [Java aracısını](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent) etkinleştirin
* Tarayıcı özel durumlarını yakalamak için web sayfalarınızda [JavaScript snippet'ini](../../azure-monitor/app/javascript.md) yükleyin.
* Bazı uygulama çerçevelerinde veya bazı ayarlarda, daha fazla özel durum yakalamak için bazı ek adımlar atmanız gerekir:
  * [Web formları](#web-forms)
  * [MVC](#mvc)
  * [Web API 1.*](#web-api-1x)
  * [Web API 2.*](#web-api-2x)
  * [WCF](#wcf)

  Bu makale, özellikle kod örneği açısından .NET Framework uygulamalarına odaklanmıştır. .NET Framework için çalışan yöntemlerden bazıları .NET Core SDK'da kullanılmaz. Bir .NET Core uygulamanız varsa [.NET Core SDK belgelerine](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) bakın.

## <a name="diagnosing-exceptions-using-visual-studio"></a>Visual Studio kullanarak özel durumları tanılama
Hata ayıklama konusunda yardımcı olmak için Visual Studio'da uygulama çözümlerini açın.

F5 kullanarak uygulamayı sunucunuzda veya geliştirme makinenizde çalıştırın.

Visual Studio'da Uygulama Öngörüleri Arama penceresini açın ve uygulamanızdaki etkinlikleri görüntülemek üzere ayarlayın. Hata ayıklama yaparken, bunu sadece Uygulama İstatistikleri düğmesini tıklatarak yapabilirsiniz.

![Projeyi sağ tıklatın ve Application Insights, Open'ı seçin.](./media/asp-net-exceptions/34.png)

Raporu sadece özel durumları göstermek için filtreleyebilirsiniz dikkat edin.

*Hiçbir istisna gösteren? Bkz. [Yakalama özel durumları.](#exceptions)*

Yığın izini göstermek için bir özel durum raporunu tıklatın.
İlgili kod dosyasını açmak için yığın takibinde bir satır başvurusu'nu tıklatın.

Kodda, CodeLens'in istisnalarla ilgili verileri gösterdiğine dikkat edin:

![CodeLens istisnalar bildirimi.](./media/asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-the-azure-portal"></a>Azure portalını kullanarak hataları tanılama
Application Insights, izlenen uygulamalarınızdaki hataları tanılamanıza yardımcı olacak bir küratörlü APM deneyimiyle birlikte gelir. Başlamak için, Araştır bölümünde bulunan Application Insights kaynak menüsündeki Hatalar seçeneğini tıklatın.
İstekleriniz için hata oranı eğilimlerini, kaç tanesinin başarısız olduğunu ve kaç kullanıcının etkilendiğini gösteren tam ekran bir görünüm görmeniz gerekir. Sağda, en iyi üç yanıt kodu, en iyi üç özel durum türü ve en iyi üç başarısız bağımlılık türü de dahil olmak üzere seçili başarısız işlem için özel en yararlı dağıtımlardan bazılarını görürsünüz.

![Hatalar triyaj görünümü (işlemler sekmesi)](./media/asp-net-exceptions/failures0719.png)

Tek bir tıklamayla, bu işlem alt kümelerinin her biri için temsili örnekleri gözden geçirebilirsiniz. Özellikle, özel durumları tanılamak için, bu gibi uçtan uca işlem ayrıntıları sekmesiyle birlikte sunulacak belirli bir özel durum sayısını tıklatabilirsiniz:

![Uçuca işlem ayrıntıları sekmesi](./media/asp-net-exceptions/end-to-end.png)

**Alternatif olarak,** belirli bir başarısız işlemin özel durumlarına bakmak yerine, en üstteki Özel Durumlar sekmesine geçerek özel durumların genel görünümünden başlayabilirsiniz. Burada izlenen uygulamanız için toplanan tüm özel durumları görebilirsiniz.

*Hiçbir istisna gösteren? Bkz. [Yakalama özel durumları.](#exceptions)*


## <a name="custom-tracing-and-log-data"></a>Özel izleme ve günlük verileri
Uygulamanıza özgü tanılama verilerini almak için, kendi telemetri verilerinizi göndermek için kod ekleyebilirsiniz. Bu istek, sayfa görünümü ve diğer otomatik olarak toplanan verilerin yanında tanılama arama görüntülenir.

Birkaç seçeneğiniz vardır:

* [TrackEvent()](../../azure-monitor/app/api-custom-events-metrics.md#trackevent) genellikle kullanım desenlerini izlemek için kullanılır, ancak gönderdiği veriler tanılama aramasında Özel Olaylar altında da görünür. Olaylar adlandırılmış ve [tanılama aramalarınızı](../../azure-monitor/app/diagnostic-search.md)filtreleyebileceğiniz dize özellikleri ve sayısal ölçümler taşıyabilir.
* [TrackTrace()](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace) POST bilgileri gibi daha uzun veri göndermenizi sağlar.
* [TrackException()](#exceptions) yığın izlerini gönderir. [İstisnalar hakkında daha fazla.](#exceptions)
* Log4Net veya NLog gibi bir günlük çerçevesi zaten kullanıyorsanız, [bu günlükleri yakalayabilir](asp-net-trace-logs.md) ve istek ve özel durum verilerinin yanı sıra tanılama aramasında görebilirsiniz.

Bu olayları görmek için sol menüden [Ara'yı](../../azure-monitor/app/diagnostic-search.md) açın, açılan menü **Olay türlerini**seçin ve ardından Özel Olay, İzleme veya Özel Durum'u seçin.

![Detaylandırma](./media/asp-net-exceptions/customevents.png)

> [!NOTE]
> Uygulamanız çok sayıda telemetri oluşturuyorsa, uyarlamalı örnekleme modülü olayların yalnızca bir temsilci fraksiyonunu göndererek portala gönderilen hacmi otomatik olarak azaltır. İlgili olaylar arasında gezinmeniz için aynı işlemin parçası olan olaylar grup olarak seçilir veya seçilir. [Örnekleme hakkında bilgi edinin.](../../azure-monitor/app/sampling.md)
>
>

### <a name="how-to-see-request-post-data"></a>İstek SONRASı verilerini görme
İstek ayrıntıları, UYGULAMA ARAMAsInda uygulamanız için gönderilen verileri içermez. Bu verilerin raporolması için:

* Uygulama projenize [SDK'yı yükleyin.](../../azure-monitor/app/asp-net.md)
* [Microsoft.ApplicationInsights.TrackTrace()](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace)numaralı telefonu aramak için uygulamanıza kod ekleyin. İleti parametresi içinde POST verilerini gönderin. İzin verilen boyutun bir sınırı vardır, bu nedenle yalnızca temel verileri göndermeyi denemelisiniz.
* Başarısız bir isteği araştırdığınızda, ilişkili izleri bulun.

## <a name="capturing-exceptions-and-related-diagnostic-data"></a><a name="exceptions"></a>Özel durumları ve ilgili tanılama verilerini yakalama
İlk başta, uygulamada hatalara neden olan tüm özel durumları portalda görmezsiniz. Tarayıcı özel durumlarını görürsünüz (web sayfalarınızda [JavaScript SDK](../../azure-monitor/app/javascript.md) kullanıyorsanız). Ama çoğu sunucu özel durumları IIS tarafından yakalanır ve bunları görmek için kod biraz yazmak zorunda.

Şunları yapabilirsiniz:

* Özel durumları bildirmek için özel durum işleyicilerine kod ekleyerek **özel durumları açıkça günlüğe** kaydedin.
* ASP.NET çerçevenizi yapılandırarak **özel durumları otomatik olarak yakalayın.** Gerekli eklemeler farklı çerçeve türleri için farklıdır.

## <a name="reporting-exceptions-explicitly"></a>Özel durumları açıkça bildirme
En basit yol, bir özel durum işleyicisi trackexception() için bir çağrı eklemektir.

```javascript
    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }
```

```csharp
    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send the exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }
```

```VB
    Dim telemetry = New TelemetryClient
    ...
    Try
      ...
    Catch ex as Exception
      ' Set up some properties:
      Dim properties = New Dictionary (Of String, String)
      properties.Add("Game", currentGame.Name)

      Dim measurements = New Dictionary (Of String, Double)
      measurements.Add("Users", currentGame.Users.Count)

      ' Send the exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try
```

Özellikler ve ölçüm parametreleri isteğe bağlıdır, ancak [filtreleme ve](../../azure-monitor/app/diagnostic-search.md) ek bilgi eklemek için yararlıdır. Örneğin, birden çok oyun çalıştırabilen bir uygulamanız varsa, belirli bir oyunla ilgili tüm özel durum raporlarını bulabilirsiniz. Her sözlük için istediğiniz kadar öğe ekleyebilirsiniz.

## <a name="browser-exceptions"></a>Tarayıcı özel durumları
Çoğu tarayıcı özel durumu bildirilir.

Web sayfanızda içerik dağıtım ağlarından veya diğer etki alanlarından gelen komut ```crossorigin="anonymous"```dosyası dosyaları bulunuyorsa, komut dosyası etiketinizin özniteliğe sahip olduğundan ve sunucunun [CORS üstbilgilerini](https://enable-cors.org/)gönderdiğinden emin olun. Bu, bu kaynaklardan işlenmemiş JavaScript özel durumları için bir yığın izleme ve ayrıntı elde etmek için izin verecektir.

## <a name="reuse-your-telemetry-client"></a>Telemetri istemcinizi yeniden kullanın

> [!NOTE]
> TelemetriClient'ın bir uygulamanın ömrü boyunca anlık olarak kullanılması ve yeniden kullanılması önerilir.

Aşağıda Doğru TelemetryClient kullanarak bir örnektir.

```csharp
public class GoodController : ApiController
{
    // OK
    private static readonly TelemetryClient telemetryClient;

    static GoodController()
    {
        telemetryClient = new TelemetryClient();
    }
}
```


## <a name="web-forms"></a>Web formları
Web formları için, ÖZEL Hatalar ile yapılandırılan yönlendirmeler olmadığında, HTTP Modülü özel durumları toplayabilecektir.

Ancak etkin yönlendirmeler varsa, Global.asax.cs'daki Application_Error işlevine aşağıdaki satırları ekleyin. (Zaten bir dosyanız yoksa Global.asax dosyası ekleyin.)

```csharp
    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }
```
## <a name="mvc"></a>MVC
Application Insights Web SDK sürüm 2.6 (beta3 ve sonraki sürüm) ile başlayarak, Application Insights MVC 5+ denetleyicileri yöntemlerine otomatik olarak atılan işlenmemiş özel durumları toplar. Bu tür özel durumları izlemek için daha önce özel bir işleyici eklediyseniz (aşağıdaki örneklerde açıklandığı gibi), özel durumların çift kez izlenmesini önlemek için bu özel durumu kaldırabilirsiniz.

Özel durum filtrelerinin işleyemediği birkaç servis vardır. Örneğin:

* Denetleyici oluşturuculardan atılan özel durumlar.
* İleti işleyicilerinden atılan özel durumlar.
* Yönlendirme sırasında atılan özel durumlar.
* Yanıt içeriği serileştirme sırasında atılan özel durumlar.
* Uygulama başlatma sırasında atılan özel durum.
* Arka plan görevlerinde atılan özel durum.

Uygulama tarafından *işlenen* tüm özel durumların yine de el ile izlenmesi gerekir.
Denetleyicilerden kaynaklanan işlenmemiş özel durumlar genellikle 500 "İç Sunucu Hatası" yanıtıyla sonuçlanır. Bu tür bir yanıt, işlenen özel durum (veya hiç istisna olmayan) sonucunda el ile `ResultCode` oluşturulursa, 500 ile ilgili istek telemetrisinde izlenir, ancak Application Insights SDK ilgili özel durumu izleyemez.

### <a name="prior-versions-support"></a>Önceki sürümler desteği
Application Insights Web SDK 2.5'in (ve öncesinde) MVC 4'ü (ve önceki) kullanıyorsanız, özel durumları izlemek için aşağıdaki örneklere bakın.

[CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) yapılandırması `Off`ise, [http modülünün](https://msdn.microsoft.com/library/ms178468.aspx) toplanması için özel durumlar kullanılabilir. Ancak, `RemoteOnly` (varsayılan) veya `On`, sonra özel durum temizlenir ve Uygulama Öngörüleri için otomatik olarak toplamak için kullanılamaz. [Bunu System.Web.Mvc.HandleErrorAttribute sınıfını](https://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx)geçersiz kılarak ve aşağıdaki farklı MVC sürümlerinde gösterildiği gibi geçersiz kılınmış sınıfı uygulayarak düzeltebilirsiniz[(GitHub kaynağı):](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)

```csharp
    using System;
    using System.Web.Mvc;
    using Microsoft.ApplicationInsights;

    namespace MVC2App.Controllers
    {
      [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
      public class AiHandleErrorAttribute : HandleErrorAttribute
      {
        public override void OnException(ExceptionContext filterContext)
        {
            if (filterContext != null && filterContext.HttpContext != null && filterContext.Exception != null)
            {
                //If customError is Off, then AI HTTPModule will report the exception
                if (filterContext.HttpContext.IsCustomErrorEnabled)
                {   //or reuse instance (recommended!). see note above
                    var ai = new TelemetryClient();
                    ai.TrackException(filterContext.Exception);
                }
            }
            base.OnException(filterContext);
        }
      }
    }
```

#### <a name="mvc-2"></a>MVC 2
HandleError özniteliğini denetleyicilerinizde yeni özniteliğinizile değiştirin.

```csharp
    namespace MVC2App.Controllers
    {
        [AiHandleError]
        public class HomeController : Controller
        {
    ...
```

[Örnek](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a>MVC 3
Global.asax.cs'da genel filtre olarak kaydolun: `AiHandleErrorAttribute`

```csharp
    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...
```

[Örnek](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a>MVC 4, MVC5
AiHandleErrorAttribute'ı FilterConfig.cs'da genel bir filtre olarak kaydedin:

```csharp
    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with the override to track unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }
```

[Örnek](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api"></a>Web API
Application Insights Web SDK sürüm 2.6 (beta3 ve sonraki sürüm) ile başlayarak, Application Insights WebAPI 2+ için denetleyici yöntemlerine otomatik olarak atılan işlenmemiş özel durumları toplar. Bu tür özel durumları izlemek için daha önce özel bir işleyici eklediyseniz (aşağıdaki örneklerde açıklandığı gibi), özel durumların çift kez izlenmesini önlemek için bu özel durumu kaldırabilirsiniz.

Özel durum filtrelerinin işleyemediği birkaç servis vardır. Örneğin:

* Denetleyici oluşturuculardan atılan özel durumlar.
* İleti işleyicilerinden atılan özel durumlar.
* Yönlendirme sırasında atılan özel durumlar.
* Yanıt içeriği serileştirme sırasında atılan özel durumlar.
* Uygulama başlatma sırasında atılan özel durum.
* Arka plan görevlerinde atılan özel durum.

Uygulama tarafından *işlenen* tüm özel durumların yine de el ile izlenmesi gerekir.
Denetleyicilerden kaynaklanan işlenmemiş özel durumlar genellikle 500 "İç Sunucu Hatası" yanıtıyla sonuçlanır. Bu tür bir yanıt, işlenen özel durum (veya hiç bir istisna) sonucu el ile oluşturulursa, 500 ile `ResultCode` ilgili bir istek telemetrisinde izlenir, ancak Application Insights SDK ilgili özel durumu izleyemez.

### <a name="prior-versions-support"></a>Önceki sürümler desteği
Application Insights Web SDK 2.5'in (ve öncesinde) WebAPI 1'i (ve önceki) kullanıyorsanız, özel durumları izlemek için aşağıdaki örneklere bakın.

#### <a name="web-api-1x"></a>Web API 1.x
Override System.Web.Http.Filters.ExceptionFilterAttribute:

```csharp
    using System.Web.Http.Filters;
    using Microsoft.ApplicationInsights;

    namespace WebAPI.App_Start
    {
      public class AiExceptionFilterAttribute : ExceptionFilterAttribute
      {
        public override void OnException(HttpActionExecutedContext actionExecutedContext)
        {
            if (actionExecutedContext != null && actionExecutedContext.Exception != null)
            {  //or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(actionExecutedContext.Exception);
            }
            base.OnException(actionExecutedContext);
        }
      }
    }
```

Bu geçersiz özniteliği belirli denetleyicilere ekleyebilir veya WebApiConfig sınıfındaki genel filtre yapılandırmasına ekleyebilirsiniz:

```csharp
    using System.Web.Http;
    using WebApi1.x.App_Start;

    namespace WebApi1.x
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            config.Routes.MapHttpRoute(name: "DefaultApi", routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional });
            ...
            config.EnableSystemDiagnosticsTracing();

            // Capture exceptions for Application Insights:
            config.Filters.Add(new AiExceptionFilterAttribute());
        }
      }
    }
```

[Örnek](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

#### <a name="web-api-2x"></a>Web API 2.x
IExceptionLogger bir uygulama ekleyin:

```csharp
    using System.Web.Http.ExceptionHandling;
    using Microsoft.ApplicationInsights;

    namespace ProductsAppPureWebAPI.App_Start
    {
      public class AiExceptionLogger : ExceptionLogger
      {
        public override void Log(ExceptionLoggerContext context)
        {
            if (context !=null && context.Exception != null)
            {//or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(context.Exception);
            }
            base.Log(context);
        }
      }
    }
```

Bunu WebApiConfig'deki hizmetlere ekleyin:

```csharp
    using System.Web.Http;
    using System.Web.Http.ExceptionHandling;
    using ProductsAppPureWebAPI.App_Start;

    namespace WebApi2WithMVC
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
            config.Services.Add(typeof(IExceptionLogger), new AiExceptionLogger());
        }
      }
     }
```

[Örnek](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

Alternatif olarak şunları yapabilir:

1. Yalnızca ExceptionHandler'ı Özel Bir IExceptionHandler uygulamasıyla değiştirin. Bu yalnızca çerçeve hala hangi yanıt iletisinin gönderileceğine karar verebildiği zaman (örneğin bağlantı iptal edildiğinde değil) çağrılır.
2. Özel Durum Filtreleri (yukarıdaki Web API 1.x denetleyicileri bölümünde açıklandığı gibi) - tüm durumlarda çağrılmadı.

## <a name="wcf"></a>WCF
Öznitelik genişleten ve IErrorHandler ve IServiceBehavior uygulayan bir sınıf ekleyin.

```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.ServiceModel.Description;
    using System.ServiceModel.Dispatcher;
    using System.Web;
    using Microsoft.ApplicationInsights;

    namespace WcfService4.ErrorHandling
    {
      public class AiLogExceptionAttribute : Attribute, IErrorHandler, IServiceBehavior
      {
        public void AddBindingParameters(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase,
            System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints,
            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)
        {
        }

        public void ApplyDispatchBehavior(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
            foreach (ChannelDispatcher disp in serviceHostBase.ChannelDispatchers)
            {
                disp.ErrorHandlers.Add(this);
            }
        }

        public void Validate(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
        }

        bool IErrorHandler.HandleError(Exception error)
        {//or reuse instance (recommended!). see note above
            var ai = new TelemetryClient();

            ai.TrackException(error);
            return false;
        }

        void IErrorHandler.ProvideFault(Exception error,
            System.ServiceModel.Channels.MessageVersion version,
            ref System.ServiceModel.Channels.Message fault)
        {
        }
      }
    }

Add the attribute to the service implementations:

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...
```

[Örnek](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a>Özel durum performans sayaçları
[Sunucunuza Application Insights Agent'ı](../../azure-monitor/app/monitor-performance-live-website-now.md) yüklediyseniz, .NET ile ölçülen özel durumlar oranının bir grafiğini alabilirsiniz. Bu, hem işlenmiş hem de işlenmemiş .NET özel durumlarını içerir.

Metrik Gezgin sekmesini açın, yeni bir grafik ekleyin ve Performans Sayaçları altında listelenen **Özel Durum oranını**seçin.

.NET çerçevesi, bir aralıktaki özel durum sayısını sayarak ve aralığın uzunluğuna bölünerek oranı hesaplar.

Bu, TrackException raporlarını sayma Uygulama Öngörüleri portalı tarafından hesaplanan 'Özel Durumlar' sayısından farklıdır. Örnekleme aralıkları farklıdır ve SDK, işlenen ve işlenmemiş tüm özel durumlar için TrackException raporları göndermez.

## <a name="next-steps"></a>Sonraki adımlar
* [REST, SQL ve bağımlılıklara yapılan diğer çağrıları izleyin](../../azure-monitor/app/asp-net-dependencies.md)
* [Sayfa yükleme sürelerini, tarayıcı özel durumlarını ve AJAX çağrılarını izleyin](../../azure-monitor/app/javascript.md)
* [Performans sayaçlarını izleme](../../azure-monitor/app/performance-counters.md)
