---
title: Azure Bahar Bulutu hakkında sık sorulan sorular | Microsoft Dokümanlar
description: Bu makalede, Azure Bahar Bulutu hakkında sık sorulan sorular yanıtlanmaktadır.
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/07/2019
ms.author: brendm
ms.openlocfilehash: 62623bcadb35f21117ddc2601195e34598c2dff5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298767"
---
# <a name="azure-spring-cloud-faq"></a>Azure İlkbahar Bulut SSS

Bu makalede, Azure Bahar Bulutu hakkında sık sorulan sorular yanıtlanmaktadır. 

## <a name="general"></a>Genel

### <a name="why-azure-spring-cloud"></a>Neden Azure Bahar Bulutu?

Azure Yay Bulutu, Spring Cloud geliştiricileri için hizmet (PaaS) olarak bir platform sağlar. Azure Yay Bulutu, uygulama koduna ve iş mantığına odaklanabilmeniz için uygulama altyapınızı yönetir. Azure Bahar Bulutu'nda yerleşik temel özellikler arasında Eureka, Config Server, Service Registry Server, Pivotal Build Service, Blue-green dağıtımları ve daha fazlası yer alır. Bu hizmet, geliştiricilerin uygulamalarını Azure Cosmos DB, MySQL için Azure Veritabanı ve Redis için Azure Önbelleği gibi diğer Azure hizmetleriyle bağlamalarına da olanak tanır.

Azure Yay Bulutu, Azure Monitörü, Uygulama Öngörüleri ve Log Analytics'i entegre ederek geliştiriciler ve operatörler için uygulama tanılama deneyimini geliştirir.

### <a name="what-service-plans-does-azure-spring-cloud-offer"></a>Azure Bahar Bulutu hangi hizmet planlarını sunuyor?

Azure Yay Bulutu önizleme döneminde tek bir hizmet planı sunar.  Bahar Bulutu dağıtımı 16 vCPU çekirdeği ve 32 gigabayt (GB) bellek içerir.  Dağıtımdaki her microservice örneği için üst sınır, 8 GB belleğe sahip 4 vCPU çekirdeğidir.

Kaynak | Miktar
------- | -------
Bahar uygulaması başına uygulama örnekleri | 20
Azure Bahar Bulutu hizmet örneği başına toplam uygulama örnekleri | 500
Abonelik başına bölge başına Azure Bahar Bulutu hizmet örnekleri | 10
Kalıcı birimler | 10 x 50 GByTe

\*_Limiti yükseltmek için bir [destek bileti](https://azure.microsoft.com/support/faq/)açın._

Daha fazla bilgi için [Azure Destek SSS'si'ne](https://azure.microsoft.com/support/faq/)bakın.

### <a name="how-secure-is-azure-spring-cloud"></a>Azure Bahar Bulutu ne kadar güvenli?

Güvenlik ve gizlilik, Azure ve Azure İlkbahar Bulutu müşterilerinin başlıca öncelikleri arasında dır. Azure, tüm bu verileri güvenli bir şekilde şifreleyerek yalnızca müşterilerin uygulama verilerine, günlüklere veya yapılandırmalara erişebilmesini sağlamaya yardımcı olur. Azure İlkbahar Bulutu'ndaki tüm hizmet örnekleri birbirinden izole edilmiştir.

Azure Yay Bulutu eksiksiz TLS/SSL ve sertifika yönetimi sağlar.

OpenJDK ve Bahar Bulutu çalışma saatleri için kritik güvenlik düzeltme emaları en kısa sürede Azure İlkbahar Bulutu'na uygulanır.

### <a name="in-which-regions-is-azure-spring-cloud-available"></a>Azure Bahar Bulutu hangi bölgelerde kullanılabilir?

Doğu ABD, Batı ABD 2, Batı Avrupa ve Güneydoğu Asya.

### <a name="what-are-the-known-limitations-of-azure-spring-cloud"></a>Azure İlkbahar Bulutu'nun bilinen sınırlamaları nelerdir?

Önizleme sürümü sırasında Azure İlkbahar Bulutu'nun bilinen sınırlamaları şunlardır:

* `spring.application.name`her uygulamayı oluşturmak için kullanılan uygulama adı tarafından geçersiz kılınacaktır.
* `server.port`Git repo'sundaki yapılandırma dosyasına izin verilmez. Yapılandırma dosyasına eklemek, uygulamanızı diğer uygulamalardan veya internetten erişilemez hale getirir.
* Azure portalı ve Azure Kaynak Yöneticisi şablonları uygulama paketlerinin yüklenmesini desteklemez. Uygulama paketlerini yalnızca Azure CLI üzerinden uygulamayı dağıtarak yükleyebilirsiniz.
* Kota sınırlamaları hakkında bilgi edinmek için Azure [Bahar Bulutu'nun sunduğu hizmet planlarını görün?](#what-service-plans-does-azure-spring-cloud-offer)

### <a name="how-can-i-provide-feedback-and-report-issues"></a>Geri bildirim ve sorunları nasıl bildirebilirim?

Azure İlkbahar Bulutu ile ilgili herhangi bir sorunla karşılaşırsanız, bir [Azure Destek İsteği](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)oluşturun. Bir özellik isteği göndermek veya geri bildirim sağlamak için [Azure Geri Bildirimi'ne](https://feedback.azure.com/forums/34192--general-feedback)gidin.

## <a name="development"></a>Geliştirme

### <a name="i-am-a-spring-cloud-developer-but-new-to-azure-what-is-the-quickest-way-for-me-to-learn-how-to-develop-an-azure-spring-cloud-application"></a>Bir Spring Cloud geliştiricisiyim ama Azure'da yeniyim. Azure Bahar Bulutu uygulamasını nasıl geliştireceklerini öğrenmemin en hızlı yolu nedir?

Azure Bahar Bulutu'na başlamanın en hızlı yolu için [Quickstart: Azure portalını kullanarak bir Azure Bahar Bulutu uygulamasını](spring-cloud-quickstart-launch-app-portal.md)başlatın'daki yönergeleri izleyin.

### <a name="what-java-runtime-does-azure-spring-cloud-support"></a>Azure Bahar Bulutu hangi Java çalışma süresini destekler?

Azure Yay Bulutu Java 8 ve 11'i destekler.

### <a name="where-can-i-view-my-spring-cloud-application-logs-and-metrics"></a>Bahar Bulutu uygulama günlüklerimi ve ölçümlerimi nerede görüntüleyebilirim?

Uygulamaya Genel Bakış sekmesinde ve [Azure Monitör](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics#interacting-with-azure-monitor-metrics) sekmesinde ölçümleri bulun.

Azure Bulutu Bulutu, Azure Depolama, EventHub ve [Log Analytics'e](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs#log-queries)Bahar Bulutu uygulama günlükleri ve ölçümleri dışa aktarmayı destekler. Log Analytics'teki tablo adı *AppPlatformLogsforSpring'dir.* Nasıl etkinleştireceklerini öğrenmek için [Tanılama hizmetlerine](diagnostic-services.md)bakın.

### <a name="does-azure-spring-cloud-support-distributed-tracing"></a>Azure İlkbahar Bulutu dağıtılmış izlemeyi destekliyor mu?

Evet. Daha fazla bilgi için [Bkz. Öğretici: Azure Yay Bulutu ile Dağıtılmış İzleme'yi kullanın.](spring-cloud-tutorial-distributed-tracing.md)

### <a name="what-resource-types-does-service-binding-support"></a>Hizmet Bağlama hangi kaynak türlerini destekler?

Şu anda üç hizmet desteklenmiştir: Azure Cosmos DB, MySQL için Azure Veritabanı ve Redis için Azure Önbelleği.

### <a name="can-i-view-add-or-move-persistent-volumes-from-inside-my-applications"></a>Kalıcı birimleri uygulamalarım içinden görüntüleyebilir, ekleyebilir veya taşıyabilir miyim?

Evet.

## <a name="deployment"></a>Dağıtım

### <a name="does-azure-spring-cloud-support-blue-green-deployment"></a>Azure İlkbahar Bulutu mavi-yeşil dağıtımı destekliyor mu?
Evet. Daha fazla bilgi için [bkz.](spring-cloud-howto-staging-environment.md)

### <a name="can-i-access-kubernetes-to-manipulate-my-application-containers"></a>Uygulama imlemi işlemek için Kubernetes'e erişebilir miyim?

Hayır.  Azure Bahar Bulutu, geliştiriciyi temel mimariden soyutlayarak uygulama kodu ve iş mantığına odaklanmanızı sağlar.

### <a name="does-azure-spring-cloud-support-building-containers-from-source"></a>Azure İlkbahar Bulutu kaynaktan kapsayıcı oluşturmayı destekliyor mu?

Evet. Daha fazla bilgi için [kaynak koddan Bahar Bulutu uygulamanızı başlat'a](spring-cloud-launch-from-source.md)bakın.

### <a name="does-azure-spring-cloud-support-autoscaling-in-app-instances"></a>Azure İlkbahar Bulutu uygulama örneklerinde otomatik ölçekletme yi destekliyor mu?

Hayır.

### <a name="what-are-the-best-practices-for-migrating-existing-spring-cloud-microservices-to-azure-spring-cloud"></a>Mevcut Bahar Bulutu mikro hizmetlerini Azure Bahar Bulutu'na geçirmek için en iyi uygulamalar nelerdir?

Mevcut Bahar Bulutu mikro hizmetlerini Azure Bahar Bulutu'na geçirerek, aşağıdaki en iyi uygulamaları gözlemlemek iyi bir fikirdir:
* Tüm uygulama bağımlılıklarının çözülmesi gerekir.
* Yapılandırma girişlerinizi, ortam değişkenlerinizi ve JVM parametrelerinizi hazırlayın, böylece bunları Azure İlkbahar Bulutu'ndaki dağıtımla karşılaştırabilirsiniz.
* Hizmet Bağlama'yı kullanmak istiyorsanız, Azure hizmetlerinizi gözden geçirin ve uygun erişim izinlerini ayarladığınızdan emin olun.
* Service Discovery hizmetimiz, Config Server ve benzeri Gibi Azure Bahar Bulutu tarafından yönetilen hizmetlerle çakışabilecek katıştırılmış hizmetleri kaldırmanızı veya devre dışı bırakmanızı öneririz.
* Resmi, kararlı Pivotal Spring kitaplıklarını kullanmanızı öneririz. Pivotal Spring kitaplıklarının resmi olmayan, beta veya çatallı sürümlerinde hizmet düzeyi anlaşması (SLA) desteği yoktur.

Geçişten sonra, uygulama örneklerinin uygun şekilde ölçeklendirildidiğinden emin olmak için CPU/RAM ölçümlerinizi ve ağ trafiğinizi izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Başka sorularınız varsa, [Azure Bahar Bulutu sorun giderme kılavuzuna](spring-cloud-troubleshoot.md)bakın.
