---
title: "Öğretici: Azure Cosmos DB'deki Cassandra API tablosuna örnek verileri yüklemek için Java uygulaması"
description: Bu öğretici, bir java uygulaması kullanarak azure Cosmos DB'deki cassandra API tablosuna örnek kullanıcı verilerinin nasıl yükleneceğimi gösterir.
author: kanshiG
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: govindk
ms.reviewer: sngun
Customer intent: As a developer, I want to build a Java application to load data to a Cassandra API table in Azure Cosmos DB so that customers can store and manage the key/value data and utilize the global distribution, elastic scaling, multi-master, and other capabilities offered by Azure Cosmos DB.
ms.openlocfilehash: 66c292bcb02e3b2b215cabe4968fa30a45422cef
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75445660"
---
# <a name="tutorial-load-sample-data-into-a-cassandra-api-table-in-azure-cosmos-db"></a>Öğretici: Azure Cosmos DB'deki Cassandra API tablosuna örnek verileri yükleyin

Geliştirici olarak, anahtar/değer çiftleri kullanan uygulamalarınız olabilir. Önemli/değer verilerini depolamak ve yönetmek için Azure Cosmos DB'deki Cassandra API hesabını kullanabilirsiniz. Bu öğretici, örnek kullanıcı verilerinin Bir Java uygulaması kullanarak Azure Cosmos DB'deki Cassandra API hesabındaki bir tabloya nasıl yükleneceğimi gösterir. Java uygulaması [Java sürücüsünü](https://github.com/datastax/java-driver) kullanır ve kullanıcı kimliği, kullanıcı adı ve kullanıcı şehri gibi kullanıcı verilerini yükler. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Verileri Cassandra tablosuna yükleme
> * Uygulamayı çalıştırma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Bu makale çok bölümlü bir öğreticiye aittir. Bu dokümanla başlamadan [önce, Cassandra API hesabı, keyspace ve tablo oluşturduğunuzdan](create-cassandra-api-account-java.md)emin olun.   

## <a name="load-data-into-the-table"></a>Tabloya veri yükleme

Cassandra API tablonuza veri yüklemek için aşağıdaki adımları kullanın:

1. "src\main\java\com\azure\cosmosdb\cassandra" klasörü altındaki "UserRepository.java" dosyasını açın ve user_id, user_name ve user_bcity alanlarını tabloya eklemek için kodu ekleyin:

   ```java
   /**
   * Insert a row into user table
   *
   * @param id   user_id
   * @param name user_name
   * @param city user_bcity
   */
   public void insertUser(PreparedStatement statement, int id, String name, String city) {
        BoundStatement boundStatement = new BoundStatement(statement);
        session.execute(boundStatement.bind(id, name, city));
   }

   /**
   * Create a PrepareStatement to insert a row to user table
   *
   * @return PreparedStatement
   */
   public PreparedStatement prepareInsertStatement() {
      final String insertStatement = "INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (?,?,?)";
   return session.prepare(insertStatement);
   }
   ```
 
2. "Src\main\java\com\azure\cosmosdb\cassandra" klasörü altında "UserProfile.java" dosyasını açın. Bu sınıf, daha önce tanımladığınız createKeyspace ve createTable yöntemlerini çağıran main yöntemini içerir. Şimdi Cassandra API tablosuna bazı örnek veriler eklemek için aşağıdaki kodu ekleyin.

   ```java
   //Insert rows into user table
   PreparedStatement preparedStatement = repository.prepareInsertStatement();
     repository.insertUser(preparedStatement, 1, "JohnH", "Seattle");
     repository.insertUser(preparedStatement, 2, "EricK", "Spokane");
     repository.insertUser(preparedStatement, 3, "MatthewP", "Tacoma");
     repository.insertUser(preparedStatement, 4, "DavidA", "Renton");
     repository.insertUser(preparedStatement, 5, "PeterS", "Everett");
   ```

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Komut istemi veya terminal penceresi açın ve klasör yolunu projeyi oluşturduğunuz yere değiştirin. Cosmosdb-cassandra-examples.jar dosyasını hedef klasörün içinde oluşturmak ve uygulamayı çalıştırmak için "mvn clean install" komutunu çalıştırın. 

```bash
cd "cassandra-demo"

mvn clean install

java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
```

Artık kullanıcı bilgilerinin tabloya eklendiğini doğrulamak için Azure portalda Veri Gezgini'ni açabilirsiniz.
    
## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Cosmos DB'deki cassandra API hesabına örnek verileri nasıl yükleyersiniz öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Cassandra API hesabından veri sorgulama](cassandra-api-query-data.md)
