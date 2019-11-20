---
title: Azure uygulama yapılandırma dayanıklılığı ve olağanüstü durum kurtarma | Microsoft Docs
description: Azure Uygulama yapılandırması ile dayanıklılık ve olağanüstü durum kurtarma uygulama hakkında genel bakış.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 05/29/2019
ms.author: yegu
ms.openlocfilehash: 291f6fe48d81397d293ab54a73e777831e25f6ea
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185272"
---
# <a name="resiliency-and-disaster-recovery"></a>Dayanıklılık ve olağanüstü durum kurtarma

Şu anda Azure Uygulama yapılandırması bölgesel bir hizmettir. Her yapılandırma deposu belirli bir Azure bölgesinde oluşturulur. Bölge genelinde kesinti, bu bölgedeki tüm depoları etkiler. Uygulama yapılandırması başka bir bölgeye otomatik yük devretme sunmaz. Bu makalede, uygulamanızın coğrafi dayanıklılığını artırmak için Azure bölgelerinde birden çok yapılandırma deposunu nasıl kullanabileceğiniz hakkında genel yönergeler sunulmaktadır.

## <a name="high-availability-architecture"></a>Yüksek kullanılabilirlik mimarisi

Bölgeler arası yedeklilik sağlamak için, farklı bölgelerde birden çok uygulama yapılandırma deposu oluşturmanız gerekir. Bu kurulumla, uygulamanızda birincil depo erişilemez hale gelirse geri dönecek en az bir ek yapılandırma deposu vardır. Aşağıdaki diyagramda, uygulamanız ve birincil ve ikincil yapılandırma depoları arasındaki topoloji gösterilmektedir:

![Coğrafi olarak yedekli depolar](./media/geo-redundant-app-configuration-stores.png)

Uygulamanız, yapılandırmasını hem birincil hem de ikincil depolardan paralel olarak yükler. Bunu yapmak, yapılandırma verilerini başarıyla alma olasılığını artırır. Verilerin her iki mağazasında da eşitlenmiş durumda tutulması sizin sorumluluğunuzdadır. Aşağıdaki bölümlerde, uygulamanıza nasıl coğrafi dayanıklılık oluşturabileceğiniz açıklanmaktadır.

## <a name="failover-between-configuration-stores"></a>Yapılandırma depoları arasında yük devretme

Teknik olarak, uygulamanız yük devretme yürütmez. Aynı anda iki uygulama yapılandırma mağazasında aynı yapılandırma verisi kümesini almaya çalışıyor. Kodunuzu önce ikincil depodan, sonra da birincil mağazadan yüklenecek şekilde düzenleyin. Bu yaklaşım, birincil depodaki yapılandırma verilerinin kullanılabilir olduğunda öncelikli olmasını sağlar. Aşağıdaki kod parçacığı, bu düzenlemeyi .NET Core CLI nasıl uygulayabileceğinizi göstermektedir:

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
    }
```

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

