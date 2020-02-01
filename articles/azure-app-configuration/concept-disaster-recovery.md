---
title: Azure uygulama yapılandırma dayanıklılığı ve olağanüstü durum kurtarma
description: Azure Uygulama yapılandırması ile dayanıklılık ve olağanüstü durum kurtarmayı nasıl uygulayacağınızı yalın yapın.
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: 889699ab184b82a7c194043d15358ecdaab5d03d
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76899646"
---
# <a name="resiliency-and-disaster-recovery"></a>Dayanıklılık ve olağanüstü durum kurtarma

Şu anda Azure Uygulama yapılandırması bölgesel bir hizmettir. Her yapılandırma deposu belirli bir Azure bölgesinde oluşturulur. Bölge genelinde kesinti, bu bölgedeki tüm depoları etkiler. Uygulama yapılandırması başka bir bölgeye otomatik yük devretme sunmaz. Bu makalede, uygulamanızın coğrafi dayanıklılığını artırmak için Azure bölgelerinde birden çok yapılandırma deposunu nasıl kullanabileceğiniz hakkında genel yönergeler sunulmaktadır.

## <a name="high-availability-architecture"></a>Yüksek kullanılabilirlik mimarisi

Bölgeler arası yedeklilik sağlamak için, farklı bölgelerde birden çok uygulama yapılandırma deposu oluşturmanız gerekir. Bu kurulumla, uygulamanızda birincil depo erişilemez hale gelirse geri dönecek en az bir ek yapılandırma deposu vardır. Aşağıdaki diyagramda, uygulamanız ve birincil ve ikincil yapılandırma depoları arasındaki topoloji gösterilmektedir:

![Coğrafi olarak yedekli depolar](./media/geo-redundant-app-configuration-stores.png)

Uygulamanız, yapılandırmasını hem birincil hem de ikincil depolardan paralel olarak yükler. Bunu yapmak, yapılandırma verilerini başarıyla alma olasılığını artırır. Verilerin her iki mağazasında da eşitlenmiş durumda tutulması sizin sorumluluğunuzdadır. Aşağıdaki bölümlerde, uygulamanıza nasıl coğrafi dayanıklılık oluşturabileceğiniz açıklanmaktadır.

## <a name="failover-between-configuration-stores"></a>Yapılandırma depoları arasında yük devretme

Teknik olarak, uygulamanız yük devretme yürütmez. Aynı anda iki uygulama yapılandırma mağazasında aynı yapılandırma verisi kümesini almaya çalışıyor. Kodunuzu önce ikincil depodan, sonra da birincil mağazadan yüklenecek şekilde düzenleyin. Bu yaklaşım, birincil depodaki yapılandırma verilerinin kullanılabilir olduğunda öncelikli olmasını sağlar. Aşağıdaki kod parçacığı, bu düzenlemeyi .NET Core CLI nasıl uygulayabileceğinizi göstermektedir:

#### <a name="net-core-2xtabcore2x"></a>[.NET Core 2. x](#tab/core2x)

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

#### <a name="net-core-3xtabcore3x"></a>[.NET Core 3. x](#tab/core3x)

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

`optional` parametreye `AddAzureAppConfiguration` işlevine geçtiğini unutmayın. `true`olarak ayarlandığında, bu parametre, işlevin yapılandırma verilerini yükleyememesi durumunda uygulamanın devam etmesini önler.

## <a name="synchronization-between-configuration-stores"></a>Yapılandırma depoları arasında eşitleme

Coğrafi olarak yedekli yapılandırma mağazalarınızın tümünün aynı veri kümesine sahip olması önemlidir. Birincil depodan verileri isteğe bağlı olarak kopyalamak için uygulama yapılandırmasındaki **dışarı aktarma** işlevini kullanabilirsiniz. Bu işlev hem Azure portal hem de CLı aracılığıyla kullanılabilir.

Azure portal, aşağıdaki adımları izleyerek başka bir yapılandırma deposuna bir değişikliği gönderebilirsiniz.

1. **İçeri/dışarı aktarma** sekmesine gidin ve > **uygulama yapılandırma** > **hedefi** > **bir kaynak seçin** **' i seçin** .

2. Açılan yeni dikey pencerede abonelik, kaynak grubu ve ikincil deponuzu kaynak adını belirtin ve ardından **Uygula**' yı seçin.

3. İkincil deponuza dışarı aktarmak istediğiniz yapılandırma verilerini seçebilmeniz için Kullanıcı arabirimi güncellenir. Varsayılan saat değerini olduğu gibi bırakabilir ve hem **etiketinden** hem de **etiketini** aynı değere ayarlayabilirsiniz. **Uygula**’yı seçin.

4. Tüm yapılandırma değişiklikleri için önceki adımları tekrarlayın.

Bu dışarı aktarma işlemini otomatik hale getirmek için Azure CLı 'yi kullanın. Aşağıdaki komut, birincil depodan ikinciye tek bir yapılandırma değişikliğinin nasıl dışarı aktarılacağını göstermektedir:

    az appconfig kv export --destination appconfig --name {PrimaryStore} --label {Label} --dest-name {SecondaryStore} --dest-label {Label}

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, uygulama yapılandırması için çalışma zamanı sırasında uygulamanızı coğrafi esnekliği elde etmek üzere nasıl geliştirecek öğrendiniz. Ayrıca derleme veya dağıtım zamanında uygulama yapılandırmasından yapılandırma verileri ekleyebilirsiniz. Daha fazla bilgi için bkz. [CI/CD işlem hattı Ile tümleştirme](./integrate-ci-cd-pipeline.md).

