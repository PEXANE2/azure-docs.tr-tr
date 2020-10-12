---
title: Azure Application Insights-Azure Işlevleri desteklenen özellikler
description: Azure Işlevleri için desteklenen özellikler Application Insights
ms.topic: reference
author: TimothyMothra
ms.author: tilee
ms.date: 4/23/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 333bba2b1d3cd83457196e38b827daa78199f235
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87033522"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Azure Işlevleri 'nin desteklediği özellikler için Application Insights

Azure Işlevleri, ILogger arabirimi aracılığıyla kullanılabilen Application Insights ile [yerleşik tümleştirme](../../azure-functions/functions-monitoring.md) sunar. Şu anda desteklenen özelliklerin listesi aşağıda verilmiştir. [Kullanmaya başlamak](../../azure-functions/functions-monitoring.md#enable-application-insights-integration)Için Azure işlevleri Kılavuzu ' nu gözden geçirin.

Işlevler çalışma zamanı sürümleri hakkında daha fazla bilgi için [buraya](../../azure-functions/functions-versions.md)bakın.

Application Insights uyumlu sürümleri hakkında daha fazla bilgi için bkz. [Bağımlılıklar](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/).

## <a name="supported-features"></a>Desteklenen özellikler

| Azure İşlevleri                       | V1                | V2 & V3   | 
|-----------------------------------    |---------------    |------------------ |
| | | | 
| **Otomatik toplama**        |                 |                   |               
| &bull; İstekleri                     | Evet             | Evet               | 
| &bull; Larý                   | Evet             | Evet               | 
| &bull; Performans sayaçları         | Evet             | Evet               |
| &bull; Bağlantılıdır                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|                 | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash; EventHub  |                 | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | Evet               | 
| | | | 
| **Desteklenen özellikler**                |                   |                   |               
| &bull; QuickPulse/Liveölçümleri       | Evet             | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash; Güvenli denetim kanalı|                 | Evet               | 
| &bull; Aşağıdakine                     | Evet             | Evet               | 
| &bull; Sinyal                   |                 | Evet               | 
| | | | 
| **Bağıntı**                       |                   |                   |               
| &bull; ServiceBus                     |                   | Evet               | 
| &bull; EventHub                       |                   | Evet               | 
| | | | 
| **Yapılandırılabilir**                      |                   |                   |           
| &bull;Tamamen yapılandırılabilir.<br/>Yönergeler için bkz. [Azure işlevleri](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) .<br/>Tüm seçenekler için bkz. [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) .               |                   | Evet                   | 


## <a name="performance-counters"></a>Performans Sayaçları

Performans sayaçlarının otomatik toplanması yalnızca Windows makinelerini işler.


## <a name="live-metrics--secure-control-channel"></a>Canlı ölçümler & güvenli denetim kanalı

Belirttiğiniz özel filtreler ölçütü, Application Insights SDK 'sindeki canlı ölçümler bileşenine geri gönderilir. Filtreler potansiyel olarak CustomerIDs gibi hassas bilgileri içerebilir. Kanalı gizli bir API anahtarı ile güvenli hale getirebilirsiniz. Yönergeler için bkz. [Denetim kanalını güvenli hale getirme](./live-stream.md#secure-the-control-channel) .

## <a name="sampling"></a>Örnekleme

Azure Işlevleri, yapılandırmasında varsayılan olarak örnekleme imkanı sunar. Daha fazla bilgi için bkz. [örneklemesi yapılandırma](../../azure-functions/functions-monitoring.md#configure-sampling).

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
