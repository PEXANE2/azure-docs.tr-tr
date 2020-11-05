---
title: Yapılandırma seçenekleri-Azure Izleyici Application Insights Java
description: Azure Izleyici Application Insights Java için yapılandırma seçenekleri
ms.topic: conceptual
ms.date: 04/16/2020
ms.custom: devx-track-java
ms.openlocfilehash: 710347061f072fe66987d88852045986c00812c8
ms.sourcegitcommit: 0d171fe7fc0893dcc5f6202e73038a91be58da03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93377692"
---
# <a name="configuration-options-for-azure-monitor-application-insights-java"></a>Azure Izleyici Application Insights Java için yapılandırma seçenekleri

> [!WARNING]
> **3,0 önizlemeden yükseltiyorsanız**
>
> JSON yapısı tamamen değiştiği için, tüm küçük harfleri içeren dosya adının yanı sıra, lütfen tüm yapılandırma seçeneklerini dikkatle gözden geçirin.

## <a name="connection-string-and-role-name"></a>Bağlantı dizesi ve rol adı

Bağlantı dizesi ve rol adı, başlamak için gereken en yaygın ayarlardır:

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "role": {
    "name": "my cloud role name"
  }
}
```

Bağlantı dizesi gereklidir ve farklı uygulamalardan aynı Application Insights kaynağına veri gönderişinizde rol adı önemlidir.

Daha fazla ayrıntı ve ek yapılandırma seçeneklerini aşağıda bulabilirsiniz.

## <a name="configuration-file-path"></a>Yapılandırma dosyası yolu

Varsayılan olarak Application Insights Java 3,0, yapılandırma dosyasının adlandırılması `applicationinsights.json` ve ile aynı dizinde bulunması beklenir `applicationinsights-agent-3.0.0.jar` .

Kendi yapılandırma dosya yolunuzu aşağıdakilerden birini kullanarak belirtebilirsiniz

* `APPLICATIONINSIGHTS_CONFIGURATION_FILE` ortam değişkeni veya
* `applicationinsights.configuration.file` Java sistem özelliği

Göreli bir yol belirtirseniz, bulunduğu dizine göre çözümlenir `applicationinsights-agent-3.0.0.jar` .

## <a name="connection-string"></a>Bağlantı dizesi

Bu gereklidir. Bağlantı dizenizi Application Insights kaynağınız için bulabilirsiniz:

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Application Insights bağlantı dizesi":::


```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000"
}
```

Ayrıca, ortam değişkenini kullanarak bağlantı dizesini de ayarlayabilirsiniz `APPLICATIONINSIGHTS_CONNECTION_STRING` .

Bağlantı dizesinin ayarlanması, Java aracısını devre dışı bırakacak.

## <a name="cloud-role-name"></a>Bulut rolü adı

Uygulama eşlemesindeki bileşeni etiketlemek için bulut rolü adı kullanılır.

Bulut rolü adını ayarlamak istiyorsanız:

```json
{
  "role": {   
    "name": "my cloud role name"
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
  "role": {
    "name": "my cloud role name",
    "instance": "my cloud role instance"
  }
}
```

Ayrıca, ortam değişkenini kullanarak bulut rolü örneğini ayarlayabilirsiniz `APPLICATIONINSIGHTS_ROLE_INSTANCE` .

## <a name="sampling"></a>Örnekleme

Maliyeti azaltmanız gerekiyorsa örnekleme yararlı olur.
Örnekleme işlem KIMLIĞI üzerinde (izleme KIMLIĞI olarak da bilinir) bir işlev olarak gerçekleştirilir, böylece aynı işlem KIMLIĞI her zaman aynı örnekleme kararına neden olur. Bu, içindeki dağıtılmış bir işlemin parçalarını, diğer bölümleri örneklenirken almanızı sağlar.

Örneğin, örneklemeyi %10 olarak ayarlarsanız, işlemlerinizin yalnızca %10 ' u görürsünüz, ancak bu %10 ' un her biri uçtan uca işlem ayrıntılarına sahip olur.

Örnekleme, **her türlü işlemi yaklaşık 1/3** yakalamak için nasıl ayarlayabileceğiniz bir örnektir. lütfen kullanım örneği için doğru olan örnekleme oranını ayarladığınızdan emin olun:

```json
{
  "sampling": {
    "percentage": 33.333
  }
}
```

Ayrıca, ortam değişkenini kullanarak örnekleme yüzdesini de ayarlayabilirsiniz `APPLICATIONINSIGHTS_SAMPLING_PERCENTAGE` .

> [!NOTE]
> Örnekleme yüzdesi için, N 'nin bir tamsayı olduğu, 100/N 'e yakın bir yüzde seçin. Şu anda örnekleme diğer değerleri desteklemiyor.

## <a name="jmx-metrics"></a>JMX ölçümleri

Bazı ek JMX ölçümleri toplamak istiyorsanız:

```json
{
  "jmxMetrics": [
    {
      "name": "JVM uptime (millis)",
      "objectName": "java.lang:type=Runtime",
      "attribute": "Uptime"
    },
    {
      "name": "MetaSpace Used",
      "objectName": "java.lang:type=MemoryPool,name=Metaspace",
      "attribute": "Usage.used"
    }
  ]
}
```

`name` Bu JMX ölçüsüne atanacak ölçüm adıdır (herhangi bir şey olabilir).

`objectName` , toplamak istediğiniz JMX MBean öğesinin [nesne adıdır](https://docs.oracle.com/javase/8/docs/api/javax/management/ObjectName.html) .

`attribute` , toplamak istediğiniz JMX MBean içindeki öznitelik adıdır.

Sayısal ve Boole JMX ölçüm değerleri desteklenir. Boolean JMX ölçümleri `0` , false için ve true için ile eşleştirilir `1` .

[//]: # "Not: burada APPLICATIONINSIGHTS_JMX_METRICS belgeleme yok"
[//]: # "env ' de gömülü JSON Messy 'dir ve yalnızca kodsuz kullanacaksınız iliştirme senaryosu için belgelenmelidir"

## <a name="custom-dimensions"></a>Özel boyutlar

Tüm telemetrinize özel boyutlar eklemek istiyorsanız:

```json
{
  "customDimensions": {
    "mytag": "my value",
    "anothertag": "${ANOTHER_VALUE}"
  }
}
```

`${...}` , başlangıçta belirtilen ortam değişkeninden değeri okumak için kullanılabilir.

## <a name="telemetry-processors-preview"></a>Telemetri işlemcileri (Önizleme)

Bu bir önizleme özelliğidir.

İstek, bağımlılık ve izleme telemetrisine uygulanacak kuralları yapılandırmanıza olanak tanır. Örneğin,
 * Hassas verileri maskeleme
 * Koşullu özel boyutlar ekleyin
 * Toplama ve görüntüleme için kullanılan telemetri adını güncelleştirme

Daha fazla bilgi için [telemetri işlemcisi](./java-standalone-telemetry-processors.md) belgelerini inceleyin.

## <a name="auto-collected-logging"></a>Otomatik toplanan günlüğe kaydetme

Log4J, Logback ve Java. util. Logging otomatik olarak işaretlenir ve bu günlüğe kaydetme çerçeveleri aracılığıyla gerçekleştirilen günlüğe kaydetme otomatik olarak toplanır.

Varsayılan olarak, günlüğe kaydetme yalnızca, veya üzeri bir günlüğe kaydetme gerçekleştirildiğinde toplanır `INFO` .

Bu koleksiyon düzeyini değiştirmek istiyorsanız:

```json
{
  "instrumentation": {
    "logging": {
      "level": "WARN"
    }
  }
}
```

Eşiği, ortam değişkenini kullanarak da ayarlayabilirsiniz `APPLICATIONINSIGHTS_INSTRUMENTATION_LOGGING_LEVEL` .

Bunlar `level` , dosyada belirtebileceğiniz geçerli değerlerdir `applicationinsights.json` ve farklı günlük çerçeveleri içindeki günlük düzeylerine nasıl karşılık gelmektedir:

| düzey             | Log4J  | Logback | TEM     |
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

## <a name="auto-collected-micrometer-metrics-including-spring-boot-actuator-metrics"></a>Otomatik toplanan mikro ölçüm ölçümleri (Spring Boot çalıştırıcı ölçümleri dahil)

Uygulamanız [mikro ölçüm](https://micrometer.io)kullanıyorsa, mikro ölçüm küresel kayıt defterine gönderilen ölçümler otomatik olarak toplanır.

Ayrıca, uygulamanız [Spring Boot çalıştırıcı](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html)kullanıyorsa, Spring Boot çalıştırıcı tarafından yapılandırılan ölçümler de otomatik olarak toplanır.

Mikro ölçüm ölçümlerinin otomatik toplanmasını devre dışı bırakmak için (Spring Boot çalıştırıcı ölçümleri dahil):

```json
{
  "instrumentation": {
    "micrometer": {
      "enabled": false
    }
  }
}
```

## <a name="heartbeat"></a>Sinyal

Application Insights Java 3,0, her 15 dakikada bir sinyal ölçümü gönderir. Uyarıları tetiklemek için sinyal ölçümünü kullanıyorsanız, bu sinyal sıklığını artırabilirsiniz:

```json
{
  "heartbeat": {
    "intervalSeconds": 60
  }
}
```

> [!NOTE]
> Sinyal verileri de Application Insights kullanımını izlemek için kullanıldığından, bu sinyalin sıklığını azaltamazsınız.

## <a name="http-proxy"></a>HTTP proxy 'Si

Uygulamanız bir güvenlik duvarının arkasındaysa ve Application Insights doğrudan bağlanamıyorsa (bkz. [Application Insights tarafından kullanılan IP adresleri](./ip-addresses.md)), Application Insights Java 3,0 ' i bir http proxy 'si kullanacak şekilde yapılandırabilirsiniz:

```json
{
  "proxy": {
    "host": "myproxy",
    "port": 8080
  }
}
```

[//]: # "0.9.0 'den büyük ölçüde önemli değişiklikler olan 0.10.0 'yi desteklemeene kadar Opentelemetri desteğinin duyurulmadığını Not edın"

[//]: # "# # Opentelemetri API 1,0 öncesi sürümler için destek"

[//]: # "Opentelemetri API 'si henüz kararlı olmadığından, Opentelemetri API 'sinin 1,0 öncesi sürümleri için destek kabul edilir"
[//]: # "Bu nedenle, aracının her sürümü Opentelemetri API 'sinin yalnızca belirli bir 1,0 öncesi sürümlerini destekler"
[//]: # "(Bu sınırlama, Opentelemetri API 1,0 yayımlandıktan sonra uygulanmaz)."

[//]: # "' ' ' JSON"
[//]: # "{"
[//]: # "  \"Önizleme \" : {"
[//]: # "    \"openTelemetryApiSupport \" : true"
[//]: # "  }"
[//]: # "}"
[//]: # "```"

## <a name="self-diagnostics"></a>Kendi kendine tanılama

"Kendi kendine tanılama" Application Insights Java 3,0 ' dan iç günlüğe kaydetme anlamına gelir.

Bu, Application Insights ile ilgili sorunları saptamak ve tanılamak için yararlı olabilir.

Varsayılan olarak, Java 3,0 günlüklerini `INFO` `applicationinsights.log` Bu yapılandırmaya karşılık gelen hem dosya hem de konsola Application Insights.

```json
{
  "selfDiagnostics": {
    "destination": "file+console",
    "level": "INFO",
    "file": {
      "path": "applicationinsights.log",
      "maxSizeMb": 5,
      "maxHistory": 1
    }
  }
}
```

`destination``file`, veya ' den biri `console` olabilir `file+console` .

`level` ,,,,, veya bunlardan biri olabilir `OFF` `ERROR` `WARN` `INFO` `DEBUG` `TRACE` .

`path` mutlak veya göreli bir yol olabilir. Göreli yollar, bulunduğu dizine göre çözümlenir `applicationinsights-agent-3.0.0.jar` .

`maxSizeMb` , oturum açmadan önce günlük dosyasının en büyük boyutudur.

`maxHistory` , tutulan günlük dosyalarının (geçerli günlük dosyasına ek olarak) alınan sayısıdır.
