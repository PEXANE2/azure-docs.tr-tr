---
title: Azure Uygulama Yapılandırma esnekliği ve olağanüstü durum kurtarma
description: Azure Uygulama Yapılandırması ile esneklik ve olağanüstü durum kurtarma yı nasıl uygulayacağınıza yalınlık edin.
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 96ef09ac081aa328014217592a7fcd3ed6314c0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77523773"
---
# <a name="resiliency-and-disaster-recovery"></a>Dayanıklılık ve olağanüstü durum kurtarma

Şu anda Azure Uygulama Yapılandırması bölgesel bir hizmettir. Her yapılandırma deposu belirli bir Azure bölgesinde oluşturulur. Bölge genelinde ki kesinti, o bölgedeki tüm mağazaları etkiler. Uygulama Yapılandırması başka bir bölgeye otomatik arıza sunmaz. Bu makalede, uygulamanızın coğrafi esnekliğini artırmak için Azure bölgelerinde birden çok yapılandırma mağazasının nasıl kullanabileceğinize ilişkin genel kılavuz verilmektedir.

## <a name="high-availability-architecture"></a>Yüksek kullanılabilirlik mimarisi

Bölgeler arası artıklığı gerçekleştirmek için farklı bölgelerde birden çok Uygulama Yapılandırma mağazası oluşturmanız gerekir. Bu kurulumla, birincil depoya erişilemiyorsa uygulamanızın geri dönmesi gereken en az bir ek yapılandırma deposu vardır. Aşağıdaki diyagram, uygulamanız ile birincil ve ikincil yapılandırma depoları arasındaki topolojiyi göstermektedir:

![Coğrafi yedekmağazalar](./media/geo-redundant-app-configuration-stores.png)

Uygulamanız yapılandırmasını hem birincil hem de ikincil mağazalardan paralel olarak yükler. Bunu yapmak, yapılandırma verilerini başarıyla alma şansını artırır. Her iki depodaki verileri eşit tutmaktan siz sorumlusunuz. Aşağıdaki bölümler, uygulamanızda coğrafi esneklik oluşturmayı açıklar.

## <a name="failover-between-configuration-stores"></a>Yapılandırma mağazaları arasında failover

Teknik olarak, uygulamanız bir hata yürütmüyor. Aynı anda iki Uygulama Yapılandırma deposundan aynı yapılandırma veri kümesini almaya çalışıyor. Kodunuzu, önce ikincil depodan sonra da birincil mağazadan yüklenir şekilde düzenleyin. Bu yaklaşım, birincil depodaki yapılandırma verilerinin kullanılabilir olduğunda öncelikli olmasını sağlar. Aşağıdaki kod snippet bu düzenlemeyi .NET Core'da nasıl uygulayabileceğinizi gösterir:

#### <a name="net-core-2x"></a>[.NET Çekirdek 2.x](#tab/core2x)

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

#### <a name="net-core-3x"></a>[.NET Çekirdek 3.x](#tab/core3x)

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

İşlev `optional` içine geçen `AddAzureAppConfiguration` parametreye dikkat edin. `true`Ayarlandığında, bu parametre, işlev yapılandırma verilerini yükleyemezse uygulamanın devam etmesini önler.

## <a name="synchronization-between-configuration-stores"></a>Yapılandırma depoları arasında eşitleme

Coğrafi yedekli yapılandırma depolarınızın hepsinin aynı veri kümesine sahip olması önemlidir. İsteğe bağlı olarak birincil mağazadan ikincil mağazaya verileri kopyalamak için App Configuration'daki **Dışa** Aktarma işlevini kullanabilirsiniz. Bu işlev hem Azure portalı hem de CLI aracılığıyla kullanılabilir.

Azure portalından, aşağıdaki adımları izleyerek başka bir yapılandırma deposunda değişiklik yapabilirsiniz.

1. **İçe/Aktar** sekmesine gidin ve**Dışa Aktarma Uygulaması Yapılandırma** > **hedefini** >  **seçin** > **bir kaynak seçin.**

1. Açılan yeni bıçakta, ikincil mağazanızın aboneliğini, kaynak grubunu ve kaynak adını belirtin ve ardından **Uygula'yı**seçin.

1. UI, ikincil mağazanıza hangi yapılandırma verilerini aktarmak istediğinizi seçebilmeniz için güncelleştirilir. Varsayılan zaman değerini olduğu gibi bırakabilir ve hem **From etiketini** hem de **etiketi** aynı değere ayarlayabilirsiniz. **Uygula**’yı seçin.

1. Tüm yapılandırma değişiklikleri için önceki adımları yineleyin.

Bu dışa aktarma işlemini otomatikleştirmek için Azure CLI'yi kullanın. Aşağıdaki komut, birincil depodan ikincil mağazaya tek bir yapılandırma değişikliğinin nasıl dışa aktarılabildiğini gösterir:

```azurecli
    az appconfig kv export --destination appconfig --name {PrimaryStore} --label {Label} --dest-name {SecondaryStore} --dest-label {Label}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Uygulama Yapılandırması için çalışma süresi boyunca coğrafi esneklik elde etmek için uygulamanızı nasıl artırabileceğinizi öğrendiniz. Ayrıca, yapılandırma verilerini Uygulama Yapılandırması'ndan oluşturma veya dağıtım zamanında katıştırabilirsiniz. Daha fazla bilgi için [c/CD ardışık bir şekilde tümleştir'e](./integrate-ci-cd-pipeline.md)bakın.
