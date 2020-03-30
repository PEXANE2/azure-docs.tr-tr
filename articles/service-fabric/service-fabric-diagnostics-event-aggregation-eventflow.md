---
title: EventFlow ile Azure Servis Kumaşı Olay Toplama
description: Azure Hizmet Kumaşı kümelerinin izlenmesi ve teşhisi için EventFlow'u kullanarak olayları toplama ve toplama hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 2/25/2019
ms.author: srrengar
ms.openlocfilehash: cde24657cc8ed78b91e72df16d51df4077a6e030
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75463082"
---
# <a name="event-aggregation-and-collection-using-eventflow"></a>EventFlow'u kullanarak olay toplama ve toplama

[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) olayları bir düğümden bir veya daha fazla izleme noktasına yönlendirebilir. Hizmet projenize Bir NuGet paketi olarak dahil olduğundan, Etkinlik Akışı kodu ve yapılandırma hizmetiyle birlikte seyahat ederek Azure Tanılama hakkında daha önce bahsedilen düğüm başına yapılandırma sorununu ortadan kaldırır. EventFlow hizmet sürecinizde çalışır ve doğrudan yapılandırılan çıktılara bağlanır. Doğrudan bağlantı sayesinde EventFlow Azure, kapsayıcı ve şirket içi hizmet dağıtımları için çalışır. Her EventFlow ardışık hattı harici bir bağlantı yaptığından, EventFlow'u kapsayıcıgibi yüksek yoğunluklu senaryolarda çalıştırırsanız dikkatli olun. Yani, çeşitli işlemlerev sahipliği varsa, birkaç giden bağlantıları olsun! Bu, Aynı işlemdeki bir `ServiceType` çalıştırmanın tüm kopyaları olduğundan, Hizmet Kumaşı uygulamaları için çok fazla bir sorun değildir ve bu giden bağlantı sayısını sınırlar. EventFlow ayrıca olay filtreleme de sunar, böylece yalnızca belirtilen filtreyle eşleşen olaylar gönderilir.

## <a name="set-up-eventflow"></a>EventFlow'u ayarlama

EventFlow ikilileri NuGet paketleri kümesi olarak kullanılabilir. Service Fabric servis projesine EventFlow eklemek için Solution Explorer'daki projeyi sağ tıklatın ve "NuGet paketlerini yönet"i seçin. "Gözat" sekmesine geçin ve`Diagnostics.EventFlow`" " için arama yapın:

![Visual Studio NuGet paket yöneticisi UI'de EventFlow NuGet paketleri](./media/service-fabric-diagnostics-event-aggregation-eventflow/eventflow-nuget.png)

"Girişler" ve "Çıktılar" ile etiketlenmiş çeşitli paketlerin bir listesinin ortaya çıkarolduğunu görürsünüz. EventFlow, çeşitli günlük sağlayıcılarını ve çözümleyicilerini destekler. EventFlow'u barındıran hizmet, uygulama günlüklerinin kaynağına ve hedefine bağlı olarak uygun paketleri içermelidir. Temel ServiceFabric paketine ek olarak, en az bir Giriş ve Çıktı yapılandırılmış olması gerekir. Örneğin, EventSource olaylarını Uygulama Öngörülerine göndermek için aşağıdaki paketleri ekleyebilirsiniz:

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource`hizmetin EventSource sınıfından ve *Microsoft-ServiceFabric-Services* ve *Microsoft-ServiceFabric-Actors*gibi standart EventSource'lardan veri yakalamak için )
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights`(günlükleri bir Azure Application Insights kaynağına göndereceğiz)
* `Microsoft.Diagnostics.EventFlow.ServiceFabric`(Service Fabric hizmet yapılandırmasından EventFlow ardışık akışının başlatılmasını sağlar ve tanılama verilerinin Service Fabric sistem durumu raporları olarak gönderilmesiyle ilgili sorunları bildirir)

>[!NOTE]
>`Microsoft.Diagnostics.EventFlow.Inputs.EventSource`paketin .NET Framework 4.6 veya daha yeni sini hedeflemesi için hizmet projesi nin gerekli liği vardır. Bu paketi yüklemeden önce proje özelliklerinde uygun hedef çerçeveyi ayarladığınızdan emin olun.

Tüm paketler yüklendikten sonra, bir sonraki adım, hizmette EventFlow'u yapılandırmak ve etkinleştirmektir.

## <a name="configure-and-enable-log-collection"></a>Günlük koleksiyonunu yapılandırma ve etkinleştirme
Günlükleri göndermekten sorumlu EventFlow ardışık bir yapılandırma dosyasında depolanan bir belirtim oluşturulur. Paket, `Microsoft.Diagnostics.EventFlow.ServiceFabric` çözüm klasörü altında `PackageRoot\Config` bir başlangıç EventFlow yapılandırma dosyası yükler, adlı. `eventFlowConfig.json` Bu yapılandırma dosyasının varsayılan hizmet `EventSource` sınıfından ve yapılandırmak istediğiniz diğer girdilerden veri yakalamak ve verileri uygun yere göndermek için değiştirilmesi gerekir.

>[!NOTE]
>Proje dosyanızda VisualStudio 2017 `eventFlowConfig.json` formatında varsa dosya otomatik olarak eklenmez. Bunu düzeltmek için `Config` klasördeki dosyayı oluşturun ve `Copy if newer`yapı eylemini .'a ayarlayın. 

Burada yukarıda belirtilen NuGet paketleri dayalı örnek bir *olayFlowConfig.json:*
```json
{
  "inputs": [
    {
      "type": "EventSource",
      "sources": [
        { "providerName": "Microsoft-ServiceFabric-Services" },
        { "providerName": "Microsoft-ServiceFabric-Actors" },
        // (replace the following value with your service's ServiceEventSource name)
        { "providerName": "your-service-EventSource-name" }
      ]
    }
  ],
  "filters": [
    {
      "type": "drop",
      "include": "Level == Verbose"
    }
  ],
  "outputs": [
    {
      "type": "ApplicationInsights",
      // (replace the following value with your AI resource's instrumentation key)
      "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
  ],
  "schemaVersion": "2016-08-11"
}
```

Hizmetin ServiceEventSource adı, ServiceEventSource sınıfına `EventSourceAttribute` uygulanan Ad özelliğinin değeridir. Tüm servis kodunun `ServiceEventSource.cs` bir parçası olan dosyada belirtilir. Örneğin, aşağıdaki kod snippet ServiceEventSource adı *MyCompany-Application1-Stateless1:*

```csharp
[EventSource(Name = "MyCompany-Application1-Stateless1")]
internal sealed class ServiceEventSource : EventSource
{
    // (rest of ServiceEventSource implementation)
}
```

Dosyanın `eventFlowConfig.json` hizmet yapılandırma paketinin bir parçası olduğunu unutmayın. Bu dosyadaki değişiklikler, hizmetin yalnızca tam veya yapılandırma yükseltmelerine, Hizmet Kumaşyükseltme sistem durumu denetimlerine ve yükseltme hatası olduğunda otomatik geri alma denetimine tabi olarak eklenebilir. Daha fazla bilgi için [Service Fabric uygulama yükseltmesi](service-fabric-application-upgrade.md)ne bakınız.

Config'in *filtreler* bölümü, EventFlow ardışık bölümünden çıktılara geçecek bilgileri daha da özelleştirmenize olanak sağlayarak belirli bilgileri bırakmanızı veya eklemenizi veya olay verilerinin yapısını değiştirmenizi sağlar. Filtreleme hakkında daha fazla bilgi için [EventFlow filtrelerine](https://github.com/Azure/diagnostics-eventflow#filters)bakın.

Son adım, Servisinizin başlangıç kodunda dosyada `Program.cs` bulunan EventFlow ardışık hattını anında açmaktır:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric;
using Microsoft.ServiceFabric.Services.Runtime;

// **** EventFlow namespace
using Microsoft.Diagnostics.EventFlow.ServiceFabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>
        private static void Main()
        {
            try
            {
                // **** Instantiate log collection via EventFlow
                using (var diagnosticsPipeline = ServiceFabricDiagnosticPipelineFactory.CreatePipeline("MyApplication-MyService-DiagnosticsPipeline"))
                {

                    ServiceRuntime.RegisterServiceAsync("Stateless1Type",
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                    ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                    Thread.Sleep(Timeout.Infinite);
                }
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
                throw;
            }
        }
    }
}
```

`CreatePipeline` Yöntemin parametresi olarak geçirilen `ServiceFabricDiagnosticsPipelineFactory` ad, EventFlow günlük toplama ardışık ardışık ardışıklığı temsil eden sistem durumu *tüzel kişiliğinin* adıdır. EventFlow karşılaşırsa ve hata yaparsa ve Service Fabric sistem aracılığıyla raporlanırsa bu ad kullanılır.

### <a name="use-service-fabric-settings-and-application-parameters-in-eventflowconfig"></a>EventFlowConfig'de Servis Kumaşı ayarlarını ve uygulama parametrelerini kullanın

EventFlow, EventFlow ayarlarını yapılandırmak için Hizmet Dokusu ayarlarını ve uygulama parametrelerini kullanmayı destekler. Değerler için bu özel sözdizimini kullanarak Service Fabric ayarları parametrelerine başvurabilirsiniz:

```json
servicefabric:/<section-name>/<setting-name>
```

`<section-name>`Service Fabric yapılandırma bölümünün adıdır `<setting-name>` ve Bir EventFlow ayarını yapılandırmak için kullanılacak değeri sağlayan yapılandırma ayarıdır. Bunun nasıl yapılacağını daha fazla okumak [için Hizmet Kumaşı ayarları ve uygulama parametreleri için Destek'e](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters)gidin.

## <a name="verification"></a>Doğrulama

Hizmetinizi başlatın ve Visual Studio'da Hata Ayıklama çıkış penceresini gözlemleyin. Hizmet başlatıldıktan sonra, hizmetinizin yapılandırılan çıktıya kayıt gönderdiğine dair kanıt görmeye başlamalısınız. Olay çözümleme ve görselleştirme platformuna gidin ve günlüklerin görünmeye başladığını onaylayın (birkaç dakika sürebilir).

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulama Öngörüleri ile Etkinlik Analizi ve Görselleştirme](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Azure Monitör günlükleri ile Olay Analizi ve Görselleştirme](service-fabric-diagnostics-event-analysis-oms.md)
* [EventFlow belgeleri](https://github.com/Azure/diagnostics-eventflow)
