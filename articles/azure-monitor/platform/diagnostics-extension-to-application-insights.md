---
title: Azure Tanılama verilerini Uygulama Öngörülerine Gönderme
description: Uygulama Öngörüleri'ne veri göndermek için Azure Tanılama genel yapılandırmasını güncelleştirin.
ms.subservice: diagnostic-extension
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/19/2016
ms.openlocfilehash: 80d971abd248ca8253a374b488c693ea9aa2ea3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672336"
---
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-to-application-insights"></a>Bulut Hizmeti, Sanal Makine veya Hizmet Kumaşı tanı verilerini Uygulama Öngörülerine Gönderin
Bulut hizmetleri, Sanal Makineler, Sanal Makine Ölçeği Setleri ve Hizmet Kumaşı, veri toplamak için Azure Tanılama uzantısını kullanır.  Azure tanılama, Verileri Azure Depolama tablolarına gönderir.  Ancak, Azure Tanılama uzantısı 1,5 veya daha sonrasını kullanarak verilerin tamamını veya bir alt kümesini başka konumlara aktarabilirsiniz.

Bu makalede, Azure Tanılama uzantısından Uygulama Öngörüleri'ne nasıl veri gönderilen veriler açıklanmaktadır.

## <a name="diagnostics-configuration-explained"></a>Tanılama yapılandırması açıklandı
Azure tanılama uzantısı 1.5, tanılama verilerini gönderebileceğiniz ek konumlar olan lavabolar tanıtıldı.

Uygulama Öngörüleri için bir lavabonun örnek yapılandırması:

```XML
<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "ApplicationInsights",
            "ApplicationInsights": "{Insert InstrumentationKey}",
            "Channels": {
                "Channel": [
                    {
                        "logLevel": "Error",
                        "name": "MyTopDiagData"
                    },
                    {
                        "logLevel": "Error",
                        "name": "MyLogData"
                    }
                ]
            }
        }
    ]
}
```
- **Lavabo** *adı* özniteliği, lavaboyu benzersiz olarak tanımlayan bir dize değeridir.

- **ApplicationInsights** öğesi, Azure tanılama verilerinin gönderildiği Uygulama öngörüleri kaynağının enstrümantasyon anahtarını belirtir.
    - Varolan bir Application Insights kaynağınız yoksa, kaynak oluşturma ve enstrümantasyon anahtarını alma hakkında daha fazla bilgi için [yeni bir Uygulama Öngörüleri kaynağı oluşturma](../../azure-monitor/app/create-new-resource.md ) bilgisine bakın.
    - Azure SDK 2.8 ve sonraki bir Bulut Hizmeti geliştiriyorsanız, bu enstrümantasyon anahtarı otomatik olarak doldurulur. Değer, Bulut Hizmeti projesini paketlemede **APPINSIGHTS_INSTRUMENTATIONKEY** hizmet yapılandırma ayarını temel ayarıdır. Bkz. [Bulut Hizmetleri yle Uygulama Öngörülerini Kullan.](../../azure-monitor/app/cloudservices.md)

- **Kanallar** öğesi bir veya daha fazla **Kanal** öğesi içerir.
    - *Ad* özniteliği benzersiz olarak bu kanala başvurur.
    - *Günlük düzeyi* özniteliği, kanalın izin verdiği günlük düzeyini belirtmenize olanak tanır. En az bilgi sırasına göre kullanılabilir günlük düzeyleri şunlardır:
        - Ayrıntılı
        - Bilgi
        - Uyarı
        - Hata
        - Kritik

Kanal filtre gibi davranır ve hedef lavaboya göndermek için belirli günlük düzeylerini seçmenize olanak tanır. Örneğin, ayrıntılı günlükleri toplayıp depolama alanına gönderebilir, ancak yalnızca Hataları lavaboya gönderebilirsiniz.

Aşağıdaki grafik bu ilişkiyi gösterir.

![Tanılama Genel Yapılandırma](media/diagnostics-extension-to-application-insights/AzDiag_Channels_App_Insights.png)

Aşağıdaki grafik, yapılandırma değerlerini ve nasıl çalıştıklarını özetler. Hiyerarşide farklı düzeylerde yapılandırmaya birden çok lavabo ekleyebilirsiniz. En üst düzeydeki lavabo küresel bir ayar gibi davranır ve tek tek öğede belirtilen olan bu küresel ayarın geçersiz kılınması gibi davranır.

![Uygulama Öngörüleri ile Tanılama Yapılandırması](media/diagnostics-extension-to-application-insights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a>Komple lavabo yapılandırma örneği
Burada ortak yapılandırma dosyasının tam bir örneği
1. tüm hataları Application Insights'a gönderir **(DiagnosticMonitorConfiguration** düğümünde belirtilir)
2. ayrıca Uygulama Günlükleri **(Günlükler** düğümünde belirtilir) için Verbose düzey günlükleri gönderir.

```XML
<WadCfg>
  <DiagnosticMonitorConfiguration overallQuotaInMB="4096"
       sinks="ApplicationInsights.MyTopDiagData"> <!-- All info below sent to this channel -->
    <DiagnosticInfrastructureLogs />
    <PerformanceCounters>
      <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
      <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
    </PerformanceCounters>
    <WindowsEventLog scheduledTransferPeriod="PT1M">
      <DataSource name="Application!*" />
    </WindowsEventLog>
    <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose"
            sinks="ApplicationInsights.MyLogData"/> <!-- This specific info sent to this channel -->
  </DiagnosticMonitorConfiguration>

<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
  </SinksConfig>
</WadCfg>
```
```JSON
"WadCfg": {
    "DiagnosticMonitorConfiguration": {
        "overallQuotaInMB": 4096,
        "sinks": "ApplicationInsights.MyTopDiagData", "_comment": "All info below sent to this channel",
        "DiagnosticInfrastructureLogs": {
        },
        "PerformanceCounters": {
            "PerformanceCounterConfiguration": [
                {
                    "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                    "sampleRate": "PT3M"
                },
                {
                    "counterSpecifier": "\\Memory\\Available MBytes",
                    "sampleRate": "PT3M"
                }
            ]
        },
        "WindowsEventLog": {
            "scheduledTransferPeriod": "PT1M",
            "DataSource": [
                {
                    "name": "Application!*"
                }
            ]
        },
        "Logs": {
            "scheduledTransferPeriod": "PT1M",
            "scheduledTransferLogLevelFilter": "Verbose",
            "sinks": "ApplicationInsights.MyLogData", "_comment": "This specific info sent to this channel"
        }
    },
    "SinksConfig": {
        "Sink": [
            {
                "name": "ApplicationInsights",
                "ApplicationInsights": "{Insert InstrumentationKey}",
                "Channels": {
                    "Channel": [
                        {
                            "logLevel": "Error",
                            "name": "MyTopDiagData"
                        },
                        {
                            "logLevel": "Verbose",
                            "name": "MyLogData"
                        }
                    ]
                }
            }
        ]
    }
}
```
Önceki yapılandırmada aşağıdaki satırlar aşağıdaki anlamlara sahiptir:

### <a name="send-all-the-data-that-is-being-collected-by-azure-diagnostics"></a>Azure tanılama tarafından toplanan tüm verileri gönderme

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights",
}
```

### <a name="send-only-error-logs-to-the-application-insights-sink"></a>Yalnızca hata günlüklerini Application Insights lavaboya gönderme

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyTopDiagData",
}
```

### <a name="send-verbose-application-logs-to-application-insights"></a>Verbose uygulama günlüklerini Application Insights'a gönderme

```XML
<Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyLogData",
}
```

## <a name="limitations"></a>Sınırlamalar

- **Kanallar yalnızca performans sayaçlarını değil, yalnızca günlük türü.** Performans sayacı öğesi içeren bir kanal belirtirseniz, bu kanal yoksayılır.
- **Bir kanalın günlük düzeyi, Azure tanılama tarafından toplananlar için günlük düzeyini aşamaz.** Örneğin, Günlükler öğesindeki Uygulama Günlüğü hatalarını toplayamaz ve Verbose günlüklerini Uygulama Öngörüsü lavabosuna göndermeye çalışamazsınız. *ZamanlanmışTransferLogLevelFilter* özniteliği her zaman lavaboya göndermeye çalıştığınız günlüklerden eşit veya daha fazla günlük toplamalıdır.
- **Azure tanılama uzantısı tarafından toplanan blob verilerini Application Insights'a gönderemezsiniz.** Örneğin, *Dizinler* düğümü altında belirtilen herhangi bir şey. Kilitlenme Dökümleri için gerçek kilitlenme dökümü blob depolamaya gönderilir ve yalnızca kilitlenme dökümünün oluşturulduğuna dair bir bildirim Application Insights'a gönderilir.

## <a name="next-steps"></a>Sonraki Adımlar
* Azure [tanılama bilgilerinizi](https://docs.microsoft.com/azure/application-insights/app-insights-cloudservices) Uygulama Öngörüleri'nde nasıl görüntüleyin öğrenin.
* Uygulamanız için Azure tanılama uzantısını etkinleştirmek için [PowerShell'i](../../cloud-services/cloud-services-diagnostics-powershell.md) kullanın.
* Uygulamanız için Azure tanılama uzantısını etkinleştirmek için [Visual Studio'yu](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines) kullanın

