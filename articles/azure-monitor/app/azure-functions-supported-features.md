---
title: Azure Application Insights-Azure Işlevleri desteklenen özellikler
description: Azure Işlevleri için desteklenen özellikler Application Insights
ms.topic: reference
author: TimothyMothra
ms.author: tilee
ms.date: 4/23/2019
ms.reviewer: mbullwin
ms.openlocfilehash: cf0c97fd65f9966bf42fa22e2c8f92263952cb7a
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77655659"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Azure Işlevleri 'nin desteklediği özellikler için Application Insights

Azure Işlevleri, ILogger arabirimi aracılığıyla kullanılabilen Application Insights ile [yerleşik tümleştirme](../../azure-functions/functions-monitoring.md) sunar. Şu anda desteklenen özelliklerin listesi aşağıda verilmiştir. [Kullanmaya başlamak](../../azure-functions/functions-monitoring.md#enable-application-insights-integration)Için Azure işlevleri Kılavuzu ' nu gözden geçirin.

Işlevler çalışma zamanı sürümleri hakkında daha fazla bilgi için [buraya](../../azure-functions/functions-versions.md)bakın.

Application Insights uyumlu sürümleri hakkında daha fazla bilgi için bkz. [Bağımlılıklar](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/).

## <a name="supported-features"></a>Desteklenen özellikler

| Azure İşlevleri                       | V1                | V2 & v3   | 
|-----------------------------------    |---------------    |------------------ |
| | | | 
| **Otomatik toplama**        |                 |                   |               
| &bull; Istekleri                     | Yes             | Yes               | 
| &bull; özel durumlar                   | Yes             | Yes               | 
| &bull; performans sayaçları         | Yes             | Yes               |
| &bull; bağımlılıklar                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | Yes               | 
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|                 | Yes               | 
| &nbsp;&nbsp;&nbsp;&mdash; EventHub  |                 | Yes               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | Yes               | 
| | | | 
| **Desteklenen özellikler**                |                   |                   |               
| QuickPulse/Liveölçümlerini &bull;       | Yes             | Yes               | 
| &nbsp;&nbsp;&nbsp;&mdash; güvenli denetim kanalı|                 | Yes               | 
| Örnekleme &bull;                     | Yes             | Yes               | 
| &bull; sinyalleri                   |                 | Yes               | 
| | | | 
| **Bağıntılı**                       |                   |                   |               
| &bull; ServiceBus                     |                   | Yes               | 
| EventHub &bull;                       |                   | Yes               | 
| | | | 
| **Yapılandırılabilir**                      |                   |                   |           
| &bull;tamamen yapılandırılabilir.<br/>Yönergeler için bkz. [Azure işlevleri](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) .<br/>Tüm seçenekler için bkz. [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) .               |                   | Yes                   | 


## <a name="performance-counters"></a>Performans Sayaçları

Performans sayaçlarının otomatik toplanması yalnızca Windows makinelerini işler.


## <a name="live-metrics--secure-control-channel"></a>Canlı ölçümler & güvenli denetim kanalı

Belirttiğiniz özel filtreler ölçütü, Application Insights SDK 'sindeki canlı ölçümler bileşenine geri gönderilir. Filtreler potansiyel olarak CustomerIDs gibi hassas bilgileri içerebilir. Kanalı gizli bir API anahtarı ile güvenli hale getirebilirsiniz. Yönergeler için bkz. [Denetim kanalını güvenli hale getirme](https://docs.microsoft.com/azure/azure-monitor/app/live-stream#secure-the-control-channel) .

## <a name="sampling"></a>Örnekleme

Azure Işlevleri, yapılandırmasında varsayılan olarak örnekleme imkanı sunar. Daha fazla bilgi için bkz. [örneklemesi yapılandırma](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling).

Projeniz, el ile Telemetriyi izlemeyi yapmak üzere Application Insights SDK üzerinde bir bağımlılık alırsa, örnekleme yapılandırmanız Işlevlerin örnekleme yapılandırmasından farklı olduğunda garip davranışlar yaşayabilirsiniz. 

Aynı yapılandırmayı Işlevlerle kullanmanızı öneririz. **İşlev v2**ile, oluşturucuya bağımlılık ekleme kullanarak aynı yapılandırmayı edinebilirsiniz:

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
