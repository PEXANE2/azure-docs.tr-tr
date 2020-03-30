---
title: Ortam başına yapılandırmayı kullanma
titleSuffix: Azure App Configuration
description: Ortam başına yapılandırma değerlerini sağlamak için etiketleri kullanma
ms.service: azure-app-configuration
author: lisaguthrie
ms.topic: conceptual
ms.date: 3/12/2020
ms.author: lcozzens
ms.openlocfilehash: d1a3323154fc73b453e041a064cfcd36299b8a08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80056793"
---
# <a name="use-labels-to-enable-different-configurations-for-different-environments"></a>Farklı ortamlar için farklı yapılandırmalar etkinleştirmek için etiketleri kullanma

Birçok uygulamanın farklı ortamlar için farklı yapılandırmalar kullanması gerekir. Bir uygulamanın arka uç veritabanı için kullanılacak bağlantı dizesini tanımlayan bir yapılandırma değeri olduğunu varsayalım. Uygulamanın geliştiricileri üretimde kullanılanveritabanından farklı bir veritabanı kullanır. Uygulama geliştirmeden üretime geçtikçe uygulama tarafından kullanılan veritabanı bağlantı dizesi değiştirilmelidir.

Azure Uygulama Yapılandırması'nda, aynı anahtar için farklı değerler tanımlamak için *etiketlerk* kullanabilirsiniz. Örneğin, *Geliştirme* ve *Üretim*için farklı değerlere sahip tek bir anahtar tanımlayabilirsiniz. App Configuration'a bağlanırken hangi etiketin yüklenebileceğini belirtebilirsiniz.

Bu işlevselliği göstermek için [Quickstart: Azure App Configuration ile ASP.NET Core uygulaması oluşturarak](./quickstart-aspnet-core-app.md) geliştirme ve üretim için farklı yapılandırma ayarlarını kullanacağız. Lütfen devam etmeden önce hızlı bir şekilde başlatın.

## <a name="specify-a-label-when-adding-a-configuration-value"></a>Yapılandırma değeri eklerken etiket belirtin

Azure portalında Configuration **Explorer'a** gidin ve hızlı başlangıçta oluşturduğunuz *TestApp:Ayarlar:FontColor* anahtarını bulun. Bağlam menüsünü seçin ve ardından **Değer Ekle'yi**tıklatın.

> [!div class="mx-imgBorder"]
> ![Değer Ekle menü öğesi](media/labels-add-value.png)

Değer **Ekle** **ekranına, kırmızının** **değerini** ve **Bir Geliştirme** **Etiketini** girin. **İçerik türünü** boş bırakın. **Uygula**’yı seçin.

## <a name="loading-configuration-values-with-a-specified-label"></a>Konfigürasyon değerlerini belirli bir etiketle yükleme

Varsayılan olarak, Azure Uygulama Yapılandırması yalnızca etiketsiz yapılandırma değerlerini yükler. Yapılandırma değerleriniz için etiketler tanımladıysanız, App Configuration'a bağlanırken kullanılacak etiket(ler)'i belirtmek istersiniz.

Son bölümde, *Geliştirme* ortamı için farklı bir yapılandırma değeri oluşturdunuz. Uygulamanın `HostingEnvironment.EnvironmentName` şu anda hangi ortamda çalıştığını dinamik olarak belirlemek için değişkeni kullanırsınız. Daha fazla bilgi için [ASP.NET](/aspnet/core/fundamentals/environments)bkz.

Çevre adını `Select` yönteme geçirerek geçerli ortama karşılık gelen etiketle yapılandırma değerlerini yükleyin:

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
> Yukarıdaki kod snippet, App Configuration bağlantı dizesini . `AppConfigConnectionString` Bu ortam değişkeninin düzgün ayarlandıkundan emin olun.

Yöntem `Select` iki kez çağrılır. İlk seferinde, yapılandırma değerlerini etiketsiz yükler. Daha sonra, geçerli ortama karşılık gelen etiketile yapılandırma değerlerini yükler. Bu ortama özgü değerler, etiketsiz karşılık gelen değerleri geçersiz kılar. Her anahtar için ortama özgü değerleri tanımlamanız gerekmez. Bir anahtarın geçerli ortama karşılık gelen bir etiketi olan bir değeri yoksa, etiketsiz değer kullanılır.

## <a name="testing-in-different-environments"></a>Farklı ortamlarda test etme

Farklı yapılandırma değerlerini sınamak `launchSettings.json` için dosyayı `Properties` dizin altında açın. Girişi `config` ' `profiles`nin altında bulun. `environmentVariables` Bölümde, değişkeni `ASPNETCORE_ENVIRONMENT` ' `Production`ye ayarlayın.

Yeni değerler kümesi yle uygulamanızı oluşturun ve çalıştırın.

```dotnetcli
dotnet build
dotnet run
```

'ye `http://localhost:5000`gitmek için bir web tarayıcısı kullanın. Yazı tipi renginin siyah olduğunu fark edeceksiniz.

![Üretim yapılandırması ile çalışan web uygulaması](media/labels-website-prod.png)

Şimdi `launchSettings.json` değişkeni `ASPNETCORE_ENVIRONMENT` ' `Development`ne ayarlamak için güncelleştir `dotnet run` komutunu yeniden çalıştırın. Yazı tipi renginin artık kırmızı olduğunu fark edeceksiniz. Bunun nedeni, uygulamanın artık `TestApp:Settings:FontColor` `Development` etikete sahip olanın değerini kullanmasıdır. Diğer tüm yapılandırma değerleri üretim değerleriyle aynı kalır.

![Geliştirme yapılandırması ile çalışan web uygulaması](media/labels-website-dev.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [ASP.NET Core'da Yapılandırma](/aspnet/core/fundamentals/configuration/)
