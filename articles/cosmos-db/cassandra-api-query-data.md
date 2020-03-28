---
title: "Öğretici: Azure Cosmos DB'deki Cassandra API hesabındaki verileri sorgula"
description: Bu öğretici, bir Java uygulaması kullanarak Azure Cosmos DB Cassandra API hesabındaki kullanıcı verilerinin nasıl sorgulanacağımı gösterir.
ms.service: cosmos-db
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.subservice: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
Customer intent: As a developer, I want to build a Java application to query data stored in a Cassandra API account of Azure Cosmos DB so that customers can manage the key/value data and utilize the global distribution, elastic scaling, multi-master, and other capabilities offered by Azure Cosmos DB.
ms.openlocfilehash: 69a9bc912f2cd52e52ca6403187f993413539ecd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "60899908"
---
# <a name="tutorial-query-data-from-a-cassandra-api-account-in-azure-cosmos-db"></a>Öğretici: Azure Cosmos DB'deki Cassandra API hesabındaki verileri sorgula

Geliştirici olarak, anahtar/değer çiftleri kullanan uygulamalarınız olabilir. Anahtar/değer verilerini depolamak ve sorgulamak için Azure Cosmos DB'deki Cassandra API hesabını kullanabilirsiniz. Bu öğretici, Azure Cosmos DB'deki bir Cassandra API hesabındaki kullanıcı verilerinin Java uygulamasını kullanarak nasıl sorgulanacağımı gösterir. Java uygulaması [Java sürücüsünü](https://github.com/datastax/java-driver) kullanır ve kullanıcı kimliği, kullanıcı adı ve kullanıcı şehri gibi kullanıcı verilerini sorgular. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Cassandra tablosundan verileri sorgula
> * Uygulamayı çalıştırma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Bu makale çok bölümlü bir öğreticiye aittir. Başlamadan önce, Cassandra API hesabı, keyspace, tablo oluşturmak ve [tabloya örnek verileri yüklemek](cassandra-api-load-data.md)için önceki adımları tamamlamak için emin olun. 

## <a name="query-data"></a>Verileri sorgulama

Cassandra API hesabınızdan veri sorgulaması yapmak için aşağıdaki adımları kullanın:

1. `src\main\java\com\azure\cosmosdb\cassandra` klasöründeki `UserRepository.java` dosyasını açın. Aşağıdaki kod bloğunu ekleyin. Bu kod üç yöntem sunar: 

   * Veritabanındaki tüm kullanıcıları sorgulama
   * Kullanıcı kimliğine göre filtreleyerek belirli bir kullanıcıyı sorgulama
   * Tabloyu silmek için

   ```java
   /**
   * Select all rows from user table
   */
   public void selectAllUsers() {

     final String query = "SELECT * FROM uprofile.user";
     List<Row> rows = session.execute(query).all();

     for (Row row : rows) {
        LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
     }
   }

   /**
   * Select a row from user table
   *
   * @param id user_id
   */
   public void selectUser(int id) {
      final String query = "SELECT * FROM uprofile.user where user_id = 3";
      Row row = session.execute(query).one();

      LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
   }

   /**
   * Delete user table.
   */
   public void deleteTable() {
     final String query = "DROP TABLE IF EXISTS uprofile.user";
     session.execute(query);
   }
   ```

2. `src\main\java\com\azure\cosmosdb\cassandra` klasöründeki `UserProfile.java` dosyasını açın. Bu sınıf, daha önce tanımladığınız createKeyspace ve createTable veri ekleme yöntemlerini çağıran main yöntemini içerir. Şimdi tüm kullanıcıları veya belirli bir kullanıcıyı sorgulayan aşağıdaki kodu ekleyin:

   ```java
   LOGGER.info("Select all users");
   repository.selectAllUsers();

   LOGGER.info("Select a user by id (3)");
   repository.selectUser(3);

   LOGGER.info("Delete the users profile table");
   repository.deleteTable();
   ```

## <a name="run-the-java-app"></a>Java uygulamasını çalıştırma
1. Bir komut istemi veya terminal penceresi açın. Aşağıdaki kod bloğunu yapıştırın. 

   Bu kod, dizin (cd) projeyi oluşturduğunuz klasör yoluna değiştirir. Ardından hedef klasörde `cosmosdb-cassandra-examples.jar` dosyasını oluşturmak için `mvn clean install` komutunu çalıştırır. Son olarak, Java uygulamasını çalıştırır.

   ```bash
   cd "cassandra-demo"
   
   mvn clean install
   
   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
   ```

2. Şimdi Azure portalda **Veri Gezgini**'ni açın ve kullanıcı tablosunun silindiğini doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, kaynak grubunu, Azure Cosmos hesabını ve ilgili tüm kaynakları silebilirsiniz. Bunu yapmak için, sanal makine için kaynak grubunu seçin, **Sil'i**seçin ve ardından silmek için kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Cosmos DB'deki bir Cassandra API hesabından verileri sorgulamayı öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Cassandra API hesabına veri geçirme](cassandra-import-data.md)


