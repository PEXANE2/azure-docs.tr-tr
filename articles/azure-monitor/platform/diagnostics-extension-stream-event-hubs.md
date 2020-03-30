---
title: Windows Azure tanılama uzantısından Azure Etkinlik Hub'larına veri gönderme
description: Azure Monitor'daki tanılama uzantısını azure etkinlik hub'ına göndermek için yapılandırın ve böylece Azure dışındaki konumlara iletebilirsiniz.
ms.subservice: diagnostic-extension
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/18/2020
ms.openlocfilehash: 5e5034e99d37d3681192c2ad066f28acd1c4aeeb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672540"
---
# <a name="send-data-from-windows-azure-diagnostics-extension-to-azure-event-hubs"></a>Windows Azure tanılama uzantısından Azure Etkinlik Hub'larına veri gönderme
Azure tanılama uzantısı, Azure Monitor'da konuk işletim sisteminden izleme verileri ve Azure sanal makineleri ve diğer bilgi işlem kaynaklarının iş yüklerini toplayan bir aracıdır. Bu makalede, Azure dışındaki konumlara iletilebilmeniz için Windows Azure Tanılama uzantısından (WAD) [Azure Etkinlik Hub'larına](https://azure.microsoft.com/services/event-hubs/) nasıl veri gönderilebildiğiniz açıklanmaktadır.

## <a name="supported-data"></a>Desteklenen veriler

Etkinlik Hub'larına gönderilebilen konuk işletim sisteminden toplanan veriler aşağıdakileri içerir. IIS Günlükleri ve kilitlenme dökümleri de dahil olmak üzere WAD tarafından toplanan diğer veri kaynakları Olay Hub'larına gönderilemez.

* Windows için Olay İzleme (ETW) olayları
* Performans sayaçları
* Windows olay günlüğündeki uygulama günlükleri de dahil olmak üzere Windows olay günlükleri
* Azure Tanılama altyapısı günlükleri

## <a name="prerequisites"></a>Ön koşullar

* Windows tanılama uzantısı 1.6 veya daha yüksek. Sürüm geçmişi için [Azure Diagnostics uzantısı yapılandırma şema sürümlerine ve geçmişine](diagnostics-extension-versions.md) ve desteklenen kaynaklara [genel bakış alada Azure Tanılama uzantısı uzantısına](diagnostics-extension-overview.md) bakın.
* Olay Hub'ları ad alanı her zaman sağlanmalıdır. Ayrıntılar için [Etkinlik Hub'ları ile başlayın.](../../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)


## <a name="configuration-schema"></a>Yapılandırma şeması
Tanılama uzantısını ve [Azure Tanılama yapılandırma şemasını](diagnostics-extension-schema-windows.md) yapılandırma şemasına başvurmak üzere etkinleştirmek ve yapılandırmak için farklı seçenekler için [Windows Azure tanılama uzantısını (WAD) yükle ve yapılandırın.](diagnostics-extension-windows-install.md) Bu makalenin geri kalanı, bir olay hub'ına veri göndermek için bu yapılandırmanın nasıl kullanılacağını açıklar. 

Azure Tanılama, günlükleri ve ölçümleri her zaman bir Azure Depolama hesabına gönderir. Ek konumlara veri gönderen bir veya daha fazla *veri lavabosu* yapılandırabilirsiniz. Her lavabo, özel yapılandırmadaki hassas bilgilerle kamu yapılandırmasının [SinksConfig öğesinde](diagnostics-extension-schema-windows.md#sinksconfig-element) tanımlanır. Olay hub'ları için bu yapılandırma aşağıdaki tablodaki değerleri kullanır.

| Özellik | Açıklama |
|:---|:---|
| Adı | Lavabonun açıklayıcı adı. Lavaboya hangi veri kaynaklarının gönderilen leri belirtmek için yapılandırmada kullanılır. |
| Url  | Olay hub'ı biçimindeki \<olay merkezinin\>url'si.servicebus.windows.net/\<olay-hub-name.\>          |
| PaylaşılanErişimKeyName | En azından **Gönderme** yetkisine sahip olay merkezi için paylaşılan bir erişim ilkesinin adı. |
| SharedAccessKey     | Olay merkeziiçin paylaşılan erişim ilkesinden birincil veya ikincil anahtar. |

Örnek genel ve özel yapılandırmalar aşağıda gösterilmiştir. Bu, olay hub veri lavabosu nasıl yapılandırılabildiğini ve kullanılacağını göstermek için tek bir performans sayacı ve olay günlüğü içeren en az yapılandırmadır. Daha karmaşık bir örnek için [Azure Tanılama yapılandırma şemasına](diagnostics-extension-schema-windows.md) bakın.

### <a name="public-configuration"></a>Genel yapılandırma

```JSON
{
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 5120
        },
        "PerformanceCounters": {
            "scheduledTransferPeriod": "PT1M",
            "sinks": "myEventHub",
            "PerformanceCounterConfiguration": [
                {
                    "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                    "sampleRate": "PT3M"
                }
            ]
        },
        "WindowsEventLog": {
            "scheduledTransferPeriod": "PT1M",
            "sinks": "myEventHub",
                "DataSource": [
                {
                    "name": "Application!*[System[(Level=1 or Level=2 or Level=3)]]"
                }
            ]
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "myEventHub",
                    "EventHub": {
                        "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
                        "SharedAccessKeyName": "SendRule"
                    }
                }
            ]
        }
    },
    "StorageAccount": "mystorageaccount",
}
```


### <a name="private-configuration"></a>Özel yapılandırma

```JSON
{
    "storageAccountName": "mystorageaccount",
    "storageAccountKey": "{base64 encoded key}",
    "storageAccountEndPoint": "https://core.windows.net",
    "EventHub": {
        "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    }
}
```



## <a name="configuration-options"></a>Yapılandırma seçenekleri
Veri lavabosuna veri göndermek için, veri kaynağının düğümünde **lavabo** özniteliğini belirtirsiniz. **Lavabo özniteliğini** nereye yerleştirdiğiniz, atamanın kapsamını belirler. Aşağıdaki örnekte, **lavabo özniteliği,** tüm alt performans sayaçlarının olay merkezine gönderilmesine neden olacak **PerformanceCounters** düğümüne tanımlanır.

```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "sinks": "MyEventHub",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        }
    ]
}
```


Aşağıdaki örnekte, **lavabo özniteliği** doğrudan yalnızca bu performans sayaçlarının olay merkezine gönderilmesine neden olacak üç sayaçiçin uygulanır. 

```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M",
            "sinks": "MyEventHub"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Rejected",
            "sampleRate": "PT3M",
            "sinks": "MyEventHub"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Queued",
            "sampleRate": "PT3M",
            "sinks": "MyEventHub"
        }
    ]
}
```

## <a name="validating-configuration"></a>Yapılandırmayı doğrulama
Verilerin olay merkezine gönderildiğini doğrulamak için çeşitli yöntemler kullanabilirsiniz. ne basit yöntem, Azure Blob Depolama veya [Azure Veri Gölü Depolama'daki Azure Olay Hub'ları aracılığıyla Yakalama etkinliklerinde](../../event-hubs/event-hubs-capture-overview.md)açıklandığı gibi Olay Hub'ları yakalamayı kullanmaktır. 


## <a name="troubleshoot-event-hubs-sinks"></a>Sorun Giderme Olay Hub'ları lavabolar

- Azure Diagnostik'in kendisi için günlükler ve hatalar içeren AZURE Depolama tablosu **WADDiagnosticInfrastructureLogsTable'a** bakın. Seçeneklerden biri, bu depolama hesabına bağlanmak, bu tabloyu görüntülemek ve son 24 saat içinde TimeStamp için bir sorgu eklemek için [Azure Depolama Gezgini](https://www.storageexplorer.com) gibi bir araç kullanmaktır. Aracı .csv dosyasını dışa aktarmak ve Microsoft Excel gibi bir uygulamada açmak için kullanabilirsiniz. Excel, hangi hatanın bildirileni görmek için **EventHub'lar**gibi arama kartı dizelerini aramayı kolaylaştırır.  

- Olay merkezinizin başarıyla sağlanmış olduğundan denetleyin. Yapılandırmanın **PrivateConfig** bölümündeki tüm bağlantı bilgileri, kaynağınızın portalda görüldüğü gibi değerleriyle eşleşmelidir. Portalda tanımlanmış bir SAS ilkesi (örnekteki*SendRule)* olduğundan ve *Gönderme* izni verildiğinden emin olun.  

## <a name="next-steps"></a>Sonraki adımlar

* [Olay Hub'larına genel bakış](../../event-hubs/event-hubs-about.md)
* [Etkinlik merkezi oluşturma](../../event-hubs/event-hubs-create.md)
* [Event Hubs ile ilgili SSS](../../event-hubs/event-hubs-faq.md)

<!-- Images. -->
[0]: ../../event-hubs/media/event-hubs-streaming-azure-diags-data/dashboard.png



