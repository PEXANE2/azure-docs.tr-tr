---
title: 'Öğretici: Azure Cosmos DB içindeki bir Cassandra API tabloya örnek veri yüklemek için Java uygulaması'
description: Bu öğreticide, bir Java uygulaması kullanılarak Azure Cosmos DB bir Cassandra API tabloya örnek Kullanıcı verilerinin nasıl yükleneceği gösterilmektedir.
author: kanshiG
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: govindk
ms.reviewer: sngun
Customer intent: As a developer, I want to build a Java application to load data to a Cassandra API table in Azure Cosmos DB so that customers can store and manage the key/value data and utilize the global distribution, elastic scaling, multi-master, and other capabilities offered by Azure Cosmos DB.
ms.openlocfilehash: 66c292bcb02e3b2b215cabe4968fa30a45422cef
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75445660"
---
# <a name="tutorial-load-sample-data-into-a-cassandra-api-table-in-azure-cosmos-db"></a>Öğretici: Azure Cosmos DB bir Cassandra API tabloya örnek veri yükleme

Bir geliştirici olarak, anahtar/değer çiftleri kullanan uygulamalarınız olabilir. Anahtar/değer verilerini depolamak ve yönetmek için Azure Cosmos DB Cassandra API hesabı kullanabilirsiniz. Bu öğreticide, bir Java uygulaması kullanarak Azure Cosmos DB bir Cassandra API hesabındaki bir tabloya örnek Kullanıcı verilerinin nasıl yükleneceği gösterilmektedir. Java uygulaması, [Java sürücüsünü](https://github.com/datastax/java-driver) kullanır ve Kullanıcı kimliği, Kullanıcı adı ve Kullanıcı şehri gibi kullanıcı verilerini yükler. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Cassandra tablosuna veri yükleme
> * Uygulamayı çalıştırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Bu makale çok bölümlü bir öğreticiye aittir. Bu belgeye başlamadan önce [Cassandra API hesabını, anahtar alanını ve tabloyu](create-cassandra-api-account-java.md) oluşturduğunuzdan emin olun.   

## <a name="load-data-into-the-table"></a>Tabloya veri yükleme

Cassandra API tablonuza veri yüklemek için aşağıdaki adımları kullanın:

1. "Src\main\java\com\azure\cosmosdb\cassandra" klasörünün altında "UserRepository. Java" dosyasını açın ve tabloya user_id, user_name ve user_bcity alanları eklemek için kodu ekleyin:

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
 
2. "Src\main\java\com\azure\cosmosdb\cassandra" klasörünün altında "UserProfile. Java" dosyasını açın. Bu sınıf, daha önce tanımladığınız createKeyspace ve createTable yöntemlerini çağıran main yöntemini içerir. Şimdi Cassandra API tablosuna bazı örnek veriler eklemek için aşağıdaki kodu ekleyin.

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

Bir komut istemi veya Terminal penceresi açın ve klasör yolunu projeyi oluşturduğunuz yere değiştirin. Hedef klasör içinde cosmosdb-Cassandra-Examples. jar dosyasını oluşturmak ve uygulamayı çalıştırmak için "MVN Clean install" komutunu çalıştırın. 

```bash
cd "cassandra-demo"

mvn clean install

java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
```

Artık kullanıcı bilgilerinin tabloya eklendiğini doğrulamak için Azure portalda Veri Gezgini'ni açabilirsiniz.
    
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Cosmos DB bir Cassandra API hesabına örnek veri yüklemeyi öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Cassandra API hesabından veri sorgulama](cassandra-api-query-data.md)
