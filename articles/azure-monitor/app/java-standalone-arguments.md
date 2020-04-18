---
title: Her ortamda çalışan Java uygulamalarını izleme - Azure Monitor Uygulama Öngörüleri
description: Java bağımsız aracı ile herhangi bir ortamda çalışan Java uygulamaları için uygulama performansı izleme uygulama enstrümanting olmadan. Dağıtılmış izleme ve uygulama haritası.
ms.topic: conceptual
ms.date: 04/16/2020
ms.openlocfilehash: 08a83fbc05276808b62a0391a5c4217cc09f6d00
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641881"
---
# <a name="configuring-jvm-args-java-standalone-agent-for-azure-monitor-application-insights"></a>Azure Monitör Uygulama Öngörüleri için JVM args Java bağımsız aracının yapılandırılması



## <a name="azure-environments"></a>Azure ortamları

Uygulama [Hizmetlerini](https://docs.microsoft.com/azure/app-service/configure-language-java#set-java-runtime-options)Yapılandır.

## <a name="spring-boot"></a>Spring Boot

JVM arg'yi `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` `-jar <myapp.jar>`daha önce bir yere ekleyin, örneğin:

```
java -javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar -jar <myapp.jar>
```

> [!NOTE]
> Args sonra `-jar <myapp.jar>` yerleştirilen program args olarak uygulamaya geçirilir.


## <a name="tomcat-8-linux"></a>Tomcat 8 (Linux)

### <a name="tomcat-installed-via-apt-get-or-yum"></a>Tomcat üzerinden `apt-get` yüklenen veya`yum`

Tomcat'ı `apt-get` yüklediyseniz `yum`veya , o `/etc/tomcat8/tomcat8.conf`zaman bir dosyanız olmalıdır.  Bu satırı bu dosyanın sonuna ekleyin:

```
JAVA_OPTS="$JAVA_OPTS -javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar"
```

### <a name="tomcat-installed-via-download-and-unzip"></a>Tomcat indirme ve unzip üzerinden yüklü

Eğer download ve unzip üzerinden Tomcat [https://tomcat.apache.org](https://tomcat.apache.org)yüklü, o `<tomcat>/bin/catalina.sh`zaman bir dosya olmalıdır.  Aşağıdaki içerikle aynı `<tomcat>/bin/setenv.sh` dizinde yeni bir dosya oluşturun:

```
CATALINA_OPTS="$CATALINA_OPTS -javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar"
```

Dosya `<tomcat>/bin/setenv.sh` zaten varsa, o dosyayı `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` değiştirin ve `CATALINA_OPTS`ekle.


## <a name="tomcat-8-windows"></a>Tomcat 8 (Windows)

### <a name="running-tomcat-from-the-command-line"></a>Tomcat'ı komut satırından çalıştırma

Dosyayı `<tomcat>/bin/catalina.bat`bulun.  Aşağıdaki içerikle aynı `<tomcat>/bin/setenv.bat` dizinde yeni bir dosya oluşturun:

```
set CATALINA_OPTS=%CATALINA_OPTS% -javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar
```

Tırnak işaretleri gerekli değildir, ancak bunları eklemek isterseniz, uygun yerleşim:

```
set "CATALINA_OPTS=%CATALINA_OPTS% -javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar"
```

Dosya `<tomcat>/bin/setenv.bat` zaten varsa, bu dosyayı `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` değiştirin ve ekle. `CATALINA_OPTS`

### <a name="running-tomcat-as-a-windows-service"></a>Tomcat'ı Windows hizmeti olarak çalıştırma

Dosyayı `<tomcat>/bin/tomcat8w.exe`bulun.  Çalıştırılabilir çalıştırın `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` ve `Java Options` `Java` sekme altında ekleyin.


## <a name="jboss-eap-7"></a>Jboss EAP 7

### <a name="standalone-server"></a>Bağımsız sunucu

Dosya `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` `JBOSS_HOME/bin/standalone.conf` (Linux) veya `JBOSS_HOME/bin/standalone.conf.bat` (Windows) mevcut `JAVA_OPTS` ortam değişkenine ekleyin:

```java    ...
    JAVA_OPTS="<b>-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar</b> -Xms1303m -Xmx1303m ..."
    ...
```

### <a name="domain-server"></a>Etki alanı sunucusu

Varolana `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` `jvm-options` `JBOSS_HOME/domain/configuration/host.xml`ekle:

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

Tek bir ana bilgisayarda birden çok yönetilen sunucu `applicationinsights.agent.id` çalıştırıyorsanız, `server`her biri `system-properties` için eklemeniz gerekir:

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

Belirtilen `applicationinsights.agent.id` değer benzersiz olmalıdır. Her JVM işleminin kendi yerel uygulamalar alametleri config ve yerel uygulamalar alametleri günlük dosyasına ihtiyacı olduğu için, uygulama öngörüleri dizininde bir alt dizini oluşturmak için kullanılır. Ayrıca, merkezi toplayıcıya raporlama `applicationinsights.properties` varsa, dosya birden çok yönetilen sunucu `applicationinsights.agent.id` tarafından paylaşılır ve `agent.id` bu nedenle belirtilen paylaşılan dosyadaki ayarı geçersiz kılmak için gereklidir. `applicationinsights.agent.rollup.id`yönetilen sunucu başına `system-properties` `agent.rollup.id` ayarı geçersiz kılmanız gerekiyorsa, sunucunun benzer şekilde belirtilebilir.


## <a name="jetty-9"></a>İskele 9

Bu satırları ekleyin`start.ini`

```
--exec
-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar
```


## <a name="payara-5"></a>Payara 5

Varolana `-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar` `jvm-options` `glassfish/domains/domain1/config/domain.xml`ekle:

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

## <a name="websphere-8"></a>WebSphere 8

Açık Yönetim Konsolu **webs uygulama sunucuları > Uygulama sunucuları > sunuculara**gidin, uygun uygulama sunucuları seçin ve tıklayın: 

```
Java and Process Management > Process definition >  Java Virtual Machine
```
"Genel JVM bağımsız değişkenleri" aşağıdakileri ekleyin:
```
-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar
```
Bundan sonra, uygulama sunucusunu kaydedin ve yeniden başlatın.


## <a name="openliberty-18"></a>OpenLiberty 18

Sunucu dizininde `jvm.options` yeni bir dosya oluşturun `<openliberty>/usr/servers/defaultServer`(örneğin), ve şu satırı ekleyin:
```
-javaagent:path/to/applicationinsights-agent-3.0.0-PREVIEW.jar
```
