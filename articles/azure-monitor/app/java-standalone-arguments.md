---
title: Tüm ortamlarda çalışan Java uygulamalarını izleme-Azure Izleyici Application Insights
description: Uygulamayı işaretlemeden Java tek başına aracı ile herhangi bir ortamda çalışan Java uygulamaları için uygulama performansı izleme. Dağıtılmış izleme ve uygulama eşlemesi.
ms.topic: conceptual
ms.date: 04/16/2020
ms.openlocfilehash: 527f1eaf04be7b5e8c89c12912a06d2f5d50321f
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82508046"
---
# <a name="configuring-jvm-args-java-standalone-agent-for-azure-monitor-application-insights"></a>Azure Izleyici için JVM args Java tek başına Aracısı 'nı yapılandırma Application Insights



## <a name="azure-environments"></a>Azure ortamları

[Uygulama hizmetleri](https://docs.microsoft.com/azure/app-service/configure-language-java#set-java-runtime-options)'ni yapılandırın.

## <a name="spring-boot"></a>Spring Boot

Daha önce `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` `-jar`bir yere JVM bağımsız değişkeni ekleyin, örneğin:

```
java -javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar -jar <myapp.jar>
```

## <a name="spring-boot-via-docker-entry-point"></a>Docker giriş noktası aracılığıyla Spring Boot

*Exec* formunu kullanıyorsanız, parametresini parametre listesine parametresinden önce `"-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar"` `"-jar"` bir yerde ekleyin, örneğin:

```
ENTRYPOINT ["java", "-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar", "-jar", "<myapp.jar>"]
```

*Kabuk* formunu kullanıyorsanız, daha önce `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` `-jar`bir yere JVM bağımsız değişkeni ekleyin, örneğin:

```
ENTRYPOINT java -javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar -jar <myapp.jar>
```

## <a name="tomcat-8-linux"></a>Tomcat 8 (Linux)

### <a name="tomcat-installed-via-apt-get-or-yum"></a>Veya ile `apt-get` yüklenen Tomcat`yum`

Veya `apt-get` `yum`aracılığıyla Tomcat yüklediyseniz, bir dosyanız `/etc/tomcat8/tomcat8.conf`olmalıdır.  Bu satırı Bu dosyanın sonuna ekleyin:

```
JAVA_OPTS="$JAVA_OPTS -javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar"
```

### <a name="tomcat-installed-via-download-and-unzip"></a>Tomcat, indirme ve unzip ile yüklendi

İndirme ve unzip aracılığıyla Tomcat 'i yüklediyseniz [https://tomcat.apache.org](https://tomcat.apache.org), bir dosyanız `<tomcat>/bin/catalina.sh`olmalıdır.  Aşağıdaki içerikle adlı `<tomcat>/bin/setenv.sh` aynı dizinde yeni bir dosya oluşturun:

```
CATALINA_OPTS="$CATALINA_OPTS -javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar"
```

Dosya `<tomcat>/bin/setenv.sh` zaten varsa, bu dosyayı değiştirip öğesine `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` `CATALINA_OPTS`ekleyin.


## <a name="tomcat-8-windows"></a>Tomcat 8 (Windows)

### <a name="running-tomcat-from-the-command-line"></a>Komut satırından Tomcat çalıştırma

Dosyayı `<tomcat>/bin/catalina.bat`bulun.  Aşağıdaki içerikle adlı `<tomcat>/bin/setenv.bat` aynı dizinde yeni bir dosya oluşturun:

```
set CATALINA_OPTS=%CATALINA_OPTS% -javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar
```

Tırnak işaretleri gerekli değildir, ancak bunları dahil etmek istiyorsanız, doğru yerleştirme şu şekilde olur:

```
set "CATALINA_OPTS=%CATALINA_OPTS% -javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar"
```

Dosya `<tomcat>/bin/setenv.bat` zaten varsa, bu dosyayı değiştirip öğesine `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` `CATALINA_OPTS`ekleyin.

### <a name="running-tomcat-as-a-windows-service"></a>Windows hizmeti olarak Tomcat çalıştırma

Dosyayı `<tomcat>/bin/tomcat8w.exe`bulun.  Bu çalıştırılabiliri çalıştırın ve `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` `Java Options` `Java` sekmesinde altına ekleyin.


## <a name="jboss-eap-7"></a>Jpatron EAP 7

### <a name="standalone-server"></a>Tek başına sunucu

Dosyadaki `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` `JBOSS_HOME/bin/standalone.conf` (Linux `JBOSS_HOME/bin/standalone.conf.bat` ) `JAVA_OPTS` veya (Windows) var olan ortam değişkenine ekleyin:

```java    ...
    JAVA_OPTS="<b>-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar</b> -Xms1303m -Xmx1303m ..."
    ...
```

### <a name="domain-server"></a>Etki alanı sunucusu

' `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` De `JBOSS_HOME/domain/configuration/host.xml`var olan `jvm-options` öğesine ekleyin:

```xml
...
<jvms>
    <jvm name="default">
        <heap size="64m" max-size="256m"/>
        <jvm-options>
            <option value="-server"/>
            <!--Add Java agent jar file here-->
            <option value="-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar"/>
            <option value="-XX:MetaspaceSize=96m"/>
            <option value="-XX:MaxMetaspaceSize=256m"/>
        </jvm-options>
    </jvm>
</jvms>
...
```

Tek bir konakta birden çok yönetilen sunucu çalıştırıyorsanız, her birine `applicationinsights.agent.id` `system-properties` `server`için öğesine eklemeniz gerekir:

```xml
...
<servers>
    <server name="server-one" group="main-server-group">
        <!--Edit system properties for server-one-->
        <system-properties> 
            <property name="applicationinsights.agent.id" value="..."/>
        </system-properties>
    </server>
    <server name="server-two" group="main-server-group">
        <socket-bindings port-offset="150"/>
        <!--Edit system properties for server-two-->
        <system-properties>
            <property name="applicationinsights.agent.id" value="..."/> 
        </system-properties>
    </server>
</servers>
...
```

Belirtilen `applicationinsights.agent.id` değer benzersiz olmalıdır. Her JVM işlemi kendi yerel ApplicationInsights yapılandırması ve yerel ApplicationInsights günlük dosyasına ihtiyaç duyduğunda, ApplicationInsights dizini altında bir alt dizin oluşturmak için kullanılır. Ayrıca, merkezi toplayıcıya raporlama yapıyorsanız, `applicationinsights.properties` dosya birden çok yönetilen sunucu tarafından paylaşılır ve bu nedenle, bu paylaşılan dosyadaki `applicationinsights.agent.id` `agent.id` ayarı geçersiz kılmak için belirtilen gereklidir. `applicationinsights.agent.rollup.id`, yönetilen sunucu başına `system-properties` `agent.rollup.id` ayarı geçersiz kılmanız gerekiyorsa sunucuda benzer şekilde belirtilebilir.


## <a name="jetty-9"></a>Jetty 9

Bu satırları Ekle`start.ini`

```
--exec
-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar
```


## <a name="payara-5"></a>Payara 5

' `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` De `glassfish/domains/domain1/config/domain.xml`var olan `jvm-options` öğesine ekleyin:

```xml
...
<java-config ...>
    <!--Edit the JVM options here-->
    <jvm-options>
        -javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar>
    </jvm-options>
        ...
</java-config>
...
```

## <a name="websphere-8"></a>Web küre 8

Yönetim konsolunu açın **sunucu > Web Sphere uygulama sunucuları > uygulama sunucuları**' na gidin, uygun uygulama sunucularını seçin ve açık ' a tıklayın: 

```
Java and Process Management > Process definition >  Java Virtual Machine
```
"Genel JVM bağımsız değişkenleri" içinde aşağıdakileri ekleyin:
```
-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar
```
Bundan sonra uygulama sunucusunu kaydedin ve yeniden başlatın.


## <a name="openliberty-18"></a>OpenLiberty 18

Sunucu dizininde yeni bir `jvm.options` dosya oluşturun (örneğin `<openliberty>/usr/servers/defaultServer`) ve şu satırı ekleyin:
```
-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar
```
