---
title: .NET Core uygulamasında Özellik bayraklarını kullanma öğreticisi | Microsoft Docs
description: Bu öğreticide, .NET Core uygulamalarında Özellik bayraklarını nasıl uygulayacağınızı öğreneceksiniz.
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: b04fe3b6451fd7250bc3b05970d49fdb8e7003bd
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76899304"
---
# <a name="tutorial-use-feature-flags-in-an-aspnet-core-app"></a>Öğretici: ASP.NET Core uygulamasında Özellik bayraklarını kullanma

.NET Core Özellik Yönetimi kitaplıkları, .NET veya ASP.NET Core uygulamasında Özellik bayraklarını uygulamak için ıdimatik destek sağlar. Bu kitaplıklar, kodlarınıza bildirimli olarak özellik bayrakları eklemenize olanak tanır, böylece tüm `if` deyimlerini el ile yazmak zorunda kalmazsınız.

Özellik Yönetimi kitaplıkları Ayrıca arka planda özellik bayrakları yaşam döngülerini yönetir. Örneğin, kitaplıklar yenileme ve önbellek bayrak durumlarını veya bir istek çağrısı sırasında bayrak durumunun sabit olmasını garanti eder. Ayrıca, ASP.NET Core kitaplığı MVC denetleyici eylemleri, görünümler, rotalar ve ara yazılım gibi kullanıma hazır tümleştirmeler sunmaktadır.

[ASP.NET Core uygulama hızlı başlangıçlarına özellik ekleme bayrakları](./quickstart-feature-flag-aspnet-core.md) , bir ASP.NET Core uygulamasına özellik bayrakları eklemenin birkaç yolunu gösterir. Bu öğreticide bu yöntemler daha ayrıntılı olarak açıklanmaktadır. Tüm başvurular için [ASP.NET Core Özellik Yönetimi belgelerine](https://go.microsoft.com/fwlink/?linkid=2091410)bakın.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Özellik kullanılabilirliğini denetlemek için uygulamanızın önemli bölümlerine özellik bayrakları ekleyin.
> * Özellik bayraklarını yönetmek için kullandığınızda uygulama yapılandırmasıyla tümleştirin.

## <a name="set-up-feature-management"></a>Özellik yönetimini ayarlama

.NET Core Feature Manager `IFeatureManager` Framework 'ün yerel yapılandırma sisteminden Özellik bayraklarını alır. Sonuç olarak, yerel *appSettings. JSON* dosyası veya ortam değişkenleri dahil olmak üzere .NET Core tarafından desteklenen herhangi bir yapılandırma kaynağını kullanarak uygulamanızın Özellik bayraklarını tanımlayabilirsiniz. `IFeatureManager` .NET Core bağımlılığı ekleme ' ye bağımlıdır. Özellik yönetimi hizmetlerini standart kuralları kullanarak kaydedebilirsiniz:

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement();
    }
}
```

Varsayılan olarak, Özellik Yöneticisi .NET Core yapılandırma verilerinin `"FeatureManagement"` bölümünden Özellik bayraklarını alır. Aşağıdaki örnek, özellik yöneticisinin bunun yerine `"MyFeatureFlags"` adlı farklı bir bölümden okumasını söyler:

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement(options =>
        {
                options.UseConfiguration(Configuration.GetSection("MyFeatureFlags"));
        });
    }
}
```

Özellik bayraklarınız içinde filtreler kullanırsanız, ek bir kitaplık eklemeniz ve kaydetmeniz gerekir. Aşağıdaki örnek, `PercentageFilter`adlı yerleşik bir özellik filtresinin nasıl kullanılacağını gösterir:

```csharp
using Microsoft.FeatureManagement;
using Microsoft.FeatureManagement.FeatureFilters;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement()
                .AddFeatureFilter<PercentageFilter>();
    }
}
```

Özellik bayraklarını uygulamanın dışında tutmanızı ve ayrı olarak yönetmenizi öneririz. Bunun yapılması, bayrak durumlarını dilediğiniz zaman değiştirmenize ve bu değişikliklerin uygulamada hemen etkili olmasını sağlar. Uygulama yapılandırması, özel bir Portal Kullanıcı arabirimi aracılığıyla tüm özellik Bayraklarınızı düzenlemek ve denetlemek için merkezi bir yer sağlar. Uygulama yapılandırması Ayrıca, bayrakları uygulamanıza doğrudan .NET Core istemci kitaplıkları aracılığıyla da sunar.

ASP.NET Core uygulamanızı uygulama yapılandırmasına bağlanmanın en kolay yolu yapılandırma sağlayıcısı `Microsoft.Azure.AppConfiguration.AspNetCore`kullanmaktır. Bu NuGet paketini kullanmak için aşağıdaki adımları izleyin.

1. *Program.cs* dosyasını açın ve aşağıdaki kodu ekleyin.

   ```csharp
   using Microsoft.Extensions.Configuration.AzureAppConfiguration;

   public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
       WebHost.CreateDefaultBuilder(args)
              .ConfigureAppConfiguration((hostingContext, config) => {
                  var settings = config.Build();
                  config.AddAzureAppConfiguration(options => {
                      options.Connect(settings["ConnectionStrings:AppConfig"])
                             .UseFeatureFlags();
                   });
              })
              .UseStartup<Startup>();
   ```

2. ASP.NET Core Web uygulaması istekleri almaya devam ederken, özellik bayrağı değerlerinin yinelenen bir aralıkta yenilenmesini sağlamak üzere bir ara yazılım eklemek için *Startup.cs* 'i açın ve `Configure` yöntemi güncelleştirin.

   ```csharp
   public void Configure(IApplicationBuilder app, IHostingEnvironment env)
   {
       app.UseAzureAppConfiguration();
       app.UseMvc();
   }
   ```

Özellik bayrağı değerlerinin zaman içinde değiştirilmesi beklenmektedir. Varsayılan olarak, özellik bayrağı değerleri 30 saniyelik bir süre için önbelleğe alınır, bu nedenle, ara yazılım bir istek aldığında tetiklenen yenileme işlemi, önbelleğe alınan değerin süresi dolana kadar değeri güncelleştirmez. Aşağıdaki kod, `options.UseFeatureFlags()` çağrısında önbelleğin süre sonu zamanının veya yoklama aralığının 5 dakikaya nasıl değiştirileceğini gösterir.

```csharp
config.AddAzureAppConfiguration(options => {
    options.Connect(settings["ConnectionStrings:AppConfig"])
           .UseFeatureFlags(featureFlagOptions => {
                featureFlagOptions.CacheExpirationTime = TimeSpan.FromMinutes(5);
           });
});
```

## <a name="feature-flag-declaration"></a>Özellik bayrağı bildirimi

Her özellik bayrağının iki bölümü vardır: bir özelliğin durumunun *Açık* olup olmadığını değerlendirmek için kullanılan bir veya daha fazla filtrenin adı ve listesi (yani, değeri `True`). Bir filtre, bir özelliğin açılması gerektiğinde kullanım durumunu tanımlar.

Bir özellik bayrağının birden çok filtresi olduğunda, filtrenin etkin olması gerektiğini belirlerken filtre listesine bir düzen eklenir. Bu noktada, özellik bayrağı *Açık*ve kalan filtre sonuçları atlanır. Filtre yoksa, özelliğin etkinleştirilmesi gerektiğini gösteriyorsa, özellik bayrağı *kapalıdır*.

Özellik Yöneticisi, özellik bayrakları için bir yapılandırma kaynağı olarak *appSettings. JSON* ' i destekler. Aşağıdaki örnekte, bir JSON dosyasında özellik bayraklarının nasıl ayarlanacağı gösterilmektedir:

```JSON
"FeatureManagement": {
    "FeatureA": true, // Feature flag set to on
    "FeatureB": false, // Feature flag set to off
    "FeatureC": {
        "EnabledFor": [
            {
                "Name": "Percentage",
                "Parameters": {
                    "Value": 50
                }
            }
        ]
    }
}
```

Kurala göre, bu JSON belgesinin `FeatureManagement` bölümü Özellik bayrağı ayarları için kullanılır. Önceki örnekte, `EnabledFor` özelliğinde tanımlanan filtreleriyle üç özellik bayrağı gösterilmektedir:

* `FeatureA` *Açık*.
* `FeatureB` *kapalı*.
* `FeatureC`, `Parameters` özelliği ile `Percentage` adlı bir filtre belirtir. `Percentage` yapılandırılabilir bir filtredir. Bu örnekte, `Percentage` `FeatureC` bayrağının *Açık*olması için %50 oranında bir olasılık belirtir.

## <a name="feature-flag-references"></a>Özellik bayrağı başvuruları

Kodda Özellik bayraklarını kolayca başvurabilmeniz için bunları `enum` değişkenleri olarak tanımlamanız gerekir:

```csharp
public enum MyFeatureFlags
{
    FeatureA,
    FeatureB,
    FeatureC
}
```

## <a name="feature-flag-checks"></a>Özellik bayrağı denetimleri

Özellik yönetiminin temel stili, ilk olarak bir özellik bayrağının *Açık*olarak ayarlanmış olup olmadığını denetme. Bu durumda, Özellik Yöneticisi özelliğin içerdiği eylemleri çalıştırır. Örneğin:

```csharp
IFeatureManager featureManager;
...
if (await featureManager.IsEnabledAsync(nameof(MyFeatureFlags.FeatureA)))
{
    // Run the following code
}
```

## <a name="dependency-injection"></a>Bağımlılık ekleme

ASP.NET Core MVC 'de, bağımlılık ekleme aracılığıyla Feature Manager `IFeatureManager` erişebilirsiniz:

```csharp
public class HomeController : Controller
{
    private readonly IFeatureManager _featureManager;

    public HomeController(IFeatureManager featureManager)
    {
        _featureManager = featureManager;
    }
}
```

## <a name="controller-actions"></a>Denetleyici eylemleri

MVC denetleyicileri ' nde, tüm denetleyici sınıfının veya belirli bir eylemin etkinleştirilip etkinleştirilmediğini denetlemek için `FeatureGate` özniteliğini kullanırsınız. Aşağıdaki `HomeController` denetleyicisi, denetleyici sınıfı içeren herhangi bir eylemden önce `FeatureA` *Açık* olmasını gerektirir:

```csharp
[FeatureGate(MyFeatureFlags.FeatureA)]
public class HomeController : Controller
{
    ...
}
```

Aşağıdaki `Index` eylemi, çalıştırılmadan önce `FeatureA` *Açık* olmasını gerektirir:

```csharp
[FeatureGate(MyFeatureFlags.FeatureA)]
public IActionResult Index()
{
    return View();
}
```

Denetim özelliği bayrağı *kapalı*olduğu IÇIN bir MVC denetleyicisi veya eylemi engellendiğinde, kayıtlı bir `IDisabledFeaturesHandler` arabirimi çağırılır. Varsayılan `IDisabledFeaturesHandler` arabirimi, yanıt gövdesi olmayan istemciye 404 durum kodu döndürür.

## <a name="mvc-views"></a>MVC görünümleri

MVC görünümlerinde, bir özellik bayrağının etkin olup olmadığına bağlı olarak içerik işlemek için bir `<feature>` etiketi kullanabilirsiniz:

```html
<feature name="FeatureA">
    <p>This can only be seen if 'FeatureA' is enabled.</p>
</feature>
```

Gereksinimler karşılanmazsa, diğer içerikleri göstermek için `negate` özniteliği kullanılabilir.

```html
<feature name="FeatureA" negate="true">
    <p>This will be shown if 'FeatureA' is disabled.</p>
</feature>
```

Özellik `<feature>` etiketi, bir listedeki herhangi bir özelliğin veya tüm özelliklerin etkinleştirilmesi halinde içerik göstermek için de kullanılabilir.

```html
<feature name="FeatureA, FeatureB" requirement="All">
    <p>This can only be seen if 'FeatureA' and 'FeatureB' are enabled.</p>
</feature>
<feature name="FeatureA, FeatureB" requirement="Any">
    <p>This can be seen if 'FeatureA', 'FeatureB', or both are enabled.</p>
</feature>
```

## <a name="mvc-filters"></a>MVC filtreleri

MVC filtrelerini bir özellik bayrağının durumuna göre etkinleştirilecek şekilde ayarlayabilirsiniz. Aşağıdaki kod, `SomeMvcFilter`adlı bir MVC filtresi ekler. Bu filtre, yalnızca `FeatureA` etkinse MVC işlem hattı içinde tetiklenir. Bu yetenek `IAsyncActionFilter`sınırlıdır. 

```csharp
using Microsoft.FeatureManagement.FeatureFilters;

IConfiguration Configuration { get; set;}

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options => {
        options.Filters.AddForFeature<SomeMvcFilter>(nameof(MyFeatureFlags.FeatureA));
    });
}
```

## <a name="middleware"></a>Yazılımlar

Özellik bayraklarını, uygulama dallarını ve ara yazılımı koşullu olarak eklemek için de kullanabilirsiniz. Aşağıdaki kod yalnızca `FeatureA` etkinleştirildiğinde istek ardışık düzenine bir ara yazılım bileşeni ekler:

```csharp
app.UseMiddlewareForFeature<ThirdPartyMiddleware>(nameof(MyFeatureFlags.FeatureA));
```

Bu kod, bir özellik bayrağını temel alarak tüm uygulamayı dallandırmak için daha genel özelliği devre dışı bırakır:

```csharp
app.UseForFeature(featureName, appBuilder => {
    appBuilder.UseMiddleware<T>();
});
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, `Microsoft.FeatureManagement` kitaplıklarını kullanarak ASP.NET Core uygulamanızda Özellik bayraklarını nasıl uygulayacağınızı öğrendiniz. ASP.NET Core ve uygulama yapılandırmasındaki özellik yönetimi desteği hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [ASP.NET Core Özellik bayrağı örnek kodu](/azure/azure-app-configuration/quickstart-feature-flag-aspnet-core)
* [Microsoft. FeatureManagement belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.featuremanagement)
* [Özellik bayraklarını yönetme](./manage-feature-flags.md)
