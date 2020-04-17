---
title: Windows Java uygulamalarını yapılandırma
description: Azure Uygulama Hizmeti'ndeki Windows VM örneklerinde çalışacak şekilde Java uygulamalarını nasıl yapılandıracak şekilde yapılandırılabildiğini öğrenin. Bu makalede, en yaygın yapılandırma görevleri gösterir.
keywords: azure uygulama hizmeti, web uygulaması, windows, oss, java
author: jasonfreeberg
ms.devlang: java
ms.topic: article
ms.date: 04/12/2019
ms.author: jafreebe
ms.reviewer: cephalin
ms.custom: seodec18
ms.openlocfilehash: 179a2120714460c955d1fd3e345ebcd963ae564d
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81453566"
---
# <a name="configure-a-windows-java-app-for-azure-app-service"></a>Azure Uygulama Hizmeti için bir Windows Java uygulamasını yapılandırma

Azure Uygulama Hizmeti, Java geliştiricilerinin Tomcat web uygulamalarını tam olarak yönetilen Windows tabanlı bir hizmette hızla oluşturmalarına, dağıtmalarına ve ölçeklendirmelerine olanak tanır. Uygulamaları Komut satırından veya IntelliJ, Eclipse veya Visual Studio Code gibi editörlerden Maven eklentileriyle dağıtın.

Bu kılavuz, Uygulama Hizmeti'ni kullanan Java geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure Uygulama Hizmeti'ni hiç kullanmadıysanız, önce [Java hızlı başlat'ı](app-service-web-get-started-java.md) okumalısınız. Java geliştirmeye özgü olmayan Uygulama Hizmeti'ni kullanma yla ilgili genel sorular [App Service Windows SSS'de](faq-configuration-and-management.md)yanıtlanır.

## <a name="deploying-your-app"></a>Uygulamanızı dağıtma

.war dosyalarınızı dağıtmak [için Maven için Azure Web Uygulama Eklentisi'ni](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) kullanabilirsiniz. Popüler IDE'lerle dağıtım, [IntelliJ için Azure Araç Seti](/java/azure/intellij/azure-toolkit-for-intellij) veya Eclipse için Azure Araç [Seti](/java/azure/eclipse/azure-toolkit-for-eclipse)ile de desteklenir.

Aksi takdirde, dağıtım yönteminiz arşiv türünüze bağlıdır:

- .war dosyalarını Tomcat'a `/api/wardeploy/` dağıtmak için arşiv dosyanızı göndermek için bitiş noktasını kullanın. Bu API hakkında daha fazla bilgi için lütfen [bu belgelere](https://docs.microsoft.com/azure/app-service/deploy-zip#deploy-war-file)bakın.
- .jar dosyalarını Java SE'ye `/api/zipdeploy/` dağıtmak için Kudu sitesinin bitiş noktasını kullanın. Bu API hakkında daha fazla bilgi için lütfen [bu belgelere](https://docs.microsoft.com/azure/app-service/deploy-zip#rest)bakın.

.war'ınızı FTP kullanarak konuşlandırmayın. FTP aracı başlangıç komut dosyalarını, bağımlılıkları veya diğer çalışma zamanı dosyalarını yüklemek için tasarlanmıştır. Bu, web uygulamalarını dağıtmak için en uygun seçim değildir.

## <a name="logging-and-debugging-apps"></a>Uygulamaları günlüğe kaydetme ve hata ayıklama

Azure portalı üzerinden her uygulama için performans raporları, trafik görselleştirmeleri ve sistem durumu denetimleri mevcuttur. Daha fazla bilgi için Azure [Uygulama Hizmeti tanılama](overview-diagnostics.md)genel bakış bakın.

### <a name="use-flight-recorder"></a>Uçuş Kaydedici'yi Kullanın

Azul JVM'leri kullanarak App Service'deki tüm Java çalışma saatleri Zulu Flight Recorder ile birlikte gelir. Java uygulamalarınızdaki davranışları ve sorun giderme sorunlarını izlemek için JVM, sistem ve Java düzeyindeki olayları kaydetmek için bunu kullanabilirsiniz.

Zamanlı bir kayıt almak için Java uygulamasının PID (İşlem Kimliği) gerekir. PID'yi bulmak için web uygulamanızın SCM sitesine https://<site nizin adı>.scm.azurewebsites.net/ProcessExplorer/ adresinden bir tarayıcı açın. Bu sayfa, web uygulamanızda çalışan işlemleri gösterir. Tabloda "java" adlı işlemi bulun ve ilgili PID'yi (İşlem Kimliği) kopyalayın.

Ardından, Hata **Ayıklama Konsolu'nu** SCM sitesinin üst araç çubuğunda açın ve aşağıdaki komutu çalıştırın. Daha `<pid>` önce kopyaladığınız işlem kimliğiyle değiştirin. Bu komut, Java uygulamanızın 30 saniyelik profil oluşturucu `timed_recording_example.jfr` kaydını başlatacak ve `D:\home` dizinde adı geçen bir dosya oluşturur.

```
jcmd <pid> JFR.start name=TimedRecording settings=profile duration=30s filename="D:\home\timed_recording_example.JFR"
```

Daha fazla bilgi için lütfen [Jcmd Komut Başvurusu'na](https://docs.oracle.com/javacomponents/jmc-5-5/jfr-runtime-guide/comline.htm#JFRRT190)bakın.

#### <a name="analyze-jfr-files"></a>Dosyaları `.jfr` analiz et

JFR dosyanızı yerel makinenize indirmek için [FTPS'yi](deploy-ftp.md) kullanın. JFR dosyasını analiz etmek için Zulu Görev Kontrol'ü indirip [kurun.](https://www.azul.com/products/zulu-mission-control/) Zulu Görev Kontrol ile ilgili talimatlar [için, Azul belgelerine](https://docs.azul.com/zmc/) ve [kurulum talimatlarına](https://docs.microsoft.com/java/azure/jdk/java-jdk-flight-recorder-and-mission-control)bakın.

### <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

Daha fazla bilgi için [Bulut Kabuğu'ndaki Akış günlüklerine](troubleshoot-diagnostic-logs.md#in-cloud-shell)bakın.

### <a name="app-logging"></a>Uygulama günlüğü

Uygulamanızın standart konsol çıktısını ve standart konsol hata akışlarını yerel dosya sistemine veya Azure Blob Depolamasına yazmak üzere Uygulama Hizmetini yapılandırmak için Azure portalı veya [Azure CLI](/cli/azure/webapp/log#az-webapp-log-config) üzerinden [uygulama günlüğe kaydetmeyi](troubleshoot-diagnostic-logs.md#enable-application-logging-windows) etkinleştirin. Yerel Uygulama Hizmeti dosya sistemi örneğine günlüğe kaydetme, yapılandırıldıktan 12 saat sonra devre dışı bırakılır. Daha uzun bekletme gerekiyorsa, çıktıyı blob depolama kabına yazmak için uygulamayı yapılandırın. Java ve Tomcat uygulama günlükleriniz */LogFiles/Application/* dizininde bulunabilir.

Uygulamanız izleme için [Logback](https://logback.qos.ch/) veya [Log4j](https://logging.apache.org/log4j) kullanıyorsa, Bu izleri, [Uygulama Öngörüleri'ndeki Java izleme günlüklerini Keşfedin'deki](/azure/application-insights/app-insights-java-trace-logs)günlük çerçeve yapılandırma yönergelerini kullanarak gözden geçirilmeüzere Azure Uygulama Öngörüleri'ne iletebilirsiniz.


## <a name="customization-and-tuning"></a>Özelleştirme ve aetme

Azure Uygulama Hizmeti, Azure portalı ve CLI aracılığıyla kutu atonlama ve özelleştirme dışında destekler. Java'ya özgü olmayan web uygulaması yapılandırması için aşağıdaki makaleleri inceleyin:

- [Uygulama ayarlarını yapılandırma](configure-common.md#configure-app-settings)
- [Özel bir etki alanı ayarlama](app-service-web-tutorial-custom-domain.md)
- [TLS bağlamalarını yapılandırma](configure-ssl-bindings.md)
- [CDN ekle](../cdn/cdn-add-to-web-app.md)
- [Kudu sitesini yapılandırın](https://github.com/projectkudu/kudu/wiki/Configurable-settings)

### <a name="set-java-runtime-options"></a>Java çalışma zamanı seçeneklerini ayarlama

Ayrılmış bellek veya diğer JVM çalışma zamanı seçeneklerini ayarlamak `JAVA_OPTS` için, seçeneklerle birlikte bir [uygulama ayarı](configure-common.md#configure-app-settings) oluşturun. Uygulama Hizmeti, bu ayarı bir ortam değişkeni olarak java çalışma süresine geçirir.

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

### <a name="pre-compile-jsp-files"></a>JSP dosyalarını Önceden Derle

Tomcat uygulamalarının performansını artırmak için, Uygulama Hizmeti'ne dağıtmadan önce JSP dosyalarınızı derleyebilirsiniz. Apache Sling tarafından sağlanan [Maven eklentisini](https://sling.apache.org/components/jspc-maven-plugin/plugin-info.html) veya bu [Ant yapı dosyasını](https://tomcat.apache.org/tomcat-9.0-doc/jasper-howto.html#Web_Application_Compilation)kullanabilirsiniz.

## <a name="secure-applications"></a>Uygulamaları güvenli hale getirme

App Service'te çalışan Java uygulamaları, diğer uygulamalarla aynı [güvenlik en iyi uygulama](/azure/security/security-paas-applications-using-app-services) kümesine sahiptir.

### <a name="authenticate-users-easy-auth"></a>Kullanıcıları kimlik doğrulaması (Easy Auth)

Azure portalında uygulama kimlik doğrulamasını **Kimlik Doğrulama ve Yetkilendirme** seçeneğiyle ayarlayın. Buradan, Azure Active Directory'yi veya Facebook, Google veya GitHub gibi sosyal girişleri kullanarak kimlik doğrulamayı etkinleştirebilirsiniz. Azure portalı yapılandırması yalnızca tek bir kimlik doğrulama sağlayıcısını yapılandırırken çalışır. Daha fazla bilgi için Azure Active Directory oturumunu ve diğer kimlik sağlayıcıları için ilgili makaleleri [kullanmak için Uygulama Hizmeti uygulamanızı yapılandır'](configure-authentication-provider-aad.md) a bakın. Birden çok oturum açma sağlayıcısını etkinleştirmeniz gerekiyorsa, [Özel Servis kimlik doğrulama](app-service-authentication-how-to.md) makalesindeki yönergeleri izleyin.

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

### <a name="configure-tlsssl"></a>TLS/SSL yapılandırma

Varolan bir TLS/SSL sertifikasını yüklemek ve uygulamanızın etki alanı adına bağlamak için [Azure Uygulama Hizmeti'nde TLS bağlayıcısı olan özel bir DNS adını Güvenli'deki](configure-ssl-bindings.md) yönergeleri izleyin. Varsayılan olarak uygulamanız, HTTP bağlantılarının SSL ve TLS'yi uygulamak için öğreticideki belirli adımları izlemesine izin vermeye devam edecektir.

### <a name="use-keyvault-references"></a>KeyVault Referanslarını Kullanma

[Azure KeyVault,](../key-vault/general/overview.md) erişim ilkeleri ve denetim geçmişiyle merkezi leştirilmiş gizli yönetim sağlar. KeyVault'ta sırları (parolalar veya bağlantı dizeleri gibi) depolayabilir ve uygulamanızdaki bu sırlara ortam değişkenleri aracılığıyla erişebilirsiniz.

İlk olarak, [Uygulamanızın Key Vault'a erişimini sağlamak](app-service-key-vault-references.md#granting-your-app-access-to-key-vault) ve [Uygulama Ayarında sırrınıza KeyVault göndermesi yapmak](app-service-key-vault-references.md#reference-syntax)için talimatları izleyin. Uygulama Hizmeti terminaline uzaktan erişirken ortam değişkenini yazdırarak başvurunun gizliye çözüldüğünü doğrulayabilirsiniz.

Bahar veya Tomcat yapılandırma dosyanızda bu sırları enjekte etmek`${MY_ENV_VAR}`için, ortam değişkenenjeksiyon sözdizimini kullanın ( ). Yay yapılandırma dosyaları için lütfen [dışa uygun yapılandırmalarla](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)ilgili bu belgelere bakın.


## <a name="configure-apm-platforms"></a>APM platformlarını yapılandırma

Bu bölümde, Linux'ta Azure Uygulama Hizmeti'nde dağıtılan Java uygulamalarının NewRelic ve AppDynamics uygulama performans izleme (APM) platformlarına nasıl bağlanılacakları gösterilmektedir.

### <a name="configure-new-relic"></a>Yeni Emaneti Yapılandır

1. [NewRelic.com'da](https://newrelic.com/signup) Yeni Emanet hesabı oluşturma
2. NewRelic Java aracısını indirin, *newrelic-java-x.x.x.zip*benzer bir dosya adı olacaktır.
3. Lisans anahtarınızı kopyalayın, aracıyı daha sonra yapılandırmak için ihtiyacınız olacak.
4. Yeni bir dizin */ev/site/wwwroot/apm*oluşturmak için [Kudu konsolunu](https://github.com/projectkudu/kudu/wiki/Kudu-console) kullanın.
5. Paketlenmemiş Yeni Emanet Java aracısı dosyalarını */home/site/wwwroot/apm*altında bir dizine yükleyin. Aracınızın dosyaları */home/site/wwwroot/apm/newrelic*adresinde olmalıdır.
6. YAML dosyasını */home/site/wwwroot/apm/newrelic/newrelic.yml* adresindedeğiştirin ve yer tutucu lisans değerini kendi lisans anahtarınızla değiştirin.
7. Azure portalında, Uygulama Hizmeti'nde uygulamanıza göz atın ve yeni bir Uygulama Ayarı oluşturun.
    - Uygulamanız Java **SE**kullanıyorsa, değeri `JAVA_OPTS` `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`ile adlı bir ortam değişkeni oluşturun.
    - **Tomcat**kullanıyorsanız, değeri `CATALINA_OPTS` `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`ile adlı bir ortam değişkeni oluşturun.

### <a name="configure-appdynamics"></a>AppDynamics'i Yapılandır

1. [AppDynamics.com'da](https://www.appdynamics.com/community/register/) bir AppDynamics hesabı oluşturun
2. AppDynamics web sitesinden Java aracısını indirin, dosya adı *AppServerAgent-x.x.x.xxxxx.zip* benzer olacaktır
3. Yeni bir dizin */ev/site/wwwroot/apm*oluşturmak için [Kudu konsolunu](https://github.com/projectkudu/kudu/wiki/Kudu-console) kullanın.
4. Java aracısı dosyalarını */home/site/wwwroot/apm*altında bir dizine yükleyin. Aracınızın dosyaları */home/site/wwwroot/apm/appdynamics*adresinde olmalıdır.
5. Azure portalında, Uygulama Hizmeti'nde uygulamanıza göz atın ve yeni bir Uygulama Ayarı oluşturun.
    - **Java SE**kullanıyorsanız, Uygulama Hizmeti adınızın `JAVA_OPTS` olduğu `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` `<app-name>` değeri içeren bir ortam değişkeni oluşturun.
    - **Tomcat**kullanıyorsanız, Uygulama Hizmeti adınızın `CATALINA_OPTS` olduğu `<app-name>` `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>` değeri içeren bir ortam değişkeni oluşturun.

>  Zaten bir `JAVA_OPTS` ortam değişkeninvarsa `CATALINA_OPTS`veya geçerli `-javaagent:/...` değerin sonuna seçeneği ekleyin.

## <a name="data-sources"></a>Veri kaynakları

### <a name="tomcat"></a>Tomcat

Bu yönergeler tüm veritabanı bağlantıları için geçerlidir. Yer tutucuları seçtiğiniz veritabanının sürücü sınıfı adı ve JAR dosyasıyla doldurmanız gerekir. Sağlanan sınıf adları ve ortak veritabanları için sürücü indirme içeren bir tablodur.

| Veritabanı   | Sürücü Sınıf Adı                             | JDBC Sürücüsü                                                                      |
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

#### <a name="finalize-configuration"></a>Yapılandırmayı sonuçlandırın

Son olarak, sürücü JARs'i Tomcat classpath'e yerleştireceğiz ve Uygulama Hizmetinizi yeniden başlatacağız. JDBC sürücü dosyalarının */home/tomcat/lib* dizinine yerleştirerek Tomcat sınıf yükleyicisinin kullanımına sunulduğundan emin olun. (Zaten yoksa bu dizini oluşturun.) Bu dosyaları Uygulama Hizmeti örneğinize yüklemek için aşağıdaki adımları gerçekleştirin:

1. Bulut [Kabuğu'nda](https://shell.azure.com)webapp uzantısını yükleyin:

    ```azurecli-interactive
    az extension add -–name webapp
    ```

2. Yerel sisteminizden Uygulama Hizmeti'ne bir SSH tüneli oluşturmak için aşağıdaki CLI komutunu çalıştırın:

    ```azurecli-interactive
    az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
    ```

3. SFTP istemcinizle yerel tünel bağlantı noktasına bağlanın ve dosyaları */home/tomcat/lib* klasörüne yükleyin.

Alternatif olarak, JDBC sürücüsünü yüklemek için bir FTP istemcisi kullanabilirsiniz. [FTP kimlik bilgilerinizi almak için](deploy-configure-credentials.md)aşağıdaki yönergeleri izleyin.

## <a name="configuring-tomcat"></a>Tomcat'ı Yapılandırma

Tomcat'ın veya `server.xml` diğer yapılandırma dosyalarını düzenlemek için öncelikle portaldaki Tomcat ana sürümünüze bir not alın.

1. Komutu çalıştırarak sürümünüz için Tomcat `env` ev dizinini bulun. Ana sürümünüzle başlayan `AZURE_TOMCAT`ve eşleşen ortam değişkenini arayın. Örneğin, `AZURE_TOMCAT85_HOME` Tomcat 8.5 için Tomcat dizinine işaret.
1. Sürümünüz için Tomcat ev dizini belirledikten sonra yapılandırma `D:\home`dizini 'ne kopyalayın. Örneğin, bir `AZURE_TOMCAT85_HOME` değeri `D:\Program Files (x86)\apache-tomcat-8.5.37`olsaydı, kopyalanan dizinin yeni yolu . `D:\home\apache-tomcat-8.5.37`

Son olarak, Uygulama Hizmetinizi yeniden başlatın. Dağıtımlarınız eskisi `D:\home\site\wwwroot\webapps` gibi yapılmalıdır.

## <a name="configure-java-se"></a>Java SE'yi yapılandır

Bir çalıştırırken . Windows'da Java SE'deki JAR uygulaması, `server.port` uygulamanız başlarken komut satırı seçeneği olarak geçirilir. HTTP bağlantı noktasını ortam değişkeninden el `HTTP_PLATFORM_PORT`ile çözebilirsiniz. Bu ortam değişkeninin değeri, uygulamanızın dinlemesi gereken HTTP bağlantı noktası olacaktır. 

## <a name="java-runtime-statement-of-support"></a>Java çalışma zamanı destek bildirimi

### <a name="jdk-versions-and-maintenance"></a>JDK versiyonları ve bakımı

Azure'un desteklenen Java Geliştirme Kiti (JDK), [Azul Systems](https://www.azul.com/)aracılığıyla sağlanan [Zulu'dur.](https://www.azul.com/downloads/azure-only/zulu/)

Windows için Azure Uygulama Hizmeti'ndeki yeni çalışma zamanı seçenekleri yle ana sürüm güncelleştirmeleri sağlanacaktır. Müşteriler, Uygulama Hizmeti dağıtımlarını yapılandırarak Java'nın bu yeni sürümlerini günceller ve büyük güncelleştirmenin gereksinimlerini karşılamasını test etmek ve sağlamaktan sorumludur.

Desteklenen JDK'lar her yılın Ocak, Nisan, Temmuz ve Ekim aylarında üç ayda bir otomatik olarak yamalı olur. Azure'da Java hakkında daha fazla bilgi için lütfen [bu destek belgesine](https://docs.microsoft.com/azure/java/jdk/)bakın.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Büyük güvenlik açıklarına yönelik düzeltmeler ve düzeltmeler, Azul Systems'den kullanıma sunulduğu anda piyasaya sürülecek. "Büyük" bir güvenlik [açığı, NIST Ortak Güvenlik Açığı Puanlama Sistemi,sürüm 2'de](https://nvd.nist.gov/cvss.cfm)9,0 veya daha yüksek bir taban puanla tanımlanır.

Tomcat 8.0 [30 Eylül 2018 itibariyle Yaşam Sonu (EOL)](https://tomcat.apache.org/tomcat-80-eol.html)ulaştı. Azure Uygulama Hizmeti'nde çalışma süresi hala kullanılabilir olsa da, Azure Güvenlik güncelleştirmelerini Tomcat 8.0'a uygulamaz. Mümkünse, başvurularınızı Tomcat 8.5 veya 9.0'a geçirin. Hem Tomcat 8.5 hem de 9.0 Azure Uygulama Hizmeti'nde kullanılabilir. Daha fazla bilgi için [resmi Tomcat sitesine](https://tomcat.apache.org/whichversion.html) bakın. 

### <a name="deprecation-and-retirement"></a>Amortisman ve emeklilik

Desteklenen bir Java çalışma süresi kullanımdan kaldırılacaksa, etkilenen çalışma süresini kullanan Azure geliştiricilere çalışma süresinin kullanım süresinin kullanımdan kaldırılmasına en az altı ay kala bir amortisman bildirimi verilir.

### <a name="local-development"></a>Yerel geliştirme

Geliştiriciler [Azul'un indirme sitesinden](https://www.azul.com/downloads/azure-only/zulu/)yerel kalkınma için Azul Zulu Enterprise JDK Üretim Sürümü indirebilirsiniz.

### <a name="development-support"></a>Geliştirme desteği

Azure destekli [Azul Zulu JDK](https://www.azul.com/downloads/azure-only/zulu/) için ürün desteği, Azure veya [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) için nitelikli bir Azure destek [planı](https://azure.microsoft.com/support/plans/)yla geliştirilirken Microsoft aracılığıyla kullanılabilir.

### <a name="runtime-support"></a>Çalışma zamanı desteği

Geliştiriciler, nitelikli bir [destek planları](https://azure.microsoft.com/support/plans/)varsa Azure Desteği aracılığıyla Azul Zulu JDK'larla ilgili bir [sorun açabilirler.](/azure/azure-portal/supportability/how-to-create-azure-support-request)

## <a name="next-steps"></a>Sonraki adımlar

Bu konu, Windows'taki Azure Uygulama Hizmeti için Java Runtime destek bildirimini sağlar.

- Azure Uygulama Hizmeti ile web uygulamalarını barındırma hakkında daha fazla bilgi edinmek için [Uygulama Hizmetine genel bakış](overview.md)alabakın.
- Azure geliştirme de Java hakkında daha fazla bilgi [için Java Geliştirme Merkezi için Azure'a](https://docs.microsoft.com/java/azure/?view=azure-java-stable)bakın.
