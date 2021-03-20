---
title: Kafka Connect kullanarak Apache Kafka ve Azure Cosmos DB Cassandra API tümleştirin
description: DataStax Apache Kafka bağlayıcısını kullanarak Kafka 'dan Azure Cosmos DB Cassandra API verileri alma hakkında bilgi edinin
author: abhirockzz
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 12/14/2020
ms.author: abhishgu
ms.reviewer: abhishgu
ms.openlocfilehash: 25972ba2bb30c39838c4822a42af292e8d8b1dba
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97803638"
---
# <a name="ingest-data-from-apache-kafka-into-azure-cosmos-db-cassandra-api-using-kafka-connect"></a>Kafka Connect kullanarak Apache Kafka Azure Cosmos DB Cassandra API verileri alma
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Mevcut Cassandra uygulamaları, [CQLv4 sürücü uyumluluğuna](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver)göre [Azure Cosmos DB Cassandra API](cassandra-introduction.md) kolayca çalışabilir. Bu özellikten yararlanarak [Apache Kafka](https://kafka.apache.org/) gibi akış platformlarıyla tümleştirin ve verileri Azure Cosmos DB ' a getirebilirsiniz.

Apache Kafka veriler (konular) yalnızca diğer uygulamalar tarafından tüketilmekte veya diğer sistemlere verildiğinde yararlıdır. [Tercih ettiğiniz dil ve istemci SDK 'sını kullanarak](https://cwiki.apache.org/confluence/display/KAFKA/Clients) [Kafka Producer/Consumer](https://kafka.apache.org/documentation/#api) API 'lerini kullanarak bir çözüm oluşturmak mümkündür. Kafka Connect alternatif bir çözüm sağlar. Apache Kafka ve diğer sistemler arasında ölçeklenebilir ve güvenilir bir şekilde veri akışı için bir platformdur. Kafka Connect, Cassandra 'yi içeren raf bağlayıcılarını desteklediğinden, Kafka Azure Cosmos DB Cassandra API bütünleştirmek için özel kod yazmanıza gerek yoktur. 

Bu makalede, bir Kafka konusundan bir veya daha fazla Cassandra tablosunun satırlarına kayıtları almak için Kafka Connect Framework üzerinde çalışır olan açık kaynaklı [DataStax Apache Kafka bağlayıcısını](https://docs.datastax.com/en/kafka/doc/kafka/kafkaIntro.html)kullanacağız. Örnek, Docker Compose kullanarak yeniden kullanılabilir bir kurulum sağlar. Bu, tüm gerekli bileşenleri tek bir komutla önyüklemenizi sağladığından oldukça kullanışlıdır. Bu bileşenler Kafka, Zookeeper, Kafka Connect worker ve örnek veri Oluşturucu uygulamasını içerir.

Bileşenler ve hizmet tanımlarının bir dökümü aşağıda verilmiştir `docker-compose` . [GitHub](https://github.com/Azure-Samples/cosmosdb-cassandra-kafka/blob/main/docker-compose.yaml)deposunda tam dosyaya başvurabilirsiniz.

- Kafka ve Zookeeper, [debezium](https://hub.docker.com/r/debezium/kafka/) görüntülerini kullanır.
- Docker kapsayıcısı olarak çalıştırmak için, DataStax Apache Kafka Bağlayıcısı var olan bir Docker görüntüsünün en üstünde ( [debezium/Connect-Base](https://github.com/debezium/docker-images/tree/master/connect-base/1.2)) kullanıma hazır. Bu görüntü, Kafka ve Kafka Connect kitaplıklarının bir yüklemesini içerir ve bu sayede özel bağlayıcılar eklemeye uygun hale gelir. [Dockerfile dosyasına](https://github.com/Azure-Samples/cosmosdb-cassandra-kafka/blob/main/connector/Dockerfile)başvurabilirsiniz.
- `data-generator`Hizmet, Kafka konusunda rastgele oluşturulan (JSON) verileri `weather-data` . Koda ve `Dockerfile` [GitHub](https://github.com/Azure-Samples/cosmosdb-cassandra-kafka/blob/main/data-generator/) deposunda başvurabilirsiniz

## <a name="prerequisites"></a>Önkoşullar

* [Azure Cosmos DB Cassandra API hesabı sağlama](create-cassandra-dotnet.md#create-a-database-account)

* [Doğrulama için csqlsh veya hosted Shell kullanma](cassandra-support.md#hosted-cql-shell-preview)

* [Docker](https://docs.docker.com/get-docker/) ve [Docker Compose](https://docs.docker.com/compose/install) 'yi yükleyip

## <a name="create-keyspace-tables-and-start-the-integration-pipeline"></a>Anahtar alanı, tablolar oluşturma ve tümleştirme ardışık düzenini başlatma

Azure portal kullanarak, Cassandra keyspace ve demo uygulaması için gereken tabloları oluşturun.

> [!NOTE]
> Aynı anahtar alanını ve tablo adlarını aşağıda gösterildiği gibi kullanın

```sql
CREATE KEYSPACE weather WITH REPLICATION = {'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1};

CREATE TABLE weather.data_by_state (station_id text, temp int, state text, ts timestamp, PRIMARY KEY (state, ts)) WITH CLUSTERING ORDER BY (ts DESC) AND cosmosdb_cell_level_timestamp=true AND cosmosdb_cell_level_timestamp_tombstones=true AND cosmosdb_cell_level_timetolive=true;

CREATE TABLE weather.data_by_station (station_id text, temp int, state text, ts timestamp, PRIMARY KEY (station_id, ts)) WITH CLUSTERING ORDER BY (ts DESC) AND cosmosdb_cell_level_timestamp=true AND cosmosdb_cell_level_timestamp_tombstones=true AND cosmosdb_cell_level_timetolive=true;
```

GitHub deposunu kopyalayın:

```bash
git clone https://github.com/Azure-Samples/cosmosdb-cassandra-kafka
cd cosmosdb-cassandra-kafka
```

Tüm hizmetleri Başlat:

```shell
docker-compose --project-name kafka-cosmos-cassandra up --build
```

> [!NOTE]
> Kapsayıcıları indirmek ve başlatmak biraz zaman alabilir: Bu yalnızca bir kerelik bir işlemdir.

Tüm kapsayıcıların başlatılıp başlatılmayacağını doğrulamak için:

```shell
docker-compose -p kafka-cosmos-cassandra ps
```

Veri Oluşturucu uygulaması, `weather-data` Kafka ' deki konuya veri pompalama verileri başlatacak. Onaylamak için de hızlı bir denetim yapabilirsiniz. Kafka Connect Worker çalıştıran Docker kapsayıcısına göz atın:


```bash
docker exec -it kafka-cosmos-cassandra_cassandra-connector_1 bash
```

Kapsayıcı kabuğunu bıraktığınızda, her zamanki Kafka konsolu tüketici işlemini başlatmanız yeterlidir ve hava durumu verilerini (JSON biçiminde) içinde akan olarak görmeniz gerekir.

```bash
cd ../bin
./kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic weather-data
```

## <a name="cassandra-sink-connector-setup"></a>Cassandra havuzu bağlayıcı kurulumu

Aşağıdaki JSON içeriğini bir dosyaya kopyalayın (adı verebilirsiniz `cassandra-sink-config.json` ). Bu konuyu, kuruluma göre güncelleştirmeniz gerekir ve bu bölümün geri kalanı bu konunun çevresinde rehberlik sağlar.

```json
{
    "name": "kafka-cosmosdb-sink",
    "config": {
        "connector.class": "com.datastax.oss.kafka.sink.CassandraSinkConnector",
        "tasks.max": "1",
        "topics": "weather-data",
        "contactPoints": "<cosmos db account name>.cassandra.cosmos.azure.com",
        "port": 10350,
        "loadBalancing.localDc": "<cosmos db region e.g. Southeast Asia>",
        "auth.username": "<enter username for cosmosdb account>",
        "auth.password": "<enter password for cosmosdb account>",
        "ssl.hostnameValidation": true,
        "ssl.provider": "JDK",
        "ssl.keystore.path": "/etc/alternatives/jre/lib/security/cacerts/",
        "ssl.keystore.password": "changeit",
        "datastax-java-driver.advanced.connection.init-query-timeout": 5000,
        "maxConcurrentRequests": 500,
        "maxNumberOfRecordsInBatch": 32,
        "queryExecutionTimeout": 30,
        "connectionPoolLocalSize": 4,
        "topic.weather-data.weather.data_by_state.mapping": "station_id=value.stationid, temp=value.temp, state=value.state, ts=value.created",
        "topic.weather-data.weather.data_by_station.mapping": "station_id=value.stationid, temp=value.temp, state=value.state, ts=value.created",
        "key.converter": "org.apache.kafka.connect.storage.StringConverter",
        "value.converter": "org.apache.kafka.connect.json.JsonConverter",
        "value.converter.schemas.enable": false,
        "offset.flush.interval.ms": 10000
    }
}
```

Özniteliklerin özeti aşağıda verilmiştir:

**Temel bağlantı**

- `contactPoints`: Cosmos DB Cassandra için iletişim noktasını girin
- `loadBalancing.localDc`: Cosmos DB hesabının bölgesini girin, örneğin Güneydoğu Asya
- `auth.username`: Kullanıcı adını girin
- `auth.password`: parolayı girin
- `port`: bağlantı noktası değerini girin (Bu, `10350` değildir `9042` . olduğu gibi bırak

**SSL yapılandırması**

Azure Cosmos DB [SSL üzerinden güvenli bağlantıyı](database-security.md) zorlar ve Kafka Connect Connector da SSL 'yi destekler.

- `ssl.keystore.path`: kapsayıcıda JDK anahtar deposu yolu- `/etc/alternatives/jre/lib/security/cacerts/`
- `ssl.keystore.password`: JDK anahtar deposu (varsayılan) parolası
- `ssl.hostnameValidation`: Kendi düğüm ana bilgisayar adı doğrulamasını kullanıyoruz
- `ssl.provider`: `JDK` SSL sağlayıcısı olarak kullanılır

**Genel parametreler**

- `key.converter`: Dize dönüştürücüsünü kullanıyoruz `org.apache.kafka.connect.storage.StringConverter`
- `value.converter`: Kafka konularındaki veriler JSON olduğundan, `org.apache.kafka.connect.json.JsonConverter`
- `value.converter.schemas.enable`: JSON yükümüzde kendisiyle ilişkilendirilmiş bir şema olmadığından (demo uygulamasının amaçları doğrultusunda), bu özniteliği olarak ayarlayarak Kafka Connect 'e bir şema bakmamasını söylemek gerekir `false` . Aksi takdirde hatalara neden olur.

### <a name="install-the-connector"></a>Bağlayıcıyı yükler

Kafka Connect REST uç noktasını kullanarak bağlayıcıyı yükler:

```shell
curl -X POST -H "Content-Type: application/json" --data @cassandra-sink-config.json http://localhost:8083/connectors
```

Durumu denetlemek için:

```
curl http://localhost:8080/connectors/kafka-cosmosdb-sink/status
```

Hepsi de uygunsa, bağlayıcı Magic 'in sihirli bir şekilde başlamamalıdır. Azure Cosmos DB kimlik doğrulaması yapılmalıdır ve verileri Kafka konusunun ( `weather-data` ) Cassandra tablolarına geri almaya `weather.data_by_state` başlar ve `weather.data_by_station`

Artık tablolardaki verileri sorgulayabilirsiniz. Azure portal, Azure Cosmos DB hesabınız için barındırılan CQL kabuğunu getirin.

:::image type="content" source="./media/cassandra-kafka-connect/cqlsh.png" alt-text="CSQLSH 'i aç":::

## <a name="query-data-from-azure-cosmos-db"></a>Azure Cosmos DB verileri sorgulama

`data_by_state`Ve tablolarını kontrol edin `data_by_station` . İşte başlamanıza yönelik bazı örnek sorgular:

```sql
select * from weather.data_by_state where state = 'state-1';
select * from weather.data_by_state where state IN ('state-1', 'state-2');
select * from weather.data_by_state where state = 'state-3' and ts > toTimeStamp('2020-11-26');

select * from weather.data_by_station where station_id = 'station-1';
select * from weather.data_by_station where station_id IN ('station-1', 'station-2');
select * from weather.data_by_station where station_id IN ('station-2', 'station-3') and ts > toTimeStamp('2020-11-26');
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md) 
* [Bölüm anahtarı en iyi uygulamaları](partitioning-overview.md#choose-partitionkey)
* [Azure Cosmos DB kapasite planlayıcısı makalelerini kullanarak ru/s 'Yi tahmin](estimate-ru-with-capacity-planner.md) etme
