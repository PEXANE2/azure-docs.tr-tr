---
title: Azure yay bulutu için sorun giderme kılavuzu | Microsoft Docs
description: Azure yay bulutu için sorun giderme kılavuzu
author: jpconnock
ms.service: spring-cloud
ms.topic: troubleshooting
ms.date: 11/04/2019
ms.author: jeconnoc
ms.openlocfilehash: 9603f4a687b55f45be2875ccaa7b801c0c5589c9
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607621"
---
# <a name="troubleshooting-guide-for-common-problems"></a>Sık karşılaşılan sorunlar için sorun giderme kılavuzu

Bu makalede, Azure yay bulutu 'nda çalışan geliştiriciler için bazı yaygın sorunlar ve sorun giderme adımları ayrıntılı olarak açıklanır. Ayrıca, [SSS makalemizi](spring-cloud-faq.md)okumanızı öneririz.

## <a name="availability-performance-and-application-issues"></a>Kullanılabilirlik, performans ve uygulama sorunları

### <a name="my-application-cannot-start-for-example-the-endpoint-cannot-be-connected-or-returns-502-after-few-retries"></a>Uygulamam başlatılamıyor (örneğin, uç nokta bağlanamaz veya birkaç yeniden denemeden sonra 502 değerini geri yükleyemezsiniz)

Günlükleri _Azure Log Analytics_dışarı aktarın. Spring uygulama günlüklerinin tablosu `AppPlatformLogsforSpring` olarak adlandırılır. Daha fazla bilgi edinmek için lütfen [Tanılama ayarlarıyla günlükleri ve ölçümleri](diagnostic-services.md) ziyaret edin

Günlüklerinizi aşağıdaki hatanın bulunması olası iki sorunlardan birini gösterir:

`org.springframework.context.ApplicationContextException: Unable to start web server`

* Beans veya bağımlılıklarından biri eksik.
* Çekirdek özelliklerinden biri eksik veya geçersiz. Büyük olasılıkla bu durumda `java.lang.IllegalArgumentException` görürsünüz.

Hizmet bağlamaları uygulama başlatma hatalarıyla da neden olabilir. Günlükleri sorgulamak için bağlı hizmetlerle ilgili anahtar sözcükleri kullanın.  Örneğin, uygulamanızın bir MySQL örneğine yerel sistem zamanına ayarlanmış bir bağlaması olduğunu varsayalım. Uygulama başlatılamazsa, günlükte şu hatayı bulabilirsiniz:

`java.sql.SQLException: The server time zone value 'Coordinated Universal Time' is unrecognized or represents more than one time zone.`

Bu hatayı onarmak için MySql örneğinizin `server parameters` gidin ve `time_zone` `SYSTEM` 'den `+0:00`'ye değiştirin.


### <a name="my-application-crashes-or-throws-an-unexpected-error"></a>Uygulamam kilitleniyor veya beklenmeyen bir hata oluşturuyor

Uygulama Kilitlenmelerinde hata ayıklarken, uygulamanın çalışma durumunu ve bulma durumunu denetleyerek başlatın. Tüm uygulamaların _çalışıyor ve çalışır_ durumda olduğundan emin olmak Için Azure Portal _uygulama yönetimi_ _' ne_gidin.

* Durum _çalışıyor_ ancak bulma durumu _güncel_değilse [uygulamama](#my-application-cannot-be-registered)git kaydedilemez.

* Bulma durumu _çalışıyorsa, uygulamanın_sistem durumunu denetlemek için _ölçümler_ ' e gidin. Aşağıdaki ölçümleri inceleyin:


  - `TomcatErrorCount` (_Tomcat. Global. Error_): tüm Spring Application özel durumları burada sayılır. Bu sayı büyükse, uygulama günlüklerinizi incelemek için _Azure Log Analytics_ 'ye gidin.

  - `AppMemoryMax` (_JVM. Memory. Max_): uygulama için kullanılabilir maksimum bellek miktarı. Tanımsız olabilir veya tanımlanmışsa zaman içinde değişebilir. Tanımlandıysa, kullanılan ve kaydedilen bellek miktarı her zaman en yüksek değerden az veya buna eşit olmalıdır. Öte yandan kullanılan > kullanılmış olsa bile kaydedilen <= en yüksek ifadesinin hala doğru olmasını sağlayacak şekilde kullanılan belleği artırmayı deneyen bir bellek ayırma işlemi `OutOfMemoryError` ile başarısız olabilir. Böyle bir durumda `-Xmx` parametresiyle en büyük yığın boyutunu artırmayı deneyin.

  - `AppMemoryUsed` (_JVM. Memory. kullanılan_): uygulama tarafından şu anda kullanılmakta olan bellek miktarı (bayt cinsinden). Normal yüklü bir Java uygulamasında bu ölçüm serisi 'testere dişi' deseni oluşturur; bellek kullanımı istikrarlı bir şekilde artar, küçük miktarlarda azalır, sonra hızla büyük miktarda düşer ve bu desen yinelenir. Bunun nedeni, Java sanal makinesi içindeki çöp toplama işleminin, koleksiyon eylemlerinin ' sawteeth ' üzerinde düşleri temsil ettiği yerdir.
    Bu ölçüm, aşağıdaki gibi bellek sorunlarını tanımlamak için önemlidir: * belirli bir mantık yolu için en fazla * bellek patlaması * aşamalı bellek sızıntıları

  Daha fazla ayrıntı için lütfen [ölçümleri](spring-cloud-concept-metrics.md)ziyaret edin.

_Azure Log Analytics_kullanmaya başlamak için [Bu başlangıç makalesini](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) ziyaret edin.

### <a name="my-application-experiences-high-cpu-usage-or-high-memory-usage"></a>Uygulamam yüksek CPU kullanımı veya yüksek bellek kullanımıyla karşılaşıyor

Uygulamanız yüksek CPU/bellek kullanımı yaşıyorsa, iki işlemlerden biri doğrudur:
* Tüm uygulama örnekleri yüksek CPU/bellek kullanımı deneyimi veya
* Bazı uygulama örnekleri yüksek CPU/bellek kullanımı yaşar.

Hangi durumun olduğunu onaylamak için,

1. _Ölçümler_’e gidin ve `Service CPU Usage Percentage` veya `Service Memory Used` öğesini seçin.
2. İzlemek istediğiniz uygulamayı belirtmek için `App=` filtresini ekleyin ve
3. Ölçümleri `Instance`göre ayırın.

Tüm örnekler yüksek CPU/bellek yaşıyorsa, uygulamayı ölçeklendirmeniz veya CPU/bellek ölçeğini ölçeklendirmeniz gerekir. Daha fazla ayrıntı için lütfen [Ölçek uygulamalarını](spring-cloud-tutorial-scale-manual.md) ziyaret edin

Örneklerden bazıları yüksek CPU/bellek yaşıyorsa, örnek durumunu ve bulma durumunu denetleyin.

Daha fazla ayrıntı için [ölçümleri](spring-cloud-concept-metrics.md)ziyaret edin.

Tüm örnekler çalışıyor ve çalışıyorsa, uygulama günlüklerinizi sorgulamak için _Azure Log Analytics_ 'ye gidin ve bunlardan herhangi birinin ölçek Bölümlendirmeyi etkileyebileceğini görmek için kod mantığınızı gözden geçirin. Daha ayrıntılı bilgi için bkz. [Tanılama ayarlarıyla günlükleri ve ölçümleri analiz](diagnostic-services.md)edin.

_Azure Log Analytics_kullanmaya başlamak için [Bu başlangıç makalesini](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) ziyaret edin. [Kusto sorgu dilini](https://docs.microsoft.com/azure/kusto/query/)kullanarak günlükleri sorgulayın.

### <a name="checklist-before-onboarding-your-spring-application-to-azure-spring-cloud"></a>Spring uygulamanızı Azure yay bulutuna eklemeden önce yapılacaklar listesi

* Uygulama, belirtilen Java çalışma zamanı sürümüyle yerel olarak çalışabilir.
* Ortam yapılandırması (CPU/RAM/örnekler), uygulama sağlayıcısı tarafından ayarlanan en düşük gereksinimleri karşılar.
* Yapılandırma öğelerinin değerleri beklenene uyuyor. Daha fazla bilgi için bkz. [config Server](spring-cloud-tutorial-config-server.md).
* Ortam değişkenlerinin beklenen değerleri var.
* JVM parametrelerinin beklenen değerleri var.
* Katıştırılmış _yapılandırma sunucusu_ ve _Spring Service kayıt defteri_ hizmetini uygulama paketinden devre dışı bırakıp kaldırmanız önerilir.
* _Hizmet Bağlama_ aracılığıyla bağlanacak Azure kaynakları varsa, hedef kaynakların çalıştığından emin olun.

## <a name="configuration-and-management"></a>Yapılandırma ve Yönetim

### <a name="i-encountered-a-problem-creating-an-azure-spring-cloud-service-instance"></a>Azure yay bulut hizmeti örneğini oluştururken bir sorunla karşılaştım

Portal aracılığıyla bir _Azure yay bulut_ hizmeti örneği sağlamaya çalıştığınızda, Azure Spring Cloud sizin için doğrulama işlemi yapar.

Bununla birlikte, Azure [CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) veya [Kaynak Yöneticisi şablonu](https://docs.microsoft.com/azure/azure-resource-manager/)aracılığıyla _Azure Spring bulut_ hizmeti örneğini sağlamaya çalışırsanız şunları doğrulayın:

* Abonelik etkin.
* Konum, _Azure Spring Cloud_tarafından [desteklenir](spring-cloud-faq.md) .
* Örnek için kaynak grubu zaten oluşturulmuş.
* Kaynak adı adlandırma kuralına uyar. (Bu, yalnızca küçük harf, sayı ve kısa çizgi içerebilir. İlk karakter bir harf olmalıdır. Son karakter bir harf veya sayı olmalıdır. Değer, 2 ila 32 karakter uzunluğunda olmalıdır.)

_Azure yay bulut_ hizmeti örneğini Kaynak Yöneticisi şablonuyla sağlamaya çalışırsanız, şablon söz dizimini denetlemek için lütfen https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates ziyaret edin.

_Azure Spring Cloud_ Service örneğinin adı, `azureapps.io`altında bir alt etki alanı adı istemek için kullanılır, bu nedenle ad var olan bir adla çakışıyorsa sağlama başarısız olur. Etkinlik günlüklerinde diğer ayrıntıları bulabilirsiniz.

### <a name="i-cannot-deploy-a-jar-package"></a>JAR paketini dağıtamıyorum

Portal veya Kaynak Yöneticisi şablonu aracılığıyla JAR/kaynak paketini karşıya yükleyemezsiniz.

[Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)kullanarak uygulama paketinizi dağıttığınızda, dağıtım ilerlemesini düzenli olarak yoklayacak ve sonunda dağıtım sonucunu gösterecektir.

Yoklama kesintiye uğrarsa, yine de şu komutu kullanarak dağıtım günlüklerini getirebilirsiniz:

`az spring-cloud app show-deploy-log -n <app-name>`

Uygulamanızın doğru [yürütülebilir jar biçiminde](https://docs.spring.io/spring-boot/docs/current/reference/html/executable-jar.html)paketlendiğinden emin olun. Değilse, aşağıdakine benzer bir hatayla karşılaşırsınız:

`Error: Invalid or corrupt jarfile /jar/38bc8ea1-a6bb-4736-8e93-e8f3b52c8714`

### <a name="i-cannot-deploy-a-source-package"></a>Kaynak paketi dağıtamıyorum

Portal veya Kaynak Yöneticisi şablonu aracılığıyla JAR/kaynak paketini karşıya yükleyemezsiniz.

Uygulama paketinizi [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) aracılığıyla dağıttığınızda, dağıtım ilerleme durumunu düzenli aralıklarla yoklar ve en sonunda dağıtım sonucunu gösterir.

Yoklama kesintiye uğrarsa, yine de şu komutu kullanarak derleme ve dağıtım günlüklerini getirebilirsiniz:

`az spring-cloud app show-deploy-log -n <app-name>`

Ancak, bir _Azure yay bulut_ hizmeti örneğinin tek seferde yalnızca bir kaynak paketi için tek bir derleme işi tetikleyebileceğini unutmayın. Daha fazla ayrıntı için lütfen [uygulama dağıtma](spring-cloud-quickstart-launch-app-portal.md) ve [ortam hazırlama kılavuzunu](spring-cloud-howto-staging-environment.md)inceleyin.

### <a name="my-application-cannot-be-registered"></a>Uygulamam kaydedilemiyor

Çoğu durumda bu durum, gerekli bağımlılıklar/hizmet bulma işlemi Pod dosyanızda düzgün şekilde yapılandırılmadığı zaman gerçekleşir. Yapılandırıldıktan sonra, yerleşik hizmet kayıt defteri sunucu uç noktası, uygulamanızla bir ortam değişkeni olarak eklenir. Uygulamalar daha sonra kendilerini hizmet kayıt defteri sunucusuna kaydeder ve diğer bağımlı mikro hizmetleri bulur.

Yeni kaydedilmiş bir örnek trafik almaya başlamadan önce en az 2 dakika bekleyin.

Mevcut bir yay bulut tabanlı çözümünü Azure 'a geçiriyorsanız, Azure Spring Cloud tarafından sağlanmış yönetilen örneklerle çakışmaktan kaçınmak için geçici _hizmet kayıt_ defteriniz ve _yapılandırma sunucusu_ örneklerinizin kaldırıldığından (veya devre dışı bırakıldığından) emin olun.

Ayrıca _Azure Log Analytics_ _hizmet kayıt defteri_ istemci günlüklerine bakabilirsiniz. Daha ayrıntılı bilgi için bkz. [Tanılama ayarlarıyla günlükleri ve ölçümleri analiz](diagnostic-services.md) etme

_Azure Log Analytics_kullanmaya başlamak için [Bu başlangıç makalesini](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) ziyaret edin. [Kusto sorgu dilini](https://docs.microsoft.com/azure/kusto/query/)kullanarak günlükleri sorgulayın.

### <a name="i-want-to-inspect-my-applications-environment-variables"></a>Uygulamamın ortam değişkenlerini incelemek istiyorum

Ortam değişkenleri, Azure Spring Cloud Framework 'ü bilgilendirerek Azure 'un uygulamanızı oluşturan hizmetlerin nerede ve nasıl yapılandırılacağını anladığından emin olmanızı sağlar.  Ortam değişkenlerinizin doğru olduğundan emin olmak olası sorunları gidermeye yönelik gerekli bir ilk adımdır.  Yay önyükleme çalıştırıcı uç noktasını, ortam değişkenlerinizi gözden geçirmek için kullanabilirsiniz.  

> [!WARNING]
> Bu yordam, test uç noktanızı kullanarak ortam değişkenlerinizi kullanıma sunar.  Test uç noktanız herkese açık ise veya uygulamanıza bir etki alanı adı atadıysanız devam edin.

1. Bu URL 'ye gidin: `https://<your application test endpoint>/actuator/health`.  
    - `{"status":"UP"}` benzer bir yanıt, uç noktanın etkinleştirildiğini gösterir.
    - Yanıt negatifse, `POM.xml`aşağıdaki bağımlılığı ekleyin:

        ```xml
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-actuator</artifactId>
            </dependency>
        ```

1. Spring Boot çalıştırıcı uç noktası etkinken Azure portal gidin ve uygulamanızın yapılandırma sayfasını bulun.  Ad `MANAGEMENT_ENDPOINTS_WEB_EXPOSURE_INCLUDE` ve değer `*` bir ortam değişkeni ekleyin. 

1. Uygulamanızı yeniden başlatın.

1. `https://<the test endpoint of your app>/actuator/env` gidin ve yanıtı inceleyin.  Şu şekilde görünmelidir:

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

`systemEnvironment`adlı alt düğümü bulun.  Bu düğüm, uygulamanızın ortam değişkenlerini içerir.

> [!IMPORTANT]
> Uygulamanızı herkese açık hale getirmeden önce ortam değişkenlerinizin görünürlüğünü ters çevirmeyi unutmayın.  Azure portal gidin, uygulamanızın yapılandırma sayfasını bulun ve bu ortam değişkenini silin: `MANAGEMENT_ENDPOINTS_WEB_EXPOSURE_INCLUDE`.

### <a name="i-cannot-find-metrics-or-logs-for-my-application"></a>Uygulamamın ölçümlerini veya günlüklerini bulamıyorum

Uygulamanın _çalıştığından ve çalışır_ hale geldiğinden emin olmak için uygulama _yönetimi_ 'negidin.

_JVM_ 'den ölçümleri görebiliyorsanız ancak _Tomcat_'ten ölçüm yoksa, uygulama paketinizdeki`spring-boot-actuator` bağımlılığının etkinleştirilip etkinleştirilmediğini ve başarıyla önleyip önolmadığını kontrol edin.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Uygulama günlükleriniz bir depolama hesabında arşivlenebiliyor ancak _Azure Log Analytics_’e gönderilemiyorsa [çalışma alanınızı doğru ayarlayıp ayarlamadığınızı](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) denetleyin. Ücretsiz bir _Azure Log Analytics_katmanı kullanıyorsanız, [ücretsiz katmanın SLA sağlamayacağını](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_3/)unutmayın.