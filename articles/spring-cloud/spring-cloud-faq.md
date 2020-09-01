---
title: Azure yay bulutu hakkında sık sorulan sorular | Microsoft Docs
description: Bu makalede, Azure Spring Cloud hakkında sık sorulan sorular yanıtlanmaktadır.
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/07/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: fcd9f0759ae885199347cc23d93e8f1c00251387
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89260535"
---
# <a name="azure-spring-cloud-faq"></a>Azure Spring Cloud hakkında SSS

Bu makalede, Azure Spring Cloud hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="general"></a>Genel

### <a name="why-azure-spring-cloud"></a>Neden Azure yay bulutu?

Azure Spring Cloud, Spring Cloud geliştiricileri için hizmet olarak platform (PaaS) sağlar. Azure Spring Cloud, uygulama altyapınızı, uygulama kodu ve iş mantığına odaklanabilmeniz için yönetir. Azure yay bulutu 'nda yerleşik olarak bulunan temel özellikler Eureka, config Server, hizmet kayıt defteri sunucusu, özetleme oluşturma hizmeti, mavi-yeşil dağıtım ve daha fazlasını içerir. Bu hizmet, geliştiricilerin uygulamalarını Azure Cosmos DB, MySQL için Azure veritabanı ve Redsıs için Azure önbelleği gibi diğer Azure hizmetleriyle bağlamasını da sağlar.

Azure Spring Cloud, Azure Izleyici, Application Insights ve Log Analytics tümleştirerek geliştiriciler ve işleçler için uygulama tanılama deneyimini geliştirir.

### <a name="how-secure-is-azure-spring-cloud"></a>Azure yay bulutu ne kadar güvenlidir?

Güvenlik ve gizlilik, Azure ve Azure Spring bulut müşterileri için en önemli önceliklerdir. Azure, tüm bu verileri güvenli bir şekilde şifreleyerek yalnızca müşterilerin uygulama verilerine, günlüklerine veya yapılandırmasına erişmesini sağlamaya yardımcı olur. 

* Azure Spring Cloud 'daki hizmet örnekleri birbirinden yalıtılmıştır.
* Azure yay bulutu, tüm TLS/SSL ve sertifika yönetimi sağlar.
* OpenJDK ve Spring Cloud çalışma zamanları için kritik güvenlik düzeltme ekleri, Azure Spring Cloud 'a mümkün olan en kısa sürede uygulanır.

### <a name="in-which-regions-is-azure-spring-cloud-available"></a>Azure yay bulutu hangi bölgelerde kullanılabilir?

Doğu ABD, Batı ABD 2, Batı Avrupa ve Güneydoğu Asya.

### <a name="what-are-the-known-limitations-of-azure-spring-cloud"></a>Azure Spring Cloud 'ın bilinen sınırlamaları nelerdir?

Azure yay bulutu aşağıdaki bilinen sınırlamalara sahiptir:
    
* `spring.application.name` , her uygulamayı oluşturmak için kullanılan uygulama adı tarafından geçersiz kılınır.
* `server.port` Varsayılan bağlantı noktası 1025 ' dir. Başka herhangi bir değer uygulanırsa, 1025 olarak geçersiz kılınır.
* Azure portal ve Azure Resource Manager şablonları uygulama paketlerinin yüklenmesini desteklemez. Uygulama paketlerini yalnızca uygulamayı Azure CLı aracılığıyla dağıtarak yükleyebilirsiniz.

### <a name="what-pricing-tiers-are-available"></a>Hangi fiyatlandırma katmanları kullanılabilir? 
Hangi birini kullanmalıyım ve her katmandaki sınırlar nelerdir?
* Azure yay bulutu iki fiyatlandırma katmanı sunar: temel ve standart. Temel katman, geliştirme ve test için hedeflenir ve Azure yay bulutu 'nı deniyor. Standart katman, genel amaçlı üretim trafiği çalıştırmak için iyileştirilmiştir. Sınırlar ve özellik düzeyi karşılaştırması için bkz. [Azure Spring Cloud fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/spring-cloud/) .

### <a name="how-can-i-provide-feedback-and-report-issues"></a>Nasıl geri bildirimde bulunabilirim ve rapor sorunları verebilir?

Azure Spring Cloud ile ilgili herhangi bir sorunla karşılaşırsanız bir [Azure destek isteği](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)oluşturun. Bir özellik isteği göndermek veya geri bildirim sağlamak için [Azure geri bildirimi](https://feedback.azure.com/forums/34192--general-feedback)' ne gidin.

## <a name="development"></a>Geliştirme

### <a name="i-am-a-spring-cloud-developer-but-new-to-azure-what-is-the-quickest-way-for-me-to-learn-how-to-develop-an-azure-spring-cloud-application"></a>Ben bir yay bulut geliştiricisiyim, ancak Azure 'da yeni. Azure yay bulut uygulaması geliştirmeyi öğrenmenin en hızlı yolu nedir?

Azure Spring Cloud ile çalışmaya başlamanın en hızlı yolu için [hızlı başlangıç: Azure Portal kullanarak bir Azure Spring Cloud uygulaması başlatma](spring-cloud-quickstart-launch-app-portal.md)' daki yönergeleri izleyin.

### <a name="what-java-runtime-does-azure-spring-cloud-support"></a>Azure yay bulutu hangi Java çalışma zamanına destekler?

Azure yay bulutu, Java 8 ve 11 ' i destekler. Bkz. [Java çalışma zamanı ve işletim sistemi sürümleri](#java-runtime-and-os-versions)

### <a name="where-can-i-view-my-spring-cloud-application-logs-and-metrics"></a>Yay bulutu uygulama Günlüklerimi ve ölçümlerini nereden görüntüleyebilirim?

Uygulama Genel Bakış sekmesinde ve [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics#interacting-with-azure-monitor-metrics) sekmesinde ölçümleri bulun.

Azure Spring Cloud, Azure depolama, EventHub ve [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs#log-queries)yay bulut uygulaması günlüklerini ve ölçümlerini vermeyi destekler. Log Analytics tablo adı *Appplatformlogsforspring*' dir. Nasıl etkinleştireceğinizi öğrenmek için bkz. [Tanılama Hizmetleri](diagnostic-services.md).

### <a name="does-azure-spring-cloud-support-distributed-tracing"></a>Azure yay bulutu dağıtılmış izlemeyi destekliyor mu?

Evet. Daha fazla bilgi için bkz. [öğretici: Azure Spring Cloud Ile dağıtılmış Izleme kullanma](spring-cloud-tutorial-distributed-tracing.md).

### <a name="what-resource-types-does-service-binding-support"></a>Hizmet bağlama hangi kaynak türlerini destekler?

Şu anda üç hizmet desteklenmektedir:
* Azure Cosmos DB
* MySQL için Azure Veritabanı
* Redsıs için Azure önbelleği.

### <a name="can-i-view-add-or-move-persistent-volumes-from-inside-my-applications"></a>Uygulamalarım içinden kalıcı birimleri görüntüleyebilir, ekleyebilir veya taşıyabilir miyim?

Evet.

### <a name="when-i-deletemove-an-azure-spring-cloud-service-instance-will-its-extension-resources-be-deletedmoved-as-well"></a>Azure yay bulut hizmeti örneğini sildiğimde/taşırken, uzantı kaynakları da silinir/taşınır mi?

Bu, uzantı kaynaklarına sahip olan kaynak sağlayıcılarının mantığına bağlıdır. Bir örneğin uzantı kaynakları `Microsoft.AppPlatform` aynı ad alanına ait değildir, bu nedenle davranış kaynak sağlayıcısına göre değişir. Örneğin, silme/taşıma işlemi, **Tanılama ayarları** kaynakları için basamaklanmaz. Silinen bir kaynak KIMLIĞIYLE yeni bir Azure yay bulutu örneği sağlandıysa veya önceki Azure yay bulutu örneği geri taşınırsa, önceki **Tanılama ayarları** kaynakları genişlemeye devam eder.

Azure CLı kullanarak Spring Cloud 'ın tanılama ayarlarını silebilirsiniz:

```azurecli
 az monitor diagnostic-settings delete --name $diagnosticSettingName --resource $azureSpringCloudResourceId
```

## <a name="java-runtime-and-os-versions"></a>Java çalışma zamanı ve işletim sistemi sürümleri

### <a name="which-versions-of-java-runtime-are-supported-in-azure-spring-cloud"></a>Azure Spring Cloud 'da hangi Java çalışma zamanı sürümleri desteklenir?

Azure Spring Cloud, en son Derlemelerle Java LTS sürümlerini destekler, şu anda 2020 Haziran, Java 8 derleme 252 ve Java 11 derleme 7 destekleniyor. Bkz [. Azure için JDK 'yi ve Azure Stack yüklemeyi](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-install)

### <a name="who-built-these-java-runtimes"></a>Bu Java çalışma zamanlarını kim derlediniz?

Azul sistemleri. Microsoft ile Azul Systems’ın desteklediği Azul Zulu for Azure - Enterprise Edition JDK, OpenJDK’nın Azure ve Azure Stack’e yönelik çok platformlu, üretime hazır bir dağıtımıdır. Java SE uygulamalarını oluşturmak ve çalıştırmak için gereken tüm bileşenleri içerir.

### <a name="how-often-will-java-runtimes-get-updated"></a>Java çalışma zamanları ne sıklıkla güncelleştirilir?

LTS ve MTS JDK sürümleri, gerektiğinde üç aylık güvenlik güncelleştirmeleri, hata düzeltmeleri ve kritik bant dışı güncelleştirmeler ve düzeltme ekleri vardır. Bu destek, Java 7 ve 8 güvenlik güncelleştirmelerinin ve Java 11 gibi daha yeni Java sürümlerinde bildirilen hata düzeltmelerinin geri bağlantı noktalarını içerir.

### <a name="how-long-will-java-8-and-java-11-lts-versions-be-supported"></a>Java 8 ve Java 11 LTS sürümleri ne kadar süreyle destekleniyor?

Bkz. [Azure ve Azure Stack için uzun süreli Java desteği](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-long-term-support).

* Java 8 LTS, Aralık 2030 ' ye kadar desteklenecektir.
* Java 11 LTS, 2027 Eylül tarihine kadar desteklenecektir.

### <a name="how-can-i-download-a-supported-java-runtime-for-local-development"></a>Yerel geliştirme için desteklenen bir Java çalışma zamanını nasıl indirebilirim?

Bkz. [Azure için JDK 'Yi yükleyip Azure Stack](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-install).

### <a name="what-is-the-retire-policy-for-older-java-runtimes"></a>Eski Java çalışma zamanları için devre dışı bırakma ilkesi nedir?

Genel bildirim, herhangi bir eski çalışma zamanı sürümü kullanımdan kaldırıldıktan sonra 12 ay sonra gönderilir. Sonraki bir sürüme geçiş yapmak için 12 aya sahip olursunuz.

* Bir Java sürümünü devre dışı bırakacağız, abonelik yöneticileri e-posta bildirimi alır.
* Devre dışı bırakma bilgileri belgelerde yayımlanacak.

### <a name="how-can-i-get-support-for-issues-at-the-java-runtime-level"></a>Java çalışma zamanı düzeyinde sorunlar için nasıl destek alabilirim?

Azure desteği ile bir destek bileti açabilirsiniz.  Bkz. [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

### <a name="what-is-the-operation-system-to-run-my-apps"></a>Uygulamalarımı çalıştırmak için işletim sistemi nedir?

En son Ubuntu LTS sürümü kullanılır; Şu anda [ubuntu 20,04 LTS (odak)](https://releases.ubuntu.com/focal/) varsayılan işletim sistemi.

### <a name="how-often-will-os-security-patches-be-applied"></a>İşletim sistemi güvenlik düzeltme eklerinin ne sıklıkta uygulanması gerekir?

Azure Spring Cloud için geçerli olan güvenlik düzeltme ekleri, aylık olarak üretime alınacaktır.
Azure Spring Cloud için geçerli olan kritik güvenlik düzeltme ekleri (CVE puanı >= 9), mümkün olan en kısa sürede kullanıma sunulacaktır.

## <a name="deployment"></a>Dağıtım

### <a name="does-azure-spring-cloud-support-blue-green-deployment"></a>Azure yay bulutu mavi yeşil dağıtımı destekliyor mu?
Evet. Daha fazla bilgi için bkz. [hazırlama ortamı ayarlama](spring-cloud-howto-staging-environment.md).

### <a name="can-i-access-kubernetes-to-manipulate-my-application-containers"></a>Uygulama kapsayıcılarımı değiştirmek için Kubernetes 'e erişebilir miyim?

Hayır.  Azure Spring Cloud, geliştirici temel mimariden soyutlar, uygulama kodu ve iş mantığına odaklanmanızı sağlar.

### <a name="does-azure-spring-cloud-support-building-containers-from-source"></a>Azure yay bulutu, kaynaktan kapsayıcılar oluşturmayı destekliyor mu?

Evet. Daha fazla bilgi için bkz. [Spring Cloud uygulamanızı kaynak koddan başlatma](spring-cloud-launch-from-source.md).

### <a name="does-azure-spring-cloud-support-autoscaling-in-app-instances"></a>Azure yay bulutu, uygulama örneklerinde otomatik ölçeklendirmeyi destekliyor mu?

Hayır.

### <a name="what-are-the-best-practices-for-migrating-existing-spring-cloud-microservices-to-azure-spring-cloud"></a>Mevcut yay bulutu mikro hizmetlerini Azure Spring Cloud 'a geçirmek için en iyi uygulamalar nelerdir?

Mevcut yay bulutu mikro hizmetlerini Azure Spring buluta geçirirken, aşağıdaki en iyi yöntemleri gözlemlemek iyi bir fikirdir:
* Tüm uygulama bağımlılıklarının çözülmesi gerekir.
* Yapılandırma girdilerinizi, ortam değişkenlerinizi ve JVM parametrelerinizi Azure Spring Cloud 'daki dağıtımla karşılaştırabilmeniz için hazırlayın.
* Hizmet bağlamayı kullanmak istiyorsanız, Azure hizmetlerinizi gözden geçirdiğinizden ve uygun erişim izinlerini ayarlamış olduğunuzdan emin olun.
* Hizmet bulma hizmeti, yapılandırma sunucusu gibi Azure Spring Cloud tarafından yönetilen hizmetlerle çakışabilecek gömülü Hizmetleri kaldırmanızı veya devre dışı bırakmanızı öneririz.
* Resmi, kararlı özette yay kitaplıklarını kullanmanızı öneririz. Özet Ilkelerin kitaplıklarının resmi olmayan, beta veya çatallı sürümlerinin hizmet düzeyi anlaşması (SLA) desteği yoktur.

Geçişten sonra, uygulama örneklerinin uygun şekilde ölçeklendirdiğinden emin olmak için CPU/RAM ölçümleri ve ağ trafiğinizi izleyin.

## <a name="trouble-shooting"></a>Sorun giderme

### <a name="what-are-the-impacts-of-service-registry-rarely-unavailable"></a>Hizmet kayıt defterinin etkileri nadiren kullanılamıyor mu?

Nadiren meydana gelen bazı senaryolarda, şöyle bir hata görebilirsiniz: 
```
RetryableEurekaHttpClient: Request execution failure with status code 401; retrying on another server if available
```
Uygulama günlüklerinizin. Bu sorun, ağ kararsız veya diğer ağ sorunları nedeniyle Spring Framework tarafından çok düşük bir ücret sunarak tanıtılmıştır. 

Kullanıcı deneyimine hiçbir etkileri olmaması gerekir, Eureka istemcisinde bu işlemi gerçekleştirmek için hem sinyal hem de yeniden deneme ilkesi vardır. Bunu tek geçici bir hata olarak düşünebilirsiniz ve güvenle atlayabilirsiniz.

Bu bölümü geliştireceğiz ve kısa bir süre sonra kullanıcıların uygulamalarından bu hatadan kaçınacağız.


## <a name="next-steps"></a>Sonraki adımlar

Başka sorularınız varsa bkz. [Azure yay bulutu sorun giderme kılavuzu](spring-cloud-troubleshoot.md).
