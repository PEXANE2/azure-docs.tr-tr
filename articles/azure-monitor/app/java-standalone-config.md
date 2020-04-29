---
title: Java uygulamalarını her yerde izleyin-Azure Izleyici Application Insights
description: Uygulamayı işaretlemeden herhangi bir ortamda çalışan Java uygulamaları için codeless uygulama performansı izleme. Dağıtılmış izleme ve uygulama haritasını kullanarak sorunlar için temel nedenini bulur.
ms.topic: conceptual
ms.date: 04/16/2020
ms.openlocfilehash: 5d930d349a2ab1efbd7a61904874bf6bdb411889
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81641894"
---
# <a name="configuration-options---java-standalone-agent-for-azure-monitor-application-insights"></a>Yapılandırma seçenekleri-Azure Izleyici için Java tek başına aracı Application Insights



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

Bağlantı dizesi gereklidir ve farklı uygulamalardan aynı Application Insights kaynağına veri gönderişinizde rol adı önemlidir.

Daha fazla ayrıntı için aşağıda daha fazla ayrıntı ve ek yapılandırma seçenekleri bulacaksınız.

## <a name="configuration-file-path"></a>Yapılandırma dosyası yolu

Varsayılan olarak, Application Insights Java 3,0 Önizlemesi yapılandırma dosyasının adlandırıldığını `ApplicationInsights.json`ve ile aynı dizinde bulunmasını bekler. `applicationinsights-agent-3.0.0-PREVIEW.jar`

Kendi yapılandırma dosya yolunuzu aşağıdakilerden birini kullanarak belirtebilirsiniz

* `APPLICATIONINSIGHTS_CONFIGURATION_FILE`ortam değişkeni veya
* `applicationinsights.configurationFile`Java sistem özelliği

Göreli bir yol belirtirseniz, bulunduğu dizine `applicationinsights-agent-3.0.0-PREVIEW.jar` göre çözümlenir.

## <a name="connection-string"></a>Bağlantı dizesi

Bu gereklidir. Bağlantı dizenizi Application Insights kaynağınız için bulabilirsiniz:

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Application Insights bağlantı dizesi":::

Ayrıca, ortam değişkenini `APPLICATIONINSIGHTS_CONNECTION_STRING`kullanarak bağlantı dizesini de ayarlayabilirsiniz.

## <a name="cloud-role-name"></a>Bulut rolü adı

Uygulama eşlemesindeki bileşeni etiketlemek için bulut rolü adı kullanılır.

Bulut rolü adını ayarlamak istiyorsanız:

```json
{
  "instrumentationSettings": {
    "preview": {   
      "roleName": "my cloud role name"
    }
  }
}
```

Bulut rolü adı ayarlanmamışsa, uygulama eşlemesindeki bileşeni etiketlemek için Application Insights kaynağın adı kullanılır.

Ayrıca, ortam değişkenini `APPLICATIONINSIGHTS_ROLE_NAME`kullanarak bulut rolü adını da ayarlayabilirsiniz.

## <a name="cloud-role-instance"></a>Bulut rolü örneği

Bulut rolü örneği, varsayılan olarak makine adına sahiptir.

Bulut rolü örneğini makine adı yerine farklı bir şekilde ayarlamak istiyorsanız:

```json
{
  "instrumentationSettings": {
    "preview": {
      "roleInstance": "my cloud role instance"
    }
  }
}
```

Ayrıca, ortam değişkenini `APPLICATIONINSIGHTS_ROLE_INSTANCE`kullanarak bulut rolü örneğini ayarlayabilirsiniz.

## <a name="application-log-capture"></a>Uygulama günlüğü yakalama

Application Insights Java 3,0 Preview, uygulama günlüğünü Log4J, Logback ve Java. util. Logging aracılığıyla otomatik olarak yakalar.

Varsayılan olarak, `WARN` düzeyinde veya üzerinde gerçekleştirilen tüm günlük kaydını yakalar.

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

Bunlar, `ApplicationInsights.json` dosyada belirtebileceğiniz `threshold` geçerli değerlerdir ve farklı günlük çerçeveleri genelinde günlük düzeylerine nasıl karşılık gelmektedir:

| `threshold`  | Log4J  | Logback | TEM     |
|--------------|--------|---------|---------|
| KAPALI          | KAPALI    | KAPALI     | KAPALI     |
| HATAYA        | HATAYA  | HATA   | OLAN  |
| HATA/ÖNEMLI | HATA  | HATA   | OLAN  |
| UYAR/UYARı | UYARıR   | UYARıR    | UYARI |
| BILGISINE         | BILGISINE   | BILGISINE    | BILGISINE    |
| KURULUMUNUN       | HATA AYIKLAMA  | HATA AYIKLAMA   | KURULUMUNUN  |
| HATA AYıKLAMA/INCE   | HATA AYIKLAMA  | HATA AYIKLAMA   | AYRıNTı    |
| ZARIF        | HATA AYIKLAMA  | HATA AYIKLAMA   | ZARIF   |
| IZLEME/FINEST | TRACE  | TRACE   | FıNEST  |
| TÜMÜ          | TÜMÜ    | TÜMÜ     | TÜMÜ     |

## <a name="jmx-metrics"></a>JMX ölçümleri

Yakalamaya ilgilendiğiniz bazı JMX ölçümleri varsa:

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

Varsayılan olarak, uygulamanız [mikro ölçüm](https://micrometer.io)kullanıyorsa, Application Insights 3,0 (Önizleme ile başlayarak), artık kendisini mikro ölçüm küresel kayıt defterine ekler ve mikro ölçüm ölçümlerini yakalar.

Bu özelliği devre dışı bırakmak istiyorsanız:

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

Varsayılan olarak, Application Insights Java 3,0 Preview, 15 dakikada bir sinyal ölçümü gönderir. Uyarıları tetiklemek için sinyal ölçümünü kullanıyorsanız, bu sinyal sıklığını artırabilirsiniz:

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
> Sinyal verileri de Application Insights kullanımını izlemek için kullanıldığından, bu sinyalin sıklığını azaltamazsınız.

## <a name="sampling"></a>Örnekleme

Maliyeti azaltmanız gerekiyorsa örnekleme yararlı olur.
Örnekleme işlem KIMLIĞI üzerinde (izleme KIMLIĞI olarak da bilinir) bir işlev olarak gerçekleştirilir, böylece aynı işlem KIMLIĞI her zaman aynı örnekleme kararına neden olur. Bu, içindeki dağıtılmış bir işlemin parçalarını, diğer bölümleri örneklenirken almanızı sağlar.

Örneğin, örneklemeyi %10 olarak ayarlarsanız, işlemlerinizin yalnızca %10 ' u görürsünüz, ancak bu %10 ' un her biri uçtan uca işlem ayrıntılarına sahip olur.

Örneklemeyi **tüm işlemlerin %10** ' a nasıl ayarlayabileceğiniz aşağıda verilmiştir. lütfen kullanım örneği için doğru olan örnekleme oranını ayarladığınızdan emin olun:

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

## <a name="http-proxy"></a>HTTP proxy 'Si

Uygulamanız bir güvenlik duvarının arkasındaysa ve Application Insights doğrudan bağlanamıyorsa (bkz. [Application Insights tarafından kullanılan IP adresleri](https://docs.microsoft.com/azure/azure-monitor/app/ip-addresses)), bir http proxy kullanmak için Java 3,0 Preview Application Insights yapılandırabilirsiniz:

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

## <a name="self-diagnostics"></a>Kendi kendine tanılama

"Kendi kendine tanılama" Application Insights Java 3,0 Preview 'dan iç günlüğe kaydetme anlamına gelir.

Bu, Application Insights ile ilgili sorunları saptamak ve tanılamak için yararlı olabilir.

Varsayılan olarak, bu yapılandırmaya karşılık gelen ve düzeyi `warn`olan konsola kaydedilir:

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

`OFF`Geçerli Düzeyler, `ERROR` `WARN` `INFO` `TRACE`,,, ve. `DEBUG`

Konsola kaydetmek yerine bir dosyaya oturum açmak istiyorsanız:

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

Dosya günlüğü kullanılırken, dosya isabetlerine `maxSizeMB`ulaştıktan sonra, geçerli günlük dosyasına ek olarak yalnızca en son tamamlanan günlük dosyasını tutarak geçiş yapılır.
