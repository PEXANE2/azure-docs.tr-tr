---
title: Azure yay bulutu için sık sorulan sorular | Microsoft Docs
description: Azure yay bulutu için SSS 'yi gözden geçirin
services: spring-cloud
author: v-vasuke
manager: jeconnoc
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: v-vasuke
ms.openlocfilehash: 09970468a277dcaf3f28b4f5065572568089a12e
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72039122"
---
# <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

Bu makalede, Azure Spring Cloud hakkında sık sorulan sorular ele alınmaktadır. 

## <a name="general"></a>Genel

### <a name="why-azure-spring-cloud"></a>Neden Azure yay bulutu?

Azure yay bulutu, Spring geliştiricileri için bir hizmet olarak platform (PaaS) sağlar. Azure yay bulutu, uygulama altyapınızı, uygulama kodu ve iş mantığına odaklanabilmeniz için yönetir. Azure yay bulutu 'nda yerleşik olarak bulunan temel özellikler, Eureka, config Server, hizmet kayıt defteri sunucusu, özetleme oluşturma hizmeti, mavi-yeşil dağıtımlar ve daha fazlasını içerir. Bu hizmet, geliştiricilerin uygulamalarını CosmosDB, MySQL ve redin için Azure önbelleği gibi Azure hizmetleriyle bağlamasını da sağlar.

Azure Spring Cloud, Azure Izleyici, Application Insights ve Log Analytics tümleştirerek geliştiriciler ve işleçler için uygulama tanılama deneyimini geliştirir.

### <a name="what-service-plans-does-azure-spring-cloud-offer"></a>Azure yay bulutu hangi hizmet planlarını sunuyor?

Azure yay bulutu, önizleme dönemi boyunca ücretsizdir.

Kaynak | Miktar
------- | -------
vCPU | 4
Hafıza | 8 GBayt
Yay uygulaması başına uygulama örnekleri | 20
Azure yay bulut hizmeti örneği başına toplam uygulama örnekleri | 50 *
Abonelik başına bölge başına Azure yay bulut hizmeti örnekleri | iki
Kalıcı birimler | 10 x 50 GBayt

*_sınırı yükseltmek için bir [destek bileti](https://azure.microsoft.com/support/faq/) açın._

Daha ayrıntılı bilgi için lütfen [Azure desteği SSS](https://azure.microsoft.com/support/faq/)bölümüne bakın.

### <a name="how-secure-is-azure-spring-cloud"></a>Azure yay bulutu ne kadar güvenlidir?

Güvenlik ve gizlilik, Azure ve Azure Spring bulut müşterileri için en önemli önceliklerden biridir. Azure, tüm bu verileri güvenli bir şekilde şifreleyerek yalnızca müşterinin uygulama verilerine, günlüklerine veya yapılandırmasına erişmesini sağlar. Azure Spring Cloud 'daki tüm hizmet örnekleri birbirinden yalıtılmıştır.

Azure yay bulutu, tüm SSL ve sertifika yönetimi sağlar.

OpenJDK ve Spring Cloud çalışma zamanları için kritik güvenlik düzeltme ekleri, Azure Spring Cloud 'a mümkün olan en kısa sürede uygulanır.

### <a name="which-regions-azure-spring-cloud-are-available"></a>Azure yay bulutu hangi bölgelerde kullanılabilir?

Doğu ABD, Batı ABD 2, Batı Avrupa ve Güneydoğu Asya.

### <a name="what-are-the-known-limitations-of-azure-spring-cloud"></a>Azure Spring Cloud 'ın bilinen sınırlamaları nelerdir?

Hizmet önizlemedeyken Azure Spring Cloud 'ın bilinen sınırlamaları aşağıda verilmiştir.

* `spring.application.name`, her uygulamayı oluşturmak için kullanılan uygulama adı tarafından geçersiz kılınır.
* Git deposundan yapılandırma dosyasında `server.port` ' a izin verilmez. Bunu yapılandırma dosyasına eklemek, uygulamanızın diğer uygulamalardan veya internet 'ten ulaşılamaz olmasına neden olabilir.
* Azure portal ve Kaynak Yöneticisi şablonları uygulama paketlerinin yüklenmesini desteklemez. Bu, yalnızca Azure CLı tarafından gerçekleştirilen uygulama dağıtımı aracılığıyla yapılabilir.
* Kota sınırlamaları için, [Azure yay bulutu 'nın sunduğu hizmet planlarını](#what-service-plans-does-azure-spring-cloud-offer)inceleyin.

### <a name="how-can-i-provide-feedback-and-report-issues"></a>Nasıl geri bildirimde bulunabilirim ve rapor sorunları verebilir?

Spring Service örneklerinizi Azure Spring Cloud 'da oluşturduysanız, bir [Azure destek isteği](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)oluşturabilirsiniz. Henüz Azure Spring buluta eklendi, özellik istemek veya geri bildirim sağlamak için [Azure geri bildirimlerine](https://feedback.azure.com/) gidebilirsiniz.

## <a name="development"></a>Geliştirme

### <a name="i-am-a-spring-developer-but-new-to-azure-what-is-the-quickest-way-for-me-to-learn-how-to-develop-aazure-spring-cloud-application"></a>Bir yay geliştiriciyim, Azure 'da yeni bir uygulama, aAzure Spring Cloud uygulaması geliştirmeyi öğrenmem için en hızlı yol nedir?

Azure yay bulutu 'nı kullanmaya başlamanın en hızlı yolu, [Bu hızlı başlangıcı](spring-cloud-quickstart-launch-app-portal.md)takip etmek için kullanılır.

### <a name="what-java-runtime-does-azure-spring-cloud-support"></a>Azure yay bulutu hangi Java çalışma zamanına destekler?

Azure yay bulutu, Java 8 ve 11 ' i destekler.

### <a name="where-can-i-see-my-spring-application-logs-and-metrics"></a>Yay uygulama Günlüklerimi ve ölçümlerini nereden görebilirim?

Uygulama Genel Bakış sekmesinde ve [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics#interacting-with-azure-monitor-metrics) sekmesinde ölçümleri bulun.

Azure yay bulutu, bahar uygulama günlüklerinizi ve ölçümlerini Azure depolama, EventHub ve [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs#log-queries)dışa aktarmayı destekler. Log Analytics tablo adı `AppPlatformLogsforSpring` ' dır. Etkinleştirmek için, [tanılama hizmetlerimiz](diagnostic-services.md)hakkındaki bu makaleyi gözden geçirin.

### <a name="does-azure-spring-cloud-support-distributed-tracing"></a>Azure yay bulutu dağıtılmış izlemeyi destekliyor mu?

Evet, daha fazla ayrıntı için [Dağıtılmış izlemeyi](spring-cloud-tutorial-distributed-tracing.md) ziyaret edin.

### <a name="what-resource-types-does-service-binding-support"></a>Hizmet bağlama hangi kaynak türlerini destekler?

Şu anda üç hizmet desteklenmektedir: Azure Cosmos DB, MySQL için Azure veritabanı ve Redsıs için Azure önbelleği.

### <a name="can-i-viewaddmove-persistent-volumes-from-inside-my-applications"></a>Uygulamalarım içinden kalıcı birimleri görüntüleyebilir/ekleyebilir/taşıyabilir miyim?
Evet.

## <a name="deployment"></a>Kurulum

### <a name="does-azure-spring-cloud-support-blue-green-deployment"></a>Azure yay bulutu mavi yeşil dağıtımı destekliyor mu?
Evet, daha fazla ayrıntı için [hazırlama ortamı kılavuzunu](spring-cloud-howto-staging-environment.md) ziyaret edin.

### <a name="can-i-access-kubernetes-to-manipulate-my-application-containers"></a>Uygulama kapsayıcılarımı değiştirmek için Kubernetes 'e erişebilir miyim?

Hayır.  Azure Spring Cloud, geliştirici temel mimariden soyutlar, uygulama kodu ve iş mantığına odaklanmanızı sağlar.

### <a name="does-azure-spring-cloud-support-building-containers-from-source"></a>Azure yay bulutu, kaynaktan kapsayıcılar oluşturmayı destekliyor mu?

Evet, daha fazla ayrıntı için [kaynaktan dağıt](spring-cloud-launch-from-source.md) ' ı ziyaret edin.

### <a name="does-azure-spring-cloud-support-autoscaling-in-app-instances"></a>Azure yay bulutu, uygulama örneklerinde otomatik ölçeklendirmeyi destekliyor mu?

Hayır.

### <a name="what-are-the-best-practices-for-migrating-existing-spring-microservices-to-azure-spring-cloud"></a>Mevcut Spring mikro hizmetlerini Azure Spring Cloud 'a geçirmek için en iyi uygulamalar nelerdir?

Mevcut Spring mikro hizmetlerini Azure Spring buluta geçirmeden önce
* Tüm uygulama bağımlılıklarının çözülmesi gerekir.
* Yapılandırma girdilerinizi, ortam değişkenlerinizi ve JVM parametrelerinizi Azure Spring Cloud 'daki dağıtımla karşılaştırabilmeniz için hazırlayın.
* Hizmet bağlamayı kullanmak istiyorsanız, Azure hizmetlerinizi gözden geçin ve uygun erişim izinlerini ayarlamış olduğunuzdan emin olun.
* Hizmet bulma hizmeti, yapılandırma sunucusu vb. gibi Azure Spring Cloud tarafından yönetilen hizmetler ile çakışabilecek gömülü Hizmetleri kaldırmanızı veya devre dışı bırakmanızı öneririz.
*-* Resmi ve kararlı özette yay kitaplıklarını kullanmanızı öneririz. Özet Ilkelerin kitaplıklarının resmi olmayan, beta veya çatallı sürümlerinin SLA desteği yoktur.

Geçişten sonra, uygulama örneklerinin uygun şekilde ölçeklendirdiğinden emin olmak için CPU/RAM ölçümlerini ve ağ trafiğini izleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla sorunuz varsa, sorun giderme kılavuzuna](spring-cloud-troubleshoot.md)göz atın.