---
title: MongoDB (3,6 sürüm) için API Azure Cosmos DB desteklenen özellikler ve sözdizimi
description: MongoDB (3,6 sürümü) tarafından desteklenen özellikler ve söz dizimi için Azure Cosmos DB API 'SI hakkında bilgi edinin.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: overview
ms.date: 01/15/2020
author: sivethe
ms.author: sivethe
ms.openlocfilehash: a32affab45ab99a89113644bb08c4f2b57d69018
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721022"
---
# <a name="azure-cosmos-dbs-api-for-mongodb-36-version-supported-features-and-syntax"></a>MongoDB için API Azure Cosmos DB (3,6 sürümü): desteklenen özellikler ve sözdizimi

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Açık kaynaklı MongoDB istemci [sürücülerinden](https://docs.mongodb.org/ecosystem/drivers)herhangi birini kullanarak mongodb için Azure Cosmos DB API 'si ile iletişim kurabilirsiniz. Azure Cosmos DB MongoDB API 'SI, mevcut istemci sürücülerinin MongoDB [kablo protokolüne](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol)bağlanarak kullanımını sağlar.

MongoDB için Azure Cosmos DB API 'sini kullanarak, kullandığınız MongoDB 'nin avantajlarından yararlanarak, Cosmos DB sağladığı tüm kurumsal yetenekler şunlardır: [küresel dağıtım](distribute-data-globally.md), [Otomatik](partition-data.md)parçalama, kullanılabilirlik ve gecikme garantisi, her alanın otomatik dizin oluşturma, Rest, yedeklemeler ve çok daha fazlası.

## <a name="protocol-support"></a>Protokol desteği

Azure Cosmos DB MongoDB için API 'SI, yeni hesaplar için varsayılan olarak MongoDB sunucu sürümü **3,6** ile uyumludur. Desteklenen işleçler ve tüm sınırlamalar veya özel durumlar aşağıda listelenmiştir. Bu protokolleri anlayan tüm istemci sürücüleri MongoDB için Azure Cosmos DB’nin API’sine bağlanabilmesi gerekir. MongoDB hesapları için Azure Cosmos DB API 'SI kullanılırken, hesapların 3,6 sürümünün uç nokta `*.mongo.cosmos.azure.com` biçimde olması durumunda, hesapların 3,2 sürümünün uç noktası `*.documents.azure.com`biçiminde olması gerektiğini unutmayın.

## <a name="query-language-support"></a>Sorgu dili desteği

MongoDB için Azure Cosmos DB API 'SI, MongoDB sorgu dili yapıları için kapsamlı destek sağlar. Aşağıda, şu anda desteklenen işlemler, işleçler, aşamalar, komutlar ve seçeneklerin ayrıntılı listesini bulabilirsiniz.

## <a name="database-commands"></a>Veritabanı komutları

MongoDB için Azure Cosmos DB API 'SI aşağıdaki veritabanı komutlarını destekler:

### <a name="query-and-write-operation-commands"></a>Sorgulama ve yazma işlemi komutları

|Komut  |Desteklenen |
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
|[Akışları Değiştir](mongodb-change-streams.md)  |  Evet  |
|GridFS |   Evet  |

### <a name="authentication-commands"></a>Kimlik doğrulama komutları

|Komut  |Desteklenen |
|---------|---------|
|authenticate    |   Evet      |
|oturumu kapat    |      Evet   |
|getnonce   |    Evet     |


### <a name="administration-commands"></a>Yönetim komutları

|Komut  |Desteklenen |
|---------|---------|
|Caklanan Koleksiyonlar   |   Hayır      |
|Clonecollectionascamış     |   Hayır      |
|collMod     |   Hayır      |
|collMod: expireAfterSeconds   |   Hayır      |
|Converttocamış   |  Hayır       |
|copydb     |  Hayır       |
|oluşturmaya   |    Evet     |
|createIndexes     |  Evet       |
|currentOp     |  Evet       |
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
|renameCollection     |    Hayır     |
|connectionStatus    |     Hayır    |

### <a name="diagnostics-commands"></a>Tanılama komutları

|Komut  |Desteklenen |
|---------|---------|
|buildInfo       |   Evet      |
|collStats    |  Evet       |
|connPoolStats     |  Hayır       |
|connectionStatus     |  Hayır       |
|dataSize     |   Hayır      |
|dbHash    |    Hayır     |
|dbStats     |   Evet      |
|açıklamak     | Hayır        |
|açıkla: executionStats     |     Hayır    |
|SaaS Uygulamaları Geliştirme     |    Hayır     |
|hostInfo     |   Hayır      |
|listDatabases       |   Evet      |
|listCommands     |  Hayır       |
|profil     |  Hayır       |
|serverStatus     |  Hayır       |
|Sayfanın Üstü     |    Hayır     |
|whatsmyuri     |   Evet      |

<a name="aggregation-pipeline"/>

## <a name="aggregation-pipelinea"></a>Toplama ardışık düzeni</a>

### <a name="aggregation-commands"></a>Toplama komutları

|Komut  |Desteklenen |
|---------|---------|
|aggregate |   Evet  |
|count     |   Evet  |
|distinct  | Evet |
|mapReduce | Hayır |

### <a name="aggregation-stages"></a>Toplama aşamaları

|Komut  |Desteklenen |
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

|Komut  |Desteklenen |
|---------|---------|
|$and| Evet|
|$or|Evet|
|$not|Evet|

### <a name="set-expressions"></a>Küme ifadeleri

|Komut  |Desteklenen |
|---------|---------|
| $setEquals | Evet|
|$setIntersection|Evet|
| $setUnion|Evet|
| $setDifference|Evet|
| $setIsSubset|Evet|
| $anyElementTrue|Evet|
| $allElementsTrue|Evet|

### <a name="comparison-expressions"></a>Karşılaştırma ifadeleri

|Komut  |Desteklenen |
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

|Komut  |Desteklenen |
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

|Komut  |Desteklenen |
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

### <a name="text-search-operator"></a>Metin arama işleci

|Komut  |Desteklenen |
|---------|---------|
| $meta | Hayır|

### <a name="array-expressions"></a>Dizi ifadeleri

|Komut  |Desteklenen |
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

### <a name="variable-operators"></a>Değişken işleçleri

|Komut  |Desteklenen |
|---------|---------|
|$map   |Hayır|
|$let   |Evet|

### <a name="system-variables"></a>Sistem değişkenleri

|Komut  |Desteklenen |
|---------|---------|
|$ $CURRENT| Evet|
|$ $DESCEND|     Evet|
|$ $KEEP     |Evet|
|$ $PRUNE    |   Evet|
|$ $REMOVE   |Evet|
|$ $ROOT     |Evet|

### <a name="literal-operator"></a>Sabit değer operatörü

|Komut  |Desteklenen |
|---------|---------|
|$literal   |Evet|

### <a name="date-expressions"></a>Tarih ifadeleri

|Komut  |Desteklenen |
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

|Komut  |Desteklenen |
|---------|---------|
| $cond| Evet|
| $ifNull| Evet|
| $switch |Evet|

### <a name="data-type-operator"></a>Veri türü işleci

|Komut  |Desteklenen |
|---------|---------|
| $type| Evet|

### <a name="accumulator-expressions"></a>Biriktiricidir ifadeleri

|Komut  |Desteklenen |
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

### <a name="merge-operator"></a>Merge işleci

|Komut  |Desteklenen |
|---------|---------|
| $mergeObjects | Evet|

## <a name="data-types"></a>Veri türleri

|Komut  |Desteklenen |
|---------|---------|
|Double |Evet    |
|Dize |Evet    |
|Nesne |Evet    |
|Dizi  |Evet    |
|Binary Data    |Evet|   
|ObjectId   |Evet    |
|Boole    |Evet    |
|Tarih   |Evet    |
|Null   |Evet    |
|32-bit tamsayı (int)   |Evet    |
|Zaman damgası  |Evet    |
|64 bit tamsayı (uzun)  |Evet    |
|MinKey |Evet    |
|MaxKey |Evet    |
|Decimal128 |Evet|   
|Normal Ifade |Evet|
|JavaScript |Evet|
|JavaScript (kapsama sahip)|   Evet |
|Undefined  |Evet    |

## <a name="indexes-and-index-properties"></a>Dizinler ve Dizin Özellikleri

### <a name="indexes"></a>Dizinler

|Komut  |Desteklenen |
|---------|---------|
|Tek alan dizini |Evet    |
|Bileşik Dizin |Evet    |
|Çok tuşlu Dizin |Evet    |
|Metin dizini |Hayır|
|2dsphere   |Evet    |
|2B Dizin   |Hayır |
|Karma Dizin   | Evet|

### <a name="index-properties"></a>Dizin Özellikleri

|Komut  |Desteklenen |
|---------|---------|
|TTL|   Evet |
|Benzersiz |Evet|
|Kısmi|   Hayır|
|Büyük/küçük harf duyarsız   |Hayır|
|Seyrek |Hayır |
|Arka plan|    Evet |

## <a name="operators"></a>İşleçler

### <a name="logical-operators"></a>Mantıksal işleçler

|Komut  |Desteklenen |
|---------|---------|
|$or    |   Evet|
|$and   |   Evet|
|$not   |   Evet|
|$nor   |   Evet| 

### <a name="element-operators"></a>Öğe işleçleri

|Komut  |Desteklenen |
|---------|---------|
|$exists|   Evet|
|$type  |   Evet|

### <a name="evaluation-query-operators"></a>Değerlendirme sorgu işleçleri

|Komut  |Desteklenen |
|---------|---------|
|$expr  |   Hayır|
|$jsonSchema    |   Hayır|
|$mod   |   Evet|
|$regex |   Evet|
|$text  | Hayır (desteklenmiyor. Bunun yerine $regex kullanın.)| 
|$where |Hayır| 

$Regex sorgularda, sola sabitlenmiş ifadeler Dizin aramasına izin verir. Ancak 'i' değiştiricisini (büyük/küçük harf duyarlığı) ve 'm' değiştiricisini (çok satırlılık) kullanmak, tüm ifadelerde koleksiyon taramasına neden olur.

'$' veya '|' karakterinin dahil edilmesinin gerektiği durumlarda iki (veya daha fazla) normal ifade sorgusu oluşturmak en iyisidir. Örneğin, şu özgün sorgu verildiğinde: ```find({x:{$regex: /^abc$/})```, aşağıdaki şekilde değiştirilmelidir:

```find({x:{$regex: /^abc/, x:{$regex:/^abc$/}})```.

İlk kısım aramayı ^abc ile başlayan belgeler ile sınırlamak için dizini kullanır, ikinci kısım ise tam girişler ile eşleşir. Çubuk işleci '|' "veya" işlevini görür - ```find({x:{$regex: /^abc|^def/})``` sorgusu 'x' alanının değerlerinin "abc" ile veya "def" ile başladığı belgelerle eşleşir. Dizinden yararlanmak için sorgunun $or işleci ile birleştirilen iki farklı sorguya bölünmesi gerekir: ```find( {$or : [{x: $regex: /^abc/}, {$regex: /^def/}] })```.

### <a name="array-operators"></a>Dizi işleçleri

|Komut  |Desteklenen | 
|---------|---------|
| $all | Evet| 
| $elemMatch | Evet| 
| $size | Evet | 

### <a name="comment-operator"></a>Açıklama işleci

|Komut  |Desteklenen | 
|---------|---------|
$comment |Evet| 

### <a name="projection-operators"></a>Projeksiyon işleçleri

|Komut  |Desteklenen |
|---------|---------|
|$elemMatch |Evet|
|$meta| Hayır|
|$slice | Evet|

### <a name="update-operators"></a>Güncelleştirme işleçleri

#### <a name="field-update-operators"></a>Alan güncelleştirme işleçleri

|Komut  |Desteklenen |
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

|Komut  |Desteklenen |
|---------|---------|
|$  |Evet|
|$[]|   Evet|
|$ [<identifier>]|   Evet|
|$addToSet  |Evet|
|$pop   |Evet|
|$pullAll|  Evet|
|$pull  |Evet|
|$push  |Evet|
|$pushAll| Evet|


#### <a name="update-modifiers"></a>Güncelleştirme değiştiricileri

|Komut  |Desteklenen |
|---------|---------|
|$each  |   Evet|
|$slice |Evet|
|$sort  |Evet|
|$position  |Evet|

#### <a name="bitwise-update-operator"></a>Bit düzeyinde güncelleştirme işleci

|Komut  |Desteklenen |
|---------|---------|
| $bit  |   Evet|    
|$bitsAllSet    |   Hayır|
|$bitsAnySet    |   Hayır|
|$bitsAllClear  |Hayır|
|$bitsAnyClear  |Hayır|

### <a name="geospatial-operators"></a>Jeo-uzamsal işleçler

İşleç | Desteklenen| 
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

|Komut  |Desteklenen |
|---------|---------|
|Cursor. batchSize () |   Evet|
|Cursor. Close () |Evet|
|Cursor. IsClosed ()|     Evet|
|Cursor. harmanlama ()|    Hayır|
|Cursor. Comment ()   |Evet|
|Cursor. Count () |Evet|
|Cursor. açıkla ()|  Hayır|
|Cursor. forEach ()   |Evet|
|Cursor. hasNext ()   |Evet|
|Cursor. İpucu ()  |Evet|
|Cursor. isExhausted ()|  Evet|
|Cursor. itcount ()   |Evet|
|Cursor. limit () |Evet|
|Cursor. map ()   |Evet|
|Cursor. maxScan ()   |Evet|
|imleç. maxTimeMS ()|    Evet|
|Cursor. Max ()   |Evet|
|Cursor. min ()   |Evet|
|Cursor. Next ()| Evet|
|Cursor. noCursorTimeout ()   |Hayır|
|Cursor. Objsleftınbatch ()   |Evet|
|imleç. oldukça ()|   Evet|
|imleç. Readsorun ()|  Evet|
|Cursor. readPref ()      |Evet|
|Cursor. returnKey () |Hayır|
|Cursor. Showrecordıd ()| Hayır|
|Cursor. size ()  |NES|
|Cursor. Skip ()  |Evet|
|cursor.sort()  |   Evet|
|imleç. tailable ()| Hayır|
|Cursor. toArray ()   |Evet|

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
