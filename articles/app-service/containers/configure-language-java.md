---
title: Linux Java uygulamalarını yapılandırma
description: Uygulamanız için önceden oluşturulmuş bir Java kapsayıcısını yapılandırmayı öğrenin. Bu makalede en sık kullanılan yapılandırma görevleri gösterilmektedir.
keywords: Azure App Service, Web uygulaması, Linux, Oss, Java, Java Ee, Jee, JavaEE
author: bmitchell287
manager: barbkess
ms.devlang: java
ms.topic: article
ms.date: 11/22/2019
ms.author: brendm
ms.reviewer: cephalin
ms.custom: seodec18
ms.openlocfilehash: 496f519ba5e4eb17060ee35ed86fba45c85336d6
ms.sourcegitcommit: 34eb5e4d303800d3b31b00b361523ccd9eeff0ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84905740"
---
# <a name="configure-a-linux-java-app-for-azure-app-service"></a>Azure App Service için Linux Java uygulaması yapılandırma

Linux üzerinde Azure App Service, Java geliştiricilerinin, tam olarak yönetilen bir Linux tabanlı hizmette Tomcat veya Java standart sürüm (SE) paketlenmiş Web uygulamalarını hızlıca oluşturmasına, dağıtmasına ve ölçeklendirmesine olanak tanır. Maven eklentilerine sahip uygulamaları, komut satırından veya IntelliJ, tutulma veya Visual Studio Code gibi düzenleyicilerde dağıtın.

Bu kılavuz, App Service içinde yerleşik bir Linux kapsayıcısı kullanan Java geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure App Service hiç kullanmadıysanız, [Java hızlı](quickstart-java.md)başlangıcını izleyin.

## <a name="deploying-your-app"></a>Uygulamanızı dağıtma

. Jar ve. war dosyalarını dağıtmak için [Maven eklentisini Azure App Service](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) kullanabilirsiniz. Popüler Ides ile dağıtım, [Azure Toolkit for IntelliJ](/azure/developer/java/toolkit-for-intellij/) veya [Azure Toolkit for Eclipse](/azure/developer/java/toolkit-for-eclipse)ile de desteklenir.

Aksi takdirde, dağıtım yönteminiz arşiv türüne bağlı olacaktır:

- . War dosyalarını Tomcat 'e dağıtmak için, arka `/api/wardeploy/` uç noktasını kullanarak arşiv DOSYANıZı gönderin. Bu API hakkında daha fazla bilgi için lütfen [Bu belgelere](https://docs.microsoft.com/azure/app-service/deploy-zip#deploy-war-file)bakın.
- Java & görüntülerinde. jar dosyalarını dağıtmak için `/api/zipdeploy/` kudu sitesinin uç noktasını kullanın. Bu API hakkında daha fazla bilgi için lütfen [Bu belgelere](https://docs.microsoft.com/azure/app-service/deploy-zip#rest)bakın.

FTP kullanarak. war veya. jar 'nizi dağıtmayın. FTP aracı başlangıç betiklerini, bağımlılıklarını veya diğer çalışma zamanı dosyalarını karşıya yüklemek üzere tasarlanmıştır. Web uygulamalarını dağıtmak için en iyi seçenek değildir.

## <a name="logging-and-debugging-apps"></a>Uygulamaları günlüğe kaydetme ve hata ayıklama

Performans raporları, trafik görselleştirmeleri ve sistem durumu sağlaması, Azure portal aracılığıyla her uygulama için kullanılabilir. Daha fazla bilgi için bkz. [Azure App Service tanılamayı genel bakış](../overview-diagnostics.md).

### <a name="ssh-console-access"></a>SSH konsol erişimi

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

### <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-linux-no-h.md)]

Daha fazla bilgi için bkz. [Cloud Shell akış günlükleri](../troubleshoot-diagnostic-logs.md#in-cloud-shell).

### <a name="app-logging"></a>Uygulama günlüğü

Uygulamanızın standart konsol çıkışını ve standart konsol hatası akışlarını yerel dosya sistemine veya Azure Blob depolama alanına yazmak üzere App Service yapılandırmak için Azure portal veya [Azure CLI](/cli/azure/webapp/log#az-webapp-log-config) aracılığıyla [Uygulama günlüğünü](../troubleshoot-diagnostic-logs.md?toc=/azure/app-service/containers/toc.json#enable-application-logging-windows) etkinleştirin. Yerel App Service dosya sistemi örneğine günlük kaydı, yapılandırıldıktan sonra 12 saat devre dışı bırakılır. Daha uzun süre bekletmeye ihtiyacınız varsa, uygulamayı bir BLOB depolama kapsayıcısına çıktı yazacak şekilde yapılandırın. Java ve Tomcat uygulama günlüklerinizi */Home/LogFiles/Application/* dizininde bulabilirsiniz.

>[!NOTE]
>Yerel App Service dosya sistemine kaydetme, 12 saat sonra yalnızca Windows tabanlı uygulama hizmetleri için geçerli olacak şekilde devre dışı bırakılmaktadır. Linux tabanlı uygulama hizmetleri için Azure Blob depolama günlüğü, yalnızca [Azure izleyici (Önizleme)](/azure/app-service/troubleshoot-diagnostic-logs#send-logs-to-azure-monitor-preview) kullanılarak yapılandırılabilir 

Uygulamanız izleme için [Logback](https://logback.qos.ch/) veya [Log4J](https://logging.apache.org/log4j) kullanıyorsa, bu izlemeleri gözden geçirme için [Application Insights Java izleme günlüklerini keşfet](/azure/application-insights/app-insights-java-trace-logs)' de günlüğe kaydetme çerçevesi yapılandırma yönergelerini kullanarak Azure Application Insights iletebilirsiniz.

### <a name="troubleshooting-tools"></a>Sorun giderme araçları

Yerleşik Java görüntüleri, [alp Linux](https://alpine-linux.readthedocs.io/en/latest/getting_started.html) işletim sistemini temel alır. `apk`Herhangi bir sorun giderme aracını veya komutunu yüklemek için paket yöneticisini kullanın.

### <a name="flight-recorder"></a>Uçuş Kaydedicisi

App Service tüm Linux Java görüntülerinin, JVM 'ye kolayca bağlanıp bir profil oluşturucu kaydı başlatmaya veya yığın dökümü oluşturmaya başlayabilmeniz için, Zulu Uçuş Kaydedicisi 'nin yüklü olması gerekir.

#### <a name="timed-recording"></a>Zamanlanmış kayıt

Kullanmaya başlamak için App Service SSH ve `jcmd` çalıştıran tüm Java işlemlerinin bir listesini görmek için komutunu çalıştırın. Jcmd 'nin yanı sıra, bir işlem KIMLIK numarası (PID) ile çalışan Java uygulamanızı görmeniz gerekir.

```shell
078990bbcd11:/home# jcmd
Picked up JAVA_TOOL_OPTIONS: -Djava.net.preferIPv4Stack=true
147 sun.tools.jcmd.JCmd
116 /home/site/wwwroot/app.jar
```

JVM 'nin 30 saniyelik bir kaydını başlatmak için aşağıdaki komutu yürütün. Bu, JVM profilini oluşturacak ve giriş dizininde *jfr_example. jfr* adlı bir jfr dosyası oluşturacaktır. (116 değerini Java uygulamanızın pid 'si ile değiştirin.)

```shell
jcmd 116 JFR.start name=MyRecording settings=profile duration=30s filename="/home/jfr_example.jfr"
```

30 saniyelik Aralık sırasında, çalışırken kaydın gerçekleşdiğini doğrulayabilirsiniz `jcmd 116 JFR.check` . Bu, verilen Java işleminin tüm kayıtlarını gösterir.

#### <a name="continuous-recording"></a>Sürekli kayıt

Çalışma zamanı performansı ([kaynak](https://assets.azul.com/files/Zulu-Mission-Control-data-sheet-31-Mar-19.pdf)) üzerinde en az etkiyle Java uygulamanızı sürekli olarak profil oluştururken, Zulu uçuş kaydedicisini kullanabilirsiniz. Bunu yapmak için, gerekli yapılandırmayla JAVA_OPTS adlı bir uygulama ayarı oluşturmak için aşağıdaki Azure CLı komutunu çalıştırın. Uygulamanız başlatıldığında JAVA_OPTS uygulama ayarının içeriği `java` komutuna geçirilir.

```azurecli
az webapp config appsettings set -g <your_resource_group> -n <your_app_name> --settings JAVA_OPTS=-XX:StartFlightRecording=disk=true,name=continuous_recording,dumponexit=true,maxsize=1024m,maxage=1d
```

Kayıt başladıktan sonra, komutunu kullanarak istediğiniz zaman geçerli kayıt verilerinin dökümünü alabilirsiniz `JFR.dump` .

```shell
jcmd <pid> JFR.dump name=continuous_recording filename="/home/recording1.jfr"
```

Daha fazla bilgi için lütfen [Jcmd komut başvurusuna](https://docs.oracle.com/javacomponents/jmc-5-5/jfr-runtime-guide/comline.htm#JFRRT190)bakın.

### <a name="analyzing-recordings"></a>Kayıtlar çözümleniyor

JFR dosyanızı yerel makinenize indirmek için [FTPS](../deploy-ftp.md) 'yi kullanın. JFR dosyasını çözümlemek için, [Zulu görevi denetimini](https://www.azul.com/products/zulu-mission-control/)indirip yükleyin. Zulu dili görev denetimi yönergeleri için [Azul belgelerine](https://docs.azul.com/zmc/) ve [yükleme yönergelerine](https://docs.microsoft.com/java/azure/jdk/java-jdk-flight-recorder-and-mission-control)bakın.

## <a name="customization-and-tuning"></a>Özelleştirme ve ayarlama

Linux için Azure App Service, Azure portal ve CLı aracılığıyla kullanıma hazır ayarlama ve özelleştirmeyi destekler. Java 'a özgü olmayan Web uygulaması yapılandırması için aşağıdaki makaleleri gözden geçirin:

- [Uygulama ayarlarını yapılandırma](../configure-common.md?toc=/azure/app-service/containers/toc.json#configure-app-settings)
- [Özel etki alanı ayarlama](../app-service-web-tutorial-custom-domain.md?toc=/azure/app-service/containers/toc.json)
- [SSL bağlamalarını yapılandırma](../configure-ssl-bindings.md?toc=/azure/app-service/containers/toc.json)
- [CDN ekleme](../../cdn/cdn-add-to-web-app.md?toc=/azure/app-service/containers/toc.json)
- [Kudu sitesini yapılandırma](https://github.com/projectkudu/kudu/wiki/Configurable-settings#linux-on-app-service-settings)

### <a name="set-java-runtime-options"></a>Java çalışma zamanı seçeneklerini ayarla

Hem Tomcat hem de Java örneği ortamlarında ayrılan belleği veya diğer JVM çalışma zamanı seçeneklerini ayarlamak için, seçeneklerle adlı bir [uygulama ayarı](../configure-common.md?toc=/azure/app-service/containers/toc.json#configure-app-settings) oluşturun `JAVA_OPTS` . App Service Linux bu ayarı, başlatıldığında Java çalışma zamanına bir ortam değişkeni olarak geçirir.

Azure portal, Web uygulaması için **uygulama ayarları** altında adlı yeni bir uygulama ayarı oluşturun, örneğin, gibi `JAVA_OPTS` ek ayarları içerir `-Xms512m -Xmx1204m` .

Uygulama ayarını Maven eklentisi ' nden yapılandırmak için, Azure eklentisi bölümüne ayar/değer etiketleri ekleyin. Aşağıdaki örnek, belirli bir minimum ve en büyük Java yığın boyutunu ayarlar:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Xms512m -Xmx1204m</value>
    </property>
</appSettings>
```

Tek bir uygulama çalıştıran geliştiriciler App Service planında bir dağıtım yuvası ile, aşağıdaki seçenekleri kullanabilir:

- B1 ve S1 örnekleri:`-Xms1024m -Xmx1024m`
- B2 ve S2 örnekleri:`-Xms3072m -Xmx3072m`
- B3 ve S3 örnekleri:`-Xms6144m -Xmx6144m`

Uygulama yığını ayarlarını ayarladığınızda, en iyi bellek ayırmayı bulmak için App Service planı ayrıntılarınızı gözden geçirin ve birden çok uygulama ve dağıtım yuvası ihtiyaçlarına sahip olmak üzere birden fazla uygulama ve dağıtım yuvası

Bir JAR uygulaması dağıtıyorsanız, yerleşik görüntünün uygulamanızı doğru bir şekilde tanımlayabilmesi için *app. jar* olarak adlandırılmalıdır. (Maven eklentisi bu yeniden adlandırmayı otomatik olarak yapar.) JAR 'nizi *app. jar*'e yeniden adlandırmak ISTEMIYORSANıZ, jar dosyanızı çalıştırmak için komutuyla bir kabuk betiği yükleyebilirsiniz. Sonra bu betiğin tam yolunu portalın Yapılandırma bölümündeki [Başlangıç Dosyası](app-service-linux-faq.md#built-in-images) metin kutusuna yapıştırın. Başlatma dizesi içine yerleştirildiği dizinden çalıştırılmaz. Bu nedenle başlatma dizenizde her zaman başvuru dosyalarının mutlak yollarını kullanın (örneğin: `java -jar /home/myapp/myapp.jar`).

### <a name="turn-on-web-sockets"></a>Web yuvalarını aç

Uygulamanın **uygulama ayarlarındaki** Azure Portal Web Yuvaları desteğini açın. Ayarın etkili olması için uygulamayı yeniden başlatmanız gerekir.

Aşağıdaki komutla Azure CLı kullanarak Web soketi desteğini açın:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --web-sockets-enabled true
```

Sonra uygulamanızı yeniden başlatın:

```azurecli-interactive
az webapp stop --name <app-name> --resource-group <resource-group-name>
az webapp start --name <app-name> --resource-group <resource-group-name>
```

### <a name="set-default-character-encoding"></a>Varsayılan karakter kodlamasını ayarla

Azure portal, Web uygulaması için **uygulama ayarları** altında, değeri ile adlı yeni bir uygulama ayarı oluşturun `JAVA_OPTS` `-Dfile.encoding=UTF-8` .

Alternatif olarak, App Service Maven eklentisini kullanarak uygulama ayarını yapılandırabilirsiniz. Eklenti yapılandırmasına ayar adı ve değer etiketlerini ekleyin:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Dfile.encoding=UTF-8</value>
    </property>
</appSettings>
```

### <a name="adjust-startup-timeout"></a>Başlangıç zaman aşımını ayarla

Java uygulamanız özellikle büyükse, başlangıç süresi sınırını artırmanız gerekir. Bunu yapmak için, bir uygulama ayarı oluşturun `WEBSITES_CONTAINER_START_TIME_LIMIT` ve App Service zaman aşımından önce bekleyeceği saniye sayısına ayarlayın. En büyük değer `1800` saniyedir.

### <a name="pre-compile-jsp-files"></a>JSP dosyalarını önceden derle

Tomcat uygulamalarının performansını artırmak için, App Service dağıtım yapmadan önce JSP dosyalarınızı derleyebilirsiniz. Apache Sling tarafından sağlanmış [Maven eklentisini](https://sling.apache.org/components/jspc-maven-plugin/plugin-info.html) veya bu [ant derleme dosyasını](https://tomcat.apache.org/tomcat-9.0-doc/jasper-howto.html#Web_Application_Compilation)kullanarak kullanabilirsiniz.

## <a name="secure-applications"></a>Uygulamaları güvenli hale getirme

Linux için App Service çalıştıran Java uygulamaları, diğer uygulamalarla aynı [güvenlik en iyi](/azure/security/security-paas-applications-using-app-services) uygulamaları kümesine sahiptir.

### <a name="authenticate-users-easy-auth"></a>Kullanıcıların kimliğini doğrulama (kolay kimlik doğrulaması)

**Kimlik doğrulama ve yetkilendirme** seçeneğiyle Azure Portal uygulama kimlik doğrulamasını ayarlayın. Buradan, Facebook, Google veya GitHub gibi Azure Active Directory veya sosyal oturum açma bilgilerini kullanarak kimlik doğrulamasını etkinleştirebilirsiniz. Azure portal yapılandırma yalnızca tek bir kimlik doğrulama sağlayıcısı yapılandırılırken kullanılabilir. Daha fazla bilgi için bkz. [App Service uygulamanızı Azure Active Directory oturum açma](../configure-authentication-provider-aad.md?toc=/azure/app-service/containers/toc.json) bilgilerini ve diğer kimlik sağlayıcılarının ilgili makalelerini kullanacak şekilde yapılandırma. Birden çok oturum açma sağlayıcısını etkinleştirmeniz gerekiyorsa, [App Service kimlik doğrulamasını özelleştirme](../app-service-authentication-how-to.md?toc=/azure/app-service/containers/toc.json) makalesindeki yönergeleri izleyin.

#### <a name="tomcat"></a>Tomcat

Tomcat uygulamanız, birincil nesneyi bir harita nesnesine aktararak kullanıcının taleplerine doğrudan erişim sağlayabilir. Map nesnesi her talep türünü, bu tür için talepler koleksiyonuna eşler. Aşağıdaki kodda `request` bir örneğidir `HttpServletRequest` .

```java
Map<String, Collection<String>> map = (Map<String, Collection<String>>) request.getUserPrincipal();
```

Artık `Map` belirli bir talep için nesneyi inceleyebilirsiniz. Örneğin, aşağıdaki kod parçacığı tüm talep türleri boyunca yinelenir ve her bir koleksiyonun içeriğini yazdırır.

```java
for (Object key : map.keySet()) {
        Object value = map.get(key);
        if (value != null && value instanceof Collection {
            Collection claims = (Collection) value;
            for (Object claim : claims) {
                System.out.println(claims);
            }
        }
    }
```

Kullanıcıları imzalamak için `/.auth/ext/logout` yolunu kullanın. Diğer eylemleri gerçekleştirmek için lütfen [App Service kimlik doğrulaması ve yetkilendirme kullanımı](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to)hakkındaki belgelere bakın. Ayrıca, Tomcat [Httpservletrequest arabiriminde](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html) ve yöntemleri için resmi belgeler vardır. Aşağıdaki servlet yöntemleri App Service yapılandırmanıza göre de alınır:

```java
public boolean isSecure()
public String getRemoteAddr()
public String getRemoteHost()
public String getScheme()
public int getServerPort()
```

Bu özelliği devre dışı bırakmak için, bir değeri olan adlı bir uygulama ayarı oluşturun `WEBSITE_AUTH_SKIP_PRINCIPAL` `1` . App Service tarafından eklenen tüm servlet filtrelerini devre dışı bırakmak için, bir değeri ile adlı bir ayar oluşturun `WEBSITE_SKIP_FILTERS` `1` .

#### <a name="spring-boot"></a>Spring Boot

Spring Boot geliştiricileri tanıdık yay güvenlik ek açıklamalarını ve API 'Lerini kullanarak uygulamaları güvenli hale getirmek için [Azure Active Directory Spring Boot Starter](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) 'ı kullanabilir. *Application. Properties* dosyanızdaki en büyük üst bilgi boyutunu artırdığınızdan emin olun. Değerini öneririz `16384` .

### <a name="configure-tlsssl"></a>TLS/SSL 'yi yapılandırma

Var olan bir SSL sertifikasını karşıya yüklemek ve uygulamanızın etki alanı adına bağlamak için, [Özel BIR DNS adını Azure App Service BIR SSL bağlamasıyla güvenli hale](../configure-ssl-bindings.md?toc=/azure/app-service/containers/toc.json) getirin. Varsayılan olarak, uygulamanız HTTP bağlantılarına hala izin verir-SSL ve TLS 'yi zorlamak için öğreticideki belirli adımları izleyin.

### <a name="use-keyvault-references"></a>Keykasası başvurularını kullanma

[Azure Keykasası](../../key-vault/general/overview.md) , erişim ilkeleri ve denetim geçmişi ile merkezi gizli yönetim sağlar. Gizli dizileri (parolalar veya bağlantı dizeleri gibi) anahtar kasasında saklayabilir ve ortam değişkenleri aracılığıyla uygulamanızdaki bu gizli dizilere erişebilirsiniz.

İlk olarak, uygulamanıza [Key Vault erişim verme](../app-service-key-vault-references.md#granting-your-app-access-to-key-vault) ve [bir uygulama ayarında gizli diziniz Için bir Anahtar Kasası başvurusu yapma](../app-service-key-vault-references.md#reference-syntax)yönergelerini izleyin. App Service terminaline uzaktan erişirken ortam değişkenini yazdırarak başvurunun gizli olarak çözümlendiğini doğrulayabilirsiniz.

Bu gizli dizileri Spring veya Tomcat yapılandırma dosyasına eklemek için ortam değişkeni ekleme sözdizimi ( `${MY_ENV_VAR}` ) kullanın. Spring yapılandırma dosyaları için lütfen [externalized yapılandırmalarında](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)bu belgelere bakın.

### <a name="using-the-java-key-store"></a>Java anahtar deposunu kullanma

Varsayılan olarak, [App Service Linux 'a yüklenen](../configure-ssl-certificate.md) tüm ortak veya özel sertifikalar, kapsayıcı başladığında Ilgili Java anahtar depolarına yüklenir. Sertifikanızı karşıya yükledikten sonra, Java anahtar deposuna yüklenmek üzere App Service yeniden başlatmanız gerekir. Ortak sertifikalar, konumundaki anahtar deposuna yüklenir `$JAVA_HOME/jre/lib/security/cacerts` ve özel sertifikalar içinde depolanır `$JAVA_HOME/lib/security/client.jks` .

Java anahtar deposundaki sertifikalarla JDBC bağlantınızı şifrelemek için ek yapılandırma gerekebilir. Lütfen seçtiğiniz JDBC sürücünüz için belgelere bakın.

- [PostgreSQL](https://jdbc.postgresql.org/documentation/head/ssl-client.html)
- [SQL Server](https://docs.microsoft.com/sql/connect/jdbc/connecting-with-ssl-encryption?view=sql-server-ver15)
- [MySQL](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-reference-using-ssl.html)
- [MongoDB](https://mongodb.github.io/mongo-java-driver/3.4/driver/tutorials/ssl/)
- [Cassandra](https://docs.datastax.com/en/developer/java-driver/4.3/)

#### <a name="initializing-the-java-key-store"></a>Java anahtar deposunu başlatma

Nesneyi başlatmak için `import java.security.KeyStore` , anahtar deposu dosyasını parolasıyla yükleyin. Her iki anahtar deposu için varsayılan parola "changeıt" dir.

```java
KeyStore keyStore = KeyStore.getInstance("jks");
keyStore.load(
    new FileInputStream(System.getenv("JAVA_HOME")+"/lib/security/cacets"),
    "changeit".toCharArray());

KeyStore keyStore = KeyStore.getInstance("pkcs12");
keyStore.load(
    new FileInputStream(System.getenv("JAVA_HOME")+"/lib/security/client.jks"),
    "changeit".toCharArray());
```

#### <a name="manually-load-the-key-store"></a>Anahtar deposunu el ile yükleme

Sertifikaları anahtar deposuna el ile yükleyebilirsiniz. `SKIP_JAVA_KEYSTORE_LOAD` `1` Sertifikaları anahtar deposuna otomatik olarak yüklemek App Service devre dışı bırakmak için değeri olan bir uygulama ayarı oluşturun. Azure portal üzerinden App Service yüklenen tüm genel sertifikalar altında depolanır `/var/ssl/certs/` . Özel sertifikalar altında depolanır `/var/ssl/private/` .

App Service [BIR SSH bağlantısı açıp](app-service-linux-ssh-support.md) komutu çalıştırarak Java anahtar aracında etkileşim kurabilir veya hata ayıklayabilirsiniz `keytool` . Komutların listesi için bkz. [anahtar araç belgeleri](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) . KeyStore API 'SI hakkında daha fazla bilgi için lütfen [resmi belgelere](https://docs.oracle.com/javase/8/docs/api/java/security/KeyStore.html)başvurun.

## <a name="configure-apm-platforms"></a>APM platformlarını yapılandırma

Bu bölümde, Newrelik ve AppDynamics uygulama performansı izleme (APM) platformlarıyla Linux üzerinde Azure App Service dağıtılan Java uygulamalarının nasıl bağlanacağı gösterilmektedir.

### <a name="configure-new-relic"></a>Yeni relik yapılandırma

1. [NewRelic.com](https://newrelic.com/signup) adresinde bir Newrelik hesabı oluşturun
2. Newrelik 'ten Java aracısını indirin, *newrelic-java-x.x.x.zip*benzer bir dosya adı olacaktır.
3. Lisans anahtarınızı kopyalayın, aracıyı daha sonra yapılandırmak için gerekir.
4. [App Service örneğine SSH ekleyin](app-service-linux-ssh-support.md) ve */Home/site/Wwwroot/APM*adlı yeni bir dizin oluşturun.
5. Paketi açılan Newrelik Java aracı dosyalarını */Home/site/Wwwroot/APM*altındaki bir dizine yükleyin. Aracınızın dosyaları */Home/site/Wwwroot/APM/newrelik*konumunda olmalıdır.
6. */Home/site/Wwwroot/APM/newrelic/newrelic.exe* konumundaki YAML dosyasını değiştirin ve yer tutucu lisans değerini kendi lisans anahtarınızla değiştirin.
7. Azure portal, App Service uygulamanıza gidin ve yeni bir uygulama ayarı oluşturun.
    - Uygulamanız **Java SE**kullanıyorsa, değeriyle adlı bir ortam değişkeni oluşturun `JAVA_OPTS` `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar` .
    - **Tomcat**kullanıyorsanız, değeriyle adlı bir ortam değişkeni oluşturun `CATALINA_OPTS` `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar` .

### <a name="configure-appdynamics"></a>AppDynamics 'i yapılandırma

1. [AppDynamics.com](https://www.appdynamics.com/community/register/) adresinde bir AppDynamics hesabı oluşturun
2. AppDynamics Web sitesinden Java aracısını indirin, dosya adı *AppServerAgent-x.x.x.xxxxx.zip* benzerdir.
3. [App Service örneğine SSH ekleyin](app-service-linux-ssh-support.md) ve */Home/site/Wwwroot/APM*adlı yeni bir dizin oluşturun.
4. Java aracı dosyalarını */Home/site/Wwwroot/APM*altındaki bir dizine yükleyin. Aracınızın dosyaları */Home/site/Wwwroot/APM/AppDynamics*konumunda olmalıdır.
5. Azure portal, App Service uygulamanıza gidin ve yeni bir uygulama ayarı oluşturun.
    - **Java SE**kullanıyorsanız, `JAVA_OPTS` App Service adınız olan değer ile adlı bir ortam değişkeni oluşturun `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` `<app-name>` .
    - **Tomcat**kullanıyorsanız, `CATALINA_OPTS` App Service adınız olan değer ile adlı bir ortam değişkeni oluşturun `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` `<app-name>` .

> [!NOTE]
> Veya için bir ortam değişkeniniz zaten varsa `JAVA_OPTS` `CATALINA_OPTS` , `-javaagent:/...` geçerli değerin sonuna bu seçeneği ekleyin.

## <a name="configure-jar-applications"></a>JAR uygulamalarını yapılandırma

### <a name="starting-jar-apps"></a>JAR uygulamaları başlatılıyor

Varsayılan olarak, App Service JAR uygulamanızın *app. jar*olarak adlandırıldığını bekliyor. Bu ada sahipse, otomatik olarak çalıştırılır. Maven kullanıcıları için,pom.xmlbölümüne ekleyerek JAR adını ayarlayabilirsiniz `<finalName>app</finalName>` `<build>` . * * Özelliği ayarlayarak [Gradle 'de aynısını](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html#org.gradle.api.tasks.bundling.Jar:archiveFileName) yapabilirsiniz `archiveFileName` .

JAR 'niz için farklı bir ad kullanmak istiyorsanız, JAR dosyanızı yürüten [Başlangıç komutunu](app-service-linux-faq.md#built-in-images) da sağlamanız gerekir. Örneğin, `java -jar my-jar-app.jar`. Portalda başlangıç Komutunuz için değeri, yapılandırma > genel ayarlar altında veya adlı bir uygulama ayarıyla ayarlayabilirsiniz `STARTUP_COMMAND` .

### <a name="server-port"></a>Sunucu bağlantı noktası

App Service Linux gelen istekleri 80 numaralı bağlantı noktasına yönlendirir, bu nedenle uygulamanızın bağlantı noktası 80 ' de dinleme yapmanız gerekir. Bunu uygulamanızın yapılandırmasında (örneğin, Spring 'ın *Application. Properties* dosyası) veya başlangıç komutunuz (örneğin, `java -jar spring-app.jar --server.port=80` ) yapabilirsiniz. Ortak Java çerçeveleri için lütfen aşağıdaki belgelere bakın:

- [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html#howto-use-short-command-line-arguments)
- [Mini Java](http://sparkjava.com/documentation#embedded-web-server)
- [Micronaut](https://docs.micronaut.io/latest/guide/index.html#runningSpecificPort)
- [Play çerçevesi](https://www.playframework.com/documentation/2.6.x/ConfiguringHttps#Configuring-HTTPS)
- [Vertx](https://vertx.io/docs/vertx-core/java/#_start_the_server_listening)
- [Quarkus](https://quarkus.io/guides/application-configuration-guide)

## <a name="data-sources"></a>Veri kaynakları

### <a name="tomcat"></a>Tomcat

Bu yönergeler tüm veritabanı bağlantıları için geçerlidir. Yer tutucuları, seçtiğiniz veritabanının sürücü sınıfı adı ve JAR dosyası ile doldurmanız gerekir. , Ortak veritabanları için sınıf adları ve sürücü indirmeleri içeren bir tablodur.

| Veritabanı   | Sürücü sınıfı adı                             | JDBC sürücüsü                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [İndir](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [İndir](https://dev.mysql.com/downloads/connector/j/) ("platformdan bağımsız" seçeneğini belirleyin) |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [İndir](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-2017#download)                                                           |

Tomcat 'i Java veritabanı bağlantısı (JDBC) veya Java Kalıcılık API 'SI (JPA) kullanacak şekilde yapılandırmak için, önce başlangıçta `CATALINA_OPTS` Tomcat tarafından okunan ortam değişkenini özelleştirin. Bu değerleri [App Service Maven eklentisindeki](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md)bir uygulama ayarı aracılığıyla ayarlayın:

```xml
<appSettings>
    <property>
        <name>CATALINA_OPTS</name>
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>
    </property>
</appSettings>
```

Veya Azure Portal içindeki **yapılandırma**  >  **uygulaması ayarları** sayfasında ortam değişkenlerini ayarlayın.

Daha sonra, veri kaynağının bir uygulama için mi yoksa Tomcat servlet üzerinde çalışan tüm uygulamalar için mi kullanılabilir olacağını saptayın.

#### <a name="application-level-data-sources"></a>Uygulama düzeyi veri kaynakları

1. Projenizin *meta INF/* dizininde bir *context.xml* dosyası oluşturun. Yoksa *meta INF/* dizin oluşturun.

2. *context.xml*, `Context` veri kaynağını bir JNDI adresine bağlamak için bir öğe ekleyin. `driverClassName`Yer tutucusunu, yukarıdaki tablodaki sürücünüzün sınıf adıyla değiştirin.

    ```xml
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ```

3. Uygulamanızdaki veri kaynağını kullanmak için uygulamanızın *web.xml* güncelleştirin.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="shared-server-level-resources"></a>Paylaşılan sunucu düzeyi kaynakları

Paylaşılan, sunucu düzeyinde bir veri kaynağı eklemek, Tomcat 'in server.xml düzenlemenizi gerektirir. İlk olarak, bir [Başlangıç betiğini](app-service-linux-faq.md#built-in-images) karşıya yükleyin ve yolu **yapılandırma**  >  **Başlangıç komutunda**betiğin yolunu ayarlayın. [FTP](../deploy-ftp.md)kullanarak başlangıç betiğini karşıya yükleyebilirsiniz.

Başlangıç betiğinizin server.xml dosyasına bir [XSL dönüştürmesi](https://www.w3schools.com/xml/xsl_intro.asp) yapıp elde edilen XML dosyasının çıktısını almak için kullanılır `/usr/local/tomcat/conf/server.xml` . Başlangıç betiği APK aracılığıyla libxslt 'yi yüklemelidir. XSL dosyanız ve başlangıç betiğimiz FTP aracılığıyla karşıya yüklenebilir. Aşağıda örnek bir başlangıç betiği verilmiştir.

```sh
# Install libxslt. Also copy the transform file to /home/tomcat/conf/
apk add --update libxslt

# Usage: xsltproc --output output.xml style.xsl input.xml
xsltproc --output /home/tomcat/conf/server.xml /home/tomcat/conf/transform.xsl /usr/local/tomcat/conf/server.xml
```

Örnek bir xsl dosyası aşağıda verilmiştir. Örnek xsl dosyası, Tomcat server.xml yeni bir bağlayıcı düğümü ekler.

```xml
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:output method="xml" indent="yes"/>

  <xsl:template match="@* | node()" name="Copy">
    <xsl:copy>
      <xsl:apply-templates select="@* | node()"/>
    </xsl:copy>
  </xsl:template>

  <xsl:template match="@* | node()" mode="insertConnector">
    <xsl:call-template name="Copy" />
  </xsl:template>

  <xsl:template match="comment()[not(../Connector[@scheme = 'https']) and
                                 contains(., '&lt;Connector') and
                                 (contains(., 'scheme=&quot;https&quot;') or
                                  contains(., &quot;scheme='https'&quot;))]">
    <xsl:value-of select="." disable-output-escaping="yes" />
  </xsl:template>

  <xsl:template match="Service[not(Connector[@scheme = 'https'] or
                                   comment()[contains(., '&lt;Connector') and
                                             (contains(., 'scheme=&quot;https&quot;') or
                                              contains(., &quot;scheme='https'&quot;))]
                                  )]
                      ">
    <xsl:copy>
      <xsl:apply-templates select="@* | node()" mode="insertConnector" />
    </xsl:copy>
  </xsl:template>

  <!-- Add the new connector after the last existing Connnector if there is one -->
  <xsl:template match="Connector[last()]" mode="insertConnector">
    <xsl:call-template name="Copy" />

    <xsl:call-template name="AddConnector" />
  </xsl:template>

  <!-- ... or before the first Engine if there is no existing Connector -->
  <xsl:template match="Engine[1][not(preceding-sibling::Connector)]"
                mode="insertConnector">
    <xsl:call-template name="AddConnector" />

    <xsl:call-template name="Copy" />
  </xsl:template>

  <xsl:template name="AddConnector">
    <!-- Add new line -->
    <xsl:text>&#xa;</xsl:text>
    <!-- This is the new connector -->
    <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true" 
               maxThreads="150" scheme="https" secure="true" 
               keystroreFile="${{user.home}}/.keystore" keystorePass="changeit"
               clientAuth="false" sslProtocol="TLS" />
  </xsl:template>
  
</xsl:stylesheet>
```

#### <a name="finalize-configuration"></a>Yapılandırmayı Sonlandır

Son olarak, sürücü JARs ' ı Tomcat Sınıfyoluna yerleştirip App Service yeniden başlatın.

1. JDBC sürücü dosyalarının, */Home/Tomcat/lib* dizinine yerleştirerek Tomcat ClassLoader için kullanılabilir olduğundan emin olun. (Henüz yoksa, bu dizini oluşturun.) Bu dosyaları App Service örneğine yüklemek için aşağıdaki adımları uygulayın:

    1. [Cloud Shell](https://shell.azure.com), WebApp uzantısını yüklerken:

      ```azurecli-interactive
      az extension add -–name webapp
      ```

    2. App Service için yerel sisteminizden bir SSH tüneli oluşturmak için aşağıdaki CLı komutunu çalıştırın:

      ```azurecli-interactive
      az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
      ```

    3. SFTP istemcinizdeki yerel tünel oluşturma bağlantı noktasına bağlanın ve dosyaları */Home/Tomcat/lib* klasörüne yükleyin.

    Alternatif olarak, bir FTP istemcisini kullanarak JDBC sürücüsünü karşıya yükleyebilirsiniz. [FTP kimlik bilgilerinizi almak için bu yönergeleri](../deploy-configure-credentials.md?toc=/azure/app-service/containers/toc.json)izleyin.

2. Sunucu düzeyinde bir veri kaynağı oluşturduysanız App Service Linux uygulamasını yeniden başlatın. Tomcat `CATALINA_BASE` `/home/tomcat` , güncelleştirilmiş yapılandırmayı sıfırlayacak ve kullanacak şekilde başlatılır.

### <a name="spring-boot"></a>Spring Boot

Spring Boot uygulamalarındaki veri kaynaklarına bağlanmak için bağlantı dizeleri oluşturmanızı ve bunları *uygulamanızın. Properties* dosyasına ekleme.

1. App Service sayfasının "yapılandırma" bölümünde, dize için bir ad belirleyin, JDBC Bağlantı dizenizi değer alanına yapıştırın ve türü "özel" olarak ayarlayın. İsteğe bağlı olarak, bu bağlantı dizesini yuva ayarı olarak ayarlayabilirsiniz.

    Bu bağlantı dizesine, uygulamamız tarafından adlı bir ortam değişkeni olarak erişilebilir `CUSTOMCONNSTR_<your-string-name>` . Örneğin, yukarıda oluşturduğumuz bağlantı dizesinin adı olacaktır `CUSTOMCONNSTR_exampledb` .

2. *Application. Properties* dosyanızda, bu bağlantı dizesine ortam değişkeni adı ile başvur. Örneğimiz için aşağıdakileri kullanacağız.

    ```yml
    app.datasource.url=${CUSTOMCONNSTR_exampledb}
    ```

Bu konuyla ilgili daha fazla bilgi için lütfen veri erişimi ve [externalized yapılandırmalarında](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) [Spring Boot belgelerine](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-data-access.html) bakın.

## <a name="use-redis-as-a-session-cache-with-tomcat"></a>Redin 'i Tomcat ile oturum önbelleği olarak kullanma

Tomcat 'i [redsıs Için Azure önbelleği](/azure/azure-cache-for-redis/)gibi bir dış oturum deposu kullanacak şekilde yapılandırabilirsiniz. Bu, bir kullanıcı uygulamanızın başka bir örneğine aktarıldığında (örneğin, otomatik ölçeklendirme, yeniden başlatma veya yük devretme gerçekleştiğinde) Kullanıcı oturumu durumunu korumanıza olanak sağlar.

Tomcat 'i Redwith ile kullanmak için uygulamanızı bir [Persistentmanager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html) uygulaması kullanacak şekilde yapılandırmanız gerekir. Aşağıdaki adımlarda bu işlem Özet [oturum Yöneticisi kullanılarak açıklanmıştır: redin-Store](https://github.com/pivotalsoftware/session-managers/tree/master/redis-store) örnek olarak.

1. Bir bash terminali açın ve `<variable>=<value>` Aşağıdaki ortam değişkenlerinin her birini ayarlamak için kullanın.

    | Değişken                 | Değer                                                                      |
    |--------------------------|----------------------------------------------------------------------------|
    | RESOURCEGROUP_NAME       | App Service örneğinizi içeren kaynak grubunun adı.       |
    | WEBAPP_NAME              | App Service örneğinizin adı.                                     |
    | WEBAPP_PLAN_NAME         | App Service planınızın adı.                                         |
    | BÖLGE                   | Uygulamanızın barındırıldığı bölgenin adı.                           |
    | REDIS_CACHE_NAME         | Redsıs örneği için Azure önbelleğinizin adı.                           |
    | REDIS_PORT               | Redsıs önbelleğinizin dinlediği SSL bağlantı noktası.                             |
    | REDIS_PASSWORD           | Örneğiniz için birincil erişim anahtarı.                                  |
    | REDIS_SESSION_KEY_PREFIX | Uygulamanızdan gelen oturum anahtarlarını tanımlamak için belirttiğiniz bir değer. |

    ```bash
    RESOURCEGROUP_NAME=<resource group>
    WEBAPP_NAME=<web app>
    WEBAPP_PLAN_NAME=<App Service plan>
    REGION=<region>
    REDIS_CACHE_NAME=<cache>
    REDIS_PORT=<port>
    REDIS_PASSWORD=<access key>
    REDIS_SESSION_KEY_PREFIX=<prefix>
    ```

    Hizmet örneğinizin **Özellikler** veya **erişim anahtarları** bölümlerine bakarak Azure Portal ad, bağlantı noktası ve erişim anahtarı bilgilerini bulabilirsiniz.

2. Uygulamanızın *src/Main/WebApp/meta-INF/context.xml* dosyasını aşağıdaki içerikle oluşturun veya güncelleştirin:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <Context path="">
        <!-- Specify Redis Store -->
        <Valve className="com.gopivotal.manager.SessionFlushValve" />
        <Manager className="org.apache.catalina.session.PersistentManager">
            <Store className="com.gopivotal.manager.redis.RedisStore"
                   connectionPoolSize="20"
                   host="${REDIS_CACHE_NAME}.redis.cache.windows.net"
                   port="${REDIS_PORT}"
                   password="${REDIS_PASSWORD}"
                   sessionKeyPrefix="${REDIS_SESSION_KEY_PREFIX}"
                   timeout="2000"
            />
        </Manager>
    </Context>
    ```

    Bu dosya, uygulamanız için oturum Yöneticisi uygulamasını belirtir ve yapılandırır. Bu, önceki adımda ayarladığınız ortam değişkenlerini kullanarak hesap bilgilerinizin kaynak dosyalarınıza ait olmasını sağlar.

3. Oturum yöneticisinin JAR dosyasını App Service örneğine yüklemek için FTP 'yi kullanın, bu dosyayı */Home/Tomcat/lib* dizinine koyarak yükleyin. Daha fazla bilgi için bkz. [FTP/S kullanarak Azure App Service uygulamanızı dağıtma](https://docs.microsoft.com/azure/app-service/deploy-ftp).

4. App Service örneğiniz için [oturum benzeşimi tanımlama bilgisini](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) devre dışı bırakın. Bunu uygulamanızda gezinerek Azure portal yapabilir ve sonra, **yapılandırma > genel ayarları ARR benzeşimi >** seçeneğini **devre dışı**olarak ayarlayabilirsiniz. Alternatif olarak, aşağıdaki komutu kullanabilirsiniz:

    ```azurecli
    az webapp update -g <resource group> -n <webapp name> --client-affinity-enabled false
    ```

    Varsayılan olarak App Service, mevcut oturumlardaki istemci isteklerinin uygulamanızın aynı örneğine yönlendirildiğinden emin olmak için oturum benzeşimi tanımlama bilgilerini kullanır. Bu varsayılan davranış yapılandırma gerektirmez, ancak uygulama örneğiniz yeniden başlatıldığında veya trafik başka bir örneğe yeniden yönlendirilbaşladığında Kullanıcı oturumu durumunu koruyamaz. Oturum tanımlama bilgisi tabanlı yönlendirmeyi kapatmak için [mevcut ARR örnek benzeşim yapılandırmasını devre dışı](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) bıraktığınızda, yapılandırılan oturum deposunun girişim olmadan çalışmasına izin verebilirsiniz.

5. App Service örneğinizin **Özellikler** bölümüne gidin ve **ek giden IP adresleri**bulun. Bunlar, uygulamanız için tüm olası giden IP adreslerini temsil eder. Bunları bir sonraki adımda kullanmak üzere kopyalayın.

6. Her IP adresi için, Redsıs örneği için Azure önbelleğinizin bir güvenlik duvarı kuralı oluşturun. Bunu, reddo örneğinizin **güvenlik duvarı** bölümünden Azure Portal yapabilirsiniz. Her kural için benzersiz bir ad belirtin ve **Başlangıç IP adresini** ve **bitiş IP adresı** değerlerini aynı IP adresi olarak ayarlayın.

7. Redsıs örneğinizin **Gelişmiş ayarlar** bölümüne gidin ve **yalnızca SSL aracılığıyla erişime izin ver** ' i **Hayır**olarak ayarlayın. Bu, Azure altyapısı aracılığıyla App Service örneğinizin Redsıs önbelleğiyle iletişim kurmasını sağlar.

8. `azure-webapp-maven-plugin`Redsıs hesap bilgilerinize başvurmak için uygulamanızın *pom.xml* dosyasındaki yapılandırmayı güncelleştirin. Bu dosya, daha önce ayarladığınız ortam değişkenlerini kullanarak hesap bilgilerinizin kaynak dosyalarınıza ait olmasını sağlar.

    Gerekirse `1.9.1` sürümünü [Azure App Service için Maven Eklentisinin](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) geçerli sürümüyle değiştirin.

    ```xml
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.9.1</version>
        <configuration>            
            <!-- Web App information -->
            <schemaVersion>v2</schemaVersion>
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appServicePlanName>${WEBAPP_PLAN_NAME}-${REGION}</appServicePlanName>
            <appName>${WEBAPP_NAME}-${REGION}</appName>
            <region>${REGION}</region>            
            <runtime>
                <os>linux</os>
                <javaVersion>jre8</javaVersion>
                <webContainer>tomcat 9.0</webContainer>
            </runtime>

            <appSettings>
                <property>
                    <name>REDIS_CACHE_NAME</name>
                    <value>${REDIS_CACHE_NAME}</value>
                </property>
                <property>
                    <name>REDIS_PORT</name>
                    <value>${REDIS_PORT}</value>
                </property>
                <property>
                    <name>REDIS_PASSWORD</name>
                    <value>${REDIS_PASSWORD}</value>
                </property>
                <property>
                    <name>REDIS_SESSION_KEY_PREFIX</name>
                    <value>${REDIS_SESSION_KEY_PREFIX}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Xms2048m -Xmx2048m -DREDIS_CACHE_NAME=${REDIS_CACHE_NAME} -DREDIS_PORT=${REDIS_PORT} -DREDIS_PASSWORD=${REDIS_PASSWORD} IS_SESSION_KEY_PREFIX=${REDIS_SESSION_KEY_PREFIX}</value>
                </property>

            </appSettings>

        </configuration>
    </plugin>
    ```

9. Uygulamanızı yeniden derleyin ve dağıtın.

    ```bash
    mvn package -DskipTests azure-webapp:deploy
    ```

Uygulamanız artık, oturum yönetimi için Redsıs önbelleğinizi kullanacaktır.

Bu yönergeleri test etmek için kullanabileceğiniz bir örnek için GitHub 'da [ölçeklendirme-durum bilgisi olan Java-Web-App-on-Azure](https://github.com/Azure-Samples/scaling-stateful-java-web-app-on-azure) deposuna bakın.

## <a name="docker-containers"></a>Docker kapsayıcıları

Kapsayıcılarınızdaki Azure tarafından desteklenen Zulu dili JDK 'yi kullanmak için, önceden oluşturulmuş görüntüleri [Azure için desteklenen Azul Zulu Kurumsal indirme sayfasından](https://www.azul.com/downloads/azure-only/zulu/) belgelendiğinden veya `Dockerfile` [Microsoft Java GitHub deposu](https://github.com/Microsoft/java/tree/master/docker)' ndan gelen örnekleri kullandığınızdan emin olun.

## <a name="statement-of-support"></a>Destek beyanı

### <a name="runtime-availability"></a>Çalışma zamanı kullanılabilirliği

Linux için App Service, Java Web uygulamalarının yönetilen barındırılmasına yönelik iki çalışma zamanını destekler:

- Web arşivi (WAR) dosyaları olarak paketlenmiş uygulamalar çalıştırmak için [Tomcat servlet kapsayıcısı](https://tomcat.apache.org/) . Desteklenen sürümler 8,5 ve 9,0 ' dir.
- Java Arşivi (JAR) dosyaları olarak paketlenmiş uygulamalar çalıştırmak için Java çalışma zamanı ortamı. Desteklenen sürümler Java 8 ve 11 ' dir.

### <a name="jdk-versions-and-maintenance"></a>JDK sürümleri ve bakım

Azul Zulu kurumsal Derlemeleriyle OpenJDK, Azure için OpenJDK ve Microsoft ve Azul sistemleri tarafından desteklenen Azure Stack için ücretsiz, çok platformlu, üretime hazırlı bir dağıtımıdır. Java SE uygulamalarını oluşturmak ve çalıştırmak için gereken tüm bileşenleri içerir. JDK 'yi [Java JDK yüklemesinden](https://aka.ms/azure-jdks)yükleyebilirsiniz.

Desteklenen JDKs, her yıl Ocak, Nisan, Temmuz ve Ekim ayında otomatik olarak üç ayda bir düzeltme eki uygulanır.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Önemli güvenlik açıklarına yönelik düzeltme ekleri ve düzeltmeler Azul sistemlerinden kullanılabilir hale geldiğinde serbest bırakılır. "Ana" güvenlik açığı, [NIST ortak güvenlik açığı Puanlama sistemi, sürüm 2](https://nvd.nist.gov/cvss.cfm)üzerinde 9,0 veya üzeri bir taban puanı tarafından tanımlanır.

### <a name="deprecation-and-retirement"></a>Kullanımdan kaldırma ve kullanımdan kaldırma

Desteklenen bir Java çalışma zamanı devre dışı bırakırsa, etkilenen çalışma zamanını kullanan Azure geliştiricileri, çalışma zamanı kullanımdan kalkmadan önce en az altı ayda bir kullanımdan kaldırma olarak verilmeyecektir.

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure hızlı başlangıç, öğreticiler ve Java başvuru belgelerini bulmak için [Java geliştiricileri Için Azure](/java/azure/) 'u ziyaret edin.

Java geliştirmeye özgü olmayan Linux için App Service kullanma hakkında genel sorular, [App Service LINUX SSS](app-service-linux-faq.md)bölümünde yanıtlanmıştır.
