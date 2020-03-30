---
title: Verileri Azure Veri Gezgini ve Spark kümeleri arasında taşımak için Apache Spark için Azure Veri Gezgini bağlayıcısını kullanın.
description: Bu konu, verileri Azure Veri Gezgini ve Apache Spark kümeleri arasında nasıl taşıyabileceğinizi gösterir.
author: orspod
ms.author: orspodek
ms.reviewer: michazag
ms.service: data-explorer
ms.topic: conceptual
ms.date: 1/14/2020
ms.openlocfilehash: b358287664ac6d6a3b641e1ab63073810ceb4c40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78208633"
---
# <a name="azure-data-explorer-connector-for-apache-spark"></a>Apache Spark için Azure Veri Gezgini Bağlayıcısı

[Apache Spark,](https://spark.apache.org/) büyük ölçekli veri işleme için birleşik bir analiz motorudur. Azure Veri Gezgini, büyük hacimli veriler üzerinde gerçek zamanlı analiz için hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. 

Spark için Azure Veri Gezgini bağlayıcısı, herhangi bir Spark kümesinde çalıştırılabilen açık [kaynak kodluydu.](https://github.com/Azure/azure-kusto-spark) Verileri Azure Veri Gezgini ve Spark kümeleri arasında taşımak için veri kaynağı ve veri lavabosu uygular. Azure Veri Gezgini ve Apache Spark'ı kullanarak veri odaklı senaryoları hedefleyen hızlı ve ölçeklenebilir uygulamalar oluşturabilirsiniz. Örneğin, makine öğrenimi (ML), Extract-Transform-Load (ETL) ve Log Analytics. Azure Veri Gezgini, bağlayıcıyla yazma, okuma ve yazma Akışı gibi standart Spark kaynak ve lavabo işlemleri için geçerli bir veri deposu haline gelir.

Toplu iş veya akış modunda Azure Veri Gezgini'ne yazabilirsiniz. Azure Veri Gezgini'nden okuma, azure veri gezginindeki verileri filtreleyerek aktarılan verilerin hacmini azaltan sütun budama ve yüklem bastırmayı destekler.

Bu konu, Azure Veri Gezgini Spark bağlayıcısının nasıl yüklenir ve yapılandırılabildiğini ve verileri Azure Veri Gezgini ve Apache Spark kümeleri arasında nasıl taşıyacağını açıklar.

> [!NOTE]
> Aşağıdaki örneklerden bazıları Bir [Azure Databricks](https://docs.azuredatabricks.net/) Spark kümesine atıfta bulunsa da, Azure Veri Gezgini Spark bağlayıcısı Databricks veya diğer Kıvılcım dağılımlarına doğrudan bağımlılık sağlamaz.

## <a name="prerequisites"></a>Ön koşullar

* [Azure Veri Gezgini kümesi ve veritabanı oluşturma](/azure/data-explorer/create-cluster-database-portal) 
* Spark kümesi oluşturma
* Azure Veri Gezgini bağlayıcı kitaplığını yükleyin:
    * [Spark 2.4, Scala 2.11](https://github.com/Azure/azure-kusto-spark/releases) için önceden inşa edilmiş kütüphaneler 
    * [Maven repo](https://mvnrepository.com/artifact/com.microsoft.azure.kusto/spark-kusto-connector)
* [Maven 3.x](https://maven.apache.org/download.cgi) yüklü

> [!TIP]
> 2.3.x sürümleri de desteklenir, ancak pom.xml bağımlılıklarında bazı değişiklikler gerektirebilir.

## <a name="how-to-build-the-spark-connector"></a>Kıvılcım konektörnasıl inşa edilir?

> [!NOTE]
> Bu adım isteğe bağlıdır. Önceden oluşturulmuş kitaplıklar kullanıyorsanız [Spark küme kurulumuna](#spark-cluster-setup)gidin.

### <a name="build-prerequisites"></a>Ön koşullar oluşturun

1. Aşağıdaki [Kusto Java SDK](/azure/kusto/api/java/kusto-java-client-library) kitaplıkları da dahil olmak üzere [bağımlılıklar](https://github.com/Azure/azure-kusto-spark#dependencies) listelenen kitaplıkları yükleyin:
    * [Kusto Veri İstemci](https://mvnrepository.com/artifact/com.microsoft.azure.kusto/kusto-data)
    * [Kusto Yutma İstemci](https://mvnrepository.com/artifact/com.microsoft.azure.kusto/kusto-ingest)

1. Kıvılcım Konektörünü oluşturmak için [bu kaynağa](https://github.com/Azure/azure-kusto-spark) bakın.

1. Maven proje tanımlarını kullanan Scala/Java uygulamaları için, uygulamanızı aşağıdaki yapıyla ilişkilendirin (en son sürüm farklı olabilir):
    
    ```Maven
       <dependency>
         <groupId>com.microsoft.azure</groupId>
         <artifactId>spark-kusto-connector</artifactId>
         <version>1.1.0</version>
       </dependency>
    ```

### <a name="build-commands"></a>Komutlar oluşturma

Kavanoz oluşturmak ve tüm testleri çalıştırmak için:

```
mvn clean package
```

Kavanoz oluşturmak için, tüm testleri çalıştırın ve yerel Maven deposuna kavanoz yükleyin:

```
mvn clean install
```

Daha fazla bilgi için [bağlayıcı kullanımına](https://github.com/Azure/azure-kusto-spark#usage)bakın.

## <a name="spark-cluster-setup"></a>Kıvılcım küme kurulumu

> [!NOTE]
> Aşağıdaki adımları gerçekleştirirken en son Azure Veri Gezgini Kıvılcım ı sürümü kullanmanız önerilir.

1. Spark 2.4.4 ve Scala 2.11 kullanarak Azure Databricks kümesini temel alan aşağıdaki Spark küme ayarlarını yapılandırın:

    ![Databricks küme ayarları](media/spark-connector/databricks-cluster.png)
    
1. Maven'den en son kıvılcım-kusto bağlayıcı kitaplığını yükleyin:
    
    ![İthalat](media/spark-connector/db-libraries-view.png) ![kitaplıkları Spark-Kusto-Bağlayıcıseçin](media/spark-connector/db-dependencies.png)

1. Gerekli tüm kitaplıkların yüklü olduğunu doğrulayın:

    ![Yüklü kitaplıkları doğrula](media/spark-connector/db-libraries-view.png)

1. JAR dosyasının kullanımı için ek sığının yüklü olduğunu doğrulay

    ![Bağımlılıkekleme](media/spark-connector/db-not-maven.png)

## <a name="authentication"></a>Kimlik doğrulaması

Azure Veri Gezgini Spark bağlayıcısı, aşağıdaki yöntemlerden birini kullanarak Azure Etkin Dizini (Azure AD) ile kimlik doğrulaması yapmanızı sağlar:
* [Azure AD uygulaması](#azure-ad-application-authentication)
* [Azure AD erişim jetonu](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#direct-authentication-with-access-token)
* [Aygıt kimlik doğrulaması](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#device-authentication) (üretim dışı senaryolar için)
* Bir [Azure Anahtar Kasası](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#key-vault) Anahtarı Kasası kaynağına erişmek için azure-keyvault paketini yükleyin ve uygulama kimlik bilgilerini sağlayın.

### <a name="azure-ad-application-authentication"></a>Azure AD uygulama kimlik doğrulaması

Azure AD uygulama kimlik doğrulaması en basit ve en yaygın kimlik doğrulama yöntemidir ve Azure Veri Gezgini Spark bağlayıcısı için önerilir.

|Özellikler  |Açıklama  |
|---------|---------|
|**KUSTO_AAD_CLIENT_ID**     |   Azure AD uygulaması (istemci) tanımlayıcısı.      |
|**KUSTO_AAD_AUTHORITY_ID**     |  Azure AD kimlik doğrulama yetkilisi. Azure AD Dizini (kiracı) Kimliği.        |
|**KUSTO_AAD_CLIENT_PASSWORD**    |    İstemci için Azure AD uygulama anahtarı.     |

### <a name="azure-data-explorer-privileges"></a>Azure Veri Gezgini ayrıcalıkları

Azure Veri Gezgini kümesinde aşağıdaki ayrıcalıkları verin:

* Okuma (veri kaynağı) için Azure AD kimliğinin hedef veritabanında *görüntüleyici* ayrıcalıkları veya hedef tabloda *yönetici* ayrıcalıkları olması gerekir.
* Yazma (veri batması) için Azure AD kimliğinin hedef veritabanında *yutma* ayrıcalıkları olması gerekir. Ayrıca, yeni tablolar oluşturmak için hedef veritabanında *kullanıcı* ayrıcalıkları olmalıdır. Hedef tablo zaten varsa, hedef tabloda *yönetici* ayrıcalıklarını yapılandırmanız gerekir.
 
Azure Veri Gezgini temel rolleri hakkında daha fazla bilgi için [rol tabanlı yetkilendirmeye](/azure/kusto/management/access-control/role-based-authorization)bakın. Güvenlik rollerini yönetmek için [güvenlik rolleri yönetimine](/azure/kusto/management/security-roles)bakın.

## <a name="spark-sink-writing-to-azure-data-explorer"></a>Kıvılcım lavabosu: Azure Veri Gezgini'ne yazma

1. Lavabo parametrelerini ayarlama:

     ```scala
    val KustoSparkTestAppId = dbutils.secrets.get(scope = "KustoDemos", key = "KustoSparkTestAppId")
    val KustoSparkTestAppKey = dbutils.secrets.get(scope = "KustoDemos", key = "KustoSparkTestAppKey")
 
    val appId = KustoSparkTestAppId
    val appKey = KustoSparkTestAppKey
    val authorityId = "72f988bf-86f1-41af-91ab-2d7cd011db47" // Optional - defaults to microsoft.com
    val cluster = "Sparktest.eastus2"
    val database = "TestDb"
    val table = "StringAndIntTable"
    ```

1. Azure Veri Gezgini kümesine Toplu Olarak Spark DataFrame yazın:

    ```scala
    import com.microsoft.kusto.spark.datasink.KustoSinkOptions
    import org.apache.spark.sql.{SaveMode, SparkSession}

    df.write
      .format("com.microsoft.kusto.spark.datasource")
      .option(KustoSinkOptions.KUSTO_CLUSTER, cluster)
      .option(KustoSinkOptions.KUSTO_DATABASE, database)
      .option(KustoSinkOptions.KUSTO_TABLE, "Demo3_spark")
      .option(KustoSinkOptions.KUSTO_AAD_CLIENT_ID, appId)
      .option(KustoSinkOptions.KUSTO_AAD_CLIENT_PASSWORD, appKey)
      .option(KustoSinkOptions.KUSTO_AAD_AUTHORITY_ID, authorityId)
      .option(KustoSinkOptions.KUSTO_TABLE_CREATE_OPTIONS, "CreateIfNotExist")
      .mode(SaveMode.Append)
      .save()  
    ```
    
   Veya basitleştirilmiş sözdizimini kullanın:
   
    ```scala
         import com.microsoft.kusto.spark.datasink.SparkIngestionProperties
         import com.microsoft.kusto.spark.sql.extension.SparkExtension._
         
         val sparkIngestionProperties = Some(new SparkIngestionProperties()) // Optional, use None if not needed
         df.write.kusto(cluster, database, table, conf, sparkIngestionProperties)
    ```
   
1. Akış verilerini yazma:

    ```scala    
    import org.apache.spark.sql.streaming.Trigger
    import java.util.concurrent.TimeUnit
    import java.util.concurrent.TimeUnit
    import org.apache.spark.sql.streaming.Trigger

    // Set up a checkpoint and disable codeGen. 
    spark.conf.set("spark.sql.streaming.checkpointLocation", "/FileStore/temp/checkpoint")
        
    // Write to a Kusto table from a streaming source
    val kustoQ = df
          .writeStream
          .format("com.microsoft.kusto.spark.datasink.KustoSinkProvider")
          .options(conf) 
          .option(KustoSinkOptions.KUSTO_WRITE_ENABLE_ASYNC, "true") // Optional, better for streaming, harder to handle errors
          .trigger(Trigger.ProcessingTime(TimeUnit.SECONDS.toMillis(10))) // Sync this with the ingestionBatching policy of the database
          .start()
    ```

## <a name="spark-source-reading-from-azure-data-explorer"></a>Kıvılcım kaynağı: Azure Veri Gezgini'nden okuma

1. Az [miktarda veri](/azure/kusto/concepts/querylimits)okurken, veri sorgusunu tanımlayın:

    ```scala
    import com.microsoft.kusto.spark.datasource.KustoSourceOptions
    import org.apache.spark.SparkConf
    import org.apache.spark.sql._
    import com.microsoft.azure.kusto.data.ClientRequestProperties

    val query = s"$table | where (ColB % 1000 == 0) | distinct ColA"
    val conf: Map[String, String] = Map(
          KustoSourceOptions.KUSTO_AAD_CLIENT_ID -> appId,
          KustoSourceOptions.KUSTO_AAD_CLIENT_PASSWORD -> appKey
        )

    val df = spark.read.format("com.microsoft.kusto.spark.datasource").
      options(conf).
      option(KustoSourceOptions.KUSTO_QUERY, query).
      option(KustoSourceOptions.KUSTO_DATABASE, database).
      option(KustoSourceOptions.KUSTO_CLUSTER, cluster).
      load()

    // Simplified syntax flavor
    import com.microsoft.kusto.spark.sql.extension.SparkExtension._
    
    val cpr: Option[ClientRequestProperties] = None // Optional
    val df2 = spark.read.kusto(cluster, database, query, conf, cpr)
    display(df2)
    ```

1. İsteğe bağlı: Geçici blob depolamasını (Azure Veri Gezgini değil) **sağlarsanız,** blob'lar oluşturulan kişinin sorumluluğundadır. Buna depolama nın sağlanması, erişim anahtarlarının döndürülmesi ve geçici yapıtların silmesi dahildir. 
    KustoBlobStorageUtils modülü, hesap ve konteyner koordinatlarını ve hesap kimlik bilgilerini temel alan blobları silmek için yardımcı işlevler veya yazma, okuma ve liste izinlerini içeren tam bir SAS URL'si içerir. Karşılık gelen RDD'ye artık gerek olmadığında, her işlem geçici blob yapılarını ayrı bir dizinde depolar. Bu dizin, Spark Driver düğümünde bildirilen okuma-hareket bilgi günlüklerinin bir parçası olarak yakalanır.

    ```scala
    // Use either container/account-key/account name, or container SaS
    val container = dbutils.secrets.get(scope = "KustoDemos", key = "blobContainer")
    val storageAccountKey = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageAccountKey")
    val storageAccountName = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageAccountName")
    // val storageSas = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageSasUrl")
    ```

    Yukarıdaki örnekte, Anahtar Kasası bağlayıcı arabirimi kullanılarak erişilemez; Databricks sırlarını kullanarak daha basit bir yöntem kullanılır.

1. Azure Veri Gezgini'nden okuyun.

    * Geçici blob depolama sını **sağlıyorsanız,** Azure Veri Gezgini'nden aşağıdaki gibi okuyun:

        ```scala
         val conf3 = Map(
              KustoSourceOptions.KUSTO_AAD_CLIENT_ID -> appId,
              KustoSourceOptions.KUSTO_AAD_CLIENT_PASSWORD -> appKey
              KustoSourceOptions.KUSTO_BLOB_STORAGE_SAS_URL -> storageSas)
        val df2 = spark.read.kusto(cluster, database, "ReallyBigTable", conf3)
        
        val dfFiltered = df2
          .where(df2.col("ColA").startsWith("row-2"))
          .filter("ColB > 12")
          .filter("ColB <= 21")
          .select("ColA")
        
        display(dfFiltered)
        ```

    * **Azure Veri Gezgini** geçici blob depolamasını sağlıyorsa, Azure Veri Gezgini'nden aşağıdaki gibi okuyun:
    
        ```scala
        val dfFiltered = df2
          .where(df2.col("ColA").startsWith("row-2"))
          .filter("ColB > 12")
          .filter("ColB <= 21")
          .select("ColA")
        
        display(dfFiltered)
        ```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Veri [Gezgini Kıvılcım Bağlayıcısı](https://github.com/Azure/azure-kusto-spark/tree/master/docs) hakkında daha fazla bilgi edinin
* [Java ve Python için örnek kod](https://github.com/Azure/azure-kusto-spark/tree/master/samples/src/main)
