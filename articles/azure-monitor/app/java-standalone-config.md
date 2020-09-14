---
title: Java uygulamalarını her yerde izleyin-Azure Izleyici Application Insights
description: Uygulamayı işaretlemeden herhangi bir ortamda çalışan Java uygulamaları için codeless uygulama performansı izleme. Dağıtılmış izleme ve uygulama haritasını kullanarak sorunlar için temel nedenini bulur.
ms.topic: conceptual
ms.date: 04/16/2020
ms.custom: devx-track-java
ms.openlocfilehash: 561a6405a49d8f15affbf6d8d4de1a7f4886826a
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90056107"
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

Varsayılan olarak, Application Insights Java 3,0 Önizlemesi yapılandırma dosyasının adlandırıldığını `ApplicationInsights.json` ve ile aynı dizinde bulunmasını bekler `applicationinsights-agent-3.0.0-PREVIEW.5.jar` .

Kendi yapılandırma dosya yolunuzu aşağıdakilerden birini kullanarak belirtebilirsiniz

* `APPLICATIONINSIGHTS_CONFIGURATION_FILE` ortam değişkeni veya
* `applicationinsights.configurationFile` Java sistem özelliği

Göreli bir yol belirtirseniz, bulunduğu dizine göre çözümlenir `applicationinsights-agent-3.0.0-PREVIEW.5.jar` .

## <a name="connection-string"></a>Bağlantı dizesi

Bu gereklidir. Bağlantı dizenizi Application Insights kaynağınız için bulabilirsiniz:

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Application Insights bağlantı dizesi":::

Ayrıca, ortam değişkenini kullanarak bağlantı dizesini de ayarlayabilirsiniz `APPLICATIONINSIGHTS_CONNECTION_STRING` .

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

Ayrıca, ortam değişkenini kullanarak bulut rolü adını da ayarlayabilirsiniz `APPLICATIONINSIGHTS_ROLE_NAME` .

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

Ayrıca, ortam değişkenini kullanarak bulut rolü örneğini ayarlayabilirsiniz `APPLICATIONINSIGHTS_ROLE_INSTANCE` .

## <a name="application-log-capture"></a>Uygulama günlüğü yakalama

Application Insights Java 3,0 Preview, uygulama günlüğünü Log4J, Logback ve Java. util. Logging aracılığıyla otomatik olarak yakalar.

Varsayılan olarak, düzeyinde veya üzerinde gerçekleştirilen tüm günlük kaydını yakalar `WARN` .

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

Bunlar `threshold` , dosyada belirtebileceğiniz geçerli değerlerdir `ApplicationInsights.json` ve farklı günlük çerçeveleri genelinde günlük düzeylerine nasıl karşılık gelmektedir:

| eşik değeri   | Log4J  | Logback | TEM     |
|-------------------|--------|---------|---------|
| KAPALI               | KAPALI    | KAPALI     | KAPALI     |
| HATAYA             | HATAYA  | HATA   | OLAN  |
| HATA (veya cıddı) | HATA  | HATA   | OLAN  |
| UYAR (veya uyarı) | UYARıR   | UYARıR    | UYARI |
| BILGISINE              | BILGISINE   | BILGISINE    | BILGISINE    |
| KURULUMUNUN            | HATA AYIKLAMA  | HATA AYIKLAMA   | KURULUMUNUN  |
| Hata ayıklama (veya AYRıNTıLı)   | HATA AYIKLAMA  | HATA AYIKLAMA   | AYRıNTı    |
| ZARIF             | HATA AYIKLAMA  | HATA AYIKLAMA   | ZARIF   |
| Izleme (veya FINEST) | TRACE  | TRACE   | FıNEST  |
| ALL               | ALL    | ALL     | ALL     |

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

## <a name="micrometer-including-metrics-from-spring-boot-actuator"></a>Mikro ölçer (Spring Boot çalıştırıcı 'daki ölçümler dahil)

Uygulamanız [mikro ölçer](https://micrometer.io)kullanıyorsa, Application Insights 3,0 (Önizleme ile başlayarak), artık mikro ölçüm küresel kayıt defterine gönderilen ölçümleri yakalar.

Uygulamanız [Spring Boot çalıştırıcı](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html)kullanıyorsa, Application Insights 3,0 (Preview ile başlayarak), şimdi Spring Boot çalıştırıcı (mikro ölçüm kullanır, ancak mikro ölçüm küresel kayıt defteri kullanmaz) tarafından yapılandırılan ölçümleri yakalar.

Bu özellikleri devre dışı bırakmak istiyorsanız:

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

Uygulamanız bir güvenlik duvarının arkasındaysa ve Application Insights doğrudan bağlanamıyorsa (bkz. [Application Insights tarafından kullanılan IP adresleri](./ip-addresses.md)), bir http proxy kullanmak için Java 3,0 Preview Application Insights yapılandırabilirsiniz:

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

Varsayılan olarak, `warn` Bu yapılandırmaya karşılık gelen ve düzeyi olan konsola kaydedilir:

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

Geçerli Düzeyler,,,, `OFF` `ERROR` `WARN` `INFO` `DEBUG` ve `TRACE` .

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

Dosya günlüğü kullanılırken, dosya isabetlerine ulaştıktan sonra, `maxSizeMB` geçerli günlük dosyasına ek olarak yalnızca en son tamamlanan günlük dosyasını tutarak geçiş yapılır.
