---
title: Azure Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri
description: Azure Cosmos DB Cassandra API'sinde Apache Cassandra özellik desteği hakkında bilgi edinin
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: overview
ms.date: 09/24/2018
ms.openlocfilehash: 0a2ace3f73379cff0b9289a8cebb10cb7930348d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240243"
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
  * min, max, avg, saymak
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
  


## <a name="cassandra-api-limits"></a>Cassandra API limitleri

Azure Cosmos DB Cassandra API'sinin bir tabloda depolanan verilerin boyutuna dair herhangi bir sınırlaması yoktur. Yüzlerce terabayt veya Petabaytlarca verinin depolanabilmesinin yanı sıra bölüm anahtarı sınırları da kabul edilir. Benzer şekilde, her varlık veya satır eşdeğeri sütun sayısı üzerinde herhangi bir sınırlama yoktur. Ancak, varlığın toplam boyutu 2 MB'ı geçmemelidir. Bölüm başına veri anahtarı, diğer tüm API'lerde olduğu gibi 20 GB'ı geçemez.

## <a name="tools"></a>Araçlar 

Azure Cosmos DB Cassandra API'si bir yönetilen hizmet platformudur. Kümeyi yönetmek için Atık Toplayıcı, Java Sanal Makinesi (JVM) ve düğüm aracı gibi yönetim ek yükü veya yardımcı programları gerekmez. İkili CQLv4 uyumluluğunu kullanan cqlsh gibi araçları destekler. 

* Azure portalının veri gezgini, ölçümleri, günlük tanılama, PowerShell ve CLI hesabı yönetmek için desteklenen diğer mekanizmalardır.

## <a name="cql-shell"></a>CQL Kabuğu  

CQLSH komut satırı yardımcı programı Apache Cassandra 3.1.1 ile birlikte gelir ve bazı ortam değişkenlerini ayarlayarak kutunun dışında çalışır.

**Windows:**

Windows kullanıyorsanız, [Linux için Windows dosya sistemini](https://docs.microsoft.com/windows/wsl/install-win10#install-the-windows-subsystem-for-linux)etkinleştirmenizi öneririz. Daha sonra aşağıdaki linux komutlarını takip edebilirsiniz.

**Unix/Linux/Mac:**

```bash
# Install default-jre and default-jdk
sudo apt install default-jre
sudo apt-get update
sudo apt install default-jdk

# Import the Baltimore CyberTrust root certificate:
curl https://cacert.omniroot.com/bc2025.crt > bc2025.crt
keytool -importcert -alias bc2025ca -file bc2025.crt

# Install the Cassandra libraries in order to get CQLSH:
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://www.apache.org/dist/cassandra/KEYS | sudo apt-key add -
sudo apt-get update
sudo apt-get install cassandra

# Export the SSL variables:
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false

# Connect to Azure Cosmos DB API for Cassandra:
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl

```

## <a name="cql-commands"></a>CQL komutları

Azure Cosmos DB, Cassandra API'si hesaplarında aşağıdaki veritabanı komutlarını destekler.

* CREATE KEYSPACE (Bu komutun çoğaltma ayarları yoksayılır)
* CREATE TABLE 
* CREATE INDEX (dizin adı belirtmeden ve henüz desteklenmeyen tam dondurulmuş dizinler)
* FILTRELEME IZIN VER
* ALTER TABLE 
* USE 
* INSERT 
* SELECT 
* UPDATE 
* BATCH- Yalnızca günlüğe kaydedilmemiş komutlar desteklenir 
* DELETE

CQL v4 uyumlu SDK aracılığıyla gerçekleştirilen tüm CRUD işlemleri, tüketilen hata ve istek birimleri hakkında ek bilgi verecektir. DELETE ve UPDATE komutları, sağlanan iş bölümünün en verimli şekilde kullanılmasını sağlamak için kaynak yönetimi göz önünde bulundurularak ele alınmalıdır.

* Not: gc_grace_seconds değeri belirtilmişse sıfır olmalıdır.

```csharp
var tableInsertStatement = table.Insert(sampleEntity); 
var insertResult = await tableInsertStatement.ExecuteAsync(); 
 
foreach (string key in insertResult.Info.IncomingPayload) 
        { 
            byte[] valueInBytes = customPayload[key]; 
            double value = Encoding.UTF8.GetString(valueInBytes); 
            Console.WriteLine($"CustomPayload:  {key}: {value}"); 
        } 
```

## <a name="consistency-mapping"></a>Tutarlılık eşleme 

Azure Cosmos DB Cassandra API'si okuma işlemleri için tutarlılık sunar.  Tutarlılık eşleme [burada](consistency-levels-across-apis.md#cassandra-mapping)ayrıntılı .

## <a name="permission-and-role-management"></a>İzin ve rol yönetimi

Azure Cosmos DB, [Azure portalı](https://portal.azure.com)üzerinden elde edilebilen anahtarları sağlama, döndürme, ölçümleri görüntüleme ve okuma-yazma ve salt okunur parolaları/anahtarları için rol tabanlı erişim denetimini (RBAC) destekler. Azure Cosmos DB, CRUD etkinlikleri için rolleri desteklemez.

## <a name="keyspace-and-table-options"></a>Anahtar Alanı ve Tablo seçenekleri

"Anahtar Alanı Oluştur" komutundaki bölge adı, sınıf, replication_factor ve veri merkezi seçenekleri şu anda yoksayılır. Sistem, bölgeleri eklemek için azure cosmos DB'nin [küresel dağıtım](global-dist-under-the-hood.md) çoğaltma yöntemini kullanır. Verilerin bölgeler arası varlığına ihtiyacınız varsa, daha fazla bilgi edinmek için powershell, CLI veya portal ile hesap düzeyinde etkinleştirebilirsiniz, bölgeler makalesinin [nasıl ekleyeceğinizi](how-to-manage-database-account.md#addremove-regions-from-your-database-account) görün. Azure Cosmos DB her yazmanın dayanıklı olmasını sağladığından Durable_writes devre dışı tutulamaz. Azure Cosmos DB, her bölgede verileri dört yinelemeden oluşan yineleme kümesi nde çoğaltır ve bu çoğaltma kümesi [yapılandırması](global-dist-under-the-hood.md) değiştirilemez.
 
Tablo oluşturulurken, sıfıra ayarlanılması gereken gc_grace_seconds hariç tüm seçenekler yoksayılır.
Keyspace ve tablo 400 RU / s minimum değeri ile "cosmosdb_provisioned_throughput" adlı ekstra bir seçenek var. Keyspace iş girişi, birden çok tablo arasında iş paylaşımına olanak tanır ve tüm tablolar sağlanan iş bürünme den yararlanmıyorken senaryolar için yararlıdır. Alter Table komutu, bölgeler arasında sağlanan iş akışının değiştirilmesine olanak tanır. 

```
CREATE  KEYSPACE  sampleks WITH REPLICATION = {  'class' : 'SimpleStrategy'}   AND cosmosdb_provisioned_throughput=2000;  

CREATE TABLE sampleks.t1(user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=2000; 

ALTER TABLE gks1.t1 WITH cosmosdb_provisioned_throughput=10000 ;

```


## <a name="usage-of-cassandra-retry-connection-policy"></a>Cassandra bağlantı yeniden deneme ilkesi kullanımı

Azure Cosmos DB, kaynak yönetimli bir sistemdir. Bu, işlemler tarafından tüketilen istek birimlerine bağlı olarak belirli bir saniyede belirli sayıda işlem yapabileceğiniz anlamına gelir. Bir uygulama belirli bir saniyede bu sınırı aşarsa, istekler oran sınırlıdır ve özel durumlar atılır. Azure Cosmos DB'deki Cassandra API, bu özel durumları Cassandra yerel protokolündeki aşırı yüklenen hatalara çevirir. Uygulamanızın, durum oranı sınırlaması durumunda istekleri engellenebilmesini ve yeniden deneyebilmesini sağlamak [için, kıvılcım](https://mvnrepository.com/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper) ve [Java](https://github.com/Azure/azure-cosmos-cassandra-extensions) uzantıları sağlanır. Azure Cosmos DB'de Cassandra API'ye erişmek için başka SDK'lar kullanıyorsanız, bu özel durumları yeniden denemek için bir bağlantı ilkesi oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

- Java uygulaması kullanarak [Cassandra API hesabı, veritabanı ve tablo oluşturmaya](create-cassandra-api-account-java.md) başlama

