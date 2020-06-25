---
title: Tüm ortamlarda Java uygulamalarını izleme-Azure Izleyici Application Insights
description: Uygulamayı işaretlemeden herhangi bir ortamda çalışan Java uygulamaları için uygulama performansı izleme. Dağıtılmış izleme ve uygulama eşlemesi.
ms.topic: conceptual
ms.date: 03/29/2020
ms.openlocfilehash: 3e3d108603ad6210143deea58049ff7b230bb6fa
ms.sourcegitcommit: 01cd19edb099d654198a6930cebd61cae9cb685b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85319712"
---
# <a name="java-codeless-application-monitoring-azure-monitor-application-insights---public-preview"></a>Java kodsuz kullanacaksınız uygulama izleme Azure izleyici Application Insights-genel önizleme

Java kodsuz kullanacaksınız uygulama izleme kolaylık sağlaması, hiçbir kod değişikliği yoktur, Java Aracısı yalnızca birkaç yapılandırma değişikliği aracılığıyla etkinleştirilebilir.

 Java Aracısı herhangi bir ortamda çalışarak tüm Java uygulamalarınızı izlemenizi sağlar. Diğer bir deyişle, şirket içinde Java uygulamalarınızı, şirket içinde, Windows, Linux 'ta çalıştırıp, Java 3,0 aracısının uygulamanızı izleyecektir.

Application Insights Java SDK 'sını uygulamanıza eklemek artık gerekli değildir, çünkü 3,0 Aracısı istekleri, bağımlılıkları ve günlükleri kendi kendine toplar.

Uygulamanız için hala özel telemetri gönderebilirsiniz. 3,0 Aracısı, tüm oto toplanan Telemetriyi izleyip onunla ilişkilendirilecektir.

## <a name="quickstart"></a>Hızlı Başlangıç

**1. aracıyı indirin**

[Applicationinsights-Agent-3.0.0-PREVIEW. 5. jar](https://github.com/microsoft/ApplicationInsights-Java/releases/download/3.0.0-PREVIEW.5/applicationinsights-agent-3.0.0-PREVIEW.5.jar) dosyasını indirin

**2. JVM 'yi aracıya işaret edin**

`-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.5.jar`Uygulamanızın JVM bağımsız değişkenlerini ekleyin

Tipik JVM bağımsız değişkenleri `-Xmx512m` ve içerir `-XX:+UseG1GC` . Bu nedenle, bunların nereye ekleneceğini biliyorsanız, bunun nereye ekleneceğini zaten öğrenmiş olursunuz.

Uygulamanızın JVM bağımsız değişkenlerini yapılandırmayla ilgili ek yardım için lütfen bkz. [3,0 Preview: JVM bağımsız değişkenlerini güncelleştirme ipuçları](https://docs.microsoft.com/azure/azure-monitor/app/java-standalone-arguments).

**3. aracıyı Application Insights kaynağına getirin**

Zaten bir Application Insights kaynağınız yoksa, [kaynak oluşturma kılavuzundaki](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource)adımları izleyerek yeni bir tane oluşturabilirsiniz.

Bir ortam değişkenini ayarlayarak aracıyı Application Insights kaynağına getirin:

```
APPLICATIONINSIGHTS_CONNECTION_STRING=InstrumentationKey=00000000-0000-0000-0000-000000000000
```

Ya da adlı bir yapılandırma dosyası oluşturup, ile `ApplicationInsights.json` aynı dizine yerleştirilerek `applicationinsights-agent-3.0.0-PREVIEW.5.jar` aşağıdaki içerikle birlikte:

```json
{
  "instrumentationSettings": {
    "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000"
  }
}
```

Bağlantı dizenizi Application Insights kaynağınız için bulabilirsiniz:

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Application Insights bağlantı dizesi":::

**4. işte!**

Şimdi uygulamanızı başlatın ve izleme verilerinizi görmek için Azure portal Application Insights kaynağına gidin.

> [!NOTE]
> İzleme verilerinizin portalda gösterilmesi birkaç dakika sürebilir.


## <a name="configuration-options"></a>Yapılandırma seçenekleri

`ApplicationInsights.json`Dosyasında, aşağıdakileri de yapılandırabilirsiniz:

* Bulut rolü adı
* Bulut rolü örneği
* Uygulama günlüğü yakalama
* JMX ölçümleri
* Micrometer
* Sinyal
* Örnekleme
* HTTP proxy 'Si
* Kendi kendine tanılama

Ayrıntılar için bkz. [3,0 genel önizleme: yapılandırma seçenekleri](https://docs.microsoft.com/azure/azure-monitor/app/java-standalone-config).

## <a name="autocollected-requests-dependencies-logs-and-metrics"></a>Oto toplanan istekler, bağımlılıklar, Günlükler ve ölçümler

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
* Log4J
* DOLAYıSıYLA SLF4J/Logback

### <a name="metrics"></a>Ölçümler

* Mikro ölçer (Spring Boot çalıştırıcı ölçümleri dahil)
* JMX ölçümleri

## <a name="sending-custom-telemetry-from-your-application"></a>Uygulamanızdan Özel telemetri gönderme

3.0 + ' da hedefiniz, Standart API 'Leri kullanarak özel telemetrinizi göndermenizi sağlamaktır.

Mikro ölçüm, Opentelemetri API ve popüler günlük çerçevesini destekliyoruz. Application Insights Java 3,0, Telemetriyi otomatik olarak yakalar ve otomatik toplanan telemetriyle birlikte ilişkilendirilecektir.

Bu nedenle, şu anda Application Insights 3,0 ile bir SDK 'Yı serbest bırakmaya planlanıyoruz.

Application Insights Java 3,0, Java SDK 'Sı 2. x Application Insights gönderilen telemetri için zaten dinliyor. Bu işlevsellik, var olan 2. x kullanıcıları için yükseltme hikayesinin önemli bir parçasıdır ve Opentelemetri API 'SI GA olana kadar özel telemetri desteğimize önemli bir boşluk girer.

## <a name="sending-custom-telemetry-using-application-insights-java-sdk-2x"></a>Java SDK 2. x Application Insights kullanarak özel telemetri gönderme

`applicationinsights-core-2.6.0.jar`Uygulamanıza ekleme (tüm 2. x sürümleri Application Insights Java 3,0 tarafından desteklenir, ancak bir seçiminiz varsa en son kullanımı buna değecektir):

```xml
  <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-core</artifactId>
    <version>2.6.0</version>
  </dependency>
```

TelemetryClient oluşturun:

  ```java
private static final TelemetryClient telemetryClient = new TelemetryClient();
```

ve bunu özel telemetri göndermek için kullanın.

### <a name="events"></a>Ekinlikler

  ```java
telemetryClient.trackEvent("WinGame");
```
### <a name="metrics"></a>Ölçümler

[Mikro ölçer](https://micrometer.io)aracılığıyla ölçüm telemetrisi gönderebilirsiniz:

```java
  Counter counter = Metrics.counter("test_counter");
  counter.increment();
```

Ya da Java SDK 'Sı 2. x Application Insights de kullanabilirsiniz:

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
En sevdiğiniz günlük çatısı aracılığıyla özel günlük telemetrisi gönderebilirsiniz.

Ya da Java SDK 'Sı 2. x Application Insights de kullanabilirsiniz:

```java
  telemetryClient.trackTrace(message, SeverityLevel.Warning, properties);
```

### <a name="exceptions"></a>Özel durumlar
En sevdiğiniz günlük çatısı aracılığıyla özel özel durum telemetrisi gönderebilirsiniz.

Ya da Java SDK 'Sı 2. x Application Insights de kullanabilirsiniz:

```java
  try {
      ...
  } catch (Exception e) {
      telemetryClient.trackException(e);
  }
```

## <a name="upgrading-from-application-insights-java-sdk-2x"></a>Application Insights Java SDK 2. x sürümünden yükseltme

Uygulamanızda zaten Application Insights Java SDK 'Sı 2. x kullanıyorsanız, kaldırmanız gerekmez. Java 3,0 Aracısı bu dosyayı algılar ve Java SDK 'Sı 2. x aracılığıyla gönderdiğiniz herhangi bir özel Telemetriyi yakalayıp, yinelenen yakalamayı engellemek için Java SDK 'Sı 2. x tarafından gerçekleştirilen herhangi bir yeniden toplamayı engeller.

> [!NOTE]
> Not: 3,0 Aracısı kullanılırken Java SDK 2. x TelemetryInitializers ve TelemetryProcessors çalıştırılmayacak.
