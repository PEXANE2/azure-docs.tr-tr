---
title: Öğretici-Redsıs için Azure önbelleğini Azure Spring Cloud uygulamanıza bağlama
description: Bu öğreticide, Reda için Azure önbelleğini Azure Spring Cloud uygulamanıza nasıl bağlayacağınız gösterilmektedir.
author: bmitchell287
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/31/2019
ms.author: brendm
ms.openlocfilehash: 94f7b5a2363b7c53e0f70500e5a0a8cb6f64e611
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76277515"
---
# <a name="bind-azure-cache-for-redis-to-your-azure-spring-cloud-application"></a>Redsıs için Azure önbelleğini Azure Spring Cloud uygulamanıza bağlama 

Spring Boot uygulamalarınızı el ile yapılandırmak yerine Azure Spring Cloud kullanarak Azure hizmetlerini otomatik olarak uygulamalarınıza bağlayabilirsiniz. Bu makalede, Redsıs için uygulamanızı Azure önbelleğine bağlama işlemi gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Dağıtılan bir Azure yay bulutu örneği
* Redsıs hizmet örneği için bir Azure önbelleği
* Azure CLı için Azure yay bulutu uzantısı

Dağıtılmış bir Azure yay bulut örneğiniz yoksa, [Azure yay bulutu uygulamasını dağıtmaya yönelik hızlı başlangıçta](spring-cloud-quickstart-launch-app-portal.md)bulunan adımları izleyin.

## <a name="bind-azure-cache-for-redis"></a>Redsıs için Azure önbelleğini bağlama

1. Aşağıdaki bağımlılığı projenizin polım. xml dosyasına ekleyin:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
    </dependency>
    ```
1. `application.properties` Dosyadaki tüm `spring.redis.*` özellikleri Kaldır

1. Kullanarak geçerli dağıtımı güncelleştirin `az spring-cloud app update` veya kullanarak `az spring-cloud app deployment create`yeni bir dağıtım oluşturun.

1. Azure portal Azure Spring Cloud Service sayfanıza gidin. **Uygulama panosu** ' na gidin ve Redsıs Için Azure önbelleğine bağlanacak uygulamayı seçin. Bu uygulama, önceki adımda güncelleştirdiğiniz veya dağıttığınız aynı bir uygulamadır.

1. **Hizmet bağlamayı** seçin ve **hizmet bağlamayı oluştur**' u seçin. Formu doldurun, **redsıs Için Azure önbelleği** **,** Redsıs sunucusu için Azure önbelleğiniz ve **birincil** anahtar seçeneğini seçtiğinizden emin olun.

1. Uygulamayı yeniden başlatın. Bağlama artık çalışmalıdır.

1. Hizmet bağlamasının doğru olduğundan emin olmak için bağlama adını seçin ve ayrıntılarını doğrulayın. `property` Alan şöyle görünmelidir:
    ```
    spring.redis.host=some-redis.redis.cache.windows.net
    spring.redis.port=6380
    spring.redis.password=abc******
    spring.redis.ssl=true
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Spring Cloud uygulamanızı Redsıs için Azure önbelleğine bağlamayı öğrendiniz. Uygulamanıza hizmet bağlama hakkında daha fazla bilgi edinmek için, bir uygulamayı MySQL için Azure veritabanı örneğine bağlama öğreticisine devam edin.

> [!div class="nextstepaction"]
> [MySQL için Azure veritabanı örneğine nasıl bağlanılacağını öğrenin](spring-cloud-tutorial-bind-mysql.md)
