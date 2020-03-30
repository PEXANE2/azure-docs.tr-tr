---
title: Bir .NET Core uygulamasında özellik bayraklarını kullanma için öğretici | Microsoft Dokümanlar
description: Bu eğitimde, .NET Core uygulamalarında özellik bayraklarını nasıl uygulayacağınızı öğrenirsiniz.
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
ms.openlocfilehash: 3182961f928a9befc5a55fb6d58e22c74ba81089
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79473447"
---
# <a name="tutorial-use-feature-flags-in-an-aspnet-core-app"></a>Öğretici: ASP.NET Core uygulamasında özellik bayraklarını kullanma

.NET Temel Özellik Yönetimi kitaplıkları, bir .NET veya ASP.NET Core uygulamasında özellik bayraklarının uygulanması için deyimsel destek sağlar. Bu kitaplıklar, bunlar için tüm `if` ifadeleri el ile yazmak zorunda kalmamak için kodunuza özellik bayrakları eklemenize olanak sağlar.

Özellik Yönetimi kitaplıkları, sahne arkasında özellik bayrak yaşam döngülerini de yönetir. Örneğin, kitaplıklar yeniler ve önbellek bayrağı durumları veya bir istek çağrısı sırasında değişmez bir bayrak durumu garanti. Buna ek olarak, ASP.NET Core kitaplığı MVC denetleyici eylemleri, görünümler, rotalar ve ara yazılımlar da dahil olmak üzere kutudan uzak tümleştirmeler sunar.

[ASP.NET Core uygulamasına özellik ekle özelliği bayrakları Quickstart,](./quickstart-feature-flag-aspnet-core.md) ASP.NET Core uygulamasında özellik bayrakları eklemenin çeşitli yollarını gösterir. Bu öğretici, bu yöntemleri daha ayrıntılı olarak açıklar. Tam bir başvuru için [ASP.NET Core özellik yönetimi belgelerine](https://go.microsoft.com/fwlink/?linkid=2091410)bakın.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Özellik kullanılabilirliğini denetlemek için uygulamanızın önemli bölümlerine özellik bayrakları ekleyin.
> * Özellik bayraklarını yönetmek için kullanırken Uygulama Yapılandırması ile tümleştirin.

## <a name="set-up-feature-management"></a>Özellik yönetimini ayarlama

.NET Core `Microsoft.FeatureManagement` özellik yöneticisini kullanmak için NuGet paketine bir başvuru ekleyin.
    
.NET Core özellik `IFeatureManager` yöneticisi, çerçevenin yerel yapılandırma sisteminden özellik bayrakları alır. Sonuç olarak, yerel *appsettings.json* dosyası veya ortam değişkenleri de dahil olmak üzere .NET Core'un desteklediği herhangi bir yapılandırma kaynağını kullanarak uygulamanızın özellik bayraklarını tanımlayabilirsiniz. `IFeatureManager`.NET Çekirdek bağımlılık enjeksiyonuna dayanır. Özellik yönetimi hizmetlerini standart kuralları kullanarak kaydedebilirsiniz:

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

Varsayılan olarak, özellik yöneticisi .NET `"FeatureManagement"` Core yapılandırma verilerinin bölümünden özellik bayrakları alır. Aşağıdaki örnek, özellik yöneticisine bunun yerine `"MyFeatureFlags"` farklı bir bölümden okumasını söyler:

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

Özellik bayraklarınızda filtreler kullanıyorsanız, ek bir kitaplık eklemeniz ve kaydetmeniz gerekir. Aşağıdaki örnek, yerleşik bir özellik filtresinin `PercentageFilter`nasıl kullanılacağını gösterir:

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

Özellik bayraklarını uygulamanın dışında tutmanızı ve bunları ayrı ayrı yönetmenizi öneririz. Bunu yapmak, bayrak durumlarını istediğiniz zaman değiştirmenize ve bu değişikliklerin uygulamada hemen etkili olmasını sağlar. App Configuration, özel bir portal kullanıcı arabirimi aracılığıyla tüm özellik bayraklarınızı düzenlemek ve denetlemek için merkezi bir yer sağlar. App Configuration ayrıca bayrakları doğrudan .NET Core istemci kitaplıkları aracılığıyla uygulamanıza sunar.

ASP.NET Core uygulamanızı App Configuration'a bağlamanın en `Microsoft.Azure.AppConfiguration.AspNetCore`kolay yolu yapılandırma sağlayıcısıdır. Bu NuGet paketini kullanmak için aşağıdaki adımları izleyin.

1. *dosya Program.csyı açın* ve aşağıdaki kodu ekleyin.

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

2. *Startup.cs* açın ve `Configure` ASP.NET Core web uygulaması istek almaya devam ederken özellik bayrak değerlerinin yinelenen bir aralıkta yenilenmesine izin vermek için bir ara yazılım eklemek için yöntemi güncelleştirin.

   ```csharp
   public void Configure(IApplicationBuilder app, IHostingEnvironment env)
   {
       app.UseAzureAppConfiguration();
       app.UseMvc();
   }
   ```

Özellik bayrağı değerlerinin zaman içinde değişmesi beklenir. Varsayılan olarak, özellik bayrağı değerleri 30 saniyelik bir süre için önbelleğe alınır, bu nedenle ara yazılım bir istek aldığında tetiklenen bir yenileme işlemi önbelleğe alınmış değer sona erene kadar değeri güncelleştirmez. Aşağıdaki kod, önbellek son kullanma süresini veya yoklama aralığını `options.UseFeatureFlags()` aramada 5 dakikaya nasıl değiştireceğinizi gösterir.

```csharp
config.AddAzureAppConfiguration(options => {
    options.Connect(settings["ConnectionStrings:AppConfig"])
           .UseFeatureFlags(featureFlagOptions => {
                featureFlagOptions.CacheExpirationTime = TimeSpan.FromMinutes(5);
           });
});
```

## <a name="feature-flag-declaration"></a>Özellik bayrağı bildirimi

Her özellik bayrağının iki bölümü vardır: bir ad ve bir özelliğin durumunun *olup* olmadığını değerlendirmek için kullanılan bir `True`veya daha fazla filtrenin listesi (diğer bir süre, değeri olduğunda). Filtre, bir özelliğin ne zaman açık olması gerektiği ne zaman için bir kullanım örneği tanımlar.

Bir özellik bayrağında birden çok filtre varsa, filtrelerden biri özelliğin etkinleştirilmesi gerektiğini belirleyene kadar filtre listesi sırayla geçiş yapılır. Bu noktada, özellik bayrağı *açık*ve kalan filtre sonuçları atlanır. Hiçbir filtre özelliğinin etkinleştirilmesi gerektiğini göstermiyorsa, özellik bayrağı *kapalıdır.*

Özellik yöneticisi özellik bayrakları için bir yapılandırma kaynağı olarak *appsettings.json* destekler. Aşağıdaki örnek, json dosyasında özellik bayraklarının nasıl ayarlanır:

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

Bu JSON `FeatureManagement` belgesinin bölüm, özellik bayrağı ayarları için kullanılır. Önceki örnekte, `EnabledFor` özellikte tanımlı filtreleri olan üç özellik bayrağı gösterilmektedir:

* `FeatureA`*üzerindedir.*
* `FeatureB`*kapalı.*
* `FeatureC`özelliği ile adlı `Percentage` bir `Parameters` filtre belirtir. `Percentage`yapılandırılabilir bir filtredir. Bu örnekte, `Percentage` bayrağın `FeatureC` *üzerinde*olması için yüzde 50'lik bir olasılık belirtir.

## <a name="feature-flag-references"></a>Özellik bayrağı başvuruları

Özellik bayraklarını kodda kolayca referans layabilmeniz `enum` için, bunları değişken olarak tanımlamalısınız:

```csharp
public enum MyFeatureFlags
{
    FeatureA,
    FeatureB,
    FeatureC
}
```

## <a name="feature-flag-checks"></a>Özellik bayrağı denetimleri

Özellik yönetiminin temel deseni, önce bir özellik *bayrağının açık*olarak ayarlı p'li olup olmadığını denetlemektir. Bu nedenle, özellik yöneticisi daha sonra özelliğin içerdiği eylemleri çalıştırır. Örnek:

```csharp
IFeatureManager featureManager;
...
if (await featureManager.IsEnabledAsync(nameof(MyFeatureFlags.FeatureA)))
{
    // Run the following code
}
```

## <a name="dependency-injection"></a>Bağımlılık ekleme

Core MVCASP.NETde, bağımlılık enjeksiyonu `IFeatureManager` yoluyla özellik yöneticisine erişebilirsiniz:

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

MVC denetleyicilerinde, tüm `FeatureGate` denetleyici sınıfının mı yoksa belirli bir eylemin etkin olup olmadığını denetlemek için özniteliği kullanırsınız. Denetleyici `HomeController` sınıfının `FeatureA` içerdiği herhangi bir eylem yürütülemeden önce aşağıdaki denetleyicinin *üzerinde* olması gerekir:

```csharp
[FeatureGate(MyFeatureFlags.FeatureA)]
public class HomeController : Controller
{
    ...
}
```

Aşağıdaki `Index` eylemin `FeatureA` çalıştırılamadan önce *üzerinde* olması gerekir:

```csharp
[FeatureGate(MyFeatureFlags.FeatureA)]
public IActionResult Index()
{
    return View();
}
```

Denetleyici özellik bayrağı *kapalı*olduğu için bir MVC denetleyicisi `IDisabledFeaturesHandler` veya eylem engellendiğinde, kayıtlı arabirim çağrılır. Varsayılan `IDisabledFeaturesHandler` arabirim, yanıt gövdesi olmayan istemciye 404 durum kodu döndürür.

## <a name="mvc-views"></a>MVC görünümleri

MVC görünümlerinde, bir `<feature>` özellik bayrağının etkin olup olmadığına bağlı olarak içeriği işlemek için bir etiket kullanabilirsiniz:

```html
<feature name="FeatureA">
    <p>This can only be seen if 'FeatureA' is enabled.</p>
</feature>
```

Gereksinimler karşılanmadığında alternatif içerik `negate` görüntülemek için öznitelik kullanılabilir.

```html
<feature name="FeatureA" negate="true">
    <p>This will be shown if 'FeatureA' is disabled.</p>
</feature>
```

Özellik `<feature>` etiketi, bir listedeki özellikler etkinse veya tümünü göstermek için de kullanılabilir.

```html
<feature name="FeatureA, FeatureB" requirement="All">
    <p>This can only be seen if 'FeatureA' and 'FeatureB' are enabled.</p>
</feature>
<feature name="FeatureA, FeatureB" requirement="Any">
    <p>This can be seen if 'FeatureA', 'FeatureB', or both are enabled.</p>
</feature>
```

## <a name="mvc-filters"></a>MVC filtreler

MVC filtreleri, özellik bayrağının durumuna göre etkinleştirilmelerini sağlayabilirsiniz. Aşağıdaki kod adlı `SomeMvcFilter`bir MVC filtresi ekler. Bu filtre, yalnızca etkinleştirildiğinde `FeatureA` MVC ardışık ardışık ardışık ardışık lıkta tetiklenir. Bu özellik `IAsyncActionFilter`ile sınırlıdır. 

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

## <a name="middleware"></a>Ara yazılım

Uygulama dallarını ve ara yazılımları koşullu olarak eklemek için özellik bayraklarını da kullanabilirsiniz. Aşağıdaki kod, yalnızca etkinleştirildiğinde `FeatureA` istek ardışık hattına bir ara yazılım bileşeni ekler:

```csharp
app.UseMiddlewareForFeature<ThirdPartyMiddleware>(nameof(MyFeatureFlags.FeatureA));
```

Bu kod, bir özellik bayrağına dayalı olarak tüm uygulamayı dallamak için daha genel bir özellik özelliği oluşturur:

```csharp
app.UseForFeature(featureName, appBuilder => {
    appBuilder.UseMiddleware<T>();
});
```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, kitaplıkları kullanarak ASP.NET Core uygulamanızda `Microsoft.FeatureManagement` özellik bayraklarının nasıl uygulanacağını öğrendiniz. ASP.NET Core ve Uygulama Yapılandırmasında özellik yönetimi desteği hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [ASP.NET Core özellik bayrak örnek kodu](/azure/azure-app-configuration/quickstart-feature-flag-aspnet-core)
* [Microsoft.FeatureManagement belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.featuremanagement)
* [Özellik bayraklarını yönetme](./manage-feature-flags.md)
