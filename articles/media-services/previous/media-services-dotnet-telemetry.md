---
title: Azure Medya Hizmetleri telemetrisini .NET| ile yapılandırma Microsoft Dokümanlar
description: Bu makalede, .NET SDK kullanarak Azure Medya Hizmetleri telemetrisini nasıl kullanacağınızı gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: f8f55e37-0714-49ea-bf4a-e6c1319bec44
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 1ffaefc51121aeb7421d6e49a3c0e58c76d4391e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61464955"
---
# <a name="configuring-azure-media-services-telemetry-with-net"></a>Azure Medya Hizmetleri telemetrisini .NET ile yapılandırma 

Bu makalede, .NET SDK kullanarak Azure Medya Hizmetleri (AMS) telemetrisini yapılandırırken atabileceğiniz genel adımlar açıklanmaktadır. 

>[!NOTE]
>AMS telemetrisinin ne olduğu ve nasıl tüketili sürüleceğine ilişkin ayrıntılı açıklama için [genel bakış](media-services-telemetry-overview.md) makalesine bakın.

Telemetri verilerini aşağıdaki yollardan birinde tüketebilirsiniz:

- Verileri doğrudan Azure Tablo Depolama'dan okuyun (örneğin, Depolama SDK'sını kullanarak). Telemetri depolama tablolarının açıklaması için [bu](https://msdn.microsoft.com/library/mt742089.aspx) makaledeki **Tüketen telemetri bilgilerine** bakın.

Veya

- Depolama verilerini okumak için Medya Hizmetleri .NET SDK'daki desteği kullanın. Bu makalede, belirtilen AMS hesabı için telemetrinin nasıl etkinleştirilen ve Azure Medya Hizmetleri .NET SDK'yı kullanarak ölçümlerin nasıl sorgulanılacak sorgulanış edilebilmektedir.  

## <a name="configuring-telemetry-for-a-media-services-account"></a>Medya Hizmetleri hesabı için telemetri yapılandırma

Telemetriyi etkinleştirmek için aşağıdaki adımlar gereklidir:

- Medya Hizmetleri hesabına iliştirilen depolama hesabının kimlik bilgilerini alın. 
- **AzureTable'a** **ayarlanmış EndPointType** ve depolama tablosunu işaret eden endPointAddress ile bir Bildirim Bitiş Noktası oluşturun.

```csharp
        INotificationEndPoint notificationEndPoint = 
                      _context.NotificationEndPoints.Create("monitoring", 
                      NotificationEndPointType.AzureTable,
                      "https://" + _mediaServicesStorageAccountName + ".table.core.windows.net/");
```

- İzlemek istediğiniz hizmetler için bir izleme yapılandırma ayarı oluşturun. En fazla birden fazla izleme yapılandırma ayarına izin verilmez. 

```csharp
        IMonitoringConfiguration monitoringConfiguration = _context.MonitoringConfigurations.Create(notificationEndPoint.Id,
            new List<ComponentMonitoringSetting>()
            {
                new ComponentMonitoringSetting(MonitoringComponent.Channel, MonitoringLevel.Normal),
                new ComponentMonitoringSetting(MonitoringComponent.StreamingEndpoint, MonitoringLevel.Normal)
            });
```

## <a name="consuming-telemetry-information"></a>Telemetri bilgilerini tüketme

Telemetri bilgilerini tüketme hakkında bilgi için [bu](media-services-telemetry-overview.md) makaleye bakın.

## <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

1. Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

2. App.config dosyanızda tanımlanan **appAyarlar'a** aşağıdaki öğeyi ekleyin:

    ```xml
        <add key="StorageAccountName" value="storage_name" />
    ```
 
## <a name="example"></a>Örnek  
    
Aşağıdaki örnekte, belirtilen AMS hesabı için telemetrinin nasıl etkinleştirilen ve Azure Medya Hizmetleri .NET SDK'yı kullanarak ölçümlerin nasıl sorgulanılanış gösterilmektedir.  

```csharp
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Linq;
using Microsoft.WindowsAzure.MediaServices.Client;

namespace AMSMetrics
{
    class Program
    {
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
            ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
            ConfigurationManager.AppSettings["AMSClientSecret"];

        private static readonly string _mediaServicesStorageAccountName =
            ConfigurationManager.AppSettings["StorageAccountName"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static IStreamingEndpoint _streamingEndpoint = null;
        private static IChannel _channel = null;

        static void Main(string[] args)
        {
            AzureAdTokenCredentials tokenCredentials =
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            _streamingEndpoint = _context.StreamingEndpoints.FirstOrDefault();
            _channel = _context.Channels.FirstOrDefault();

            var monitoringConfigurations = _context.MonitoringConfigurations;
            IMonitoringConfiguration monitoringConfiguration = null;

            // No more than one monitoring configuration settings is allowed.
            if (monitoringConfigurations.ToArray().Length != 0)
            {
                monitoringConfiguration = _context.MonitoringConfigurations.FirstOrDefault();
            }
            else
            {
                INotificationEndPoint notificationEndPoint =
                          _context.NotificationEndPoints.Create("monitoring",
                          NotificationEndPointType.AzureTable, GetTableEndPoint());

                monitoringConfiguration = _context.MonitoringConfigurations.Create(notificationEndPoint.Id,
                    new List<ComponentMonitoringSetting>()
                    {
                    new ComponentMonitoringSetting(MonitoringComponent.Channel, MonitoringLevel.Normal),
                    new ComponentMonitoringSetting(MonitoringComponent.StreamingEndpoint, MonitoringLevel.Normal)

                    });
            }

            //Print metrics for a Streaming Endpoint.
            PrintStreamingEndpointMetrics();

            Console.ReadLine();
        }

        private static string GetTableEndPoint()
        {
            return "https://" + _mediaServicesStorageAccountName + ".table.core.windows.net/";
        }

        private static void PrintStreamingEndpointMetrics()
        {
            Console.WriteLine(string.Format("Telemetry for streaming endpoint '{0}'", _streamingEndpoint.Name));

            DateTime timerangeEnd = DateTime.UtcNow;
            DateTime timerangeStart = DateTime.UtcNow.AddHours(-5);

            // Get some streaming endpoint metrics.
            var telemetry = _streamingEndpoint.GetTelemetry();

            var res = telemetry.GetStreamingEndpointRequestLogs(timerangeStart, timerangeEnd);

            Console.Title = "Streaming endpoint metrics:";

            foreach (var log in res)
            {
                Console.WriteLine("AccountId: {0}", log.AccountId);
                Console.WriteLine("BytesSent: {0}", log.BytesSent);
                Console.WriteLine("EndToEndLatency: {0}", log.EndToEndLatency);
                Console.WriteLine("HostName: {0}", log.HostName);
                Console.WriteLine("ObservedTime: {0}", log.ObservedTime);
                Console.WriteLine("PartitionKey: {0}", log.PartitionKey);
                Console.WriteLine("RequestCount: {0}", log.RequestCount);
                Console.WriteLine("ResultCode: {0}", log.ResultCode);
                Console.WriteLine("RowKey: {0}", log.RowKey);
                Console.WriteLine("ServerLatency: {0}", log.ServerLatency);
                Console.WriteLine("StatusCode: {0}", log.StatusCode);
                Console.WriteLine("StreamingEndpointId: {0}", log.StreamingEndpointId);
                Console.WriteLine();
            }

            Console.WriteLine();
        }

        private static void PrintChannelMetrics()
        {
            if (_channel == null)
            {
                Console.WriteLine("There are no channels in this AMS account");
                return;
            }

            Console.WriteLine(string.Format("Telemetry for channel '{0}'", _channel.Name));

            DateTime timerangeEnd = DateTime.UtcNow;
            DateTime timerangeStart = DateTime.UtcNow.AddHours(-5);

            // Get some channel metrics.
            var telemetry = _channel.GetTelemetry();

            var channelMetrics = telemetry.GetChannelHeartbeats(timerangeStart, timerangeEnd);

            // Print the channel metrics.
            Console.WriteLine("Channel metrics:");

            foreach (var channelHeartbeat in channelMetrics.OrderBy(x => x.ObservedTime))
            {
                Console.WriteLine(
                    "    Observed time: {0}, Last timestamp: {1}, Incoming bitrate: {2}",
                    channelHeartbeat.ObservedTime,
                    channelHeartbeat.LastTimestamp,
                    channelHeartbeat.IncomingBitrate);
            }

            Console.WriteLine();
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
