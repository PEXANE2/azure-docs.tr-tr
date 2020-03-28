---
title: Öğretici - Azure Cosmos DB'yi Azure İlkbahar Bulutu uygulamanıza bağlama
description: Bu eğitimde, Azure Cosmos DB'yi Azure İlkbahar Bulutu uygulamanıza nasıl bağlatıyarız öğrenin
author: bmitchell287
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/06/2019
ms.author: brendm
ms.openlocfilehash: 1566b6ab59e858217adcf6818e1d62f851f37eb1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76277559"
---
# <a name="bind-an-azure-cosmos-db-database-to-your-azure-spring-cloud-application"></a>Azure Cosmos DB veritabanını Azure İlkbahar Bulutu uygulamanıza bağlama

Bahar Önyükleme uygulamalarınızı el ile yapılandırmak yerine, Azure Bulutu'nu kullanarak belirli Azure hizmetlerini uygulamalarınız için otomatik olarak bağlayabilirsiniz. Bu makalede, uygulamanızın Azure Cosmos DB veritabanına nasıl bağlanılmayı gösterin.

Ön koşullar:

* Dağıtılmış bir Azure İlkbahar Bulutu örneği. Başlamak [için Azure CLI üzerinden dağıtıma hızlı bir şekilde başlamamızı](spring-cloud-quickstart-launch-app-cli.md) takip edin.
* En az katılımcı izin düzeyine sahip bir Azure Cosmos DB hesabı.

## <a name="bind-azure-cosmos-db"></a>Azure Cosmos DB'yi Bağlama

Azure Cosmos DB'de bağlamayı destekleyen beş farklı API türü vardır. Aşağıdaki yordam, bunların nasıl kullanılacağını gösterir:

1. Azure Cosmos DB veritabanı oluşturun. Yardım için bir [veritabanı oluşturmaya](https://docs.microsoft.com/azure/cosmos-db/create-cosmosdb-resources-portal) hızlı başlamaya bakın. 

1. Veritabanınızın adını kaydedin. Bu yordam için veritabanı adı **testdb'dir.**

1. Azure İlkbahar Bulutu uygulamanızın pom.xml dosyasına aşağıdaki bağımlılıklardan birini ekleyin. API türünüze uygun bağımlılığı seçin.

    * API türü: Çekirdek (SQL)

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

    * API türü: Azure Tablosu

      ```xml
      <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-storage-spring-boot-starter</artifactId>
          <version>2.0.5</version>
      </dependency>
      ```

1. Geçerli `az spring-cloud app update` dağıtımı güncelleştirmek veya `az spring-cloud app deployment create` yeni bir dağıtım oluşturmak için kullanın. Bu komutlar, yeni bağımlılıkla uygulamayı günceller veya oluşturur.

1. Azure portalındaki Azure İlkbahar Buluthizmet sayfanıza gidin. Uygulama **Panosu'na** gidin ve Azure Cosmos DB'ye bağlamak için uygulamayı seçin. Bu uygulama, önceki adımda güncelleştirdiğiniz veya dağıttığınız uygulamayla aynıdır.

1. **Hizmet bağlamayı**seçin ve **hizmet bağlama yı**seçin. Formu doldurmak için şunları seçin:
   * **Bağlama türü** değeri **Azure Cosmos DB**.
   * API türü.
   * Veritabanı adın.
   * Azure Cosmos DB hesabı.

    > [!NOTE]
    > Cassandra kullanıyorsanız, veritabanı adı için bir anahtar alanı kullanın.

1. Uygulama sayfasında **Yeniden Başlat'ı** seçerek uygulamayı yeniden başlatın.

1. Hizmetin doğru şekilde bağlı olduğundan emin olmak için bağlama adını seçin ve ayrıntılarını doğrulayın. Alan `property` bu örneğe benzer olmalıdır:

    ```
    azure.cosmosdb.uri=https://<some account>.documents.azure.com:443
    azure.cosmosdb.key=abc******
    azure.cosmosdb.database=testdb
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure İlkbahar Bulutu uygulamanızı azure cosmos DB veritabanına bağlamayı öğrendiniz. Uygulamanızı Redis önbelleği için azure önbelleğine nasıl bağlatıracağınıöğrenmek için bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Redis önbelleği için Azure Önbelleğine nasıl bağlanacağınız hakkında bilgi edinin](spring-cloud-tutorial-bind-redis.md)
