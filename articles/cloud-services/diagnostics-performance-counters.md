---
title: Azure Bulut Hizmetlerinde Performans Sayaçlarında Topla | Microsoft Dokümanlar
description: Azure Tanılama ve Uygulama Öngörüleri ile Bulut Hizmetleri'nde performans sayaçlarını nasıl keşfeder, kullanacağınızı ve oluşturabileceğinizi öğrenin.
services: cloud-services
documentationcenter: .net
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 02/02/2018
ms.author: tagore
ms.openlocfilehash: 3b4028a09f69acd5d7a6579b4610785ed32e227d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77469536"
---
# <a name="collect-performance-counters-for-your-azure-cloud-service"></a>Azure Bulut Hizmetiniz için performans sayaçları toplama

Performans sayaçları, uygulamanızın ve ana bilgisayar ınızın ne kadar iyi performans gösterdiğini izlemeniz için bir yol sağlar. Windows Server donanım, uygulamalar, işletim sistemi ve daha fazlası ile ilgili birçok farklı performans sayaçları sağlar. Performans sayaçlarını toplayıp Azure'a göndererek, daha iyi kararlar almaya yardımcı olmak için bu bilgileri analiz edebilirsiniz. 

## <a name="discover-available-counters"></a>Kullanılabilir sayaçları keşfedin

Performans sayacı iki bölümden, bir küme adı (kategori olarak da bilinir) ve bir veya daha fazla sayaçtan oluşur. Kullanılabilir performans sayaçlarının listesini almak için PowerShell'i kullanabilirsiniz:

```powershell
Get-Counter -ListSet * | Select-Object CounterSetName, Paths | Sort-Object CounterSetName

CounterSetName                                  Paths
--------------                                  -----
.NET CLR Data                                   {\.NET CLR Data(*)\SqlClient...
.NET CLR Exceptions                             {\.NET CLR Exceptions(*)\# o...
.NET CLR Interop                                {\.NET CLR Interop(*)\# of C...
.NET CLR Jit                                    {\.NET CLR Jit(*)\# of Metho...
.NET Data Provider for Oracle                   {\.NET Data Provider for Ora...
.NET Data Provider for SqlServer                {\.NET Data Provider for Sql...
.NET Memory Cache 4.0                           {\.NET Memory Cache 4.0(*)\C...
AppV Client Streamed Data Percentage            {\AppV Client Streamed Data ...
ASP.NET                                         {\ASP.NET\Application Restar...
ASP.NET Apps v4.0.30319                         {\ASP.NET Apps v4.0.30319(*)...
ASP.NET State Service                           {\ASP.NET State Service\Stat...
ASP.NET v2.0.50727                              {\ASP.NET v2.0.50727\Applica...
ASP.NET v4.0.30319                              {\ASP.NET v4.0.30319\Applica...
Authorization Manager Applications              {\Authorization Manager Appl...

#... results cut to save space ...
```

Özellik `CounterSetName` bir seti (veya kategoriyi) temsil eder ve performans sayaçlarının neyle ilişkili olduğunu iyi bir göstergesidir. Özellik, `Paths` bir küme için sayaçkoleksiyonunu temsil eder. Ayrıca sayaç kümesi `Description` hakkında daha fazla bilgi için özellik alabilirsiniz.

Bir küme için tüm sayaçları almak `CounterSetName` için değeri `Paths` kullanın ve koleksiyonu genişletin. Her yol öğesi sorgulayabileceğiniz bir sayaçtır. Örneğin, `Processor` kümeyle ilgili kullanılabilir sayaçları almak için `Paths` koleksiyonu genişletin:

```powershell
Get-Counter -ListSet * | Where-Object CounterSetName -eq "Processor" | Select -ExpandProperty Paths

\Processor(*)\% Processor Time
\Processor(*)\% User Time
\Processor(*)\% Privileged Time
\Processor(*)\Interrupts/sec
\Processor(*)\% DPC Time
\Processor(*)\% Interrupt Time
\Processor(*)\DPCs Queued/sec
\Processor(*)\DPC Rate
\Processor(*)\% Idle Time
\Processor(*)\% C1 Time
\Processor(*)\% C2 Time
\Processor(*)\% C3 Time
\Processor(*)\C1 Transitions/sec
\Processor(*)\C2 Transitions/sec
\Processor(*)\C3 Transitions/sec
```

Bu tek tek karşı yollar, bulut hizmetinin kullandığı tanılama çerçevesine eklenebilir. Performans sayacı yolunun nasıl oluşturulduru hakkında daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85))

## <a name="collect-a-performance-counter"></a>Performans sayacı topla

Azure Tanılama veya Uygulama Öngörüleri için bulut hizmetinize bir performans sayacı eklenebilir.

### <a name="application-insights"></a>Application Insights

Bulut Hizmetleri için Azure Uygulama Öngörüleri, hangi performans sayaçlarını toplamak istediğinizi belirtmenize olanak tanır. [Projenize Application Insights ekledikten](../azure-monitor/app/cloudservices.md#sdk)sonra Visual Studio projenize **ApplicationInsights.config** adlı bir config dosyası eklenir. Bu config dosyası, Application Insights'ın ne tür bilgileri toplayıp Azure'a gönderdiğini tanımlar.

**ApplicationInsights.config** dosyasını açın ve **ApplicationInsights** > **TelemetriModules** öğesini bulun. Her `<Add>` alt öğe, yapılandırmasıyla birlikte toplanacak bir telemetri türünü tanımlar. Performans sayacı telemetri `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector`modül türü . Bu öğe zaten tanımlanmışsa, ikinci kez eklemeyin. Toplanacak her performans sayacı, adı `<Counters>`verilen bir düğüm altında tanımlanır. Sürücü performans sayaçlarını toplayan bir örnek aşağıda verilmiştir:

```xml
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">

  <TelemetryModules>

    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\LogicalDisk(C:)\Disk Write Bytes/sec" ReportAs="Disk write (C:)" />
        <Add PerformanceCounter="\LogicalDisk(C:)\Disk Read Bytes/sec" ReportAs="Disk read (C:)" />
      </Counters>
    </Add>

  </TelemetryModules>

<!-- ... cut to save space ... -->
```

Her performans sayacı altında `<Add>` `<Counters>`bir öğe olarak temsil edilir. Öznitelik, `PerformanceCounter` hangi performans sayacının toplandığını tanımlar. Öznitelik, `ReportAs` performans sayacı için Azure portalında görüntülenecek başlıktır. Topladığınız herhangi bir performans sayacı portalda **Özel** adlı bir kategoriye konur. Azure Tanılama'nın aksine, bu performans sayaçlarının toplanıp Azure'a gönderilme aralığını ayarlayamazsınız. Application Insights ile performans sayaçları her dakika toplanır ve gönderilir. 

Application Insights aşağıdaki performans sayaçlarını otomatik olarak toplar:

* \Süreç(?? APP_WIN32_PROC??) \% İşlemci Süresi
* \Memory\Available Bytes
* \.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec
* \Process(??APP_WIN32_PROC??)\Private Bytes
* \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
* \Processor(_Total)\% Processor Time

Daha fazla bilgi için, Azure Bulut Hizmetleri için Uygulama Öngörüleri ve Uygulama [Öngörüleri'ndeki Sistem performans](../azure-monitor/app/performance-counters.md) [sayaçlarına](../azure-monitor/app/cloudservices.md#performance-counters)bakın.

### <a name="azure-diagnostics"></a>Azure Tanılama

> [!IMPORTANT]
> Tüm bu veriler depolama hesabına toplanmış olsa da, portal verileri grafiklemek için yerel bir yol **sağlamaz.** Uygulama Öngörüleri gibi başka bir tanılama hizmetini uygulamanıza entegre eleştirmeniz önerilir.

Bulut Hizmetleri için Azure Tanılama uzantısı, hangi performans sayaçlarını toplamak istediğinizi belirtmenize olanak tanır. Azure Tanılama'yı ayarlamak için [Bulut Hizmeti İzleme Genel Bakış'a](cloud-services-how-to-monitor.md#setup-diagnostics-extension)bakın.

Toplamak istediğiniz performans sayaçları **diagnostics.wadcfgx** dosyasında tanımlanır. Visual Studio'da bu dosyayı açın (rol başına tanımlanır) ve **DiagnosticsConfiguration** > **PublicConfig** > **WadCfg** > **DiagnosticMonitorConfiguration** > **PerformanceCounters** öğesini bulun. Alt öğe olarak yeni bir **PerformanceCounterConfiguration** öğesi ekleyin. Bu öğenin iki `counterSpecifier` `sampleRate`özniteliği vardır: ve . Öznitelik, `counterSpecifier` hangi sistem performans sayacı kümesinin (önceki bölümde özetlenen) toplanmasını tanımlar. Değer, `sampleRate` bu değerin ne sıklıkta yoklendiğini gösterir. Bir bütün olarak, tüm performans sayaçları üst `PerformanceCounters` öğenin `scheduledTransferPeriod` öznitelik değerine göre Azure'a aktarılır.

`PerformanceCounters` Şema öğesi hakkında daha fazla bilgi için [Azure Tanılama Şeması'na](../azure-monitor/platform/diagnostics-extension-schema-windows.md#performancecounters-element)bakın.

Öznitelik tarafından `sampleRate` tanımlanan dönem, performans sayacının ne sıklıkta yoklandığını belirtmek için XML süresi veri türünü kullanır. Aşağıdaki örnekte, oran `PT3M`, yani `[P]eriod[T]ime[3][M]inutes`: her üç dakikada bir ayarlanır.

Nasıl tanımlandığı `sampleRate` ve `scheduledTransferPeriod` nasıl tanımlandığı hakkında daha fazla bilgi için [W3 XML Tarih ve Saat Tarih Türleri](https://www.w3schools.com/XML/schema_dtypes_date.asp) öğreticisinde Süre Veri **Türü** bölümüne bakın.

```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig>
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096">

        <!-- ... cut to save space ... -->

        <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />

          <!-- This is a new perf counter which will track the C: disk read activity in bytes per second, every minute -->
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(C:)\Disk Read Bytes/sec" sampleRate="PT1M" />

        </PerformanceCounters>
      </DiagnosticMonitorConfiguration>
    </WadCfg>
    
    <!-- ... cut to save space ... -->

  </PublicConfig>
</DiagnosticsConfiguration>
```

## <a name="create-a-new-perf-counter"></a>Yeni bir perf sayacı oluşturma

Yeni bir performans sayacı oluşturulabilir ve kodunuz tarafından kullanılabilir. Yeni bir performans sayacı oluşturan kodunuz yüksek çalışıyor olmalı, aksi takdirde başarısız olur. Bulut hizmeti `OnStart` başlangıç kodunuz, rolü yükseltilmiş bir bağlamda çalıştırmanızı gerektiren performans sayacıoluşturabilir. Veya yükseltilmiş çalışan ve performans sayacı nı oluşturan bir başlangıç görevi oluşturabilirsiniz. Başlangıç görevleri hakkında daha fazla bilgi için, [bulut hizmeti için başlangıç görevlerini nasıl yapılandırıp çalıştırılabilirsiniz.](cloud-services-startup-tasks.md)

Rolünüzü yüksek çalışacak şekilde yapılandırmak `<Runtime>` için [.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) dosyasına bir öğe ekleyin.

```xml
<ServiceDefinition name="CloudServiceLoadTesting" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRoleWithSBQueue1" vmsize="Large">

    <!-- ... cut to save space ... -->

    <Runtime executionContext="elevated">
      
    </Runtime>

    <!-- ... cut to save space ... -->

  </WorkerRole>
</ServiceDefinition>
```

Birkaç kod satırıyla yeni bir performans sayacı oluşturabilir ve kaydedebilirsiniz. Hem `System.Diagnostics.PerformanceCounterCategory.Create` kategoriyi hem de sayacı oluşturan aşırı yükleme yöntemini kullanın. Aşağıdaki kod ilk olarak kategorinin var olup olmadığını denetler ve eksikse hem kategoriyi hem de sayacı oluşturur.

```csharp
using System.Diagnostics;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRoleWithSBQueue1
{
    public class WorkerRole : RoleEntryPoint
    {
        // Perf counter variable representing times service was used.
        private PerformanceCounter counterServiceUsed;

        public override bool OnStart()
        {
            // ... Other startup code here ...

            // Define the category and counter names.
            string perfCounterCatName = "MyService";
            string perfCounterName = "Times Used";

            // Create the counter if needed. Our counter category only has a single counter.
            // Both the category and counter are created in the same method call.
            if (!PerformanceCounterCategory.Exists(perfCounterCatName))
            {
                PerformanceCounterCategory.Create(perfCounterCatName, "Collects information about the cloud service.", 
                                                  PerformanceCounterCategoryType.SingleInstance, 
                                                  perfCounterName, "How many times the cloud service was used.");
            }

            // Get reference to our counter
            counterServiceUsed = new PerformanceCounter(perfCounterCatName, perfCounterName);
            counterServiceUsed.ReadOnly = false;
            
            return base.OnStart();
        }

        // ... cut class code to save space
    }
}
```

Sayacı kullanmak istediğinizde, aramayı `Increment` `IncrementBy` veya yöntemi arayın.

```csharp
// Increase the counter by 1
counterServiceUsed.Increment();
```

Uygulamanız özel sayacınızı kullandığına göre, sayacı izlemek için Azure Tanılama veya Uygulama Öngörüleri'ni yapılandırmanız gerekir.


### <a name="application-insights"></a>Application Insights

Daha önce de belirtildiği gibi, Application Insights için performans sayaçları **ApplicationInsights.config** dosyasında tanımlanır. **Open ApplicationInsights.config** ve **ApplicationInsights** > **TelemetryModules** > **Kontrpuanlar** **öğesini bulun.** >  Bir `<Add>` alt öğe oluşturun `PerformanceCounter` ve özniteliği nizkodunızda oluşturduğunuz performans sayacının kategorisine ve adına ayarlayın. `ReportAs` Özniteliği portalda görmek istediğiniz dost bir ada ayarlayın.

```xml
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">

  <TelemetryModules>

    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <!-- ... cut other perf counters to save space ... -->

        <!-- This new perf counter matches the [category name]\[counter name] defined in your code -->
        <Add PerformanceCounter="\MyService\Times Used" ReportAs="Service used counter" />
      </Counters>
    </Add>

  </TelemetryModules>

<!-- ... cut to save space ... -->
```

### <a name="azure-diagnostics"></a>Azure Tanılama

Daha önce belirtildiği gibi, toplamak istediğiniz performans sayaçları **diagnostics.wadcfgx** dosyasında tanımlanır. Visual Studio'da bu dosyayı açın (rol başına tanımlanır) ve **DiagnosticsConfiguration** > **PublicConfig** > **WadCfg** > **DiagnosticMonitorConfiguration** > **PerformanceCounters** öğesini bulun. Alt öğe olarak yeni bir **PerformanceCounterConfiguration** öğesi ekleyin. `counterSpecifier` Özniteliği, kodunuzda oluşturduğunuz performans sayacının kategorisine ve adına ayarlayın. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig>
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096">

        <!-- ... cut to save space ... -->

        <PerformanceCounters scheduledTransferPeriod="PT1M">
          <!-- ... cut other perf counters to save space ... -->
          
          <!-- This new perf counter matches the [category name]\[counter name] defined in your code -->
          <PerformanceCounterConfiguration counterSpecifier="\MyService\Times Used" sampleRate="PT1M" />

        </PerformanceCounters>
      </DiagnosticMonitorConfiguration>
    </WadCfg>
    
    <!-- ... cut to save space ... -->

  </PublicConfig>
</DiagnosticsConfiguration>
```

## <a name="more-information"></a>Daha fazla bilgi

- [Azure Cloud Services için Application Insights](../azure-monitor/app/cloudservices.md#performance-counters)
- [Uygulama Öngörüleri'nde sistem performans sayaçları](../azure-monitor/app/performance-counters.md)
- [Karşı Yol Belirtme](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85))
- [Azure Tanılama Şeması - Performans Sayaçları](../azure-monitor/platform/diagnostics-extension-schema-windows.md#performancecounters-element)



