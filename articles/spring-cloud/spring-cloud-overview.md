---
title: Azure Spring Cloud 'a giriş
description: Azure 'da Java Spring uygulamalarını dağıtmak ve yönetmek için Azure Spring Cloud 'ın özelliklerini ve avantajlarını öğrenin.
author: jpconnock
ms.service: spring-cloud
ms.topic: overview
ms.date: 10/04/2019
ms.author: jeconnoc
ms.openlocfilehash: c58e6b2a0c0de8295df65b44fbdeaeb2d4461948
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72166524"
---
# <a name="what-is-azure-spring-cloud"></a>Azure yay bulutu nedir?

Azure yay bulutu, Azure 'a Spring Boot tabanlı mikro hizmet uygulamalarının, sıfır kod değişiklikleriyle dağıtılmasını kolaylaştırır.  Azure yay bulutu, geliştiricilerin, yay bulut uygulamalarının yaşam döngüsünü yöneterek kendi kodlarına odaklanmasını sağlar.  Spring Cloud, kapsamlı izleme ve tanılama, yapılandırma yönetimi, hizmet bulma, CI/CD tümleştirmesi, mavi yeşil dağıtımlar ve daha fazlasını kullanarak yaşam döngüsü yönetimi sağlar.

Azure Spring Cloud, Azure ekosisteminin bir parçası olarak depolama, veritabanları, izleme ve daha fazlasını içeren diğer Azure hizmetlerine kolayca bağlamayı sağlar.

Azure yay bulutu Şu anda önizleme aşamasındadır.  Bu hizmete önizleme sırasında erişim için lütfen [Bu formu doldurun](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR-LA2geqX-ZLhi-Ado1LD3tUNDk2VFpGUzYwVEJNVkhLRlcwNkZFUFZEUS4u
).

Başlamak için [Azure CLI](spring-cloud-quickstart-launch-app-cli.md), [Azure Portal](spring-cloud-quickstart-launch-app-portal.md)veya [Maven](spring-cloud-quickstart-launch-app-maven.md)kullanarak Spring Cloud hızlı başlangıcı ' nı tamamlayın.

## <a name="application-configuration"></a>Uygulama yapılandırması

### <a name="spring-cloud-config-server"></a>Yay bulutu yapılandırma sunucusu

Azure Spring Cloud config Server, hem sunucu hem de istemci tarafı desteğiyle dağıtılmış bir sistemde externalized yapılandırma sağlar.  Yapılandırma sunucusu, tüm ortamlarda uygulama özelliklerini yönetmek için merkezi bir konum sağlar.  Daha fazla bilgi edinmek için [Spring Cloud config Server başvurusunu](https://spring.io/projects/spring-cloud-config.md) ziyaret edin ve öğreticiyi doldurun.

### <a name="enable-bluegreen-deployments"></a>Mavi/yeşil dağıtımları etkinleştir

Azure Spring Cloud, kodu üretim ortamlarına serbest bırakma ve güncelleştirme için mavi/yeşil dağıtımları destekler.  Bu değişiklik yönetimi deseninin uygulanması, geliştiricilerin gerekli olduğunda anında geri dönüş güvenliği ile özellik ve kod değişiklikleri uygulamasına olanak tanır.  Azure, geliştiricilerin birden çok üretim ortamını yöneterek kod yazmaya odaklanmasını ve uygulama kesintiye uğramadan kod değişikliklerini güncelleştirmeyi veya geri almayı kolaylaştırır.  Hazırlama ortamları ve mavi/yeşil dağıtımlar hakkında daha fazla bilgi edinmek için bu [nasıl yapılır makalesini](spring-cloud-howto-staging-environment.md)ziyaret edin.

### <a name="automate-cicd-pipelines"></a>CI/CD işlem hatlarını otomatikleştirin

Azure Spring Cloud, Azure CLı kullanarak Azure DevOps ile tümleştirme sağlar.  Azure DevOps kullanarak, Spring uygulamanıza kod tümleştirmeyi ve dağıtımı otomatikleştirebileceğinizi unutmayın.  Daha fazla bilgi edinmek için bu [makaleyi](spring-cloud-howto-cicd.md)ziyaret edin.

### <a name="scale-your-application"></a>Uygulamanızı ölçeklendirin

Azure Spring Cloud, Azure Spring Cloud panonuzda mikro hizmetleri kolayca ölçeklendirmenize olanak tanır.  Hem vCPU sayısı hem de mikro hizmetlerinizin kullanabildiği bellek miktarı, gereksinimlerinize uyacak şekilde ölçeklendirilebilir veya azaltılabilir.  Ölçeklendirme, saniyeler içinde etkili olur ve kod değişikliği veya yeniden dağıtım gerektirmez.  Daha fazla bilgi edinmek için bu [öğreticiyi](spring-cloud-tutorial-scale-manual.md)izleyin.

## <a name="application-monitoring"></a>Uygulama Izleme

### <a name="monitor-your-application-using-distributed-tracing-and-azure-app-insights"></a>Dağıtılmış izleme ve Azure Uygulama öngörülerini kullanarak uygulamanızı izleyin

Yay bulutu 'nın dağıtılmış izleme araçları, geliştiricilerin uygulamasındaki mikro hizmetler arasındaki karmaşık bağlantıları hata ayıklamasına ve izlemesine olanak sağlar.  Azure Application Insights [Spring Cloud ınuth](https://spring.io/projects/spring-cloud-sleuth) 'i tümleştirerek [](../azure-monitor/insights/insights-overview.md), Azure, doğrudan Azure Portal güçlü dağıtılmış izleme özelliği sağlar.  Daha fazla bilgi edinmek için bu [öğreticiyi](spring-cloud-tutorial-distributed-tracing.md)izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [CLı 'dan yay bulut uygulamanızı başlatın](spring-cloud-quickstart-launch-app-cli.md)
