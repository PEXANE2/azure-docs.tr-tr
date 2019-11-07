---
title: MySQL için Azure veritabanı 'nı Azure Spring Cloud uygulamanıza bağlama | Microsoft Docs
description: Bu makalede Azure MySQL 'i Azure Spring Cloud uygulamanıza nasıl bağlayacağınız gösterilmektedir
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: jeconnoc
ms.openlocfilehash: b6de5bb3b25c111d1b7775ea9570a4ae2cf45042
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607582"
---
# <a name="tutorial-bind-azure-services-to-your-azure-spring-cloud-application-azure-database-for-mysql"></a>Öğretici: Azure hizmetlerini Azure Spring Cloud uygulamanıza bağlama: MySQL için Azure veritabanı

Azure yay bulutu, Spring Boot uygulamanızı el ile yapılandırmak yerine, Azure hizmetlerini uygulamalarınıza otomatik olarak bağlamanıza olanak tanır. Bu öğreticide, uygulamanızı Azure MySQL 'e nasıl bağlayacağınız gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Dağıtılan bir Azure yay bulutu örneği
* MySQL için Azure veritabanı hesabı
* Azure CLI

Dağıtılmış bir Azure yay bulut örneğiniz yoksa, ilk Spring Cloud uygulamanızı dağıtmak için bu [hızlı](spring-cloud-quickstart-launch-app-portal.md) başlangıçta bulunan adımları izleyin.

## <a name="bind-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nı bağlama

1. Azure MySQL hesabınızın yönetici kullanıcı adını ve parolasını aklınızda yapın. Sunucuya bağlanın ve MySQL istemcisinden `testdb` adlı bir veritabanı oluşturun. Yeni yönetici olmayan bir hesap oluşturun.

1. Aşağıdaki bağımlılığı projenizin `pom.xml` ekleyin

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    ```
1. Varsa, `application.properties` dosyasında `spring.datasource.*` özelliklerini kaldırın.

1. `az spring-cloud app update` kullanarak geçerli dağıtımı güncelleştirin veya bu değişiklik için `az spring-cloud app deployment create`kullanarak yeni bir dağıtım oluşturun.  Bu komutlar uygulamayı yeni bağımlılık ile güncelleştirir veya oluşturur.

1. Azure portal Azure Spring Cloud Service sayfanıza gidin. **Uygulama panosunu** bulun ve Azure MySQL 'e bağlanacak uygulamayı seçin.  Bu, önceki adımda güncelleştirdiğiniz veya dağıttığınız bir uygulamadır. Sonra `Service binding` ' yi seçin ve `Create service binding` düğmesini seçin. **Bağlama türü** `Azure MySQL`, daha önce kullandığınız veritabanı adı ve ilk adımda not ettiğiniz Kullanıcı adı ve parola ' yı seçtiğinizden emin olmak için formu doldurun.

1. Uygulamayı yeniden başlatın ve bu bağlama artık çalışmalıdır.

1. Hizmet bağlamasının doğru olduğundan emin olmak için bağlama adını seçin ve ayrıntılarını doğrulayın. `property` alanı şöyle görünmelidir:
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

