---
title: Azure Cosmos DB Cassandra API genel hatalarda sorun giderme
description: Bu belge, Azure Cosmos DB ile karşılaşılan yaygın sorunları gidermeye yönelik yolları ele almaktadır Cassandra API
author: TheovanKraay
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: troubleshooting
ms.date: 12/01/2020
ms.author: thvankra
ms.openlocfilehash: 75ce307056d24f84e9f72f746eb77b09b89e21b2
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98927495"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API genel sorunları giderme
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Azure Cosmos DB Cassandra API, popüler açık kaynaklı Apache Cassandra veritabanı için [kablo protokol desteği](cassandra-support.md) sağlayan ve [Azure Cosmos DB](./introduction.md)tarafından desteklenen bir uyumluluk katmanıdır. Tam olarak yönetilen bir bulutta yerel hizmet olarak, Azure Cosmos DB Cassandra API [kullanılabilirlik, aktarım hızı ve tutarlılık hakkında garanti](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_3/) sağlar. Bu garanti, Apache Cassandra 'ın eski uygulamalarında mümkün değildir. Cassandra API ayrıca sıfır bakım platformu işlemlerini ve sıfır kapalı kalma süresi düzeltme ekini kolaylaştırır. Bu nedenle, arka uç işlemlerinin birçoğu Apache Cassandra 'dan farklı olduğundan, yaygın hatalardan kaçınmak için belirli ayarları ve yaklaşımları öneririz. 

Bu makalede, Azure Cosmos DB Cassandra API kullanan uygulamalar için sık karşılaşılan hatalar ve çözümler açıklanmaktadır.

## <a name="common-errors-and-solutions"></a>Genel hatalar ve çözümleri

| Hata               |  Açıklama             | Çözüm  |
|---------------------|--------------------------|-----------|
| OverloadedException (Java) | Tüketilen istek birimlerinin toplam sayısı, anahtar alanı veya tablo üzerinde sağlanan istek birimlerinden daha fazla. Bu nedenle istekler kısıtlanıyor. | Azure Portal bir anahtar alanı veya tabloya atanan üretilen işi ölçeklendirmeyi düşünün (Cassandra API ölçeklendirme işlemleri için [buraya](manage-scale-cassandra.md) bakın) veya bir yeniden deneme ilkesi uygulayabilirsiniz. Java için bkz. [v3. x sürücüsü](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample) ve [v4. x sürücüsü](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample-v4)için yeniden deneme örnekleri. Ayrıca bkz. [Java Için Azure Cosmos Cassandra uzantıları](https://github.com/Azure/azure-cosmos-cassandra-extensions) |
| OverloadedException (Java) yeterli aktarım hızına sahip | İstek hacmi ve/veya tüketilen istek birimi maliyeti için yeterli işleme sağlanmakta olmasına rağmen sistem daraltma istekleri gibi görünüyor  | Cassandra API şema düzeyindeki işlemler (CREATE TABLE, ALTER TABLE, DROP TABLE) için sistem üretilen iş bütçesini uygular. Bu bütçe, bir üretim sistemindeki şema işlemleri için yeterince olmalıdır. Ancak, çok sayıda şema düzeyi işlem yaptıysanız, bu sınırı aşmanız mümkündür. Bu bütçe kullanıcı denetimli olmadığından, çalıştırılan şema işlemlerinin sayısını azaltmayı göz önünde bulundurmanız gerekir. Bu eylemi gerçekleştirmek sorunu çözmezse veya iş yükünüz için uygun değilse, [bir Azure destek isteği oluşturun](../azure-portal/supportability/how-to-create-azure-support-request.md).|
| ClosedConnectionException (Java) | Başarılı bağlantıları izleyen bir boşta kalma süresi dolduktan sonra uygulama bağlanamıyor| Bu hata, 4 dakika olan Azure LoadBalancers 'in boşta kalma zaman aşımından kaynaklanıyor olabilir. Etkin tut ayarını sürücüde tut (aşağıya bakın) ve işletim sisteminde etkin tutma ayarlarını artırın veya [Azure Load Balancer boşta kalma zaman aşımını ayarlayın](../load-balancer/load-balancer-tcp-idle-timeout.md?tabs=tcp-reset-idle-portal). |
| Diğer aralıklı bağlantı hataları (Java) | Bağlantı, beklenmedik şekilde düşerek veya zaman aşımına uğrar | Java için Apache Cassandra sürücüleri iki yerel yeniden bağlantı ilkesi sağlar: `ExponentialReconnectionPolicy` ve `ConstantReconnectionPolicy` . Varsayılan değer: `ExponentialReconnectionPolicy`. Ancak, Azure Cosmos DB Cassandra API için `ConstantReconnectionPolicy` 2 saniyelik bir gecikmeyle önerilir. Java v4. x sürücüsü için [sürücü belgelerine](https://docs.datastax.com/en/developer/java-driver/4.9/manual/core/reconnection/)  ve [burada](https://docs.datastax.com/en/developer/java-driver/3.7/manual/reconnection/) Java 3. x Kılavuzu için (aşağıdaki örneklere bakın) bakın.|

Hata bilgileriniz yukarıda listelenmiyorsa ve [Cassandra API desteklenen bir işlemi](cassandra-support.md)yürütürken bir hata yaşıyorsanız, *Yerel Apache Cassandra kullanırken hatanın mevcut olmadığı* durumlarda [bir Azure destek isteği oluşturun](../azure-portal/supportability/how-to-create-azure-support-request.md)

## <a name="configuring-reconnectionpolicy-for-java-driver"></a>Java sürücüsü için ReconnectionPolicy yapılandırma

### <a name="version-3x"></a>Sürüm 3. x

Java sürücüsünün 3. x sürümü için, bir küme nesnesi oluştururken yeniden bağlanma ilkesini yapılandırın:

```java
import com.datastax.driver.core.policies.ConstantReconnectionPolicy;

Cluster.builder()
  .withReconnectionPolicy(new ConstantReconnectionPolicy(2000))
  .build();
```

### <a name="version-4x"></a>Sürüm 4. x

Java sürücüsünün 4. x sürümü için, dosyadaki ayarları geçersiz kılarak yeniden bağlanma ilkesini yapılandırın `reference.conf` :

```xml
datastax-java-driver {
  advanced {
    reconnection-policy{
      # The driver provides two implementations out of the box: ExponentialReconnectionPolicy and
      # ConstantReconnectionPolicy. We recommend ConstantReconnectionPolicy for Cassandra API, with 
      # base-delay of 2 seconds.
      class = ConstantReconnectionPolicy
      base-delay = 2 second
    }
}
```

## <a name="enable-keep-alive-for-java-driver"></a>Java sürücüsü için canlı tutmayı etkinleştir

### <a name="version-3x"></a>Sürüm 3. x

Java sürücüsünün 3. x. x için, bir küme nesnesi oluştururken etkin tut ' u ayarlayın ve [işletim sisteminde](https://knowledgebase.progress.com/articles/Article/configure-OS-TCP-KEEPALIVE-000080089)canlı tutma özelliğinin etkinleştirildiğinden emin olun:

```java
import java.net.SocketOptions;
    
SocketOptions options = new SocketOptions();
options.setKeepAlive(true);
cluster = Cluster.builder().addContactPoints(contactPoints).withPort(port)
  .withCredentials(cassandraUsername, cassandraPassword)
  .withSocketOptions(options)
  .build();
```

### <a name="version-4x"></a>Sürüm 4. x

Java sürücüsünün 4. x sürümü için, ' deki ayarları geçersiz kılarak canlı tut ' u ayarlayın `reference.conf` ve [işletim sisteminde etkin](https://knowledgebase.progress.com/articles/Article/configure-OS-TCP-KEEPALIVE-000080089)tut özelliğinin etkinleştirildiğinden emin olun:

```xml
datastax-java-driver {
  advanced {
    socket{
      keep-alive = true
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure Cosmos DB Cassandra API [desteklenen özellikler](cassandra-support.md) hakkında bilgi edinin.
- [Yerel Apache Cassandra 'dan Azure Cosmos DB Cassandra API geçiş](cassandra-migrate-cosmos-db-databricks.md) yapmayı öğrenin