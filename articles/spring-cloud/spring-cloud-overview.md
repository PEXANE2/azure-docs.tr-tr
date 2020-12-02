---
title: Azure Spring Cloud 'a giriş
description: Azure 'da Java Spring uygulamalarını dağıtmak ve yönetmek için Azure Spring Cloud 'ın özelliklerini ve avantajlarını öğrenin.
author: bmitchell287
ms.service: spring-cloud
ms.topic: overview
ms.date: 09/08/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: 74ebdafb835aff75f282b9d6ac02d8ccf672a2be
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96501099"
---
# <a name="what-is-azure-spring-cloud"></a>Azure Spring Cloud nedir?

Azure yay bulutu, Azure 'a Spring Boot tabanlı mikro hizmet uygulamalarının, sıfır kod değişiklikleriyle dağıtılmasını kolaylaştırır.  Azure yay bulutu, Spring bulut uygulamalarının altyapısını yönetir, böylelikle geliştiriciler koda odaklanabilirler.  Spring Cloud, kapsamlı izleme ve tanılama, yapılandırma yönetimi, hizmet bulma, CI/CD tümleştirmesi, mavi yeşil dağıtımlar ve daha fazlasını kullanarak yaşam döngüsü yönetimi sağlar.

Azure Spring Cloud, hem Java [Spring Boot](https://spring.io/projects/spring-boot) hem de ASP.NET Core [steeltoe](https://steeltoe.io/) uygulamalarını destekler. Steeltoe desteği şu anda genel önizleme olarak sunulmaktadır. Genel Önizleme teklifleri, resmi sürümünden önceki yeni özelliklerle denemeler yapmanızı sağlar.  Genel Önizleme özellikleri ve Hizmetleri üretim kullanımı için tasarlanmamıştır.  Daha fazla bilgi için bkz. [SSS](https://azure.microsoft.com/support/faq/) veya dosya a [destek isteği](../azure-portal/supportability/how-to-create-azure-support-request.md).

Azure Spring Cloud, Azure ekosisteminin bir parçası olarak depolama, veritabanları, izleme ve daha fazlasını içeren diğer Azure hizmetlerine kolayca bağlamayı sağlar.

Bu giriş, Azure Spring Cloud 'ın aşağıdaki yeteneklerini açıklamaktadır:

* Yapılandırma Sunucusu
* Mavi/yeşil dağıtımlar
* Uygulama ölçeklendirme
* Azure DevOps ile tümleştirme
* Uygulama izleme

## <a name="spring-cloud-config-server"></a>Yay bulutu yapılandırma sunucusu

Azure Spring Cloud config Server, hem sunucu hem de istemci tarafı desteğiyle dağıtılmış bir sistemde externalized yapılandırma sağlar.  Azure Spring Cloud config Server, tüm ortamlarda uygulama özelliklerini yönetmek için merkezi bir konumdur. Daha fazla bilgi için bkz. [Spring Cloud config Server başvurusu](https://spring.io/projects/spring-cloud-config). 

## <a name="bluegreen-deployments"></a>Mavi/yeşil dağıtımlar

Azure Spring Cloud, kodu üretim ortamlarına serbest bırakma ve güncelleştirme için mavi/yeşil dağıtımları destekler.  Bu değişiklik yönetimi deseninin, geliştiricilerin, gerekli olduğunda anında geri dönüş güvenliği ile özellik ve kod değişiklikleri uygulamasına olanak tanır.  Geliştiriciler, uygulama kesintiye uğramadan kod değişikliklerini güncelleştirmek veya geri almak için birden çok üretim ortamında kod yazmaya yoğunlaşmasını sağlayabilir.  Hazırlama ortamları ve mavi/yeşil dağıtımlar hakkında daha fazla bilgi edinmek için bu [nasıl yapılır makalesini](spring-cloud-howto-staging-environment.md)ziyaret edin.

## <a name="cicd-pipeline-automation"></a>CI/CD işlem hattı otomasyonu

Azure Spring Cloud, Azure CLı kullanarak Azure DevOps ile tümleştirme sağlar.  Azure DevOps kullanarak, Spring uygulamanıza kod tümleştirmeyi ve dağıtımı otomatikleştirebileceğinizi unutmayın.  Daha fazla bilgi edinmek için bu [makaleyi](spring-cloud-howto-cicd.md)ziyaret edin.

## <a name="application-scaling"></a>Uygulama ölçeklendirme

Azure Spring Cloud, Azure Spring Cloud panonuzda mikro hizmetleri kolayca ölçeklendirmenize olanak tanır.  Hem vCPU sayısı hem de mikro hizmetlerinizin kullanabildiği bellek miktarı, gereksinimlerinize uyacak şekilde ölçeklendirilebilir veya azaltılabilir.  Ölçeklendirme, saniyeler içinde etkili olur ve kod değişikliği veya yeniden dağıtım gerektirmez.  Daha fazla bilgi edinmek için bu [öğreticiyi](spring-cloud-tutorial-scale-manual.md)izleyin.

## <a name="application-monitoring"></a>Uygulama izleme

Yay bulutu 'nın dağıtılmış izleme araçları, geliştiricilerin bir uygulamadaki mikro hizmetler arasındaki karmaşık bağlantıları hata ayıklamasına ve izlemesine olanak sağlar.  Azure Application Insights [Spring Cloud ınuth](https://spring.io/projects/spring-cloud-sleuth) 'i tümleştirerek [Application Insights](../azure-monitor/monitor-reference.md), Azure, doğrudan Azure Portal güçlü dağıtılmış izleme özelliği sağlar.  Daha fazla bilgi edinmek için bu [öğreticiyi](spring-cloud-tutorial-distributed-tracing.md)izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için [yay bulutu hızlı](spring-cloud-quickstart.md) başlangıcı ' nı doldurun

GitHub 'da örnekler mevcuttur: [Azure Spring Cloud Samples](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/).