---
title: Öğretici - MySQL örneği için Azure Veritabanını Azure Bulut u uygulamanıza bağlama
description: Bu öğretici, MySQL örneği için bir Azure Veritabanını Azure Bulut u uygulamanıza nasıl bağladığınızı gösterir
author: bmitchell287
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: brendm
ms.openlocfilehash: 657aa70d77fd1af9fd2121a3e98ea3aca7773642
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76277552"
---
# <a name="tutorial-bind-an-azure-database-for-mysql-instance-to-your-azure-spring-cloud-application"></a>Öğretici: MySQL örneği için Azure Veritabanını Azure İlkbahar Bulutu uygulamanıza bağlama 

Azure İlkbahar Bulutu ile, Bahar Önyükleme uygulamanızı el ile yapılandırmak zorunda kalmak yerine belirli Azure hizmetlerini uygulamalarınız için otomatik olarak bağlayabilirsiniz. Bu öğretici, MySQL örneği için uygulamanızı Azure Veritabanınıza nasıl bağlayabildiğinizi gösterir.

## <a name="prerequisites"></a>Ön koşullar

* Dağıtılmış bir Azure İlkbahar Bulutu örneği
* MySQL hesabı için bir Azure Veritabanı
* Azure CLI

Dağıtılmış bir Azure İlkbahar Bulutu örneğiniz yoksa, Quickstart: İlk Bahar Bulutu uygulamanızı dağıtmak için [Azure portalını kullanarak bir Azure Bahar Bulutu uygulamasını başlatın'](spring-cloud-quickstart-launch-app-portal.md) daki yönergeleri izleyin.

## <a name="bind-your-app-to-your-azure-database-for-mysql-instance"></a>MySQL örneği için uygulamanızı Azure Veritabanınıza bağla

1. MySQL hesabı için Azure Veritabanınızın yönetici kullanıcı adını ve parolasını not edin. 

1. Sunucuya bağlanın, MySQL istemcisinden **testdb** adında bir veritabanı oluşturun ve ardından yönetici olmayan yeni bir hesap oluşturun.

1. Projenizin *pom.xml* dosyasına aşağıdaki bağımlılıkları ekleyin:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    ```
1. *application.properties* dosyasında, tüm `spring.datasource.*` özellikleri kaldırın.

1. Geçerli dağıtımı çalıştırarak `az spring-cloud app update`güncel güncelleştirme veya çalıştırarak `az spring-cloud app deployment create`bu değişiklik için yeni bir dağıtım oluşturun.  Bu komutlar, yeni bağımlılıkla uygulamayı güncelleştirin veya oluşturur.

1. Azure portalında, **Azure İlkbahar Bulutu** hizmet sayfanızda **Uygulama Panosu'nu**arayın ve ardından MySQL örneği için Azure Veritabanınıza bağlatıiçin uygulamayı seçin.  Bu, önceki adımda güncelleştirdiğiniz veya dağıtdığınız uygulamadır. 

1. **Hizmet bağlama'yı**seçin ve ardından Hizmet **Bağlama Oluştur** düğmesini seçin. 

1. Formu doldurun, Daha önce kullandığınız veritabanı adını kullanarak ve ilk adımda belirttiğiniz kullanıcı adı ve parolayı kullanarak **Bağlama türü**olarak Azure **MySQL'i** seçin.

1. Uygulamayı yeniden başlatın ve bu bağlama nın artık çalışması gerekir.

1. Hizmet bağlamanın doğru olduğundan emin olmak için bağlama adını seçin ve ayrıntılarını doğrulayın. Alan `property` şu na benzemelidir:
    ```
    spring.datasource.url=jdbc:mysql://some-server.mysql.database.azure.com:3306/testdb?useSSL=true&requireSSL=false&useLegacyDatetimeCode=false&serverTimezone=UTC
    spring.datasource.username=admin@some-server
    spring.datasource.password=abc******
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Bahar Bulutu uygulamanızı MySQL örneği için bir Azure Veritabanı'na bağlamayı öğrendiniz.  Azure Bahar Bulutu hizmetinizi yönetme hakkında daha fazla bilgi edinmek için hizmet bulma ve kayıt hakkındaki makaleye bakın.

> [!div class="nextstepaction"]
> [Bahar Bulutu Hizmet Kayıt Defteri'ni kullanarak hizmet bulma ve kayda olanak sağlama](spring-cloud-service-registration.md)
