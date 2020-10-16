---
title: Azure Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri
description: Azure Cosmos DB Cassandra API'sinde Apache Cassandra özellik desteği hakkında bilgi edinin
author: TheovanKraay
ms.author: thvankra
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: overview
ms.date: 09/14/2020
ms.openlocfilehash: f9d472b94b5490d00eac6d160af40c61d547534b
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92107496"
---
# <a name="apache-cassandra-features-supported-by-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri 

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. CQL Ikili protokol v4 [kablo protokolü](https://github.com/apache/cassandra/blob/trunk/doc/native_protocol_v4.spec) ile uyumlu açık kaynaklı Cassandra istemci [sürücüleri](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver)aracılığıyla Azure Cosmos DB Cassandra API ile iletişim kurabilirsiniz. 

Azure Cosmos DB Cassandra API’sini kullanarak Apache Cassandra API’leri avantajlarının yanı sıra Azure Cosmos DB’nin sunduğu kurumsal özelliklerin keyfini çıkarabilirsiniz. [Genel dağıtım](distribute-data-globally.md), [otomatik ölçek genişletme bölümlemesi](cassandra-partitioning.md), kullanılabilirlik ve gecikme süresi garantileri, REST’te şifreleme, yedeklemeler ve çok daha fazlası kurumsal özelliklere dahildir.

## <a name="cassandra-protocol"></a>Cassandra protokolü 

Azure Cosmos DB Cassandra API, Cassandra sorgu dili (CQL) v 3.11 API 'siyle uyumludur (sürüm 2. x ile geriye dönük olarak uyumludur). Desteklenen CQL komutları, araçlar, sınırlamalar ve özel durumlar aşağıda listelenmiştir. Bu protokolleri anlayan bir istemci sürücüsü Azure Cosmos DB Cassandra API'sine bağlanabilir.

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

|Komut  |Desteklenir |
|---------|---------|
| ascii  | Evet |
| bigint  | Evet |
| blob  | Evet |
| boolean  | Evet |
| counter  | Evet |
| tarih  | Evet |
| decimal  | Evet |
| double  | Evet |
| float  | Evet |
| frozen  | Evet |
| inet  | Evet |
| int  | Evet |
| list  | Evet |
| set  | Evet |
| smallint  | Evet |
| metin  | Evet |
| time  | Evet |
| timestamp  | Evet |
| timeuuid  | Evet |
| tinyint  | Evet |
| tuple  | Evet |
| uuid  | Evet |
| varchar  | Evet |
| varint  | Evet |
| tuples | Evet | 
| udts  | Evet |
| map | Evet |

Statik, veri türü bildirimi için desteklenir.

## <a name="cql-functions"></a>CQL işlevleri

Azure Cosmos DB Cassandra API'si aşağıdaki CQL işlevlerini destekler:

|Komut  |Desteklenir |
|---------|---------|
| Simgesinde | Evet |
| ttl | Evet |
| WriteTime | Evet |
| atama | Hayır |

\* Cassandra API, belirteci bir izdüşüm/seçici olarak destekler ve yalnızca bir where yan tümcesinin sol tarafında belirtece (PK) izin verir. Örneğin, `WHERE token(pk) > 1024` desteklenir, ancak `WHERE token(pk) > token(100)` desteklenmez.


Toplama işlevleri:

|Komut  |Desteklenir |
|---------|---------|
| dk | Evet |
| max | Evet |
| cin | Evet |
| count | Evet |

Blob dönüştürme işlevleri:
 
|Komut  |Desteklenir |
|---------|---------|
| typeAsBlob(value)   | Evet |
| blobAsType(value) | Evet |


UUID ve timeuuıd işlevleri:
 
|Komut  |Desteklenir |
|---------|---------|
| dateOf()  | Evet |
| now()  | Evet |
| minTimeuuid()  | Evet |
| unixTimestampOf()  | Evet |
| toDate(timeuuid)  | Evet |
| toTimestamp(timeuuid)  | Evet |
| toUnixTimestamp(timeuuid)  | Evet |
| toDate(timestamp)  | Evet |
| toUnixTimestamp(timestamp)  | Evet |
| toTimestamp(date)  | Evet |
| toUnixTimestamp(date) | Evet |


  
## <a name="cql-commands"></a>CQL komutları

Azure Cosmos DB, Cassandra API'si hesaplarında aşağıdaki veritabanı komutlarını destekler.

|Komut  |Desteklenir |
|---------|---------|
| FILTRELEMEYE IZIN VER | Evet |
| ANAHTAR ALANıNı DEĞIŞTIR | Yok (PaaS hizmeti, çoğaltma dahili olarak yönetilen)|
| GERÇEKLEŞTIRILMIŞ GÖRÜNÜMÜ DEĞIŞTIR | Hayır |
| ROLÜ DEĞIŞTIR | Hayır |
| ALTER TABLE | Evet |
| DEĞIŞTIRME TÜRÜ | Hayır |
| KULLANıCı DEĞIŞTIR | Hayır |
| IŞLEMINI | Evet (yalnızca günlüğe kaydedilen Batch)|
| COMPACT STORAGE | Yok (PaaS hizmeti) |
| TOPLAMA OLUŞTUR | Hayır | 
| ÖZEL DIZIN OLUŞTURMA (SASı) | Hayır |
| CREATE INDEX | Evet ( [Dizin adı belirtmeden](cassandra-secondary-index.md), kümeleme anahtarlarındaki dizinler veya tam dondurulmuş koleksiyon desteklenmez) |
| CREATE FUNCTION | Hayır |
| ANAHTAR alanı oluştur (çoğaltma ayarları yoksayıldı) | Evet |
| GERÇEKLEŞTIRILMIŞ GÖRÜNÜM OLUŞTUR | Hayır |
| CREATE TABLE | Evet |
| TETIKLEYICI OLUŞTUR | Hayır |
| OLUŞTURMA TÜRÜ | Evet |
| ROL OLUŞTUR | Hayır |
| Kullanıcı oluştur (yerel Apache Cassandra 'da kullanım dışı) | Hayır |
| DELETE | Evet |
| SIL (IF KOŞULUNDA hafif işlemler)| Evet |
| DISTINCT | Hayır |
| BıRAKMA TOPLAMı | Hayır |
| DROP FUNCTION | Hayır |
| DROP INDEX | Evet |
| ANAHTAR UZAYıNı BıRAK | Evet |
| GERÇEKLEŞTIRILMIŞ GÖRÜNÜMÜ BıRAK | Hayır |
| ROLÜ BıRAK | Hayır |
| DROP TABLE | Evet |
| TETIKLEYICIYI BıRAK | Hayır | 
| BıRAKMA TÜRÜ | Evet |
| KULLANıCıYı bırak (yerel Apache Cassandra 'da kullanım dışı) | Hayır |
| GRANT | Hayır |
| INSERT | Evet |
| Ekle (If koşulu ile hafif işlemler)| Evet |
| IZINLERI LISTELE | Hayır |
| ROLLERI LISTELE | Hayır |
| KULLANıCıLARı LISTELEME (yerel Apache Cassandra 'da kullanım dışı) | Hayır |
| REVOKE | Hayır |
| SELECT | Evet |
| Seç (IF KOŞULUNDA hafif işlemler)| Hayır |
| UPDATE | Evet |
| GÜNCELLEŞTIR (If koşulu ile hafif işlemler)| Hayır |
| KESILEMEDI | Hayır |
| USE | Evet |

## <a name="json-support"></a>JSON desteği
|Komut  |Desteklenir |
|---------|---------|
| JSON SEÇIN | Evet |
| JSON EKLE | Evet |
| fromJson () | Hayır |
| toJson () | Hayır |


## <a name="cassandra-api-limits"></a>Cassandra API limitleri

Azure Cosmos DB Cassandra API'sinin bir tabloda depolanan verilerin boyutuna dair herhangi bir sınırlaması yoktur. Yüzlerce terabayt veya Petabaytlarca verinin depolanabilmesinin yanı sıra bölüm anahtarı sınırları da kabul edilir. Benzer şekilde, her varlık veya satır eşdeğeri sütun sayısıyla ilgili herhangi bir sınıra sahip değildir. Ancak, varlığın toplam boyutu 2 MB 'ı aşmamalıdır. Bölüm anahtarı başına veri, diğer tüm API 'lerde olduğu gibi 20 GB 'yi aşamaz.

## <a name="tools"></a>Araçlar 

Azure Cosmos DB Cassandra API'si bir yönetilen hizmet platformudur. Kümeyi yönetmek için Atık Toplayıcı, Java Sanal Makinesi (JVM) ve düğüm aracı gibi yönetim ek yükü veya yardımcı programları gerekmez. İkili CQLv4 uyumluluğunu kullanan cqlsh gibi araçları destekler. 

* Azure portal veri Gezgini, ölçümler, günlük tanılama, PowerShell ve CLı, hesabı yönetmek için desteklenen diğer mekanizmalarda bulunur.

## <a name="hosted-cql-shell-preview"></a>Barındırılan CQL kabuğu (Önizleme)

Barındırılan yerel Cassandra kabuğunu (CDıSH v 5.0.1) doğrudan [Azure Portal](data-explorer.md) veya [Azure cosmos Gezgini](https://cosmos.azure.com/)'ndeki Veri Gezgini açabilirsiniz. CQL kabuğu 'nu etkinleştirmeden önce hesabınızdaki [Not defterleri özelliğini etkinleştirmeniz](enable-notebooks.md) gerekir (henüz etkinleştirilmemişse, üzerine tıklandığınızda sorulur `Open Cassandra Shell` ). Desteklenen Azure bölgeleri için [Azure Cosmos DB hesapları için not defterlerini etkinleştirme](enable-notebooks.md) bölümünde vurgulanan nota bakın.

:::image type="content" source="./media/cassandra-support/cqlsh.png" alt-text="CSQLSH 'i aç&quot;:::

Ayrıca, bir yerel makinede yüklü olan CSQLSH kullanarak Azure Cosmos DB Cassandra API bağlayabilirsiniz. Bu, Apache Cassandra 3.1.1 ile birlikte gelir ve ortam değişkenlerini ayarlayarak kutudan çıkar. Aşağıdaki bölümler, Windows veya Linux 'ta, CSQLSH kullanarak Azure Cosmos DB ' de Cassandra API yüklemek, yapılandırmak ve kurmak için yönergeler içerir.

> [!NOTE]
> Azure Cosmos DB Cassandra API bağlantıları, CÖNSH 'in DataStax Enterprise (DSE) sürümleriyle çalışmaz. Lütfen Cassandra API bağlanırken CSQLSH 'in açık kaynaklı Apache Cassandra sürümlerini kullandığınızdan emin olun. 

**Pencerelerin**

Windows kullanıyorsanız, [Linux Için Windows FileSystem](https://docs.microsoft.com/windows/wsl/install-win10#install-the-windows-subsystem-for-linux)'ı etkinleştirmenizi öneririz. Daha sonra aşağıdaki Linux komutlarını izleyebilirsiniz.

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
echo &quot;deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -
sudo apt-get update
sudo apt-get install cassandra

# Export the SSL variables:
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false

# Connect to Azure Cosmos DB API for Cassandra:
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl

```

CQL v4 uyumlu SDK aracılığıyla yürütülen tüm CRUD işlemleri, hata ve tüketilen istek birimleri hakkında ek bilgiler döndürür. Sağlanan aktarım hızını en verimli şekilde kullanmasını sağlamak için, DELETE ve UPDATE komutları dikkate alınması gereken kaynak yönetimi ile birlikte işlenmelidir.

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

Azure Cosmos DB Cassandra API'si okuma işlemleri için tutarlılık sunar.  Tutarlılık eşlemesi [burada](consistency-levels-across-apis.md#cassandra-mapping)ayrıntılı olarak verilmiştir.

## <a name="permission-and-role-management"></a>İzin ve rol yönetimi

Azure Cosmos DB, sağlama, anahtar döndürme, ölçümleri görüntüleme ve [Azure Portal](https://portal.azure.com)aracılığıyla elde edilebilir salt okuma ve salt okuma parolaları/anahtarları için rol tabanlı erişim denetimi 'NI (RBAC) destekler. Azure Cosmos DB CRUD etkinlikleri için rolleri desteklemez.

## <a name="keyspace-and-table-options"></a>Keyspace ve tablo seçenekleri

"Keyspace oluşturma" komutunda bölge adı, sınıf, replication_factor ve veri merkezi seçenekleri şu anda yok sayılır. Sistem, bölgeleri eklemek için temel Azure Cosmos DB [genel dağıtım](global-dist-under-the-hood.md) çoğaltma yöntemini kullanır. Bölgeler arası veri varlığına ihtiyacınız varsa, daha fazla bilgi edinmek için PowerShell, CLı veya Portal ile hesap düzeyinde etkinleştirebilir, daha fazla bilgi edinmek için bkz. [bölge ekleme](how-to-manage-database-account.md#addremove-regions-from-your-database-account) makalesi. Azure Cosmos DB her yazmanın dayanıklı olmasını sağladığından Durable_writes devre dışı bırakılamaz. Her bölgede, Azure Cosmos DB, verileri dört yinelemeden oluşan çoğaltma kümesi genelinde çoğaltır ve bu çoğaltma kümesi [yapılandırması](global-dist-under-the-hood.md) değiştirilemez.
 
Tablo oluşturulurken tüm seçenekler yok sayılır, gc_grace_seconds hariç, sıfıra ayarlanmalıdır.
Anahtar alanı ve tablo, en az 400 RU/sn değeri olan "cosmosdb_provisioned_throughput" adlı ek bir seçeneğe sahiptir. Anahtar alanı verimlilik, birden çok tablo arasında üretilen iş aktarımına izin verir ve tüm tablolar sağlanan aktarım hızını kullanmadan senaryolar için yararlıdır. Alter table komutu bölgeler genelinde sağlanan aktarım hızını değiştirmeye izin verir. 

```
CREATE  KEYSPACE  sampleks WITH REPLICATION = {  'class' : 'SimpleStrategy'}   AND cosmosdb_provisioned_throughput=2000;  

CREATE TABLE sampleks.t1(user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=2000; 

ALTER TABLE gks1.t1 WITH cosmosdb_provisioned_throughput=10000 ;

```
## <a name="secondary-index"></a>İkincil dizin
Cassandra API, dondurulmuş koleksiyon türleri, ondalık ve değişken türleri dışındaki tüm veri türlerinde ikincil dizinleri destekler. 

## <a name="usage-of-cassandra-retry-connection-policy"></a>Cassandra bağlantı yeniden deneme ilkesi kullanımı

Azure Cosmos DB, kaynak tarafından yönetilen bir sistemdir. Bu, işlemler tarafından tüketilen istek birimlerine göre belirli bir saniye içinde belirli sayıda işlem yapabileceğiniz anlamına gelir. Bir uygulama belirli bir saniye içindeki sınırı aşarsa, istekler hız sınırlı olur ve özel durumlar oluşturulur. Azure Cosmos DB Cassandra API, bu özel durumları Cassandra Native protokolünde aşırı yüklenmiş hatalara dönüştürür. Uygulamanızın hız sınırlaması durumunda istekleri ele geçirebilir ve yeniden denediğinden emin olmak için [Spark](https://mvnrepository.com/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper) ve [Java](https://github.com/Azure/azure-cosmos-cassandra-extensions) uzantıları sağlanır. Ayrıca, Azure Cosmos DB Cassandra API bağlanırken [sürüm 3](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample) ve [sürüm 4](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample-v4) DataStax sürücüleri için Java kod örneklerine bakın. Azure Cosmos DB Cassandra API erişmek için diğer SDK 'lar kullanıyorsanız, bu özel durumlar üzerinde yeniden denemek için bir bağlantı ilkesi oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

- Java uygulaması kullanarak [Cassandra API hesabı, veritabanı ve tablo oluşturmaya](create-cassandra-api-account-java.md) başlama
