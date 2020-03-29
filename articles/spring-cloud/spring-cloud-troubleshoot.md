---
title: Azure Bahar Bulutu için sorun giderme kılavuzu | Microsoft Dokümanlar
description: Azure Bahar Bulutu için sorun giderme kılavuzu
author: bmitchell287
ms.service: spring-cloud
ms.topic: troubleshooting
ms.date: 11/04/2019
ms.author: brendm
ms.openlocfilehash: 5dcdb03a6d4ec4f448108dbd771a44f362aa7f20
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277576"
---
# <a name="troubleshoot-common-azure-spring-cloud-issues"></a>Sık karşılaşılan Azure İlkbahar Bulutu sorunlarını giderme

Bu makalede, Sorun Giderme Azure İlkbahar Bulut geliştirme sorunları için yönergeler sağlar. Daha fazla bilgi için Azure [İlkbahar Bulut SSS'sine](spring-cloud-faq.md)bakın.

## <a name="availability-performance-and-application-issues"></a>Kullanılabilirlik, performans ve uygulama sorunları

### <a name="my-application-cant-start-for-example-the-endpoint-cant-be-connected-or-it-returns-a-502-after-a-few-retries"></a>Uygulamam başlatılamaz (örneğin, bitiş noktası bağlanamıyor veya birkaç denemeden sonra 502 döndürür)

Günlükleri Azure Log Analytics'e aktarın. Bahar uygulama günlükleri için tablo *AppPlatformLogsforSpring*adlandırılır. Daha fazla bilgi edinmek için [tanılama ayarlarıyla günlükleri ve ölçümleri analiz etme'ye](diagnostic-services.md)bakın.

Günlüklerinizde aşağıdaki hata iletisi görünebilir:

> "org.springframework.context.ApplicationContextException: Web sunucusu başlatılamıyor"

İleti, olası iki sorundan birini gösterir: 
* Fasulyelerden biri ya da bağımlılıklarından biri eksik.
* Çekirdek özelliklerinden biri eksik veya geçersiz. Bu durumda, "java.lang.IllegalArgumentException" büyük olasılıkla görüntülenir.

Hizmet bağlamaları da uygulama başlatma hatalarına neden olabilir. Günlükleri sorgulamak için, bağlı hizmetlerle ilgili anahtar kelimeleri kullanın. Örneğin, uygulamanızın yerel sistem saatine ayarlanmış bir MySQL örneğine bağlanması olduğunu varsayalım. Uygulama başlatılamazsa, günlükte aşağıdaki hata iletisi görünebilir:

> "java.sql.SQLException: sunucu saat dilimi değeri 'Eşgüdümlü Evrensel Saat' tanınmayan veya birden fazla saat dilimini temsil eder."

Bu hatayı düzeltmek için `server parameters` MySQL örneğinizin bölümüne `time_zone` gidin ve değeri *System'den* *+0:00'a*değiştirin.


### <a name="my-application-crashes-or-throws-an-unexpected-error"></a>Uygulamam kilitleniyor veya beklenmeyen bir hata oluşturuyor

Uygulama hata hata sını yaparken, uygulamanın çalışma durumunu ve bulma durumunu denetleyerek başlayın. Bunu yapmak için, tüm uygulamaların durumunun _Çalışır_ ve _GÜNCEL_olduğundan emin olmak için Azure portalındaki _Uygulama yönetimine_ gidin.

* Durum _Çalışıyorsa_ ancak bulma durumu _UP_değilse, ["Başvurum kaydedilemez"](#my-application-cant-be-registered) bölümüne gidin.

* Bulma durumu _UP_ise, uygulamanın durumunu denetlemek için Ölçümler'e gidin. Aşağıdaki ölçümleri inceleyin:


  - `TomcatErrorCount`(_tomcat.global.error_): Tüm Bahar uygulama özel durumları burada sayılır. Bu numara büyükse, uygulama günlüklerinizi incelemek için Azure Log Analytics'e gidin.

  - `AppMemoryMax`(_jvm.memory.max_): Uygulama için kullanılabilen maksimum bellek miktarıdır. Tutar tanımlanmamış olabilir veya tanımlanırsa zaman içinde değişebilir. Tanımlanmışsa, kullanılan ve işlenen bellek miktarı her zaman maksimumdan daha az veya eşittir. Ancak, bir bellek ayırma, `OutOfMemoryError` *kullanılan*> taahhüt edilen belleği artırmaya çalışırsa , *<= max hala* doğru olsa bile, bir ileti ile başarısız olabilir. Böyle bir durumda, `-Xmx` parametre yi kullanarak maksimum yığın boyutunu artırmaya çalışın.

  - `AppMemoryUsed`(_jvm.memory.used_): Uygulama tarafından şu anda kullanılan baytlarda bellek miktarı. Normal bir yük Java uygulaması için, bu metrik seri, bellek kullanımının giderek arttığı ve küçük artışlarla azaldığı ve aniden çok düştüğü ve sonra desenin yinelendiği bir *testere dişi* deseni oluşturur. Bu metrik seri, toplama eylemlerinin testere dişi desenindeki düşüşleri temsil ettiği Java sanal makinesindeki çöp toplama nedeniyle oluşur.
    
    Bu metrik, bellek sorunlarını belirlemeye yardımcı olmak için önemlidir:
    * En başta bir hafıza patlaması.
    * Belirli bir mantık yolu için dalgalanma bellek ayırma.
    * Kademeli hafıza sızıntıları.

  Daha fazla bilgi için [Bkz. Ölçümler.](spring-cloud-concept-metrics.md)

Azure Günlük Analitiği hakkında daha fazla bilgi edinmek için Azure [Monitor'da Günlük Analitiği'ne başlayın](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)bölümüne bakın.

### <a name="my-application-experiences-high-cpu-usage-or-high-memory-usage"></a>Uygulamam yüksek CPU kullanımı veya yüksek bellek kullanımıyla karşılaşıyor

Uygulamanız yüksek CPU veya bellek kullanımı yla karşınızdaysa, iki şeyden biri doğrudur:
* Tüm uygulama örnekleri yüksek CPU veya bellek kullanımı deneyim.
* Bazı uygulama örnekleri nde yüksek CPU veya bellek kullanımı yaşanmaktadır.

Hangi durumun geçerli olduğunu belirlemek için aşağıdakileri yapın:

1. **Ölçümler'e**gidin ve ardından **Service CPU Kullanım Yüzdesi** veya Kullanılan **Hizmet Belleği'ni**seçin.
2. Hangi uygulamayı izlemek istediğinizi belirtmek için bir **Uygulama=** filtresi ekleyin.
3. Ölçümleri **Örne**göre böl.

*Tüm örneklerde* yüksek CPU veya bellek kullanımı yaşanıyorsa, uygulamayı ölçeklendirmeniz veya CPU veya bellek kullanımını ölçeklendirmeniz gerekir. Daha fazla bilgi için [Bkz. Öğretici: Azure Bahar Bulutu'nda bir uygulamayı ölçeklendirin.](spring-cloud-tutorial-scale-manual.md)

*Bazı örneklerde* yüksek CPU veya bellek kullanımı yaşanıyorsa, örnek durumunu ve bulma durumunu denetleyin.

Daha fazla bilgi [için Azure İlkbahar Bulutu Ölçümleri'ne](spring-cloud-concept-metrics.md)bakın.

Tüm örnekler çalışır durumdaysa, uygulama günlüklerinizi sorgulamak ve kod mantığınızı gözden geçirmek için Azure Log Analytics'e gidin. Bu, herhangi birinin ölçek bölümleme sini etkileyip etkilemediğini görmenize yardımcı olur. Daha fazla bilgi için [bkz.](diagnostic-services.md)

Azure Günlük Analitiği hakkında daha fazla bilgi edinmek için Azure [Monitor'da Günlük Analitiği'ne başlayın](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)bölümüne bakın. [Kusto sorgu dilini](https://docs.microsoft.com/azure/kusto/query/)kullanarak günlükleri sorgula.

### <a name="checklist-for-deploying-your-spring-application-to-azure-spring-cloud"></a>Bahar uygulamanızı Azure Bahar Bulutu'na dağıtmak için denetim listesi

Başvurunuza binmeden önce aşağıdaki ölçütleri karşıladığından emin olun:

* Uygulama belirtilen Java çalışma zamanı sürümü ile yerel olarak çalıştırılabilir.
* Ortam config (CPU / RAM / Örnekler) uygulama sağlayıcısı tarafından belirlenen minimum gereksinimi karşılar.
* Yapılandırma öğelerinin beklenen değerleri vardır. Daha fazla bilgi için [Config Server'a](spring-cloud-tutorial-config-server.md)bakın.
* Ortam değişkenlerinin beklenen değerleri vardır.
* JVM parametreleri beklenen değerlere sahiptir.
* Katıştırılmış _Config Server_ ve _Yay Hizmet Kayıt Defteri_ hizmetlerini uygulama paketinden devre dışı kaldırmanızı veya kaldırmanızı tavsiye ettik.
* _Hizmet Bağlama_ aracılığıyla bağlanacak Azure kaynakları varsa, hedef kaynakların çalıştığından emin olun.

## <a name="configuration-and-management"></a>Yapılandırma ve Yönetim

### <a name="i-encountered-a-problem-with-creating-an-azure-spring-cloud-service-instance"></a>Azure İlkbahar Bulutu hizmeti örneği oluşturmayla ilgili bir sorunla karşılaştım

Azure portalını kullanarak bir Azure İlkbahar Bulutu hizmeti örneği ayarladığınızda, Azure İlkbahar Bulutu doğrulamayı sizin yeriniz için gerçekleştirir.

Ancak [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) veya [Azure Kaynak Yöneticisi şablonu](https://docs.microsoft.com/azure/azure-resource-manager/)kullanarak Azure İlkbahar Bulutu hizmeti örneğini ayarlamaya çalışırsanız, aşağıdakileri doğrulayın:

* Abonelik etkindir.
* Konum Azure İlkbahar Bulutu tarafından [desteklenir.](spring-cloud-faq.md)
* Örnek için kaynak grubu zaten oluşturulur.
* Kaynak adı adlandırma kuralına uygundur. Yalnızca küçük harfler, sayılar ve tireler içermelidir. İlk karakter bir harf olmalıdır. Son karakter bir harf veya sayı olmalıdır. Değer 2 ila 32 karakter içermelidir.

Kaynak Yöneticisi şablonu kullanarak Azure İlkbahar Bulutu hizmet örneğini ayarlamak istiyorsanız, öncelikle [Azure Kaynak Yöneticisi şablonlarının yapısını ve sözdizimini anlayın'a](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)bakın.

Azure İlkbahar Bulutu hizmet örneğinin adı altında bir alt `azureapps.io`etki alanı adı istemek için kullanılır, böylece ad varolan bir adla çakışıyorsa kurulum başarısız olur. Etkinlik günlüklerinde daha fazla ayrıntı bulabilirsiniz.

### <a name="i-cant-deploy-a-jar-package"></a>JAR paketi dağıtamıyorum

Azure portalını veya Kaynak Yöneticisi şablonu kullanarak Java Archive dosyasını (JAR)/kaynak paketi yükleyemezsiniz.

Uygulama paketinizi [Azure CLI'yi](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)kullanarak dağıttığınızda, Azure CLI dağıtım ilerlemesini düzenli aralıklarla yoklar ve sonunda dağıtım sonucunu görüntüler.

Yoklama kesintiye uğrarsa, yine de şu komutu kullanarak dağıtım günlüklerini getirebilirsiniz:

`az spring-cloud app show-deploy-log -n <app-name>`

Uygulamanızın doğru [yürütülebilir JAR biçiminde](https://docs.spring.io/spring-boot/docs/current/reference/html/executable-jar.html)paketlendirilmesini sağlayın. Doğru şekilde paketlenmemişse, aşağıdakilere benzer bir hata iletisi alırsınız:

> "Hata: Geçersiz veya bozuk jarfile /jar/38bc8ea1-a6bb-4736-8e93-e8f3b52c8714"

### <a name="i-cant-deploy-a-source-package"></a>Kaynak paketi dağıtamıyorum

Azure portalını veya Kaynak Yöneticisi şablonu kullanarak JAR/kaynak paketi yükleyemezsiniz.

Uygulama paketinizi [Azure CLI'yi](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)kullanarak dağıttığınızda, Azure CLI dağıtım ilerlemesini düzenli aralıklarla yoklar ve sonunda dağıtım sonucunu görüntüler.

Yoklama kesintiye uğrarsa, yine de şu komutu kullanarak derleme ve dağıtım günlüklerini getirebilirsiniz:

`az spring-cloud app show-deploy-log -n <app-name>`

Ancak, bir Azure İlkbahar Bulutu hizmet örneğinin aynı anda tek bir kaynak paket için yalnızca bir yapı işini tetikleyebilir. Daha fazla bilgi için [bkz.](spring-cloud-quickstart-launch-app-portal.md) [Set up a staging environment in Azure Spring Cloud](spring-cloud-howto-staging-environment.md)

### <a name="my-application-cant-be-registered"></a>Başvurum kaydedilemez

Çoğu durumda, *Gerekli Bağımlılıklar* ve *Hizmet Bulma* Proje Nesnesi Modeli (POM) dosyanızda düzgün şekilde yapılandırılmtığında bu durum oluşur. Yapılandırıldıktan sonra yerleşik Hizmet Kayıt Defteri sunucu bitiş noktası, uygulamanızla birlikte bir ortam değişkeni olarak enjekte edilir. Uygulamalar daha sonra Kendilerini Service Registry sunucusuna kaydeder ve diğer bağımlı mikro hizmetleri keşfeder.

Yeni kaydedilmiş bir örneğin trafik almaya başlamasından önce en az iki dakika bekleyin.

Mevcut Bir Bahar Bulutu tabanlı çözümü Azure'a taşıyorsanız, Azure Bahar Bulutu tarafından sağlanan yönetilen örneklerle çakışmamak için özel _Hizmet Kayıt Defteri_ nizin ve _Config Server_ örneklerinizin kaldırıldığından (veya devre dışı bırakıldığından) emin olun.

Azure Günlük Analizi'ndeki _Hizmet Kayıt Defteri_ istemci günlüklerini de kontrol edebilirsiniz. Daha fazla bilgi için [bkz.](diagnostic-services.md)

Azure Günlük Analitiği hakkında daha fazla bilgi edinmek için Azure [Monitor'da Günlük Analitiği'ne başlayın](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)bölümüne bakın. [Kusto sorgu dilini](https://docs.microsoft.com/azure/kusto/query/)kullanarak günlükleri sorgula.

### <a name="i-want-to-inspect-my-applications-environment-variables"></a>Uygulamamın ortam değişkenlerini incelemek istiyorum

Ortam değişkenleri Azure Bulut çerçevesini bilgilendirerek Azure'un uygulamanızı oluşturan hizmetleri nerede ve nasıl yapılandıracağı konusunda anlamasını sağlar. Ortam değişkenlerinizin doğru olduğundan emin olmak, olası sorunları gidermede gerekli bir ilk adımdır.  Çevre değişkenlerinizi gözden geçirmek için Bahar Önyükleme Aktüatör uç noktasını kullanabilirsiniz.  

> [!WARNING]
> Bu yordam, test bitiş noktanızı kullanarak ortam değişkenlerinizi ortaya çıkarır.  Test bitiş noktanız herkese açıksa veya uygulamanız için bir etki alanı adı atadıysanız devam etmeyin.

1. `https://<your application test endpoint>/actuator/health` kısmına gidin.  
    - Benzer bir `{"status":"UP"}` yanıt bitiş noktasının etkin olduğunu gösterir.
    - Yanıt negatifse, *POM.xml* dosyanıza aşağıdaki bağımlılıkları ekleyin:

        ```xml
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-actuator</artifactId>
            </dependency>
        ```

1. Bahar Önyükleme Aktüatör son noktası etkinleştirildiğinde, Azure portalına gidin ve uygulamanızın yapılandırma sayfasını arayın.  Ad `MANAGEMENT_ENDPOINTS_WEB_EXPOSURE_INCLUDE` ve değer `*` içeren bir ortam değişkeni ekleyin. 

1. Başvurunuzu yeniden başlatın.

1. Yanıtı `https://<your application test endpoint>/actuator/env` inceleyin.  Şu şekilde görünmelidir:

    ```json
    {
        "activeProfiles": [],
        "propertySources": {,
            "name": "server.ports",
            "properties": {
                "local.server.port": {
                    "value": 1025
                }
            }
        }
    }
    ```

Adlı `systemEnvironment`alt düğüm arayın.  Bu düğüm, uygulamanızın ortam değişkenlerini içerir.

> [!IMPORTANT]
> Uygulamanızı herkese açık hale getirmeden önce ortam değişkenlerinizin maruz kalmasını tersine çevirmeyi unutmayın.  Azure portalına gidin, uygulamanızın yapılandırma sayfasını arayın ve bu `MANAGEMENT_ENDPOINTS_WEB_EXPOSURE_INCLUDE`ortam değişkenini silin: .

### <a name="i-cant-find-metrics-or-logs-for-my-application"></a>UygulamamIçin ölçümleri veya günlükleri bulamıyorum

Uygulama durumlarının _Çalışma_ ve _UP_olduğundan emin olmak için **Uygulama yönetimine** gidin.

_JVM'den_ ölçümler görebiliyor ancak _Tomcat'tan_ölçüm yoksa, `spring-boot-actuator` uygulama paketinizde bağımlılığın etkin olup olmadığını ve başarılı bir şekilde önyükleme yapıp olmadığını denetleyin.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Uygulama günlükleriniz bir depolama hesabına arşivlenebilir ancak Azure Günlük Analizi'ne gönderilmezse, [çalışma alanınızı doğru ayarlayıp ayarlamadığınızı](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)kontrol edin. Ücretsiz bir Azure Günlük Analizi katmanı kullanıyorsanız, [ücretsiz katmanın hizmet düzeyi sözleşmesi (SLA) sağlamadığını](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_3/)unutmayın.
