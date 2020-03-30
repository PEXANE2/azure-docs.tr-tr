---
title: Azure Uygulama Öngörülerinde Telemetri kanalları | Microsoft Dokümanlar
description: .NET ve .NET Core için Azure Application Insights SDK'larında telemetri kanalları nasıl özelleştirilir?
ms.topic: conceptual
ms.date: 05/14/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 9c292246f947e4d3a364f79b31fe7a1deebd33d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275704"
---
# <a name="telemetry-channels-in-application-insights"></a>Uygulama Öngörülerinde Telemetri kanalları

Telemetri kanalları [Azure Uygulama Öngörüleri SDK'larının](../../azure-monitor/app/app-insights-overview.md)ayrılmaz bir parçasıdır. Arabellemeve telemetrinin Application Insights hizmetine iletilmesini yönetirler. SDK'ların .NET ve .NET Core sürümlerinde iki dahili telemetri kanalı vardır: `InMemoryChannel` ve `ServerTelemetryChannel`. Bu makalede, kanal davranışının nasıl özelleştirilen dahil olmak üzere her kanal ayrıntılı olarak açıklanmaktadır.

## <a name="what-are-telemetry-channels"></a>Telemetri kanalları nedir?

Telemetri kanalları, telemetri öğelerini arabelleğe almak ve sorgulama ve analiz için depoladıkları Application Insights hizmetine göndermekten sorumludur. Bir telemetri kanalı [`Microsoft.ApplicationInsights.ITelemetryChannel`](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.channel.itelemetrychannel?view=azure-dotnet) arabirimi uygulayan herhangi bir sınıftır.

Tüm `Send(ITelemetry item)` telemetri başförleri ve telemetri işlemcileri çağrıldıktan sonra bir telemetri kanalı nın yöntemi çağrılır. Bu nedenle, bir telemetri işlemcisi tarafından bırakılan herhangi bir öğe kanala ulaşmaz. `Send()`öğeleri genellikle anında arka uca göndermez. Genellikle, bunları bellekte arabelleğe alır ve verimli iletim için toplu olarak gönderir.

[Canlı Ölçümler Akışı](live-stream.md) da telemetri canlı akışı güçler özel bir kanal vardır. Bu kanal normal telemetri kanalından bağımsızdır ve bu belge bu kanal için geçerli değildir.

## <a name="built-in-telemetry-channels"></a>Dahili telemetri kanalları

Uygulama Öngörüleri .NET ve .NET Core SDK'lar iki dahili kanalla gemi:

* `InMemoryChannel`: Öğeleri gönderilene kadar bellekte arabelleğe alan hafif bir kanal. Öğeler bellekte arabelleğe alınır ve her 30 saniyede bir veya 500 öğe arabelleğe alındığında kızartılır. Bu kanal, bir arızadan sonra telemetri göndermeye çalışmadığından en az güvenilirlik garantisi sunar. Bu kanal öğeleri diskte tutmaz, bu nedenle gönderilmemiş öğeler uygulama nın kapatılmasında kalıcı olarak kaybolur (zarif olsun veya olmasın). Bu kanal, `Flush()` bellekteki telemetri öğelerini eşzamanlı olarak zorlamak için kullanılabilecek bir yöntem uygular. Bu kanal, senkron floş ideal olan kısa süreli uygulamalar için uygundur.

    Bu kanal, daha büyük Microsoft.ApplicationInsights NuGet paketinin bir parçasıdır ve başka hiçbir şey yapılandırılmadığında SDK'nın kullandığı varsayılan kanaldır.

* `ServerTelemetryChannel`: Yeniden deneme ilkeleri ve verileri yerel bir diskte depolama özelliğine sahip daha gelişmiş bir kanal. Bu kanal, geçici hatalar oluşursa telemetri göndermeyi yeniden dener. Bu kanal, ağ kesintileri veya yüksek telemetri hacimleri sırasında öğeleri diskte tutmak için yerel disk depolama sını da kullanır. Bu yeniden deneme mekanizmaları ve yerel disk depolama nedeniyle, bu kanal daha güvenilir olarak kabul edilir ve tüm üretim senaryoları için önerilir. Bu kanal, resmi belgelere göre yapılandırılan [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) ve [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) uygulamaları için varsayılan dır. Bu kanal, uzun süren işlemlere sahip sunucu senaryoları için optimize edilebiyi kullanır. Bu [`Flush()`](#which-channel-should-i-use) kanal tarafından uygulanan yöntem eşzamanlı değildir.

    Bu kanal Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel NuGet paketi olarak gönderilir ve Microsoft.ApplicationInsights.Web veya Microsoft.ApplicationInsights.AspNetCore NuGet'i kullandığınızda otomatik olarak elde edilir Paket.

## <a name="configure-a-telemetry-channel"></a>Bir telemetri kanalInı yapılandırma

Bir telemetri kanalını etkin telemetri yapılandırmasına ayarlayarak yapılandırAbilirsiniz. ASP.NET uygulamalar için yapılandırma, telemetri kanalı `TelemetryConfiguration.Active`örneğini '' `ApplicationInsights.config`veya değiştirerek ayarlamayı içerir. ASP.NET Core uygulamaları için yapılandırma, kanalı Bağımlılık Enjeksiyon Kapsayıcısı'na eklemeyi içerir.

Aşağıdaki bölümlerde, kanal `StorageFolder` ayarı çeşitli uygulama türlerinde yapılandırma örnekleri gösterilmektedir. `StorageFolder`yapılandırılabilir ayarlardan yalnızca biridir. Yapılandırma ayarlarının tam listesi için, bu makalenin ilerleyen bölümlerinde [ayarlar bölümüne](telemetry-channels.md#configurable-settings-in-channels) bakın.

### <a name="configuration-by-using-applicationinsightsconfig-for-aspnet-applications"></a>ASP.NET uygulamalar için ApplicationInsights.config kullanarak yapılandırma

[ApplicationInsights.config'in](configuration-with-applicationinsights-config.md) aşağıdaki `ServerTelemetryChannel` `StorageFolder` bölümü, ayarlanmış olarak yapılandırılan kanalı özel bir konuma gösterir:

```xml
    <TelemetrySinks>
        <Add Name="default">
            <TelemetryProcessors>
                <!-- Telemetry processors omitted for brevity  -->
            </TelemetryProcessors>
            <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel">
                <StorageFolder>d:\temp\applicationinsights</StorageFolder>
            </TelemetryChannel>
        </Add>
    </TelemetrySinks>
```

### <a name="configuration-in-code-for-aspnet-applications"></a>ASP.NET uygulamaları için kod yapılandırması

Aşağıdaki kod, özel bir konuma `StorageFolder` ayarlanmış bir 'ServerTelemetryChannel' örneğini ayarlar. Bu kodu uygulamanın başında, genellikle Global.aspx.cs `Application_Start()` yönteminde ekleyin.

```csharp
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
protected void Application_Start()
{
    var serverTelemetryChannel = new ServerTelemetryChannel();
    serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
    serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
    TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
}
```

### <a name="configuration-in-code-for-aspnet-core-applications"></a>ASP.NET Core uygulamaları için kod yapılandırması

Burada `ConfigureServices` gösterildiği gibi `Startup.cs` sınıfın yöntemini değiştirin:

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

public void ConfigureServices(IServiceCollection services)
{
    // This sets up ServerTelemetryChannel with StorageFolder set to a custom location.
    services.AddSingleton(typeof(ITelemetryChannel), new ServerTelemetryChannel() {StorageFolder = @"d:\temp\applicationinsights" });

    services.AddApplicationInsightsTelemetry();
}

```

> [!IMPORTANT]
> ASP.NET Core uygulamaları `TelemetryConfiguration.Active` için kanalı kullanarak yapılandırma nız önerilmez.

### <a name="configuration-in-code-for-netnet-core-console-applications"></a>.NET/.NET Core konsol uygulamaları için kod yapılandırması

Konsol uygulamaları için kod hem .NET hem de .NET Core için aynıdır:

```csharp
var serverTelemetryChannel = new ServerTelemetryChannel();
serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
```

## <a name="operational-details-of-servertelemetrychannel"></a>ServerTelemetryChannel'ın operasyonel detayları

`ServerTelemetryChannel`gelen öğeleri bellek arabelleğinde saklar. Öğeler seri hale getirilir, sıkıştırılır ve `Transmission` her 30 saniyede bir veya 500 öğe arabelleğe alındığında bir örneğe depolanır. Tek `Transmission` bir örnek 500'e kadar öğe içerir ve Application Insights hizmetine tek bir HTTPS çağrısı üzerinden gönderilen bir telemetri toplu nu temsil eder.

Varsayılan olarak, en fazla `Transmission` 10 örnek paralel olarak gönderilebilir. Telemetri daha hızlı hızlarda geliyorsa veya ağ veya Application Insights `Transmission` arka uç yavaşsa, örnekler bellekte depolanır. Bu bellek `Transmission` içi arabelleğin varsayılan kapasitesi 5 MB'dır. Bellek içi kapasite aşıldığında, `Transmission` örnekler yerel diskte 50 MB'a kadar depolanır. `Transmission`örnekler, ağ sorunları olduğunda da yerel diskte depolanır. Yalnızca yerel bir diskte depolanan öğeler bir uygulama çökmesi hayatta. Uygulama yeniden başladığında gönderilirler.

## <a name="configurable-settings-in-channels"></a>Kanallarda yapılandırılabilir ayarlar

Her kanal için yapılandırılabilir ayarların tam listesi için bkz:

* [InMemoryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/BASE/src/Microsoft.ApplicationInsights/Channel/InMemoryChannel.cs)

* [ServerTelemetryKanal](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/BASE/src/ServerTelemetryChannel/ServerTelemetryChannel.cs)

Burada en `ServerTelemetryChannel`sık kullanılan ayarları şunlardır:

1. `MaxTransmissionBufferCapacity`: Bellekteki aktarımları arabelleğe almak için kanal tarafından kullanılan, baytlarda maksimum bellek miktarı. Bu kapasiteye ulaşıldığında, yeni öğeler doğrudan yerel diske depolanır. Varsayılan değer 5 MB'dır. Daha yüksek bir değer ayarlamak daha az disk kullanımına yol açar, ancak uygulama çökerse bellekteki öğelerin kaybolacağını unutmayın.

1. `MaxTransmissionSenderCapacity`: Aynı anda Application Insights'a gönderilecek en fazla `Transmission` örnek sayısı. Varsayılan değer 10'dur. Bu ayar, büyük bir telemetri hacmi oluşturulduğunda önerilen daha yüksek bir sayıya yapılandırılabilir. Yüksek hacim genellikle yük testi sırasında veya örnekleme kapatıldığında oluşur.

1. `StorageFolder`: Gerektiğinde öğeleri diske depolamak için kanal tarafından kullanılan klasör. Windows'da, başka bir yol açıkça belirtilmemişse % LOCALAPPDATA% veya %TEMP% kullanılır. Windows dışındaki ortamlarda geçerli bir konum belirtmeniz gerekir veya telemetri yerel diskte depolanmayacak.

## <a name="which-channel-should-i-use"></a>Hangi kanalı kullanmalıyım?

`ServerTelemetryChannel`uzun süren uygulamaları içeren çoğu üretim senaryosu için önerilir. Uygulanan `Flush()` `ServerTelemetryChannel` yöntem eşzamanlı değildir ve bellek veya diskten bekleyen tüm öğelerin gönderilmesini de garanti etmez. Bu kanalı, uygulamanın kapanmak üzere olduğu senaryolarda kullanıyorsanız, aradıktan `Flush()`sonra biraz gecikme yapmanızı öneririz. Tam olarak gerektirebilecek gecikme miktarı tahmin edilebilir değildir. Bellekte kaç öğe veya `Transmission` örnek olduğu, diskte kaç tane olduğu, kaç tanesinin arka uca aktarıldığı ve kanalın üstel geri dönüş senaryolarının ortasında olup olmadığı gibi etkenlere bağlıdır.

Senkron floş yapmanız gerekiyorsa, kullanmanızı `InMemoryChannel`öneririz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="does-the-application-insights-channel-guarantee-telemetry-delivery-if-not-what-are-the-scenarios-in-which-telemetry-can-be-lost"></a>Application Insights kanalı telemetri teslimatını garanti ediyor mu? Değilse, telemetrinin kaybedilebildiği senaryolar nelerdir?

Kısa cevap, yerleşik kanalların hiçbirinin arka uca telemetri teslimatı için işlem tipi bir garanti sunmasıdır. `ServerTelemetryChannel`güvenilir teslimat `InMemoryChannel` için karşılaştırıldığında daha gelişmiş, ama aynı zamanda telemetri göndermek için sadece en iyi çaba girişimi yapar. Telemetri hala bu yaygın senaryolar da dahil olmak üzere çeşitli durumlarda kaybolabilir:

1. Uygulama çöktüğinde bellekteki öğeler kaybolur.

1. Telemetri ağ sorunlarının uzun süre lerde kaybolur. Telemetri, ağ kesintileri sırasında veya Application Insights arka uçta sorunlar oluştuğunda yerel diske depolanır. Ancak, 48 saatten eski öğeler atılır.

1. Telemetriyi Windows'da depolamak için varsayılan disk konumları %LOCALAPPDATA% veya %TEMP% 'dir. Bu konumlar genellikle makinenin yereldir. Uygulama fiziksel olarak bir konumdan diğerine taşınırsa, özgün konumda depolanan herhangi bir telemetri kaybolur.

1. Windows'daki Web Apps'ta varsayılan disk depolama konumu D:\local\LocalAppData'dır. Bu yer kalıcı değil. Uygulama yeniden başlatmalarında, ölçeklendirmelerde ve diğer bu tür işlemlerde silinir ve bu da orada depolanan herhangi bir telemetrinin kaybolmasına yol açarak. Varsayılanı geçersiz kılabilir ve depolama alanını D:\home gibi kalıcı bir konuma belirtebilirsiniz. Ancak, bu tür kalıcı konumlar uzak depolama tarafından sunulan ve bu yüzden yavaş olabilir.

### <a name="does-servertelemetrychannel-work-on-systems-other-than-windows"></a>ServerTelemetryChannel Windows dışındaki sistemlerde çalışıyor mu?

Paketinin adı ve ad alanı "WindowsServer"ı içerse de, bu kanal aşağıdaki istisna lar dışında Windows dışındaki sistemlerde desteklenir. Windows dışındaki sistemlerde kanal varsayılan olarak yerel bir depolama klasörü oluşturmaz. Yerel bir depolama klasörü oluşturmanız ve kanalı kullanacak şekilde yapılandırmanız gerekir. Yerel depolama yapılandırıldıktan sonra, kanal tüm sistemlerde aynı şekilde çalışır.

### <a name="does-the-sdk-create-temporary-local-storage-is-the-data-encrypted-at-storage"></a>SDK geçici yerel depolama alanı oluşturuyor mu? Veriler depoda şifrelenmiş mi?

SDK, telemetri öğelerini ağ sorunları sırasında veya azaltma sırasında yerel depolama alanında depolar. Bu veriler yerel olarak şifrelenmez.

Windows sistemleri için SDK, %TEMP% veya %LOCALAPPDATA% dizininde otomatik olarak geçici bir yerel klasör oluşturur ve yalnızca yöneticilere ve geçerli kullanıcıya erişimi kısıtlar.

Windows dışındaki sistemlerde, SDK tarafından otomatik olarak yerel depolama alanı oluşturulmaz ve bu nedenle varsayılan olarak hiçbir veri yerel olarak depolanır. Bir depolama dizini kendiniz oluşturabilir ve kanalı kullanacak şekilde yapılandırabilirsiniz. Bu durumda, dizinin güvenli olduğundan emin olmak sizin sorumluluğunuzdadır.
[Veri koruma ve gizlilik](data-retention-privacy.md#does-the-sdk-create-temporary-local-storage)hakkında daha fazla bilgi edinin.

## <a name="open-source-sdk"></a>Açık kaynak SDK
Uygulama Öngörüleri için her SDK gibi, kanallar açık kaynak kodvardır. [Resmi GitHub repo'sunda](https://github.com/Microsoft/ApplicationInsights-dotnet)kodu okuyun ve koda katkıda bulunun veya sorunları bildirin.

## <a name="next-steps"></a>Sonraki adımlar

* [Örnekleme](../../azure-monitor/app/sampling.md)
* [SDK Sorun Giderme](../../azure-monitor/app/asp-net-troubleshoot-no-data.md)
