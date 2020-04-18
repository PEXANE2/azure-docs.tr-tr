---
title: Java uygulamalarını her yerde izleyin - Azure Monitör Uygulama Öngörüleri
description: Uygulamayı enstrümanting olmadan herhangi bir ortamda çalışan Java uygulamaları için kodsuz uygulama performans izleme. Dağıtılmış izleme ve uygulama eşlemi kullanarak d sorunlarının temel nedenini bulun.
ms.topic: conceptual
ms.date: 04/16/2020
ms.openlocfilehash: 5d930d349a2ab1efbd7a61904874bf6bdb411889
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641894"
---
# <a name="configuration-options---java-standalone-agent-for-azure-monitor-application-insights"></a>Yapılandırma seçenekleri - Azure Monitörü Uygulama Öngörüleri için Java bağımsız aracısı



## <a name="connection-string-and-role-name"></a>Bağlantı dizesi ve rol adı

```json
{
  "instrumentationSettings": {
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
    "preview": {
      "roleName": "my cloud role name"
    }
  }
}
```

Bağlantı dizesi gereklidir ve rol adı, farklı uygulamalardan aynı Application Insights kaynağına veri gönderdiğinde önemlidir.

Daha fazla ayrıntı için daha fazla ayrıntı ve ek yapılandırma seçenekleri aşağıda bulabilirsiniz.

## <a name="configuration-file-path"></a>Yapılandırma dosya yolu

Varsayılan olarak, Application Insights Java 3.0 Preview `ApplicationInsights.json`yapılandırma dosyasının adlandırılmasını ve `applicationinsights-agent-3.0.0-PREVIEW.jar`'' ile aynı dizinde bulunmasını bekler.

Kendi yapılandırma dosya yolunuzu aşağıdaki

* `APPLICATIONINSIGHTS_CONFIGURATION_FILE`çevre değişkeni veya
* `applicationinsights.configurationFile`Java sistem özelliği

Göreceli bir yol belirtirseniz, bulunduğu dizine `applicationinsights-agent-3.0.0-PREVIEW.jar` göre çözülür.

## <a name="connection-string"></a>Bağlantı dizesi

Bu gereklidir. Bağlantı dizenizi Application Insights kaynağınızda bulabilirsiniz:

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Uygulama Öngörüleri Bağlantı Dizesi":::

Ayrıca, çevre değişkenini `APPLICATIONINSIGHTS_CONNECTION_STRING`kullanarak bağlantı dizesini ayarlayabilirsiniz.

## <a name="cloud-role-name"></a>Bulut rol adı

Bulut rol adı, bileşenin uygulama haritasında etiketlemek için kullanılır.

Bulut rol adını ayarlamak istiyorsanız:

```json
{
  "instrumentationSettings": {
    "preview": {   
      "roleName": "my cloud role name"
    }
  }
}
```

Bulut rol adı ayarlanmazsa, uygulama haritasında bileşeni etiketlemek için Application Insights kaynağının adı kullanılır.

Ayrıca, ortam değişkenini `APPLICATIONINSIGHTS_ROLE_NAME`kullanarak bulut rol adını da ayarlayabilirsiniz.

## <a name="cloud-role-instance"></a>Bulut rolü örneği

Bulut rolü örneği varsayılan olarak makine adına verilir.

Bulut rol örneğini makine adı yerine farklı bir şeye ayarlamak istiyorsanız:

```json
{
  "instrumentationSettings": {
    "preview": {
      "roleInstance": "my cloud role instance"
    }
  }
}
```

Ayrıca, ortam değişkenini `APPLICATIONINSIGHTS_ROLE_INSTANCE`kullanarak bulut rol örneğini de ayarlayabilirsiniz.

## <a name="application-log-capture"></a>Uygulama günlüğü yakalama

Application Insights Java 3.0 Preview, Log4j, Logback ve java.util.log üzerinden uygulama günlüğünü otomatik olarak yakalar.

Varsayılan `WARN` olarak, düzeyinde veya üzerinde gerçekleştirilen tüm günlük yakalar.

Bu eşiği değiştirmek istiyorsanız:

```json
{
  "instrumentationSettings": {
    "preview": {
      "instrumentation": {
        "logging": {
          "threshold": "ERROR"
        }
      }
    }
  }
}
```

Bunlar, dosyada belirtebileceğiniz geçerli `threshold` değerler ve bunların farklı günlük düzeylerine nasıl karşılık geldiğini: `ApplicationInsights.json`

| `threshold`  | Log4j  | Giriş | TEM     |
|--------------|--------|---------|---------|
| KAPALI          | KAPALI    | KAPALI     | KAPALI     |
| Ölümcül        | Ölümcül  | HATA   | Şiddetli  |
| HATA/ŞIDDETLI | HATA  | HATA   | Şiddetli  |
| UYARI/UYARMA | Uyarmak   | Uyarmak    | UYARI |
| Bilgi         | Bilgi   | Bilgi    | Bilgi    |
| Config       | HATA AYIKLAMA  | HATA AYIKLAMA   | Config  |
| HATA AyıkLAMA/İNCE   | HATA AYIKLAMA  | HATA AYIKLAMA   | Iyi    |
| Ince        | HATA AYIKLAMA  | HATA AYIKLAMA   | Ince   |
| İz / FINEST | TRACE  | TRACE   | En iyi  |
| TÜMÜ          | TÜMÜ    | TÜMÜ     | TÜMÜ     |

## <a name="jmx-metrics"></a>JMX ölçümleri

Yakalamak istediğiniz bazı JMX ölçümleri varsa:

```json
{
  "instrumentationSettings": {
    "preview": {
        "jmxMetrics": [
        {
          "objectName": "java.lang:type=ClassLoading",
          "attribute": "LoadedClassCount",
          "display": "Loaded Class Count"
        },
        {
          "objectName": "java.lang:type=MemoryPool,name=Code Cache",
          "attribute": "Usage.used",
          "display": "Code Cache Used"
        }
      ]
    }
  }
}
```

## <a name="micrometer"></a>Micrometer

Varsayılan olarak, uygulamanız [Mikrometre](https://micrometer.io)kullanıyorsa, Application Insights 3.0 (Preview.2 ile başlayarak) artık kendisini Mikrometre genel kayıt defterine ekler ve Mikrometre ölçümlerini yakalar.

Bu özelliği devre dışı kılmış olmak istiyorsanız:

```json
{
  "instrumentationSettings": {
    "preview": {
      "instrumentation": {
        "micrometer": {
          "enabled": false
        }
      }
    }
  }
}
```

## <a name="heartbeat"></a>Sinyal

Varsayılan olarak, Application Insights Java 3.0 Önizleme her 15 dakikada bir sinyal ölçümü gönderir. Uyarıları tetiklemek için sinyal ölçümü kullanıyorsanız, bu sinyalin sıklığını artırabilirsiniz:

```json
{
  "instrumentationSettings": {
    "preview": {
        "heartbeat": {
            "intervalSeconds": 60
        }
    }
  }
}
```

> [!NOTE]
> Bu sinyalin sıklığını azaltamazsınız, çünkü sinyal verileri Application Insights kullanımını izlemek için de kullanılır.

## <a name="sampling"></a>Örnekleme

Maliyeti azaltmanız gerekiyorsa örnekleme yararlıdır.
Örnekleme, aynı işlem kimliğiher zaman aynı örnekleme kararıyla sonuçlanacak şekilde, işlem kimliği (iz kimliği olarak da bilinir) üzerinde bir işlev olarak gerçekleştirilir. Bu, dağıtılmış bir işlemin parçalarını örneklenirken diğer bölümleri örneklenirken almamanızı sağlar.

Örneğin, örneklemeyi %10 olarak ayarlarsanız, hareketlerinizin yalnızca %10'unu görürsünüz, ancak bu %10'ların her biri tam uçtan uca işlem ayrıntılarına sahip olur.

Örneklemeyi **tüm işlemlerin %10'una** nasıl ayarladığınıza bir örnek verilmiştir - lütfen kullanım durumunuz için doğru olan örnekleme oranını belirlediğinizden emin olun:

```json
{
  "instrumentationSettings": {
    "preview": {
        "sampling": {
            "fixedRate": {
                "percentage": 10
            }
          }
        }
    }
}
```

## <a name="http-proxy"></a>HTTP Proxy

Uygulamanız bir güvenlik duvarının arkasındaysa ve doğrudan Application Insights'a bağlanamıyorsa [(Bkz. Application Insights tarafından kullanılan IP adresleri),](https://docs.microsoft.com/azure/azure-monitor/app/ip-addresses)Http proxy'sini kullanmak üzere Application Insights Java 3.0 Önizlemesini yapılandırabilirsiniz:

```json
{
  "instrumentationSettings": {
    "preview": {
      "httpProxy": {
        "host": "myproxy",
        "port": 8080
      }
    }
  }
}
```

## <a name="self-diagnostics"></a>Kendi kendine teşhis

"Kendi kendine tanılama", Application Insights Java 3.0 Preview'un dahili günlüğe kaydetmesini ifade eder.

Bu, Application Insights'ın kendisiyle ilgili sorunları tespit etmek ve tanılamanız için yararlı olabilir.

Varsayılan olarak, bu yapılandırmaya `warn`karşılık gelen düzeyi ile konsol günlükleri:

```json
{
  "instrumentationSettings": {
    "preview": {
        "selfDiagnostics": {
            "destination": "console",
            "level": "WARN"
        }
    }
  }
}
```

Geçerli seviyeler `OFF` `ERROR`, `WARN` `INFO`, `DEBUG`, `TRACE`, , ve .

Konsola günlüğe kaydetmek yerine bir dosyada oturum açmak istiyorsanız:

```json
{
  "instrumentationSettings": {
    "preview": {
        "selfDiagnostics": {
            "destination": "file",
            "directory": "/var/log/applicationinsights",
            "level": "WARN",
            "maxSizeMB": 10
        }    
    }
  }
}
```

Dosya günlüğe kaydetme kullanırken, dosya tıka ıldığında, `maxSizeMB`geçerli günlük dosyasına ek olarak yalnızca en son tamamlanan günlük dosyasını tutarak rollover olur.
