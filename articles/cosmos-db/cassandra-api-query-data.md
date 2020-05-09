---
title: 'Öğretici: Azure Cosmos DB Cassandra API hesabındaki verileri sorgulama'
description: Bu öğreticide, bir Java uygulaması kullanarak Azure Cosmos DB Cassandra API hesabındaki Kullanıcı verilerinin nasıl sorgulanyapılacağı gösterilmektedir.
ms.service: cosmos-db
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.subservice: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: 28bb69dc21702a7c98755dc07389ea4147848f7a
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82853015"
---
# <a name="tutorial-query-data-from-a-cassandra-api-account-in-azure-cosmos-db"></a>Öğretici: Azure Cosmos DB Cassandra API hesabındaki verileri sorgulama

Bir geliştirici olarak, anahtar/değer çiftleri kullanan uygulamalarınız olabilir. Anahtar/değer verilerini depolamak ve sorgulamak için Azure Cosmos DB Cassandra API hesabı kullanabilirsiniz. Bu öğreticide, bir Java uygulaması kullanarak Azure Cosmos DB bir Cassandra API hesabındaki Kullanıcı verilerinin nasıl sorgulanyapılacağı gösterilmektedir. Java uygulaması, [Java sürücüsünü](https://github.com/datastax/java-driver) kullanır ve Kullanıcı kimliği, Kullanıcı adı ve Kullanıcı şehri gibi kullanıcı verilerini sorgular. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Cassandra tablosundan verileri sorgulama
> * Uygulamayı çalıştırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Bu makale çok bölümlü bir öğreticiye aittir. Başlamadan önce, Cassandra API hesabı, anahtar alanı, tabloyu oluşturmak ve [tabloya örnek veri yüklemek](cassandra-api-load-data.md)için önceki adımları tamamlamayı unutmayın. 

## <a name="query-data"></a>Verileri sorgulama

Cassandra API hesabınızdan veri sorgulaması yapmak için aşağıdaki adımları kullanın:

1. `src\main\java\com\azure\cosmosdb\cassandra` klasöründeki `UserRepository.java` dosyasını açın. Aşağıdaki kod bloğunu ekleyin. Bu kod üç yöntem sunar: 

   * Veritabanındaki tüm kullanıcıları sorgulama
   * Kullanıcı kimliğine göre filtreleyerek belirli bir kullanıcıyı sorgulama
   * Bir tabloyu silmek için

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

   Bu kod, dizini (CD) projeyi oluşturduğunuz klasör yoluyla değiştirir. Ardından hedef klasörde `cosmosdb-cassandra-examples.jar` dosyasını oluşturmak için `mvn clean install` komutunu çalıştırır. Son olarak, Java uygulamasını çalıştırır.

   ```bash
   cd "cassandra-demo"
   
   mvn clean install
   
   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
   ```

2. Şimdi Azure portalda **Veri Gezgini**'ni açın ve kullanıcı tablosunun silindiğini doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında, kaynak grubunu, Azure Cosmos hesabını ve ilgili tüm kaynakları silebilirsiniz. Bunu yapmak için, sanal makine için kaynak grubunu seçin, **Sil**' i seçin ve ardından silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Cosmos DB Cassandra API bir hesaptan verileri sorgulamayı öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Cassandra API hesabına veri geçirme](cassandra-import-data.md)


