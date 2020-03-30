---
title: Azure Uygulama Öngörüleri'ndeki Java izleme günlüklerini keşfedin
description: Uygulama Öngörülerinde Log4J veya Logback izlemelerini arama
ms.topic: conceptual
ms.date: 05/18/2019
ms.openlocfilehash: da1b76d52ab93f4d1be7196d6eb7286579481119
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77657223"
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Uygulama Öngörüleri'ndeki Java izleme günlüklerini keşfedin
İzleme için Logback veya Log4J (v1.2 veya v2.0) kullanıyorsanız, izleme günlüklerinizin otomatik olarak Application Insights'a gönderilmesini sağlayabilir ve bunları keşfedip araştırabilirsiniz.

> [!TIP]
> Uygulama Öngörüleri Enstrümantasyon Anahtarınızı yalnızca bir kez ayarlamanız gerekir. Java Spring gibi bir çerçeve kullanıyorsanız, anahtarı uygulamanızın yapılandırmasında başka bir yerde kaydetmiş olabilirsiniz.

## <a name="using-the-application-insights-java-agent"></a>Uygulama Öngörüleri Java aracısını kullanma

Varsayılan olarak, Application Insights Java aracısı `WARN` düzeyinde ve üzerinde gerçekleştirilen günlüğe kaydetmeyi otomatik olarak yakalar.

`AI-Agent.xml` Dosyayı kullanarak yakalanan günlüğe kaydetme eşiğini değiştirebilirsiniz:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsightsAgent>
   <Instrumentation>
      <BuiltIn>
         <Logging threshold="info"/>
      </BuiltIn>
   </Instrumentation>
</ApplicationInsightsAgent>
```

Dosyayı kullanarak Java aracısının günlük yakalamasını `AI-Agent.xml` devre dışı kullanabilirsiniz:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsightsAgent>
   <Instrumentation>
      <BuiltIn>
         <Logging enabled="false"/>
      </BuiltIn>
   </Instrumentation>
</ApplicationInsightsAgent>
```

## <a name="alternatively-as-opposed-to-using-the-java-agent-you-can-follow-the-instructions-below"></a>Alternatif olarak (Java aracısını kullanmanın aksine), aşağıdaki talimatları takip edebilirsiniz

### <a name="install-the-java-sdk"></a>Java SDK'yı yükleyin

Daha önce yapmadıysanız, [Java için Application Insights SDK'yı][java]yüklemek için yönergeleri izleyin.

### <a name="add-logging-libraries-to-your-project"></a>Projenize günlük kitaplıkları ekleme
*Projeniz için uygun yolu seçin.*

#### <a name="if-youre-using-maven"></a>Maven kullanıyorsanız...
Projeniz yapı için Maven'i kullanmak üzere ayarlanmışsa, aşağıdaki kod parçacıklarından birini pom.xml dosyanızda birleştirin.

Sonra ikilileri indirmek için proje bağımlılıklarını yenileyin.

*Giriş*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v2.0*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v1.2*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a>Gradle kullanıyorsanız...
Projeniz yapı için Gradle'ı kullanmak üzere ayarlanmışsa, build.gradle dosyanızdaki `dependencies` gruba aşağıdaki satırlardan birini ekleyin:

Sonra ikilileri indirmek için proje bağımlılıklarını yenileyin.

**Giriş**

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '2.0.+'
```

**Log4J v2.0**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '2.0.+'
```

**Log4J v1.2**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '2.0.+'
```

#### <a name="otherwise-"></a>Aksi taktirde...
Uygulama Öngörüleri Java SDK'yı el ile yüklemek için yönergeleri izleyin, kavanozu indirin (Maven Central Page'e geldikten sonra yükleme bölümündeki 'jar' linkine tıklayın) uygun uygulama için ve indirilen apandis kavanozu projeye ekleyin.

| Günlükçü | İndirme | Kitaplık |
| --- | --- | --- |
| Giriş |[Logback apandis kavanozu](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-logback%22) |applicationinsights-logging-logback |
| Log4J v2.0 |[Log4J v2 appender Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j2%22) |uygulamalarinsights-log-log4j2 |
| Log4j v1.2 |[Log4J v1.2 appender Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j1_2%22) |uygulamalariçgörü-günlük-log4j1_2 |


### <a name="add-the-appender-to-your-logging-framework"></a>Uygulamayı günlük çerçevenize ekleyin
İzlemeleri almaya başlamak için, ilgili kod parçacıklarını Log4J veya Logback yapılandırma dosyasıyla birleştirin: 

*Giriş*

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
        <instrumentationKey>[APPLICATION_INSIGHTS_KEY]</instrumentationKey>
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

*Log4J v2.0*

```XML

    <Configuration packages="com.microsoft.applicationinsights.log4j.v2">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" instrumentationKey="[APPLICATION_INSIGHTS_KEY]" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>
```

*Log4J v1.2*

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
        <param name="instrumentationKey" value="[APPLICATION_INSIGHTS_KEY]" />
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

Application Insights appenders herhangi bir yapılandırılmış logger tarafından başvurulabilir ve mutlaka kök logger tarafından (yukarıdaki kod örneklerinde gösterildiği gibi).

## <a name="explore-your-traces-in-the-application-insights-portal"></a>Application Insights portalında izlerinizi keşfedin
Projenizi Uygulama Öngörüleri'ne iz gönderecek şekilde yapılandırdığınıza göre, bu izleri [Arama][diagnostic] bıçağındaki Application Insights portalında görüntüleyebilir ve arayabilirsiniz.

Loggers aracılığıyla gönderilen özel durumlar, portalda Özel Durum Telemetrisi olarak görüntülenir.

![Application Insights portalında, açık Arama](./media/java-trace-logs/01-diagnostics.png)

## <a name="next-steps"></a>Sonraki adımlar
[Tanılama arama][diagnostic]

<!--Link references-->

[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[java]: java-get-started.md


