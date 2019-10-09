---
title: MySQL için Azure veritabanı 'nı Azure Spring Cloud uygulamanıza bağlama | Microsoft Docs
description: Bu makalede Azure MySQL 'i Azure Spring Cloud uygulamanıza nasıl bağlayacağınız gösterilmektedir
services: spring-cloud
author: v-vasuke
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: v-vasuke
ms.openlocfilehash: e2add139f5cfd8299ec809793dd822b051d0f542
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72039024"
---
# <a name="tutorial-bind-azure-services-to-your-azure-spring-cloud-application-azure-database-for-mysql"></a>Öğretici: Azure hizmetlerini Azure Spring Cloud uygulamanıza bağlama: MySQL için Azure veritabanı

Azure yay bulutu, Spring Boot uygulamanızı el ile yapılandırmak yerine, Azure hizmetlerini uygulamalarınıza otomatik olarak bağlamanıza olanak tanır. Bu öğreticide, uygulamanızı Azure MySQL 'e nasıl bağlayacağınız gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Dağıtılan bir Azure yay bulutu örneği
* MySQL için Azure veritabanı hesabı
* Azure CLI

Gerekirse, aşağıdaki komutu kullanarak Azure CLı için Azure yay bulutu uzantısını yüklemelisiniz:

```azurecli
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

>[!TIP]
> Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  Git, JDK, Maven ve Azure CLı 'nin en son sürümleri de dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız, shell.azure.com adresinden [Azure Cloud Shell](https://shell.azure.com) başlatın.  [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Cloud Shell hakkında daha fazla bilgi edinebilirsiniz

## <a name="bind-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nı bağlama

1. Azure MySQL hesabınızın yönetici kullanıcı adını ve parolasını aklınızda yapın. Sunucuya bağlanın ve MySQL istemcisinden `testdb` adlı bir veritabanı oluşturun. Yeni yönetici olmayan bir hesap oluşturun.

1. Projenizin @no__t aşağıdaki bağımlılığı ekleyin-0

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    ```
1. @No__t-1 dosyasında, varsa `spring.datasource.*` özelliklerini kaldırın.

1. @No__t-0 kullanarak geçerli dağıtımı güncelleştirin veya bu değişiklik için `az spring-cloud app deployment create` kullanarak yeni bir dağıtım oluşturun.  Bu komutlar uygulamayı yeni bağımlılık ile güncelleştirir veya oluşturur.

1. Azure portal Azure Spring Cloud Service sayfanıza gidin. **Uygulama panosunu** bulun ve Azure MySQL 'e bağlanacak uygulamayı seçin.  Bu, önceki adımda güncelleştirdiğiniz veya dağıttığınız bir uygulamadır. Sonra `Service binding` ' ı seçin ve `Create service binding` düğmesini seçin. @No__t-1 **bağlama türünü** , daha önce kullandığınız veritabanı adını ve ilk adımda not ettiğiniz Kullanıcı adını ve parolayı seçtiğinizden emin olmak için formu doldurun.

1. Uygulamayı yeniden başlatın ve bu bağlama artık çalışmalıdır.

1. Hizmet bağlamasının doğru olduğundan emin olmak için bağlama adını seçin ve ayrıntılarını doğrulayın. @No__t-0 alanı şöyle görünmelidir:
    ```
    spring.datasource.url=jdbc:mysql://some-server.mysql.database.azure.com:3306/testdb?useSSL=true&requireSSL=false&useLegacyDatetimeCode=false&serverTimezone=UTC
    spring.datasource.username=admin@some-server
    spring.datasource.password=abc******
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Spring Cloud uygulamanızı bir MySQL DB 'ye bağlamayı öğrendiniz.  Azure yay bulut hizmetinizi yönetme hakkında daha fazla bilgi edinmek için, bkz. hizmet bulma ve kayıt hakkında bilgi edinmek için okumaya devam edin.

> [!div class="nextstepaction"]
> [Spring Cloud Service kayıt defterini kullanarak hizmet bulmayı ve kayıtlarını etkinleştirmeyi öğrenin](spring-cloud-service-registration.md).

