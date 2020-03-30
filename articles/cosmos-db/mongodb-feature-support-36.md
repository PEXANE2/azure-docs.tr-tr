---
title: Azure Cosmos DB'nin MongoDB (3,6 sürüm) için api destekli özellikleri ve sözdizimi
description: Azure Cosmos DB'nin MongoDB (3,6 sürüm) destekli özellikler ve sözdizimi için API'si hakkında bilgi edinin.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: overview
ms.date: 01/15/2020
author: sivethe
ms.author: sivethe
ms.openlocfilehash: cde731f9d9e673446bc4d08117004b028db2a7f9
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77462471"
---
# <a name="azure-cosmos-dbs-api-for-mongodb-36-version-supported-features-and-syntax"></a>MongoDB için Azure Cosmos DB API’si (sürüm 3.6): desteklenen özellikler ve söz dizimi

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Açık kaynak kodlu MongoDB istemci [sürücülerinden](https://docs.mongodb.org/ecosystem/drivers)herhangi birini kullanarak Azure Cosmos DB'nin MongoDB api'si ile iletişim kurabilirsiniz. Azure Cosmos DB'nin MongoDB için API'si, MongoDB [tel protokolüne](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol)bağlı kalarak varolan istemci sürücülerin kullanılmasını sağlar.

Azure Cosmos DB'nin MongoDB için API'sini kullanarak, Cosmos DB'nin sağladığı tüm kurumsal özelliklerle alışık olduğunuz MongoDB'nin avantajlarından yararlanabilirsiniz: [küresel dağıtım,](distribute-data-globally.md) [otomatik parçalama,](partition-data.md)kullanılabilirlik ve gecikme garantileri, istirahatte şifreleme, yedeklemeler ve çok daha fazlası.

## <a name="protocol-support"></a>Protokol Desteği

Azure Cosmos DB'nin MongoDB için API'si, yeni hesaplar için varsayılan olarak MongoDB sunucu sürümü **3.6** ile uyumludur. Desteklenen işleçler ve tüm sınırlamalar veya özel durumlar aşağıda listelenmiştir. Bu protokolleri anlayan tüm istemci sürücüleri MongoDB için Azure Cosmos DB’nin API’sine bağlanabilmesi gerekir. MongoDB hesapları için Azure Cosmos DB'nin API'sini kullanırken, hesapların 3,6 `*.mongo.cosmos.azure.com` sürümü biçiminde bitiş noktasına sahipken, hesapların 3,2 sürümünde bitiş noktasının biçiminde `*.documents.azure.com`olduğunu unutmayın.

## <a name="query-language-support"></a>Dil desteğini sorgula

Azure Cosmos DB'nin MongoDB için API'si, MongoDB sorgu dili yapıları için kapsamlı destek sağlar. Aşağıda şu anda desteklenen operasyonların, işleçlerin, aşamaların, komutların ve seçeneklerin ayrıntılı listesini bulabilirsiniz.

## <a name="database-commands"></a>Veritabanı komutları

Azure Cosmos DB'nin MongoDB için API'si aşağıdaki veritabanı komutlarını destekler:

### <a name="query-and-write-operation-commands"></a>Sorgulama ve yazma işlemi komutları

|Komut  |Destekleniyor |
|---------|---------|
|delete | Evet |
|find | Evet     |
|findAndModify | Evet  |
|getLastError|   Evet |
|getMore  |  Evet  |
|getPrevError | Hayır  |
|insert  |   Evet  |
|parallelCollectionScan  | Evet   |
|resetError |   Hayır  |
|update  |   Evet  |
|[Akış değiştirme](mongodb-change-streams.md)  |  Evet  |
|GridFS |   Evet  |

### <a name="authentication-commands"></a>Kimlik doğrulama komutları

|Komut  |Destekleniyor |
|---------|---------|
|authenticate    |   Evet      |
|logout    |      Evet   |
|getnonce   |    Evet     |


### <a name="administration-commands"></a>Yönetim komutları

|Komut  |Destekleniyor |
|---------|---------|
|Kapaklı Koleksiyonlar   |   Hayır      |
|klonCollectionAsCapped     |   Hayır      |
|collMod     |   Hayır      |
|collMod: expireAfterSeconds   |   Hayır      |
|convertToCapped   |  Hayır       |
|copydb     |  Hayır       |
|oluşturmaya   |    Evet     |
|createIndexes     |  Evet       |
|akımOp     |  Evet       |
|drop     |   Evet      |
|dropDatabase     |  Evet       |
|dropIndexes     |   Evet      |
|filemd5    |   Evet      |
|killCursors    |  Evet       |
|killOp     |   Hayır      |
|listCollections     |  Evet       |
|listDatabases     |  Evet       |
|listIndexes     |  Evet       |
|reIndex     |    Evet     |
|renameHatırlama     |    Hayır     |
|connectionStatus    |     Hayır    |

### <a name="diagnostics-commands"></a>Tanılama komutları

|Komut  |Destekleniyor |
|---------|---------|
|buildInfo       |   Evet      |
|collStats    |  Evet       |
|connPoolStats     |  Hayır       |
|connectionStatus     |  Hayır       |
|Datasize     |   Hayır      |
|dbHash    |    Hayır     |
|dbStats     |   Evet      |
|Açıklaya     | Hayır        |
|açıklamak: executionStats     |     Hayır    |
|özellikler     |    Hayır     |
|hostInfo     |   Hayır      |
|listDatabases       |   Evet      |
|listKomutlar     |  Hayır       |
|Profiler     |  Hayır       |
|Serverstatus     |  Hayır       |
|üst     |    Hayır     |
|whatsmyuri     |   Evet      |

<a name="aggregation-pipeline"/>

## <a name="aggregation-pipelinea"></a>Toplama ardışık düzeni</a>

### <a name="aggregation-commands"></a>Toplama komutları

|Komut  |Destekleniyor |
|---------|---------|
|aggregate |   Evet  |
|count     |   Evet  |
|distinct  | Evet |
|mapReduce | Hayır |

### <a name="aggregation-stages"></a>Toplama aşamaları

|Komut  |Destekleniyor |
|---------|---------|
|$collStats |Hayır|
|$project   |Evet|
|$match |Evet|
|$redact|   Evet|
|$limit |Evet|
|$skip  |Evet|
|$unwind|   Evet|
|$group |   Evet|
|$sample|       Evet|
|$sort  |Evet|
|$geoNear|  Hayır|
|$lookup    |   Evet|
|$out       |Evet|
|$indexStats|       Hayır|
|$facet |Hayır|
|$bucket|   Hayır|
|$bucketAuto|   Hayır|
|$sortByCount|  Evet|
|$addFields |Evet|
|$replaceRoot|  Evet|
|$count |Evet|
|$currentOp|    Hayır|
|$listLocalSessions |Hayır|
|$listSessions  |Hayır|
|$graphLookup   |Hayır|

### <a name="boolean-expressions"></a>Mantıksal ifadeler

|Komut  |Destekleniyor |
|---------|---------|
|$and| Evet|
|$or|Evet|
|$not|Evet|

### <a name="set-expressions"></a>Küme ifadeleri

|Komut  |Destekleniyor |
|---------|---------|
| $setEquals | Evet|
|$setIntersection|Evet|
| $setUnion|Evet|
| $setDifference|Evet|
| $setIsSubset|Evet|
| $anyElementTrue|Evet|
| $allElementsTrue|Evet|

### <a name="comparison-expressions"></a>Karşılaştırma ifadeleri

|Komut  |Destekleniyor |
|---------|---------|
|$cmp     |  Evet       |
|$eq|   Evet| 
|$gt |  Evet| 
|$gte|  Evet| 
|$lt    |Evet|
|$lte|  Evet| 
|$ne    |   Evet| 
|$in    |   Evet| 
|$nin   |   Evet| 

### <a name="arithmetic-expressions"></a>Aritmetik ifadeler

|Komut  |Destekleniyor |
|---------|---------|
|$abs |  Evet       |
| $add |  Evet       |
| $ceil |  Evet       |
| $divide |  Evet       |
| $exp |  Evet       |
| $floor |  Evet       |
| $ln |  Evet       |
| $log |  Evet       |
| $log10 |  Evet       |
| $mod |  Evet       |
| $multiply |  Evet       |
| $pow |  Evet       |
| $sqrt |  Evet       |
| $subtract |  Evet       |
| $trunc |  Evet       |

### <a name="string-expressions"></a>Dize ifadeleri

|Komut  |Destekleniyor |
|---------|---------|
|$concat |  Evet       |
| $indexOfBytes|  Evet       |
| $indexOfCP|  Evet       |
| $split|  Evet       |
| $strLenBytes|  Evet       |
| $strLenCP|  Evet       |
| $strcasecmp|  Evet       |
| $substr|  Evet       |
| $substrBytes|  Evet       |
| $substrCP|  Evet       |
| $toLower|  Evet       |
| $toUpper|  Evet       |

### <a name="text-search-operator"></a>Metin arama operatörü

|Komut  |Destekleniyor |
|---------|---------|
| $meta | Hayır|

### <a name="array-expressions"></a>Dizi ifadeleri

|Komut  |Destekleniyor |
|---------|---------|
|$arrayElemAt   |   Evet|
|$arrayToObject|    Evet|
|$concatArrays  |   Evet|
|$filter    |   Evet|
|$indexOfArray  |Evet|
|$isArray   |   Evet|
|$objectToArray |Evet|
|$range |Evet|
|$reverseArray  |   Evet|
|$reduce|   Evet|
|$size  |   Evet|
|$slice |   Evet|
|$zip   |   Evet|
|$in    |   Evet|

### <a name="variable-operators"></a>Değişken işleçler

|Komut  |Destekleniyor |
|---------|---------|
|$map   |Hayır|
|$let   |Evet|

### <a name="system-variables"></a>Sistem değişkenleri

|Komut  |Destekleniyor |
|---------|---------|
|$CURRENT dolar| Evet|
|$DESCEND dolar|     Evet|
|$KEEP dolar     |Evet|
|$PRUNE dolar    |   Evet|
|$REMOVE $   |Evet|
|$ROOT dolar     |Evet|

### <a name="literal-operator"></a>Gerçek işleç

|Komut  |Destekleniyor |
|---------|---------|
|$literal   |Evet|

### <a name="date-expressions"></a>Tarih ifadeleri

|Komut  |Destekleniyor |
|---------|---------|
|$dayOfYear |Evet    |
|$dayOfMonth|   Evet |
|$dayOfWeek |Evet    |
|$year  |Evet    |
|$month |Evet|   
|$week  |Evet    |
|$hour  |Evet    |
|$minute|   Evet|    
|$second    |Evet    |
|$millisecond|  Evet|    
|$dateToString  |Evet    |
|$isoDayOfWeek  |Evet    |
|$isoWeek   |Evet    |
|$dateFromParts|    Hayır| 
|$dateToParts   |Hayır |
|$dateFromString|   Hayır|
|$isoWeekYear   |Evet    |

### <a name="conditional-expressions"></a>Koşullu ifadeler

|Komut  |Destekleniyor |
|---------|---------|
| $cond| Evet|
| $ifNull| Evet|
| $switch |Evet|

### <a name="data-type-operator"></a>Veri türü işleci

|Komut  |Destekleniyor |
|---------|---------|
| $type| Evet|

### <a name="accumulator-expressions"></a>Akümülatör ifadeleri

|Komut  |Destekleniyor |
|---------|---------|
|$sum   |Evet    |
|$avg   |Evet    |
|$first|    Evet|
|$last  |Evet    |
|$max   |Evet    |
|$min   |Evet    |
|$push| Evet|
|$addToSet| Evet|
|$stdDevPop|    Hayır  |
|$stdDevSamp|   Hayır|

### <a name="merge-operator"></a>Birleştirme işleci

|Komut  |Destekleniyor |
|---------|---------|
| $mergeObjects | Evet|

## <a name="data-types"></a>Veri türleri

|Komut  |Destekleniyor |
|---------|---------|
|Çift |Evet    |
|Dize |Evet    |
|Nesne |Evet    |
|Dizi  |Evet    |
|İkili Veri    |Evet|   
|ObjectId   |Evet    |
|Boole    |Evet    |
|Tarih   |Evet    |
|Null   |Evet    |
|32-bit Tümseger (int)   |Evet    |
|Zaman damgası  |Evet    |
|64 bit Tümseger (uzun)  |Evet    |
|MinKey |Evet    |
|MaxKey |Evet    |
|Ondalık 128 |Evet|   
|Normal ifade |Evet|
|JavaScript |Evet|
|JavaScript (kapsam ile)|   Evet |
|Tanımsız  |Evet    |

## <a name="indexes-and-index-properties"></a>Dizinler ve dizin özellikleri

### <a name="indexes"></a>Dizinler

|Komut  |Destekleniyor |
|---------|---------|
|Tek Alan Dizini |Evet    |
|Bileşik Endeks |Evet    |
|Çok Anahtarlı Dizini |Evet    |
|Metin Dizini |Hayır|
|2dsphere   |Evet    |
|2b Dizin   |Hayır |
|Hashed Endeksi   | Evet|

### <a name="index-properties"></a>Dizin özellikleri

|Komut  |Destekleniyor |
|---------|---------|
|TTL|   Evet |
|Benzersiz |Evet|
|Kısmi|   Hayır|
|Büyük/Küçük Harf Duyarsız   |Hayır|
|Seyrek |Hayır |
|Arka plan|    Evet |

## <a name="operators"></a>İşleçler

### <a name="logical-operators"></a>Mantıksal işleçler

|Komut  |Destekleniyor |
|---------|---------|
|$or    |   Evet|
|$and   |   Evet|
|$not   |   Evet|
|$nor   |   Evet| 

### <a name="element-operators"></a>Eleman işleçleri

|Komut  |Destekleniyor |
|---------|---------|
|$exists|   Evet|
|$type  |   Evet|

### <a name="evaluation-query-operators"></a>Değerlendirme sorgu operatörleri

|Komut  |Destekleniyor |
|---------|---------|
|$expr  |   Hayır|
|$jsonSchema    |   Hayır|
|$mod   |   Evet|
|$regex |   Evet|
|$text  | Hayır (Desteklenmez. Bunun yerine $regex kullanın.)| 
|$where |Hayır| 

sorguların $regex, sola bağlı ifadeler dizin araması sağlar. Ancak 'i' değiştiricisini (büyük/küçük harf duyarlığı) ve 'm' değiştiricisini (çok satırlılık) kullanmak, tüm ifadelerde koleksiyon taramasına neden olur.

'$' veya '|' karakterinin dahil edilmesinin gerektiği durumlarda iki (veya daha fazla) normal ifade sorgusu oluşturmak en iyisidir. Örneğin, aşağıdaki özgün sorgu ```find({x:{$regex: /^abc$/})```göz önüne alındığında: , aşağıdaki gibi değiştirilmesi gerekir:

```find({x:{$regex: /^abc/, x:{$regex:/^abc$/}})```.

İlk kısım aramayı ^abc ile başlayan belgeler ile sınırlamak için dizini kullanır, ikinci kısım ise tam girişler ile eşleşir. Çubuk işleci '|' "veya" işlevini görür - ```find({x:{$regex: /^abc|^def/})``` sorgusu 'x' alanının değerlerinin "abc" ile veya "def" ile başladığı belgelerle eşleşir. Dizinden yararlanmak için sorgunun $or işleci ile birleştirilen iki farklı sorguya bölünmesi gerekir: ```find( {$or : [{x: $regex: /^abc/}, {$regex: /^def/}] })```.

### <a name="array-operators"></a>Dizi işleçleri

|Komut  |Destekleniyor | 
|---------|---------|
| $all | Evet| 
| $elemMatch | Evet| 
| $size | Evet | 

### <a name="comment-operator"></a>Yorum operatörü

|Komut  |Destekleniyor | 
|---------|---------|
$comment |Evet| 

### <a name="projection-operators"></a>Projeksiyon operatörleri

|Komut  |Destekleniyor |
|---------|---------|
|$elemMatch |Evet|
|$meta| Hayır|
|$slice | Evet|

### <a name="update-operators"></a>Güncelleştirme işleçleri

#### <a name="field-update-operators"></a>Alan güncelleştirme işleçleri

|Komut  |Destekleniyor |
|---------|---------|
|$inc   |   Evet|
|$mul   |   Evet|
|$rename    |   Evet|
|$setOnInsert|  Evet|
|$set   |Evet|
|$unset| Evet|
|$min   |Evet|
|$max   |Evet|
|$currentDate   | Evet|

#### <a name="array-update-operators"></a>Dizi güncelleştirme işleçleri

|Komut  |Destekleniyor |
|---------|---------|
|$  |Evet|
|$[]|   Evet|
|$[<identifier>]|   Evet|
|$addToSet  |Evet|
|$pop   |Evet|
|$pullAll|  Evet|
|$pull  |Evet|
|$push  |Evet|
|$pushAll| Evet|


#### <a name="update-modifiers"></a>Değiştiricileri güncelleştir

|Komut  |Destekleniyor |
|---------|---------|
|$each  |   Evet|
|$slice |Evet|
|$sort  |Evet|
|$position  |Evet|

#### <a name="bitwise-update-operator"></a>Bit düzeyinde güncelleştirme işleci

|Komut  |Destekleniyor |
|---------|---------|
| $bit  |   Evet|    
|$bitsAllSet    |   Hayır|
|$bitsAnySet    |   Hayır|
|$bitsAllClear  |Hayır|
|$bitsAnyClear  |Hayır|

### <a name="geospatial-operators"></a>Jeo-uzamsal işleçler

İşleç | Destekleniyor| 
--- | --- |
$geoWithin | Evet |
$geoIntersects | Evet | 
$near |  Evet |
$nearSphere |  Evet |
$geometry |  Evet |
$minDistance | Evet |
$maxDistance | Evet |
$center | Evet |
$centerSphere | Evet |
$box | Evet |
$polygon |  Evet |

## <a name="cursor-methods"></a>İmleç yöntemleri

|Komut  |Destekleniyor |
|---------|---------|
|imleç.batchSize() |   Evet|
|imleç.close() |Evet|
|imleç.isKapalı()|     Evet|
|imleç.collation()|    Hayır|
|imleç.comment()   |Evet|
|imleç.count() |Evet|
|imleç.explain()|  Hayır|
|imleç.forEach()   |Evet|
|imleç.hasNext()   |Evet|
|imleç.ipucu()  |Evet|
|imleç.isExhausted()|  Evet|
|imleç.itcount()   |Evet|
|imleç.limit() |Evet|
|imlesor.map()   |Evet|
|imleç.maxScan()   |Evet|
|imleç.maxTimeMS()|    Evet|
|imleç.max()   |Evet|
|imleç.min()   |Evet|
|imleç.next()| Evet|
|imleç.noCursorTimeout()   |Hayır|
|imleç.objsLeftInBatch()   |Evet|
|imleç.pretty()|   Evet|
|imleç.readConcern()|  Evet|
|imleç.readPref()      |Evet|
|imleç.returnKey() |Hayır|
|imleç.showRecordId()| Hayır|
|imleç.boyut()  |Nes|
|imleç.skip()  |Evet|
|cursor.sort()  |   Evet|
|imleç.tailable()| Hayır|
|imleç.toArray()   |Evet|

## <a name="sort-operations"></a>İşlemleri sıralama

`findOneAndUpdate` İşlem işlendirilirken, tek bir alandaki sınıf işleyişleri desteklenir, ancak birden fazla alandaki sıkılım işlemi desteklenmez.

## <a name="unique-indexes"></a>Benzersiz dizinler

Benzersiz dizinler, varsayılan "_id" anahtarında özgünlüğün korunmasına benzer bir şekilde bir koleksiyondaki tüm belgeler genelinde belirli bir alanda yinelenen değer olmamasını sağlar. 'Benzersiz' kısıtlama da dahil olmak üzere createIndex komutunu kullanarak Cosmos DB'de özel dizinler oluşturabilirsiniz.

## <a name="time-to-live-ttl"></a>Etkin kalma süresi (TTL)

Cosmos DB, belgenin zaman damgasına dayalı bir zaman-to-live (TTL) destekler. Azure [portalına](https://portal.azure.com)giderek koleksiyonlar için TTL etkinleştirilebilir.

## <a name="user-and-role-management"></a>Kullanıcı ve rol yönetimi

Cosmos DB henüz kullanıcıları ve rolleri desteklemez. Ancak Cosmos DB, [Azure portalı](https://portal.azure.com) (Connection String sayfası) üzerinden elde edilebilen rol tabanlı erişim denetimini (RBAC) ve salt okuma ve salt okunur parolaları/anahtarları destekler.

## <a name="replication"></a>Çoğaltma

Cosmos DB, en düşük katmanlarda otomatik, yerel çoğaltmayı destekler. Bu mantık, düşük gecikme süresi ve küresel çoğaltma elde etmek için genişletilir. Cosmos DB el ile çoğaltma komutlarını desteklemez.

## <a name="write-concern"></a>Yazma Sorunu

Bazı uygulamalar, yazma işlemi sırasında gereken yanıt sayısını belirten bir [Yazma Endişesi'ne](https://docs.mongodb.com/manual/reference/write-concern/) dayanır. Cosmos DB’nin arka planda çoğaltma işleme şekli nedeniyle varsayılan olarak tüm yazma işlemleri otomatik olarak çekirdektir. İstemci kodu tarafından belirtilen herhangi bir yazma endişesi yoksayılır. Daha fazla bilgi için bkz. [Kullanılabilirlik ve performansı en üst düzeye çıkarmak için tutarlılık düzeylerini kullanma](consistency-levels.md).

## <a name="sharding"></a>Parçalama

Azure Cosmos DB, otomatik, sunucu tarafı parçalamasını destekler. Parça oluşturmayı, yerleştirmeyi ve dengelemeyi otomatik olarak yönetir. Azure Cosmos DB el ile parçalama komutlarını desteklemez, bu da addShard, balancerStart, moveChunk gibi komutları çağırmak zorunda olmadığınız anlamına gelir. Kapsayıcıları oluştururken veya verileri sorgularken yalnızca parça anahtarını belirtmeniz gerekir.

## <a name="sessions"></a>Oturumlar

Azure Cosmos DB henüz sunucu tarafı oturumları komutlarını desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için [Mongo 3.6 sürüm özelliklerini](https://devblogs.microsoft.com/cosmosdb/azure-cosmos-dbs-api-for-mongodb-now-supports-server-version-3-6/) kontrol edin
- Azure Cosmos DB'nin MongoDB için API'si ile [Studio 3T'yi](mongodb-mongochef.md) nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB'nin API'si ile [Robo 3T'yi](mongodb-robomongo.md) nasıl kullanacağınızı öğrenin.
- Azure Cosmos DB'nin MongoDB için API'si ile MongoDB [örneklerini](mongodb-samples.md) keşfedin.

<sup>Not: Bu makalede, Azure Cosmos DB'nin MongoDB veritabanlarıyla tel protokolü uyumluluğu sağlayan bir özelliği açıklanmaktadır. Microsoft, bu hizmeti sağlamak için MongoDB veritabanlarını çalıştırmaz. Azure Cosmos DB, MongoDB, Inc. şirketine bağlı değildir.</sup>
