---
title: Azure Bahar Bulutuna Giriş
description: Azure'da Java Bahar uygulamalarını dağıtmak ve yönetmek için Azure Bahar Bulutu'nun özelliklerini ve avantajlarını öğrenin.
author: bmitchell287
ms.service: spring-cloud
ms.topic: overview
ms.date: 11/4/2019
ms.author: brendm
ms.openlocfilehash: 4426044b3608be0ded378f4f56cbec6bc1948d75
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78273250"
---
# <a name="what-is-azure-spring-cloud"></a>Azure Spring Cloud nedir?

Azure İlkbahar Bulutu, Bahar Önyükleme tabanlı mikro hizmet uygulamalarını sıfır kod değişikliğiyle Azure'a dağıtmayı kolaylaştırır.  Azure Bahar Bulutu, Geliştiricilerin kodlarına odaklanabilmeleri için Bahar Bulutu uygulamalarının yaşam döngüsünü yönetir.  Spring Cloud, kapsamlı izleme ve tanılama, yapılandırma yönetimi, hizmet bulma, CI/CD tümleştirmesi, mavi-yeşil dağıtımlar ve daha fazlasını kullanarak yaşam döngüsü yönetimi sağlar.

Azure ekosisteminin bir parçası olarak Azure Bulutu, depolama, veritabanları, izleme ve daha fazlası dahil olmak üzere diğer Azure hizmetlerine kolay bağlanmaolanağı sağlar.

Azure İlkbahar Bulutu şu anda genel önizleme olarak sunulmaktadır. Genel önizleme teklifleri, müşterilerin resmi yayınlanmalarından önce yeni özelliklerle denemeler e-sayılsa.  Genel önizleme özellikleri ve hizmetleri üretim kullanımı için değildir.  Önizlemeler sırasında destek hakkında daha fazla bilgi için lütfen [SSS'mizi](https://azure.microsoft.com/support/faq/) gözden geçirin veya daha fazla bilgi edinmek için bir [Destek isteği](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) dosyalayın.

Başlamak için, [Azure CLI, Azure](spring-cloud-quickstart-launch-app-cli.md) [portalı](spring-cloud-quickstart-launch-app-portal.md)veya [Maven'i](spring-cloud-quickstart-launch-app-maven.md)kullanarak Bahar Bulutu hızlı başlat'ı tamamlayın.

GitHub: Azure Bahar [Bulut Örnekleri'nde](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql)daha fazla örnek mevcuttur.

## <a name="application-configuration"></a>Uygulama Yapılandırması

### <a name="spring-cloud-config-server"></a>Bahar Bulut Config Sunucu

Azure Yay Bulutu Config Server, hem sunucu hem de istemci tarafı desteğiyle dağıtılmış bir sistemde dışa şiselleştirilmiş yapılandırma sağlar.  Config Server, tüm ortamlardaki uygulama özelliklerini yönetmek için merkezi bir konum sağlar.  Daha fazla bilgi için [Spring Cloud Config Server başvurularını](https://spring.io/projects/spring-cloud-config.md) ziyaret edin ve öğreticiyi tamamlayın.

### <a name="enable-bluegreen-deployments"></a>Mavi/yeşil dağıtımları etkinleştirme

Azure Yay Bulutu, kod yayımlamak ve üretim ortamlarına güncellemek için mavi/yeşil dağıtımları destekler.  Bu değişiklik yönetimi deseninden yararlanmak, geliştiricilerin gerektiğinde anında geri dönüş güvenliğiyle özellikler ve kod değişiklikleri uygulamasına olanak tanır.  Azure, geliştiricilerin birden çok üretim ortamını yöneterek ve uygulamayı kesintiye uğratmadan kod değişikliklerini güncelleştirmeyi veya geri almanızı kolaylaştırarak kod yazmaya odaklanmalarına olanak tanır.  Evreleme ortamları ve mavi/yeşil dağıtımlar hakkında daha fazla bilgi edinmek için bu [Nasıl İş Yapmak Makalesi'ni ziyaret edin.](spring-cloud-howto-staging-environment.md)

### <a name="automate-cicd-pipelines"></a>CI/CD ardışık hatlarını otomatikleştirin

Azure İlkbahar Bulutu, Azure CLI'yi kullanarak Azure DevOps ile tümleştirme sağlar.  Azure DevOps'leri kullanarak, kod tümleştirmesini ve Bahar uygulamanıza dağıtımını otomatikleştirebilirsiniz.  Daha fazla bilgi için bu makaleyi ziyaret [edin.](spring-cloud-howto-cicd.md)

### <a name="scale-your-application"></a>Başvurunuzu ölçeklendirin

Azure Bahar Bulutu, Azure Bahar Bulutu panonuzdaki mikro hizmetleri kolayca ölçeklendirmenize olanak tanır.  Hem vCPUs sayısı hem de mikro hizmetlerinizin kullanabileceği bellek miktarı gereksinimlerinize uyacak şekilde büyütülebilir veya küçültülür.  Ölçekleme saniyeler içinde etkinleşir ve kod değişiklikleri veya yeniden dağıtım gerektirmez.  Daha fazla bilgi için bu [eğitimi tamamlayın.](spring-cloud-tutorial-scale-manual.md)

## <a name="application-monitoring"></a>Uygulama İzleme

### <a name="monitor-your-application-using-distributed-tracing-and-azure-app-insights"></a>Dağıtılmış izleme ve Azure Uygulama Öngörüleri kullanarak uygulamanızı izleyin

Spring Cloud'un dağıtılmış izleme araçları, geliştiricilerin bir uygulamadaki mikro hizmetler arasındaki karmaşık bağlantıları hata ayıklamalarına ve izlemelerine olanak tanır.  [Azure, Spring Cloud Sleuth'u](https://spring.io/projects/spring-cloud-sleuth) [Azure'un Uygulama Öngörüleri](../azure-monitor/insights/insights-overview.md)ile entegre ederek doğrudan Azure portalından güçlü dağıtılmış izleme özelliği sağlar.  Daha fazla bilgi için bu [eğitimi tamamlayın.](spring-cloud-tutorial-distributed-tracing.md)

## <a name="next-steps"></a>Sonraki adımlar

- [CLI'dan Bahar Bulutu uygulamanızı başlatın](spring-cloud-quickstart-launch-app-cli.md)
