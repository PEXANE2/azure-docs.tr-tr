---
title: Azure Uygulama Öngörüleri - Azure İşlevleri Desteklenen Özellikler
description: Azure İşlevleri için Uygulama Öngörüleri Desteklenen Özellikler
ms.topic: reference
author: TimothyMothra
ms.author: tilee
ms.date: 4/23/2019
ms.reviewer: mbullwin
ms.openlocfilehash: cf0c97fd65f9966bf42fa22e2c8f92263952cb7a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77655659"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Azure İşlevleri için Uygulama Öngörüleri desteklenen özellikler

Azure İşlevleri, ILogger Arabirimi üzerinden kullanılabilen Application Insights ile [yerleşik tümleştirme](../../azure-functions/functions-monitoring.md) sunar. Aşağıda şu anda desteklenen özelliklerin listesi verilmiştir. Başlangıç için Azure İşlevler kılavuzunu gözden [geçirin.](../../azure-functions/functions-monitoring.md#enable-application-insights-integration)

İşlevler çalışma zamanı sürümleri hakkında daha fazla bilgi için [buraya](../../azure-functions/functions-versions.md)bakın.

Uygulama Öngörüleri'nin uyumlu sürümleri hakkında daha fazla bilgi için [Bkz.](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/)

## <a name="supported-features"></a>Desteklenen özellikler

| Azure İşlevleri                       | V1                | V2 & V3   | 
|-----------------------------------    |---------------    |------------------ |
| | | | 
| **Otomatik toplama**        |                 |                   |               
| &bull;Istek                     | Evet             | Evet               | 
| &bull;Özel durum                   | Evet             | Evet               | 
| &bull;Performans Sayaçları         | Evet             | Evet               |
| &bull;Bağımlılık                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash;HTTP Kı.,      |                 | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash;ServiceBus|                 | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash;EventHub  |                 | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash;Sql       |                 | Evet               | 
| | | | 
| **Desteklenen özellikler**                |                   |                   |               
| &bull;QuickPulse/ LiveMetrics       | Evet             | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash;Güvenli Kontrol Kanalı|                 | Evet               | 
| &bull;Örnekleme                     | Evet             | Evet               | 
| &bull;Sinyal                   |                 | Evet               | 
| | | | 
| **Bağıntı**                       |                   |                   |               
| &bull;ServiceBus                     |                   | Evet               | 
| &bull;EventHub                       |                   | Evet               | 
| | | | 
| **Yapılandırılabilir**                      |                   |                   |           
| &bull;Tamamen yapılandırılabilir.<br/>Yönergeler için [Azure İşlevleri'ne](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) bakın.<br/>Tüm seçenekler için [Asp.NET Core'a](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) bakın.               |                   | Evet                   | 


## <a name="performance-counters"></a>Performans Sayaçları

Performans Sayaçları otomatik toplama yalnızca Windows makineleri çalışır.


## <a name="live-metrics--secure-control-channel"></a>Canlı Ölçümler & Güvenli Kontrol Kanalı

Belirlediğiniz özel filtreler ölçütleri, Uygulama Öngörüleri SDK'daki Canlı Ölçümler bileşenine geri gönderilir. Filtreler, customerID'ler gibi hassas bilgiler içerebilir. Gizli bir API tuşu ile kanalı güvenli hale getirebilirsiniz. Bkz. Talimatlar için [denetim kanalını Güvenli'ye](https://docs.microsoft.com/azure/azure-monitor/app/live-stream#secure-the-control-channel) bakın.

## <a name="sampling"></a>Örnekleme

Azure İşlevleri, yapılandırmalarında varsayılan olarak Örnekleme'yi sağlar. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling)

Projeniz el ile telemetri izleme yapmak için Application Insights SDK'ya bağımlı ysa, örnekleme yapılandırmanız Fonksiyonların örnekleme yapılandırmandan farklıysa garip davranışlarla karşılaşabilirsiniz. 

İşlevler ile aynı yapılandırmayı kullanmanızı öneririz. **Functions v2**ile, oluşturucunuzda bağımlılık enjeksiyonunu kullanarak aynı yapılandırmayı elde edebilirsiniz:

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Extensibility;

public class Function1 
{

    private readonly TelemetryClient telemetryClient;

    public Function1(TelemetryConfiguration configuration)
    {
        this.telemetryClient = new TelemetryClient(configuration);
    }

    [FunctionName("Function1")]
    public async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, ILogger logger)
    {
        this.telemetryClient.TrackTrace("C# HTTP trigger function processed a request.");
    }
}
```
