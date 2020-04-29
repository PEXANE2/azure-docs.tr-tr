---
title: Ortam başına yapılandırma kullan
titleSuffix: Azure App Configuration
description: Ortam başına yapılandırma değerlerini sağlamak için etiketleri kullanma
ms.service: azure-app-configuration
author: lisaguthrie
ms.topic: conceptual
ms.date: 3/12/2020
ms.author: lcozzens
ms.openlocfilehash: d1a3323154fc73b453e041a064cfcd36299b8a08
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80056793"
---
# <a name="use-labels-to-enable-different-configurations-for-different-environments"></a>Farklı ortamlarda farklı yapılandırmaların etkinleştirilmesi için etiketleri kullanma

Birçok uygulamanın farklı ortamlar için farklı konfigürasyonlar kullanması gerekir. Bir uygulamanın, arka uç veritabanı için kullanılacak bağlantı dizesini tanımlayan bir yapılandırma değeri olduğunu varsayalım. Uygulamanın geliştiricileri üretimde kullanılan farklı bir veritabanını kullanır. Uygulama geliştirmeden üretime taşınıyor gibi uygulama tarafından kullanılan veritabanı bağlantı dizesi de değişiklik vermelidir.

Azure Uygulama yapılandırmasında, aynı anahtar için farklı değerler tanımlamak üzere *etiketleri* kullanabilirsiniz. Örneğin, *geliştirme* ve *Üretim*için farklı değerlerle tek bir anahtar tanımlayabilirsiniz. Uygulama yapılandırmasına bağlanırken hangi etiketlerin yükleneceğini belirtebilirsiniz.

Bu işlevselliği göstermek için hızlı başlangıçta oluşturulan Web uygulamasını değiştireceksiniz: geliştirme ve üretim için farklı yapılandırma ayarlarını kullanmak üzere [Azure Uygulama yapılandırmasıyla bir ASP.NET Core uygulaması oluşturma](./quickstart-aspnet-core-app.md) . Devam etmeden önce lütfen hızlı başlangıcı doldurun.

## <a name="specify-a-label-when-adding-a-configuration-value"></a>Yapılandırma değeri eklenirken bir etiket belirtin

Azure portal, **yapılandırma Gezgini** ' ne gidin ve hızlı başlangıçta oluşturduğunuz *TestApp: Settings: fontcolor* anahtarını bulun. Bağlam menüsünü seçin ve ardından **Değer Ekle**' ye tıklayın.

> [!div class="mx-imgBorder"]
> ![Değer menü öğesi Ekle](media/labels-add-value.png)

**Değer Ekle** ekranında, **kırmızı** ve bir **geliştirme** **etiketi** **değeri** girin. **İçerik türünü** boş bırak. **Uygula**’yı seçin.

## <a name="loading-configuration-values-with-a-specified-label"></a>Yapılandırma değerleri belirtilen etiketle yükleniyor

Azure Uygulama yapılandırması, varsayılan olarak yalnızca etiketi olmayan yapılandırma değerlerini yükler. Yapılandırma değerleriniz için Etiketler tanımladıysanız, uygulama yapılandırmasına bağlanılırken kullanılacak etiketleri belirtmek isteyeceksiniz.

Son bölümde, *geliştirme* ortamı için farklı bir yapılandırma değeri oluşturdunuz. Bu `HostingEnvironment.EnvironmentName` değişkeni, uygulamanın hangi ortamda çalışmakta olduğunu dinamik olarak tespit etmek için kullanırsınız. Daha fazla bilgi için bkz. [ASP.NET Core birden çok ortam kullanma](/aspnet/core/fundamentals/environments).

Yapılandırma değerlerini, ortam adını `Select` yöntemine geçirerek geçerli ortama karşılık gelen etiketle yükleyin:

```csharp
        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(options =>
                    options
                        .Connect(Environment.GetEnvironmentVariable("AppConfigConnectionString"))
                        // Load configuration values with no label
                        .Select(KeyFilter.Any, LabelFilter.Null)
                        // Override with any configuration values specific to current hosting env
                        .Select(KeyFilter.Any, hostingContext.HostingEnvironment.EnvironmentName)
                );
            })
            .UseStartup<Startup>());
```

> [!IMPORTANT]
> Yukarıdaki kod parçacığı, uygulama yapılandırma bağlantı dizesini adlı `AppConfigConnectionString`bir ortam değişkeninden yükler. Bu ortam değişkeninin doğru ayarlandığından emin olun.

`Select` Yöntemi iki kez çağrılır. İlk kez, hiçbir etiketi olmayan yapılandırma değerlerini yükler. Ardından, yapılandırma değerlerini geçerli ortama karşılık gelen etiketle yükler. Bu ortama özgü değerler etiketi olmayan karşılık gelen değerleri geçersiz kılar. Her anahtar için ortama özgü değerler tanımlamanız gerekmez. Bir anahtar, geçerli ortama karşılık gelen etikete sahip bir değere sahip değilse, etiketi olmayan değer kullanılır.

## <a name="testing-in-different-environments"></a>Farklı ortamlarda test etme

Farklı yapılandırma değerlerini test etmek için, `launchSettings.json` dosyayı `Properties` dizin altında açın. Altında `config` `profiles`girişi bulun. `environmentVariables` Bölümünde, `ASPNETCORE_ENVIRONMENT` değişkenini olarak `Production`ayarlayın.

Yeni değerler ayarlandığında uygulamanızı derleyin ve çalıştırın.

```dotnetcli
dotnet build
dotnet run
```

Gezinmek için bir Web tarayıcısı kullanın `http://localhost:5000`. Yazı tipi renginin siyah olduğunu fark edeceksiniz.

![Üretim yapılandırması ile çalışan Web uygulaması](media/labels-website-prod.png)

Şimdi `launchSettings.json` `ASPNETCORE_ENVIRONMENT` değişkenini olarak `Development`ayarlamak için güncelleştirin. `dotnet run` komutunu yeniden çalıştırın. Yazı tipi renginin artık kırmızı olduğunu fark edeceksiniz. Bunun nedeni, uygulamanın artık `TestApp:Settings:FontColor` `Development` etiketine sahip olan değerini kullanmadır. Diğer tüm yapılandırma değerleri, üretim değerleriyle aynı kalır.

![Geliştirme yapılandırmasıyla çalışan Web uygulaması](media/labels-website-dev.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [ASP.NET Core yapılandırma](/aspnet/core/fundamentals/configuration/)
