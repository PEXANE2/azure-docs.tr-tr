---
title: Azure Databricks'ten Azure Cosmos DB Cassandra API'ye erişin
description: Bu makalede, Azure Veri Tuğlaları'ndan Azure Cosmos DB Cassandra API ile nasıl çalışılabilenler ele allanmıştır.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 37a06b19285c1196b5d87830ea176d4bd0d4eade
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60894020"
---
# <a name="access-azure-cosmos-db-cassandra-api-data-from-azure-databricks"></a>Azure Veri Tuğlaları'ndan Azure Cosmos DB Cassandra API verilerine erişin

Bu makalede, [Azure Veri Tuğlaları'nda](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks)Spark'tan Azure Cosmos DB Cassandra API ile nasıl çalışılalı çalışacağı ayrıntılı olarak açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

* [Azure Cosmos DB Cassandra API hesabı sağlama](create-cassandra-dotnet.md#create-a-database-account)

* [Azure Cosmos DB Cassandra API'ye bağlanmanın temellerini gözden geçirin](cassandra-spark-generic.md)

* [Azure Databricks kümesini sağlama](../azure-databricks/quickstart-create-databricks-workspace-portal.md)

* [Cassandra API ile çalışmak için kod örneklerini gözden geçirin](cassandra-spark-generic.md#next-steps)

* [Eğer çok tercih ederseniz doğrulama için cqlsh kullanın](cassandra-spark-generic.md#connecting-to-azure-cosmos-db-cassandra-api-from-spark)

* **Cassandra konektörü için Cassandra API örnek yapılandırması:**

  Cassandra API'nin bağlayıcısı, Cassandra bağlantı ayrıntılarının kıvılcım bağlamının bir parçası olarak başlatılmasını gerektirir. Bir Databricks dizüstü bilgisayar başlattığınızda, kıvılcım bağlamı zaten başlatılır ve durdurmak ve yeniden başlatmanız tavsiye edilmez. Bir çözüm, Cassandra API örnek yapılandırmasını küme düzeyinde, küme kıvılcım yapılandırmasına eklemektir. Bu küme başına bir kerelik bir etkinliktir. Boşluk ayrılmış anahtar değer çifti olarak Spark yapılandırmasına aşağıdaki kodu ekleyin:
 
  ```scala
  spark.cassandra.connection.host YOUR_COSMOSDB_ACCOUNT_NAME.cassandra.cosmosdb.azure.com
  spark.cassandra.connection.port 10350
  spark.cassandra.connection.ssl.enabled true
  spark.cassandra.auth.username YOUR_COSMOSDB_ACCOUNT_NAME
  spark.cassandra.auth.password YOUR_COSMOSDB_KEY
  ```

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

* **Cassandra Spark konektörü:** - Azure Cosmos DB Cassandra API'sini Spark ile entegre etmek için Cassandra konektörü Azure Databricks kümesine iliştirilmelidir. Kümeeklemek için:

  * Databricks çalışma zamanı sürümünü, Spark sürümünü gözden geçirin. Ardından Cassandra Spark konektörüne uygun [maven koordinatlarını](https://mvnrepository.com/artifact/com.datastax.spark/spark-cassandra-connector) bulun ve kümeye takın. Bağlayıcı kitaplığını kümeye eklemek için ["Maven paketi veya Kıvılcım paketi yükle"](https://docs.databricks.com/user-guide/libraries.html) makalesine bakın. Örneğin, "Databricks Runtime sürüm 4.3", "Spark 2.3.1" ve "Scala 2.11" için maven koordinat`spark-cassandra-connector_2.11-2.3.1`

* **Azure Cosmos DB Cassandra API'ye özel kitaplık:** - Cassandra Spark bağlayıcısından Azure Cosmos DB Cassandra API'ye yeniden deneme ilkesini yapılandırmak için özel bir bağlantı fabrikası gereklidir. `com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0`Kitaplığı kümeye eklemek için [maven koordinatlarını](https://search.maven.org/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper/1.0.0/jar) ekleyin.

## <a name="sample-notebooks"></a>Örnek not defterleri

İndirmeniz için GitHub repo'da Azure Databricks [örnek not defterlerinin](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-databricks/tree/master/notebooks/scala) listesi mevcuttur. Bu örnekler, Spark'tan Azure Cosmos DB Cassandra API'ye nasıl bağlanıştırılan ve veriler üzerinde farklı CRUD işlemleri gerçekleştirmek için içerir. Ayrıca [tüm not defterlerini](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-databricks/tree/master/dbc) Databricks küme çalışma alanına aktarabilir ve çalıştırabilirsiniz. 

## <a name="accessing-azure-cosmos-db-cassandra-api-from-spark-scala-programs"></a>Spark Scala programlarından Azure Cosmos DB Cassandra API'ye erişim

Azure Veri Tuğlaları'nda otomatik işlemler olarak çalıştırılacak kıvılcım [programları, kıvılcım gönder](https://spark.apache.org/docs/latest/submitting-applications.html)kullanılarak kümeye gönderilir ve Azure Databricks işlerinde çalıştırılmak üzere zamanlanır.

Azure Cosmos DB Cassandra API ile etkileşimkurmak için Spark Scala programları oluşturmaya başlamanıza yardımcı olacak bağlantılar aşağıda veda edilmiştir.
* [Bir Spark Scala programından Azure Cosmos DB Cassandra API'ye bağlanma](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-connector-sample/blob/master/src/main/scala/com/microsoft/azure/cosmosdb/cassandra/SampleCosmosDBApp.scala)
* [Azure Veri Tuğlaları'nda otomatik bir iş olarak Bir Spark Scala programı nasıl çalıştırılır?](https://docs.azuredatabricks.net/user-guide/jobs.html)
* [Cassandra API ile çalışmak için kod örneklerinin tam listesi](cassandra-spark-generic.md#next-steps)

## <a name="next-steps"></a>Sonraki adımlar

Java uygulamasını kullanarak [Cassandra API hesabı, veritabanı ve tablo oluşturmaya](create-cassandra-api-account-java.md) başlayın.
