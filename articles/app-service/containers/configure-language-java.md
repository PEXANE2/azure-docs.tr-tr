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
ms.openlocfilehash: 9c95772c8f10d7170a06d1d6793545a60fc8dd7c
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75750746"
---
# <a name="configure-a-linux-java-app-for-azure-app-service"></a>Azure App Service için bir Linux Java uygulaması yapılandırma

Linux üzerinde Azure App Service, Java geliştiricilerinin, tam olarak yönetilen bir Linux tabanlı hizmette Tomcat, yayana veya Java standart sürüm (SE) paketlenmiş Web uygulamalarını hızlıca oluşturmasına, dağıtmasına ve ölçeklendirmesine olanak tanır. Maven eklentilerine sahip uygulamaları, komut satırından veya IntelliJ, tutulma veya Visual Studio Code gibi düzenleyicilerde dağıtın.

Bu kılavuz, App Service içinde yerleşik bir Linux kapsayıcısı kullanan Java geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure App Service hiç kullanmadıysanız, önce [PostgreSQL öğreticisiyle](tutorial-java-enterprise-postgresql-app.md) [Java hızlı başlangıç](quickstart-java.md) ve Java 'yı izleyin.

## <a name="deploying-your-app"></a>Uygulamanızı dağıtma

. Jar ve. war dosyalarını dağıtmak için [Maven eklentisini Azure App Service](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) kullanabilirsiniz. Popüler Ides ile dağıtım, [Azure Toolkit for IntelliJ](/java/azure/intellij/azure-toolkit-for-intellij) veya [Azure Toolkit for Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse)ile de desteklenir.

Aksi takdirde, dağıtım yönteminiz arşiv türüne bağlı olacaktır:

- . War dosyalarını Tomcat 'e dağıtmak için `/api/wardeploy/` uç noktasını kullanarak arşiv dosyanızı GÖNDERIN. Bu API hakkında daha fazla bilgi için lütfen [Bu belgelere](https://docs.microsoft.com/azure/app-service/deploy-zip#deploy-war-file)bakın.
- Java & görüntülerinde. jar dosyalarını dağıtmak için, kudu sitesinin `/api/zipdeploy/` uç noktasını kullanın. Bu API hakkında daha fazla bilgi için lütfen [Bu belgelere](https://docs.microsoft.com/azure/app-service/deploy-zip#rest)bakın.

FTP kullanarak. war veya. jar 'nizi dağıtmayın. FTP aracı başlangıç betiklerini, bağımlılıklarını veya diğer çalışma zamanı dosyalarını karşıya yüklemek üzere tasarlanmıştır. Web uygulamalarını dağıtmak için en iyi seçenek değildir.

## <a name="logging-and-debugging-apps"></a>Uygulamaları günlüğe kaydetme ve hata ayıklama

Performans raporları, trafik görselleştirmeleri ve sistem durumu sağlaması, Azure portal aracılığıyla her uygulama için kullanılabilir. Daha fazla bilgi için bkz. [Azure App Service tanılamayı genel bakış](../overview-diagnostics.md).

### <a name="ssh-console-access"></a>SSH konsol erişimi

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

### <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

Daha fazla bilgi için bkz. [Cloud Shell akış günlükleri](../troubleshoot-diagnostic-logs.md#in-cloud-shell).

### <a name="app-logging"></a>Uygulama günlüğü

Uygulamanızın standart konsol çıkışını ve standart konsol hatası akışlarını yerel dosya sistemine veya Azure Blob depolama alanına yazmak üzere App Service yapılandırmak için Azure portal veya [Azure CLI](/cli/azure/webapp/log#az-webapp-log-config) aracılığıyla [Uygulama günlüğünü](../troubleshoot-diagnostic-logs.md?toc=/azure/app-service/containers/toc.json#enable-application-logging-windows) etkinleştirin. Yerel App Service dosya sistemi örneğine günlük kaydı, yapılandırıldıktan sonra 12 saat devre dışı bırakılır. Daha uzun süre bekletmeye ihtiyacınız varsa, uygulamayı bir BLOB depolama kapsayıcısına çıktı yazacak şekilde yapılandırın. Java ve Tomcat uygulama günlüklerinizi */Home/LogFiles/Application/* dizininde bulabilirsiniz.

Uygulamanız izleme için [Logback](https://logback.qos.ch/) veya [Log4J](https://logging.apache.org/log4j) kullanıyorsa, bu izlemeleri gözden geçirme için [Application Insights Java izleme günlüklerini keşfet](/azure/application-insights/app-insights-java-trace-logs)' de günlüğe kaydetme çerçevesi yapılandırma yönergelerini kullanarak Azure Application Insights iletebilirsiniz.

### <a name="troubleshooting-tools"></a>Sorun giderme araçları

Yerleşik Java görüntüleri, [alp Linux](https://alpine-linux.readthedocs.io/en/latest/getting_started.html) işletim sistemini temel alır. Herhangi bir sorun giderme aracını veya komutunu yüklemek için `apk` paket yöneticisini kullanın.

### <a name="flight-recorder"></a>Uçuş Kaydedicisi

App Service tüm Linux Java görüntülerinin, JVM 'ye kolayca bağlanıp bir profil oluşturucu kaydı başlatmaya veya yığın dökümü oluşturmaya başlayabilmeniz için, Zulu Uçuş Kaydedicisi 'nin yüklü olması gerekir.

#### <a name="timed-recording"></a>Zamanlanmış kayıt

Kullanmaya başlamak için App Service SSH ve çalıştıran tüm Java işlemlerinin bir listesini görmek için `jcmd` komutunu çalıştırın. Jcmd 'nin yanı sıra, bir işlem KIMLIK numarası (PID) ile çalışan Java uygulamanızı görmeniz gerekir.

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

30 saniyelik Aralık sırasında, `jcmd 116 JFR.check`çalıştırarak kaydın gerçekleşdiğini doğrulayabilirsiniz. Bu, verilen Java işleminin tüm kayıtlarını gösterir.

#### <a name="continuous-recording"></a>Sürekli kayıt

Çalışma zamanı performansı ([kaynak](https://assets.azul.com/files/Zulu-Mission-Control-data-sheet-31-Mar-19.pdf)) üzerinde en az etkiyle Java uygulamanızı sürekli olarak profil oluştururken, Zulu uçuş kaydedicisini kullanabilirsiniz. Bunu yapmak için, gerekli yapılandırmayla JAVA_OPTS adlı bir uygulama ayarı oluşturmak için aşağıdaki Azure CLı komutunu çalıştırın. Uygulamanız başlatıldığında JAVA_OPTS uygulama ayarının içeriği `java` komutuna geçirilir.

```azurecli
az webapp config appsettings set -g <your_resource_group> -n <your_app_name> --settings JAVA_OPTS=-XX:StartFlightRecording=disk=true,name=continuous_recording,dumponexit=true,maxsize=1024m,maxage=1d
```

Kayıt başladıktan sonra, `JFR.dump` komutunu kullanarak istediğiniz zaman geçerli kayıt verilerinin dökümünü alabilirsiniz.

```shell
jcmd <pid> JFR.dump name=continuous_recording filename="/home/recording1.jfr"
```

Daha fazla bilgi için lütfen [Jcmd komut başvurusuna](https://docs.oracle.com/javacomponents/jmc-5-5/jfr-runtime-guide/comline.htm#JFRRT190)bakın.

### <a name="analyzing-recordings"></a>Kayıtlar çözümleniyor

JFR dosyanızı yerel makinenize indirmek için [FTPS](../deploy-ftp.md) 'yi kullanın. JFR dosyasını çözümlemek için, [Zulu görevi denetimini](https://www.azul.com/products/zulu-mission-control/)indirip yükleyin. Zulu dili görev denetimi yönergeleri için [Azul belgelerine](https://docs.azul.com/zmc/) ve [yükleme yönergelerine](https://docs.microsoft.com/java/azure/jdk/java-jdk-flight-recorder-and-mission-control)bakın.

## <a name="customization-and-tuning"></a>Özelleştirme ve ayarlama

Linux için Azure App Service, Azure portal ve CLı aracılığıyla kullanıma hazır ayarlama ve özelleştirmeyi destekler. Java 'a özgü olmayan Web uygulaması yapılandırması için aşağıdaki makaleleri gözden geçirin:

- [Uygulama ayarlarını yapılandır](../configure-common.md?toc=/azure/app-service/containers/toc.json#configure-app-settings)
- [Özel etki alanı ayarlama](../app-service-web-tutorial-custom-domain.md?toc=/azure/app-service/containers/toc.json)
- [SSL bağlamalarını yapılandırma](../configure-ssl-bindings.md?toc=/azure/app-service/containers/toc.json)
- [CDN ekleme](../../cdn/cdn-add-to-web-app.md?toc=/azure/app-service/containers/toc.json)
- [Kudu sitesini yapılandırma](https://github.com/projectkudu/kudu/wiki/Configurable-settings#linux-on-app-service-settings)

### <a name="set-java-runtime-options"></a>Java çalışma zamanı seçeneklerini ayarla

Hem Tomcat hem de Java örneği ortamlarında ayrılan belleği veya diğer JVM çalışma zamanı seçeneklerini ayarlamak için, seçeneklerle `JAVA_OPTS` adlı bir [uygulama ayarı](../configure-common.md?toc=/azure/app-service/containers/toc.json#configure-app-settings) oluşturun. App Service Linux bu ayarı, başlatıldığında Java çalışma zamanına bir ortam değişkeni olarak geçirir.

Azure portal, Web uygulaması için **uygulama ayarları** altında, `-Xms512m -Xmx1204m`gibi ek ayarları içeren `JAVA_OPTS` adlı yeni bir uygulama ayarı oluşturun.

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

- B1 ve S1 örnekleri: `-Xms1024m -Xmx1024m`
- B2 ve S2 örnekleri: `-Xms3072m -Xmx3072m`
- B3 ve S3 örnekleri: `-Xms6144m -Xmx6144m`

Uygulama yığını ayarlarını ayarladığınızda, en iyi bellek ayırmayı bulmak için App Service planı ayrıntılarınızı gözden geçirin ve birden çok uygulama ve dağıtım yuvası ihtiyaçlarına sahip olmak üzere birden fazla uygulama ve dağıtım yuvası

Bir JAR uygulaması dağıtıyorsanız, yerleşik görüntünün uygulamanızı doğru bir şekilde tanımlayabilmesi için *app. jar* olarak adlandırılmalıdır. (Maven eklentisi bu yeniden adlandırmayı otomatik olarak yapar.) JAR 'nizi *app. jar*'e yeniden adlandırmak ISTEMIYORSANıZ, jar dosyanızı çalıştırmak için komutuyla bir kabuk betiği yükleyebilirsiniz. Sonra bu betiğin tam yolunu portalın yapılandırma bölümündeki [başlangıç dosyası](app-service-linux-faq.md#built-in-images) metin kutusuna yapıştırın. Başlangıç betiği, yerleştirildiği dizinden çalışmaz. Bu nedenle, başlangıç betikinizdeki dosyalara başvurmak için her zaman mutlak yollar kullanın (örneğin: `java -jar /home/myapp/myapp.jar`).

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

Azure portal, Web uygulaması için **uygulama ayarları** altında, `JAVA_OPTS` adlı yeni bir uygulama ayarı oluşturun `-Dfile.encoding=UTF-8`değeri.

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

Java uygulamanız özellikle büyükse, başlangıç süresi sınırını artırmanız gerekir. Bunu yapmak için, bir uygulama ayarı oluşturun, `WEBSITES_CONTAINER_START_TIME_LIMIT` ve App Service zaman aşımından önce bekleyeceği saniye sayısına ayarlayın. En büyük değer `1800` saniyedir.

### <a name="pre-compile-jsp-files"></a>JSP dosyalarını önceden derle

Tomcat uygulamalarının performansını artırmak için, App Service dağıtım yapmadan önce JSP dosyalarınızı derleyebilirsiniz. Apache Sling tarafından sağlanmış [Maven eklentisini](https://sling.apache.org/components/jspc-maven-plugin/plugin-info.html) veya bu [ant derleme dosyasını](https://tomcat.apache.org/tomcat-9.0-doc/jasper-howto.html#Web_Application_Compilation)kullanarak kullanabilirsiniz.

## <a name="secure-applications"></a>Uygulamaları güvenli hale getirme

Linux için App Service çalıştıran Java uygulamaları, diğer uygulamalarla aynı [güvenlik en iyi](/azure/security/security-paas-applications-using-app-services) uygulamaları kümesine sahiptir.

### <a name="authenticate-users-easy-auth"></a>Kullanıcıların kimliğini doğrulama (kolay kimlik doğrulaması)

**Kimlik doğrulama ve yetkilendirme** seçeneğiyle Azure Portal uygulama kimlik doğrulamasını ayarlayın. Buradan, Facebook, Google veya GitHub gibi Azure Active Directory veya sosyal oturum açma bilgilerini kullanarak kimlik doğrulamasını etkinleştirebilirsiniz. Azure portal yapılandırma yalnızca tek bir kimlik doğrulama sağlayıcısı yapılandırılırken kullanılabilir. Daha fazla bilgi için bkz. [App Service uygulamanızı Azure Active Directory oturum açma](../configure-authentication-provider-aad.md?toc=/azure/app-service/containers/toc.json) bilgilerini ve diğer kimlik sağlayıcılarının ilgili makalelerini kullanacak şekilde yapılandırma. Birden çok oturum açma sağlayıcısını etkinleştirmeniz gerekiyorsa, [App Service kimlik doğrulamasını özelleştirme](../app-service-authentication-how-to.md?toc=/azure/app-service/containers/toc.json) makalesindeki yönergeleri izleyin.

#### <a name="tomcat-and-wildfly"></a>Tomcat ve Yavaya

Tomcat veya Yavaya yönelik uygulamalar, birincil nesneyi bir harita nesnesine aktararak kullanıcının taleplerine doğrudan erişim sağlayabilir. Map nesnesi her talep türünü, bu tür için talepler koleksiyonuna eşler. Aşağıdaki kodda, `request` bir `HttpServletRequest`örneğidir.

```java
Map<String, Collection<String>> map = (Map<String, Collection<String>>) request.getUserPrincipal();
```

Artık belirli bir talep için `Map` nesnesini inceleyebilirsiniz. Örneğin, aşağıdaki kod parçacığı tüm talep türleri boyunca yinelenir ve her bir koleksiyonun içeriğini yazdırır.

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

Bu özelliği devre dışı bırakmak için, `1`değeri olan `WEBSITE_AUTH_SKIP_PRINCIPAL` adlı bir uygulama ayarı oluşturun. App Service tarafından eklenen tüm servlet filtrelerini devre dışı bırakmak için, `1`değeriyle `WEBSITE_SKIP_FILTERS` adında bir ayar oluşturun.

#### <a name="spring-boot"></a>Spring Boot

Spring Boot geliştiricileri tanıdık yay güvenlik ek açıklamalarını ve API 'Lerini kullanarak uygulamaları güvenli hale getirmek için [Azure Active Directory Spring Boot Starter](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) 'ı kullanabilir. *Application. Properties* dosyanızdaki en büyük üst bilgi boyutunu artırdığınızdan emin olun. `16384`değerini öneririz.

### <a name="configure-tlsssl"></a>TLS/SSL 'yi yapılandırma

Var olan bir SSL sertifikasını karşıya yüklemek ve uygulamanızın etki alanı adına bağlamak için, [Özel BIR DNS adını Azure App Service BIR SSL bağlamasıyla güvenli hale](../configure-ssl-bindings.md?toc=/azure/app-service/containers/toc.json) getirin. Varsayılan olarak, uygulamanız HTTP bağlantılarına hala izin verir-SSL ve TLS 'yi zorlamak için öğreticideki belirli adımları izleyin.

### <a name="use-keyvault-references"></a>Keykasası başvurularını kullanma

[Azure Keykasası](../../key-vault/key-vault-overview.md) , erişim ilkeleri ve denetim geçmişi ile merkezi gizli yönetim sağlar. Gizli dizileri (parolalar veya bağlantı dizeleri gibi) anahtar kasasında saklayabilir ve ortam değişkenleri aracılığıyla uygulamanızdaki bu gizli dizilere erişebilirsiniz.

İlk olarak, uygulamanıza [Key Vault erişim verme](../app-service-key-vault-references.md#granting-your-app-access-to-key-vault) ve [bir uygulama ayarında gizli diziniz Için bir Anahtar Kasası başvurusu yapma](../app-service-key-vault-references.md#reference-syntax)yönergelerini izleyin. App Service terminaline uzaktan erişirken ortam değişkenini yazdırarak başvurunun gizli olarak çözümlendiğini doğrulayabilirsiniz.

Bu gizli dizileri Spring veya Tomcat yapılandırma dosyasına eklemek için ortam değişkeni ekleme söz dizimini (`${MY_ENV_VAR}`) kullanın. Spring yapılandırma dosyaları için lütfen [externalized yapılandırmalarında](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)bu belgelere bakın.

### <a name="using-the-java-key-store"></a>Java anahtar deposunu kullanma

Varsayılan olarak, [App Service Linux 'a yüklenen](../configure-ssl-certificate.md) tüm ortak veya özel sertifikalar, kapsayıcı başladığında Ilgili Java anahtar depolarına yüklenir. Sertifikanızı karşıya yükledikten sonra, Java anahtar deposuna yüklenmek üzere App Service yeniden başlatmanız gerekir. Ortak sertifikalar `$JAVA_HOME/jre/lib/security/cacerts`konumundaki anahtar deposuna yüklenir ve özel sertifikalar `$JAVA_HOME/lib/security/client.jks`depolanır.

Java anahtar deposundaki sertifikalarla JDBC bağlantınızı şifrelemek için ek yapılandırma gerekebilir. Lütfen seçtiğiniz JDBC sürücünüz için belgelere bakın.

- [PostgreSQL](https://jdbc.postgresql.org/documentation/head/ssl-client.html)
- [SQL Server](https://docs.microsoft.com/sql/connect/jdbc/connecting-with-ssl-encryption?view=sql-server-ver15)
- [MySQL](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-reference-using-ssl.html)
- [MongoDB](https://mongodb.github.io/mongo-java-driver/3.4/driver/tutorials/ssl/)
- [Cassandra](https://docs.datastax.com/en/developer/java-driver/4.3/)

#### <a name="initializing-the-java-key-store"></a>Java anahtar deposunu başlatma

`import java.security.KeyStore` nesnesini başlatmak için, anahtar deposu dosyasını parolayla yükleyin. Her iki anahtar deposu için varsayılan parola "changeıt" dir.

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

Sertifikaları anahtar deposuna el ile yükleyebilirsiniz. App Service sertifikaları anahtar deposuna otomatik olarak yüklemesini devre dışı bırakmak için `1` değeri ile `SKIP_JAVA_KEYSTORE_LOAD`bir uygulama ayarı oluşturun. Azure portal üzerinden App Service yüklenen tüm genel sertifikalar `/var/ssl/certs/`altında depolanır. Özel sertifikalar `/var/ssl/private/`altında depolanır.

App Service [BIR SSH bağlantısı açıp](app-service-linux-ssh-support.md) komut `keytool`çalıştırarak Java anahtar aracında etkileşim kurabilir veya hata ayıklayabilirsiniz. Komutların listesi için bkz. [anahtar araç belgeleri](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) . KeyStore API 'SI hakkında daha fazla bilgi için lütfen [resmi belgelere](https://docs.oracle.com/javase/8/docs/api/java/security/KeyStore.html)başvurun.

## <a name="configure-apm-platforms"></a>APM platformlarını yapılandırma

Bu bölümde, Newrelik ve AppDynamics uygulama performansı izleme (APM) platformlarıyla Linux üzerinde Azure App Service dağıtılan Java uygulamalarının nasıl bağlanacağı gösterilmektedir.

### <a name="configure-new-relic"></a>Yeni relik yapılandırma

1. [NewRelic.com](https://newrelic.com/signup) adresinde bir Newrelik hesabı oluşturun
2. Newrelik 'ten Java aracısını indirin, *newrelic-Java-x. x. x. zip*dosyasına benzer bir dosya adı olacaktır.
3. Lisans anahtarınızı kopyalayın, aracıyı daha sonra yapılandırmak için gerekir.
4. [App Service örneğine SSH ekleyin](app-service-linux-ssh-support.md) ve */Home/site/Wwwroot/APM*adlı yeni bir dizin oluşturun.
5. Paketi açılan Newrelik Java aracı dosyalarını */Home/site/Wwwroot/APM*altındaki bir dizine yükleyin. Aracınızın dosyaları */Home/site/Wwwroot/APM/newrelik*konumunda olmalıdır.
6. */Home/site/Wwwroot/APM/newrelic/newrelic.exe* konumundaki YAML dosyasını değiştirin ve yer tutucu lisans değerini kendi lisans anahtarınızla değiştirin.
7. Azure portal, App Service uygulamanıza gidin ve yeni bir uygulama ayarı oluşturun.
    - Uygulamanız **Java SE**kullanıyorsa, `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`değeri ile `JAVA_OPTS` adlı bir ortam değişkeni oluşturun.
    - **Tomcat**kullanıyorsanız, `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`değeri ile `CATALINA_OPTS` adlı bir ortam değişkeni oluşturun.
    - KıI kullanıyorsanız,Java Aracısı ve jpatron yapılandırması yükleme hakkında rehberlik Için [buradaki](https://docs.newrelic.com/docs/agents/java-agent/additional-installation/wildfly-version-11-installation-java) yeni relik belgelerine bakın.

### <a name="configure-appdynamics"></a>AppDynamics 'i yapılandırma

1. [AppDynamics.com](https://www.appdynamics.com/community/register/) adresinde bir AppDynamics hesabı oluşturun
2. AppDynamics Web sitesinden Java aracısını indirin, dosya adı *AppServerAgent-x. x. x. xxxxx. zip* ile benzerdir.
3. [App Service örneğine SSH ekleyin](app-service-linux-ssh-support.md) ve */Home/site/Wwwroot/APM*adlı yeni bir dizin oluşturun.
4. Java aracı dosyalarını */Home/site/Wwwroot/APM*altındaki bir dizine yükleyin. Aracınızın dosyaları */Home/site/Wwwroot/APM/AppDynamics*konumunda olmalıdır.
5. Azure portal, App Service uygulamanıza gidin ve yeni bir uygulama ayarı oluşturun.
    - **Java SE**kullanıyorsanız, `JAVA_OPTS` adında bir ortam değişkeni oluşturun; burada `<app-name>` App Service adınız `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`.
    - **Tomcat**kullanıyorsanız, `CATALINA_OPTS` adında bir ortam değişkeni oluşturun; burada `<app-name>` App Service adınız `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`.
    - KıI kullanıyorsanız,Java Aracısı ve jpatron yapılandırması yükleme hakkında rehberlik Için [buradaki](https://docs.appdynamics.com/display/PRO45/JBoss+and+Wildfly+Startup+Settings) AppDynamics belgelerine bakın.

> [!NOTE]
> `JAVA_OPTS` veya `CATALINA_OPTS`için bir ortam değişkeni zaten varsa, `-javaagent:/...` seçeneğini geçerli değerin sonuna ekleyin.

## <a name="configure-jar-applications"></a>JAR uygulamalarını yapılandırma

### <a name="starting-jar-apps"></a>JAR uygulamaları başlatılıyor

Varsayılan olarak, App Service JAR uygulamanızın *app. jar*olarak adlandırıldığını bekliyor. Bu ada sahipse, otomatik olarak çalıştırılır. Maven kullanıcıları için, *Pod. xml*'nizin `<build>` bölümüne `<finalName>app</finalName>` ekleyerek jar adını ayarlayabilirsiniz. `archiveFileName` özelliğini ayarlayarak [Gradle ile aynı](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html#org.gradle.api.tasks.bundling.Jar:archiveFileName) yapabilirsiniz.

JAR 'niz için farklı bir ad kullanmak istiyorsanız, JAR dosyanızı yürüten [Başlangıç komutunu](app-service-linux-faq.md#built-in-images) da sağlamanız gerekir. Örneğin, `java -jar my-jar-app.jar`. Portalda başlangıç Komutunuz için değeri, yapılandırma > Genel ayarlar altında veya `STARTUP_COMMAND`adlı bir uygulama ayarıyla ayarlayabilirsiniz.

### <a name="server-port"></a>Sunucu bağlantı noktası

App Service Linux gelen istekleri 80 numaralı bağlantı noktasına yönlendirir, bu nedenle uygulamanızın bağlantı noktası 80 ' de dinleme yapmanız gerekir. Bunu uygulamanızın yapılandırmasında (örneğin, Spring 'ın *Application. Properties* dosyası) veya başlangıç komutunuz (örneğin, `java -jar spring-app.jar --server.port=80`) yapabilirsiniz. Ortak Java çerçeveleri için lütfen aşağıdaki belgelere bakın:

- [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html#howto-use-short-command-line-arguments)
- [Mini Java](http://sparkjava.com/documentation#embedded-web-server)
- [Micronaut](https://docs.micronaut.io/latest/guide/index.html#runningSpecificPort)
- [Play çerçevesi](https://www.playframework.com/documentation/2.6.x/ConfiguringHttps#Configuring-HTTPS)
- [Vertx](https://vertx.io/docs/vertx-core/java/#_start_the_server_listening)
- [Quarkus](https://quarkus.io/guides/application-configuration-guide)

## <a name="data-sources"></a>Veri kaynakları

### <a name="tomcat"></a>Tomcat

Bu yönergeler tüm veritabanı bağlantıları için geçerlidir. Yer tutucuları, seçtiğiniz veritabanının sürücü sınıfı adı ve JAR dosyası ile doldurmanız gerekir. , Ortak veritabanları için sınıf adları ve sürücü indirmeleri içeren bir tablodur.

| Veritabanı   | Sürücü sınıfı adı                             | JDBC Sürücüsü                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [İndir](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [İndir](https://dev.mysql.com/downloads/connector/j/) ("platformdan bağımsız" seçeneğini belirleyin) |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [İndir](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-2017#available-downloads-of-jdbc-driver-for-sql-server)                                                           |

Tomcat 'i Java veritabanı bağlantısı (JDBC) veya Java Kalıcılık API 'SI (JPA) kullanacak şekilde yapılandırmak için, önce başlangıçta Tomcat tarafından okunan `CATALINA_OPTS` ortam değişkenini özelleştirin. Bu değerleri [App Service Maven eklentisindeki](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md)bir uygulama ayarı aracılığıyla ayarlayın:

```xml
<appSettings>
    <property>
        <name>CATALINA_OPTS</name>
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>
    </property>
</appSettings>
```

Ya da Azure portal içindeki **yapılandırma** > **uygulama ayarları** sayfasında ortam değişkenlerini ayarlayın.

Daha sonra, veri kaynağının bir uygulama için mi yoksa Tomcat servlet üzerinde çalışan tüm uygulamalar için mi kullanılabilir olacağını saptayın.

#### <a name="application-level-data-sources"></a>Uygulama düzeyi veri kaynakları

1. Projenizin *meta INF/* dizininde bir *Context. xml* dosyası oluşturun. Yoksa *meta INF/* dizin oluşturun.

2. *Context. xml*dosyasında veri kaynağını bir JNDI adresine bağlamak için bir `Context` öğesi ekleyin. `driverClassName` yer tutucusunu, yukarıdaki tablodaki sürücünüzün sınıf adıyla değiştirin.

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

3. Uygulamanızdaki veri kaynağını kullanmak için uygulamanızın *Web. xml* ' i güncelleştirin.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="shared-server-level-resources"></a>Paylaşılan sunucu düzeyi kaynakları

Paylaşılan, sunucu düzeyinde bir veri kaynağı eklemek, Tomcat 'in Server. xml ' i düzenlemenizi gerektirir. İlk olarak, bir [Başlangıç betiğini](app-service-linux-faq.md#built-in-images) karşıya yükleyin ve yolu **yapılandırma** > **Başlangıç komutunda**komut dosyasına ayarlayın. [FTP](../deploy-ftp.md)kullanarak başlangıç betiğini karşıya yükleyebilirsiniz.

Başlangıç komut dosyanız, Server. xml dosyasına bir [XSL dönüştürmesi](https://www.w3schools.com/xml/xsl_intro.asp) yapar ve sonuçta elde edilen xml dosyasının `/usr/local/tomcat/conf/server.xml`çıktı olur. Başlangıç betiği APK aracılığıyla libxslt 'yi yüklemelidir. XSL dosyanız ve başlangıç betiğimiz FTP aracılığıyla karşıya yüklenebilir. Aşağıda örnek bir başlangıç betiği verilmiştir.

```sh
# Install libxslt. Also copy the transform file to /home/tomcat/conf/
apk add --update libxslt

# Usage: xsltproc --output output.xml style.xsl input.xml
xsltproc --output /home/tomcat/conf/server.xml /home/tomcat/conf/transform.xsl /usr/local/tomcat/conf/server.xml
```

Örnek bir xsl dosyası aşağıda verilmiştir. Örnek xsl dosyası, Tomcat Server. xml dosyasına yeni bir bağlayıcı düğümü ekler.

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

2. Sunucu düzeyinde bir veri kaynağı oluşturduysanız App Service Linux uygulamasını yeniden başlatın. Tomcat, `CATALINA_BASE` `/home/tomcat` sıfırlar ve güncelleştirilmiş yapılandırmayı kullanır.

### <a name="spring-boot"></a>Spring Boot

Spring Boot uygulamalarındaki veri kaynaklarına bağlanmak için bağlantı dizeleri oluşturmanızı ve bunları *uygulamanızın. Properties* dosyasına ekleme.

1. App Service sayfasının "yapılandırma" bölümünde, dize için bir ad belirleyin, JDBC Bağlantı dizenizi değer alanına yapıştırın ve türü "özel" olarak ayarlayın. İsteğe bağlı olarak, bu bağlantı dizesini yuva ayarı olarak ayarlayabilirsiniz.

    Bu bağlantı dizesine, uygulamamız tarafından `CUSTOMCONNSTR_<your-string-name>`adlı bir ortam değişkeni olarak erişilebilir. Örneğin, yukarıda oluşturduğumuz bağlantı dizesi `CUSTOMCONNSTR_exampledb`olarak adlandırılıyordu.

2. *Application. Properties* dosyanızda, bu bağlantı dizesine ortam değişkeni adı ile başvur. Örneğimiz için aşağıdakileri kullanacağız.

    ```yml
    app.datasource.url=${CUSTOMCONNSTR_exampledb}
    ```

Bu konuyla ilgili daha fazla bilgi için lütfen veri erişimi ve [externalized yapılandırmalarında](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) [Spring Boot belgelerine](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-data-access.html) bakın.

## <a name="configure-java-ee-wildfly"></a>Java EE 'ı yapılandırma (Yavaya)

> [!NOTE]
> App Service Linux 'ta Java Enterprise Edition Şu anda önizleme aşamasındadır. Bu yığın üretime yönelik iş **için önerilmez.**

Linux üzerinde Azure App Service, Java geliştiricilerinin tam olarak yönetilen bir Linux tabanlı hizmette Java Enterprise (Java EE) uygulamalarını oluşturmalarına, dağıtmasına ve ölçeklendirmesine olanak tanır.  Temel Java kurumsal çalışma zamanı ortamı, [Açık kaynaklı bir](https://wildfly.org/) uygulama sunucusudur.

Bu bölüm aşağıdaki alt bölümleri içerir:

- [App Service ile ölçeklendirme](#scale-with-app-service)
- [Uygulama sunucusu yapılandırmasını özelleştirme](#customize-application-server-configuration)
- [Modülleri ve bağımlılıkları yükler](#install-modules-and-dependencies)
- [Veri kaynaklarını yapılandırma](#configure-data-sources)
- [İleti Aracısı olarak Service Bus kullanma](#use-service-bus-as-a-message-broker)

### <a name="scale-with-app-service"></a>App Service ile ölçeklendirme

Linux üzerinde App Service çalışan uygulama sunucusu, etki alanı yapılandırmasında değil tek başına modda çalışır. App Service planını ölçeklendirirseniz, her bir Yavama örneği tek başına sunucu olarak yapılandırılır.

[Ölçek kuralları](../../monitoring-and-diagnostics/monitoring-autoscale-get-started.md) ile uygulamanızı dikey veya yatay olarak ölçeklendirin ve [örnek sayınız arttırın](../manage-scale-up.md?toc=/azure/app-service/containers/toc.json).

### <a name="customize-application-server-configuration"></a>Uygulama sunucusu yapılandırmasını özelleştirme

Web uygulaması örneklerinin durum bilgisi yoktur, bu nedenle başlatılan her yeni örnek, uygulama tarafından gerekli olan Yavama yapılandırmasını desteklemek için başlangıçta yapılandırılmalıdır.
Şu şekilde bir başlangıç Bash betiği yazabilirsiniz:

- Veri kaynaklarını ayarlama
- Mesajlaşma sağlayıcılarını yapılandırma
- Sunucu yapılandırmasına diğer modüller ve bağımlılıklar ekleyin.

Betik çalışır duruma geldiğinde ve uygulama başlamadan önce çalışır. Betik, uygulama sunucusunu sunucu başladıktan sonra gereken herhangi bir yapılandırma veya değişiklik ile yapılandırmak için */opt/JBoss/WildFly/bin/JBoss-cli.sh* adresinden çağrılan [jpatron CLI](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface) 'yi kullanmalıdır.

Yavayı yapılandırmak için CLı etkileşimli modunu kullanmayın. Bunun yerine, Jpatron CLı için `--file` komutunu kullanarak komutların bir komut dosyası sağlayabilirsiniz, örneğin:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Başlangıç betiğini */Home/site/Deployments/Tools*gibi, */Home* dizininiz altındaki App Service örneğinizdeki BIR konuma yüklemek için FTP 'yi kullanın. Daha fazla bilgi için bkz. [FTP/S kullanarak Azure App Service uygulamanızı dağıtma](https://docs.microsoft.com/azure/app-service/deploy-ftp).

Azure portal **Başlangıç betiği** alanını başlangıç kabuğu betiğinizin konumuna ayarlayın, örneğin */Home/site/Deployments/Tools/Your-Startup-Script.exe*.

Komut dosyasında kullanmak üzere ortam değişkenlerini geçirmek için uygulama yapılandırmasında uygulama [ayarları](../configure-common.md?toc=/azure/app-service/containers/toc.json#configure-app-settings) sağlayın. Uygulama ayarları, uygulamanızı sürüm denetiminden yapılandırmak için gereken bağlantı dizelerini ve diğer gizli dizileri saklar.

### <a name="install-modules-and-dependencies"></a>Modülleri ve bağımlılıkları yükler

Jpatron CLı aracılığıyla modülleri ve bunların bağımlılıklarını ve Yavaları bir dosya yoluna eklemek için, aşağıdaki dosyaları kendi dizinleriyle oluşturmanız gerekecektir. Bazı modüller ve bağımlılıklar, JNDı adlandırması veya diğer API 'ye özgü yapılandırma gibi ek yapılandırmalar gerektirebilir, bu nedenle çoğu durumda bir bağımlılığı yapılandırmak için ihtiyacınız olan en az bir küme budur.

- [XML modül tanımlayıcısı](https://jboss-modules.github.io/jboss-modules/manual/#descriptors). Bu XML dosyası modülünüzün adını, özniteliklerini ve bağımlılıklarını tanımlar. Bu [örnek Module. xml dosyası](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/administration_and_configuration_guide/example_postgresql_xa_datasource) bir Postgres modülünü, jar dosya JDBC bağımlılığını ve diğer modül bağımlılıklarını tanımlar.
- Modülünüzün gerekli olan tüm JAR Dosya bağımlılıkları.
- Yeni modülü yapılandırmak için Jpatron CLı komutlarınız ile bir betik. Bu dosya, sunucuyu bağımlılığı kullanacak şekilde yapılandırmak için Jpatron CLı tarafından yürütülecek komutlarınızı içerecektir. Modüller, veri kaynakları ve mesajlaşma sağlayıcıları ekleme komutları hakkındaki belgeler için [Bu belgeye](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli)bakın.
- Jpatron CLı 'yı çağırmak ve önceki adımda betiği yürütmek için bir bash başlangıç betiği. Bu dosya, App Service örneğiniz yeniden başlatıldığında veya bir genişleme sırasında yeni örnekler sağlandığında yürütülür. Bu başlangıç betiği, Jpatron komutlarına jpatron CLı 'ye geçirildiği sürece uygulamanız için başka bir yapılandırma gerçekleştirebileceğiniz yerdir. En azından, bu dosya Jpatron CLI komut betiğinizi Jpatron CLı 'ya geçirmek için tek bir komut olabilir:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Modülünüzün dosyalarını ve içeriğini aldıktan sonra, modülü, Yavaya uygulama sunucusuna eklemek için aşağıdaki adımları izleyin.

1. Dosyalarınızı, */Home/site/Deployments/Tools*gibi bir App Service örneğindeki bir konuma yüklemek için FTP 'yi kullanın. Daha fazla bilgi için bkz. [FTP/S kullanarak Azure App Service uygulamanızı dağıtma](../deploy-ftp.md).
2. Azure portal 'nin **yapılandırma** > **Genel ayarlar** sayfasında, **Başlangıç betiği** alanını başlangıç kabuğu betiğinizin konumuna ayarlayın, örneğin */Home/site/Deployments/Tools/Startup.exe*.
3. Portalın **genel bakış** bölümündeki veya Azure CLI kullanarak **yeniden başlat** düğmesine basarak App Service örneğinizi yeniden başlatın.

### <a name="configure-data-sources"></a>Veri kaynaklarını yapılandırma

Bir veri kaynağına erişmek için bir yata/Jpatron yapılandırmak üzere, yukarıda belirtilen genel işlemi "modülleri ve bağımlılıkları yüklemek" bölümünde kullanın. Aşağıdaki bölümde PostgreSQL, MySQL ve SQL Server veri kaynakları için bu işlemle ilgili belirli ayrıntılar verilmektedir.

Bu bölümde, zaten bir uygulamanız, bir App Service örneği ve bir Azure veritabanı hizmet örneği olduğunuz varsayılmaktadır. Aşağıdaki yönergeler App Service adınızı, kaynak grubunu ve veritabanı bağlantı bilgilerinizi ifade eder. Bu bilgileri Azure portal bulabilirsiniz.

Örnek uygulama kullanarak başlangıçtan itibaren işlemin tamamına geçmeyi tercih ediyorsanız, bkz. [öğretici: Azure 'Da Java EE ve Postgres Web uygulaması oluşturma](tutorial-java-enterprise-postgresql-app.md).

Aşağıdaki adımlarda, mevcut App Service ve veritabanınızı bağlama gereksinimleri açıklanmaktadır.

1. [PostgreSQL](https://jdbc.postgresql.org/download.html), [MySQL](https://dev.mysql.com/downloads/connector/j/)veya [SQL Server](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server)için JDBC sürücüsünü indirin. Sürücü. jar dosyasını almak için indirilen arşivin paketini açın.

2. *Module. xml* gibi bir ada sahip bir dosya oluşturun ve aşağıdaki biçimlendirmeyi ekleyin. `<module name>` yer tutucusunu (açılı ayraçlar dahil) PostgreSQL için `org.postgres`, MySQL için `com.mysql` veya SQL Server için `com.microsoft` değiştirin. `<JDBC .jar file path>`, önceki adımdaki. jar dosyasının adı ile değiştirin. Bu dosyanın tam yolu da dahil olmak üzere, dosyanın App Service örneğinize yerleştirebilirsiniz. Bu, */Home* dizini altında herhangi bir konum olabilir.

    ```xml
    <?xml version="1.0" ?>
    <module xmlns="urn:jboss:module:1.1" name="<module name>">
        <resources>
           <resource-root path="<JDBC .jar file path>" />
        </resources>
        <dependencies>
            <module name="javax.api"/>
            <module name="javax.transaction.api"/>
        </dependencies>
    </module>
    ```

3. *DataSource-Commands. CLI* gibi bir ada sahip bir dosya oluşturun ve aşağıdaki kodu ekleyin. `<JDBC .jar file path>`, önceki adımda kullandığınız değerle değiştirin. `<module file path>`, önceki adımdaki dosya adı ve App Service yoluyla değiştirin (örneğin, */Home/Module.xml*).

    **PostgreSQL**

    ```console
    module add --name=org.postgres --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=postgres:add(driver-name=postgres,driver-module-name=org.postgres,driver-class-name=org.postgresql.Driver,driver-xa-datasource-class-name=org.postgresql.xa.PGXADataSource)

    data-source add --name=postgresDS --driver-name=postgres --jndi-name=java:jboss/datasources/postgresDS --connection-url=$DATABASE_CONNECTION_URL --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=org.postgresql.Driver --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLExceptionSorter --jta=true --use-java-context=true --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLValidConnectionChecker

    reload --use-current-server-config=true
    ```

    **MySQL**

    ```console
    module add --name=com.mysql --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=mysql:add(driver-name=mysql,driver-module-name=com.mysql,driver-class-name=com.mysql.cj.jdbc.Driver)

    data-source add --name=mysqlDS --jndi-name=java:jboss/datasources/mysqlDS --connection-url=$DATABASE_CONNECTION_URL --driver-name=mysql --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=com.mysql.cj.jdbc.Driver --jta=true --use-java-context=true --exception-sorter-class-name=com.mysql.cj.jdbc.integration.jboss.ExtendedMysqlExceptionSorter

    reload --use-current-server-config=true
    ```

    **SQL Server**

    ```console
    module add --name=com.microsoft --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=sqlserver:add(driver-name=sqlserver,driver-module-name=com.microsoft,driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver,driver-datasource-class-name=com.microsoft.sqlserver.jdbc.SQLServerDataSource)

    data-source add --name=sqlDS --jndi-name=java:jboss/datasources/sqlDS --driver-name=sqlserver --connection-url=$DATABASE_CONNECTION_URL --validate-on-match=true --background-validation=false --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLValidConnectionChecker --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLExceptionSorter

    reload --use-current-server-config=true
    ```

    Bu dosya, bir sonraki adımda açıklanan başlangıç betiği tarafından çalıştırılır. Bu, bir Yavama modülü olarak JDBC sürücüsünü yükler, karşılık gelen bir veri kaynağını oluşturur ve değişikliklerin etkili olabilmesi için sunucuyu yeniden yükler.

4. *Startup.sh* gibi bir ada sahip bir dosya oluşturun ve aşağıdaki kodu ekleyin. `<JBoss CLI script>`, önceki adımda oluşturduğunuz dosyanın adıyla değiştirin. Dosyayı App Service Örneğinizde yerleştireceğiniz konuma tam yolu eklediğinizden emin olun. Örneğin, */Home/DataSource-Commands.exe*.

    ```bash
    #!/usr/bin/env bash
    /opt/jboss/wildfly/bin/jboss-cli.sh -c --file=<JBoss CLI script>
    ```

5. JDBC. jar dosyasını, modül XML dosyasını, Jpatron CLı betiğini ve başlangıç betiğini App Service örneğinize yüklemek için FTP 'yi kullanın. Bu dosyaları, önceki adımlarda belirttiğiniz konuma (örneğin, */Home*) koyun. FTP hakkında daha fazla bilgi için bkz. [FTP/S kullanarak Azure App Service uygulamanızı dağıtma](https://docs.microsoft.com/azure/app-service/deploy-ftp).

6. Veritabanı bağlantı bilgilerinizi tutan App Service ayarları eklemek için Azure CLı 'yi kullanın. `<resource group>` ve `<webapp name>` App Service kullandığı değerlerle değiştirin. `<database server name>`, `<database name>`, `<admin name>`ve `<admin password>` yerine veritabanı bağlantı bilgilerinizi koyun. App Service ve veritabanı bilgilerinizi Azure portal alabilirsiniz.

    **PostgreSQL**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:postgresql://<database server name>:5432/<database name>?ssl=true \
            DATABASE_SERVER_ADMIN_FULL_NAME=<admin name> \
            DATABASE_SERVER_ADMIN_PASSWORD=<admin password>
    ```

    **MySQL**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT \
            DATABASE_SERVER_ADMIN_FULL_NAME=<admin name> \
            DATABASE_SERVER_ADMIN_PASSWORD=<admin password>
    ```

    **SQL Server:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
    ```

    DATABASE_CONNECTION_URL değerler her veritabanı sunucusu için farklıdır ve Azure portal değerlerden farklıdır. Burada gösterilen (ve kod parçacıklarında) URL biçimleri, şu şekilde kullanım için gereklidir:

    * **PostgreSQL:** `jdbc:postgresql://<database server name>:5432/<database name>?ssl=true`
    * **MySQL:** `jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT`
    * **SQL Server:** `jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;`

7. Azure portal App Service gidin ve **yapılandırma** > **Genel ayarlar** sayfasını bulun. **Başlangıç betiği** alanını başlangıç betiğinizin adı ve konumuna (örneğin, */Home/Startup.exe*) ayarlayın.

App Service bir sonraki sefer yeniden başlatıldığında, başlangıç betiğini çalıştırır ve gerekli yapılandırma adımlarını gerçekleştirir. Bu yapılandırmanın doğru şekilde gerçekleştiğinin test olması için SSH kullanarak App Service erişebilir ve sonra başlangıç betiğini Bash isteminden kendiniz çalıştırabilirsiniz. App Service günlüklerini de inceleyebilirsiniz. Bu seçenekler hakkında daha fazla bilgi için bkz. [uygulamaları günlüğe kaydetme ve hata ayıklama](#logging-and-debugging-apps).

Daha sonra, uygulamanızın Yavaya yapılandırmasını güncelleştirmeniz ve yeniden dağıtmanız gerekir. Aşağıdaki adımları kullanın:

1. Uygulamanız için *src/Main/Resources/meta-INF/kalıcılık. xml* dosyasını açın ve `<jta-data-source>` öğesini bulun. İçeriğini burada gösterildiği gibi değiştirin:

    **PostgreSQL**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

    **MySQL**

    ```xml
    <jta-data-source>java:jboss/datasources/mysqlDS</jta-data-source>
    ```

    **SQL Server**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

2. Bash isteminde aşağıdaki komutu kullanarak uygulamanızı yeniden derleyin ve yeniden dağıtın:

    ```bash
    mvn package -DskipTests azure-webapp:deploy
    ```

3. Azure portal **genel bakış** bölümündeki veya Azure clı kullanarak App Service örneğinizi **yeniden başlatın.**

App Service örneğiniz artık veritabanınıza erişecek şekilde yapılandırılmıştır.

Veritabanı bağlantısını, yavalar ile yapılandırma hakkında daha fazla bilgi için bkz. [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7), [MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource)veya [SQL Server](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898).

### <a name="use-service-bus-as-a-message-broker"></a>İleti Aracısı olarak Service Bus kullanma

, Bir ileti Aracısı olarak [Azure Service Bus](/azure/service-bus-messaging) kullanmak için, yavaların ve ileti odaklı beden yapılandırabilirsiniz. Yapılandırma sonrasında, Java Ileti hizmeti (JMS) istemciniz olarak [Apache Qpid](https://qpid.apache.org) kullanarak ileti gönderebilir ve alabilirsiniz. Bir JMS kaynak bağdaştırıcısı (JMS RA) yapılandırmak için birkaç adım vardır. Bu, Kurumsal Java Beans (EJBs) ile uzak bir JMS bağlantı fabrikası ve kuyruğu yapılandırır. Bu uzaktan kurulum Azure Service Bus işaret eder ve AMQP protokolü için Apache Qpid JMS sağlayıcısını kullanır.

Aşağıdaki adımlarda gereken yapılandırma ve kod açıklanır. Bu adımlarda, çekirdeklere, bir Service Bus ad alanı, bir kuyruk ve abonelikle ilgili bir konuyla ilgili App Service bir örnek oluşturmuş olduğunuz varsayılır. Bu kaynakları oluşturma hakkında daha fazla bilgi için bkz.:

- [Hızlı başlangıç: Linux üzerinde Azure App Service Java uygulaması oluşturma](/azure/app-service/containers/quickstart-java)
- [Hızlı başlangıç: Azure CLı kullanarak Service Bus kuyruğu oluşturma](/azure/service-bus-messaging/service-bus-quickstart-cli)
- [Hızlı başlangıç: konuya bir Service Bus konu ve abonelik oluşturmak için Azure portal kullanın](/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal)

1. Bir bash terminali açın ve Azure Kaynak bilgilerinizi ortam değişkenlerine kaydetmek için aşağıdaki komutları kullanın. Yer tutucuları (açılı ayraçlar dahil) belirtilen değerlerle değiştirin.

    | Değişken            | Değer                                                                      |
    |---------------------|----------------------------------------------------------------------------|
    | RESOURCEGROUP_NAME  | App Service örneğinizi içeren kaynak grubunun adı.       |
    | WEBAPP_NAME         | App Service örneğinizin adı.                                     |
    | BÖLGE              | Uygulamanızın barındırıldığı bölgenin adı.                           |
    | DEFAULT_SBNAMESPACE | Service Bus ad alanının adı.                                    |
    | SB_SAS_POLICY       | Ad alanınız için paylaşılan erişim imzası (SAS) ilkesinin adı.   |
    | SB_SAS_KEY          | Kuyruğunuzun SAS ilkesi için birincil veya ikincil anahtar.                  |
    | SB_QUEUE            | Service Bus kuyruğunun adı.                                        |
    | SB_TOPIC            | Service Bus konusunun adı.                                        |
    | SB_SUBSCRIPTION     | Konunun abonelik adı.                                |

    ```bash
    RESOURCEGROUP_NAME=<resource group>
    WEBAPP_NAME=<web app>
    WEBAPP_PLAN_NAME=${WEBAPP_NAME}-appservice-plan
    REGION=<region>
    DEFAULT_SBNAMESPACE=<namespace>
    SB_SAS_POLICY=<SAS policy>
    SB_SAS_KEY=<SAS key>
    SB_QUEUE=<queue>
    SB_TOPIC=<topic>
    SB_SUBSCRIPTION=<subscription>
    PROVIDER_URL=amqps://${DEFAULT_SBNAMESPACE}.servicebus.windows.net?amqp.idleTimeout=120000
    ```

    Bu bilgileri Azure portal bulabilirsiniz. SAS ilkesi ve anahtarı için, uygulamanızın hem kuyruğunuza hem de konu aboneliğinize erişebilmesi için ad alanının değerlerini kullandığınızdan emin olun. Bu değerleri Azure portal bulmak için, ad alanı kaynağınız ' ne gidin, **paylaşılan erişim ilkeleri**' ni seçin ve ardından **RootManageSharedAccessKey** ilkesini seçin.

2. [Apache Qpid JMS sağlayıcısını](https://qpid.apache.org/components/jms/index.html)indirin. *LIB* ve *lib/optional* dizinlerindeki. jar dosyalarını bulun.

3. *Module. xml* adlı bir dosya oluşturun ve aşağıdaki biçimlendirmeyi ekleyin. `<version>` yer tutucunun her örneğini (açılı ayraçlar dahil) her. jar dosyası için doğru sürüm ile değiştirin; böylece dosya adları, 1. Adımda ayıkladığınız dosyalarla eşleşir.

    ```xml
    <module xmlns="urn:jboss:module:1.1" name="org.jboss.genericjms.provider">
        <resources>
            <resource-root path="proton-j-<version>.jar"/>
            <resource-root path="qpid-jms-client-<version>.jar"/>
            <resource-root path="slf4j-log4j12-<version>.jar"/>
            <resource-root path="slf4j-api-<version>.jar"/>
            <resource-root path="log4j-<version>.jar"/>
            <resource-root path="netty-buffer-<version>.jar" />
            <resource-root path="netty-codec-<version>.jar" />
            <resource-root path="netty-codec-http-<version>.jar" />
            <resource-root path="netty-common-<version>.jar" />
            <resource-root path="netty-handler-<version>.jar" />
            <resource-root path="netty-resolver-<version>.jar" />
            <resource-root path="netty-transport-<version>.jar" />
            <resource-root path="netty-transport-native-epoll-<version>-linux-x86_64.jar" />
            <resource-root path="netty-transport-native-kqueue-<version>-osx-x86_64.jar" />
            <resource-root path="netty-transport-native-unix-common-<version>.jar" />
            <resource-root path="qpid-jms-discovery-<version>jar" />
        </resources>
        <dependencies>
            <module name="javax.api"/>
            <module name="javax.jms.api"/>
        </dependencies>
    </module>
    ```

4. *Startup.sh* adlı bir dosya oluşturun ve aşağıdaki kodu ekleyin.

    ```bash
    echo "Generating jndi.properties file in /home/site/deployments/tools directory"
    echo "connectionfactory.mymdbconnection=amqps://${DEFAULT_SBNAMESPACE}.servicebus.windows.net?amqp.idleTimeout=120000&jms.username=${SB_SAS_POLICY}&jms.password=${SB_SAS_KEY}" > /home/site/deployments/tools/jndi.properties
    echo "queue.mymdbqueue=${SB_QUEUE}" >> /home/site/deployments/tools/jndi.properties
    echo "topic.mymdbtopic=${SB_TOPIC}" >> /home/site/deployments/tools/jndi.properties
    echo "queue.mymdbsubscription=${SB_TOPIC}/Subscriptions/${SB_SUBSCRIPTION}" >> /home/site/deployments/tools/jndi.properties
    echo "====== contents of /home/site/deployments/tools/jndi.properties ======"
    cat /home/site/deployments/tools/jndi.properties
    echo "====== EOF /home/site/deployments/tools/jndi.properties ======"
    echo "Generating commands.cli file for /home/site/deployments/tools directory"
    echo "# Start batching commands" > /home/site/deployments/tools/commands.cli
    echo "batch" >> /home/site/deployments/tools/commands.cli
    echo "# Configure the ee subsystem to enable MDB annotation property substitution" >> /home/site/deployments/tools/commands.cli
    echo "/subsystem=ee:write-attribute(name=annotation-property-replacement,value=true)" >> /home/site/deployments/tools/commands.cli
    echo "# Define system properties to be used in the substititution" >> /home/site/deployments/tools/commands.cli
    echo "/system-property=property.mymdb.queue:add(value=java:global/remoteJMS/mymdbqueue})" >> /home/site/deployments/tools/commands.cli
    echo "/system-property=property.mymdb.topic:add(value=java:global/remoteJMS/mymdbsubscription)" >> /home/site/deployments/tools/commands.cli
    echo "/system-property=property.connection.factory:add(value=java:global/remoteJMS/mymdbconnection)" >> /home/site/deployments/tools/commands.cli
    echo "/subsystem=ee:list-add(name=global-modules, value={\"name\" => \"org.jboss.genericjms.provider\", \"slot\" =>\"main\"}" >> /home/site/deployments/tools/commands.cli
    echo "/subsystem=naming/binding=\"java:global/remoteJMS\":add(binding-type=external-context,module=org.jboss.genericjms.provider,class=javax.naming.InitialContext,environment=[java.naming.factory.initial=org.apache.qpid.jms.jndi.JmsInitialContextFactory,org.jboss.as.naming.lookup.by.string=true,java.naming.provider.url=/home/site/deployments/tools/jndi.properties])" >> /home/site/deployments/tools/commands.cli
    echo "/subsystem=resource-adapters/resource-adapter=generic-ra:add(module=org.jboss.genericjms,transaction-support=XATransaction)" >> /home/site/deployments/tools/commands.cli
    echo "/subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd:add(class-name=org.jboss.resource.adapter.jms.JmsManagedConnectionFactory, jndi-name=java:/jms/mymdbconnection)" >> /home/site/deployments/tools/commands.cli
    echo "/subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd/config-properties=ConnectionFactory:add(value=mymdbconnection)" >> /home/site/deployments/tools/commands.cli
    echo "/subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd/config-properties=JndiParameters:add(value=\"java.naming.factory.initial=org.apache.qpid.jms.jndi.JmsInitialContextFactory;java.naming.provider.url=/home/site/deployments/tools/jndi.properties\")" >> /home/site/deployments/tools/commands.cli
    echo "/subsystem=resource-adapters/resource-adapter=generic-ra/connection-definitions=sbf-cd:write-attribute(name=security-application,value=true)" >> /home/site/deployments/tools/commands.cli
    echo "/subsystem=ejb3:write-attribute(name=default-resource-adapter-name, value=generic-ra)" >> /home/site/deployments/tools/commands.cli
    echo "# Run the batch commands" >> /home/site/deployments/tools/commands.cli
    echo "run-batch" >> /home/site/deployments/tools/commands.cli
    echo "reload" >> /home/site/deployments/tools/commands.cli
    echo "====== contents of /home/site/deployments/tools/commands.cli ======"
    cat /home/site/deployments/tools/commands.cli
    echo "======= EOF /home/site/deployments/tools/commands.cli ========"
    mkdir /opt/jboss/wildfly/modules/system/layers/base/org/jboss/genericjms/provider
    mkdir /opt/jboss/wildfly/modules/system/layers/base/org/jboss/genericjms/provider/main
    cp  /home/site/deployments/tools/*.jar /opt/jboss/wildfly/modules/system/layers/base/org/jboss/genericjms/provider/main/
    cp /home/site/deployments/tools/module.xml /opt/jboss/wildfly/modules/system/layers/base/org/jboss/genericjms/provider/main/
    cp /home/site/deployments/tools/jndi.properties /opt/jboss/wildfly/standalone/configuration/
    /opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/home/site/deployments/tools/commands.cli
    echo "Startup Run done"
    ```

    App Service örneğiniz her başlatıldığında bu betiği çalıştırır ve bu kod, Yavalara gereken ek yapılandırma sağlar. Bu betik, uygulamanızın bağımlılıklarını gereken konumlara kopyalar. Ayrıca, adım 1 ' de gösterilen ortam değişkenlerini kullanan *jndi. Properties* ve *Commands. cli* dosyalarını oluşturur. Bu değerler, daha sonraki bir adımda App Service örneğinize de geçirilir.

    *Commands. cli* dosyası, başlangıç betiği tarafından başlatılan bir [YAVAMA CLI](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface) betiğine sahiptir. Bu dosyadaki komutlar JMS ve JNDI 'yi yapılandırarak *JNDI. Properties* dosyasını kullanır. Bu komutlar, uygulamanız ile Service Bus kuyruğu veya konusu arasında bir bağlantı oluşturur.

5. . Jar dosyalarını, *Module. xml* dosyasını ve *Startup.sh* dosyasını App SERVICE örneğine yüklemek için FTP 'yi kullanın. *Startup.sh* 'i */Home* dizinine koyun ve diğer dosyaları */Home/site/Deployments/Tools* dizinine yerleştirin. Bağımlılıkların geçişli olarak kapatılmasını sağlamak için *Module. xml* dosyasında listelenen her. jar dosyasını karşıya yüklediğinizden emin olun. FTP hakkında daha fazla bilgi için bkz. [FTP/S kullanarak Azure App Service uygulamanızı dağıtma](https://docs.microsoft.com/azure/app-service/deploy-ftp).

6. Aşağıdaki `import` deyimlerini eklemek için MessageListener uygulamanızı güncelleştirin:

    ```java
    import javax.ejb.ActivationConfigProperty;
    import javax.ejb.MessageDriven;
    import javax.ejb.TransactionAttribute;
    import javax.ejb.TransactionAttributeType;
    import javax.ejb.TransactionManagement;
    import javax.ejb.TransactionManagementType;
    import javax.jms.JMSException;
    import javax.jms.Message;
    import javax.jms.MessageListener;
    import javax.jms.TextMessage;
    ```

7. Ardından, aşağıdaki örnekle eşleşecek şekilde dinleyici sınıfı ek açıklamalarınızı güncelleştirin. Bu sınıf, ileti alındığını günlüğe kaydeden örnek bir uygulama sağlar.

    ```java
    @TransactionManagement(TransactionManagementType.BEAN)
    @TransactionAttribute(TransactionAttributeType.NOT_SUPPORTED)
    @MessageDriven(name = "MyQueueListener", activationConfig = {
            @ActivationConfigProperty(propertyName = "connectionFactory", propertyValue = "${property.connection.factory}"),
            @ActivationConfigProperty(propertyName = "destinationLookup", propertyValue = "${property.mymdb.queue}"),
            @ActivationConfigProperty(propertyName = "destinationType", propertyValue = "javax.jms.Queue"),
            @ActivationConfigProperty(propertyName = "acknowledgeMode", propertyValue = "Auto-acknowledge") })
    public class MyQueueListener implements MessageListener {

        private static final Logger LOGGER = Logger.getLogger(TopicListener.class.toString());

        public void onMessage(Message rcvMessage) {
            TextMessage msg = null;
            try {
                if (rcvMessage instanceof TextMessage) {
                    msg = (TextMessage) rcvMessage;
                    LOGGER.info("Received Message from topic: " + msg.getText());
                } else {
                    LOGGER.warning("Message of wrong type: " + rcvMessage.getClass().getName());
                }
            } catch (JMSException e) {
                LOGGER.warning("Exception on message : " + e.getMessage());
                throw new RuntimeException(e);
            }
        }
    }
    ```

    `connectionFactory` ve `destinationLookup` değerleri, *Startup.sh* betiği tarafından yapılandırılan yavaya ve sistem özelliği değerlerini ifade eder. `destinationType` değeri, bir Service Bus Queue örneğine bağlanmakta olduğunuzu belirten `javax.jms.Queue`. Bu değer, aşağıda gösterildiği gibi bir Service Bus konusuna bağlandığınızda `javax.jms.Topic` olmalıdır:

    ```java
    @TransactionManagement(TransactionManagementType.BEAN)
    @TransactionAttribute(TransactionAttributeType.NOT_SUPPORTED)
    @MessageDriven(name = "MyTopicListener", activationConfig = {
            @ActivationConfigProperty(propertyName = "connectionFactory", propertyValue = "${property.connection.factory}"),
            @ActivationConfigProperty(propertyName = "destinationLookup", propertyValue = "${property.mymdb.topic}"),
            @ActivationConfigProperty(propertyName = "destinationType", propertyValue = "javax.jms.Topic"),
            @ActivationConfigProperty(propertyName = "acknowledgeMode", propertyValue = "Auto-acknowledge") })
        public class MyTopicListener implements MessageListener {
        // ...
    }
    ```

8. Aşağıdaki bağımlılıkları eklemek için *pom. xml* dosyanızın `dependencies` bölümünü güncelleştirin:

    ```xml
    <dependencies>
        <dependency>
            <groupId>org.apache.qpid</groupId>
            <artifactId>qpid-jms-client</artifactId>
            <version>0.40.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.qpid</groupId>
            <artifactId>proton-j</artifactId>
            <version>0.31.0</version>
        </dependency>
        <dependency>
            <groupId>javax.enterprise</groupId>
            <artifactId>cdi-api</artifactId>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.jboss.spec.javax.ejb</groupId>
            <artifactId>jboss-ejb-api_3.2_spec</artifactId>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.jboss.spec.javax.jms</groupId>
            <artifactId>jboss-jms-api_2.0_spec</artifactId>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.jboss.spec.javax.servlet</groupId>
            <artifactId>jboss-servlet-api_4.0_spec</artifactId>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.jboss.spec.javax.annotation</groupId>
            <artifactId>jboss-annotations-api_1.3_spec</artifactId>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    ```

9. *Poe. xml* dosyanızdaki `azure-webapp-maven-plugin` yapılandırmasını, Service Bus hesabı bilgilerinize başvuracak şekilde güncelleştirin. Gerekirse, [Azure App Service Için Maven eklentisinin](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)güncel sürümüne `1.7.0` değiştirin.

    ```xml
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.7.0</version>
        <configuration>

            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>

            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>wildfly 14-jre8</linuxRuntime>

            <appSettings>
                <property>
                    <name>DEFAULT_SBNAMESPACE</name>
                    <value>${DEFAULT_SBNAMESPACE}</value>
                </property>
                <property>
                    <name>SB_SAS_POLICY</name>
                    <value>${SB_SAS_POLICY}</value>
                </property>
                <property>
                    <name>SB_SAS_KEY</name>
                    <value>${SB_SAS_KEY}</value>
                </property>
                <property>
                    <name>PROVIDER_URL</name>
                    <value>${PROVIDER_URL}</value>
                </property>
                <property>
                    <name>SB_QUEUE</name>
                    <value>${SB_QUEUE}</value>
                </property>
                <property>
                    <name>SB_TOPIC</name>
                    <value>${SB_TOPIC}</value>
                </property>
                <property>
                    <name>SB_SUBSCRIPTION</name>
                    <value>${SB_SUBSCRIPTION}</value>
                </property>
            </appSettings>
        </configuration>
    </plugin>
    ```

    Bu ayarlar, App Service örneğinizi yerel olarak ayarlamış olduğunuz ortam değişkenlerine sahip olacak şekilde yapılandırır. Hesap bilgilerinizin kaynak dosyalarınıza ait olmasını sağlamak için ortam değişkenlerini kullanır.

10. Uygulamanızı yeniden derleyin ve dağıtın.

    ```bash
    mvn package -DskipTests azure-webapp:deploy
    ```

İleti temelli çekirdeklere artık ileti mekanizması olarak Service Bus kullanacak şekilde yapılandırılmıştır.

App Service bir sonraki sefer yeniden başlatıldığında, başlangıç betiğini çalıştırır ve gerekli yapılandırma adımlarını gerçekleştirir. Bu yapılandırmanın doğru şekilde gerçekleştiğinin test olması için SSH kullanarak App Service erişebilir ve sonra başlangıç betiğini Bash isteminden kendiniz çalıştırabilirsiniz. App Service günlüklerini de inceleyebilirsiniz. Bu seçenekler hakkında daha fazla bilgi için bkz. [uygulamaları günlüğe kaydetme ve hata ayıklama](#logging-and-debugging-apps).

Bu yönergeleri test etmek için kullanabileceğiniz bir örnek için GitHub 'da [Migrate-Java-EE-App-to-Azure-2](https://github.com/Azure-Samples/migrate-java-ee-app-to-azure-2) deposuna bakın ve `helloworld-mdb-propertysubstitution` örneğe bakın.

## <a name="use-redis-as-a-session-cache-with-tomcat"></a>Redin 'i Tomcat ile oturum önbelleği olarak kullanma

Tomcat 'i [redsıs Için Azure önbelleği](/azure/azure-cache-for-redis/)gibi bir dış oturum deposu kullanacak şekilde yapılandırabilirsiniz. Bu, bir kullanıcı uygulamanızın başka bir örneğine aktarıldığında (örneğin, otomatik ölçeklendirme, yeniden başlatma veya yük devretme gerçekleştiğinde) Kullanıcı oturumu durumunu korumanıza olanak sağlar.

Tomcat 'i Redwith ile kullanmak için uygulamanızı bir [Persistentmanager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html) uygulaması kullanacak şekilde yapılandırmanız gerekir. Aşağıdaki adımlarda bu işlem Özet [oturum Yöneticisi kullanılarak açıklanmıştır: redin-Store](https://github.com/pivotalsoftware/session-managers/tree/master/redis-store) örnek olarak.

1. Bir bash terminali açın ve aşağıdaki ortam değişkenlerinin her birini ayarlamak için `<variable>=<value>` kullanın.

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

2. Aşağıdaki içerikle uygulamanızın *src/Main/WebApp/meta-INF/Context. xml* dosyasını oluşturun veya güncelleştirin:

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

8. Uygulamanızın *Pod. xml* dosyasındaki `azure-webapp-maven-plugin` yapılandırmasını, redsıs hesap bilgilerinize başvuracak şekilde güncelleştirin. Bu dosya, daha önce ayarladığınız ortam değişkenlerini kullanarak hesap bilgilerinizin kaynak dosyalarınıza ait olmasını sağlar.

    Gerekirse, [Azure App Service Için Maven eklentisinin](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)güncel sürümüne `1.7.0` değiştirin.

    ```xml
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.7.0</version>
        <configuration>

            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appServicePlanName>${WEBAPP_PLAN_NAME}-${REGION}</appServicePlanName>
            <appName>${WEBAPP_NAME}-${REGION}</appName>
            <region>${REGION}</region>
            <linuxRuntime>tomcat 9.0-jre8</linuxRuntime>

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

Kapsayıcılarınızdaki Azure tarafından desteklenen Zulu dili JDK 'yi kullanmak için, önceden oluşturulmuş görüntüleri [Azure için desteklenen Azul Zulu Kurumsal indirme sayfasından](https://www.azul.com/downloads/azure-only/zulu/) belgelendiğinden veya [Microsoft Java GitHub deposundan](https://github.com/Microsoft/java/tree/master/docker)`Dockerfile` örnekleri kullandığınızdan emin olun.

## <a name="statement-of-support"></a>Destek beyanı

### <a name="runtime-availability"></a>Çalışma zamanı kullanılabilirliği

Linux için App Service, Java Web uygulamalarının yönetilen barındırılmasına yönelik iki çalışma zamanını destekler:

- Web arşivi (WAR) dosyaları olarak paketlenmiş uygulamalar çalıştırmak için [Tomcat servlet kapsayıcısı](https://tomcat.apache.org/) . Desteklenen sürümler 8,5 ve 9,0 ' dir.
- Java Arşivi (JAR) dosyaları olarak paketlenmiş uygulamalar çalıştırmak için Java çalışma zamanı ortamı. Desteklenen sürümler Java 8 ve 11 ' dir.

### <a name="jdk-versions-and-maintenance"></a>JDK sürümleri ve bakım

Azul Zulu kurumsal Derlemeleriyle OpenJDK, Azure için OpenJDK ve Microsoft ve Azul sistemleri tarafından desteklenen Azure Stack için ücretsiz, çok platformlu, üretime hazırlı bir dağıtımıdır. Java ve uygulamaları oluşturmak ve çalıştırmak için tüm bileşenleri içerir. JDK 'yi [Java JDK yüklemesinden](https://aka.ms/azure-jdks)yükleyebilirsiniz.

Desteklenen JDKs, her yıl Ocak, Nisan, Temmuz ve Ekim ayında otomatik olarak üç ayda bir düzeltme eki uygulanır.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Önemli güvenlik açıklarına yönelik düzeltme ekleri ve düzeltmeler Azul sistemlerinden kullanılabilir hale geldiğinde serbest bırakılır. "Ana" güvenlik açığı, [NIST ortak güvenlik açığı Puanlama sistemi, sürüm 2](https://nvd.nist.gov/cvss.cfm)üzerinde 9,0 veya üzeri bir taban puanı tarafından tanımlanır.

### <a name="deprecation-and-retirement"></a>Kullanımdan kaldırma ve kullanımdan kaldırma

Desteklenen bir Java çalışma zamanı devre dışı bırakırsa, etkilenen çalışma zamanını kullanan Azure geliştiricileri, çalışma zamanı kullanımdan kalkmadan önce en az altı ayda bir kullanımdan kaldırma olarak verilmeyecektir.

## <a name="next-steps"></a>Sonraki adımlar

Azure hızlı başlangıç, öğreticiler ve Java başvuru belgelerini bulmak için [Java geliştiricileri Için Azure](/java/azure/) 'u ziyaret edin.

Java geliştirmeye özgü olmayan Linux için App Service kullanma hakkında genel sorular, [App Service LINUX SSS](app-service-linux-faq.md)bölümünde yanıtlanmıştır.
