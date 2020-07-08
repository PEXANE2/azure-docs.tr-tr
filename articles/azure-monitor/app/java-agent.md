---
title: Java Web Apps performans izleme-Azure Application Insights
description: Application Insights ile Java Web sitenizin genişletilmiş performansı ve kullanımı izleniyor.
ms.topic: conceptual
ms.date: 01/10/2019
ms.openlocfilehash: b047a8dd8c67679a5cc8a45e8be82f9ab5227aa4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81537551"
---
# <a name="monitor-dependencies-caught-exceptions-and-method-execution-times-in-java-web-apps"></a>Java Web uygulamalarında bağımlılıkları izleme, özel durumlar ve Yöntem yürütme süreleri


[Java Web uygulamanızı Application Insights][java]olarak belirttiyseniz, herhangi bir kod değişikliği yapmadan daha derin Öngörüler almak Için Java aracısını kullanabilirsiniz:

* **Bağımlılıklar:** Uygulamanızın diğer bileşenlere yaptığı çağrılar hakkındaki veriler (şunlar dahil):
  * Apache HttpClient, OkHttp ve ile yapılan **gıden http çağrıları** `java.net.HttpURLConnection` yakalanır.
  * Jedsıs istemcisi aracılığıyla yapılan **redsıs çağrıları** yakalanır.
  * **JDBC sorguları** -MySQL ve PostgreSQL için çağrı 10 saniyeden uzun sürerse, aracı sorgu planını raporlar.

* **Uygulama günlüğü:** HTTP istekleri ve diğer telemetri ile uygulama günlüklerinizi yakalayın ve ilişkilendirin
  * **Log4J 1,2**
  * **Log4j2**
  * **Logback**

* **Daha iyi işlem adlandırması:** (portalda isteklerin toplaması için kullanılır)
  * **Yay** tabanlı `@RequestMapping` .
  * **Jax-RS** tabanlı `@Path` . 

Java aracısını kullanmak için sunucunuza yüklersiniz. Web uygulamalarınızın [Application Insights Java SDK 'sı][java]ile işaretlenmiş olması gerekir. 

## <a name="install-the-application-insights-agent-for-java"></a>Java için Application Insights aracısını yükler
1. Java sunucunuzu çalıştıran makinede [aracıyı indirin](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest). İndirdiğiniz Java Aracısı sürümünün Application Insights Java SDK çekirdeği ve web paketlerinin sürümüyle aynı olmasına dikkat edin.
2. Uygulama sunucusu başlangıç betiğini düzenleyin ve aşağıdaki JVM bağımsız değişkenini ekleyin:
   
    `-javaagent:<full path to the agent JAR file>`
   
    Örneğin, bir Linux makinesindeki Tomcat:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Uygulama sunucunuzu yeniden başlatın.

## <a name="configure-the-agent"></a>Configure the agent
Adlı bir dosya oluşturun `AI-Agent.xml` ve aracı jar dosyasıyla aynı klasöre yerleştirin.

XML dosyasının içeriğini ayarlayın. İstediğiniz özellikleri eklemek veya atlamak için aşağıdaki örneği düzenleyin.

```XML
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsightsAgent>
   <Instrumentation>
      <BuiltIn enabled="true">

         <!-- capture logging via Log4j 1.2, Log4j2, and Logback, default is true -->
         <Logging enabled="true" />

         <!-- capture outgoing HTTP calls performed through Apache HttpClient, OkHttp,
              and java.net.HttpURLConnection, default is true -->
         <HTTP enabled="true" />

         <!-- capture JDBC queries, default is true -->
         <JDBC enabled="true" />

         <!-- capture Redis calls, default is true -->
         <Jedis enabled="true" />

         <!-- capture query plans for JDBC queries that exceed this value (MySQL, PostgreSQL),
              default is 10000 milliseconds -->
         <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>

      </BuiltIn>
   </Instrumentation>
</ApplicationInsightsAgent>
```

## <a name="additional-config-spring-boot"></a>Ek yapılandırma (yay önyüklemesi)

`java -javaagent:/path/to/agent.jar -jar path/to/TestApp.jar`

Azure Uygulama Hizmetleri için aşağıdakileri yapın:

* Ayarlar > Uygulama Ayarları'nı seçin.
* Uygulama Ayarları'nın altında yeni bir anahtar değer çifti ekleyin:

Anahtar: `JAVA_OPTS` değer:`-javaagent:D:/home/site/wwwroot/applicationinsights-agent-2.5.0.jar`

Java aracısının en son sürümü için [buradaki](https://github.com/Microsoft/ApplicationInsights-Java/releases
)yayınları kontrol edin. 

Aracı, projenizde D:/Home/site/Wwwroot/dizinde bitecek bir kaynak olarak paketlenmesi gerekir. Aracının doğru App Service dizininde olduğunu, **geliştirme araçları**  >  **Gelişmiş Araçlar**  >  **hata ayıklama konsolu** ' na giderek ve site dizininin içeriğini inceleyerek emin olabilirsiniz.    

* Ayarları Kaydedin ve uygulamanızı Yeniden başlatın. (Bu adımlar yalnızca Windows üzerinde çalışan uygulama hizmetleri için geçerlidir.)

> [!NOTE]
> AI-Agent.xml ve aracı jar dosyası aynı klasörde olmalıdır. Bunlar genellikle `/resources` Proje klasörüne yerleştirilir.  

#### <a name="enable-w3c-distributed-tracing"></a>W3C dağıtılmış izlemeyi etkinleştir

Aşağıdakileri AI-Agent.xml ekleyin:

```xml
<Instrumentation>
   <BuiltIn enabled="true">
      <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
   </BuiltIn>
</Instrumentation>
```

> [!NOTE]
> Geriye dönük uyumluluk modu varsayılan olarak etkindir ve enableW3CBackCompat parametresi isteğe bağlıdır ve yalnızca kapatmak istediğinizde kullanılmalıdır. 

İdeal olarak, tüm hizmetlerinizin W3C protokolünü destekleyen SDK 'ların daha yeni bir sürümüne güncelleştirildiği durum söz konusu olabilir. En kısa sürede W3C desteğiyle SDK 'ların daha yeni bir sürümüne taşınması önemle önerilir.

**Hem [gelen](correlation.md#enable-w3c-distributed-tracing-support-for-java-apps) hem de giden (aracı) yapılandırmalarının** tam olarak aynı olduğundan emin olun.

## <a name="view-the-data"></a>Verileri görüntüleme
Application Insights kaynağında, toplanan uzak bağımlılık ve Yöntem yürütme süreleri [performans kutucuğunun altında][metrics]görüntülenir.

Bağımlılık, özel durum ve Yöntem raporlarının tek tek örneklerini aramak için [arama][diagnostic]' yı açın.

[Bağımlılık sorunlarını tanılama-daha fazla bilgi edinin](../../azure-monitor/app/asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Sorularınız mı var? Sorunlarınız mı var?
* Veri yok mu? [Güvenlik Duvarı özel durumlarını ayarlama](../../azure-monitor/app/ip-addresses.md)
* [Java sorun giderme](java-troubleshoot.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#track-exception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/platform/metrics-charts.md
