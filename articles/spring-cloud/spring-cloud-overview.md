---
title: Azure Spring Cloud 'a giriş
description: Azure 'da Java Spring uygulamalarını dağıtmak ve yönetmek için Azure Spring Cloud 'ın özelliklerini ve avantajlarını öğrenin.
author: bmitchell287
ms.service: spring-cloud
ms.topic: overview
ms.date: 12/02/2020
ms.author: brendm
ms.custom: devx-track-java, contperf-fy21q2
ms.openlocfilehash: fc3f78131518b4d8740c25ab37c48d4444deef10
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97563152"
---
# <a name="what-is-azure-spring-cloud"></a>Azure Spring Cloud nedir?

Azure Spring Cloud, herhangi bir kod değişikliği yapmadan Spring Boot mikro hizmet uygulamalarını Azure 'a dağıtmayı kolaylaştırır.  Bu hizmet, geliştiricilerin koda odaklanabilmesi için yay bulutu uygulamalarının altyapısını yönetir.  Azure Spring Cloud, kapsamlı izleme ve tanılama, yapılandırma yönetimi, hizmet bulma, CI/CD tümleştirmesi, mavi-yeşil dağıtımlar ve daha fazlasını kullanarak yaşam döngüsü yönetimi sağlar.

## <a name="why-use-azure-spring-cloud"></a>Azure yay bulutu neden kullanılmalıdır?

Uygulamaların Azure Spring Cloud 'a dağıtılması birçok avantaj sunar.  Şunları yapabilirsiniz:
* Mevcut yay uygulamalarını verimli bir şekilde geçirin ve bulut Ölçeklendirmesi ve maliyetlerini yönetin.
* Çeviklik ve teslimin hızını artırmak için yayı bulut desenleriyle modernleştirin.
* Bulut ölçeğinde Java 'Yı çalıştırın ve karmaşık altyapı olmadan daha yüksek kullanım yapın.
* Kapsayıcısız bağımlılıklar olmadan hızlı bir şekilde geliştirin ve dağıtın.
* Üretim iş yüklerini etkili ve kolay bir şekilde izleyin.

Azure Spring Cloud, hem Java [Spring Boot](https://spring.io/projects/spring-boot) hem de ASP.NET Core [steeltoe](https://steeltoe.io/) uygulamalarını destekler. Steeltoe desteği şu anda genel önizleme olarak sunulmaktadır. Genel Önizleme teklifleri, resmi sürümünden önceki yeni özelliklerle denemeler yapmanızı sağlar. Genel Önizleme özellikleri ve Hizmetleri üretim kullanımı için tasarlanmamıştır. Daha fazla bilgi için bkz. [SSS](https://azure.microsoft.com/support/faq/) veya dosya a [destek isteği](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

## <a name="service-overview"></a>Hizmete genel bakış

Azure Spring Cloud, Azure ekosisteminin bir parçası olarak depolama, veritabanları, izleme ve daha fazlasını içeren diğer Azure hizmetlerine kolayca bağlamayı sağlar.  

  ![Azure yay bulutuna genel bakış](media/spring-cloud-principles/azure-spring-cloud-overview.png)

* Azure yay bulutu, altyapıyı yönetmenin sorunsuz bir şekilde uygulama oluşturmaya ve çalıştırmaya odaklanmanızı sağlayan Spring Boot uygulamaları için tam olarak yönetilen bir hizmettir.

* Yalnızca JARs veya kodunuzu dağıtmanız yeterlidir ve Azure Spring Cloud, uygulamalarınızı yay hizmeti çalışma zamanı ve yerleşik uygulama yaşam döngüsü ile otomatik olarak dağıtacaktır.

* İzleme basittir. Dağıtımdan sonra uygulama performansını izleyebilir, hataları giderebilir ve uygulamaları hızlı bir şekilde geliştirebilirsiniz. 

* Azure 'un ekosistemleri ve hizmetleriyle tam tümleştirme.

* Azure yay bulutu, tam olarak yönetilen altyapı, yerleşik yaşam döngüsü yönetimi ve izleme kolaylığı ile kurumsal olarak hazır.

## <a name="documentation-overview"></a>Belgelere genel bakış
Bu belge, Azure Spring Cloud Services 'ı kullanmaya başlama ve bu avantajlardan yararlanma hakkında bilgi sağlayan bölümleri içerir.

* Kullanmaya başlayın
    * [İlk uygulamanızı yayınlama](spring-cloud-quickstart.md)
    * [Azure yay bulut hizmeti sağlama](spring-cloud-quickstart-provision-service-instance.md)
    * [Yapılandırma sunucusunu ayarlama]()
    * [Uygulama oluşturma ve dağıtma](spring-cloud-quickstart-deploy-apps.md)
    * [Günlük ölçümlerini ve izlemeyi kullanma](spring-cloud-quickstart-logs-metrics-tracing.md)
* Nasıl yapılır
    * [Geliştirme](spring-cloud-tutorial-prepare-app-deployment.md): mevcut bir Java Spring uygulamasını Azure Spring Cloud 'a dağıtmak için hazırlayın. Azure yay bulutu, düzgün yapılandırıldığında, Java Spring bulut uygulamalarını izlemek, ölçeklendirmek ve güncelleştirmek için güçlü hizmetler sağlar.
    * [Dağıtma](spring-cloud-howto-staging-environment.md): Azure yay bulutu 'ndaki mavi yeşil dağıtım modelini kullanarak hazırlama dağıtımı ayarlama. Mavi/yeşil dağıtım, yeni (yeşil) sürüm dağıtıldığı sırada mevcut (mavi) sürümü canlı tutma prensibine dayanan bir Azure DevOps Sürekli Teslim düzenidir.
    * [Uygulamaları yapılandırma](spring-cloud-howto-start-stop-delete.md): Azure Spring Cloud uygulamalarınızı başlatın, durdurun ve silin. Azure portal veya Azure CLı kullanarak Azure Spring Cloud 'da bir uygulamanın durumunu değiştirin.
    * [Ölçek](spring-cloud-tutorial-scale-manual.md): Azure Portal veya otomatik ölçeklendirme ayarlarını kullanarak Azure Spring Cloud panosunu kullanarak herhangi bir mikro hizmet uygulamasını ölçeklendirin. Veritabanları, depolama ve Anahtar kasaları gibi dış kaynaklarla iletişim kurmak için genel IP 'Ler kullanılabilir.
    * [Uygulamaları izleme](spring-cloud-tutorial-distributed-tracing.md): karmaşık sorunları kolayca ayıklamanız ve izlemek için dağıtılmış izleme araçları. Azure Spring Cloud, Azure 'ın Application Insights yay bulutuna karşı tümleşir. Bu tümleştirme Azure portal güçlü dağıtılmış izleme yeteneği sağlar.
    * [Güvenli uygulamalar](spring-cloud-howto-enable-system-assigned-managed-identity.md): Azure kaynakları, Azure Active Directory otomatik olarak yönetilen bir kimlik sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için kullanabilirsiniz.
    * [Diğer Azure hizmetleriyle tümleştirme](spring-cloud-tutorial-bind-cosmos.md): Spring Boot uygulamalarınızı el ile yapılandırmak yerine, seçili Azure hizmetlerini uygulamalarınıza otomatik olarak bağlayarak, örneğin uygulamanızı bir Azure Cosmos db veritabanına bağlayabilirsiniz.
    * [Otomatikleştir](spring-cloud-howto-cicd.md): sürekli tümleştirme ve sürekli teslim araçları, var olan uygulamalara en az çaba ve riske sahip güncelleştirmeleri hızlı bir şekilde dağıtmanızı sağlar. Azure DevOps bu temel görevleri düzenlemeye ve denetlemeye yardımcı olur. 
    * [Sorun giderme](spring-cloud-howto-self-diagnose-solve.md): Azure Spring Cloud Diagnostics, uygulama sorunlarını gidermenize yardımcı olacak etkileşimli bir deneyim sunar. Yapılandırma gerekmez. Sorunları bulduğunuzda, Azure yay Bulut Tanılaması sorunları tanımlar ve sorun gidermeye ve sorunları çözmeye yardımcı olan bilgiler için sizi yönlendirir.
    * [Geçiş](https://docs.microsoft.com/azure/developer/java/migration/migrate-spring-boot-to-azure-spring-cloud): mevcut bir yay bulutu uygulamasını veya Spring Boot uygulamasını Azure Spring Cloud üzerinde çalışacak şekilde geçirme.

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için [yay bulutu hızlı](spring-cloud-quickstart.md) başlangıcı ' nı doldurun

GitHub 'da örnekler mevcuttur: [Azure Spring Cloud Samples](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/).
