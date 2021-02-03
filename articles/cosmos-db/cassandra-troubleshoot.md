---
title: Azure Cosmos DB Cassandra API genel hatalarda sorun giderme
description: Bu belge, Azure Cosmos DB ile karşılaşılan yaygın sorunları gidermeye yönelik yolları ele almaktadır Cassandra API
author: TheovanKraay
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: troubleshooting
ms.date: 12/01/2020
ms.author: thvankra
ms.openlocfilehash: 6d9a74729768a326379b5efddb864a4fee02fa59
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99493234"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API genel sorunları giderme
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Azure Cosmos DB Cassandra API, popüler açık kaynaklı Apache Cassandra veritabanı için [kablo protokol desteği](cassandra-support.md) sağlayan ve [Azure Cosmos DB](./introduction.md)tarafından desteklenen bir uyumluluk katmanıdır. Tam olarak yönetilen bir bulutta yerel hizmet olarak, Azure Cosmos DB Cassandra API [kullanılabilirlik, aktarım hızı ve tutarlılık hakkında garanti](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_3/) sağlar. Bu garanti, Apache Cassandra 'ın eski uygulamalarında mümkün değildir. Cassandra API ayrıca sıfır bakım platformu işlemlerini ve sıfır kapalı kalma süresi düzeltme ekini kolaylaştırır. Bu nedenle, arka uç işlemlerinin birçoğu Apache Cassandra 'dan farklı olduğundan, yaygın hatalardan kaçınmak için belirli ayarları ve yaklaşımları öneririz. 

Bu makalede, Azure Cosmos DB Cassandra API kullanan uygulamalar için sık karşılaşılan hatalar ve çözümler açıklanmaktadır. Hata bilgileriniz aşağıda listelenmiyorsa ve [Cassandra API desteklenen bir işlemi](cassandra-support.md)yürütürken bir hata yaşıyorsanız, *Yerel Apache Cassandra kullanırken hatanın mevcut olmadığı* durumlarda [bir Azure destek isteği oluşturun](../azure-portal/supportability/how-to-create-azure-support-request.md).

## <a name="nonodeavailableexception"></a>NoNodeAvailableException
Bu, çok sayıda olası neden ve iç özel durum içeren üst düzey bir sarmalayıcı özel durumdur ve bunların birçoğu istemci ile ilgili olabilir. 
### <a name="solution"></a>Çözüm
Bazı popüler nedenler ve çözümler aşağıdaki gibidir: 
- Azure LoadBalancers için boşta kalma zaman aşımı: Bu da olarak bildirimde bulunabilir `ClosedConnectionException` . Bunu çözmek için, etkin durumda tut ayarını sürücüde ( [aşağıya](#enable-keep-alive-for-java-driver)bakın) ayarlayın ve işletim sisteminde etkin tutma ayarlarını artırın veya [Azure Load Balancer boşta kalma zaman aşımını ayarlayın](../load-balancer/load-balancer-tcp-idle-timeout.md?tabs=tcp-reset-idle-portal). 
- **İstemci uygulama kaynak tükenmesi:** istemci makinelerin isteği tamamlayacak yeterli kaynağı olduğundan emin olun. 

## <a name="cannot-connect-to-host"></a>Konağa bağlanılamıyor
Şu hatayı görebilirsiniz: `Cannot connect to any host, scheduling retry in 600000 milliseconds` . 

### <a name="solution"></a>Çözüm
Bu, istemci tarafında SNAT tükenmesi olabilir. Bu sorunu denemek için lütfen [giden bağlantılar Için SNAT](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) konumundaki adımları izleyin. Bu Ayrıca, Azure Yük dengeleyicinin varsayılan olarak 4 dakikalık boş zaman aşımı olduğu bir boşta kalma zaman aşımı sorunu olabilir. Bkz. [yük dengeleyici boşta zaman aşımı](../load-balancer/load-balancer-tcp-idle-timeout.md?tabs=tcp-reset-idle-portal). Sürücü ayarlarından ( [aşağıya](#enable-keep-alive-for-java-driver)bakın) TCP-canlı tutmayı etkinleştirin ve `keepAlive` işletim sistemindeki aralığı 4 dakikadan daha kısa bir süre ayarlayın.

 

## <a name="overloadedexception-java"></a>OverloadedException (Java)
Tüketilen istek birimlerinin toplam sayısı, anahtar alanı veya tablo üzerinde sağlanan istek birimlerinden daha fazla. Bu nedenle istekler kısıtlanıyor.
### <a name="solution"></a>Çözüm
Azure Portal bir anahtar alanı veya tabloya atanan üretilen işi ölçeklendirmeyi düşünün (Cassandra API ölçeklendirme işlemleri için [buraya](manage-scale-cassandra.md) bakın) veya bir yeniden deneme ilkesi uygulayabilirsiniz. Java için bkz. [v3. x sürücüsü](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample) ve [v4. x sürücüsü](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample-v4)için yeniden deneme örnekleri. Ayrıca bkz. [Java Için Azure Cosmos Cassandra uzantıları](https://github.com/Azure/azure-cosmos-cassandra-extensions).

### <a name="overloadedexception-even-with-sufficient-throughput"></a>Yeterli aktarım hızı ile bile OverloadedException 
İstek hacmi ve/veya tüketilen istek birimi maliyeti için yeterli işleme sağlanmakta olmasına rağmen sistem azaltma istekleri gibi görünüyor. Beklenmeyen hız sınırlandırmasının iki olası nedeni vardır:
- **Şema düzeyi işlemleri:** Cassandra API şema düzeyindeki işlemler (CREATE TABLE, ALTER TABLE, DROP TABLE) için sistem üretilen iş bütçesini uygular. Bu bütçe, bir üretim sistemindeki şema işlemleri için yeterince olmalıdır. Ancak, çok sayıda şema düzeyi işlem yaptıysanız, bu sınırı aşmanız mümkündür. Bu bütçe kullanıcı denetimli olmadığından, çalıştırılan şema işlemlerinin sayısını azaltmayı göz önünde bulundurmanız gerekir. Bu eylemi gerçekleştirmek sorunu çözmezse veya iş yükünüz için uygun değilse, [bir Azure destek isteği oluşturun](../azure-portal/supportability/how-to-create-azure-support-request.md).
- **Veri eğriltme:** Cassandra API aktarım hızı sağlandığında fiziksel bölümler arasında eşit olarak bölünür ve her fiziksel bölümün üst sınırı vardır. Belirli bir bölümden eklenen veya sorgulanan yüksek miktarda veriniz varsa, bu tablo için büyük miktarda genel işleme (istek birimleri) sağlanmamasına karşın hız sınırlı olabilir. Veri modelinizi gözden geçirin ve etkin bölümlere neden olabilecek çok fazla eğme olmadığından emin olun. 

## <a name="intermittent-connectivity-errors-java"></a>Aralıklı bağlantı hataları (Java) 
Bağlantı beklenmedik bir şekilde bırakılır veya zaman aşımına uğrar.

### <a name="solution"></a>Çözüm 
Java için Apache Cassandra sürücüleri iki yerel yeniden bağlantı ilkesi sağlar: `ExponentialReconnectionPolicy` ve `ConstantReconnectionPolicy` . Varsayılan değer: `ExponentialReconnectionPolicy`. Ancak, Azure Cosmos DB Cassandra API için `ConstantReconnectionPolicy` 2 saniyelik bir gecikmeyle önerilir. Java v4. x sürücüsü için [sürücü belgelerine](https://docs.datastax.com/en/developer/java-driver/4.9/manual/core/reconnection/)  ve [burada](https://docs.datastax.com/en/developer/java-driver/3.7/manual/reconnection/) Java 3. x Kılavuzu için aşağıdaki [Java sürücü için yeniden bağlantı ilkesini yapılandırma](#configuring-reconnectionpolicy-for-java-driver) konusuna bakın.

## <a name="error-with-load-balancing-policy"></a>Yük Dengeleme ilkesiyle ilgili hata

Java DataStax sürücüsünün v3. x bölümünde, aşağıdakine benzer kodla bir yük dengeleme ilkesi uyguladıysanız:

```java
cluster = Cluster.builder()
        .addContactPoint(cassandraHost)
        .withPort(cassandraPort)
        .withCredentials(cassandraUsername, cassandraPassword)
        .withPoolingOptions(new PoolingOptions() .setConnectionsPerHost(HostDistance.LOCAL, 1, 2)
                .setMaxRequestsPerConnection(HostDistance.LOCAL, 32000).setMaxQueueSize(Integer.MAX_VALUE))
        .withSSL(sslOptions)
        .withLoadBalancingPolicy(DCAwareRoundRobinPolicy.builder().withLocalDc("West US").build())
        .withQueryOptions(new QueryOptions().setConsistencyLevel(ConsistencyLevel.LOCAL_QUORUM))
        .withSocketOptions(getSocketOptions())
        .build();
```

İçin değeri, `withLocalDc()` iletişim noktası veri merkezi ile eşleşmiyorsa, zaman aralıklı bir hatayla karşılaşabilirsiniz: `com.datastax.driver.core.exceptions.NoHostAvailableException: All host(s) tried for query failed (no host was tried)` . 

### <a name="solution"></a>Çözüm 
[CosmosLoadBalancingPolicy](https://github.com/Azure/azure-cosmos-cassandra-extensions/blob/master/package/src/main/java/com/microsoft/azure/cosmos/cassandra/CosmosLoadBalancingPolicy.java) uygulayın (iş yapmak için DataStax ikincil sürümünü yükseltmeniz gerekebilir):

```java
LoadBalancingPolicy loadBalancingPolicy = new CosmosLoadBalancingPolicy.Builder().withWriteDC("West US").withReadDC("West US").build();
```

## <a name="count-fails-on-large-table"></a>Büyük tabloda başarısız olan sayı
`select count(*) from table`Çok sayıda satır çalıştırırken veya benzer olduğunda, sunucu zaman aşımına uğrar.

### <a name="solution"></a>Çözüm 
Yerel bir CSQLSH istemcisi kullanıyorsanız, `--connect-timeout` veya ayarlarını değiştirmeyi deneyebilirsiniz `--request-timeout` (daha fazla ayrıntıya bakın). [](https://cassandra.apache.org/doc/latest/tools/cqlsh.html) Bu yeterli değilse ve sayım hala zaman aşımına uğrarsa, Azure portal Azure Cosmos DB arka uç telemetrisinden bir dizi kayıt alabilir, ölçüm ' i seçip, `document count` ardından veritabanı veya koleksiyon için bir filtre ekleyebilirsiniz (Azure Cosmos DB). Daha sonra, zaman içinde kayıt sayısının sayımını yapmak istediğiniz zaman içinde ortaya çıkan grafiğin üzerine gelin.

:::image type="content" source="./media/cassandra-troubleshoot/metrics.png" alt-text="Ölçüm görünümü":::


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