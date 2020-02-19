---
title: MongoDB (3,6 sürüm) için API Azure Cosmos DB desteklenen özellikler ve sözdizimi
description: MongoDB (3,6 sürümü) tarafından desteklenen özellikler ve söz dizimi için Azure Cosmos DB API 'SI hakkında bilgi edinin.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: overview
ms.date: 01/15/2020
author: sivethe
ms.author: sivethe
ms.openlocfilehash: cde731f9d9e673446bc4d08117004b028db2a7f9
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77462471"
---
# <a name="azure-cosmos-dbs-api-for-mongodb-36-version-supported-features-and-syntax"></a>MongoDB için API Azure Cosmos DB (3,6 sürümü): desteklenen özellikler ve sözdizimi

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Açık kaynaklı MongoDB istemci [sürücülerinden](https://docs.mongodb.org/ecosystem/drivers)herhangi birini kullanarak mongodb için Azure Cosmos DB API 'si ile iletişim kurabilirsiniz. Azure Cosmos DB MongoDB API 'SI, mevcut istemci sürücülerinin MongoDB [kablo protokolüne](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol)bağlanarak kullanımını sağlar.

MongoDB için Azure Cosmos DB API 'sini kullanarak, kullandığınız MongoDB avantajlarından yararlanarak, Cosmos DB sağladığı tüm kurumsal yetenekler sayesinde, [genel dağıtım](distribute-data-globally.md), [Otomatik](partition-data.md)parçalar, kullanılabilirlik ve gecikme garantisi, Rest, yedeklemeler ve çok daha fazlası için de kullanabilirsiniz.

## <a name="protocol-support"></a>Protokol desteği

Azure Cosmos DB MongoDB için API 'SI, yeni hesaplar için varsayılan olarak MongoDB sunucu sürümü **3,6** ile uyumludur. Desteklenen işleçler ve tüm sınırlamalar veya özel durumlar aşağıda listelenmiştir. Bu protokolleri anlayan tüm istemci sürücüleri MongoDB için Azure Cosmos DB’nin API’sine bağlanabilmesi gerekir. MongoDB hesapları için Azure Cosmos DB API 'SI kullanılırken, hesapların 3,6 sürümünün uç nokta `*.mongo.cosmos.azure.com` biçimde olması durumunda, hesapların 3,2 sürümünün uç noktası `*.documents.azure.com`biçiminde olması gerektiğini unutmayın.

## <a name="query-language-support"></a>Sorgu dili desteği

MongoDB için Azure Cosmos DB API 'SI, MongoDB sorgu dili yapıları için kapsamlı destek sağlar. Aşağıda, şu anda desteklenen işlemler, işleçler, aşamalar, komutlar ve seçeneklerin ayrıntılı listesini bulabilirsiniz.

## <a name="database-commands"></a>Veritabanı komutları

MongoDB için Azure Cosmos DB API 'SI aşağıdaki veritabanı komutlarını destekler:

### <a name="query-and-write-operation-commands"></a>Sorgulama ve yazma işlemi komutları

|Komut  |Destekleniyor |
|---------|---------|
|delete | Yes |
|find | Yes     |
|findAndModify | Yes  |
|getLastError|   Yes |
|getMore  |  Yes  |
|getPrevError | Hayır  |
|insert  |   Yes  |
|parallelCollectionScan  | Yes   |
|resetError |   Hayır  |
|update  |   Yes  |
|[Akışları Değiştir](mongodb-change-streams.md)  |  Yes  |
|GridFS |   Yes  |

### <a name="authentication-commands"></a>Kimlik doğrulama komutları

|Komut  |Destekleniyor |
|---------|---------|
|authenticate    |   Yes      |
|logout    |      Yes   |
|getnonce   |    Yes     |


### <a name="administration-commands"></a>Yönetim komutları

|Komut  |Destekleniyor |
|---------|---------|
|Caklanan Koleksiyonlar   |   Hayır      |
|Clonecollectionascamış     |   Hayır      |
|collMod     |   Hayır      |
|collMod: expireAfterSeconds   |   Hayır      |
|Converttocamış   |  Hayır       |
|copydb     |  Hayır       |
|oluşturmaya   |    Yes     |
|createIndexes     |  Yes       |
|currentOp     |  Yes       |
|drop     |   Yes      |
|dropDatabase     |  Yes       |
|dropIndexes     |   Yes      |
|filemd5    |   Yes      |
|killCursors    |  Yes       |
|killOp     |   Hayır      |
|listCollections     |  Yes       |
|listDatabases     |  Yes       |
|listIndexes     |  Yes       |
|reIndex     |    Yes     |
|renameCollection     |    Hayır     |
|connectionStatus    |     Hayır    |

### <a name="diagnostics-commands"></a>Tanılama komutları

|Komut  |Destekleniyor |
|---------|---------|
|buildInfo       |   Yes      |
|collStats    |  Yes       |
|connPoolStats     |  Hayır       |
|connectionStatus     |  Hayır       |
|dataSize     |   Hayır      |
|dbHash    |    Hayır     |
|dbStats     |   Yes      |
|açıklamak     | Hayır        |
|açıkla: executionStats     |     Hayır    |
|özellikler     |    Hayır     |
|hostInfo     |   Hayır      |
|listDatabases       |   Yes      |
|listCommands     |  Hayır       |
|profil oluşturucu     |  Hayır       |
|serverStatus     |  Hayır       |
|Sayfanın Üstü     |    Hayır     |
|whatsmyuri     |   Yes      |

<a name="aggregation-pipeline"/>

## <a name="aggregation-pipelinea"></a>Toplama ardışık düzeni</a>

### <a name="aggregation-commands"></a>Toplama komutları

|Komut  |Destekleniyor |
|---------|---------|
|aggregate |   Yes  |
|count     |   Yes  |
|distinct  | Yes |
|mapReduce | Hayır |

### <a name="aggregation-stages"></a>Toplama aşamaları

|Komut  |Destekleniyor |
|---------|---------|
|$collStats |Hayır|
|$project   |Yes|
|$match |Yes|
|$redact|   Yes|
|$limit |Yes|
|$skip  |Yes|
|$unwind|   Yes|
|$group |   Yes|
|$sample|       Yes|
|$sort  |Yes|
|$geoNear|  Hayır|
|$lookup    |   Yes|
|$out       |Yes|
|$indexStats|       Hayır|
|$facet |Hayır|
|$bucket|   Hayır|
|$bucketAuto|   Hayır|
|$sortByCount|  Yes|
|$addFields |Yes|
|$replaceRoot|  Yes|
|$count |Yes|
|$currentOp|    Hayır|
|$listLocalSessions |Hayır|
|$listSessions  |Hayır|
|$graphLookup   |Hayır|

### <a name="boolean-expressions"></a>Mantıksal ifadeler

|Komut  |Destekleniyor |
|---------|---------|
|$and| Yes|
|$or|Yes|
|$not|Yes|

### <a name="set-expressions"></a>Küme ifadeleri

|Komut  |Destekleniyor |
|---------|---------|
| $setEquals | Yes|
|$setIntersection|Yes|
| $setUnion|Yes|
| $setDifference|Yes|
| $setIsSubset|Yes|
| $anyElementTrue|Yes|
| $allElementsTrue|Yes|

### <a name="comparison-expressions"></a>Karşılaştırma ifadeleri

|Komut  |Destekleniyor |
|---------|---------|
|$cmp     |  Yes       |
|$eq|   Yes| 
|$gt |  Yes| 
|$gte|  Yes| 
|$lt    |Yes|
|$lte|  Yes| 
|$ne    |   Yes| 
|$in    |   Yes| 
|$nin   |   Yes| 

### <a name="arithmetic-expressions"></a>Aritmetik ifadeler

|Komut  |Destekleniyor |
|---------|---------|
|$abs |  Yes       |
| $add |  Yes       |
| $ceil |  Yes       |
| $divide |  Yes       |
| $exp |  Yes       |
| $floor |  Yes       |
| $ln |  Yes       |
| $log |  Yes       |
| $log10 |  Yes       |
| $mod |  Yes       |
| $multiply |  Yes       |
| $pow |  Yes       |
| $sqrt |  Yes       |
| $subtract |  Yes       |
| $trunc |  Yes       |

### <a name="string-expressions"></a>Dize ifadeleri

|Komut  |Destekleniyor |
|---------|---------|
|$concat |  Yes       |
| $indexOfBytes|  Yes       |
| $indexOfCP|  Yes       |
| $split|  Yes       |
| $strLenBytes|  Yes       |
| $strLenCP|  Yes       |
| $strcasecmp|  Yes       |
| $substr|  Yes       |
| $substrBytes|  Yes       |
| $substrCP|  Yes       |
| $toLower|  Yes       |
| $toUpper|  Yes       |

### <a name="text-search-operator"></a>Metin arama işleci

|Komut  |Destekleniyor |
|---------|---------|
| $meta | Hayır|

### <a name="array-expressions"></a>Dizi ifadeleri

|Komut  |Destekleniyor |
|---------|---------|
|$arrayElemAt   |   Yes|
|$arrayToObject|    Yes|
|$concatArrays  |   Yes|
|$filter    |   Yes|
|$indexOfArray  |Yes|
|$isArray   |   Yes|
|$objectToArray |Yes|
|$range |Yes|
|$reverseArray  |   Yes|
|$reduce|   Yes|
|$size  |   Yes|
|$slice |   Yes|
|$zip   |   Yes|
|$in    |   Yes|

### <a name="variable-operators"></a>Değişken işleçleri

|Komut  |Destekleniyor |
|---------|---------|
|$map   |Hayır|
|$let   |Yes|

### <a name="system-variables"></a>Sistem değişkenleri

|Komut  |Destekleniyor |
|---------|---------|
|$ $CURRENT| Yes|
|$ $DESCEND|     Yes|
|$ $KEEP     |Yes|
|$ $PRUNE    |   Yes|
|$ $REMOVE   |Yes|
|$ $ROOT     |Yes|

### <a name="literal-operator"></a>Sabit değer operatörü

|Komut  |Destekleniyor |
|---------|---------|
|$literal   |Yes|

### <a name="date-expressions"></a>Tarih ifadeleri

|Komut  |Destekleniyor |
|---------|---------|
|$dayOfYear |Yes    |
|$dayOfMonth|   Yes |
|$dayOfWeek |Yes    |
|$year  |Yes    |
|$month |Yes|   
|$week  |Yes    |
|$hour  |Yes    |
|$minute|   Yes|    
|$second    |Yes    |
|$millisecond|  Yes|    
|$dateToString  |Yes    |
|$isoDayOfWeek  |Yes    |
|$isoWeek   |Yes    |
|$dateFromParts|    Hayır| 
|$dateToParts   |Hayır |
|$dateFromString|   Hayır|
|$isoWeekYear   |Yes    |

### <a name="conditional-expressions"></a>Koşullu ifadeler

|Komut  |Destekleniyor |
|---------|---------|
| $cond| Yes|
| $ifNull| Yes|
| $switch |Yes|

### <a name="data-type-operator"></a>Veri türü işleci

|Komut  |Destekleniyor |
|---------|---------|
| $type| Yes|

### <a name="accumulator-expressions"></a>Biriktiricidir ifadeleri

|Komut  |Destekleniyor |
|---------|---------|
|$sum   |Yes    |
|$avg   |Yes    |
|$first|    Yes|
|$last  |Yes    |
|$max   |Yes    |
|$min   |Yes    |
|$push| Yes|
|$addToSet| Yes|
|$stdDevPop|    Hayır  |
|$stdDevSamp|   Hayır|

### <a name="merge-operator"></a>Merge işleci

|Komut  |Destekleniyor |
|---------|---------|
| $mergeObjects | Yes|

## <a name="data-types"></a>Veri türleri

|Komut  |Destekleniyor |
|---------|---------|
|çift |Yes    |
|Dize |Yes    |
|Nesne |Yes    |
|Dizi  |Yes    |
|Binary Data    |Yes|   
|ObjectId   |Yes    |
|Boole    |Yes    |
|Tarih   |Yes    |
|Null   |Yes    |
|32-bit tamsayı (int)   |Yes    |
|Zaman damgası  |Yes    |
|64 bit tamsayı (uzun)  |Yes    |
|MinKey |Yes    |
|MaxKey |Yes    |
|Decimal128 |Yes|   
|Normal Ifade |Yes|
|JavaScript |Yes|
|JavaScript (kapsama sahip)|   Yes |
|Tanımlanmamış  |Yes    |

## <a name="indexes-and-index-properties"></a>Dizinler ve Dizin Özellikleri

### <a name="indexes"></a>Dizinler

|Komut  |Destekleniyor |
|---------|---------|
|Tek alan dizini |Yes    |
|Bileşik Dizin |Yes    |
|Çok tuşlu Dizin |Yes    |
|Metin dizini |Hayır|
|2dsphere   |Yes    |
|2B Dizin   |Hayır |
|Karma Dizin   | Yes|

### <a name="index-properties"></a>Dizin Özellikleri

|Komut  |Destekleniyor |
|---------|---------|
|TTL|   Yes |
|Benzersiz |Yes|
|Kısmi|   Hayır|
|Büyük/küçük harf duyarsız   |Hayır|
|Seyrek |Hayır |
|Arka plan|    Yes |

## <a name="operators"></a>İşleçler

### <a name="logical-operators"></a>Mantıksal işleçler

|Komut  |Destekleniyor |
|---------|---------|
|$or    |   Yes|
|$and   |   Yes|
|$not   |   Yes|
|$nor   |   Yes| 

### <a name="element-operators"></a>Öğe işleçleri

|Komut  |Destekleniyor |
|---------|---------|
|$exists|   Yes|
|$type  |   Yes|

### <a name="evaluation-query-operators"></a>Değerlendirme sorgu işleçleri

|Komut  |Destekleniyor |
|---------|---------|
|$expr  |   Hayır|
|$jsonSchema    |   Hayır|
|$mod   |   Yes|
|$regex |   Yes|
|$text  | Hayır (desteklenmiyor. Bunun yerine $regex kullanın.)| 
|$where |Hayır| 

$Regex sorgularda, sola sabitlenmiş ifadeler Dizin aramasına izin verir. Ancak 'i' değiştiricisini (büyük/küçük harf duyarlığı) ve 'm' değiştiricisini (çok satırlılık) kullanmak, tüm ifadelerde koleksiyon taramasına neden olur.

'$' veya '|' karakterinin dahil edilmesinin gerektiği durumlarda iki (veya daha fazla) normal ifade sorgusu oluşturmak en iyisidir. Örneğin, şu özgün sorgu verildiğinde: ```find({x:{$regex: /^abc$/})```, aşağıdaki şekilde değiştirilmelidir:

```find({x:{$regex: /^abc/, x:{$regex:/^abc$/}})```.

İlk kısım aramayı ^abc ile başlayan belgeler ile sınırlamak için dizini kullanır, ikinci kısım ise tam girişler ile eşleşir. Çubuk işleci '|' "veya" işlevini görür - ```find({x:{$regex: /^abc|^def/})``` sorgusu 'x' alanının değerlerinin "abc" ile veya "def" ile başladığı belgelerle eşleşir. Dizinden yararlanmak için sorgunun $or işleci ile birleştirilen iki farklı sorguya bölünmesi gerekir: ```find( {$or : [{x: $regex: /^abc/}, {$regex: /^def/}] })```.

### <a name="array-operators"></a>Dizi işleçleri

|Komut  |Destekleniyor | 
|---------|---------|
| $all | Yes| 
| $elemMatch | Yes| 
| $size | Yes | 

### <a name="comment-operator"></a>Açıklama işleci

|Komut  |Destekleniyor | 
|---------|---------|
$comment |Yes| 

### <a name="projection-operators"></a>Projeksiyon işleçleri

|Komut  |Destekleniyor |
|---------|---------|
|$elemMatch |Yes|
|$meta| Hayır|
|$slice | Yes|

### <a name="update-operators"></a>Güncelleştirme işleçleri

#### <a name="field-update-operators"></a>Alan güncelleştirme işleçleri

|Komut  |Destekleniyor |
|---------|---------|
|$inc   |   Yes|
|$mul   |   Yes|
|$rename    |   Yes|
|$setOnInsert|  Yes|
|$set   |Yes|
|$unset| Yes|
|$min   |Yes|
|$max   |Yes|
|$currentDate   | Yes|

#### <a name="array-update-operators"></a>Dizi güncelleştirme işleçleri

|Komut  |Destekleniyor |
|---------|---------|
|$  |Yes|
|$[]|   Yes|
|$ [<identifier>]|   Yes|
|$addToSet  |Yes|
|$pop   |Yes|
|$pullAll|  Yes|
|$pull  |Yes|
|$push  |Yes|
|$pushAll| Yes|


#### <a name="update-modifiers"></a>Güncelleştirme değiştiricileri

|Komut  |Destekleniyor |
|---------|---------|
|$each  |   Yes|
|$slice |Yes|
|$sort  |Yes|
|$position  |Yes|

#### <a name="bitwise-update-operator"></a>Bit düzeyinde güncelleştirme işleci

|Komut  |Destekleniyor |
|---------|---------|
| $bit  |   Yes|    
|$bitsAllSet    |   Hayır|
|$bitsAnySet    |   Hayır|
|$bitsAllClear  |Hayır|
|$bitsAnyClear  |Hayır|

### <a name="geospatial-operators"></a>Jeo-uzamsal işleçler

İşleç | Destekleniyor| 
--- | --- |
$geoWithin | Yes |
$geoIntersects | Yes | 
$near |  Yes |
$nearSphere |  Yes |
$geometry |  Yes |
$minDistance | Yes |
$maxDistance | Yes |
$center | Yes |
$centerSphere | Yes |
$box | Yes |
$polygon |  Yes |

## <a name="cursor-methods"></a>İmleç yöntemleri

|Komut  |Destekleniyor |
|---------|---------|
|Cursor. batchSize () |   Yes|
|Cursor. Close () |Yes|
|Cursor. IsClosed ()|     Yes|
|Cursor. harmanlama ()|    Hayır|
|Cursor. Comment ()   |Yes|
|Cursor. Count () |Yes|
|Cursor. açıkla ()|  Hayır|
|Cursor. forEach ()   |Yes|
|Cursor. hasNext ()   |Yes|
|Cursor. İpucu ()  |Yes|
|Cursor. isExhausted ()|  Yes|
|Cursor. itcount ()   |Yes|
|Cursor. limit () |Yes|
|Cursor. map ()   |Yes|
|Cursor. maxScan ()   |Yes|
|imleç. maxTimeMS ()|    Yes|
|Cursor. Max ()   |Yes|
|Cursor. min ()   |Yes|
|Cursor. Next ()| Yes|
|Cursor. noCursorTimeout ()   |Hayır|
|Cursor. Objsleftınbatch ()   |Yes|
|imleç. oldukça ()|   Yes|
|imleç. Readsorun ()|  Yes|
|Cursor. readPref ()      |Yes|
|Cursor. returnKey () |Hayır|
|Cursor. Showrecordıd ()| Hayır|
|Cursor. size ()  |NES|
|Cursor. Skip ()  |Yes|
|cursor.sort()  |   Yes|
|imleç. tailable ()| Hayır|
|Cursor. toArray ()   |Yes|

## <a name="sort-operations"></a>Sıralama işlemleri

`findOneAndUpdate` işlemi kullanılırken, tek bir alanda sıralama işlemleri desteklenir, ancak birden çok alanda sıralama işlemleri desteklenmez.

## <a name="unique-indexes"></a>Benzersiz dizinler

Benzersiz dizinler, varsayılan "_id" anahtarında özgünlüğün korunmasına benzer bir şekilde bir koleksiyondaki tüm belgeler genelinde belirli bir alanda yinelenen değer olmamasını sağlar. ' Unique ' kısıtlaması dahil CreateIndex komutunu kullanarak Cosmos DB özel dizinler oluşturabilirsiniz.

## <a name="time-to-live-ttl"></a>Etkin kalma süresi (TTL)

Cosmos DB, belgenin zaman damgasına göre yaşam süresi (TTL) desteği sağlar. TTL, [Azure Portal](https://portal.azure.com)giderek koleksiyonlar için etkinleştirilebilir.

## <a name="user-and-role-management"></a>Kullanıcı ve rol yönetimi

Cosmos DB henüz kullanıcıları ve rolleri desteklemez. Ancak Cosmos DB, rol tabanlı erişim denetimi (RBAC) ve [Azure Portal](https://portal.azure.com) (bağlantı dizesi sayfası) aracılığıyla elde edilebilir salt okuma ve salt yazılır parolaları/anahtarları destekler.

## <a name="replication"></a>Çoğaltma

Cosmos DB, en düşük katmanlarda otomatik, yerel çoğaltmayı destekler. Bu mantık, düşük gecikme süresi ve küresel çoğaltma elde etmek için genişletilir. Cosmos DB el ile çoğaltma komutlarını desteklemez.

## <a name="write-concern"></a>Yazma Sorunu

Bazı uygulamalar, yazma işlemi sırasında gereken yanıt sayısını belirten bir [yazma kaygısını](https://docs.mongodb.com/manual/reference/write-concern/) kullanır. Cosmos DB’nin arka planda çoğaltma işleme şekli nedeniyle varsayılan olarak tüm yazma işlemleri otomatik olarak çekirdektir. İstemci kodu tarafından belirtilen herhangi bir yazma sorunu yok sayılır. Daha fazla bilgi için bkz. [Kullanılabilirlik ve performansı en üst düzeye çıkarmak için tutarlılık düzeylerini kullanma](consistency-levels.md).

## <a name="sharding"></a>Parçalama

Azure Cosmos DB, otomatik, sunucu tarafı parçalamasını destekler. Parça oluşturma, yerleştirme ve dengelemeyi otomatik olarak yönetir. Azure Cosmos DB, el ile parçalı komutları desteklemez, bu, addShard, balancerStart, moveChunk gibi komutları çağırmak zorunda olmadığınız anlamına gelir. Yalnızca kapsayıcıları oluştururken veya verileri sorgularken parça anahtarını belirtmeniz gerekir.

## <a name="sessions"></a>Oturumlar

Azure Cosmos DB, sunucu tarafı oturumları komutlarını henüz desteklemiyor.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için [Mongo 3,6 sürüm özelliklerini](https://devblogs.microsoft.com/cosmosdb/azure-cosmos-dbs-api-for-mongodb-now-supports-server-version-3-6/) inceleyin
- [Studio 3T](mongodb-mongochef.md) 'ı Azure Cosmos DB MongoDB IÇIN API 'si ile nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB API 'SI ile [Robo 3T kullanmayı](mongodb-robomongo.md) öğrenin.
- MongoDB için Azure Cosmos DB API 'siyle MongoDB [örneklerini](mongodb-samples.md) gezin.

<sup>Note: Bu makalede, MongoDB veritabanlarıyla kablo protokol uyumluluğu sağlayan Azure Cosmos DB bir özelliği açıklanmaktadır. Microsoft bu hizmeti sağlamak için MongoDB veritabanlarını çalıştırmaz. Azure Cosmos DB MongoDB, Inc ile bağlantılı değildir.</sup>
