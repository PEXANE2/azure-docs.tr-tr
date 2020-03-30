---
title: Windows masaüstü uygulamaları için kullanımı ve performansı izleme
description: Application Insights ile Windows masaüstü uygulamanızın kullanımını ve performansını analiz edin.
ms.topic: conceptual
ms.date: 10/29/2019
ms.openlocfilehash: 8234b9ba2c92fc64cfa8f598db99954e00caab45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670840"
---
# <a name="monitoring-usage-and-performance-in-classic-windows-desktop-apps"></a>Klasik Windows Masaüstü uygulamalarında kullanımı ve performansı izleme

Şirket içinde, Azure’da ve diğer bulutlarda barındırılan tüm uygulamalar Application Insights’tan faydalanabilir. Tek sınırlama Application Insights hizmetine [iletişim izni verme](../../azure-monitor/app/ip-addresses.md) gerekliliğidir. Evrensel Windows Platformu (UWP) uygulamalarını izlemek için [Visual Studio App Center](../../azure-monitor/learn/mobile-center-quickstart.md)’ı öneririz.

## <a name="to-send-telemetry-to-application-insights-from-a-classic-windows-application"></a>Bir Klasik Windows uygulamasından Application Insights’a telemetri göndermek için
1. [Azure portalında](https://portal.azure.com)[bir Application Insights kaynağı oluşturun](../../azure-monitor/app/create-new-resource.md ). Uygulama türü olarak ASP.NET uygulamasını seçin.
2. İzleme Anahtarının bir kopyasını oluşturun. Yeni oluşturduğunuz kaynağın Temel Bileşenler açılan penceresinde anahtarı bulun. 
3. Visual Studio’da uygulama projenizin NuGet paketlerini düzenleyin ve şunu ekleyin: Microsoft.ApplicationInsights.WindowsServer. (Alternatif olarak, standart telemetri toplama modülleri olmaksızın yalnızca API’nın kendisini istiyorsanız Microsoft.ApplicationInsights seçeneğini belirleyin.)
4. İzleme anahtarını kodunuzda ayarlayın:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "` *anahtarınız* `";`
   
    veya ApplicationInsights.config öğesinde ayarlayın (standart telemetri paketlerinden birini yüklediyseniz):
   
    `<InstrumentationKey>`*anahtarınız*`</InstrumentationKey>` 
   
    ApplicationInsights.config dosyasını kullanırsanız, bunun özelliklerinin **Build Action = Content, Copy to Output Directory = Copy** olarak ayarlandığından emin olun.
5. Telemetri göndermek için [API’yi kullanın](../../azure-monitor/app/api-custom-events-metrics.md).
6. Uygulamanızı çalıştırın ve Azure portalında oluşturduğunuz kaynaktaki telemetriyi görün.

## <a name="example-code"></a><a name="telemetry"></a>Örnek kod
```csharp
using Microsoft.ApplicationInsights;

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
        {
            e.Cancel = true;

            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="override-storage-of-computer-name"></a>Bilgisayar adının depolamayı geçersiz kılma

Varsayılan olarak bu SDK, telemetri yayan sistemin bilgisayar adını toplar ve saklar. Koleksiyonu geçersiz kılmak için bir telemetri Initializer kullanmanız gerekir:

**Aşağıdaki gibi özel TelemetryInitializer yazın.**

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace CustomInitializer.Telemetry
{
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                //set custom role name here, you can pass an empty string if needed.
                  telemetry.Context.Cloud.RoleInstance = "Custom RoleInstance";
            }
        }
    }
}
```
Enstrümantasyon anahtarını `Program.cs` `Main()` ayarlayarak aşağıdaki yöntemde başharfi anında belirleyin:

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;

   static void Main()
        {
            TelemetryConfiguration.Active.InstrumentationKey = "{Instrumentation-key-here}";
            TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
        }
```

## <a name="next-steps"></a>Sonraki adımlar
* [Pano oluşturma](../../azure-monitor/app/overview-dashboard.md)
* [Tanılama Araması](../../azure-monitor/app/diagnostic-search.md)
* [Ölçümleri keşfetme](../../azure-monitor/app/metrics-explorer.md)
* [Analytics sorguları yazma](../../azure-monitor/app/analytics.md)

