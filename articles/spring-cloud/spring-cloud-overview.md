---
title: Azure Spring Cloud 'a giriş
description: Azure 'da Java Spring uygulamalarını dağıtmak ve yönetmek için Azure Spring Cloud 'ın özelliklerini ve avantajlarını öğrenin.
author: bmitchell287
ms.service: spring-cloud
ms.topic: overview
ms.date: 11/4/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: dda95932551cdb28c5f28207c05c8e3e78e753b8
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89050854"
---
# <a name="what-is-azure-spring-cloud"></a>Azure Spring Cloud nedir?

Azure yay bulutu, Azure 'a Spring Boot tabanlı mikro hizmet uygulamalarının, sıfır kod değişiklikleriyle dağıtılmasını kolaylaştırır.  Azure yay bulutu, Spring bulut uygulamalarının altyapısını yönetir, böylelikle geliştiriciler koda odaklanabilirler.  Spring Cloud, kapsamlı izleme ve tanılama, yapılandırma yönetimi, hizmet bulma, CI/CD tümleştirmesi, mavi yeşil dağıtımlar ve daha fazlasını kullanarak yaşam döngüsü yönetimi sağlar.

Azure Spring Cloud, Azure ekosisteminin bir parçası olarak depolama, veritabanları, izleme ve daha fazlasını içeren diğer Azure hizmetlerine kolayca bağlamayı sağlar.

Azure yay bulutu Şu anda genel önizleme olarak sunulmaktadır. Genel Önizleme teklifleri, müşterilerin resmi sürümünden önceki yeni özelliklerle deneme yapmasına olanak tanır.  Genel Önizleme özellikleri ve Hizmetleri üretim kullanımı için tasarlanmamıştır.  Önizleme sırasında destek hakkında daha fazla bilgi için lütfen [SSS](https://azure.microsoft.com/support/faq/) veya dosya dosyası [destek isteği](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) inceleyerek daha fazla bilgi edinebilirsiniz.

Bu giriş, Azure Spring Cloud Configuration Server, mavi/yeşil dağıtımları etkinleştirme, uygulamaları ölçeklendirme ve uygulama performansının nasıl izleneceği açıklanmaktadır.

## <a name="spring-cloud-config-server"></a>Yay bulutu yapılandırma sunucusu

Azure Spring Cloud config Server, hem sunucu hem de istemci tarafı desteğiyle dağıtılmış bir sistemde externalized yapılandırma sağlar.  Azure Spring Cloud config Server, tüm ortamlarda uygulama özelliklerini yönetmek için merkezi bir konumdur. Daha fazla bilgi için bkz. [Spring Cloud config Server başvurusu](https://spring.io/projects/spring-cloud-config.md). 

## <a name="enable-bluegreen-deployments"></a>Mavi/yeşil dağıtımları etkinleştir

Azure Spring Cloud, kodu üretim ortamlarına serbest bırakma ve güncelleştirme için mavi/yeşil dağıtımları destekler.  Bu değişiklik yönetimi deseninin, geliştiricilerin, gerekli olduğunda anında geri dönüş güvenliği ile özellik ve kod değişiklikleri uygulamasına olanak tanır.  Geliştiriciler, uygulama kesintiye uğramadan kod değişikliklerini güncelleştirmek veya geri almak için birden çok üretim ortamında kod yazmaya yoğunlaşmasını sağlayabilir.  Hazırlama ortamları ve mavi/yeşil dağıtımlar hakkında daha fazla bilgi edinmek için bu [nasıl yapılır makalesini](spring-cloud-howto-staging-environment.md)ziyaret edin.

## <a name="automate-cicd-pipelines"></a>CI/CD işlem hatlarını otomatikleştirin

Azure Spring Cloud, Azure CLı kullanarak Azure DevOps ile tümleştirme sağlar.  Azure DevOps kullanarak, Spring uygulamanıza kod tümleştirmeyi ve dağıtımı otomatikleştirebileceğinizi unutmayın.  Daha fazla bilgi edinmek için bu [makaleyi](spring-cloud-howto-cicd.md)ziyaret edin.

## <a name="scale-your-application"></a>Uygulamanızı ölçeklendirin

Azure Spring Cloud, Azure Spring Cloud panonuzda mikro hizmetleri kolayca ölçeklendirmenize olanak tanır.  Hem vCPU sayısı hem de mikro hizmetlerinizin kullanabildiği bellek miktarı, gereksinimlerinize uyacak şekilde ölçeklendirilebilir veya azaltılabilir.  Ölçeklendirme, saniyeler içinde etkili olur ve kod değişikliği veya yeniden dağıtım gerektirmez.  Daha fazla bilgi edinmek için bu [öğreticiyi](spring-cloud-tutorial-scale-manual.md)izleyin.

## <a name="application-monitoring"></a>Uygulama izleme

### <a name="monitor-your-application-using-distributed-tracing-and-azure-app-insights"></a>Dağıtılmış izleme ve Azure Uygulama öngörülerini kullanarak uygulamanızı izleyin

Yay bulutu 'nın dağıtılmış izleme araçları, geliştiricilerin bir uygulamadaki mikro hizmetler arasındaki karmaşık bağlantıları hata ayıklamasına ve izlemesine olanak sağlar.  Azure Application Insights [Spring Cloud ınuth](https://spring.io/projects/spring-cloud-sleuth) 'i tümleştirerek [Application Insights](../azure-monitor/insights/insights-overview.md), Azure, doğrudan Azure Portal güçlü dağıtılmış izleme özelliği sağlar.  Daha fazla bilgi edinmek için bu [öğreticiyi](spring-cloud-tutorial-distributed-tracing.md)izleyin.

## <a name="next-steps"></a>Sonraki adımlar
Başlamak için yay bulutu hızlı başlangıcı ' nı doldurun:
> [!div class="nextstepaction"]
> [Hızlı başlangıç: ilk Azure Spring Cloud uygulamanızı dağıtın](spring-cloud-quickstart.md)

GitHub 'da daha fazla örnek vardır: [Azure Spring Cloud Samples](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
