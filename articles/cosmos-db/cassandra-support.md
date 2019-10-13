---
title: Azure Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri ve komutları
description: Azure Cosmos DB Cassandra API'sinde Apache Cassandra özellik desteği hakkında bilgi edinin
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: overview
ms.date: 09/24/2018
ms.openlocfilehash: 0dcca2175d6ccc35a51bccb1e47f75d25cb8b11f
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72299195"
---
# <a name="apache-cassandra-features-supported-by-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri 

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB Cassandra API'si ile, Cassandra Sorgu Dili (CQL) v4 [kablo protokolü](https://github.com/apache/cassandra/blob/trunk/doc/native_protocol_v4.spec) uyumlu açık kaynak Cassandra istemci [sürücüleri](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver) aracılığıyla iletişim kurabilirsiniz. 

Azure Cosmos DB Cassandra API’sini kullanarak Apache Cassandra API’leri avantajlarının yanı sıra Azure Cosmos DB’nin sunduğu kurumsal özelliklerin keyfini çıkarabilirsiniz. [Genel dağıtım](distribute-data-globally.md), [otomatik ölçek genişletme bölümlemesi](partition-data.md), kullanılabilirlik ve gecikme süresi garantileri, REST’te şifreleme, yedeklemeler ve çok daha fazlası kurumsal özelliklere dahildir.

## <a name="cassandra-protocol"></a>Cassandra protokolü 

Azure Cosmos DB Cassandra API'si CQL’nin **v4** sürümüyle uyumludur. Desteklenen CQL komutları, araçlar, sınırlamalar ve özel durumlar aşağıda listelenmiştir. Bu protokolleri anlayan bir istemci sürücüsü Azure Cosmos DB Cassandra API'sine bağlanabilir.

## <a name="cassandra-driver"></a>Cassandra sürücü

Cassandra sürücülerinin aşağıdaki sürümleri, Azure Cosmos DB Cassandra API'si tarafından desteklenir:

* [Java 3.5+](https://github.com/datastax/java-driver)  
* [C# 3.5+](https://github.com/datastax/csharp-driver)  
* [Nodejs 3.5+](https://github.com/datastax/nodejs-driver)  
* [Python 3.15+](https://github.com/datastax/python-driver)  
* [C++ 2.9](https://github.com/datastax/cpp-driver)   
* [PHP 1.3](https://github.com/datastax/php-driver)  
* [Gocql](https://github.com/gocql/gocql)  
 
## <a name="cql-data-types"></a>CQL veri türleri 

Azure Cosmos DB Cassandra API'si aşağıdaki CQL veri türlerini destekler:

* ascii  
* bigint  
* blob  
* boole  
* counter  
* date  
* decimal  
* double  
* float  
* frozen  
* inet  
* int  
* list  
* set  
* smallint  
* metin  
* time  
* timestamp  
* timeuuid  
* tinyint  
* tuple  
* uuid  
* varchar  
* varint  
* tuples  
* udts  
* map  

## <a name="cql-functions"></a>CQL işlevleri

Azure Cosmos DB Cassandra API'si aşağıdaki CQL işlevlerini destekler:

* Belirteç  
* Toplama işlevleri
  * Min, Max, AVG, Count
* Blob dönüşüm işlevleri 
  * typeAsBlob(value)  
  * blobAsType(value)
* UUID ve timeuuid işlevleri 
  * dateOf()  
  * now()  
  * minTimeuuid()  
  * unixTimestampOf()  
  * toDate(timeuuid)  
  * toTimestamp(timeuuid)  
  * toUnixTimestamp(timeuuid)  
  * toDate(timestamp)  
  * toUnixTimestamp(timestamp)  
  * toTimestamp(date)  
  * toUnixTimestamp(date) 
  


## <a name="cassandra-query-language-limits"></a>Cassandra Sorgu Dili sınırları

Azure Cosmos DB Cassandra API'sinin bir tabloda depolanan verilerin boyutuna dair herhangi bir sınırlaması yoktur. Yüzlerce terabayt veya Petabaytlarca verinin depolanabilmesinin yanı sıra bölüm anahtarı sınırları da kabul edilir. Benzer şekilde her varlık ve satır eşdeğerinin sütun sayısı üzerinde sınırı yoktur, ancak varlığın toplam boyutu 2 MB’ı aşmamalıdır.

## <a name="tools"></a>Araçlar 

Azure Cosmos DB Cassandra API'si bir yönetilen hizmet platformudur. Kümeyi yönetmek için Atık Toplayıcı, Java Sanal Makinesi (JVM) ve düğüm aracı gibi yönetim ek yükü veya yardımcı programları gerekmez. İkili CQLv4 uyumluluğunu kullanan cqlsh gibi araçları destekler. 

* Azure portalın veri gezgini, ölçümler, günlük tanılaması, PowerShell ve CLI, hesabı yönetmek için desteklenen diğer mekanizmalardır.

## <a name="cql-shell"></a>CQL Kabuğu  

CQLSH komut satırı yardımcı programı, Apache Cassandra 3.1.1 ile birlikte gelir ve aşağıdaki ortam değişkenleri etkin olarak çalışır:

Aşağıdaki komutları çalıştırmadan önce [cacerts deposuna bir Baltimore kök sertifikası ekleyin](https://docs.microsoft.com/java/azure/java-sdk-add-certificate-ca-store?view=azure-java-stable#to-add-a-root-certificate-to-the-cacerts-store). 

**Windows:** 

```bash
set SSL_VERSION=TLSv1_2 
SSL_CERTIFICATE=<path to Baltimore root ca cert>
set CQLSH_PORT=10350 
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl 
```
**Unix/Linux/Mac:**

```bash
export SSL_VERSION=TLSv1_2 
export SSL_CERTFILE=<path to Baltimore root ca cert>
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl 
```

## <a name="cql-commands"></a>CQL komutları

Azure Cosmos DB, Cassandra API'si hesaplarında aşağıdaki veritabanı komutlarını destekler.

* ANAHTAR alanı oluştur (Bu komutun çoğaltma ayarları yoksayılır, sistem temel [Azure Cosmos DB çoğaltma modelini](global-dist-under-the-hood.md)kullanır. Bölgeler arası veri varlığına ihtiyacınız varsa, daha fazla bilgi edinmek için PowerShell, CLı veya Portal ile hesap düzeyinde etkinleştirebilir, daha fazla bilgi edinmek için bkz. [hesabınız için bölge ekleme veya kaldırma](how-to-manage-database-account.md#addremove-regions-from-your-database-account) makalesi.
* CREATE TABLE 
* ALTER TABLE 
* USE 
* INSERT 
* SELECT 
* GÜNCELLEŞTİRME 
* BATCH- Yalnızca günlüğe kaydedilmemiş komutlar desteklenir 
* DELETE

CQLV4 uyumlu SDK aracılığıyla yürütülen tüm CRUD işlemleri hata, tüketilen istek birimleri hakkında ek bilgiler döndürür. Sağlanan aktarım hızını doğru şekilde kullanmaktan kaçınmak için, silme ve güncelleştirme komutlarının, kaynak yönetimi ile birlikte işlenmesi gerekir. 
* Not: gc_grace_seconds değeri belirtilmişse sıfır olmalıdır.

```csharp
var tableInsertStatement = table.Insert(sampleEntity); 
var insertResult = await tableInsertStatement.ExecuteAsync(); 
 
foreach (string key in insertResult.Info.IncomingPayload) 
        { 
            byte[] valueInBytes = customPayload[key]; 
            double value = Encoding.UTF8.GetString(valueInBytes); 
            Console.WriteLine($“CustomPayload:  {key}: {value}”); 
        } 
```

## <a name="consistency-mapping"></a>Tutarlılık eşleme 

Azure Cosmos DB Cassandra API'si okuma işlemleri için tutarlılık sunar.  Tutarlılık eşlemesi [burada [(https://docs.microsoft.com/azure/cosmos-db/consistency-levels-across-apis#cassandra-mapping) ) ayrıntılıdır.

## <a name="permission-and-role-management"></a>İzin ve rol yönetimi

Azure Cosmos DB, sağlama, anahtar döndürme, ölçümleri görüntüleme ve [Azure Portal](https://portal.azure.com)aracılığıyla elde edilebilir salt okuma ve salt okuma parolaları/anahtarları için rol tabanlı erişim denetimi 'NI (RBAC) destekler. Azure Cosmos DB CRUD etkinlikleri için rolleri desteklemez. 

## <a name="keyspace-and-table-options"></a>Keyspace ve tablo seçenekleri

Anahtar alanı oluşturma komutunda bölge adı, sınıf, replication_factor, veri merkezi seçenekleri yok sayılır. Gerekli bölgeleri eklerseniz sistem temel Azure Cosmos DB [genel dağıtımını](https://docs.microsoft.com/en-us/azure/cosmos-db/global-dist-under-the-hood) kullanır. Bölgeler arası veri varlığına ihtiyacınız varsa, daha fazla bilgi edinmek için bu belgeyi PowerShell, CLı veya Portal ile hesap düzeyinde etkinleştirebilirsiniz, bu belgeye bakın: https://docs.microsoft.com/en-us/azure/cosmos-db/how-to-manage-database-account#addremove-regions-from-your-database-account. Cosmos DB, her yazmanın dayanıklı olduğundan emin olmak için, Durable_writes devre dışı bırakılamaz. Cosmos DB her bölgede, 4 çoğaltmalardan oluşan replicaset genelinde verileri çoğaltır ve bu replicaset [yapılandırması](https://docs.microsoft.com/en-us/azure/cosmos-db/global-dist-under-the-hood) değiştirilemez. Tüm tablo oluşturma seçenekleri, gc_grace_seconds hariç yok sayılır, sıfır olmalıdır.
Keyspace ve tablo, en düşük 400 değerine sahip ek Option-cosmosdb_provisioned_throughput. Anahtar alanı verimlilik, birden çok tablo arasında üretilen iş aktarımına ve tüm tablolar aktarım hızını kullanmadan senaryolar için yararlıdır. Alter table, bölgeler genelinde sağlanan üretilen işin değiştirilmesini sağlar. YINELEME = {' class ' Ile KEYSPACE örneklemeler oluşturma: ' Simplestrateji '} ve cosmosdb_provisioned_throughput = 2000;  
Cosmosdb_provisioned_throughput = 2000 Ile sampleks. T1 (user_id int PRIMARY KEY, LastName Text) CREATE TABLE ALTER TABLE gks1. T1 WITH cosmosdb_provisioned_throughput = 10000;

## <a name="usage-of-cassandra-retry-connection-policy"></a>Cassandra yeniden deneme bağlantı ilkesi kullanımı

Azure Cosmos DB kaynak tarafından yönetilen sistemdir. Bu, belirli bir saniye içinde, işlemler tarafından tüketilen istek birimlerine göre sağlanan aktarım hızı ile sınırlı sayıda işlem yapabileceğiniz anlamına gelir. Uygulama, belirtilen bir ikinci istek hızının bu sınırı aşarsa özel durumlar atılır. Cosmos DB Cassandra API, bu özel durumları Cassandra Native protokolünde aşırı yüklenmiş hatalara dönüştürür. Uygulamanızın hız sınırlaması için yeniden deneme işlemini yapabildiğinden ve bir [Spark](https://mvnrepository.com/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper) ve [Java](https://github.com/Azure/azure-cosmos-cassandra-extensions) Yardımcısı sağlandığından emin olmak için. Cosmos DB Cassandra API erişmek için diğer SDK 'lar kullanıyorsanız lütfen bu özel durumları almaya yeniden denemek için bağlantı ilkesi oluşturun. 

## <a name="next-steps"></a>Sonraki adımlar

- Java uygulaması kullanarak [Cassandra API hesabı, veritabanı ve tablo oluşturmaya](create-cassandra-api-account-java.md) başlama

