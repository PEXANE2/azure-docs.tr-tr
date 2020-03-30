---
title: Azure İşlevlerini kullanarak özel kullanılabilirlik testleri oluşturun ve çalıştırın
description: Bu doküman, TimerTrigger işlevinde verilen yapılandırmaya göre düzenli aralıklarla çalışacak TrackAvailability() ile bir Azure İşlevi'nin nasıl oluşturulacağını kapsayacaktır. Bu testin sonuçları, kullanılabilirlik sonuçları verilerini sorgulayıp uyarabileceğiniz Application Insights kaynağınıza gönderilir. Özelleştirilmiş testler, portal Kullanıcı Arabirimi'ni kullanarak mümkün olandan daha karmaşık kullanılabilirlik testleri yazmanızı, Azure VNET'inizin içindeki bir uygulamayı izlemenizi, bitiş noktası adresini değiştirmenizi veya bölgenizde kullanılamıyorsa kullanılabilirlik testi oluşturmanıza olanak tanır.
ms.topic: conceptual
author: morgangrobin
ms.author: mogrobin
ms.date: 11/22/2019
ms.openlocfilehash: 476d66c51c10a5fcfb3cb0319c47b3338d28812c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77665808"
---
# <a name="create-and-run-custom-availability-tests-using-azure-functions"></a>Azure İşlevlerini kullanarak özel kullanılabilirlik testleri oluşturun ve çalıştırın

Bu makalede, kendi iş mantığınızla TimerTrigger işlevinde verilen yapılandırmaya göre periyodik olarak çalışacak TrackAvailability() ile bir Azure İşlevi nasıl oluşturulacağı ele alınacaktır. Bu testin sonuçları, kullanılabilirlik sonuçları verilerini sorgulayıp uyarabileceğiniz Application Insights kaynağınıza gönderilir. Bu, portaldaki [Kullanılabilirlik İzleme](../../azure-monitor/app/monitor-web-app-availability.md) yoluyla yapabileceklerine benzer özelleştirilmiş testler oluşturmanıza olanak tanır. Özelleştirilmiş testler, portal Kullanıcı Arabirimi'ni kullanarak mümkün olandan daha karmaşık kullanılabilirlik testleri yazmanızı, Azure VNET'inizin içindeki bir uygulamayı izlemenizi, bitiş noktası adresini değiştirmenizi veya bu özellik bölgenizde kullanılamıyorsa bile kullanılabilirlik testi oluşturmanıza olanak tanır.

> [!NOTE]
> Bu örnek, yalnızca TrackAvailability() API çağrısının bir Azure İşlevi içinde nasıl çalıştığını niçin gösterdiğinigöstermek için tasarlanmıştır. Bunu tam işlevsel bir kullanılabilirlik testine dönüştürmek için gerekli olan temel HTTP Test kodu/iş mantığının nasıl yazılamayacağı değil. Varsayılan olarak, bu örnekte yürürseniz, her zaman bir hata oluşturacak bir kullanılabilirlik testi oluşturursunuz.

## <a name="create-timer-triggered-function"></a>Zamanlayıcı tetikleme işlevi oluşturma

- Uygulama Öngörüleri Kaynağınız varsa:
    - Varsayılan olarak Azure İşlevleri bir Uygulama Öngörüleri kaynağı oluşturur, ancak zaten oluşturulmuş kaynaklarınızdan birini kullanmak isterseniz, oluşturma sırasında bunu belirtmeniz gerekir.
    - Aşağıdaki seçeneklerle [bir Azure İşlevleri kaynağı nın ve Timer tetikleme işlevinin](https://docs.microsoft.com/azure/azure-functions/functions-create-scheduled-function) (temizlemeden önce dur) nasıl oluşturulacağına ilişkin yönergeleri izleyin.
        -  En üstteki **İzleme** sekmesini seçin.

            ![ Kendi App Insights kaynağınızla bir Azure İşlevler uygulaması oluşturun](media/availability-azure-functions/create-function-app.png)

        - Application Insights açılır kutusunu seçin ve kaynağınızın adını yazın veya yazın.

            ![Varolan Application Insights kaynağını seçme](media/availability-azure-functions/app-insights-resource.png)

        - **Gözden Geçir ' i** seçin + oluştur
- Zamanlayıcı tetikleme işleviniz için henüz oluşturulmuş bir Uygulama Öngörüleri Kaynağınız yoksa:
    - Varsayılan olarak Azure İşlevler uygulamanızı oluştururken sizin için bir Uygulama Öngörüleri kaynağı oluşturur.
    - [Azure İşlevleri kaynağının ve Timer tetikleme işlevinin](https://docs.microsoft.com/azure/azure-functions/functions-create-scheduled-function) nasıl oluşturulacağına ilişkin yönergeleri izleyin (temizlemeden önce durdurun).

## <a name="sample-code"></a>Örnek kod

Aşağıdaki kodu run.csx dosyasına kopyalayın (bu, önceden varolan kodun yerini alır). Bunu yapmak için Azure İşlevler uygulamanıza gidin ve soldaki zamanlayıcı tetikleyici işlevinizi seçin.

>[!div class="mx-imgBorder"]
>![Azure portalında Azure işlevinin run.csx'i](media/availability-azure-functions/runcsx.png)

> [!NOTE]
> Kullanacağınız Bitiş Noktası Adresi `EndpointAddress= https://dc.services.visualstudio.com/v2/track`için: . Kaynağınız Azure Kamu Veya Azure Çin gibi bir bölgede bulunmadığı sürece, bu durumda varsayılan uç noktaları geçersiz kılma konusunda bu [makaleye](https://docs.microsoft.com/azure/azure-monitor/app/custom-endpoints#regions-that-require-endpoint-modification) başvurun ve bölgeniz için uygun Telemetri Kanalı bitiş noktasını seçin.

```C#
#load "runAvailabilityTest.csx"
 
using System;
using System.Diagnostics;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
 
// The Application Insights Instrumentation Key can be changed by going to the overview page of your Function App, selecting configuration, and changing the value of the APPINSIGHTS_INSTRUMENTATIONKEY Application setting.
// DO NOT replace the code below with your instrumentation key, the key's value is pulled from the environment variable/application setting key/value pair.
private static readonly string instrumentationKey = Environment.GetEnvironmentVariable("APPINSIGHTS_INSTRUMENTATIONKEY");
 
//[CONFIGURATION_REQUIRED]
// If your resource is in a region like Azure Government or Azure China, change the endpoint address accordingly.
// Visit https://docs.microsoft.com/azure/azure-monitor/app/custom-endpoints#regions-that-require-endpoint-modification for more details.
private const string EndpointAddress = "https://dc.services.visualstudio.com/v2/track";
 
private static readonly TelemetryConfiguration telemetryConfiguration = new TelemetryConfiguration(instrumentationKey, new InMemoryChannel { EndpointAddress = EndpointAddress });
private static readonly TelemetryClient telemetryClient = new TelemetryClient(telemetryConfiguration);
 
public async static Task Run(TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"Entering Run at: {DateTime.Now}");
 
    if (myTimer.IsPastDue)
    {
        log.LogWarning($"[Warning]: Timer is running late! Last ran at: {myTimer.ScheduleStatus.Last}");
    }
 
    // [CONFIGURATION_REQUIRED] provide {testName} accordingly for your test function
    string testName = "AvailabilityTestFunction";
 
    // REGION_NAME is a default environment variable that comes with App Service
    string location = Environment.GetEnvironmentVariable("REGION_NAME");
 
    log.LogInformation($"Executing availability test run for {testName} at: {DateTime.Now}");
    string operationId = Guid.NewGuid().ToString("N");
 
    var availability = new AvailabilityTelemetry
    {
        Id = operationId,
        Name = testName,
        RunLocation = location,
        Success = false
    };
 
    var stopwatch = new Stopwatch();
    stopwatch.Start();
 
    try
    {
        await RunAvailbiltyTestAsync(log);
        availability.Success = true;
    }
    catch (Exception ex)
    {
        availability.Message = ex.Message;
 
        var exceptionTelemetry = new ExceptionTelemetry(ex);
        exceptionTelemetry.Context.Operation.Id = operationId;
        exceptionTelemetry.Properties.Add("TestName", testName);
        exceptionTelemetry.Properties.Add("TestLocation", location);
        telemetryClient.TrackException(exceptionTelemetry);
    }
    finally
    {
        stopwatch.Stop();
        availability.Duration = stopwatch.Elapsed;
        availability.Timestamp = DateTimeOffset.UtcNow;
 
        telemetryClient.TrackAvailability(availability);
        // call flush to ensure telemetry is sent
        telemetryClient.Flush();
    }
}

```

Görünüm dosyalarının sağ tarafında **Ekle'yi**seçin. Aşağıdaki yapılandırma ile yeni dosya **işlevi.proj** arayın.

```C#
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
        <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.8.2" /> <!-- Ensure you’re using the latest version -->
    </ItemGroup>
</Project>

```

>[!div class="mx-imgBorder"]
>![Sağdaki seç, ekleyin. Dosya işlevini adlandırın.proj](media/availability-azure-functions/addfile.png)

Görünüm dosyalarının sağ tarafında **Ekle'yi**seçin. Aşağıdaki yapılandırma ile yeni dosya **runAvailabilityTest.csx'i** arayın.

```C#
public async static Task RunAvailbiltyTestAsync(ILogger log)
{
    // Add your business logic here.
    throw new NotImplementedException();
}

```

## <a name="check-availability"></a>Kullanılabilirliği kontrol edin

Her şeyin çalıştığından emin olmak için, Application Insights kaynağınızın Kullanılabilirlik sekmesindeki grafiğe bakabilirsiniz.

> [!NOTE]
> RunAvailabilityTest.csx'te kendi iş mantığınızı uyguladıysanız, aşağıdaki ekran görüntülerinde olduğu gibi başarılı sonuçlar görürsünüz, eğer yapmadıysanız başarısız sonuçlar görürsünüz.

>[!div class="mx-imgBorder"]
>![Başarılı sonuçlarla kullanılabilirlik sekmesi](media/availability-azure-functions/availtab.png)

Testinizi Azure İşlevlerini kullanarak ayarladığınızda, Kullanılabilirlik sekmesinde **Test Ekle'yi** kullanmanın aksine testinizin adının görünmeyeceğini ve testle etkileşimkuramadığınızı fark edeceksiniz. Sonuçlar görselleştirilmiştir, ancak portal üzerinden bir kullanılabilirlik testi oluşturduğunuzda aldığınız aynı ayrıntılı görünüm yerine özet bir görünüm elde edersiniz.

Uçuça işlem ayrıntılarını görmek için, alıştırma altında **Başarılı** veya **Başarısız'ı** seçin ve ardından bir örnek seçin. Ayrıca grafikte bir veri noktası seçerek uçuca işlem ayrıntılarına da ulaşabilirsiniz.

>[!div class="mx-imgBorder"]
>![Örnek kullanılabilirlik testi seçin](media/availability-azure-functions/sample.png)

>[!div class="mx-imgBorder"]
>![Uçuca işlem ayrıntıları](media/availability-azure-functions/end-to-end.png)

Her şeyi olduğu gibi çalıştırırsanız (iş mantığı eklemeden), o zaman testin başarısız olduğunu görürsünüz.

## <a name="query-in-logs-analytics"></a>Günlüklerde Sorgula (Analytics)

Kullanılabilirlik sonuçlarını, bağımlılıkları ve daha fazlasını görüntülemek için Günlükler'i (analitik) kullanabilirsiniz. Günlükler hakkında daha fazla bilgi edinmek için [Günlük sorgusuna genel bakışı](../../azure-monitor/log-query/log-query-overview.md)ziyaret edin.

>[!div class="mx-imgBorder"]
>![Kullanılabilirlik sonuçları](media/availability-azure-functions/availabilityresults.png)

>[!div class="mx-imgBorder"]
>![Bağımlılıklar](media/availability-azure-functions/dependencies.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama Eşlemesi](../../azure-monitor/app/app-map.md)
- [İşlem tanılama](../../azure-monitor/app/transaction-diagnostics.md)
