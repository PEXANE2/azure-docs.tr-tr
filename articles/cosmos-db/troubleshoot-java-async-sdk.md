---
title: Azure Cosmos DB Java Async SDK tanılama ve sorun giderme
description: Azure Cosmos DB sorunlarını tanımlamak, tanılamak ve sorun gidermek için istemci tarafı günlüğü ve diğer üçüncü taraf araçları gibi özellikleri kullanın.
author: moderakh
ms.service: cosmos-db
ms.date: 04/30/2019
ms.author: moderakh
ms.devlang: java
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 572139743c66546622450cef8f8a0fa264d24779
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65519973"
---
# <a name="troubleshoot-issues-when-you-use-the-java-async-sdk-with-azure-cosmos-db-sql-api-accounts"></a>Azure Cosmos DB SQL API hesaplarıyla Java Async SDK’sını kullanırken karşılaşılan sorunları giderme
Bu makalede, Azure Cosmos DB SQL API hesaplarıyla [Java Async SDK'yı](sql-api-sdk-async-java.md) kullandığınızda sık karşılaşılan sorunlar, geçici çözüm adımları ve araçlar ele alılmıştır.
Java Async SDK’sı, Azure Cosmos DB SQL API’sine erişim için istemci tarafı mantıksal temsili sağlar. Bu makalede, sorunla karşılaştığınızda size yardımcı olacak araçlar ve yaklaşımlar açıklanır.

Bu listeyle başlayın:

* Bu makaledeki [Ortak sorunlar ve geçici çözüm ler] bölümüne bir göz atın.
* [GitHub açık kaynak](https://github.com/Azure/azure-cosmosdb-java)kullanılabilir SDK, bak. Etkin olarak izlenen bir [sorun bölümü](https://github.com/Azure/azure-cosmosdb-java/issues) var. Geçici çözümle ilgili benzer bir sorunun dosyalanmış olup olmadığını denetleyin.
* Performans [ipuçlarını](performance-tips-async-java.md)gözden geçirin ve önerilen uygulamaları izleyin.
* Bir çözüm bulamadıysanız, bu makalenin geri kalanını okuyun. Sonra bir [GitHub sorunu](https://github.com/Azure/azure-cosmosdb-java/issues)dosya.

## <a name="common-issues-and-workarounds"></a><a name="common-issues-workarounds"></a>Yaygın sorunlar ve geçici çözümler

### <a name="network-issues-netty-read-timeout-failure-low-throughput-high-latency"></a>Ağ sorunları, Netty okuma zaman sonu hatası, düşük iş sonu, yüksek gecikme süresi

#### <a name="general-suggestions"></a>Genel öneriler
* Uygulamanın Azure Cosmos DB hesabınızla aynı bölgede çalıştığından emin olun. 
* Uygulamanın çalıştığı ana bilgisayarda CPU kullanımını kontrol edin. CPU kullanımı yüzde 90 veya daha fazlaysa, uygulamanızı daha yüksek yapılandırmaya sahip bir ana bilgisayarda çalıştırın. Veya yükü daha fazla makineye dağıtabilirsiniz.

#### <a name="connection-throttling"></a>Bağlantı azaltma
Bağlantı azaltma, [ana bilgisayar makinesindeki bağlantı sınırı] veya [Azure SNAT (PAT) bağlantı noktası tükenmesi]nedeniyle gerçekleşebilir.

##### <a name="connection-limit-on-a-host-machine"></a><a name="connection-limit-on-host"></a>Ana makinede bağlantı sınırı
Red Hat gibi bazı Linux sistemlerinin toplam açık dosya sayısı üzerinde bir üst sınırı vardır. Linux'taki soketler dosya olarak uygulandığından, bu sayı toplam bağlantı sayısını da sınırlar.
Şu komutu çalıştırın.

```bash
ulimit -a
```
"Nofile" olarak tanımlanan, izin verilen maksimum açık dosya sayısının bağlantı havuzu boyutunun en az iki katı olması gerekir. Daha fazla bilgi için [Performans ipuçlarına](performance-tips-async-java.md)bakın.

##### <a name="azure-snat-pat-port-exhaustion"></a><a name="snat"></a>Azure SNAT (PAT) bağlantı noktası tükenmesi

Uygulamanız herkese açık bir IP adresi olmadan Azure Sanal Makinelerde dağıtılıyorsa, varsayılan olarak [Azure SNAT bağlantı noktaları](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) VM'nizin dışındaki herhangi bir uç noktaya bağlantılar kurar. VM'den Azure Cosmos DB bitiş noktasına izin verilen bağlantı sayısı [Azure SNAT yapılandırması](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports)ile sınırlıdır.

 Azure SNAT bağlantı noktaları yalnızca VM'nizin özel bir IP adresine sahip olması ve VM'den gelen bir işlem ortak bir IP adresine bağlanmaya çalıştığında kullanılır. Azure SNAT sınırlamasını önlemek için iki geçici çözüm vardır:

* Azure Cosmos DB hizmet bitiş noktanızı Azure Sanal Makineler sanal ağınızın alt ağına ekleyin. Daha fazla bilgi için Azure [Sanal Ağ hizmeti bitiş noktalarına](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)bakın. 

    Hizmet bitiş noktası etkinleştirildiğinde, istekler artık ortak bir IP'den Azure Cosmos DB'ye gönderilmez. Bunun yerine, sanal ağ ve alt ağ kimliği gönderilir. Bu değişiklik, yalnızca genel IP'lere izin verilirse güvenlik duvarının düşmesine neden olabilir. Bir güvenlik duvarı kullanıyorsanız, hizmet bitiş noktasını etkinleştirdiğinizde, [Sanal Ağ ALA'larını](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl)kullanarak güvenlik duvarına bir alt ağ ekleyin.
* Azure VM'nize herkese açık bir IP atayın.

##### <a name="cant-reach-the-service---firewall"></a><a name="cant-connect"></a>Hizmete ulaşamıyorum - güvenlik duvarı
``ConnectTimeoutException``SDK'nın hizmete ulaşamayacağını gösterir.
Doğrudan modu kullanırken aşağıdakilere benzer bir hata alabilirsiniz:
```
GoneException{error=null, resourceAddress='https://cdb-ms-prod-westus-fd4.documents.azure.com:14940/apps/e41242a5-2d71-5acb-2e00-5e5f744b12de/services/d8aa21a5-340b-21d4-b1a2-4a5333e7ed8a/partitions/ed028254-b613-4c2a-bf3c-14bd5eb64500/replicas/131298754052060051p//', statusCode=410, message=Message: The requested resource is no longer available at the server., getCauseInfo=[class: class io.netty.channel.ConnectTimeoutException, message: connection timed out: cdb-ms-prod-westus-fd4.documents.azure.com/101.13.12.5:14940]
```

Uygulama makinenizde çalışan bir güvenlik duvarınız varsa, doğrudan mod tarafından kullanılan 10.000 ila 20.000 bağlantı noktası aralığını açın.
Ayrıca [bir ana makinede Bağlantı sınırını](#connection-limit-on-host)izleyin.

#### <a name="http-proxy"></a>HTTP proxy

BIR HTTP proxy kullanıyorsanız, SDK'da `ConnectionPolicy`yapılandırılan bağlantı sayısını desteklediğinden emin olun.
Aksi takdirde, bağlantı sorunlarıyla karşı karşıya kaldığınız da olur.

#### <a name="invalid-coding-pattern-blocking-netty-io-thread"></a>Geçersiz kodlama deseni: Netty IO iş parçacığı engelleme

SDK, Azure Cosmos DB ile iletişim kurmak için [Netty](https://netty.io/) IO kitaplığını kullanır. SDK Async API'leri vardır ve Netty'nin engelleyici olmayan IO API'lerini kullanır. SDK'nın IO çalışması IO Netty iplikleri üzerinde gerçekleştirilir. IO Netty iş parçacığı sayısı, uygulama makinesinin CPU çekirdek sayısıyla aynı şekilde yapılandırılmıştır. 

Netty IO iplikleri sadece engelleyici olmayan Netty IO çalışmaları için kullanılmak üzere hazırlanmaktadır. SDK, Netty IO iş parçacığının api çağırma sonucunu uygulamanın koduna döndürür. Uygulama Netty iş parçacığı üzerinde sonuç aldıktan sonra uzun süreli bir işlem gerçekleştirirse, SDK dahili IO çalışmasını gerçekleştirmek için yeterli IO iş parçacığına sahip olmayabilir. Bu tür uygulama kodlaması düşük iş elde etme, yüksek gecikme sonu ve `io.netty.handler.timeout.ReadTimeoutException` hatalara neden olabilir. Geçici çözüm, işlemin zaman aldığını bildiğinizde iş parçacığını değiştirmektir.

Örneğin, aşağıdaki kod parçacığına bir göz atın. Netty iş parçacığı üzerinde birkaç milisaniyeden fazla süren uzun süreli çalışma gerçekleştirebilirsiniz. Eğer öyleyse, sonunda hiçbir Netty IO iş parçacığı iO iş işlemek için mevcut bir duruma alabilirsiniz. Sonuç olarak, bir ReadTimeoutException hatası alırsınız.
```java
@Test
public void badCodeWithReadTimeoutException() throws Exception {
    int requestTimeoutInSeconds = 10;

    ConnectionPolicy policy = new ConnectionPolicy();
    policy.setRequestTimeoutInMillis(requestTimeoutInSeconds * 1000);

    AsyncDocumentClient testClient = new AsyncDocumentClient.Builder()
            .withServiceEndpoint(TestConfigurations.HOST)
            .withMasterKeyOrResourceToken(TestConfigurations.MASTER_KEY)
            .withConnectionPolicy(policy)
            .build();

    int numberOfCpuCores = Runtime.getRuntime().availableProcessors();
    int numberOfConcurrentWork = numberOfCpuCores + 1;
    CountDownLatch latch = new CountDownLatch(numberOfConcurrentWork);
    AtomicInteger failureCount = new AtomicInteger();

    for (int i = 0; i < numberOfConcurrentWork; i++) {
        Document docDefinition = getDocumentDefinition();
        Observable<ResourceResponse<Document>> createObservable = testClient
                .createDocument(getCollectionLink(), docDefinition, null, false);
        createObservable.subscribe(r -> {
                    try {
                        // Time-consuming work is, for example,
                        // writing to a file, computationally heavy work, or just sleep.
                        // Basically, it's anything that takes more than a few milliseconds.
                        // Doing such operations on the IO Netty thread
                        // without a proper scheduler will cause problems.
                        // The subscriber will get a ReadTimeoutException failure.
                        TimeUnit.SECONDS.sleep(2 * requestTimeoutInSeconds);
                    } catch (Exception e) {
                    }
                },

                exception -> {
                    //It will be io.netty.handler.timeout.ReadTimeoutException.
                    exception.printStackTrace();
                    failureCount.incrementAndGet();
                    latch.countDown();
                },
                () -> {
                    latch.countDown();
                });
    }

    latch.await();
    assertThat(failureCount.get()).isGreaterThan(0);
}
```
   Geçici çözüm, zaman alan işi gerçekleştirdiğiniz iş parçacığıdeğiştirmektir. Uygulamanız için zamanlayıcının tek tonluk bir örneğini tanımlayın.
   ```java
// Have a singleton instance of an executor and a scheduler.
ExecutorService ex  = Executors.newFixedThreadPool(30);
Scheduler customScheduler = rx.schedulers.Schedulers.from(ex);
   ```
   Örneğin, hesaplama gereği ağır işler veya IO'nun engellenmesi gibi zaman alan işler yapmanız gerekebilir. Bu durumda, IŞ parçacığı nın API'yi kullanarak sizin `customScheduler` tarafından sağlanan bir alt işçiye `.observeOn(customScheduler)` geçiş ini.
```java
Observable<ResourceResponse<Document>> createObservable = client
        .createDocument(getCollectionLink(), docDefinition, null, false);

createObservable
        .observeOn(customScheduler) // Switches the thread.
        .subscribe(
            // ...
        );
```
Kullanarak, `observeOn(customScheduler)`Netty IO iş parçacığı bırakın ve özel zamanlayıcı tarafından sağlanan kendi özel iş parçacığı geçmek. Bu değişiklik sorunu çözer. Artık başarısız `io.netty.handler.timeout.ReadTimeoutException` olmayacaksın.

### <a name="connection-pool-exhausted-issue"></a>Bağlantı havuzu tükenmiş sorunu

`PoolExhaustedException`istemci tarafı hatasıdır. Bu hata, uygulama iş yükünüzün SDK bağlantı havuzunun hizmet edebileceğinden daha yüksek olduğunu gösterir. Bağlantı havuzu boyutunu artırın veya birden çok uygulama üzerindeki yükü dağıtın.

### <a name="request-rate-too-large"></a>İstek oranı çok büyük
Bu hata sunucu tarafı hatasıdır. Bu, size sağlanan iş lerinizi tükettiğinizi gösterir. Daha sonra tekrar dene. Bu hatayı sık sık alırsanız, koleksiyon iş tindesinde bir artış düşünün.

### <a name="failure-connecting-to-azure-cosmos-db-emulator"></a>Azure Cosmos DB emülatörüne bağlanma hatası

Azure Cosmos DB emülatörü HTTPS sertifikası kendi imzasıdır. SDK'nın emülatörle çalışması için emülatör sertifikasını bir Java TrustStore'a aktarın. Daha fazla bilgi için azure [cosmos DB emülatör sertifikalarını dışa](local-emulator-export-ssl-certificates.md)aktarın.

### <a name="dependency-conflict-issues"></a>Bağımlılık Çakışma Sorunları

```console
Exception in thread "main" java.lang.NoSuchMethodError: rx.Observable.toSingle()Lrx/Single;
```

Yukarıdaki özel durum, RxJava lib'in eski bir sürümüne (örn. 1.2.2) bağımlı olduğunuzu göstermektedir. Bizim SDK RxJava önceki sürümünde mevcut olmayan API'ler olan RxJava 1.3.8 dayanır. 

Bu tür issuses için geçici çözüm diğer bağımlılık RxJava-1.2.2 getiriyor belirlemek ve RxJava-1.2.2 geçişli bağımlılık hariç ve CosmosDB SDK yeni sürümü getirmek izin vermektir.

Hangi kitaplığın RxJava-1.2.2'yi getirdiğini belirlemek için proje pom.xml dosyanızın yanındaki aşağıdaki komutu çalıştırın:
```bash
mvn dependency:tree
```
Daha fazla bilgi [için, maven bağımlılık ağacı kılavuzuna](https://maven.apache.org/plugins/maven-dependency-plugin/examples/resolving-conflicts-using-the-dependency-tree.html)bakın.

RxJava-1.2.2'yi tanımladıktan sonra, projenizin diğer bağımlılığının geçişli bağımlılık olduğunu, pom dosyanızdaki lib'e olan bağımlılığı değiştirebilir ve RxJava geçişli bağımlılığını hariç tutabilirsiniz:

```xml
<dependency>
  <groupId>${groupid-of-lib-which-brings-in-rxjava1.2.2}</groupId>
  <artifactId>${artifactId-of-lib-which-brings-in-rxjava1.2.2}</artifactId>
  <version>${version-of-lib-which-brings-in-rxjava1.2.2}</version>
  <exclusions>
    <exclusion>
      <groupId>io.reactivex</groupId>
      <artifactId>rxjava</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

Daha fazla bilgi için [geçişli bağımlılık kılavuzunu hariç](https://maven.apache.org/guides/introduction/introduction-to-optional-and-excludes-dependencies.html)tut.


## <a name="enable-client-sdk-logging"></a><a name="enable-client-sice-logging"></a>İstemci SDK günlüğe kaydetmeyi etkinleştirme

Java Async SDK log4j ve logback gibi popüler günlük çerçeveleri oturum destekleyen günlük cephe olarak SLF4j kullanır.

Örneğin, log4j'yi günlük çerçevesi olarak kullanmak istiyorsanız, Java classpath'inize aşağıdaki libs'leri ekleyin.

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>${slf4j.version}</version>
</dependency>
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>${log4j.version}</version>
</dependency>
```

Ayrıca bir log4j config ekleyin.
```
# this is a sample log4j configuration

# Set root logger level to DEBUG and its only appender to A1.
log4j.rootLogger=INFO, A1

log4j.category.com.microsoft.azure.cosmosdb=DEBUG
#log4j.category.io.netty=INFO
#log4j.category.io.reactivex=INFO
# A1 is set to be a ConsoleAppender.
log4j.appender.A1=org.apache.log4j.ConsoleAppender

# A1 uses PatternLayout.
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%d %5X{pid} [%t] %-5p %c - %m%n
```

Daha fazla bilgi [için, sfl4j günlük kılavuzuna](https://www.slf4j.org/manual.html)bakın.

## <a name="os-network-statistics"></a><a name="netstats"></a>İşletim sistemi ağ istatistikleri
Gibi durumlarda kaç bağlantı olduğunu anlamak için netstat komutunu `ESTABLISHED` `CLOSE_WAIT`çalıştırın.

Linux'ta aşağıdaki komutu çalıştırabilirsiniz.
```bash
netstat -nap
```
Sonucu yalnızca Azure Cosmos DB bitiş noktasına bağlantılara filtreleyin.

`ESTABLISHED` Eyaletteki Azure Cosmos DB bitiş noktasına bağlantı sayısı, yapılandırılan bağlantı havuzu boyutundan büyük olamaz.

Azure Cosmos DB bitiş noktasına birçok bağlantı `CLOSE_WAIT` durumda olabilir. 1000'den fazla olabilir. Yüksek bir sayı bağlantıların hızlı bir şekilde kurulduğunu ve yıkıldığını gösterir. Bu durum potansiyel olarak sorunlara neden olur. Daha fazla bilgi için [Ortak sorunlar ve geçici çözüm bölümüne] bakın.

 <!--Anchors-->
[Yaygın sorunlar ve geçici çözümler]: #common-issues-workarounds
[Enable client SDK logging]: #enable-client-sice-logging
[Ana makinede bağlantı sınırı]: #connection-limit-on-host
[Azure SNAT (PAT) bağlantı noktası tükenmesi]: #snat


