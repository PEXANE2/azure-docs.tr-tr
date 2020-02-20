---
title: Azure Cloud Services performans sayaçlarında toplayın | Microsoft Docs
description: Azure Tanılama ve Application Insights ile Cloud Services performans sayaçlarını bulmayı, kullanmayı ve oluşturmayı öğrenin.
services: cloud-services
documentationcenter: .net
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 02/02/2018
ms.author: tagore
ms.openlocfilehash: 3b4028a09f69acd5d7a6579b4610785ed32e227d
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77469536"
---
# <a name="collect-performance-counters-for-your-azure-cloud-service"></a>Azure bulut hizmetiniz için performans sayaçlarını toplayın

Performans sayaçları, uygulamanızın ve konağın ne kadar iyi performans gösterdiğini izlemeniz için bir yol sağlar. Windows Server, donanım, uygulamalar, işletim sistemi ve daha fazlası ile ilgili birçok farklı performans sayacı sağlar. Performans sayaçlarını toplayıp Azure 'a göndererek, daha iyi kararlar almanıza yardımcı olması için bu bilgileri çözümleyebilirsiniz. 

## <a name="discover-available-counters"></a>Kullanılabilir sayaçları keşfet

Bir performans sayacı iki bölümden oluşur, küme adı (kategori olarak da bilinir) ve bir veya daha fazla sayaç. Kullanılabilir performans sayaçlarının listesini almak için PowerShell kullanabilirsiniz:

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

`CounterSetName` özelliği bir kümeyi (veya kategorisini) temsil eder ve performans sayaçlarının ilişkili olduğu iyi bir göstergedir. `Paths` özelliği, bir küme için sayaçların koleksiyonunu temsil eder. Ayrıca, sayaç kümesi hakkında daha fazla bilgi için `Description` özelliğini de alabilirsiniz.

Bir küme için tüm sayaçları almak üzere `CounterSetName` değerini kullanın ve `Paths` koleksiyonunu genişletin. Her yol öğesi, sorgulayabilmeniz için kullanabileceğiniz bir sayaçtır. Örneğin, `Processor` kümesiyle ilgili kullanılabilir sayaçları almak için `Paths` koleksiyonunu genişletin:

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

Bu bireysel sayaç yolları, bulut hizmetinizin kullandığı tanılama çerçevesine eklenebilir. Performans sayacı yolunun nasıl oluşturulduğu hakkında daha fazla bilgi için bkz. [sayaç yolu belirtme](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85)).

## <a name="collect-a-performance-counter"></a>Bir performans sayacı toplayın

Azure Tanılama veya Application Insights için bulut hizmetinize bir performans sayacı eklenebilir.

### <a name="application-insights"></a>Application Insights

Cloud Services için Azure Application Insights, hangi performans sayaçlarını toplamak istediğinizi belirtmenize olanak tanır. [Projenize Application Insights](../azure-monitor/app/cloudservices.md#sdk)ekledikten sonra, Visual Studio projenize **ApplicationInsights. config** adlı bir yapılandırma dosyası eklenir. Bu yapılandırma dosyası, hangi tür bilgilerin Application Insights toplayıp Azure 'a göndereceğini tanımlar.

**ApplicationInsights. config** dosyasını açın ve **ApplicationInsights** > **TelemetryModules** öğesini bulun. Her bir `<Add>` alt öğesi, kendi yapılandırması ile birlikte toplanacak bir telemetri türünü tanımlar. Performans sayacı telemetri modülü türü `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector`. Bu öğe zaten tanımlanmışsa, ikinci kez eklemeyin. Toplanacak her performans sayacı `<Counters>`adlı bir düğüm altında tanımlanmıştır. Sürücü performans sayaçlarını toplayan bir örnek aşağıda verilmiştir:

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

Her performans sayacı `<Counters>`altında bir `<Add>` öğesi olarak gösterilir. `PerformanceCounter` özniteliği toplanacak performans sayacını tanımlar. `ReportAs` özniteliği, performans sayacı için Azure portal görüntülenecek başlıktır. Topladığınız herhangi bir performans sayacı, portalda **özel** adlı bir kategoriye konur. Azure Tanılama farklı olarak, bu performans sayaçlarının toplandığı ve Azure 'a gönderildiği aralığı ayarlayamazsınız. Application Insights, performans sayaçları her dakikada toplanır ve gönderilir. 

Application Insights, aşağıdaki performans sayaçlarını otomatik olarak toplar:

* \Process (?? APP_WIN32_PROC??)\% Işlemci zamanı
* \Memory\Available Bytes
* \.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec
* \Process(??APP_WIN32_PROC??)\Private Bytes
* \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
* \Processor(_Total)\% Processor Time

Daha fazla bilgi için bkz. [Azure Cloud Services için](../azure-monitor/app/cloudservices.md#performance-counters)Application Insights ve Application Insights [Sistem performans sayaçları](../azure-monitor/app/performance-counters.md) .

### <a name="azure-diagnostics"></a>Azure Tanılama

> [!IMPORTANT]
> Bu veriler depolama hesabında toplanırken, Portal verileri grafiğe yönelik yerel bir yol **sağlamaz.** Application Insights gibi başka bir tanılama hizmetini uygulamanıza tümleştirmeniz kesinlikle önerilir.

Cloud Services için Azure Tanılama uzantısı, hangi performans sayaçlarını toplamak istediğinizi belirtmenizi sağlar. Azure Tanılama ayarlamak için bkz. [bulut hizmeti Izlemeye genel bakış](cloud-services-how-to-monitor.md#setup-diagnostics-extension).

Toplamak istediğiniz performans sayaçları, **Diagnostics. wadcfgx** dosyasında tanımlanmıştır. Visual Studio 'da bu dosyayı açın (rol başına tanımlanmıştır) ve **Diagnosticsconfiguration** > **Publicconfig** > **Wadcfg** > **diagnosticmonitorconfiguration** > **PerformanceCounters** öğesini bulun. Alt öğe olarak yeni bir **PerformanceCounterConfiguration** öğesi ekleyin. Bu öğe iki özniteliğe sahiptir: `counterSpecifier` ve `sampleRate`. `counterSpecifier` özniteliği, hangi sistem performans sayacı kümesinin (önceki bölümde ana hatlarıyla açıklanmıştır) toplanacağını tanımlar. `sampleRate` değeri, bu değerin ne sıklıkta sorgulandığını gösterir. Bir bütün olarak, tüm performans sayaçları üst `PerformanceCounters` öğenin `scheduledTransferPeriod` öznitelik değerine göre Azure 'a aktarılır.

`PerformanceCounters` şeması öğesi hakkında daha fazla bilgi için [Azure tanılama şemasına](../azure-monitor/platform/diagnostics-extension-schema-windows.md#performancecounters-element)bakın.

`sampleRate` özniteliği tarafından tanımlanan süre, performans sayacının ne sıklıkla yoklandığını göstermek için XML Duration veri türünü kullanır. Aşağıdaki örnekte, oran `PT3M`olarak ayarlanır, bu da üç dakikada bir `[P]eriod[T]ime[3][M]inutes`: her üç dakikada bir.

`sampleRate` ve `scheduledTransferPeriod` nasıl tanımlandığı hakkında daha fazla bilgi için, [w3 XML tarih ve saat tarih türleri](https://www.w3schools.com/XML/schema_dtypes_date.asp) öğreticisindeki **süre veri türü** bölümüne bakın.

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

## <a name="create-a-new-perf-counter"></a>Yeni bir performans sayacı oluşturun

Kodunuz tarafından yeni bir performans sayacı oluşturulabilir ve kullanılabilir. Yeni bir performans sayacı oluşturan kodunuzun yükseltilmiş olarak çalışıyor olması gerekir, aksi takdirde başarısız olur. Bulut hizmetiniz `OnStart` başlangıç kodu, rolü yükseltilmiş bir bağlamda çalıştırmanızı gerektiren performans sayacını oluşturabilir. Ya da yükseltilmiş olarak çalışan ve performans sayacını oluşturan bir başlangıç görevi oluşturabilirsiniz. Başlangıç görevleri hakkında daha fazla bilgi için bkz. [bir bulut hizmeti için başlangıç görevlerini yapılandırma ve çalıştırma](cloud-services-startup-tasks.md).

Rolünüzü yükseltilmiş olarak çalışacak şekilde yapılandırmak için [. csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) dosyasına bir `<Runtime>` öğesi ekleyin.

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

Birkaç satırlık kodla yeni bir performans sayacı oluşturup kaydedebilirsiniz. Hem kategoriyi hem de sayacı oluşturan `System.Diagnostics.PerformanceCounterCategory.Create` yöntemi aşırı yüklemesini kullanın. Aşağıdaki kod, kategorinin mevcut olup olmadığını denetler ve eksikse, hem kategori hem de sayacı oluşturur.

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

Sayacı kullanmak istediğinizde `Increment` veya `IncrementBy` yöntemini çağırın.

```csharp
// Increase the counter by 1
counterServiceUsed.Increment();
```

Uygulamanız özel Sayaçlarınızı kullandığından, sayacı izlemek için Azure Tanılama veya Application Insights yapılandırmanız gerekir.


### <a name="application-insights"></a>Application Insights

Daha önce belirtildiği gibi, Application Insights için performans sayaçları **ApplicationInsights. config** dosyasında tanımlanmıştır. **ApplicationInsights. config** dosyasını açın ve **ApplicationInsights** > **TelemetryModules** >  > **sayaçlarını** **Ekle** öğesini bulun. Bir `<Add>` alt öğesi oluşturun ve `PerformanceCounter` özniteliğini kodunuzda oluşturduğunuz performans sayacının kategorisi ve adı olarak ayarlayın. `ReportAs` özniteliğini portalda görmek istediğiniz kolay bir ad olarak ayarlayın.

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

Daha önce belirtildiği gibi, toplamak istediğiniz performans sayaçları **Diagnostics. wadcfgx** dosyasında tanımlanmıştır. Visual Studio 'da bu dosyayı açın (rol başına tanımlanmıştır) ve **Diagnosticsconfiguration** > **Publicconfig** > **Wadcfg** > **diagnosticmonitorconfiguration** > **PerformanceCounters** öğesini bulun. Alt öğe olarak yeni bir **PerformanceCounterConfiguration** öğesi ekleyin. `counterSpecifier` özniteliğini kodunuzda oluşturduğunuz performans sayacının kategorisi ve adı olarak ayarlayın. 

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

- [Azure Cloud Services Application Insights](../azure-monitor/app/cloudservices.md#performance-counters)
- [Application Insights 'de sistem performans sayaçları](../azure-monitor/app/performance-counters.md)
- [Sayaç yolu belirtme](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85))
- [Azure Tanılama şeması-performans sayaçları](../azure-monitor/platform/diagnostics-extension-schema-windows.md#performancecounters-element)



