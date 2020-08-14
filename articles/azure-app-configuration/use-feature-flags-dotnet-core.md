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
ms.date: 08/12/2020
ms.author: lcozzens
ms.custom: devx-track-csharp, mvc
ms.openlocfilehash: 3f8a43a1ff28206a4bcc5fd059f69492c83eb34d
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88224722"
---
# <a name="tutorial-use-feature-flags-in-an-aspnet-core-app"></a>Öğretici: ASP.NET Core uygulamasında Özellik bayraklarını kullanma

.NET Core Özellik Yönetimi kitaplıkları, .NET veya ASP.NET Core uygulamasında Özellik bayraklarını uygulamak için ıdimatik destek sağlar. Bu kitaplıklar, kodlarınıza bildirimli olarak özellik bayrakları eklemenize olanak tanır, böylece tüm deyimlerini el ile yazmak zorunda kalmazsınız `if` .

Özellik Yönetimi kitaplıkları Ayrıca arka planda özellik bayrakları yaşam döngülerini yönetir. Örneğin, kitaplıklar yenileme ve önbellek bayrak durumlarını veya bir istek çağrısı sırasında bayrak durumunun sabit olmasını garanti eder. Ayrıca, ASP.NET Core kitaplığı MVC denetleyici eylemleri, görünümler, rotalar ve ara yazılım gibi kullanıma hazır tümleştirmeler sunmaktadır.

[ASP.NET Core uygulama hızlı başlangıçlarına özellik ekleme bayrakları](./quickstart-feature-flag-aspnet-core.md) , bir ASP.NET Core uygulamasına özellik bayrakları eklemenin birkaç yolunu gösterir. Bu öğreticide bu yöntemler daha ayrıntılı olarak açıklanmaktadır. Tüm başvurular için [ASP.NET Core Özellik Yönetimi belgelerine](https://go.microsoft.com/fwlink/?linkid=2091410)bakın.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Özellik kullanılabilirliğini denetlemek için uygulamanızın önemli bölümlerine özellik bayrakları ekleyin.
> * Özellik bayraklarını yönetmek için kullandığınızda uygulama yapılandırmasıyla tümleştirin.

## <a name="set-up-feature-management"></a>Özellik yönetimini ayarlama

`Microsoft.FeatureManagement.AspNetCore` `Microsoft.FeatureManagement` .NET Core Feature Manager 'ı kullanmak için ve NuGet paketlerine bir başvuru ekleyin.
    
.NET Core Feature Manager, `IFeatureManager` Framework 'ün yerel yapılandırma sisteminden Özellik bayraklarını alır. Sonuç olarak, dosyadaki veya ortam değişkenlerinin yerel *appsettings.js* dahil olmak üzere .NET Core tarafından desteklenen herhangi bir yapılandırma kaynağını kullanarak uygulamanızın Özellik bayraklarını tanımlayabilirsiniz. `IFeatureManager` .NET Core bağımlılığı ekleme 'yi kullanır. Özellik yönetimi hizmetlerini standart kuralları kullanarak kaydedebilirsiniz:

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

Varsayılan olarak, Özellik Yöneticisi `"FeatureManagement"` .NET Core yapılandırma verilerinin bölümünden Özellik bayraklarını alır. Aşağıdaki örnek, özellik yöneticisini bunun yerine farklı bir bölümden okumasını söyler `"MyFeatureFlags"` :

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

Özellik bayraklarınız içinde filtreler kullanırsanız, ek bir kitaplık eklemeniz ve kaydetmeniz gerekir. Aşağıdaki örnek, adlı yerleşik bir özellik filtresinin nasıl kullanılacağını gösterir `PercentageFilter` :

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

ASP.NET Core uygulamanızı uygulama yapılandırmasına bağlanmanın en kolay yolu yapılandırma sağlayıcısıdır `Microsoft.Azure.AppConfiguration.AspNetCore` . Bu NuGet paketini kullanmak için aşağıdaki adımları izleyin.

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

2. *Startup.cs* `Configure` ASP.NET Core Web uygulaması istekleri almaya devam ederken, özellik bayrağı değerlerinin yinelenen bir aralıkta yenilenmesini sağlamak üzere bir ara yazılım eklemek için Startup.cs açın ve yöntemi güncelleştirin.

   ```csharp
   public void Configure(IApplicationBuilder app, IHostingEnvironment env)
   {
       app.UseAzureAppConfiguration();
       app.UseMvc();
   }
   ```

Özellik bayrağı değerlerinin zaman içinde değiştirilmesi beklenmektedir. Varsayılan olarak, özellik bayrağı değerleri 30 saniyelik bir süre için önbelleğe alınır, bu nedenle, ara yazılım bir istek aldığında tetiklenen yenileme işlemi, önbelleğe alınan değerin süresi dolana kadar değeri güncelleştirmez. Aşağıdaki kod, önbellekte sona erme saati veya yoklama aralığının 5 dakikaya nasıl değiştirileceğini gösterir `options.UseFeatureFlags()` .

```csharp
config.AddAzureAppConfiguration(options => {
    options.Connect(settings["ConnectionStrings:AppConfig"])
           .UseFeatureFlags(featureFlagOptions => {
                featureFlagOptions.CacheExpirationTime = TimeSpan.FromMinutes(5);
           });
});
```

## <a name="feature-flag-declaration"></a>Özellik bayrağı bildirimi

Her özellik bayrağının iki bölümü vardır: bir özelliğin durumunun *Açık* olup olmadığını değerlendirmek için kullanılan bir veya daha fazla filtrenin adı ve listesi (yani, değeri olduğunda `True` ). Bir filtre, bir özelliğin açılması gerektiğinde kullanım durumunu tanımlar.

Bir özellik bayrağının birden çok filtresi olduğunda, filtrenin etkin olması gerektiğini belirlerken filtre listesine bir düzen eklenir. Bu noktada, özellik bayrağı *Açık*ve kalan filtre sonuçları atlanır. Filtre yoksa, özelliğin etkinleştirilmesi gerektiğini gösteriyorsa, özellik bayrağı *kapalıdır*.

Özellik Yöneticisi özellik bayrakları için yapılandırma kaynağı olarak *appsettings.js* destekler. Aşağıdaki örnekte, bir JSON dosyasında özellik bayraklarının nasıl ayarlanacağı gösterilmektedir:

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

Kurala göre, `FeatureManagement` Bu JSON belgesinin bölümü Özellik bayrağı ayarları için kullanılır. Önceki örnekte, içinde kendi filtreleri tanımlanmış olan üç özellik bayrakları gösterilmektedir `EnabledFor` :

* `FeatureA`*Açık*.
* `FeatureB`*kapalıdır*.
* `FeatureC` özelliği olan adlı bir filtre belirtir `Percentage` `Parameters` . `Percentage` , yapılandırılabilir bir filtredir. Bu örnekte, `Percentage` `FeatureC` bayrağın *Açık*olması için yüzde 50 olasılık değerini belirtir.

## <a name="feature-flag-references"></a>Özellik bayrağı başvuruları

Kodda Özellik bayraklarını kolayca başvurabilmeniz için bunları değişken olarak tanımlamanız gerekir `enum` :

```csharp
public enum MyFeatureFlags
{
    FeatureA,
    FeatureB,
    FeatureC
}
```

## <a name="feature-flag-checks"></a>Özellik bayrağı denetimleri

Özellik yönetiminin temel stili, ilk olarak bir özellik bayrağının *Açık*olarak ayarlanmış olup olmadığını denetme. Bu durumda, Özellik Yöneticisi özelliğin içerdiği eylemleri çalıştırır. Örnek:

```csharp
IFeatureManager featureManager;
...
if (await featureManager.IsEnabledAsync(nameof(MyFeatureFlags.FeatureA)))
{
    // Run the following code
}
```

## <a name="dependency-injection"></a>Bağımlılık ekleme

ASP.NET Core MVC 'de `IFeatureManager` bağımlılık ekleme aracılığıyla Özellik yöneticisine erişebilirsiniz:

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

MVC denetleyicileri ' nde, `FeatureGate` bir denetleyici sınıfının veya belirli bir eylemin etkin olup olmadığını denetlemek için özniteliğini kullanırsınız. Aşağıdaki `HomeController` denetleyicinin, `FeatureA` Denetleyici sınıfı içeren *on* herhangi bir eylemde yürütülmesi gerekir:

```csharp
using Microsoft.FeatureManagement.Mvc;

[FeatureGate(MyFeatureFlags.FeatureA)]
public class HomeController : Controller
{
    ...
}
```

Aşağıdaki `Index` eylemin `FeatureA` çalıştırılabilmesi için önce *Açık* olması gerekir:

```csharp
using Microsoft.FeatureManagement.Mvc;

[FeatureGate(MyFeatureFlags.FeatureA)]
public IActionResult Index()
{
    return View();
}
```

Denetim özelliği bayrağı *kapalı*olduğu IÇIN bir MVC denetleyicisi veya eylemi engellendiğinde, kayıtlı bir `IDisabledFeaturesHandler` arabirim çağırılır. Varsayılan `IDisabledFeaturesHandler` arabirim, yanıt gövdesi olmayan istemciye 404 durum kodu döndürür.

## <a name="mvc-views"></a>MVC görünümleri

MVC görünümlerinde, bir `<feature>` özellik bayrağının etkin olup olmadığına bağlı olarak içerik işlemek için bir etiketi kullanabilirsiniz:

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

Özellik `<feature>` etiketi, bir listedeki herhangi bir özellik veya tüm özelliklerin etkinleştirilmesi halinde içerik göstermek için de kullanılabilir.

```html
<feature name="FeatureA, FeatureB" requirement="All">
    <p>This can only be seen if 'FeatureA' and 'FeatureB' are enabled.</p>
</feature>
<feature name="FeatureA, FeatureB" requirement="Any">
    <p>This can be seen if 'FeatureA', 'FeatureB', or both are enabled.</p>
</feature>
```

## <a name="mvc-filters"></a>MVC filtreleri

MVC filtrelerini bir özellik bayrağının durumuna göre etkinleştirilecek şekilde ayarlayabilirsiniz. Aşağıdaki kod adlı bir MVC filtresi ekler `SomeMvcFilter` . Bu filtre, yalnızca etkinse MVC işlem hattı içinde tetiklenir `FeatureA` . Bu özellik ile sınırlıdır `IAsyncActionFilter` . 

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

Özellik bayraklarını, uygulama dallarını ve ara yazılımı koşullu olarak eklemek için de kullanabilirsiniz. Aşağıdaki kod yalnızca etkin olduğunda istek ardışık düzenine bir ara yazılım bileşeni ekler `FeatureA` :

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

Bu öğreticide, kitaplıkları kullanarak ASP.NET Core uygulamanızda Özellik bayraklarını nasıl uygulayacağınızı öğrendiniz `Microsoft.FeatureManagement` . ASP.NET Core ve uygulama yapılandırmasındaki özellik yönetimi desteği hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [ASP.NET Core Özellik bayrağı örnek kodu](/azure/azure-app-configuration/quickstart-feature-flag-aspnet-core)
* [Microsoft. FeatureManagement belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.featuremanagement)
* [Özellik bayraklarını yönetme](./manage-feature-flags.md)
