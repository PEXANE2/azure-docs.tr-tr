---
title: Metrik telemetri için veri modeli - Azure Uygulama Öngörüleri
description: Metrik telemetri için Uygulama Öngörüleri veri modeli
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 3e4a1fc3de58b8e65ab9c7a288bdf3eb37e7bae0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671979"
---
# <a name="metric-telemetry-application-insights-data-model"></a>Metrik telemetri: Uygulama Öngörüleri veri modeli

[Uygulama Öngörüleri](../../azure-monitor/app/app-insights-overview.md)tarafından desteklenen iki tür metrik telemetri vardır: tek ölçüm ve önceden toplanmış metrik. Tek ölçüm sadece bir ad ve değerdir. Önceden toplanan metrik, metrik değerlendirme aralığındaki minimum ve maksimum değeri ve bunun standart sapması belirtir.

Önceden toplanmış metrik telemetri, toplama süresinin bir dakika olduğunu varsayar.

Application Insights tarafından desteklenen birkaç tanınmış metrik ad vardır. Bu ölçümler performanceCounters tablosuna yerleştirilir.

Sistem ve işlem sayaçlarını temsil eden metrik:

| **.NET adı**             | **Platform agnostik adı** | **REST API adı** | **Açıklama**
| ------------------------- | -------------------------- | ----------------- | ---------------- 
| `\Processor(_Total)\% Processor Time` | Çalışmalar devam ediyor... | [işlemciCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessorCpuPercentage) | toplam makine Işlemcisi
| `\Memory\Available Bytes`                 | Çalışmalar devam ediyor... | [memoryAvailableBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FmemoryAvailableBytes) | Bilgisayarda çalışan işlemler için kullanılabilen, baytlarda bulunan fiziksel bellek miktarını gösterir. Sıfırlanmış, boş ve bekleme bellek listelerindeki alan miktarı nın toplamı ile hesaplanır. Ücretsiz bellek kullanıma hazırdır; sıfırlanmış bellek, daha sonraki işlemlerin önceki bir işlem tarafından kullanılan verileri görmesini önlemek için sıfırlarla dolu bellek sayfalarından oluşur; bekleme belleği, diske giderken bir işlemin çalışma kümesinden (fiziksel belleği) kaldırılan ancak geri çağrılmak üzere hala kullanılabilen bellektir. [Bkz. Bellek Nesnesi](https://msdn.microsoft.com/library/ms804008.aspx)
| `\Process(??APP_WIN32_PROC??)\% Processor Time` | Çalışmalar devam ediyor... | [processCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessCpuPercentage) | Uygulamayı barındıran sürecin CPU'su
| `\Process(??APP_WIN32_PROC??)\Private Bytes`      | Çalışmalar devam ediyor... | [processPrivateBayt](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessPrivateBytes) | uygulamayı barındıran işlem tarafından kullanılan bellek
| `\Process(??APP_WIN32_PROC??)\IO Data Bytes/sec` | Çalışmalar devam ediyor... | [processIOBytesPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessIOBytesPerSecond) | uygulamayı barındıran prosese göre çalışan G/Ç işlemlerinin oranı
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec`             | Çalışmalar devam ediyor... | [istekleriPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsPerSecond) | uygulama tarafından işlenen isteklerin oranı 
| `\.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec`    | Çalışmalar devam ediyor... | [istisnalarPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FexceptionsPerSecond) | uygulama tarafından atılan istisnaoranı
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time`   | Çalışmalar devam ediyor... | [requestExecutionTime](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestExecutionTime) | ortalama istekleri yürütme süresi
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue` | Çalışmalar devam ediyor... | [requestsInQueue](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsInQueue) | kuyruktaki işleme için bekleyen istek sayısı

## <a name="name"></a>Adı

Application Insights portalında ve Kullanıcı Arabirimi'nde görmek istediğiniz metnin adı. 

## <a name="value"></a>Değer

Ölçüm için tek değer. Toplama için bireysel ölçümlerin toplamı.

## <a name="count"></a>Sayı

Toplanan ölçümün metrik ağırlığı. Bir ölçüm için ayarlanmamalıdır.

## <a name="min"></a>Min

Toplanan ölçümün minimum değeri. Bir ölçüm için ayarlanmamalıdır.

## <a name="max"></a>Maks

Toplanan ölçümün maksimum değeri. Bir ölçüm için ayarlanmamalıdır.

## <a name="standard-deviation"></a>Standart sapma

Toplanan metrik standart sapma. Bir ölçüm için ayarlanmamalıdır.

## <a name="custom-properties"></a>Özel özellikler

Metrik, metnin `CustomPerfCounter` windows `true` performans sayacını temsil ettiğini belirtmek için özel özellik kümesine sahip. Bu ölçümler performanceCounters tablosuna yerleştirilir. CustomMetrics'te değil. Ayrıca bu ölçümün adı kategori, sayaç ve örnek adlarını ayıklamak için ayrıştırılır.

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Özel olaylar [ve ölçümler için Uygulama Öngörüleri API'sini](../../azure-monitor/app/api-custom-events-metrics.md#trackmetric)nasıl kullanacağınızı öğrenin.
- Uygulama Öngörüleri türleri ve veri modeli için [veri modeline](data-model.md) bakın.
- Application Insights tarafından desteklenen [platformlara](../../azure-monitor/app/platforms.md) göz atın.
