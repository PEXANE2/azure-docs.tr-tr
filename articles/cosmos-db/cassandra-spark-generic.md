---
title: Azure Cosmos DB Cassandra API ile Spark'tan çalışma
description: Bu makale, Spark Cosmos DB Cassandra API entegrasyonu için ana sayfadır.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/01/2019
ms.openlocfilehash: cb34ea44c069f067d13a6480531a94a1a515f380
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70241236"
---
# <a name="connect-to-azure-cosmos-db-cassandra-api-from-spark"></a>Spark'tan Azure Cosmos DB Cassandra API’sine bağlama

Bu makale, Spark'ın Azure Cosmos DB Cassandra API tümleştirmesi ile ilgili bir dizi makaleden biridir. Makaleler bağlantı, Veri Tanımı Dili(DDL) işlemleri, temel Veri Düzenleme Dili(DML) işlemleri ve Spark'tan gelişmiş Azure Cosmos DB Cassandra API tümleştirmesini kapsamaktadır. 

## <a name="prerequisites"></a>Ön koşullar
* [Azure Cosmos DB Cassandra API hesabı sağlama.](create-cassandra-dotnet.md#create-a-database-account)

* Spark ortamı seçiminizi sağlama [[Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) | [Azure HDInsight-Spark](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql) | Diğerleri].

## <a name="dependencies-for-connectivity"></a>Bağlantı için bağımlılıklar
* **Cassandra için kıvılcım konektörü:** Kıvılcım konektörü Azure Cosmos DB Cassandra API'ye bağlanmak için kullanılır.  [Maven merkezinde]( https://mvnrepository.com/artifact/com.datastax.spark/spark-cassandra-connector) bulunan ve Kıvılcım ortamınızın Kıvılcım ve Scala sürümleriyle uyumlu olan konektörün sürümünü tanımlayın ve kullanın.

* **Cassandra API için Azure Cosmos DB yardımcı kitaplığı:** Kıvılcım konektörüne ek olarak, Azure Cosmos DB'den [azure-cosmos-cassandra-spark-helper]( https://search.maven.org/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper/1.0.0/jar) adlı başka bir kitaplığa ihtiyacınız vardır. Bu kitaplık özel bağlantı fabrikası ve yeniden deneme ilkesi sınıfları içerir.

  Azure Cosmos DB'deki yeniden deneme ilkesi, HTTP durum kodu 429("İstek Oranı Büyük") özel durumlarını işleyeceğiniz şekilde yapılandırılır. Azure Cosmos DB Cassandra API, bu özel durumları Cassandra yerel protokolündeki aşırı yüklü hatalara çevirir ve geri dönüşlerle yeniden deneyebilirsiniz. Azure Cosmos DB sağlanan iş ortası modelini kullandığından, giriş/çıkış oranları arttığında istek oranı sınırlaması durumları oluşur. Yeniden deneme ilkesi, kıvılcım işlerinizi, kabınız için ayrılan iş artışını bir an için aşan veri artışlarına karşı korur.

  > [!NOTE] 
  > Yeniden deneme politikası kıvılcım işlerinizi yalnızca anlık ani artışlara karşı koruyabilir. İş yükünüzü çalıştırmak için gereken yeterli RUs yapılandırılmadıysanız, yeniden deneme ilkesi geçerli değildir ve yeniden deneme ilkesi sınıfı özel durumu yeniden atar.

* **Azure Cosmos DB hesap bağlantı ayrıntıları:** Azure Cassandra API hesap adınız, hesap bitiş noktanız ve anahtarınız.
    
## <a name="spark-connector-throughput-configuration-parameters"></a>Kıvılcım konektörü iş birliş yapılandırma parametreleri

Aşağıdaki tabloda, bağlayıcı tarafından sağlanan Azure Cosmos DB Cassandra API'ye özgü iş ortası yapılandırma parametreleri listelanmaktadır. Tüm yapılandırma parametrelerinin ayrıntılı bir listesi için, Spark Cassandra Connector GitHub deposunun [yapılandırma başvuru](https://github.com/datastax/spark-cassandra-connector/blob/master/doc/reference.md) sayfasına bakın.

| **Özellik Adı** | **Varsayılan değer** | **Açıklama** |
|---------|---------|---------|
| spark.cassandra.output.batch.size.rows |  1 |Tek bir toplu iş başına satır sayısı. Bu parametreyi 1 olarak ayarlayın. Bu parametre, ağır iş yükleri için daha yüksek iş elde etmek için kullanılır. |
| spark.cassandra.connection.connections_per_executor_max  | None | Yürütme başına düğüm başına maksimum bağlantı sayısı. 10*n, bir n-düğüm Cassandra kümesinde düğüm başına 10 bağlantıya eşdeğerdir. Bu nedenle, 5 düğüm Cassandra kümesi için her düğüm başına 5 bağlantı gerekiyorsa, bu yapılandırmayı 25 olarak ayarlamanız gerekir. Bu değeri, paralellik derecesine veya kıvılcım işlerinizin yapılandırıldırıldığı yürütücü sayısına göre değiştirin.   |
| spark.cassandra.output.concurrent.writes  |  100 | Yürütücü başına oluşabilecek paralel yazma sayısını tanımlar. "batch.size.rows"u 1 olarak ayarladığınızdan, bu değeri buna göre ölçeklediğinden emin olun. Bu değeri, paralellik derecesine veya iş yükünüz için elde etmek istediğiniz iş başına göre değiştirin. |
| spark.cassandra.concurrent.reads |  512 | Yürütücü başına oluşabilecek paralel okuma sayısını tanımlar. Bu değeri, iş yükünüz için elde etmek istediğiniz paralellik derecesine veya iş yükünüze göre değiştirin  |
| spark.cassandra.output.throughput_mb_per_sec  | None | Uygulayıcı başına toplam yazma iş başına yazmayı tanımlar. Bu parametre, kıvılcım iş çıktınız için bir üst sınır olarak kullanılabilir ve cosmos kapsayıcınızın sağlanan iş bişine dayandırılabilir.   |
| spark.cassandra.input.reads_per_sec| None   | Uygulayıcı başına toplam okuma iş bilgisini tanımlar. Bu parametre, kıvılcım iş çıktınız için bir üst sınır olarak kullanılabilir ve cosmos kapsayıcınızın sağlanan iş bişine dayandırılabilir.  |
| spark.cassandra.output.batch.grouping.buffer.size |  1000  | Cassandra API'ye göndermeden önce bellekte depolanabilen tek bir kıvılcım görevi başına toplu iş sayısını tanımlar |
| spark.cassandra.connection.keep_alive_ms | 60000 | Kullanılmayan bağlantıların kullanılabilir olduğu süreyi tanımlar. | 

Kıvılcım işleriniz için beklediğiniz iş yüküne ve Cosmos DB hesabınız için sağladığınız iş başına göre bu parametrelerin iş kısmını ve paralellik derecesini ayarlayın.

## <a name="connecting-to-azure-cosmos-db-cassandra-api-from-spark"></a>Azure Cosmos DB Cassandra API'ye Kıvılcım'dan bağlanma

### <a name="cqlsh"></a>cqlsh
Aşağıdaki komutlar, cqlsh'ten Azure CosmosDB Cassandra API'ye nasıl bağlanılabildiğini ayrıntılı olarak açıklatır.  Bu, Spark'taki örnekleri çalıştırırken doğrulama için yararlıdır.<br>
**Linux/Unix/Mac'ten:**

```bash
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false
cqlsh.py YOUR-COSMOSDB-ACCOUNT-NAME.cassandra.cosmosdb.azure.com 10350 -u YOUR-COSMOSDB-ACCOUNT-NAME -p YOUR-COSMOSDB-ACCOUNT-KEY --ssl
```

### <a name="1--azure-databricks"></a>1. Azure Veri Tuğlaları
Aşağıdaki makale, Azure Veri Tuğlaları küme sağlama, Azure Cosmos DB Cassandra API'ye bağlanmak için küme yapılandırması ve DDL işlemlerini, DML işlemlerini ve daha fazlasını kapsayan birkaç örnek not defterini kapsamaktadır.<BR>
[Azure veri tuğlalarından Azure Cosmos DB Cassandra API ile çalışın](cassandra-spark-databricks.md)<BR>
  
### <a name="2--azure-hdinsight-spark"></a>2. Azure HDInsight-Kıvılcım
Aşağıdaki makale, HDinsight-Spark hizmeti, sağlama, Azure Cosmos DB Cassandra API'ye bağlanmak için küme yapılandırması ve DDL işlemlerini, DML işlemlerini ve daha fazlasını kapsayan birkaç örnek not defterini kapsamaktadır.<BR>
[Azure HDInsight-Spark'tan Azure Cosmos DB Cassandra API ile çalışın](cassandra-spark-hdinsight.md)
 
### <a name="3--spark-environment-in-general"></a>3. Genel olarak kıvılcım ortamı
Yukarıdaki bölümler Azure Spark tabanlı PaaS hizmetlerine özgü olsa da, bu bölüm herhangi bir genel Kıvılcım ortamını kapsar.  Bağlayıcı bağımlılıkları, içeri aktarma ve Kıvılcım oturumu yapılandırması aşağıda ayrıntılı olarak açıklanmıştır. "Sonraki adımlar" bölümü DDL işlemleri, DML işlemleri ve daha fazlası için kod örneklerini kapsar.  

#### <a name="connector-dependencies"></a>Bağlayıcı bağımlılıkları:

1. [Kıvılcım için Cassandra konektörü](cassandra-spark-generic.md#dependencies-for-connectivity) almak için maven koordinatları ekleyin
2. Cassandra API için [Azure Cosmos DB yardımcı kitaplığı](cassandra-spark-generic.md#dependencies-for-connectivity) için maven koordinatlarını ekleyin

#### <a name="imports"></a>Ithalat:

```scala
import org.apache.spark.sql.cassandra._
//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//CosmosDB library for multiple retry
import com.microsoft.azure.cosmosdb.cassandra
```

#### <a name="spark-session-configuration"></a>Spark oturumu yapılandırması:

```scala
//Connection-related
spark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")

//Throughput-related. You can adjust the values as needed
spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler, Azure Cosmos DB Cassandra API ile Kıvılcım tümleştirmesini göstermektedir. 
 
* [DDL işlemleri](cassandra-spark-ddl-ops.md)
* [Oluşturma/ekleme işlemleri](cassandra-spark-create-ops.md)
* [İşlemleri okuma](cassandra-spark-read-ops.md)
* [İşlemleri yukarı yaslar](cassandra-spark-upsert-ops.md)
* [İşlemleri silme](cassandra-spark-delete-ops.md)
* [Toplama işlemleri](cassandra-spark-aggregation-ops.md)
* [Tablo kopyalama işlemleri](cassandra-spark-table-copy-ops.md)
