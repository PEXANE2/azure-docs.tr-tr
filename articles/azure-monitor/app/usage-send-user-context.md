---
title: Etkinliği izlemek için kullanıcı bağlamı bilgileri - Azure Uygulama Öngörüleri
description: Uygulama Öngörüleri'nde her birine benzersiz ve kalıcı bir kimlik dizesi atayarak kullanıcıların hizmetinizde nasıl hareket ettiğini izleyin.
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 01/03/2019
ms.reviewer: abgreg;mbullwin
ms.openlocfilehash: beb5a0f7ad3733aaf12b0880af4fba23a705a7e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670942"
---
# <a name="send-user-context-ids-to-enable-usage-experiences-in-azure-application-insights"></a>Azure Uygulama Öngörüleri'nde kullanım deneyimlerini etkinleştirmek için kullanıcı bağlamı tünemi gönderme

## <a name="tracking-users"></a>Kullanıcıları izleme

Application Insights, kullanıcılarınızı bir dizi ürün kullanım aracı yla izlemenizi ve izlemenizi sağlar:

- [Kullanıcılar, Oturumlar, Etkinlikler](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
- [Huniler](https://docs.microsoft.com/azure/application-insights/usage-funnels)
- [Bekletme](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention) Kohortlar
- [Çalışma Kitapları](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

Bir kullanıcının zaman içinde ne yaptığını izlemek için Application Insights'ın her kullanıcı veya oturum için bir kimlik alması gerekir. Her özel olaya veya sayfa görünümüne aşağıdaki t.c.'leri ekleyin.

- Kullanıcılar, Huniler, Bekletme ve Kohortlar: Kullanıcı kimliğini ekleyin.
- Oturumlar: Oturum kimliğini ekleyin.

> [!NOTE]
> Bu, Uygulama Öngörüleri ile kullanıcı etkinliğini izlemek için el kitabı adımlarını özetleyen gelişmiş bir makaledir. Birçok web uygulamasında **bu adımlar gerekli olmayabilir**, çünkü [istemci/tarayıcı tarafındaki JavaScript SDK](../../azure-monitor/app/website-monitoring.md )ile birlikte varsayılan sunucu tarafındaki SDK'lar genellikle kullanıcı etkinliğini otomatik olarak izlemek için yeterlidir. Sunucu tarafındaki SDK'ya ek olarak [istemci tarafı izlemeyi](../../azure-monitor/app/website-monitoring.md ) yapılandırmadıysanız, önce bunu yapın ve kullanıcı davranışı analizi araçlarının beklendiği gibi performans gösterip kullanmadığını test edin.

## <a name="choosing-user-ids"></a>Kullanıcı tünelerini seçme

Kullanıcı işleri, kullanıcıların zaman içinde nasıl olduğu Kimliği niçin kalıcı olarak sayılsın çeşitli yaklaşımlar vardır.

- Hizmetinizde zaten bulunan bir kullanıcıtanımı.
- Hizmetin bir tarayıcıya erişimi varsa, tarayıcıda kimliği olan bir çerez geçirebilir. Tanımlama bilgisi kullanıcının tarayıcısında kaldığı sürece kimlik devam eder.
- Gerekirse, her oturumda yeni bir kimlik kullanabilirsiniz, ancak kullanıcılarla ilgili sonuçlar sınırlı olacaktır. Örneğin, bir kullanıcının davranışının zaman içinde nasıl değiştiğini göremezsin.

Kimlik, her kullanıcıyı benzersiz olarak tanımlayacak kadar bir Guid veya başka bir dize kompleksi olmalıdır. Örneğin, uzun bir rasgele sayı olabilir.

Kimlik kullanıcı hakkında kişisel olarak tanımlayıcı bilgiler içeriyorsa, Application Insights'a kullanıcı kimliği olarak göndermek uygun bir değer değildir. Böyle bir kimliği kimlik [doğrulaması](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users)olarak gönderebilirsiniz, ancak kullanım senaryoları için kullanıcı kimliği gereksinimini karşılamaz.

## <a name="aspnet-apps-setting-the-user-context-in-an-itelemetryinitializer"></a>ASP.NET uygulamaları: Kullanıcı bağlamını iTelemetryInitializer'da ayarlama

[Burada](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#addmodify-properties-itelemetryinitializer)ayrıntılı olarak açıklandığı gibi, bir telemetri başharfoluşturun. Oturum kimliğini istek telemetrisi aracılığıyla geçirin ve Context.User.Id ve Context.Session.Id ayarlayın.

Bu örnek, kullanıcı kimliğini oturumdan sonra süresi dolan bir tanımlayıcıya ayarlar. Mümkünse, oturumlar arasında devam eden bir kullanıcı kimliği kullanın.

### <a name="telemetry-initializer"></a>Telemetri başlandırıcı

```csharp
using System;
using System.Web;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace MvcWebRole.Telemetry
{
  /*
   * Custom TelemetryInitializer that sets the user ID.
   *
   */
  public class MyTelemetryInitializer : ITelemetryInitializer
  {
    public void Initialize(ITelemetry telemetry)
    {
        var ctx = HttpContext.Current;

        // If telemetry initializer is called as part of request execution and not from some async thread
        if (ctx != null)
        {
            var requestTelemetry = ctx.GetRequestTelemetry();
 
            // Set the user and session ids from requestTelemetry.Context.User.Id, which is populated in Application_PostAcquireRequestState in Global.asax.cs.
            if (requestTelemetry != null && !string.IsNullOrEmpty(requestTelemetry.Context.User.Id) &&
                (string.IsNullOrEmpty(telemetry.Context.User.Id) || string.IsNullOrEmpty(telemetry.Context.Session.Id)))
            {
                // Set the user id on the Application Insights telemetry item.
                telemetry.Context.User.Id = requestTelemetry.Context.User.Id;
 
                // Set the session id on the Application Insights telemetry item.
                telemetry.Context.Session.Id = requestTelemetry.Context.User.Id;
            }
        }
    }
  }
}
```

### <a name="globalasaxcs"></a>Global.asax.cs

```csharp
using System.Web;
using System.Web.Http;
using System.Web.Mvc;
using System.Web.Optimization;
using System.Web.Routing;

namespace MvcWebRole.Telemetry
{
    public class MvcApplication : HttpApplication
    {
        protected void Application_Start()
        {
            AreaRegistration.RegisterAllAreas();
            GlobalConfiguration.Configure(WebApiConfig.Register);
            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
            RouteConfig.RegisterRoutes(RouteTable.Routes);
            BundleConfig.RegisterBundles(BundleTable.Bundles);
        }
 
        protected void Application_PostAcquireRequestState()
        {
            var requestTelemetry = Context.GetRequestTelemetry();
 
            if (HttpContext.Current.Session != null && requestTelemetry != null && string.IsNullOrEmpty(requestTelemetry.Context.User.Id))
            {
                requestTelemetry.Context.User.Id = Session.SessionID;
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım deneyimlerini etkinleştirmek için [özel etkinlikler](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views)göndermeye başlayın.
- Zaten özel etkinlikler veya sayfa görünümleri gönderiyorsanız, kullanıcıların hizmetinizi nasıl kullandığını öğrenmek için Kullanım araçlarını keşfedin.
    - [Kullanıma genel bakış](usage-overview.md)
    - [Kullanıcılar, Oturumlar ve Etkinlikler](usage-segmentation.md)
    - [Huniler](usage-funnels.md)
    - [Bekletme](usage-retention.md)
    - [Çalışma Kitapları](../../azure-monitor/app/usage-workbooks.md)
