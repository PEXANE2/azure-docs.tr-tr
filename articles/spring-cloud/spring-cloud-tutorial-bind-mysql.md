---
title: Öğretici-MySQL için Azure veritabanı örneğini Azure Spring Cloud uygulamanıza bağlama
description: Bu öğreticide, MySQL için Azure veritabanı örneğini Azure Spring Cloud uygulamanıza nasıl bağlayacağınız gösterilmektedir
author: bmitchell287
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: brendm
ms.openlocfilehash: 657aa70d77fd1af9fd2121a3e98ea3aca7773642
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76277552"
---
# <a name="tutorial-bind-an-azure-database-for-mysql-instance-to-your-azure-spring-cloud-application"></a>Öğretici: MySQL için Azure veritabanı örneğini Azure Spring Cloud uygulamanıza bağlama 

Azure Spring Cloud ile, Spring Boot uygulamanızı el ile yapılandırmak yerine, Azure hizmetlerini otomatik olarak uygulamalarınıza bağlayabilirsiniz. Bu öğreticide, uygulamanızı MySQL için Azure veritabanı örneğine nasıl bağlayacağınız gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Dağıtılan bir Azure yay bulutu örneği
* MySQL için Azure veritabanı hesabı
* Azure CLI

Dağıtılmış bir Azure yay bulut örneğiniz yoksa, ilk Spring Cloud uygulamanızı dağıtmak için [Azure Portal kullanarak Azure yay bulutu uygulamasını başlatma](spring-cloud-quickstart-launch-app-portal.md) bölümündeki yönergeleri izleyin.

## <a name="bind-your-app-to-your-azure-database-for-mysql-instance"></a>Uygulamanızı MySQL için Azure veritabanı örneğine bağlama

1. MySQL için Azure veritabanı hesabınızın yönetici kullanıcı adını ve parolasını aklınızda bir yere göz önüne alın. 

1. Sunucuya bağlanın, MySQL istemcisinden **TestDB** adlı bir veritabanı oluşturun ve ardından yeni yönetici olmayan bir hesap oluşturun.

1. Projenizin *POI. xml* dosyasında aşağıdaki bağımlılığı ekleyin:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    ```
1. *Application. Properties* dosyasında tüm `spring.datasource.*` özellikleri kaldırın.

1. Çalıştırarak `az spring-cloud app update`geçerli dağıtımı güncelleştirin veya bu değişiklik için çalıştırarak `az spring-cloud app deployment create`yeni bir dağıtım oluşturun.  Bu komutlar uygulamayı yeni bağımlılığı ile güncelleştirir veya oluşturur.

1. Azure portal **Azure Spring Cloud** Service sayfasında, **uygulama panosunu**bulun ve ardından MySQL için Azure veritabanı örneğine bağlamak üzere uygulamayı seçin.  Bu, önceki adımda güncelleştirdiğiniz veya dağıttığınız aynı uygulamadır. 

1. **Hizmet bağlaması**' nı seçin ve ardından **hizmet bağlaması oluştur** düğmesini seçin. 

1. Daha önce kullandığınız veritabanı adını kullanarak ve ilk adımda not ettiğiniz Kullanıcı adı ve parolayı kullanarak, formu doldurun, **bağlama türü**olarak **Azure MySQL** ' i seçin.

1. Uygulamayı yeniden başlatın ve bu bağlama artık çalışmalıdır.

1. Hizmet bağlamasının doğru olduğundan emin olmak için bağlama adını seçin ve sonra ayrıntılarını doğrulayın. `property` Alan şöyle görünmelidir:
    ```
    spring.datasource.url=jdbc:mysql://some-server.mysql.database.azure.com:3306/testdb?useSSL=true&requireSSL=false&useLegacyDatetimeCode=false&serverTimezone=UTC
    spring.datasource.username=admin@some-server
    spring.datasource.password=abc******
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Spring Cloud uygulamanızı bir MySQL için Azure veritabanı örneğine nasıl bağlayacağınız hakkında daha fazla öğrendiniz.  Azure yay bulut hizmetinizi yönetme hakkında daha fazla bilgi edinmek için hizmet bulma ve kayıt hakkındaki makaleye bakın.

> [!div class="nextstepaction"]
> [Spring Cloud Service kayıt defteri 'ni kullanarak hizmet bulmayı ve kaydını etkinleştirme](spring-cloud-service-registration.md)
