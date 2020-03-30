---
title: Java web uygulamaları performans izleme - Azure Application Insights
description: Uygulama Öngörüleri ile Java web sitenizin genişletilmiş performansı ve kullanım izleme.
ms.topic: conceptual
ms.date: 01/10/2019
ms.openlocfilehash: b29618179d22eac97a07bf41906465aba1fd7929
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77657036"
---
# <a name="monitor-dependencies-caught-exceptions-and-method-execution-times-in-java-web-apps"></a>Java web uygulamalarında bağımlılıkları, yakalanan özel durumları ve yöntem yürütme sürelerini izleyin


[Java web uygulamanızı Application Insights ile enstrümante][java]kullandıysanız, herhangi bir kod değişikliği olmadan daha derin bilgiler edinmek için Java Aracısını kullanabilirsiniz:

* **Bağımlılıklar:** Uygulamanızın diğer bileşenlere yaptığı aramalarla ilgili veriler:
  * **Giden HTTP aramaları** Apache httpClient, OkHttp `java.net.HttpURLConnection` üzerinden yapılan ve yakalanır.
  * Jedi'ların müşterisi aracılığıyla yapılan **Redis aramaları** yakalanır.
  * **JDBC sorguları** - MySQL ve PostgreSQL için, arama 10 saniyeden uzun sürerse, aracı sorgu planını bildirir.

* **Uygulama günlüğü:** Başvuru günlüklerinizi HTTP istekleri ve diğer telemetrilerle yakalayın ve ilişkilendirin
  * **Log4j 1.2**
  * **Log4j2**
  * **Giriş**

* **Daha iyi işlem adlandırma:** (portalda isteklerin toplanması için kullanılır)
  * **Bahar** - `@RequestMapping`dayalı .
  * **JAX-RS** - `@Path`dayalı . 

Java aracısını kullanmak için sunucunuza yüklersiniz. Web uygulamalarınız [Application Insights Java SDK][java]ile birlikte kullanılmalıdır. 

## <a name="install-the-application-insights-agent-for-java"></a>Java için Application Insights aracısını yükleyin
1. Java sunucunuzu çalıştıran makinede [aracıyı indirin](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest). İndirdiğiniz Java Aracısı sürümünün Application Insights Java SDK çekirdeği ve web paketlerinin sürümüyle aynı olmasına dikkat edin.
2. Uygulama sunucusu başlangıç komut dosyasını düzenleme ve aşağıdaki JVM bağımsız değişkenini ekleyin:
   
    `-javaagent:<full path to the agent JAR file>`
   
    Örneğin, Bir Linux makinesinde Tomcat'ta:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Uygulama sunucunuzu yeniden başlatın.

## <a name="configure-the-agent"></a>Configure the agent
Adlandırılmış `AI-Agent.xml` bir dosya oluşturun ve aracı JAR dosyasıyla aynı klasöre yerleştirin.

xml dosyasının içeriğini ayarlayın. İstediğiniz özellikleri eklemek veya atlamak için aşağıdaki örneği edin.

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

## <a name="additional-config-spring-boot"></a>Ek config (Bahar Önyükleme)

`java -javaagent:/path/to/agent.jar -jar path/to/TestApp.jar`

Azure Uygulama Hizmetleri için aşağıdakileri yapın:

* Ayarlar > Uygulama Ayarları'nı seçin.
* Uygulama Ayarları'nın altında yeni bir anahtar değer çifti ekleyin:

Anahtar: `JAVA_OPTS` Değer:`-javaagent:D:/home/site/wwwroot/applicationinsights-agent-2.5.0.jar`

Java aracısının en son sürümü için, [burada](https://github.com/Microsoft/ApplicationInsights-Java/releases
)bültenleri kontrol edin. 

Aracı, projenizde d:/home/site/wwwroot/ dizininde son bulmasını sağlayacak bir kaynak olarak paketlenmelidir. **Geliştirme Araçları** > **Gelişmiş Araçlar** > **Hata Ayıklama** Konsolu'na giderek ve site dizininin içeriğini inceleyerek aracınızın doğru Uygulama Hizmeti dizininde olduğunu doğrulayabilirsiniz.    

* Ayarları Kaydedin ve uygulamanızı Yeniden başlatın. (Bu adımlar yalnızca Windows'ta çalışan Uygulama Hizmetleri için geçerlidir.)

> [!NOTE]
> AI-Agent.xml ve aracı kavanoz dosyası aynı klasörde olmalıdır. Bunlar genellikle projenin klasöründe `/resources` bir araya yerleştirilir.  

#### <a name="enable-w3c-distributed-tracing"></a>W3C dağıtılmış izlemeyi etkinleştirme

AI-Agent.xml'e aşağıdakileri ekleyin:

```xml
<Instrumentation>
   <BuiltIn enabled="true">
      <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
   </BuiltIn>
</Instrumentation>
```

> [!NOTE]
> Geriye dönük uyumluluk modu varsayılan olarak etkinleştirilir ve etkinleştirilen W3CBackCompat parametresi isteğe bağlıdır ve yalnızca kapatmak istediğinizde kullanılmalıdır. 

İdeal olarak, tüm hizmetleriniz W3C protokolünü destekleyen SDK'ların daha yeni sürümüne güncelleştirildiğinde durum böyledir. W3C desteği ile SDK'ların en kısa sürede yeni sürümüne geçmen önerilir.

**Hem [gelen](correlation.md#enable-w3c-distributed-tracing-support-for-java-apps) hem de giden (aracı) yapılandırmaların** tamamen aynı olduğundan emin olun.

## <a name="view-the-data"></a>Verileri görüntüleme
Application Insights kaynağında, birleştirilmiş uzak bağımlılık ve yöntem yürütme süreleri [Performans döşemesinin altında][metrics]görünür.

Bağımlılık, özel durum ve yöntem raporlarının tek tek örneklerini aramak için [Arama'yı][diagnostic]açın.

[Bağımlılık sorunlarını tanılama - daha fazla bilgi edinin.](../../azure-monitor/app/asp-net-dependencies.md#diagnosis)

## <a name="questions-problems"></a>Sorularınız mı var? Sorunlarınız mı var?
* Veri yok mu? [Güvenlik duvarı özel durumlarını ayarlama](../../azure-monitor/app/ip-addresses.md)
* [Java sorun giderme](java-troubleshoot.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#track-exception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
