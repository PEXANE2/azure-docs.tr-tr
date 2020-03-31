---
title: Servis kayıt defterini ve keşfi otomatikleştirin
description: Bahar Bulutu Hizmet Kayıt Defteri'ni kullanarak hizmet bulma ve kaydetmeyi otomatikleştirin
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/05/2019
ms.author: brendm
ms.openlocfilehash: 6c217096f0ba4200f49bd1fd8056768a6f6f6dbd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76278850"
---
# <a name="discover-and-register-your-spring-cloud-services"></a>Spring Cloud hizmetlerinizi keşfedin ve kaydedin

Service Discovery, mikro hizmet tabanlı bir mimari için önemli bir gereksinimdir.  Her istemcinin el ile yapılandırılması zaman alır ve insan hatası olasılığını ortaya çıkarır.  Azure İlkbahar Bulut Hizmet Kayıt Defteri bu sorunu çözer.  Bir hizmet kayıt defteri sunucusu yapılandırıldıktan sonra, uygulamanızın mikro hizmetleri için hizmet kaydını ve keşfini denetler. Service Registry sunucusu dağıtılan mikro hizmetlerin bir kayıt defterini tutar, istemci tarafı yük dengelemesine olanak tanır ve servis sağlayıcılarını DNS'ye güvenmeden istemcilerden ayırabilir.

## <a name="register-your-application-using-spring-cloud-service-registry"></a>Bahar Bulutu Hizmet Kayıt Defteri'ni kullanarak başvurunuzu kaydedin

Başvurunuzun Bahar Bulutu Hizmet Kayıt Defteri'ni kullanarak hizmet kaydı nı ve keşfi yönetebilmeleri için, uygulamanın *pom.xml* dosyasına birkaç bağımlılık eklenmelidir.
*Pom.xml'inize* *spring-cloud-starter-netflix-eureka-client* ve *spring-cloud-starter-azure-cloud-client* bağımlılıklarını ekleyin

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.1.0</version>
    </dependency>
```

## <a name="update-the-top-level-class"></a>Üst düzey sınıfı güncelleştirme

Son olarak, uygulamanızın üst düzey sınıfına bir ek açıklama ekliyoruz

 ```java
    package foo.bar;

    import org.springframework.boot.SpringApplication;
    import org.springframework.cloud.client.SpringCloudApplication;

    @SpringCloudApplication
    public class DemoApplication {
        public static void main(String... args) {
            SpringApplication.run(DemoApplication.class, args);
        }
    }
 ```

Bahar Bulut Hizmet Kayıt Defteri sunucu bitiş noktası, uygulamanızda bir ortam değişkeni olarak enjekte edilecektir.  Microservices artık kendilerini Hizmet Kayıt Defteri sunucusuna kaydedebilecek ve diğer bağımlı mikro hizmetleri keşfedebilecektir.

Değişikliklerin sunucudan tüm mikro hizmetlere yayılmasının birkaç dakika sürebileceğini unutmayın.
