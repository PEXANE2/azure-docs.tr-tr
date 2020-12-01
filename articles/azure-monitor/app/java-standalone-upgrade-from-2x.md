---
title: 2. x-Azure Izleyici Application Insights Java 'dan yükseltme
description: Azure Izleyici 'den yükseltme Application Insights Java 2. x
ms.topic: conceptual
ms.date: 11/25/2020
ms.openlocfilehash: d1d09c09afbabd40a32cbb80f1901112c37ac3da
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96355134"
---
# <a name="upgrading-from-application-insights-java-sdk-2x"></a>Application Insights Java SDK 2. x sürümünden yükseltme

Uygulamanızda zaten Application Insights Java SDK 'Sı 2. x kullanıyorsanız, kaldırmanız gerekmez.
Java 3,0 Aracısı bu dosyayı algılar ve Java SDK 'Sı 2. x aracılığıyla gönderdiğiniz herhangi bir özel Telemetriyi yakalayıp, yinelenen telemetrinin önlenmesi için Java SDK 2. x tarafından gerçekleştirilen herhangi bir otomatik koleksiyonu engeller.

Application Insights 2. x Aracısı kullanıyorsanız, `-javaagent:` 2. x aracısına işaret eden JVM bağımsız değişken 'i kaldırmanız gerekir.

## <a name="telemetryinitializers-and-telemetryprocessors"></a>TelemetryInitializers ve TelemetryProcessors

Java SDK 2. x TelemetryInitializers ve TelemetryProcessors, 3,0 Aracısı kullanılırken çalıştırılmayacak.
Bu, daha önce gerekli olan kullanım örneklerinin birçoğu, [özel boyutları](./java-standalone-config.md#custom-dimensions) yapılandırarak veya [telemetri işlemcileri](./java-standalone-telemetry-processors.md)yapılandırarak 3,0 'de çözülebilir.

## <a name="multiple-applications-in-a-single-jvm"></a>Tek bir JVM 'de birden çok uygulama

Şu anda 3,0, çalışan işlem başına yalnızca tek bir [bağlantı dizesini ve rol adını](./java-standalone-config.md#connection-string-and-role-name) destekler. Özellikle, farklı bağlantı dizelerini veya farklı rol adlarını kullanan aynı Tomcat dağıtımında birden çok Tomcat Web uygulamanız olamaz.

## <a name="http-request-telemetry-names"></a>HTTP istek telemetri adları

3,0 sürümündeki HTTP istek telemetrisi adları genellikle Application Insights portalında U/X ' te daha iyi bir toplu görünüm sağlayacak şekilde değiştirilmiştir.

Ancak bazı uygulamalar için, önceki telemetri adları tarafından sağlanmış olan U/X içindeki toplanmış görünümü yine de tercih edebilirsiniz. Bu durumda, önceki adlara dönmek için 3,0 içindeki telemetri işlemcileri önizleme özelliğini kullanabilirsiniz.

### <a name="to-prefix-the-telemetry-name-with-the-http-method-get-post-etc"></a>Http yöntemiyle telemetri adına önek olarak ( `GET` , `POST` , vb.):

```
{
  "preview": {
    "processors": [
      {
        "type": "span",
        "include": {
          "matchType": "regexp",
          "attributes": [
            { "key": "http.method", "value": "" }
          ],
          "spanNames": [ "^/" ]
        },
        "name": {
          "toAttributes": {
            "rules": [ "^(?<tempName>.*)$" ]
          }
        }
      },
      {
        "type": "span",
        "include": {
          "matchType": "strict",
          "attributes": [
            { "key": "tempName" }
          ]
        },
        "name": {
          "fromAttributes": [ "http.method", "tempName" ],
          "separator": " "
        }
      },
      {
        "type": "attribute",
        "include": {
          "matchType": "strict",
          "attributes": [
            { "key": "tempName" }
          ]
        },
        "actions": [
          { "key": "tempName", "action": "delete" }
        ]
      }
    ]
  }
}
```

### <a name="to-set-the-telemetry-name-to-the-full-url-path"></a>Telemetri adını tam URL yoluna ayarlamak için

```
{
  "preview": {
    "processors": [
      {
        "type": "span",
        "include": {
          "matchType": "strict",
          "attributes": [
            { "key": "http.method" },
            { "key": "http.url" }
          ]
        },
        "name": {
          "fromAttributes": [ "http.url" ]
        }
      },
      {
        "type": "span",
        "include": {
          "matchType": "strict",
          "attributes": [
            { "key": "http.method" },
            { "key": "http.url" }
          ]
        },
        "name": {
          "toAttributes": {
            "rules": [ "https?://[^/]+(?<tempPath>/[^?]*)" ]
          }
        }
      },
      {
        "type": "span",
        "include": {
          "matchType": "strict",
          "attributes": [
            { "key": "tempPath" }
          ]
        },
        "name": {
          "fromAttributes": [ "tempPath" ]
        }
      },
      {
        "type": "attribute",
        "include": {
          "matchType": "strict",
          "attributes": [
            { "key": "tempPath" }
          ]
        },
        "actions": [
          { "key": "tempPath", "action": "delete" }
        ]
      }
    ]
  }
}
```
