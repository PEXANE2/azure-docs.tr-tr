---
title: Azure uygulama yapılandırma dayanıklılığı ve olağanüstü durum kurtarma
description: Azure Uygulama yapılandırması ile dayanıklılık ve olağanüstü durum kurtarmayı nasıl uygulayacağınızı yalın yapın.
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 96ef09ac081aa328014217592a7fcd3ed6314c0e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77523773"
---
# <a name="resiliency-and-disaster-recovery"></a>Dayanıklılık ve olağanüstü durum kurtarma

Şu anda Azure Uygulama yapılandırması bölgesel bir hizmettir. Her yapılandırma deposu belirli bir Azure bölgesinde oluşturulur. Bölge genelinde kesinti, bu bölgedeki tüm depoları etkiler. Uygulama yapılandırması başka bir bölgeye otomatik yük devretme sunmaz. Bu makalede, uygulamanızın coğrafi dayanıklılığını artırmak için Azure bölgelerinde birden çok yapılandırma deposunu nasıl kullanabileceğiniz hakkında genel yönergeler sunulmaktadır.

## <a name="high-availability-architecture"></a>Yüksek kullanılabilirlik mimarisi

Bölgeler arası yedeklilik sağlamak için, farklı bölgelerde birden çok uygulama yapılandırma deposu oluşturmanız gerekir. Bu kurulumla, uygulamanızda birincil depo erişilemez hale gelirse geri dönecek en az bir ek yapılandırma deposu vardır. Aşağıdaki diyagramda, uygulamanız ve birincil ve ikincil yapılandırma depoları arasındaki topoloji gösterilmektedir:

![Coğrafi olarak yedekli depolar](./media/geo-redundant-app-configuration-stores.png)

Uygulamanız, yapılandırmasını hem birincil hem de ikincil depolardan paralel olarak yükler. Bunu yapmak, yapılandırma verilerini başarıyla alma olasılığını artırır. Verilerin her iki mağazasında da eşitlenmiş durumda tutulması sizin sorumluluğunuzdadır. Aşağıdaki bölümlerde, uygulamanıza nasıl coğrafi dayanıklılık oluşturabileceğiniz açıklanmaktadır.

## <a name="failover-between-configuration-stores"></a>Yapılandırma depoları arasında yük devretme

Teknik olarak, uygulamanız yük devretme yürütmez. Aynı anda iki uygulama yapılandırma mağazasında aynı yapılandırma verisi kümesini almaya çalışıyor. Kodunuzu önce ikincil depodan, sonra da birincil mağazadan yüklenecek şekilde düzenleyin. Bu yaklaşım, birincil depodaki yapılandırma verilerinin kullanılabilir olduğunda öncelikli olmasını sağlar. Aşağıdaki kod parçacığı, bu düzenlemeyi .NET Core 'da nasıl uygulayabileceğinizi göstermektedir:

#### <a name="net-core-2x"></a>[.NET Core 2. x](#tab/core2x)

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                  .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
        })
        .UseStartup<Startup>();
    
```

#### <a name="net-core-3x"></a>[.NET Core 3. x](#tab/core3x)

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
            webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                    .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
            })
            .UseStartup<Startup>());
```
---

`AddAzureAppConfiguration` İşleve geçirilen `optional` parametreye dikkat edin. Olarak `true`ayarlandığında, bu parametre, işlev yapılandırma verilerini yükleyememesi durumunda uygulamanın devam etmesini önler.

## <a name="synchronization-between-configuration-stores"></a>Yapılandırma depoları arasında eşitleme

Coğrafi olarak yedekli yapılandırma mağazalarınızın tümünün aynı veri kümesine sahip olması önemlidir. Birincil depodan verileri isteğe bağlı olarak kopyalamak için uygulama yapılandırmasındaki **dışarı aktarma** işlevini kullanabilirsiniz. Bu işlev hem Azure portal hem de CLı aracılığıyla kullanılabilir.

Azure portal, aşağıdaki adımları izleyerek başka bir yapılandırma deposuna bir değişikliği gönderebilirsiniz.

1. **İçeri/dışarı aktarma** sekmesine gidin ve**uygulama yapılandırma** > **hedefini** >  **dışarı aktar** > ' ı seçin ve**kaynak seçin**.

1. Açılan yeni dikey pencerede abonelik, kaynak grubu ve ikincil deponuzu kaynak adını belirtip **Uygula**' yı seçin.

1. İkincil deponuza dışarı aktarmak istediğiniz yapılandırma verilerini seçebilmeniz için Kullanıcı arabirimi güncellenir. Varsayılan saat değerini olduğu gibi bırakabilir ve hem **etiketinden** hem de **etiketini** aynı değere ayarlayabilirsiniz. **Uygula**’yı seçin.

1. Tüm yapılandırma değişiklikleri için önceki adımları tekrarlayın.

Bu dışarı aktarma işlemini otomatik hale getirmek için Azure CLı 'yi kullanın. Aşağıdaki komut, birincil depodan ikinciye tek bir yapılandırma değişikliğinin nasıl dışarı aktarılacağını göstermektedir:

```azurecli
    az appconfig kv export --destination appconfig --name {PrimaryStore} --label {Label} --dest-name {SecondaryStore} --dest-label {Label}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, uygulama yapılandırması için çalışma zamanı sırasında uygulamanızı coğrafi esnekliği elde etmek üzere nasıl geliştirecek öğrendiniz. Ayrıca derleme veya dağıtım zamanında uygulama yapılandırmasından yapılandırma verileri ekleyebilirsiniz. Daha fazla bilgi için bkz. [CI/CD işlem hattı Ile tümleştirme](./integrate-ci-cd-pipeline.md).
