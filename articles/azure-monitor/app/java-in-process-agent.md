---
title: Java uygulamalarını her ortamda izleyin - Azure Monitör Uygulama Öngörüleri
description: Uygulama enstrümanting olmadan herhangi bir ortamda çalışan Java uygulamaları için uygulama performansı izleme. Dağıtılmış izleme ve uygulama haritası.
ms.topic: conceptual
ms.date: 03/29/2020
ms.openlocfilehash: b9c1a52051e63beee9a784714a7bb1a6a79e8759
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687726"
---
# <a name="java-codeless-application-monitoring-azure-monitor-application-insights---public-preview"></a>Azure Monitor Uygulama Öngörüleri izleme Java kodsuz uygulama - genel önizleme

Java kodsuz uygulama izleme tüm basitlik hakkında - hiçbir kod değişikliği vardır, Java aracısı yapılandırma değişiklikleri sadece bir çift ile etkinleştirilebilir.

 Java aracısı her ortamda çalışır ve tüm Java uygulamalarınızı izlemenizi sağlar. Başka bir deyişle, Ister VM'lerde, ister şirket içinde, ister AKS'de, Ister Windows' da, Linux' ta Java uygulamalarınızı çalıştırın, adını siz adlayın, Java 3.0 aracısı uygulamanızı izleyecektir.

3.0 aracı istekleri, bağımlılıkları ve günlükleri tek başına otomatik olarak topladığı için, Uygulama Öngörüleri Java SDK'nın uygulamanıza eklenmesi artık gerekli değildir.

Yine de uygulamanızdan özel telemetri gönderebilirsiniz. 3.0 aracısı izleyecek ve tüm otomatik toplanan telemetri ile birlikte ilişkilendirmek.

## <a name="quickstart"></a>Hızlı Başlangıç

**1. Aracıyı indirin**

[Uygulamaları indir-agent-3.0.0-PREVIEW.3.jar](https://github.com/microsoft/ApplicationInsights-Java/releases/download/3.0.0-PREVIEW.3/applicationinsights-agent-3.0.0-PREVIEW.3.jar)

**2. JVM'yi ajana doğru titretin**

Uygulamanızın JVM args ekleyin `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.3.jar`

Tipik JVM args içerir `-Xmx512m` ve. `-XX:+UseG1GC` Eğer bunları nereye ekleyacağınızı biliyorsanız, o zaman bunu nereye ekleyebileceğinizi zaten biliyorsunuzdemektir.

Uygulamanızın JVM args yapılandırma ile ek yardım için, [3.0 Önizleme bakın: JVM args güncellemek için ipuçları.](https://docs.microsoft.com/azure/azure-monitor/app/java-standalone-arguments)

**3. Aracıyı Application Insights kaynağınıza yönlendirin**

Zaten bir Uygulama Öngörüleri kaynağınız yoksa, [kaynak oluşturma kılavuzundaki](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource)adımları izleyerek yeni bir kaynak oluşturabilirsiniz.

Bir ortam değişkeni ayarlayarak aracıyı Application Insights kaynağınıza yönlendirin:

```
APPLICATIONINSIGHTS_CONNECTION_STRING=InstrumentationKey=00000000-0000-0000-0000-000000000000
```

Veya adlı `ApplicationInsights.json`bir yapılandırma dosyası oluşturarak ve aşağıdaki `applicationinsights-agent-3.0.0-PREVIEW.3.jar`içerikle aynı dizine yerleştirerek:

```json
{
  "instrumentationSettings": {
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000"
  }
}
```

Bağlantı dizenizi Application Insights kaynağınızda bulabilirsiniz:

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Uygulama Öngörüleri Bağlantı Dizesi":::

**4. Bu kadar!**

Şimdi uygulamanızı başlatın ve izleme verilerinizi görmek için Azure portalındaki Application Insights kaynağınıza gidin.

> [!NOTE]
> İzleme verilerinizin portalda gösterilmesi birkaç dakika sürebilir.


## <a name="configuration-options"></a>Yapılandırma seçenekleri

Dosyada, `ApplicationInsights.json` ayrıca yapılandırabilirsiniz:

* Bulut rol adı
* Bulut rolü örneği
* Uygulama günlüğü yakalama
* JMX ölçümleri
* Micrometer
* Sinyal
* Örnekleme
* HTTP Proxy
* Kendi kendine teşhis

[3.0 Genel Önizleme: Yapılandırma Seçenekleri'nde](https://docs.microsoft.com/azure/azure-monitor/app/java-standalone-config)ayrıntılara bakın.

## <a name="autocollected-requests-dependencies-logs-and-metrics"></a>Otomatik toplanan istekler, bağımlılıklar, günlükler ve ölçümler

### <a name="requests"></a>İstekler

* JMS Tüketiciler
* Kafka Tüketiciler
* Netty/WebFlux
* Servlet
* Bahar Zamanlama

### <a name="dependencies-with-distributed-trace-propagation"></a>Dağıtılmış izleme yayılımı olan bağımlılıklar

* Apache HttpClient ve HttpAsyncClient
* gRPC
* java.net.HttpURLConnection
* Jms
* Kafka
* Netty istemci
* okhttp

### <a name="other-dependencies"></a>Diğer bağımlılıklar

* Cassandra
* JDBC
* MongoDB (async ve sync)
* Redis (Marul ve Jedi)

### <a name="logs"></a>Günlükler

* java.util.logging
* Log4j
* SLF4J/Giriş

### <a name="metrics"></a>Ölçümler

* Micrometer
* JMX Ölçümleri

## <a name="sending-custom-telemetry-from-your-application"></a>Uygulamanızdan özel telemetri gönderme

3.0+'daki amacımız, standart API'leri kullanarak özel telemetrinizi göndermenize olanak sağlamaktır.

Mikrometre, OpenTelemetry API ve popüler günlük çerçevelerini destekliyoruz. Uygulama Insights Java 3.0 otomatik olarak telemetri yakalayacak ve otomatik toplanan telemetri tüm ile birlikte ilişkilendirmek.

Bu nedenle, şu anda Application Insights 3.0 içeren bir SDK yayınlamayı planlamiyoruz.

Uygulama Insights Java 3.0 zaten Uygulama Insights Java SDK 2.x gönderilen telemetri için dinliyor. Bu işlevsellik, mevcut 2.x kullanıcıları için yükseltme hikayesinin önemli bir parçasıdır ve OpenTelemetri API GA olana kadar özel telemetri desteğimizde önemli bir boşluğu doldurur.

## <a name="sending-custom-telemetry-using-application-insights-java-sdk-2x"></a>Uygulama Öngörüleri Java SDK 2.x kullanarak özel telemetri gönderme

Uygulamanıza ekleyin `applicationinsights-core-2.6.0.jar` (tüm 2.x sürümleri Application Insights Java 3.0 tarafından desteklenir, ancak bir seçeneğiniz varsa en son kullanmaya değer):

```xml
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-core</artifactId>
    <version>2.6.0</version>
  </dependency>
```

Bir Telemetriİstesi Oluşturun:

  ```java
private static final TelemetryClient telemetryClient = new TelemetryClient();
```

ve özel telemetri göndermek için kullanabilirsiniz.

### <a name="events"></a>Olaylar

  ```java
telemetryClient.trackEvent("WinGame");
```
### <a name="metrics"></a>Ölçümler

[Mikrometre](https://micrometer.io)ile metrik telemetri gönderebilirsiniz:

```java
  Counter counter = Metrics.counter("test_counter");
  counter.increment();
```

Veya Application Insights Java SDK 2.x'i de kullanabilirsiniz:

```java
  telemetryClient.trackMetric("queueLength", 42.0);
```

### <a name="dependencies"></a>Bağımlılıklar

```java
  boolean success = false;
  long startTime = System.currentTimeMillis();
  try {
      success = dependency.call();
  } finally {
      long endTime = System.currentTimeMillis();
      RemoteDependencyTelemetry telemetry = new RemoteDependencyTelemetry();
      telemetry.setTimestamp(new Date(startTime));
      telemetry.setDuration(new Duration(endTime - startTime));
      telemetryClient.trackDependency(telemetry);
  }
```

### <a name="logs"></a>Günlükler
En sevdiğiniz günlük çerçevesi üzerinden özel günlük telemetri gönderebilirsiniz.

Veya Application Insights Java SDK 2.x'i de kullanabilirsiniz:

```java
  telemetryClient.trackTrace(message, SeverityLevel.Warning, properties);
```

### <a name="exceptions"></a>Özel Durumlar
En sevdiğiniz günlük çerçevesi üzerinden özel özel durum telemetrisi gönderebilirsiniz.

Veya Application Insights Java SDK 2.x'i de kullanabilirsiniz:

```java
  try {
      ...
  } catch (Exception e) {
      telemetryClient.trackException(e);
  }
```

## <a name="upgrading-from-application-insights-java-sdk-2x"></a>Uygulama Öngörüleri Java SDK 2.x yükseltme

Uygulamanızda Uygulama Öngörüleri Java SDK 2.x kullanıyorsanız, uygulamanızı kaldırmanız gerekmez. Java 3.0 aracısı bunu algılar ve Java SDK 2.x üzerinden gönderdiğiniz özel telemetrileri yakalar ve ilişkilendirirken, java SDK 2.x tarafından gerçekleştirilen otomatik toplamayı yineleyerek yakalamayı önler.

> [!NOTE]
> Not: Java SDK 2.x TelemetriInitializers ve Telemetriİşlemciler 3.0 aracısı kullanırken çalışmaz.
