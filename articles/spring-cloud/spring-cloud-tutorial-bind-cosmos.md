---
title: Öğretici-Azure Spring Cloud uygulamanıza bir Azure Cosmos DB bağlama
description: Bu öğreticide, Azure Cosmos DB Azure Spring Cloud uygulamanıza nasıl bağlayacağınızı öğrenin
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/06/2019
ms.author: jeconnoc
ms.openlocfilehash: 8eeb67419d2da90ff1e2d2beb8cb59a85c3bbacb
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75461625"
---
# <a name="bind-an-azure-cosmos-db-database-to-your-azure-spring-cloud-application"></a>Azure Cosmos DB veritabanını Azure Spring Cloud uygulamanıza bağlama

Spring Boot uygulamalarınızı el ile yapılandırmak yerine Azure Spring Cloud kullanarak Azure hizmetlerini otomatik olarak uygulamalarınıza bağlayabilirsiniz. Bu makalede, uygulamanızı bir Azure Cosmos DB veritabanına nasıl bağlayacağınız gösterilmektedir.

Ön koşullar:

* Dağıtılmış bir Azure yay bulutu örneği. Kullanmaya başlamak için [Azure CLI aracılığıyla dağıtmaya yönelik hızlı başlangıç](spring-cloud-quickstart-launch-app-cli.md) Öğreticimizi izleyin.
* En düşük izin düzeyi katkıda bulunan Azure Cosmos DB hesabı.

## <a name="bind-azure-cosmos-db"></a>Bağlama Azure Cosmos DB

Azure Cosmos DB, bağlamayı destekleyen beş farklı API türüne sahiptir. Aşağıdaki yordamda bunların nasıl kullanılacağı gösterilmektedir:

1. Azure Cosmos DB veritabanı oluşturun. Yardım için [veritabanı oluşturma](https://docs.microsoft.com/azure/cosmos-db/create-cosmosdb-resources-portal) hızlı başlangıç kılavuzuna bakın. 

1. Veritabanınızın adını kaydedin. Bu yordam için veritabanı adı **TestDB**' dir.

1. Azure Spring Cloud uygulamanızın Pod. xml dosyasına aşağıdaki bağımlılıklardan birini ekleyin. API türü için uygun olan bağımlılığı seçin.

    * API türü: çekirdek (SQL)

      ```xml
      <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
          <version>2.1.6</version>
      </dependency>
      ```

    * API türü: MongoDB

      ```xml
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-data-mongodb</artifactId>
      </dependency>
      ```

    * API türü: Cassandra

      ```xml
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-data-cassandra</artifactId>
      </dependency>
      ```

    * API türü: Gremlin (grafik)

      ```xml
      <dependency>
          <groupId>com.microsoft.spring.data.gremlin</groupId>
          <artifactId>spring-data-gremlin</artifactId>
          <version>2.1.7</version>
      </dependency>
      ```

    * API türü: Azure tablosu

      ```xml
      <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-storage-spring-boot-starter</artifactId>
          <version>2.0.5</version>
      </dependency>
      ```

1. Geçerli dağıtımı güncelleştirmek için `az spring-cloud app update` kullanın veya yeni bir dağıtım oluşturmak için `az spring-cloud app deployment create` kullanın. Bu komutlar uygulamayı yeni bağımlılık ile güncelleştirir veya oluşturur.

1. Azure portal Azure Spring Cloud Service sayfanıza gidin. **Uygulama panosu** ' na gidin ve Azure Cosmos DB bağlanacak uygulamayı seçin. Bu uygulama, önceki adımda güncelleştirdiğiniz veya dağıttığınız aynı bir uygulamadır.

1. **Hizmet bağlaması**' nı seçin ve **hizmet bağlamayı oluştur**' u seçin. Formu doldururken şunları seçin:
   * **Bağlama türü** değeri **Azure Cosmos DB**.
   * API türü.
   * Veritabanı adınız.
   * Azure Cosmos DB hesabı.

    > [!NOTE]
    > Cassandra kullanıyorsanız, veritabanı adı için bir anahtar alanı kullanın.

1. Uygulama sayfasında **Yeniden Başlat** ' i seçerek uygulamayı yeniden başlatın.

1. Hizmetin doğru şekilde bağlandığından emin olmak için bağlama adını seçin ve ayrıntılarını doğrulayın. `property` alan bu örneğe benzer olmalıdır:

    ```
    azure.cosmosdb.uri=https://<some account>.documents.azure.com:443
    azure.cosmosdb.key=abc******
    azure.cosmosdb.database=testdb
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Spring Cloud uygulamanızı bir Azure Cosmos DB veritabanına bağlamayı öğrendiniz. Uygulamanızı Redsıs önbelleği için bir Azure önbelleğine bağlamayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Redsıs önbelleği için bir Azure önbelleğine bağlamayı öğrenin](spring-cloud-tutorial-bind-redis.md)
