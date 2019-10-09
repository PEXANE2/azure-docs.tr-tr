---
title: Redsıs için Azure önbelleğini Azure Spring Cloud uygulamanıza bağlama | Microsoft Docs
description: Reda için Azure önbelleğini Azure Spring Cloud uygulamanıza bağlamayı öğrenin
services: spring-cloud
author: v-vasuke
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/06/2019
ms.author: v-vasuke
ms.openlocfilehash: d8fa31207baecc80674fb11b492927800676b8a2
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038947"
---
# <a name="tutorial-bind-azure-services-to-your-azure-spring-cloud-application-azure-cache-for-redis"></a>Öğretici: Azure hizmetlerini Azure Spring Cloud uygulamanıza bağlama: Redsıs için Azure önbelleği

Azure yay bulutu, Spring Boot uygulamanızı el ile yapılandırmak yerine, Azure hizmetlerini uygulamalarınıza otomatik olarak bağlamanıza olanak tanır. Bu makalede, redin için uygulamanızı Azure önbelleğine bağlama işlemi gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Dağıtılan bir Azure yay bulutu örneği
* Redsıs hizmet örneği için bir Azure önbelleği
* Azure CLı için Azure yay bulutu uzantısı

Gerekirse, aşağıdaki komutu kullanarak Azure CLı için Azure yay bulutu uzantısını yüklemelisiniz:

```azurecli
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

>[!TIP]
> Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  Git, JDK, Maven ve Azure CLı 'nin en son sürümleri de dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız, shell.azure.com adresinden [Azure Cloud Shell](https://shell.azure.com) başlatın.  [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Cloud Shell hakkında daha fazla bilgi edinebilirsiniz

## <a name="bind-azure-cache-for-redis"></a>Redsıs için Azure önbelleğini bağlama

1. Projenizin @no__t aşağıdaki bağımlılığı ekleyin-0

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
    </dependency>
    ```
1. @No__t-1 dosyasında, varsa `spring.redis.*` özelliklerini kaldırın

1. @No__t-0 kullanarak geçerli dağıtımı güncelleştirin veya `az spring-cloud app deployment create` kullanarak yeni bir dağıtım oluşturun.

1. Azure portal Azure Spring Cloud Service sayfanıza gidin. **Uygulama panosunu** bulun ve Redsıs Için Azure önbelleğine bağlanacak uygulamayı seçin.  Bu, önceki adımda güncelleştirdiğiniz veya dağıttığınız bir uygulamadır. Sonra `Service binding` ' ı seçin ve `Create service binding` düğmesini seçin. Formu doldurun, `Azure Cache for Redis`, Redsıs sunucunuz ve birincil anahtar **seçeneğini seçtiğinizden emin** olun. 

1. Uygulamayı yeniden başlatın ve bu bağlamanın şimdi çalışması gerekir.

1. Hizmet bağlamasının doğru olduğundan emin olmak için bağlama adını seçin ve ayrıntılarını doğrulayın. @No__t-0 alanı şöyle görünmelidir:
    ```
    spring.redis.host=some-redis.redis.cache.windows.net
    spring.redis.port=6380
    spring.redis.password=abc******
    spring.redis.ssl=true
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Spring Cloud uygulamanızı Azure Redsıs önbelleğine bağlamayı öğrendiniz.  Uygulamanıza hizmet bağlama hakkında daha fazla bilgi edinmek için, bir uygulamayı MySQL DB 'ye bağlama öğreticisine geçin.

> [!div class="nextstepaction"]
> Azure [MySQL hizmetini Azure Spring Cloud Service 'e bağlamayı öğrenin](spring-cloud-tutorial-bind-mysql.md).