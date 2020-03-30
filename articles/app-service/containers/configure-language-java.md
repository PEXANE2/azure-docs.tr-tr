---
title: Linux Java uygulamalarını yapılandırma
description: Uygulamanız için önceden oluşturulmuş bir Java kapsayıcısını nasıl yapılandıracaklarınızı öğrenin. Bu makalede, en yaygın yapılandırma görevleri gösterir.
keywords: azure uygulama hizmeti, web uygulaması, linux, oss, java, java ee, jee, javaee
author: bmitchell287
manager: barbkess
ms.devlang: java
ms.topic: article
ms.date: 11/22/2019
ms.author: brendm
ms.reviewer: cephalin
ms.custom: seodec18
ms.openlocfilehash: 970701606811cbd61a9bfebe39ff82cdc91d5693
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80245846"
---
# <a name="configure-a-linux-java-app-for-azure-app-service"></a>Azure App Service için Linux Java uygulaması yapılandırma

Linux'taki Azure Uygulama Hizmeti, Java geliştiricilerinin Tomcat veya Java Standard Edition (SE) paketlenmiş web uygulamalarını tam olarak yönetilen Linux tabanlı bir hizmette hızla oluşturmalarına, dağıtmalarına ve ölçeklendirmelerine olanak tanır. Uygulamaları Komut satırından veya IntelliJ, Eclipse veya Visual Studio Code gibi editörlerden Maven eklentileriyle dağıtın.

Bu kılavuz, Uygulama Hizmeti'nde yerleşik bir Linux kapsayıcısı kullanan Java geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure Uygulama Hizmeti'ni hiç kullanmadıysanız, [Java quickstart'ını](quickstart-java.md)izleyin.

## <a name="deploying-your-app"></a>Uygulamanızı dağıtma

[Azure Uygulama Hizmeti için Maven Eklentisini](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) hem .jar hem de .war dosyalarını dağıtmak için kullanabilirsiniz. Popüler IDE'lerle dağıtım, [IntelliJ için Azure Araç Seti](/java/azure/intellij/azure-toolkit-for-intellij) veya Eclipse için Azure Araç [Seti](/java/azure/eclipse/azure-toolkit-for-eclipse)ile de desteklenir.

Aksi takdirde, dağıtım yönteminiz arşiv türünüze bağlıdır:

- .war dosyalarını Tomcat'a `/api/wardeploy/` dağıtmak için arşiv dosyanızı göndermek için bitiş noktasını kullanın. Bu API hakkında daha fazla bilgi için lütfen [bu belgelere](https://docs.microsoft.com/azure/app-service/deploy-zip#deploy-war-file)bakın.
- Java SE görüntülerine .jar dosyalarını `/api/zipdeploy/` dağıtmak için Kudu sitesinin bitiş noktasını kullanın. Bu API hakkında daha fazla bilgi için lütfen [bu belgelere](https://docs.microsoft.com/azure/app-service/deploy-zip#rest)bakın.

FTP kullanarak .war veya .jar'ınızı dağıtmayın. FTP aracı başlangıç komut dosyalarını, bağımlılıkları veya diğer çalışma zamanı dosyalarını yüklemek için tasarlanmıştır. Bu, web uygulamalarını dağıtmak için en uygun seçim değildir.

## <a name="logging-and-debugging-apps"></a>Uygulamaları günlüğe kaydetme ve hata ayıklama

Azure portalı üzerinden her uygulama için performans raporları, trafik görselleştirmeleri ve sistem durumu denetimleri mevcuttur. Daha fazla bilgi için Azure [Uygulama Hizmeti tanılama](../overview-diagnostics.md)genel bakış bakın.

### <a name="ssh-console-access"></a>SSH konsol erişimi

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

### <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

Daha fazla bilgi için [Bulut Kabuğu'ndaki Akış günlüklerine](../troubleshoot-diagnostic-logs.md#in-cloud-shell)bakın.

### <a name="app-logging"></a>Uygulama günlüğü

Uygulamanızın standart konsol çıktısını ve standart konsol hata akışlarını yerel dosya sistemine veya Azure Blob Depolamasına yazmak üzere Uygulama Hizmetini yapılandırmak için Azure portalı veya [Azure CLI](/cli/azure/webapp/log#az-webapp-log-config) üzerinden [uygulama günlüğe kaydetmeyi](../troubleshoot-diagnostic-logs.md?toc=/azure/app-service/containers/toc.json#enable-application-logging-windows) etkinleştirin. Yerel Uygulama Hizmeti dosya sistemi örneğine günlüğe kaydetme, yapılandırıldıktan 12 saat sonra devre dışı bırakılır. Daha uzun bekletme gerekiyorsa, çıktıyı blob depolama kabına yazmak için uygulamayı yapılandırın. Java ve Tomcat uygulama günlükleriniz */home/LogFiles/Application/* dizininde bulunabilir.

Uygulamanız izleme için [Logback](https://logback.qos.ch/) veya [Log4j](https://logging.apache.org/log4j) kullanıyorsa, Bu izleri, [Uygulama Öngörüleri'ndeki Java izleme günlüklerini Keşfedin'deki](/azure/application-insights/app-insights-java-trace-logs)günlük çerçeve yapılandırma yönergelerini kullanarak gözden geçirilmeüzere Azure Uygulama Öngörüleri'ne iletebilirsiniz.

### <a name="troubleshooting-tools"></a>Sorun giderme araçları

Dahili Java görüntüleri [Alp Linux](https://alpine-linux.readthedocs.io/en/latest/getting_started.html) işletim sistemine dayanır. Herhangi `apk` bir sorun giderme aracı veya komutları yüklemek için paket yöneticisini kullanın.

### <a name="flight-recorder"></a>Uçuş Kayıt Cihazı

Uygulama Hizmeti'ndeki tüm Linux Java görüntüleri, JVM'ye kolayca bağlanabilmeniz ve bir profil oluşturucu kaydı başlatabilmeniz veya yığın dökümü oluşturabilmeniz için Zulu Flight Recorder yüklü.

#### <a name="timed-recording"></a>Zamanlı Kayıt

Başlamak için, SSH App Hizmeti içine `jcmd` ve çalışan tüm Java süreçlerinin listesini görmek için komutu çalıştırın. JCMD'nin kendisine ek olarak, Java uygulamanızın bir işlem kimlik numarası (pid) ile çalıştığını görmeniz gerekir.

```shell
078990bbcd11:/home# jcmd
Picked up JAVA_TOOL_OPTIONS: -Djava.net.preferIPv4Stack=true
147 sun.tools.jcmd.JCmd
116 /home/site/wwwroot/app.jar
```

JVM'nin 30 saniyelik kaydını başlatmak için aşağıdaki komutu uygulayın. Bu, JVM'nin profilini oluşturur ve ev dizininde *jfr_example.jfr* adlı bir JFR dosyası oluşturur. (Java uygulamanızın pid'i ile 116'yı değiştirin.)

```shell
jcmd 116 JFR.start name=MyRecording settings=profile duration=30s filename="/home/jfr_example.jfr"
```

30 saniyelik aralıkta, kayıt çalıştırarak `jcmd 116 JFR.check`yer alıyor doğrulayabilirsiniz. Bu, verilen Java işlemine ait tüm kayıtları gösterir.

#### <a name="continuous-recording"></a>Sürekli Kayıt

Zulu Flight Recorder'ı kullanarak Java uygulamanızın profilini çalışma zamanı performansı üzerinde en az etkiyle[(kaynak)](https://assets.azul.com/files/Zulu-Mission-Control-data-sheet-31-Mar-19.pdf)sürekli olarak kullanabilirsiniz. Bunu yapmak için, gerekli yapılandırmaya sahip JAVA_OPTS adlı bir Uygulama Ayarı oluşturmak için aşağıdaki Azure CLI komutunu çalıştırın. uygulamanız başlatıldığında JAVA_OPTS Uygulama Ayarı'nın içeriği `java` komuta aktarılır.

```azurecli
az webapp config appsettings set -g <your_resource_group> -n <your_app_name> --settings JAVA_OPTS=-XX:StartFlightRecording=disk=true,name=continuous_recording,dumponexit=true,maxsize=1024m,maxage=1d
```

Kayıt başladıktan sonra, `JFR.dump` komutu kullanarak geçerli kayıt verilerini istediğiniz zaman dökümü yapabilirsiniz.

```shell
jcmd <pid> JFR.dump name=continuous_recording filename="/home/recording1.jfr"
```

Daha fazla bilgi için lütfen [Jcmd Komut Başvurusu'na](https://docs.oracle.com/javacomponents/jmc-5-5/jfr-runtime-guide/comline.htm#JFRRT190)bakın.

### <a name="analyzing-recordings"></a>Kayıtları Analiz Etme

JFR dosyanızı yerel makinenize indirmek için [FTPS'yi](../deploy-ftp.md) kullanın. JFR dosyasını analiz etmek için Zulu Görev Kontrol'ü indirip [kurun.](https://www.azul.com/products/zulu-mission-control/) Zulu Görev Kontrol ile ilgili talimatlar [için, Azul belgelerine](https://docs.azul.com/zmc/) ve [kurulum talimatlarına](https://docs.microsoft.com/java/azure/jdk/java-jdk-flight-recorder-and-mission-control)bakın.

## <a name="customization-and-tuning"></a>Özelleştirme ve aetme

Linux için Azure Uygulama Hizmeti, Azure portalı ve CLI üzerinden kutu atonlama ve özelleştirme dışında destekler. Java'ya özgü olmayan web uygulaması yapılandırması için aşağıdaki makaleleri inceleyin:

- [Uygulama ayarlarını yapılandırma](../configure-common.md?toc=/azure/app-service/containers/toc.json#configure-app-settings)
- [Özel bir etki alanı ayarlama](../app-service-web-tutorial-custom-domain.md?toc=/azure/app-service/containers/toc.json)
- [SSL bağlamalarını yapılandırma](../configure-ssl-bindings.md?toc=/azure/app-service/containers/toc.json)
- [CDN ekle](../../cdn/cdn-add-to-web-app.md?toc=/azure/app-service/containers/toc.json)
- [Kudu sitesini yapılandırın](https://github.com/projectkudu/kudu/wiki/Configurable-settings#linux-on-app-service-settings)

### <a name="set-java-runtime-options"></a>Java çalışma zamanı seçeneklerini ayarlama

Hem Tomcat hem de Java SE ortamlarında ayrılmış bellek veya diğer JVM `JAVA_OPTS` çalışma zamanı seçeneklerini ayarlamak için seçeneklerle birlikte bir [uygulama ayarı](../configure-common.md?toc=/azure/app-service/containers/toc.json#configure-app-settings) oluşturun. App Service Linux, bu ayarı bir ortam değişkeni olarak Java çalışma süresine aktarMaktadır.

Azure portalında, web uygulaması için **Uygulama Ayarları** altında, `JAVA_OPTS` ', ', ', `-Xms512m -Xmx1204m`', ', ', 'ler gibi ek ayarları içeren yeni bir uygulama ayarı oluşturun.

Maven eklentisinden uygulama ayarını yapılandırmak için Azure eklentisi bölümüne ayar/değer etiketleri ekleyin. Aşağıdaki örnek, belirli bir minimum ve maksimum Java yığın boyutu ayarlar:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Xms512m -Xmx1204m</value>
    </property>
</appSettings>
```

Uygulama Hizmeti planında tek bir dağıtım yuvası olan tek bir uygulama çalıştıran geliştiriciler aşağıdaki seçenekleri kullanabilir:

- B1 ve S1 örnekleri:`-Xms1024m -Xmx1024m`
- B2 ve S2 örnekleri:`-Xms3072m -Xmx3072m`
- B3 ve S3 örnekleri:`-Xms6144m -Xmx6144m`

Uygulama yığını ayarlarını ayarlarken, Uygulama Hizmeti planı ayrıntılarınızı gözden geçirin ve en uygun bellek tahsisini bulmak için birden çok uygulama ve dağıtım yuvası nın gerekeni göz önünde bulundurun.

Bir JAR uygulaması dağıtıyorsanız, yerleşik görüntünün uygulamanızı doğru bir şekilde tanımlayabilmesi için *app.jar* olarak adlandırılmalıdır. (Maven eklentisi bu yeniden adlandırmayı otomatik olarak yapar.) JAR'ınızı *app.jar*olarak yeniden adlandırmak istemiyorsanız, JAR'ınızı çalıştırmak için komutuyla bir kabuk komutdosyası yükleyebilirsiniz. Sonra bu betiğin tam yolunu portalın Yapılandırma bölümündeki [Başlangıç Dosyası](app-service-linux-faq.md#built-in-images) metin kutusuna yapıştırın. Başlatma dizesi içine yerleştirildiği dizinden çalıştırılmaz. Bu nedenle başlatma dizenizde her zaman başvuru dosyalarının mutlak yollarını kullanın (örneğin: `java -jar /home/myapp/myapp.jar`).

### <a name="turn-on-web-sockets"></a>Web soketlerini açın

Uygulama için **Uygulama ayarlarında** Azure portalındaki web soketleri için desteği açın. Ayarın etkili olması için uygulamayı yeniden başlatmanız gerekir.

Azure CLI'yi kullanarak web soketi desteğini aşağıdaki komutla açın:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --web-sockets-enabled true
```

Ardından başvurunuzu yeniden başlatın:

```azurecli-interactive
az webapp stop --name <app-name> --resource-group <resource-group-name>
az webapp start --name <app-name> --resource-group <resource-group-name>
```

### <a name="set-default-character-encoding"></a>Varsayılan karakter kodlamayı ayarlama

Azure portalında, web uygulaması için **Uygulama Ayarları** altında, `JAVA_OPTS` değer `-Dfile.encoding=UTF-8`içeren yeni bir uygulama ayarı oluşturun.

Alternatif olarak, App Service Maven eklentisini kullanarak uygulama ayarını yapılandırabilirsiniz. Eklenti yapılandırmasında ayar adı ve değer etiketlerini ekleyin:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Dfile.encoding=UTF-8</value>
    </property>
</appSettings>
```

### <a name="adjust-startup-timeout"></a>Başlatma zaman arasını ayarlama

Java uygulamanız özellikle büyükse, başlangıç süresini artırmanız gerekir. Bunu yapmak için bir uygulama `WEBSITES_CONTAINER_START_TIME_LIMIT` ayarı oluşturun ve uygulama hizmetinin zamanlamayı sona ermeden önce beklemesi gereken saniye sayısına ayarlayın. Maksimum değer `1800` saniyedir.

### <a name="pre-compile-jsp-files"></a>JSP dosyalarını Önceden Derle

Tomcat uygulamalarının performansını artırmak için, Uygulama Hizmeti'ne dağıtmadan önce JSP dosyalarınızı derleyebilirsiniz. Apache Sling tarafından sağlanan [Maven eklentisini](https://sling.apache.org/components/jspc-maven-plugin/plugin-info.html) veya bu [Ant yapı dosyasını](https://tomcat.apache.org/tomcat-9.0-doc/jasper-howto.html#Web_Application_Compilation)kullanabilirsiniz.

## <a name="secure-applications"></a>Uygulamaları güvenli hale getirme

Linux için App Service'de çalışan Java uygulamaları, diğer uygulamalarla aynı [güvenlik en iyi uygulama](/azure/security/security-paas-applications-using-app-services) kümesine sahiptir.

### <a name="authenticate-users-easy-auth"></a>Kullanıcıları kimlik doğrulaması (Easy Auth)

Azure portalında uygulama kimlik doğrulamasını **Kimlik Doğrulama ve Yetkilendirme** seçeneğiyle ayarlayın. Buradan, Azure Active Directory'yi veya Facebook, Google veya GitHub gibi sosyal girişleri kullanarak kimlik doğrulamayı etkinleştirebilirsiniz. Azure portalı yapılandırması yalnızca tek bir kimlik doğrulama sağlayıcısını yapılandırırken çalışır. Daha fazla bilgi için Azure Active Directory oturumunu ve diğer kimlik sağlayıcıları için ilgili makaleleri [kullanmak için Uygulama Hizmeti uygulamanızı yapılandır'](../configure-authentication-provider-aad.md?toc=/azure/app-service/containers/toc.json) a bakın. Birden çok oturum açma sağlayıcısını etkinleştirmeniz gerekiyorsa, [Özel Servis kimlik doğrulama](../app-service-authentication-how-to.md?toc=/azure/app-service/containers/toc.json) makalesindeki yönergeleri izleyin.

#### <a name="tomcat"></a>Tomcat

Tomcat uygulamanız, Temel nesneyi bir Harita nesnesine atlayarak kullanıcının taleplerine doğrudan servletten erişebilir. Harita nesnesi, her talep türünü bu türdeki taleplerin bir koleksiyonuyla eşler. Aşağıdaki kodda, `request` bir `HttpServletRequest`örneğidir.

```java
Map<String, Collection<String>> map = (Map<String, Collection<String>>) request.getUserPrincipal();
```

Artık nesneyi `Map` belirli bir hak talebi için inceleyebilirsiniz. Örneğin, aşağıdaki kod tüm talep türlerini snippet iterates ve her koleksiyonun içeriğini yazdırır.

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

Kullanıcıları oturum dışı etmek `/.auth/ext/logout` için yolu kullanın. Diğer işlemleri gerçekleştirmek için lütfen [Uygulama Hizmeti Kimlik Doğrulaması ve Yetkilendirme kullanımına](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to)ilişkin belgelere bakın. Ayrıca Tomcat [HttpServletRequest arayüzü](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html) ve yöntemleri hakkında resmi belgeler bulunmaktadır. Aşağıdaki servlet yöntemleri, Uygulama Hizmeti yapılandırmanıza bağlı olarak da sulandırılır:

```java
public boolean isSecure()
public String getRemoteAddr()
public String getRemoteHost()
public String getScheme()
public int getServerPort()
```

Bu özelliği devre dışı düşürmek için `WEBSITE_AUTH_SKIP_PRINCIPAL` , `1`. değeri ile adlandırılan bir Uygulama Ayarı oluşturun App Service tarafından eklenen tüm servlet filtrelerini devre `WEBSITE_SKIP_FILTERS` dışı bırakmak `1`için , .değeri olan bir ayar oluşturun

#### <a name="spring-boot"></a>Spring Boot

Bahar Önyükleme geliştiricileri, tanıdık Bahar Güvenliği ek açıklamaları ve API'leri kullanarak uygulamaları güvence altına almak için [Azure Active Directory İlkbahar Önyükleme başlatıcısını](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) kullanabilir. *Uygulama.özellikler* dosyanızdaki en yüksek üstbilgi boyutunu artırdığından emin olun. Biz bir değer `16384`öneririz .

### <a name="configure-tlsssl"></a>TLS/SSL yapılandırma

Varolan bir SSL sertifikasını yüklemek ve uygulamanızın etki alanı adına bağlamak için [Azure Uygulama Hizmeti'nde SSL bağlayıcısı olan özel bir DNS adını Güvenli'deki](../configure-ssl-bindings.md?toc=/azure/app-service/containers/toc.json) yönergeleri izleyin. Varsayılan olarak uygulamanız, HTTP bağlantılarının SSL ve TLS'yi uygulamak için öğreticideki belirli adımları izlemesine izin vermeye devam edecektir.

### <a name="use-keyvault-references"></a>KeyVault Referanslarını Kullanma

[Azure KeyVault,](../../key-vault/key-vault-overview.md) erişim ilkeleri ve denetim geçmişiyle merkezi leştirilmiş gizli yönetim sağlar. KeyVault'ta sırları (parolalar veya bağlantı dizeleri gibi) depolayabilir ve uygulamanızdaki bu sırlara ortam değişkenleri aracılığıyla erişebilirsiniz.

İlk olarak, [Uygulamanızın Key Vault'a erişimini sağlamak](../app-service-key-vault-references.md#granting-your-app-access-to-key-vault) ve [Uygulama Ayarında sırrınıza KeyVault göndermesi yapmak](../app-service-key-vault-references.md#reference-syntax)için talimatları izleyin. Uygulama Hizmeti terminaline uzaktan erişirken ortam değişkenini yazdırarak başvurunun gizliye çözüldüğünü doğrulayabilirsiniz.

Bahar veya Tomcat yapılandırma dosyanızda bu sırları enjekte etmek`${MY_ENV_VAR}`için, ortam değişkenenjeksiyon sözdizimini kullanın ( ). Yay yapılandırma dosyaları için lütfen [dışa uygun yapılandırmalarla](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)ilgili bu belgelere bakın.

### <a name="using-the-java-key-store"></a>Java Anahtar Mağazasını Kullanma

Varsayılan olarak, Uygulama Hizmeti [Linux'a yüklenen](../configure-ssl-certificate.md) tüm ortak veya özel sertifikalar, konteyner başladığında ilgili Java Key Mağazaları'na yüklenir. Sertifikanızı yükledikten sonra, Uygulama Hizmetinizin Java Key Store'a yüklenmesi için yeniden başlatmanız gerekir. Genel sertifikalar Key Store'a `$JAVA_HOME/jre/lib/security/cacerts`yüklenir ve özel sertifikalar ' da `$JAVA_HOME/lib/security/client.jks`saklanır.

Java Key Store'daki sertifikalarla JDBC bağlantınızı şifrelemek için ek yapılandırma gerekebilir. Lütfen seçtiğiniz JDBC sürücüsünün belgelerine bakın.

- [PostgreSQL](https://jdbc.postgresql.org/documentation/head/ssl-client.html)
- [SQL Server](https://docs.microsoft.com/sql/connect/jdbc/connecting-with-ssl-encryption?view=sql-server-ver15)
- [Mysql](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-reference-using-ssl.html)
- [MongoDB](https://mongodb.github.io/mongo-java-driver/3.4/driver/tutorials/ssl/)
- [Cassandra](https://docs.datastax.com/en/developer/java-driver/4.3/)

#### <a name="initializing-the-java-key-store"></a>Java Anahtar Mağazası'nı başlatma

Nesneyi başlatmayı `import java.security.KeyStore` sağlamak için keystore dosyasını parolayla yükleyin. Her iki anahtar deposu için varsayılan parola "değiştir"dir.

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

#### <a name="manually-load-the-key-store"></a>Anahtar deposunu el ile yükleyin

Sertifikaları anahtar deposuna el ile yükleyebilirsiniz. Uygulama Hizmeti'nin `SKIP_JAVA_KEYSTORE_LOAD` `1` sertifikaları anahtar mağazaya otomatik olarak yüklemesini devre dışı düşürme değerine sahip bir uygulama ayarı oluşturun. Azure portalı üzerinden Uygulama Hizmeti'ne yüklenen tüm genel `/var/ssl/certs/`sertifikalar . Özel sertifikalar altında `/var/ssl/private/`saklanır.

Uygulama Hizmetinize [bir SSH bağlantısı açarak](app-service-linux-ssh-support.md) ve komutu çalıştırarak Java `keytool`Anahtar Aracı'nı etkileşime alabilir veya hata ayıklayabilirsiniz. Komutlistesi için [Anahtar Aracı belgelerine](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) bakın. KeyStore API hakkında daha fazla bilgi için lütfen [resmi belgelere](https://docs.oracle.com/javase/8/docs/api/java/security/KeyStore.html)bakın.

## <a name="configure-apm-platforms"></a>APM platformlarını yapılandırma

Bu bölümde, Linux'ta Azure Uygulama Hizmeti'nde dağıtılan Java uygulamalarının NewRelic ve AppDynamics uygulama performans izleme (APM) platformlarına nasıl bağlanılacakları gösterilmektedir.

### <a name="configure-new-relic"></a>Yeni Emaneti Yapılandır

1. [NewRelic.com'da](https://newrelic.com/signup) bir NewRelic hesabı oluşturma
2. NewRelic Java aracısını indirin, *newrelic-java-x.x.x.zip*benzer bir dosya adı olacaktır.
3. Lisans anahtarınızı kopyalayın, aracıyı daha sonra yapılandırmak için ihtiyacınız olacak.
4. [SSH App Service örneğinize ve](app-service-linux-ssh-support.md) yeni bir dizin */home/site/wwwroot/apm*oluşturun.
5. Paketlenmemiş NewRelic Java aracıdosyalarını */home/site/wwwroot/apm*altında bir dizine yükleyin. Aracınızın dosyaları */home/site/wwwroot/apm/newrelic*adresinde olmalıdır.
6. YAML dosyasını */home/site/wwwroot/apm/newrelic/newrelic.yml* adresindedeğiştirin ve yer tutucu lisans değerini kendi lisans anahtarınızla değiştirin.
7. Azure portalında, Uygulama Hizmeti'nde uygulamanıza göz atın ve yeni bir Uygulama Ayarı oluşturun.
    - Uygulamanız Java **SE**kullanıyorsa, değeri `JAVA_OPTS` `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`ile adlı bir ortam değişkeni oluşturun.
    - **Tomcat**kullanıyorsanız, değeri `CATALINA_OPTS` `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`ile adlı bir ortam değişkeni oluşturun.

### <a name="configure-appdynamics"></a>AppDynamics'i Yapılandır

1. [AppDynamics.com'da](https://www.appdynamics.com/community/register/) bir AppDynamics hesabı oluşturun
2. AppDynamics web sitesinden Java aracısını indirin, dosya adı *AppServerAgent-x.x.x.xxxxx.zip* benzer olacaktır
3. [SSH App Service örneğinize ve](app-service-linux-ssh-support.md) yeni bir dizin */home/site/wwwroot/apm*oluşturun.
4. Java aracısı dosyalarını */home/site/wwwroot/apm*altında bir dizine yükleyin. Aracınızın dosyaları */home/site/wwwroot/apm/appdynamics*adresinde olmalıdır.
5. Azure portalında, Uygulama Hizmeti'nde uygulamanıza göz atın ve yeni bir Uygulama Ayarı oluşturun.
    - **Java SE**kullanıyorsanız, Uygulama Hizmeti adınızın `JAVA_OPTS` olduğu `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` `<app-name>` değeri içeren bir ortam değişkeni oluşturun.
    - **Tomcat**kullanıyorsanız, Uygulama Hizmeti adınızın `CATALINA_OPTS` olduğu `<app-name>` `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` değeri içeren bir ortam değişkeni oluşturun.

> [!NOTE]
> Zaten bir `JAVA_OPTS` ortam değişkeninvarsa `CATALINA_OPTS`veya geçerli `-javaagent:/...` değerin sonuna seçeneği ekleyin.

## <a name="configure-jar-applications"></a>JAR Uygulamalarını Yapılandırma

### <a name="starting-jar-apps"></a>JAR Uygulamalarını Başlatma

Varsayılan olarak, Uygulama Hizmeti JAR uygulamanızın *app.jar*olarak adlandırılmasını bekler. Bu ada sahipse, otomatik olarak çalıştırılacaktır. Maven kullanıcıları *için, pom.xml* `<finalName>app</finalName>` bölümüne `<build>` ekleyerek JAR adını ayarlayabilirsiniz. Aynı özelliği ayarlayarak `archiveFileName` [Gradle'de](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html#org.gradle.api.tasks.bundling.Jar:archiveFileName) de yapabilirsiniz.

JAR'ınız için farklı bir ad kullanmak istiyorsanız, JAR dosyanızı çalıştıran [Başlangıç Komutunu](app-service-linux-faq.md#built-in-images) da sağlamanız gerekir. Örneğin, `java -jar my-jar-app.jar`. Başlangıç Komutunuzun değerini Portal'da, Configuration > Genel Ayarlar'ın altında `STARTUP_COMMAND`veya 'adı verilen bir Uygulama Ayarı ile ayarlayabilirsiniz.

### <a name="server-port"></a>Sunucu Bağlantı Noktası

App Service Linux gelen istekleri port 80'e yönlendirir, bu nedenle uygulamanız 80. Bunu uygulamanızın yapılandırmasında (Bahar'ın *application.properties* dosyası gibi) veya Başlangıç Komutunuzda (örneğin) `java -jar spring-app.jar --server.port=80`yapabilirsiniz. Lütfen ortak Java çerçeveleri için aşağıdaki belgelere bakın:

- [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html#howto-use-short-command-line-arguments)
- [SparkJava](http://sparkjava.com/documentation#embedded-web-server)
- [Mikronaut](https://docs.micronaut.io/latest/guide/index.html#runningSpecificPort)
- [Oyun Çerçevesi](https://www.playframework.com/documentation/2.6.x/ConfiguringHttps#Configuring-HTTPS)
- [Vertx](https://vertx.io/docs/vertx-core/java/#_start_the_server_listening)
- [Kuarkus](https://quarkus.io/guides/application-configuration-guide)

## <a name="data-sources"></a>Veri kaynakları

### <a name="tomcat"></a>Tomcat

Bu yönergeler tüm veritabanı bağlantıları için geçerlidir. Yer tutucuları seçtiğiniz veritabanının sürücü sınıfı adı ve JAR dosyasıyla doldurmanız gerekir. Sağlanan sınıf adları ve ortak veritabanları için sürücü indirme içeren bir tablodur.

| Database   | Sürücü Sınıf Adı                             | JDBC Sürücüsü                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [İndir](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [İndir](https://dev.mysql.com/downloads/connector/j/) ("Platform Bağımsız"ı seçin) |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [İndir](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-2017#download)                                                           |

Tomcat'ı Java Veritabanı Bağlantısı (JDBC) veya Java Kalıcılık API'sını (JPA) kullanacak şekilde yapılandırmak için, önce Başlangıç sırasında Tomcat tarafından okunan `CATALINA_OPTS` ortam değişkenini özelleştirin. Bu değerleri [App Service Maven eklentisinde](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md)bir uygulama ayarı üzerinden ayarlayın:

```xml
<appSettings>
    <property>
        <name>CATALINA_OPTS</name>
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>
    </property>
</appSettings>
```

Veya Azure portalındaki **Yapılandırma** > **Uygulama Ayarları** sayfasında ortam değişkenlerini ayarlayın.

Ardından, veri kaynağının bir uygulama için mi yoksa Tomcat servlet'inde çalışan tüm uygulamalarda mı kullanılabileceğini belirleyin.

#### <a name="application-level-data-sources"></a>Uygulama düzeyinde veri kaynakları

1. Projenizin *META-INF/* dizininde bir *context.xml* dosyası oluşturun. YOKSA *META-INF/* dizini oluşturun.

2. *Context.xml*olarak, `Context` bir JNDI adresine veri kaynağı bağlamak için bir öğe ekleyin. Yer `driverClassName` tutucuyu yukarıdaki tablodan sürücünüzün sınıf adı ile değiştirin.

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

3. Uygulamanızdaki veri kaynağını kullanmak için uygulamanızın *web.xml'ini* güncelleyin.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="shared-server-level-resources"></a>Paylaşılan sunucu düzeyinde kaynaklar

Paylaşılan, sunucu düzeyinde bir veri kaynağı eklemek, Tomcat'ın server.xml'ini düzenlemenizi gerektirir. İlk olarak, bir [başlangıç komut dosyası](app-service-linux-faq.md#built-in-images) yükleyin ve **Configuration** > **Startup Komutu**komut dosyasına yol ayarlayın. FtP kullanarak başlangıç [FTP](../deploy-ftp.md)komut dosyasını yükleyebilirsiniz.

Başlangıç komut dosyanız server.xml dosyasına [xsl dönüşümü](https://www.w3schools.com/xml/xsl_intro.asp) yapacak ve elde `/usr/local/tomcat/conf/server.xml`edilen xml dosyasını . Başlangıç komut dosyası apk üzerinden libxslt yüklemeniz gerekir. xsl dosyanız ve başlangıç komut dosyanız FTP üzerinden yüklenebilir. Aşağıda bir örnek başlangıç komut dosyası dır.

```sh
# Install libxslt. Also copy the transform file to /home/tomcat/conf/
apk add --update libxslt

# Usage: xsltproc --output output.xml style.xsl input.xml
xsltproc --output /home/tomcat/conf/server.xml /home/tomcat/conf/transform.xsl /usr/local/tomcat/conf/server.xml
```

Örnek bir xsl dosyası aşağıda verilmiştir. Örnek xsl dosyası Tomcat server.xml'e yeni bir bağlayıcı düğüm ekler.

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

#### <a name="finalize-configuration"></a>Yapılandırmayı sonuçlandırın

Son olarak, sürücü JARs Tomcat classpath yerleştirin ve App Service yeniden başlatın.

1. JDBC sürücü dosyalarının */home/tomcat/lib* dizinine yerleştirerek Tomcat sınıf yükleyicisinin kullanımına sunulduğundan emin olun. (Zaten yoksa bu dizini oluşturun.) Bu dosyaları Uygulama Hizmeti örneğinize yüklemek için aşağıdaki adımları gerçekleştirin:

    1. Bulut [Kabuğu'nda](https://shell.azure.com)webapp uzantısını yükleyin:

      ```azurecli-interactive
      az extension add -–name webapp
      ```

    2. Yerel sisteminizden Uygulama Hizmeti'ne bir SSH tüneli oluşturmak için aşağıdaki CLI komutunu çalıştırın:

      ```azurecli-interactive
      az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
      ```

    3. SFTP istemcinizle yerel tünel bağlantı noktasına bağlanın ve dosyaları */home/tomcat/lib* klasörüne yükleyin.

    Alternatif olarak, JDBC sürücüsünü yüklemek için bir FTP istemcisi kullanabilirsiniz. [FTP kimlik bilgilerinizi almak için](../deploy-configure-credentials.md?toc=/azure/app-service/containers/toc.json)aşağıdaki yönergeleri izleyin.

2. Sunucu düzeyinde bir veri kaynağı oluşturduysanız, App Service Linux uygulamasını yeniden başlatın. Tomcat güncelleştirilmiş `CATALINA_BASE` `/home/tomcat` yapılandırmayı sıfırlar ve kullanır.

### <a name="spring-boot"></a>Spring Boot

Bahar Önyükleme uygulamalarındaki veri kaynaklarına bağlanmak için, bağlantı dizeleri oluşturmanızı ve *bunları uygulamanızın özellikleri* dosyanıza enjekte etmenizi öneririz.

1. App Service sayfasının "Yapılandırma" bölümünde, dize için bir ad ayarlayın, JDBC bağlantı dizenizi değer alanına yapıştırın ve türü "Özel" olarak ayarlayın. İsteğe bağlı olarak bu bağlantı dizelerini yuva ayarı olarak ayarlayabilirsiniz.

    Bu bağlantı dizesi, uygulamamız için `CUSTOMCONNSTR_<your-string-name>`bir ortam değişkeni olarak erişilebilir. Örneğin, yukarıda oluşturduğumuz bağlantı dizesi adı `CUSTOMCONNSTR_exampledb`verilecektir.

2. *Application.properties* dosyanızda, bu bağlantı dizesini ortam değişken adı ile başvurun. Örneğimiz için, aşağıdakileri kullanırız.

    ```yml
    app.datasource.url=${CUSTOMCONNSTR_exampledb}
    ```

Bu konu hakkında daha fazla bilgi için lütfen veri erişimi ve [dışaaklanmış yapılandırmalarla](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) [ilgili Bahar Önyükleme belgelerine](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-data-access.html) bakın.

## <a name="use-redis-as-a-session-cache-with-tomcat"></a>Redis'i Tomcat ile oturum önbelleği olarak kullanma

Tomcat'ı [Redis için Azure Önbelleği](/azure/azure-cache-for-redis/)gibi harici bir oturum deposu kullanacak şekilde yapılandırabilirsiniz. Bu, bir kullanıcı uygulamanızın başka bir örneğine aktarıldığında (örneğin alışveriş sepeti verileri gibi) kullanıcı oturumu durumunu (alışveriş sepeti verileri gibi) korumanızı sağlar( örneğin otomatik ölçekleme, yeniden başlatma veya başarısız lık gerçekleştiğinde.

Redis ile Tomcat'ı kullanmak için uygulamanızı [PersistentManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html) uygulaması kullanacak şekilde yapılandırmanız gerekir. Aşağıdaki adımlar, bu işlemi [Pivotal Session Manager:redis-store'u](https://github.com/pivotalsoftware/session-managers/tree/master/redis-store) örnek olarak kullanarak açıklar.

1. Bir Bash terminali `<variable>=<value>` açın ve aşağıdaki ortam değişkenlerinin her birini ayarlamak için kullanın.

    | Değişken                 | Değer                                                                      |
    |--------------------------|----------------------------------------------------------------------------|
    | RESOURCEGROUP_NAME       | Uygulama Hizmeti örneğini içeren kaynak grubunun adı.       |
    | WEBAPP_NAME              | Uygulama Hizmeti örneğinizin adı.                                     |
    | WEBAPP_PLAN_NAME         | Uygulama Hizmeti planınızın adı.                                         |
    | BÖLGE                   | Uygulamanızın barındırıldığı bölgenin adı.                           |
    | REDIS_CACHE_NAME         | Redis için Azure Önbelleğinizin adı.                           |
    | REDIS_PORT               | Redis önbelleğinizin dinlediği SSL bağlantı noktası.                             |
    | REDIS_PASSWORD           | Örneğiniz için birincil erişim anahtarı.                                  |
    | REDIS_SESSION_KEY_PREFIX | Uygulamanızdan gelen oturum anahtarlarını tanımlamak için belirlediğiniz bir değer. |

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

    Hizmet örneğinizin **Özellikler** veya **Erişim anahtarları** bölümlerine bakarak Azure portalındaki ad, bağlantı noktası ve erişim anahtar bilgilerini bulabilirsiniz.

2. Uygulamanızın *src/main/webapp/META-INF/context.xml* dosyasını aşağıdaki içerikle oluşturun veya güncelleyin:

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

    Bu dosya, uygulamanız için oturum yöneticisi uygulamasını belirtir ve yapılandırır. Hesap bilgilerinizi kaynak dosyalarınızdan uzak tutmak için önceki adımda ayarladığınız ortam değişkenlerini kullanır.

3. Oturum yöneticisinin JAR dosyasını Uygulama Hizmeti örneğinize yükleyerek */home/tomcat/lib* dizinine yerleştirmek için FTP'yi kullanın. Daha fazla bilgi için [ftp/S kullanarak uygulamanızı Azure Uygulama Hizmetine dağıtın'](https://docs.microsoft.com/azure/app-service/deploy-ftp)a bakın.

4. Uygulama Hizmeti örneğiniz için [oturum arabilgisini](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) devre dışı kınla. Bunu, uygulamanıza yön vererek ve ardından > Genel yapılandırma ayarlarını > **ARR afinitesini** > **Off'a**ayarlayarak Azure portalından yapabilirsiniz. Alternatif olarak, aşağıdaki komutu kullanabilirsiniz:

    ```azurecli
    az webapp update -g <resource group> -n <webapp name> --client-affinity-enabled false
    ```

    Varsayılan olarak, Uygulama Hizmeti, varolan oturumlarla olan istemci isteklerinin uygulamanızın aynı örneğine yönlendirilmesini sağlamak için oturum afiyeti tanımlama bilgilerini kullanır. Bu varsayılan davranış yapılandırma gerektirmez, ancak uygulama örneğiniz yeniden başlatıldığında veya trafik başka bir örne yönlendirildiğinde kullanıcı oturumu durumunu koruyamaz. Oturum çereztabanlı yönlendirmeyi kapatmak için [varolan ARR Instance Affinity](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) yapılandırmasını devre dışı bıraktığınızda, yapılandırılan oturum deposunun girişim olmadan çalışmasına izin verirsiniz.

5. Uygulama Hizmeti örneğinizin **Özellikler** bölümüne gidin ve **Ek Giden IP Adreslerini**bulun. Bunlar, uygulamanız için olası tüm giden IP adreslerini temsil eder. Bunları bir sonraki adımda kullanmak üzere kopyalayın.

6. Her IP adresi için, Redis örneğinde Azure Önbelleğinde bir güvenlik duvarı kuralı oluşturun. Bunu Redis örneğinizin Güvenlik **Duvarı** bölümünden Azure portalında yapabilirsiniz. Her kural için benzersiz bir ad sağlayın ve **Başlat IP adresi** ve Ip Sonu **adres** değerlerini aynı IP adresine ayarlayın.

7. Redis örneğinizin **Gelişmiş ayarlar** bölümüne gidin ve yalnızca SSL üzerinden **No'ya** **erişime izin ver'i** ayarlayın. Bu, Uygulama Hizmeti örneğinizin Azure altyapısı üzerinden Redis önbelleğinizle iletişim kurmasını sağlar.

8. Redis `azure-webapp-maven-plugin` hesap bilgilerinize başvurmak için uygulamanızın *pom.xml* dosyasındaki yapılandırmayı güncelleyin. Bu dosya, hesap bilgilerinizi kaynak dosyalarınızdan uzak tutmak için daha önce ayarladığınız ortam değişkenlerini kullanır.

    Gerekirse `1.7.0` sürümünü [Azure App Service için Maven Eklentisinin](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) geçerli sürümüyle değiştirin.

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

9. Uygulamanızı yeniden oluşturup yeniden dağıtın.

    ```bash
    mvn package -DskipTests azure-webapp:deploy
    ```

Uygulamanız artık oturum yönetimi için Redis önbelleğinizi kullanacaktır.

Bu yönergeleri sınamak için kullanabileceğiniz bir örnek için, GitHub'daki [ölçekleme durumu-java-web-app-on-azure](https://github.com/Azure-Samples/scaling-stateful-java-web-app-on-azure) repo'ya bakın.

## <a name="docker-containers"></a>Docker kapsayıcıları

Kapsayıcılarınızda Azure destekli Zulu JDK'yı kullanmak [için, azure indirme sayfası için desteklenen Azul Zulu Enterprise'dan](https://www.azul.com/downloads/azure-only/zulu/) belgelenen önceden `Dockerfile` oluşturulmuş görüntüleri çektiğinizden ve kullandığınızdan emin olun veya Microsoft [Java GitHub reposundaki](https://github.com/Microsoft/java/tree/master/docker)örnekleri kullanın.

## <a name="statement-of-support"></a>Destek bildirimi

### <a name="runtime-availability"></a>Çalışma zamanı kullanılabilirliği

Linux için App Service, Java web uygulamalarının yönetilen barındırması için iki çalışma süresi destekler:

- Web arşivi (WAR) dosyaları olarak paketlenmiş uygulamaları çalıştırmak için [Tomcat servlet kapsayıcı.](https://tomcat.apache.org/) Desteklenen sürümler 8.5 ve 9.0'dır.
- Java arşivi (JAR) dosyaları olarak paketlenmiş uygulamaları çalıştırmak için Java SE çalışma zamanı ortamı. Desteklenen sürümler Java 8 ve 11'dir.

### <a name="jdk-versions-and-maintenance"></a>JDK versiyonları ve bakımı

OpenJDK'nın Azul Zulu Enterprise yapıları, Microsoft ve Azul Systems tarafından desteklenen Azure ve Azure Yığını için OpenJDK'nın ücretsiz, çok platformlu, üretime hazır dağıtımıdır. Java SE uygulamalarını oluşturmak ve çalıştırmak için gereken tüm bileşenleri içerir. [Java JDK Kurulumu'ndan](https://aka.ms/azure-jdks)JDK yükleyebilirsiniz.

Desteklenen JDK'lar her yılın Ocak, Nisan, Temmuz ve Ekim aylarında üç ayda bir otomatik olarak yamalı olur.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Büyük güvenlik açıklarına yönelik düzeltmeler ve düzeltmeler, Azul Systems'den kullanıma sunulduğu anda piyasaya sürülecek. "Büyük" bir güvenlik [açığı, NIST Ortak Güvenlik Açığı Puanlama Sistemi,sürüm 2'de](https://nvd.nist.gov/cvss.cfm)9,0 veya daha yüksek bir taban puanla tanımlanır.

### <a name="deprecation-and-retirement"></a>Amortisman ve emeklilik

Desteklenen bir Java çalışma süresi kullanımdan kaldırılacaksa, etkilenen çalışma süresini kullanan Azure geliştiricilere çalışma süresinin kullanım süresinin kullanımdan kaldırılmasına en az altı ay kala bir amortisman bildirimi verilir.

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure hızlı başlangıçlarını, öğreticileri ve Java başvuru belgelerini bulmak [için Java Geliştiricileri](/java/azure/) için Azure merkezini ziyaret edin.

Java geliştirmeözel olmayan Linux için App Service kullanma hakkında genel sorular [App Service Linux SSS](app-service-linux-faq.md)yanıtlanır.
