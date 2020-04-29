---
title: Veri başvurusunu izleme Azure Cosmos DB | Microsoft Docs
description: Azure Cosmos DB verileri izlemek için günlük ve ölçüm başvurusu.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 11/11/2019
ms.author: bwren
ms.custom: subject-monitoring
ms.subservice: logs
ms.openlocfilehash: d243224192b5761af45d387690f5fb41b84481e6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77588731"
---
# <a name="azure-cosmos-db-monitoring-data-reference"></a>Azure Cosmos DB veri izleme başvurusu
Bu makalede Azure Cosmos DB'nin performansını ve kullanılabilirliğini analiz etmek için toplanan günlük ve ölçüm verilerine başvuru sağlar. Azure Cosmos DB izleme verilerini toplama ve çözümleme hakkında ayrıntılar için bkz. [izleme Cosmos DB](monitor-cosmos-db.md) .


## <a name="resource-logs"></a>Kaynak günlükleri
Aşağıdaki tabloda, Azure Izleyici günlüklerinde veya Azure Storage 'da toplandıklarında Azure Cosmos DB kaynak günlüklerinin özellikleri listelenmektedir. Azure Izleyici günlüklerinde, MICROSOFT 'un **ResourceProvider** değeri ile **AzureDiagnostics** tablosunda toplanır *. DOCUMENTDB*. 

| Azure depolama alanı veya özelliği | Azure Izleyici günlükleri özelliği | Açıklama |
| --- | --- | --- |
| **time** | **TimeGenerated** | İşlem gerçekleştiği tarih ve saat (UTC). |
| **RESOURCEID** | **Kaynak** | Günlüklerin etkinleştirildiği Azure Cosmos DB hesabı.|
| **alan** | **Kategori** | Azure Cosmos DB günlükleri, **Dataplanerequests**, **mongorequests**, **QueryRuntimeStatistics**, **partitionkeystatıstıcs**, **partitionkeyrutüketim**, **controlplanerequests** , kullanılabilir günlük türleridir. |
| **operationName** | **OperationName** | İşlemin adı. Bu değer şu işlemlerden herhangi biri olabilir: oluşturma, güncelleştirme, okuma, ReadFeed, silme, değiştirme, yürütme, SqlQuery, Query, JSQuery, Head, HeadFeed veya upsert.   |
| **özelliklerinin** | yok | Bu alanın içeriği, izleyen satırlarda açıklanmıştır. |
| **Etkinlik kimliği** | **activityId_g** | Günlüğe kaydedilen işlem için benzersiz GUID. |
| **Kullanıcı** | **userAgent_s** | İsteği gerçekleştiren istemci Kullanıcı aracısını belirten bir dize. Biçim {User Agent Name}/{Version} biçimindedir.|
| **requestResourceType** | **requestResourceType_s** | Erişilen kaynağın türü. Bu değer, şu kaynak türlerinden herhangi biri olabilir: veritabanı, kapsayıcı, belge, ek, Kullanıcı, Izin, StoredProcedure, tetikleyici, UserDefinedFunction veya teklif. |
| **Durum** | **statusCode_s** | İşlemin yanıt durumu. |
| **Requestresourceıd** | **ResourceId** | İstekle ilgili olan RESOURCEID. Değer, gerçekleştirilen işleme bağlı olarak databaseRid, collectionRid veya belge kimliği ' ni işaret edebilir.|
| **ClientIpAddress** | **clientIpAddress_s** | İstemcinin IP adresi. |
| **Istek ücreti** | **requestCharge_s** | İşlem tarafından kullanılan RUs sayısı |
| **collectionRid** | **collectionId_s** | Koleksiyonun benzersiz KIMLIĞI.|
| **sürenin** | **duration_s** | İşlemin süresi (milisaniye cinsinden). |
| **requestLength** | **requestLength_s** | İsteğin bayt cinsinden uzunluğu. |
| **responseLength** | **responseLength_s** | Yanıtın bayt cinsinden uzunluğu.|
| **resourceTokenUserRid** | **resourceTokenUserRid_s** | Bu değer, kimlik doğrulaması için [kaynak belirteçleri](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) kullanılırken boş değildir. Değer, kullanıcının kaynak KIMLIĞINE işaret eder. |

Tüm Azure Izleyici günlük kategorilerinin ve ilişkili şemaların bağlantılarının listesi için bkz. [Azure Izleyici günlükleri kategorileri ve şemaları](../azure-monitor/platform/diagnostic-logs-schema.md). 

## <a name="metrics"></a>Ölçümler
Aşağıdaki tablolarda Azure CosmOS DB için toplanan platform ölçümleri listelenmektedir. Tüm ölçümler, **Standart ölçümler Cosmos DB**ad alanında depolanır.

Tüm Azure Izleyici desteği ölçümlerinin bir listesi (CosmosDB dahil) için bkz. [Azure izleyici desteklenen ölçümler](../azure-monitor/platform/metrics-supported.md). 

#### <a name="request-metrics"></a>İstek ölçümleri
            
|Ölçüm (ölçüm görünen adı)|Birim (toplama türü) |Açıklama|Boyutlar| Zaman granluğunu| Eski ölçüm eşlemesi | Kullanım |
|---|---|---|---| ---| ---| ---|
| TotalRequests (Toplam Istek) | Sayı (sayı) | Yapılan istek sayısı| DatabaseName, CollectionName, bölge, StatusCode| Tümü | TotalRequests, http 2xx, http 3xx, HTTP 400, http 401, Iç sunucu hatası, hizmet kullanılamıyor, kısıtlanmış Istekler, saniye başına ortalama Istek | Durum kodu başına istekleri, bir dakika ayrıntı düzeyi kapsayıcısını izlemek için kullanılır. Saniye başına ortalama istek almak için dakika olarak sayı toplama kullanın ve 60 ile bölün. |
| MetadataRequests (meta veri Istekleri) |Sayı (sayı) | Meta veri isteklerinin sayısı. Azure Cosmos DB, her hesap için sistem meta veri kapsayıcısını korur, bu da koleksiyonları, veritabanlarını, vb. ve bunların yapılandırmalarının ücretsiz olarak numaralandırılmasını sağlar. | DatabaseName, CollectionName, bölge, StatusCode| Tümü| |Meta veri istekleri nedeniyle kısıtları izlemek için kullanılır.|
| MongoRequests (Mongo Istekleri) | Sayı (sayı) | Yapılan Mongo Isteği sayısı | DatabaseName, CollectionName, Region, CommandName, ErrorCode| Tümü |Mongo sorgu Isteği hızı, Mongo güncelleştirme Isteği hızı, Mongo silme Isteği oranı, Mongo ekleme Isteği hızı, Mongo sayımı Istek hızı| Mongo istek hatalarını izlemek için kullanılır, komut türü başına kullanımlar. |

#### <a name="request-unit-metrics"></a>İstek birimi ölçümleri

|Ölçüm (ölçüm görünen adı)|Birim (toplama türü)|Açıklama|Boyutlar| Zaman granluğunu| Eski ölçüm eşlemesi | Kullanım |
|---|---|---|---| ---| ---| ---|
| Mongorequestücret(Mongo Istek ücreti) | Sayı (Toplam) |Tüketilen Mongo Istek birimleri| DatabaseName, CollectionName, Region, CommandName, ErrorCode| Tümü |Mongo sorgu Isteği ücreti, Mongo güncelleştirme Isteği ücreti, Mongo silme Isteği ücreti, Mongo ekleme Isteği ücreti, Mongo sayım Isteği ücreti| Mongo kaynağını bir dakika içinde izlemek için kullanılır.|
| TotalRequestUnits (Toplam Istek birimi)| Sayı (Toplam) | Tüketilen istek birimleri| DatabaseName, CollectionName, bölge, StatusCode |Tümü| TotalRequestUnits| Toplam RU kullanımını bir dakika ayrıntı düzeyinde izlemek için kullanılır. Saniye başına tüketilen ortalama RU 'yu almak için, dakikada toplam toplama kullanın ve 60 ile bölün.|
| Provisionedüretilen iş hacmi (sağlanan aktarım hızı)| Sayı (en fazla) |Kapsayıcı ayrıntı düzeyinde sağlanan aktarım hızı| DatabaseName, ContainerName| 5 milyon| | Kapsayıcı başına sağlanan aktarım hızını izlemek için kullanılır.|

#### <a name="storage-metrics"></a>Depolama ölçümleri

|Ölçüm (ölçüm görünen adı)|Birim (toplama türü)|Açıklama|Boyutlar| Zaman granluğunu| Eski ölçüm eşlemesi | Kullanım |
|---|---|---|---| ---| ---| ---|
| AvailableStorage (kullanılabilir depolama) |Bayt (Toplam) | Bölge başına 5 dakikalık ayrıntı düzeyinde raporlanan toplam kullanılabilir depolama alanı| DatabaseName, CollectionName, bölgesi| 5 milyon| Kullanılabilir Depolama| Kullanılabilir depolama kapasitesini (yalnızca sabit depolama koleksiyonları için geçerlidir) izlemek için kullanılan en az ayrıntı düzeyi 5 dakika olmalıdır.| 
| DataUsage (veri kullanımı) |Bayt (Toplam) |Bölge başına 5 dakikalık ayrıntı düzeyinde raporlanan toplam veri kullanımı| DatabaseName, CollectionName, bölgesi| 5 milyon |Veri boyutu | Kapsayıcıda ve bölgede toplam veri kullanımını izlemek için kullanılır, en az ayrıntı düzeyi 5 dakika olmalıdır.|
| Indexusage (Dizin kullanımı) | Bayt (Toplam) |Bölge başına 5 dakikalık ayrıntı düzeyinde raporlanan toplam dizin kullanımı| DatabaseName, CollectionName, bölgesi| 5 milyon| Dizin boyutu| Kapsayıcıda ve bölgede toplam veri kullanımını izlemek için kullanılır, en az ayrıntı düzeyi 5 dakika olmalıdır. |
| DocumentQuota (belge kotası) | Bayt (Toplam) | Bölge başına 5 dakikalık ayrıntı düzeyinde raporlanan toplam depolama kotası.| DatabaseName, CollectionName, bölgesi| 5 milyon |Depolama Kapasitesi| Kapsayıcıda ve bölgede toplam kotayı izlemek için kullanılan minimum ayrıntı düzeyi 5 dakika olmalıdır.|
| DocumentCount (belge sayısı) | Sayı (Toplam) |Bölge başına 5 dakikalık ayrıntı düzeyinde raporlanan toplam belge sayısı| DatabaseName, CollectionName, bölgesi| 5 milyon |Belge sayısı|Kapsayıcıda ve bölgede belge sayısını izlemek için kullanılan minimum ayrıntı düzeyi 5 dakika olmalıdır.|

#### <a name="latency-metrics"></a>Gecikme süresi ölçümleri

|Ölçüm (ölçüm görünen adı)|Birim (toplama türü)|Açıklama|Boyutlar| Zaman granluğunu| Kullanım |
|---|---|---|---| ---| ---|
| ReplicationLatency (çoğaltma gecikme süresi)| Milisaniye (en düşük, en fazla, Ortalama) | Coğrafi olarak etkinleştirilen hesap için kaynak ve hedef bölgelerde P99 çoğaltma gecikme süresi| SourceRegion, TargetRegion| Tümü | Coğrafi olarak çoğaltılan bir hesabın iki bölgesi arasındaki P99 çoğaltma gecikmesini izlemek için kullanılır. |
| Sunucu tarafı gecikme süresi| Milisaniye (Ortalama) | İsteği işlemek için sunucu tarafından harcanan süre. | CollectionName, ConnectionMode, DatabaseName, OperationType, Publicapitürü, Region | Tümü | Azure Cosmos DB sunucusundaki istek gecikmesini izlemek için kullanılır. |



#### <a name="availability-metrics"></a>Kullanılabilirlik ölçümleri

|Ölçüm (ölçüm görünen adı) |Birim (toplama türü)|Açıklama| Zaman granluğunu| Eski ölçüm eşlemesi | Kullanım |
|---|---|---|---| ---| ---|
| ServiceAvailability (hizmet kullanılabilirliği)| Yüzde (en düşük, en fazla) | Hesap istekleri bir saat ayrıntı düzeyinde kullanılabilirlik| 1 sa | Hizmet kullanılabilirliği | Geçilen toplam istek yüzdesini temsil eder. Durum kodu 410, 500 ya da Tarih ayrıntı düzeyi olarak hesabın kullanılabilirliğini izlemek için kullanılan 503 ise sistem hatası nedeniyle bir istek başarısız olarak kabul edilir. |


#### <a name="cassandra-api-metrics"></a>Cassandra API ölçümleri

|Ölçüm (ölçüm görünen adı)|Birim (toplama türü)|Açıklama|Boyutlar| Zaman granluğunu| Kullanım |
|---|---|---|---| ---| ---|
| CassandraRequests (Cassandra Istekleri) | Sayı (sayı) | Yapılan Cassandra API isteği sayısı| DatabaseName, CollectionName, ErrorCode, bölge, OperationType, ResourceType| Tümü| Cassandra isteklerini bir dakikalık ayrıntı düzeyinde izlemek için kullanılır. Saniye başına ortalama istek almak için dakika olarak sayı toplama kullanın ve 60 ile bölün.|
| CassandraRequestCharges (Cassandra Istek ücretleri) | Sayı (Toplam, en az, en fazla, Ortalama) | Cassandra API istekleri tarafından tüketilen istek birimleri| DatabaseName, CollectionName, bölge, OperationType, ResourceType| Tümü| Bir Cassandra API hesabıyla dakikada kullanılan ru 'yi izlemek için kullanılır.|
| Cassandraconnectionkapanışları (Cassandra bağlantı kapanışları) |Sayı (sayı) |Kapatılan Cassandra bağlantısı sayısı| ClosureReason, bölge| Tümü | İstemcilerle Azure Cosmos DB Cassandra API arasındaki bağlantıyı izlemek için kullanılır.|

## <a name="see-also"></a>Ayrıca Bkz.

- İzleme Azure Cosmos DB açıklaması için bkz. [izleme Azure Cosmos DB](monitor-cosmos-db.md) .
- Azure kaynaklarını izleme hakkında ayrıntılı bilgi için bkz. Azure [izleyici ile Azure kaynaklarını izleme](../azure-monitor/insights/monitor-azure-resource.md) .
