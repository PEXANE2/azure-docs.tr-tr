---
title: Öğretici - Redis için Azure Önbelleğini Azure İlkbahar Bulutu uygulamanıza bağla
description: Bu öğretici, Redis için Azure Önbelleğini Azure İlkbahar Bulutu uygulamanıza nasıl bağladığınızı gösterir
author: bmitchell287
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/31/2019
ms.author: brendm
ms.openlocfilehash: 94f7b5a2363b7c53e0f70500e5a0a8cb6f64e611
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76277515"
---
# <a name="bind-azure-cache-for-redis-to-your-azure-spring-cloud-application"></a>Redis için Azure Önbelleğini Azure İlkbahar Bulutu uygulamanıza bağlama 

Bahar Önyükleme uygulamalarınızı el ile yapılandırmak yerine, Azure Bulutu'nu kullanarak belirli Azure hizmetlerini uygulamalarınız için otomatik olarak bağlayabilirsiniz. Bu makalede, redis için Azure Önbelleği'ne uygulamanızı nasıl bağlarınız gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Dağıtılmış bir Azure İlkbahar Bulutu örneği
* Redis hizmet örneği için bir Azure Önbelleği
* Azure CLI için Azure İlkbahar Bulutu uzantısı

Dağıtılmış bir Azure İlkbahar Bulutu örneğiniz yoksa, [bir Azure İlkbahar Bulutu uygulamasını dağıtmaya hızlı başlangıç](spring-cloud-quickstart-launch-app-portal.md)adımlarını izleyin.

## <a name="bind-azure-cache-for-redis"></a>Redis için Azure Önbelleğini Bağla

1. Projenizin pom.xml dosyasına aşağıdaki bağımlılığı ekleyin:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
    </dependency>
    ```
1. Dosyadaki `spring.redis.*` `application.properties` tüm özellikleri kaldırma

1. Geçerli dağıtımı kullanarak `az spring-cloud app update` güncelleştirin veya `az spring-cloud app deployment create`yeni bir dağıtım oluşturun.

1. Azure portalındaki Azure İlkbahar Buluthizmet sayfanıza gidin. Uygulama **Panosu'na** gidin ve Redis için Azure Önbelleğine bağlatıiçin uygulamayı seçin. Bu uygulama, önceki adımda güncelleştirdiğiniz veya dağıttığınız uygulamayla aynıdır.

1. **Hizmet bağlamayı** seçin ve **Hizmet bağlayıcısı oluştur'u**seçin. Redis için **Bağlayıcı tür** değeri Azure **Önbelleği, Redis**için Azure Önbelleği ve **Birincil** anahtar seçeneğini seçtiğinizden emin olmak için formu doldurun.

1. Uygulamayı yeniden başlatın. Bağlama şimdi çalışması gerekir.

1. Hizmet bağlamanın doğru olduğundan emin olmak için bağlama adını seçin ve ayrıntılarını doğrulayın. Alan `property` şu na benzemelidir:
    ```
    spring.redis.host=some-redis.redis.cache.windows.net
    spring.redis.port=6380
    spring.redis.password=abc******
    spring.redis.ssl=true
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure İlkbahar Bulutu uygulamanızı Redis için Azure Önbelleği'ne bağlamayı öğrendiniz. Uygulamanıza bağlanma hizmetleri hakkında daha fazla bilgi edinmek için, mySQL örneği için bir Azure Veritabanına uygulama bağlama öğreticisine devam edin.

> [!div class="nextstepaction"]
> [MySQL örneği için Azure Veritabanına nasıl bağlanıp bağlanılamayı öğrenin](spring-cloud-tutorial-bind-mysql.md)
