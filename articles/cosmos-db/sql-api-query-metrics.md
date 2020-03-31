---
title: Azure Cosmos DB SQL API için SQL sorgu ölçümleri
description: Azure Cosmos DB isteklerinin SQL sorgu performansını nasıl enstrüman ve hata ayıklama hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.openlocfilehash: ae1773ec1d470b9cff2efb00c200427b7b4c2fb4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69614828"
---
# <a name="tuning-query-performance-with-azure-cosmos-db"></a>Azure Cosmos DB ile sorgu performansını ayarlama

Azure Cosmos DB, şema veya ikincil dizinler gerektirmeden [verileri sorgulamak için](how-to-sql-query.md)bir SQL API sağlar. Bu makalede, geliştiriciler için aşağıdaki bilgileri sağlar:

* Azure Cosmos DB'nin SQL sorgu yürütmesi nasıl çalıştığına ilişkin üst düzey ayrıntılar
* Sorgu isteği ve yanıt üstbilgileri ve istemci SDK seçenekleri yle ilgili ayrıntılar
* Sorgu performansı için ipuçları ve en iyi uygulamalar
* Sorgu performansını hata ayıklamak için SQL yürütme istatistiklerini nasıl kullanabileceğine örnekler

## <a name="about-sql-query-execution"></a>SQL sorgu yürütme hakkında

Azure Cosmos DB'de, verileri herhangi bir depolama boyutuna veya [iş kaynağı isteğine](partition-data.md)kadar büyüyebilen kapsayıcılarda depolarsınız. Azure Cosmos DB, veri büyümesini veya sağlanan iş veri sini artırmak için verileri kapakların altındaki fiziksel bölümler arasında sorunsuz bir şekilde ölçeklendiriyor. REST API'yi veya desteklenen [SQL SDK'larından](sql-api-sdk-dotnet.md)birini kullanarak herhangi bir kapsayıcıya SQL sorguları verebilirsiniz.

Bölümleme kısa bir bakış: verilerin fiziksel bölümler arasında nasıl bölündüğünü belirleyen "şehir" gibi bir bölüm anahtarı tanımlarsınız. Tek bir bölüm anahtarına ait veriler (örneğin, "şehir" == "Seattle") fiziksel bir bölüm içinde depolanır, ancak genellikle tek bir fiziksel bölümün birden çok bölüm anahtarı vardır. Bir bölüm depolama boyutuna ulaştığında, hizmet bölümü sorunsuz bir şekilde iki yeni bölüme böler ve bölüm anahtarını bu bölümler arasında eşit olarak böler. Bölümler geçici olduğundan, API'ler bölüm anahtarı işleme aralıklarını gösteren bir "bölüm anahtar aralığı" soyutlama kullanır. 

Azure Cosmos DB'ye bir sorgu verdiyseniz, SDK şu mantıksal adımları gerçekleştirir:

* Sorgu yürütme planını belirlemek için SQL sorgusunu ayrıştın. 
* Sorgu, bölüm tuşuna karşı bir `SELECT * FROM c WHERE c.city = "Seattle"`filtre içeriyorsa, örneğin , tek bir bölüme yönlendirilir. Sorgunun bölüm anahtarında bir filtresi yoksa, tüm bölümlerde yürütülür ve sonuçlar istemci tarafında birleştirilir.
* Sorgu, istemci yapılandırmasını temel alan, seri veya paralel olarak her bölüm içinde yürütülür. Her bölüm içinde, sorgu karmaşıklığına, yapılandırılmış sayfa boyutuna ve koleksiyonun sağlanan iş haline bağlı olarak bir veya daha fazla gidiş-dönüş turu yapabilir. Her yürütme sorgu yürütme tarafından tüketilen [istek birimlerinin](request-units.md) sayısını döndürür ve isteğe bağlı olarak, sorgu yürütme istatistikleri. 
* SDK, sorgu sonuçlarının bölümler arasında özetlenmesi gerçekleştirir. Örneğin, sorgu bölümler arasında bir ORDER BY içeriyorsa, tek tek bölümlerden elde edilen sonuçlar, genel olarak sıralanmış sırada sonuçları döndürmek için birleştirilir. Sorgu gibi `COUNT`bir toplama ise, tek tek bölümlerden sayımları genel sayı üretmek için toplanır.

SDK'lar sorgu yürütmeiçin çeşitli seçenekler sağlar. Örneğin, .NET'te bu seçenekler `FeedOptions` sınıfta kullanılabilir. Aşağıdaki tabloda bu seçenekleri ve sorgu yürütme süresini nasıl etkiledikleri açıklanmaktadır. 

| Seçenek | Açıklama |
| ------ | ----------- |
| `EnableCrossPartitionQuery` | Birden fazla bölüm boyunca yürütülmesi gereken herhangi bir sorgu için doğru ayarlanmış olmalıdır. Bu, geliştirme sırasında bilinçli performans dengeleri yapmanızı sağlayan açık bir bayraktır. |
| `EnableScanInQuery` | Dizin oluşturmayı devre dışı bıraktıysanız, ancak yine de bir tarak sorguyu çalıştırmak istiyorsanız, doğru olarak ayarlanmalıdır. Yalnızca istenen filtre yolu için dizin oluşturma devre dışı bırakılırsa uygulanabilir. | 
| `MaxItemCount` | Sunucuya gidiş dönüş başına döndürülecek maksimum öğe sayısı. -1'e ayarlayarak, sunucunun öğe sayısını yönetmesine izin verebilirsiniz. Veya, gidiş-dönüş başına yalnızca az sayıda öğe almak için bu değeri düşürebilirsiniz. 
| `MaxBufferedItemCount` | Bu istemci tarafı seçeneğidir ve çapraz bölümleme ORDER BY gerçekleştirirken bellek tüketimini sınırlamak için kullanılır. Daha yüksek bir değer, çapraz bölme sıralamanın gecikme süresini azaltmaya yardımcı olur. |
| `MaxDegreeOfParallelism` | Azure Cosmos veritabanı hizmetinde paralel sorgu yürütme sırasında istemci tarafında çalışan eşzamanlı işlem sayısını alır veya ayarlar. Pozitif özellik değeri, eşzamanlı işlem sayısını ayarlanan değerle sınırlar. 0'dan az olarak ayarlanırsa, sistem otomatik olarak çalışacak eşzamanlı işlem sayısına karar verir. |
| `PopulateQueryMetrics` | Derleme süresi, dizin döngü süresi ve belge yükleme süresi gibi sorgu yürütmenin çeşitli aşamalarında harcanan zamanın istatistiklerinin ayrıntılı günlüğe kaydedilmesini sağlar. Sorgu performansı sorunlarını tanılamak için sorgu istatistiklerinden çıktıyı Azure Desteği ile paylaşabilirsiniz. |
| `RequestContinuation` | Herhangi bir sorgu tarafından döndürülen opak devamı belirteci geçerek sorgu yürütme devam edebilirsiniz. Devam belirteci sorgu yürütme için gerekli tüm durumu kapsüller. |
| `ResponseContinuationTokenLimitInKb` | Sunucu tarafından döndürülen devam belirteci nin maksimum boyutunu sınırlayabilirsiniz. Uygulama ana bilgisayarınızın yanıt üstbilgi boyutunda sınırları varsa bunu ayarlamanız gerekebilir. Bunu ayarlamak, sorgu için tüketilen toplam süreyi ve US'yi artırabilir.  |

Örneğin, bölüm anahtarı `/city` olarak bir koleksiyonda istenen ve 100.000 RU/s'lik bir iş bölümü yle birlikte verilen bölüm anahtarıyla ilgili örnek bir sorgu alalım. Bu sorguyu `CreateDocumentQuery<T>` .NET'te aşağıdaki gibi kullanarak talep esiniz:

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
        MaxItemCount = -1, 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();
```

Yukarıda gösterilen SDK parçacığı, aşağıdaki REST API isteğine karşılık gelir:

```
POST https://arramacquerymetrics-westus.documents.azure.com/dbs/db/colls/sample/docs HTTP/1.1
x-ms-continuation: 
x-ms-documentdb-isquery: True
x-ms-max-item-count: -1
x-ms-documentdb-query-enablecrosspartition: True
x-ms-documentdb-query-parallelizecrosspartitionquery: True
x-ms-documentdb-query-iscontinuationexpected: True
x-ms-documentdb-populatequerymetrics: True
x-ms-date: Tue, 27 Jun 2017 21:52:18 GMT
authorization: type%3dmaster%26ver%3d1.0%26sig%3drp1Hi83Y8aVV5V6LzZ6xhtQVXRAMz0WNMnUuvriUv%2b4%3d
x-ms-session-token: 7:8,6:2008,5:8,4:2008,3:8,2:2008,1:8,0:8,9:8,8:4008
Cache-Control: no-cache
x-ms-consistency-level: Session
User-Agent: documentdb-dotnet-sdk/1.14.1 Host/32-bit MicrosoftWindowsNT/6.2.9200.0
x-ms-version: 2017-02-22
Accept: application/json
Content-Type: application/query+json
Host: arramacquerymetrics-westus.documents.azure.com
Content-Length: 52
Expect: 100-continue

{"query":"SELECT * FROM c WHERE c.city = 'Seattle'"}
```

Her sorgu yürütme sayfası `POST` `Accept: application/query+json` üstbilgi ile BIR REST API ve gövdedeki SQL sorgusu karşılık gelir. Her sorgu yürütme devam etmek için istemci `x-ms-continuation` ve sunucu arasında yankılanan belirteç ile sunucuya bir veya daha fazla gidiş-dönüş gezileri yapar. FeedOptions'daki yapılandırma seçenekleri istek üstbilgisi biçiminde sunucuya aktarılır. Örneğin, `MaxItemCount` karşılık `x-ms-max-item-count`gelir. 

İstek aşağıdaki (okunabilirlik için kesilen) yanıtı döndürür:

```
HTTP/1.1 200 Ok
Cache-Control: no-store, no-cache
Pragma: no-cache
Transfer-Encoding: chunked
Content-Type: application/json
Server: Microsoft-HTTPAPI/2.0
Strict-Transport-Security: max-age=31536000
x-ms-last-state-change-utc: Tue, 27 Jun 2017 21:01:57.561 GMT
x-ms-resource-quota: documentSize=10240;documentsSize=10485760;documentsCount=-1;collectionSize=10485760;
x-ms-resource-usage: documentSize=1;documentsSize=884;documentsCount=2000;collectionSize=1408;
x-ms-item-count: 2000
x-ms-schemaversion: 1.3
x-ms-alt-content-path: dbs/db/colls/sample
x-ms-content-path: +9kEANVq0wA=
x-ms-xp-role: 1
x-ms-documentdb-query-metrics: totalExecutionTimeInMs=33.67;queryCompileTimeInMs=0.06;queryLogicalPlanBuildTimeInMs=0.02;queryPhysicalPlanBuildTimeInMs=0.10;queryOptimizationTimeInMs=0.00;VMExecutionTimeInMs=32.56;indexLookupTimeInMs=0.36;documentLoadTimeInMs=9.58;systemFunctionExecuteTimeInMs=0.00;userFunctionExecuteTimeInMs=0.00;retrievedDocumentCount=2000;retrievedDocumentSize=1125600;outputDocumentCount=2000;writeOutputTimeInMs=18.10;indexUtilizationRatio=1.00
x-ms-request-charge: 604.42
x-ms-serviceversion: version=1.14.34.4
x-ms-activity-id: 0df8b5f6-83b9-4493-abda-cce6d0f91486
x-ms-session-token: 2:2008
x-ms-gatewayversion: version=1.14.33.2
Date: Tue, 27 Jun 2017 21:59:49 GMT
```

Sorgudan döndürülen anahtar yanıt üstbilgisi aşağıdakileri içerir:

| Seçenek | Açıklama |
| ------ | ----------- |
| `x-ms-item-count` | Yanıtta döndürülen öğe sayısı. Bu, sağlanan `x-ms-max-item-count`, maksimum yanıt yükü boyutuna sığabilecek madde sayısına, sağlanan iş girdisi ve sorgu yürütme süresine bağlıdır. |  
| `x-ms-continuation:` | Ek sonuçlar varsa, sorgunun yürütülmesini devam ettirmek için devam belirteci. | 
| `x-ms-documentdb-query-metrics` | Yürütme için sorgu istatistikleri. Bu, sorgu yürütmenin çeşitli aşamalarında harcanan zamanın istatistiklerini içeren sınırlı bir dizedir. ' `x-ms-documentdb-populatequerymetrics` olarak ayarlanırsa `True`döndürülür | 
| `x-ms-request-charge` | Sorgu tarafından tüketilen [istek birimlerinin](request-units.md) sayısı. | 

REST API istek üstbilgileri ve seçenekleri hakkında ayrıntılı bilgi için, [REST API'yi kullanarak kaynakları sorgulama](https://docs.microsoft.com/rest/api/cosmos-db/querying-cosmosdb-resources-using-the-rest-api)ya da

## <a name="best-practices-for-query-performance"></a>Sorgu performansı için en iyi uygulamalar
Azure Cosmos DB sorgu performansını etkileyen en yaygın etkenler şunlardır. Bu makalede bu konuların her birini daha derine iniyoruz.

| Faktörü | İpucu | 
| ------ | -----| 
| Sağlanan aktarım hızı | Sorgu başına RU'yu ölçün ve sorgularınız için gerekli verilen iş sonuna sahip olduğundan emin olun. | 
| Bölümleme ve bölme tuşları | Düşük gecikme süresiyle filtre yan tümcesindeki bölüm anahtar değeriyle sorguları tercih edin. |
| SDK ve sorgu seçenekleri | Doğrudan bağlantı gibi SDK en iyi uygulamalarını izleyin ve istemci tarafındaki sorgu yürütme seçeneklerini ayarlayın. |
| Ağ gecikmesi | Ölçümde ağ ek yükü için hesap ve en yakın bölgeden okumak için çok yönlü API'ler kullanın. |
| Dizin Oluşturma İlkesi | Sorgu için gerekli dizin oluşturma yollarına/ilkesine sahip olduğundan emin olun. |
| Yürütme yi sorgula ölçümleri | Sorgu ve veri şekillerinin olası yeniden yazımlarını belirlemek için sorgu yürütme ölçümlerini çözümleyin.  |

### <a name="provisioned-throughput"></a>Sağlanan aktarım hızı
Cosmos DB'de, her biri istek birimlerinde (RU) saniyede ifade edilen ayrılmış iş ortası olan veri kapsayıcıları oluşturursunuz. 1-KB belgenin okunması 1 RU'dur ve her işlem (sorgular dahil) karmaşıklığına bağlı olarak sabit sayıda RUs'a normalleştirilmiştir. Örneğin, kabınız için 1000 RU/s'niz varsa ve 5 `SELECT * FROM c WHERE c.city = 'Seattle'` RUs tüketen bir sorgunuz varsa, saniyede 200 sorgu (1000 RU/s) / (5 RU/sorgu) = 200 sorgu/s tür sorgu gerçekleştirebilirsiniz. 

200'den fazla sorgu/sn gönderirseniz, hizmet 200/s'lerin üzerinde hız sınırlayıcı gelen istekleri başlatır. SDK'lar bir geri dönüş/yeniden deneme gerçekleştirerek bu servis talebiyle otomatik olarak işleyebilir, bu nedenle bu sorgular için daha yüksek bir gecikme gecikmesi fark edebilirsiniz. Sağlanan iş girdisini gerekli değere yükseltme, sorgu gecikmenizi ve iş inizi artırır. 

İstek birimleri hakkında daha fazla bilgi edinmek için [İstek birimlerine](request-units.md)bakın.

### <a name="partitioning-and-partition-keys"></a>Bölümleme ve bölme tuşları
Azure Cosmos DB ile, genellikle sorgular en hızlı/en verimliden daha yavaş/daha az verimliye doğru aşağıdaki sırada gerçekleştirir. 

* Tek bir bölüm tuşu ve öğe tuşu alın
* Tek bir bölüm anahtarında filtre yan tümcesi ile sorgu
* Herhangi bir özellik üzerinde eşitlik veya aralık filtresi yan tümcesi olmadan sorgula
* Filtresiz sorgula

Tüm bölümlere danışması gereken sorguların daha yüksek gecikme ye ihtiyacı vardır ve daha yüksek RUs'lar tüketebilir. Her bölüm tüm özelliklere karşı otomatik dizin oluşturma olduğundan, sorgu bu durumda dizin verimli bir şekilde sunulabilir. Paralellik seçeneklerini kullanarak bölümleri daha hızlı kapsayan sorgular yapabilirsiniz.

Bölümleme ve bölümleme tuşları hakkında daha fazla bilgi edinmek için [Azure Cosmos DB'de Bölümleme bölümüne](partition-data.md)bakın.

### <a name="sdk-and-query-options"></a>SDK ve sorgu seçenekleri
Azure Cosmos DB'den en iyi istemci performansının nasıl elde edilebildiğini öğrenmek için [Performans İpuçları](performance-tips.md) ve [Performans testlerine](performance-testing.md) bakın. Buna en son SDK'ların kullanılması, varsayılan bağlantı sayısı, çöp toplama sıklığı gibi platforma özgü yapılandırmalar ve Direct/TCP gibi hafif bağlantı seçeneklerinin kullanılması dahildir. 


#### <a name="max-item-count"></a>Maksimum Madde Sayısı
Sorgular için, değer `MaxItemCount` in uçtan uca sorgu süresi üzerinde önemli bir etkisi olabilir. Sunucuya yapılan her gidiş-dönüş, (Varsayılan `MaxItemCount` 100 öğe) öğe sayısından daha fazla döndürmez. Bunu daha yüksek bir değere ayarlamak (-1 maksimumdur ve önerilir) özellikle büyük sonuç kümelerine sahip sorgular için sunucu ve istemci arasındaki gidiş-dönüş sayısını sınırlayarak sorgu sürenizi genel olarak artırır.

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxItemCount = -1, 
    }).AsDocumentQuery();
```

#### <a name="max-degree-of-parallelism"></a>Paralelliğin Maksimum Derecesi
Sorgular için, `MaxDegreeOfParallelism` özellikle bölümler arası sorgular gerçekleştiriyorsanız (bölüm anahtarı değerinde filtre olmadan) uygulamanız için en iyi yapılandırmaları tanımlamak için ayarlayın. `MaxDegreeOfParallelism`paralel görevlerin maksimum sayısını, yani paralel olarak ziyaret edilecek en fazla bölümü denetler. 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();
```

Farz edelim ki.
* D = Varsayılan Maksimum paralel görev sayısı (= istemci makinesindeki toplam işlemci sayısı)
* P = Kullanıcı tarafından belirtilen en fazla paralel görev sayısı
* N = Bir sorguyu yanıtlamak için ziyaret edilmesi gereken bölüm sayısı

Paralel sorguların P'nin farklı değerleri için nasıl etki edeceği aşağıda veda edilmiştir.
* (P == 0) => Seri Modu
* (P == 1) => Bir görevin maksimum
* (P > 1) => Min (P, N) paralel görevleri 
* (P < 1) => Min (N, D) paralel görevleri

SDK sürüm notları ve uygulanan sınıflar ve yöntemlerle ilgili ayrıntılar için [SQL SDK'lara](sql-api-sdk-dotnet.md) bakın

### <a name="network-latency"></a>Ağ gecikmesi
Küresel dağıtımı nasıl ayarlayacak ve en yakın bölgeye bağlanmakonusunda [Azure Cosmos DB genel dağıtımına](tutorial-global-distribution-sql-api.md) bakın. Birden çok tur-trips yapmanız veya sorgudan ayarlanmış büyük bir sonuç almanız gerektiğinde ağ gecikmesi sorgu performansı üzerinde önemli bir etkiye sahiptir. 

Sorgu yürütme ölçümlerine ilişkin bölüm, sorgu yürütmede harcanan `totalExecutionTimeInMs`süre ile ağ aktarımında harcanan süre arasında ayrım yapabilmeniz için sorguların sunucu yürütme süresini nasıl alabileceğinizi açıklar.

### <a name="indexing-policy"></a>Dizin oluşturma ilkesi
Bkz. [Dizine](index-policy.md) dizini dizini oluşturma, türler ve modlar ve bunların sorgu yürütmeyi nasıl etkilediği. Varsayılan olarak, dizin oluşturma ilkesi, eşitlik sorguları için etkili olan dizeleri için Karma dizin oluşturma kullanır, ancak sorgular tarafından aralık sorguları/sırası için değil. Dizeleri için aralık sorguları gerekiyorsa, tüm dizeleri için Aralık dizin türü belirtmenizi öneririz. 

Varsayılan olarak, Azure Cosmos DB tüm verilere otomatik dizin oluşturma uygular. Yüksek performanslı ekleme senaryoları için, her kesici uç işlemi için RU maliyetini azaltacağı için yolları hariç almayı düşünün. 

## <a name="query-execution-metrics"></a>Yürütme yi sorgula ölçümleri
İsteğe bağlı `x-ms-documentdb-populatequerymetrics` üstbilgiden (.NET`FeedOptions.PopulateQueryMetrics` SDK'da) geçerek sorgu yürütmesi hakkında ayrıntılı ölçümler alabilirsiniz. Döndürülen `x-ms-documentdb-query-metrics` değer, sorgu yürütmesinin gelişmiş sorun giderme için dir. 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();

// Returns metrics by partition key range Id
IReadOnlyDictionary<string, QueryMetrics> metrics = result.QueryMetrics;

```

| Ölçüm | Birim | Açıklama | 
| ------ | -----| ----------- |
| `totalExecutionTimeInMs` | milisaniye | Yürütme süresini sorgula | 
| `queryCompileTimeInMs` | milisaniye | Zamanı derleme sorgusu  | 
| `queryLogicalPlanBuildTimeInMs` | milisaniye | Mantıksal sorgu planı oluşturma zamanı | 
| `queryPhysicalPlanBuildTimeInMs` | milisaniye | Fiziksel sorgu planı oluşturma zamanı | 
| `queryOptimizationTimeInMs` | milisaniye | Sorguyı en iyi duruma geçirmede harcanan süre | 
| `VMExecutionTimeInMs` | milisaniye | Sorgu çalışma zamanında harcanan süre | 
| `indexLookupTimeInMs` | milisaniye | Fiziksel dizin katmanında harcanan süre | 
| `documentLoadTimeInMs` | milisaniye | Belgelerin yüklenmesinde harcanan süre  | 
| `systemFunctionExecuteTimeInMs` | milisaniye | Milisaniye cinsinden sistem (yerleşik) işlevlerini yürütme için harcanan toplam süre  | 
| `userFunctionExecuteTimeInMs` | milisaniye | Kullanıcı tanımlı işlevleri milisaniye cinsinden yürütmek için harcanan toplam süre | 
| `retrievedDocumentCount` | count | Alınan toplam belge sayısı  | 
| `retrievedDocumentSize` | Bayt | Baytlarda alınan belgelerin toplam boyutu  | 
| `outputDocumentCount` | count | Çıktı belgelerinin sayısı | 
| `writeOutputTimeInMs` | milisaniye | Yürütme süresini milisaniye cinsinden sorgula | 
| `indexUtilizationRatio` | oranı (<=1) | Filtre yle eşleşen belge sayısının yüklenen belge sayısına oranı  | 

İstemci SDK'ları, her bölüm içinde sorguyu sunmak için dahili olarak birden çok sorgu işlemi yapabilir. İstemci, toplam sonuçlar aşArsa, `x-ms-max-item-count`sorgu bölüm için sağlanan iş belini aşarsa veya sorgu yükü sayfa başına maksimum boyuta ulaşırsa veya sorgu sistem tahsis edilen zaman aşımı sınırına ulaşırsa, bölüm başına birden fazla arama yapar. Her kısmi sorgu `x-ms-documentdb-query-metrics` yürütme si bu sayfa için bir döndürür. 

Aşağıda bazı örnek sorgular ve sorgu yürütmesinden döndürülen ölçümlerden bazılarının nasıl yorumlanacağı ve aşağıdakiler ve 

| Sorgu | Örnek Metrik | Açıklama | 
| ------ | -----| ----------- |
| `SELECT TOP 100 * FROM c` | `"RetrievedDocumentCount": 101` | Alınan belge sayısı TOP yan tümcesi ile eşleşecek şekilde 100+1'dir. Sorgu süresi çoğunlukla `WriteOutputTime` harcanıyor ve `DocumentLoadTime` bir tarayıp bu yüzden. | 
| `SELECT TOP 500 * FROM c` | `"RetrievedDocumentCount": 501` | Alınan DocumentCount artık daha yüksektir (TOP yan tümcesi ile eşleşecek şekilde 500+1). | 
| `SELECT * FROM c WHERE c.N = 55` | `"IndexLookupTime": "00:00:00.0009500"` | Yaklaşık 0,9 ms indexLookupTime'da anahtar araması için harcanıyor, `/N/?`çünkü bu bir dizin araması. | 
| `SELECT * FROM c WHERE c.N > 55` | `"IndexLookupTime": "00:00:00.0017700"` | Bir dizi araması olduğu için indexLookupTime'da aralık taramak için biraz daha fazla `/N/?`zaman (1,7 ms) harcandı. | 
| `SELECT TOP 500 c.N FROM c` | `"IndexLookupTime": "00:00:00.0017700"` | Önceki sorgularla `DocumentLoadTime` aynı zaman harcanıyor, ancak yalnızca bir özelliği yansıttığımız için daha düşük. `WriteOutputTime` | 
| `SELECT TOP 500 udf.toPercent(c.N) FROM c` | `"UserDefinedFunctionExecutionTime": "00:00:00.2136500"` | Yaklaşık 213 ms `UserDefinedFunctionExecutionTime` her değeri üzerinde UDF `c.N`yürütülmesi harcanıyor. |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(c.Name, 'Den')` | `"IndexLookupTime": "00:00:00.0006400", "SystemFunctionExecutionTime": "00:00:00.0074100"` | Yaklaşık 0,6 ms `IndexLookupTime` harcanmış `/Name/?`. Sorgu yürütme süresinin çoğu (~7 `SystemFunctionExecutionTime`ms) içinde. |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(LOWER(c.Name), 'den')` | `"IndexLookupTime": "00:00:00", "RetrievedDocumentCount": 2491,  "OutputDocumentCount": 500` | Sorgu, kullandığı `LOWER`ndan tarayıp , 2491 alınan belgeden 500'ü döndürülderken, tarayıp gerçekleştirilir. |


## <a name="next-steps"></a>Sonraki adımlar
* Desteklenen SQL sorgu işleçleri ve anahtar kelimeler hakkında bilgi edinmek için [SQL sorgusuna](sql-query-getting-started.md)bakın. 
* İstek birimleri hakkında bilgi edinmek için [istek birimlerine](request-units.md)bakın.
* Dizin oluşturma ilkesi hakkında bilgi edinmek için [dizin oluşturma ilkesine](index-policy.md) bakın 


