---
title: Azure Application Insights Java SDK ile Mikrometre nasıl kullanılır?
description: Uygulama Öngörüleri Bahar Önyükleme ve Bahar Önyükleme olmayan uygulamalar ile Mikrometre kullanarak adım kılavuz adım.
ms.topic: conceptual
author: lgayhardt
ms.author: lagayhar
ms.date: 11/01/2018
ms.openlocfilehash: dd04087db32f0bbfa75dafa7e12c355e5ab7b515
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670075"
---
# <a name="how-to-use-micrometer-with-azure-application-insights-java-sdk"></a>Azure Application Insights Java SDK ile Mikrometre nasıl kullanılır?
Mikrometre uygulama izleme JVM tabanlı uygulama kodu ölçümleri ölçer ve verileri favori izleme sistemlerine aktarmanızı sağlar. Bu makalede, hem Bahar Önyükleme ve bahar önyükleme olmayan uygulamalar için Uygulama Öngörüleri ile Mikrometre nasıl kullanılacağını öğretecektir.

## <a name="using-spring-boot-15x"></a>Bahar Önyükleme 1.5x kullanma
Pom.xml veya build.gradle dosyanıza aşağıdaki bağımlılıkları ekleyin: 
* [Uygulama Öngörüleri bahar-önyükleme-starter](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/azure-application-insights-spring-boot-starter) 2.5.0 veya sonrası
* Mikrometre Azure Kayıt Defteri 1.1.0 veya üzeri
* [Mikrometre Spring Legacy](https://micrometer.io/docs/ref/spring/1.5) 1.1.0 veya üzeri (bu Bahar çerçevesinde autoconfig kodu backports).
* [ApplicationInsights Kaynak](../../azure-monitor/app/create-new-resource.md )

Adımlar

1. Bahar Önyükleme uygulamanızın pom.xml dosyasını güncelleştirin ve aşağıdaki bağımlılıkları ekleyin:

    ```XML
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-spring-boot-starter</artifactId>
        <version>2.5.0</version>
    </dependency>

    <dependency>
        <groupId>io.micrometer</groupId>
        <artifactId>micrometer-spring-legacy</artifactId>
        <version>1.1.0</version>
    </dependency>

    <dependency>
        <groupId>io.micrometer</groupId>
        <artifactId>micrometer-registry-azure-monitor</artifactId>
        <version>1.1.0</version>
    </dependency>

    ```
2. Application Insights Instrumentation tuşu ile application.properties veya yml dosyasını aşağıdaki özelliği kullanarak güncelleştirin:

     `azure.application-insights.instrumentation-key=<your-instrumentation-key-here>`
1. Uygulamanızı oluşturun ve çalıştırın
2. Yukarıdakiler, Azure Monitor'a otomatik olarak toplanan önceden toplanmış ölçümlerle çalışmaya devam etmenizi gerekir. Uygulama Insights Bahar Önyükleme başlatıcı ince ayar hakkında ayrıntılı bilgi için [GitHub üzerinde readme](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md)bakın.

## <a name="using-spring-2x"></a>Yay 2.x kullanma

Pom.xml veya build.gradle dosyanıza aşağıdaki bağımlılıkları ekleyin:

* Uygulama Öngörüleri Bahar-önyükleme-starter 2.1.2 veya üzeri
* Azure-bahar-önyükleme-ölçümleri-başlangıçlar 2.0.7 veya sonrası
* [Uygulama Öngörüleri Kaynağı](../../azure-monitor/app/create-new-resource.md )

Adımlar:

1. Bahar Önyükleme uygulamanızın pom.xml dosyasını güncelleştirin ve aşağıdaki bağımlılığı ekleyin:

    ```XML
    <dependency> 
          <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-spring-boot-metrics-starter</artifactId>
          <version>2.0.7</version>
    </dependency>
    ```
1. Application Insights Instrumentation tuşu ile application.properties veya yml dosyasını aşağıdaki özelliği kullanarak güncelleştirin:

     `azure.application-insights.instrumentation-key=<your-instrumentation-key-here>`
3. Uygulamanızı oluşturun ve çalıştırın
4. Yukarıdakiler, Azure Monitor'a otomatik olarak toplanan önceden toplanan ölçümlerle çalışmanızı gerektırmalıdır. Uygulama Insights Bahar Önyükleme başlatıcı ince ayar hakkında ayrıntılı bilgi için [GitHub üzerinde readme](https://github.com/Microsoft/azure-spring-boot/releases/latest)bakın.

Varsayılan Ölçümler:

*    Tomcat, JVM, Logback Ölçümleri, Log4J ölçümleri, Çalışma Zamanı Ölçümleri, İşlemci Ölçümleri, FileDescriptorMetrics için otomatik olarak yapılandırılan ölçümler.
*    Örneğin, Netflix Hystrix sınıf yolunda mevcutsa biz de bu ölçümleri almak. 
*    Aşağıdaki ölçümler ilgili çekirdekler ekleyerek kullanılabilir. 
        - Önbellek (KafeinÖnbellek, EhCache2, GuavaCache, HazelcastCache, JÖnbellek)     
        - DataBaseTableMetrics 
        - Hazırda BeklemeMeler 
        - İskeleMetrics 
        - OkHttp3 ölçümleri 
        - Kafka Ölçütleri 

 

Otomatik ölçümler koleksiyonu nasıl kapatIlir: 
 
- JVM Ölçümleri: 
    - management.metrics.binders.jvm.enabled=false 
- Giriş Ölçümleri: 
    - management.metrics.binders.logback.enabled=false
- Çalışma Zamanı Ölçümleri: 
    - management.metrics.binders.uptime.enabled=false 
- İşlemci Ölçümleri:
    -  management.metrics.binders.processor.enabled=false 
- DosyaTanımlayıcıÖlçümler:
    - management.metrics.binders.files.enabled=false 
- Hystrix Ölçümleri eğer sınıf yolunda kitaplık: 
    - management.metrics.binders.hystrix.enabled=false 
- AspectJ Ölçümleri eğer sınıf yolunda kitaplık: 
    - spring.aop.enabled=false 

> [!NOTE]
> İlkbahar Önyükleme uygulamanızın uygulama.özellikleri veya application.yml dosyasında yukarıdaki özellikleri belirtin

## <a name="use-micrometer-with-non-spring-boot-web-applications"></a>Bahar Önyükleme web uygulamaları yla Mikrometre kullanın

Pom.xml veya build.gradle dosyanıza aşağıdaki bağımlılıkları ekleyin:

* Uygulama Öngörüleri Web Otomatik 2.5.0 veya sonrası
* Mikrometre Azure Kayıt Defteri 1.1.0 veya üzeri
* [Uygulama Öngörüleri Kaynağı](../../azure-monitor/app/create-new-resource.md )

Adımlar:

1. pom.xml veya build.gradle dosyanıza aşağıdaki bağımlılıkları ekleyin:

    ```XML
        <dependency>
            <groupId>io.micrometer</groupId>
            <artifactId>micrometer-registry-azure-monitor</artifactId>
            <version>1.1.0</version>
        </dependency>
        
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>applicationinsights-web-auto</artifactId>
            <version>2.5.0</version>
        </dependency>
     ```

2. Dosyayı kaynaklar klasörüne koyun: `ApplicationInsights.xml`

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">
    
       <!-- The key from the portal: -->
       <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>
    
       <!-- HTTP request component (not required for bare API) -->
       <TelemetryModules>
          <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
          <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
          <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
       </TelemetryModules>
    
       <!-- Events correlation (not required for bare API) -->
       <!-- These initializers add context data to each event -->
       <TelemetryInitializers>
          <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
          <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
          <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
          <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
          <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>
       </TelemetryInitializers>
    
    </ApplicationInsights>
    ```

3. Örnek Servlet sınıfı (zamanlayıcı metrik yakar):

    ```Java
        @WebServlet("/hello")
        public class TimedDemo extends HttpServlet {
    
          private static final long serialVersionUID = -4751096228274971485L;
    
          @Override
          @Timed(value = "hello.world")
          protected void doGet(HttpServletRequest request, HttpServletResponse response)
              throws ServletException, IOException {
    
            response.getWriter().println("Hello World!");
            MeterRegistry registry = (MeterRegistry) getServletContext().getAttribute("AzureMonitorMeterRegistry");
    
        //create new Timer metric
            Timer sampleTimer = registry.timer("timer");
            Stream<Integer> infiniteStream = Stream.iterate(0, i -> i+1);
            infiniteStream.limit(10).forEach(integer -> {
              try {
                Thread.sleep(1000);
                sampleTimer.record(integer, TimeUnit.MILLISECONDS);
              } catch (Exception e) {}
               });
          }
          @Override
          public void init() throws ServletException {
            System.out.println("Servlet " + this.getServletName() + " has started");
          }
          @Override
          public void destroy() {
            System.out.println("Servlet " + this.getServletName() + " has stopped");
          }
    
        }
    
    ```

4. Örnek yapılandırma sınıfı:

    ```Java
         @WebListener
         public class MeterRegistryConfiguration implements ServletContextListener {
     
           @Override
           public void contextInitialized(ServletContextEvent servletContextEvent) {
     
         // Create AzureMonitorMeterRegistry
           private final AzureMonitorConfig config = new AzureMonitorConfig() {
             @Override
             public String get(String key) {
                 return null;
             }
            @Override
               public Duration step() {
                 return Duration.ofSeconds(60);}
     
             @Override
             public boolean enabled() {
                 return false;
             }
         };
     
      MeterRegistry azureMeterRegistry = AzureMonitorMeterRegistry.builder(config);
     
             //set the config to be used elsewhere
             servletContextEvent.getServletContext().setAttribute("AzureMonitorMeterRegistry", azureMeterRegistry);
     
           }
     
           @Override
           public void contextDestroyed(ServletContextEvent servletContextEvent) {
     
           }
         }
    ```

Ölçümler hakkında daha fazla bilgi edinmek için [Mikrometre belgelerine](https://micrometer.io/docs/)bakın.

Farklı ölçüm türlerinin nasıl oluşturulacağını anlatan diğer örnek kodresmi[Micrometer GitHub repo'sunda](https://github.com/micrometer-metrics/micrometer/tree/master/samples/micrometer-samples-core/src/main/java/io/micrometer/core/samples)bulunabilir.

## <a name="how-to-bind-additional-metrics-collection"></a>Ek ölçüm toplama bağlama

### <a name="springbootspring"></a>SpringBoot/Bahar

İlgili metrik kategoriden bir fasulye oluşturun. Örneğin, Guava önbellek Ölçümlerine ihtiyacımız olduğunu varsayabiliriz:

```Java
    @Bean
    GuavaCacheMetrics guavaCacheMetrics() {
        Return new GuavaCacheMetrics();
    }
```
Varsayılan olarak etkinleştirilmeyen ancak yukarıdaki şekilde bağlanabilen birkaç ölçüm vardır. Tam liste için [resmi Micrometer GitHub repo'ya](https://github.com/micrometer-metrics/micrometer/tree/master/micrometer-core/src/main/java/io/micrometer/core/instrument/binder )bakın.

### <a name="non-spring-apps"></a>Bahar dışı uygulamalar
Yapılandırma dosyasına aşağıdaki bağlama kodunu ekleyin:
```Java 
    New GuavaCacheMetrics().bind(registry);
```

## <a name="next-steps"></a>Sonraki adımlar

* Mikrometre hakkında daha fazla bilgi edinmek için resmi [Mikrometre belgelerine](https://micrometer.io/docs)bakın.
* Azure'da Bahar hakkında bilgi edinmek için Azure [belgelerinde](https://docs.microsoft.com/java/azure/spring-framework/?view=azure-java-stable)resmi Bahar'a bakın.
