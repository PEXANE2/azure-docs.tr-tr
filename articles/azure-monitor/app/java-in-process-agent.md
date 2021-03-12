---
title: Azure Izleyici Application Insights Java
description: Kod değişikliğine gerek duymadan herhangi bir ortamda çalışan Java uygulamaları için uygulama performansı izleme. Dağıtılmış izleme ve uygulama eşlemesi.
ms.topic: conceptual
ms.date: 03/29/2020
author: MS-jgol
ms.custom: devx-track-java
ms.author: jgol
ms.openlocfilehash: b695df29b7a4704ee9e4e25e402fa0de8f2b7685
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103008221"
---
# <a name="java-codeless-application-monitoring-azure-monitor-application-insights"></a>Java kodsuz kullanacaksınız uygulama izleme Azure izleyici Application Insights

Java kodsuz kullanacaksınız uygulama izleme kolaylık sağlaması, hiçbir kod değişikliği yoktur, Java Aracısı yalnızca birkaç yapılandırma değişikliği aracılığıyla etkinleştirilebilir.

 Java Aracısı herhangi bir ortamda çalışarak tüm Java uygulamalarınızı izlemenizi sağlar. Diğer bir deyişle, şirket içinde Java uygulamalarınızı, şirket içinde, Windows, Linux 'ta çalıştırıp, Java 3,0 aracısının uygulamanızı izleyecektir.

Application Insights Java SDK 'sını uygulamanıza eklemek artık gerekli değildir, çünkü 3,0 Aracısı istekleri, bağımlılıkları ve günlükleri kendi kendine otomatik olarak toplar.

Uygulamanız için hala özel telemetri gönderebilirsiniz. 3,0 Aracısı, otomatik olarak toplanan Telemetriyi izleyip onunla ilişkilendirilecektir.

3,0 Aracısı Java 8 ve üstünü destekler.

## <a name="quickstart"></a>Hızlı Başlangıç

**1. aracıyı indirin**

> [!WARNING]
> **3,0 önizlemeden yükseltiyorsanız**
>
> JSON yapısı tamamen değiştiği için tüm [yapılandırma seçeneklerini](./java-standalone-config.md) dikkatle gözden geçirin. Bu, tüm küçük harfleri gösteren dosya adının tümüne ek olarak.

[Applicationinsights-Agent-3.0.2. jar](https://github.com/microsoft/ApplicationInsights-Java/releases/download/3.0.2/applicationinsights-agent-3.0.2.jar) dosyasını indirin

**2. JVM 'yi aracıya işaret edin**

`-javaagent:path/to/applicationinsights-agent-3.0.2.jar`Uygulamanızın JVM bağımsız değişkenlerini ekleyin

Tipik JVM bağımsız değişkenleri `-Xmx512m` ve içerir `-XX:+UseG1GC` . Bu nedenle, bunların nereye ekleneceğini biliyorsanız, bunun nereye ekleneceğini zaten öğrenmiş olursunuz.

Uygulamanızın JVM bağımsız değişkenlerini yapılandırma hakkında daha fazla yardım için lütfen [JVM bağımsız değişkenlerini güncelleştirme ipuçlarına](./java-standalone-arguments.md)bakın.

**3. aracıyı Application Insights kaynağına getirin**

Zaten bir Application Insights kaynağınız yoksa, [kaynak oluşturma kılavuzundaki](./create-new-resource.md)adımları izleyerek yeni bir tane oluşturabilirsiniz.

Bir ortam değişkenini ayarlayarak aracıyı Application Insights kaynağına getirin:

```
APPLICATIONINSIGHTS_CONNECTION_STRING=InstrumentationKey=...
```

Ya da adlı bir yapılandırma dosyası oluşturup, ile `applicationinsights.json` aynı dizine yerleştirilerek `applicationinsights-agent-3.0.2.jar` aşağıdaki içerikle birlikte:

```json
{
  "connectionString": "InstrumentationKey=..."
}
```

Bağlantı dizenizi Application Insights kaynağınız için bulabilirsiniz:

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Application Insights bağlantı dizesi":::

**4. işte!**

Şimdi uygulamanızı başlatın ve izleme verilerinizi görmek için Azure portal Application Insights kaynağına gidin.

> [!NOTE]
> İzleme verilerinizin portalda gösterilmesi birkaç dakika sürebilir.


## <a name="configuration-options"></a>Yapılandırma seçenekleri

`applicationinsights.json`Dosyasında, aşağıdakileri de yapılandırabilirsiniz:

* Bulut rolü adı
* Bulut rolü örneği
* Örnekleme
* JMX ölçümleri
* Özel boyutlar
* Telemetri işlemcileri (Önizleme)
* Otomatik toplanan günlüğe kaydetme
* Otomatik toplanan mikro ölçüm ölçümleri (Spring Boot çalıştırıcı ölçümleri dahil)
* Sinyal
* HTTP proxy 'Si
* Kendi kendine tanılama

Tüm ayrıntılar için bkz. [yapılandırma seçenekleri](./java-standalone-config.md) .

## <a name="auto-collected-requests-dependencies-logs-and-metrics"></a>Otomatik toplanan istekler, bağımlılıklar, Günlükler ve ölçümler

### <a name="requests"></a>İstekler

* JMS tüketicileri
* Kafka tüketicileri
* Netty/Webflox
* Servi sağlar
* Yay zamanlaması

### <a name="dependencies-with-distributed-trace-propagation"></a>Dağıtılmış izleme yaymaya sahip bağımlılıklar

* Apache HttpClient ve HttpAsyncClient
* gRPC
* Java. net. HttpURLConnection
* JMS
* Kafka
* Netty istemcisi
* OkHttp

### <a name="other-dependencies"></a>Diğer bağımlılıklar

* Cassandra
* JDBC
* MongoDB (Async ve Sync)
* Redsıs (Lettuce ve Jedsıs)

### <a name="logs"></a>Günlükler

* Java. util. Logging
* Log4J (MDC özellikleri dahil)
* DOLAYıSıYLA SLF4J/Logback (MDC özellikleri dahil)

### <a name="metrics"></a>Ölçümler

* Mikro ölçer (Spring Boot çalıştırıcı ölçümleri dahil)
* JMX ölçümleri

## <a name="send-custom-telemetry-from-your-application"></a>Uygulamanızdan Özel telemetri gönderin

3.0 + ' da hedefiniz, Standart API 'Leri kullanarak özel telemetrinizi göndermenizi sağlamaktır.

Mikro ölçer, popüler günlük çerçeveleri ve Application Insights Java 2. x SDK 'sını şu ana kadar destekliyoruz.
Application Insights Java 3,0, bu API 'Ler aracılığıyla gönderilen Telemetriyi otomatik olarak yakalar ve otomatik toplanan telemetriyle ilişkilendirir.

### <a name="supported-custom-telemetry"></a>Desteklenen özel telemetri

Aşağıdaki tablo, Java 3,0 Aracısı 'nı tamamlamak için etkinleştirebileceğiniz, şu anda desteklenen özel telemetri türlerini temsil eder. Özetlemek gerekirse, özel ölçümler mikro ölçüm aracılığıyla desteklenir, özel özel durumlar ve izlemeler günlüğe kaydetme çerçeveleri aracılığıyla etkinleştirilebilir ve tüm özel telemetri türleri [Java 2. x SDK Application Insights](#send-custom-telemetry-using-the-2x-sdk)aracılığıyla desteklenir.

|                     | Micrometer | Log4J, logback, Tem | 2. x SDK |
|---------------------|------------|---------------------|---------|
| **Özel olaylar**   |            |                     |  Yes    |
| **Özel ölçümler**  |  Yes       |                     |  Yes    |
| **Bağımlılıklar**    |            |                     |  Yes    |
| **Özel durumlar**      |            |  Yes                |  Yes    |
| **Sayfa Görüntülemeleri**      |            |                     |  Yes    |
| **İstekler**        |            |                     |  Yes    |
| **İzlemeler**          |            |  Yes                |  Yes    |

Application Insights 3,0 ile bir SDK 'Yı Şu anda serbest bırakmaya planlanıyoruz.

Application Insights Java 3,0, Java 2. x SDK Application Insights gönderilen Telemetriyi zaten dinliyor. Bu işlevsellik, var olan 2. x kullanıcıları için yükseltme hikayesinin önemli bir parçasıdır ve Opentelemetri API 'SI GA olana kadar özel telemetri desteğimize önemli bir boşluk girer.

### <a name="send-custom-metrics-using-micrometer"></a>Mikro ölçüm kullanarak özel ölçümleri gönderme

Uygulamanıza mikro ölçüm ekleyin:

```xml
<dependency>
  <groupId>io.micrometer</groupId>
  <artifactId>micrometer-core</artifactId>
  <version>1.6.1</version>
</dependency>
```

Bir ölçüm oluşturmak için mikro ölçüm [genel kayıt defteri](https://micrometer.io/docs/concepts#_global_registry) 'ni kullanın:

```java
static final Counter counter = Metrics.counter("test_counter");
```

ve bunları kullanarak ölçümleri kaydedebilirsiniz:

```java
counter.increment();
```

### <a name="send-custom-traces-and-exceptions-using-your-favorite-logging-framework"></a>En sevdiğiniz günlük çerçevesini kullanarak özel izlemeler ve özel durumlar gönderin

Log4J, Logback ve Java. util. Logging otomatik olarak işaretlenir ve bu günlüğe kaydetme çerçeveleri aracılığıyla gerçekleştirilen günlüğe kaydetme, izleme ve özel durum telemetrisi olarak otomatik toplanır.

Varsayılan olarak günlük kaydı yalnızca BILGI düzeyinde veya yukarıdaki günlüğe kaydetme gerçekleştirildiğinde toplanır.
Bu düzeyin nasıl değiştirileceği için [yapılandırma seçeneklerine](./java-standalone-config.md#auto-collected-logging) bakın.

Günlükleriniz için özel boyutlar eklemek istiyorsanız, [Log4j 1,2 MDC](https://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/MDC.html), [Log4J 2 MDC](https://logging.apache.org/log4j/2.x/manual/thread-context.html)veya [logback MDC](http://logback.qos.ch/manual/mdc.html)kullanabilirsiniz ve Application Insights Java 3,0, bu MDC özelliklerini, izleme ve özel durum telemetrisi için özel boyutlar olarak otomatik olarak yakalar.

### <a name="send-custom-telemetry-using-the-2x-sdk"></a>2. x SDK 'sını kullanarak özel telemetri gönderme

`applicationinsights-core-2.6.2.jar`Uygulamanıza ekleme (tüm 2. x sürümleri Application Insights Java 3,0 tarafından desteklenir, ancak bir seçiminiz varsa en son kullanımı buna değecektir):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-core</artifactId>
  <version>2.6.2</version>
</dependency>
```

TelemetryClient oluşturun:

  ```java
static final TelemetryClient telemetryClient = new TelemetryClient();
```

ve özel telemetri göndermek için kullanın:

##### <a name="events"></a>Ekinlikler

```java
telemetryClient.trackEvent("WinGame");
```

##### <a name="metrics"></a>Ölçümler

```java
telemetryClient.trackMetric("queueLength", 42.0);
```

##### <a name="dependencies"></a>Bağımlılıklar

```java
boolean success = false;
long startTime = System.currentTimeMillis();
try {
    success = dependency.call();
} finally {
    long endTime = System.currentTimeMillis();
    RemoteDependencyTelemetry telemetry = new RemoteDependencyTelemetry();
    telemetry.setSuccess(success);
    telemetry.setTimestamp(new Date(startTime));
    telemetry.setDuration(new Duration(endTime - startTime));
    telemetryClient.trackDependency(telemetry);
}
```

##### <a name="logs"></a>Günlükler

```java
telemetryClient.trackTrace(message, SeverityLevel.Warning, properties);
```

##### <a name="exceptions"></a>Özel durumlar

```java
try {
    ...
} catch (Exception e) {
    telemetryClient.trackException(e);
}
```

### <a name="add-request-custom-dimensions-using-the-2x-sdk"></a>2. x SDK 'sını kullanarak istek özel boyutları ekleme

> [!NOTE]
> Bu özellik yalnızca 3.0.2 ve üzeri sürümlerde

`applicationinsights-web-2.6.2.jar`Uygulamanıza ekleme (tüm 2. x sürümleri Application Insights Java 3,0 tarafından desteklenir, ancak bir seçiminiz varsa en son kullanımı buna değecektir):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.2</version>
</dependency>
```

ve kodunuzda özel boyutlar ekleyin:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
requestTelemetry.getProperties().put("mydimension", "myvalue");
```

### <a name="set-the-request-telemetry-user_id-using-the-2x-sdk"></a>2. x SDK 'sını kullanarak istek telemetrisi user_Id ayarlama

> [!NOTE]
> Bu özellik yalnızca 3.0.2 ve üzeri sürümlerde

`applicationinsights-web-2.6.2.jar`Uygulamanıza ekleme (tüm 2. x sürümleri Application Insights Java 3,0 tarafından desteklenir, ancak bir seçiminiz varsa en son kullanımı buna değecektir):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.2</version>
</dependency>
```

ve `user_Id` kodunuzda ' i ayarlayın:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
requestTelemetry.getContext().getUser().setId("myuser");
```

### <a name="override-the-request-telemetry-name-using-the-2x-sdk"></a>2. x SDK 'sını kullanarak istek telemetri adını geçersiz kılın

> [!NOTE]
> Bu özellik yalnızca 3.0.2 ve üzeri sürümlerde

`applicationinsights-web-2.6.2.jar`Uygulamanıza ekleme (tüm 2. x sürümleri Application Insights Java 3,0 tarafından desteklenir, ancak bir seçiminiz varsa en son kullanımı buna değecektir):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.2</version>
</dependency>
```

ve kodunuzda adı ayarlayın:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
requestTelemetry.setName("myname");
```

### <a name="get-the-request-telemetry-id-and-the-operation-id-using-the-2x-sdk"></a>2. x SDK 'sını kullanarak istek telemetri kimliği ve işlem kimliği alın

> [!NOTE]
> Bu özellik yalnızca 3.0.3-BETA ve üzeri sürümlerde

`applicationinsights-web-2.6.2.jar`Uygulamanıza ekleme (tüm 2. x sürümleri Application Insights Java 3,0 tarafından desteklenir, ancak bir seçiminiz varsa en son kullanımı buna değecektir):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.2</version>
</dependency>
```

ve kodunuzda istek telemetri kimliği ve işlem kimliği alın:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
String requestId = requestTelemetry.getId();
String operationId = requestTelemetry.getContext().getOperation().getId();
```
