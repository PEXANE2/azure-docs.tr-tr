---
title: Azure Cosmos DB izleme veri başvurusu | Microsoft Dokümanlar
description: Azure Cosmos DB'den gelen verileri izlemek için günlük ve ölçümler başvurusu.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 11/11/2019
ms.author: bwren
ms.custom: subject-monitoring
ms.subservice: logs
ms.openlocfilehash: d243224192b5761af45d387690f5fb41b84481e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588731"
---
# <a name="azure-cosmos-db-monitoring-data-reference"></a>Azure Cosmos DB veri izleme başvurusu
Bu makalede Azure Cosmos DB'nin performansını ve kullanılabilirliğini analiz etmek için toplanan günlük ve ölçüm verilerine başvuru sağlar. Azure [Cosmos DB](monitor-cosmos-db.md) için izleme verilerini toplama ve analiz etme hakkında ayrıntılı bilgi için İzleme Cosmos DB'ye bakın.


## <a name="resource-logs"></a>Kaynak günlükleri
Aşağıdaki tabloda, Azure Monitör Günlükleri veya Azure Depolama'da toplandıklarında Azure Cosmos DB kaynak günlüklerinin özellikleri listelenmektedir. Azure Monitor Günlükleri'nde, Microsoft'un **Kaynak Sağlayıcısı** değerine sahip **AzureDiagnostics** tablosunda *toplanır. DOCUMENTDB*. 

| Azure Depolama alanı veya özelliği | Azure Monitör Günlükleri özelliği | Açıklama |
| --- | --- | --- |
| **time** | **TimeGenerated** | İşlemin gerçekleştiği tarih ve saat (UTC). |
| **Resourceıd** | **Kaynak** | Günlüklerin etkin olduğu Azure Cosmos DB hesabı.|
| **category** | **Kategori** | Azure Cosmos DB günlükleri için, **DataPlaneRequests**, **MongoRequests**, **QueryRuntimeStatistics**, **PartitionKeyStatistics**, **PartitionKeyRUConsumption**, **ControlPlaneRequests** kullanılabilir günlük türleridir. |
| **operationName** | **Operasyon Adı** | Operasyonun adı. Bu değer aşağıdaki işlemlerden herhangi biri olabilir: Oluştur, Güncelleştir, Oku, OkuFeed, Sil, Değiştir, Yürüt, SqlQuery, Sorgu, JSQuery, Head, HeadFeed veya Upsert.   |
| **Özellikler** | yok | Bu alanın içeriği izleyen satırlarda açıklanmıştır. |
| **Activityıd** | **activityId_g** | Günlüğe kaydedilmiş işlem için benzersiz GUID. |
| **Useragent** | **userAgent_s** | İsteği gerçekleştiren istemci kullanıcı aracısını belirten bir dize. Biçim {kullanıcı aracısı adı}/{version}'dur.|
| **requestResourceType** | **requestResourceType_s** | Erişilen kaynağın türü. Bu değer aşağıdaki kaynak türlerinden herhangi biri olabilir: Veritabanı, Kapsayıcı, Belge, Eki, Kullanıcı, İzin, Depolanmış Yordam, Tetikleyici, UserDefinedFunction veya Teklif. |
| **Statuscode** | **statusCode_s** | İşlemin yanıt durumu. |
| **requestResourceId** | **ResourceId** | İstekle ilgili kaynak Kimliği. Değer, gerçekleştirilen işlembağlı olarak veritabanıRid, collectionRid veya documentRid'e işaret edebilir.|
| **clientIpAddress** | **clientIpAddress_s** | Müşterinin IP adresi. |
| **requestCharge** | **requestCharge_s** | İşlem tarafından kullanılan RUS sayısı |
| **koleksiyonRid** | **collectionId_s** | Koleksiyon için benzersiz kimlik.|
| **Süre** | **duration_s** | Çalışma süresi, milisaniye cinsinden. |
| **requestLength** | **requestLength_s** | İsteğin uzunluğu, baytlar halinde. |
| **responseUzunluk** | **responseLength_s** | Yanıtın uzunluğu, baytlar halinde.|
| **kaynakTokenUserRid** | **resourceTokenUserRid_s** | [Kaynak belirteçleri](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) kimlik doğrulaması için kullanıldığında bu değer boş değildir. Değer, kullanıcının kaynak kimliğine işaret verir. |

Tüm Azure Monitor günlük kategorilerinin ve ilişkili şemalara bağlantılar listesi için Bkz. [Azure Monitör günlükleri kategorileri ve şemaları.](../azure-monitor/platform/diagnostic-logs-schema.md) 

## <a name="metrics"></a>Ölçümler
Aşağıdaki tablolarda Azure CosmOS DB için toplanan platform ölçümleri listelenir. Tüm ölçümler ad alanı **Cosmos DB standart ölçümleri**saklanır.

Tüm Azure Monitor destek ölçümlerinin (CosmosDB dahil) listesi için Bkz. [Azure Monitor desteklenen ölçümler.](../azure-monitor/platform/metrics-supported.md) 

#### <a name="request-metrics"></a>İstek ölçümleri
            
|Metrik (Metrik Görüntü Adı)|Ünite (Toplama Türü) |Açıklama|Boyutlar| Zaman tanecikleri| Eski metrik eşleme | Kullanım |
|---|---|---|---| ---| ---| ---|
| Toplam İstek (Toplam İstek) | Sayım (Sayım) | Yapılan istek sayısı| DatabaseName, CollectionName, Bölge, StatusCode| Tümü | TotalRequests, Http 2xx, Http 3xx, Http 400, Http 401, Internal Server hatası, Hizmet Kullanılamıyor, Daraltılmış İstekler, Saniye Başına Ortalama İstekler | Durum kodu başına istekleri izlemek için kullanılır, bir dakika ayrıntılı lık konteyner. Saniyede ortalama istek almak için, dakikada Sayma toplama kullanın ve 60'a bölün. |
| MetadataRequests (Meta veri istekleri) |Sayım (Sayım) | Meta veri isteklerisayısı. Azure Cosmos DB, her hesap için koleksiyonları, veritabanlarını vb. ve bunların yapılandırmalarını ücretsiz olarak sayanıza olanak tanıyan sistem meta veri kapsayıcısını tutar. | DatabaseName, CollectionName, Bölge, StatusCode| Tümü| |Meta veri istekleri nedeniyle gazları izlemek için kullanılır.|
| MongoRequests (Mongo İstekleri) | Sayım (Sayım) | Yapılan Mongo İstekLerinin Sayısı | DatabaseName, CollectionName, Bölge, Komut Adı, Hata Kodu| Tümü |Mongo Sorgu İstek Oranı, Mongo Güncelleme İstek Oranı, Mongo Silme İstek Oranı, Mongo Ekle İstek Oranı, Mongo Sayısı İstek Oranı| Mongo istek hatalarını, komut türü başına kullanımları izlemek için kullanılır. |

#### <a name="request-unit-metrics"></a>İstek Birimi ölçümleri

|Metrik (Metrik Görüntü Adı)|Ünite (Toplama Türü)|Açıklama|Boyutlar| Zaman tanecikleri| Eski metrik eşleme | Kullanım |
|---|---|---|---| ---| ---| ---|
| MongoRequestCharge (Mongo İstek Ücreti) | Sayı (Toplam) |Mongo İstek Birimleri Tüketilen| DatabaseName, CollectionName, Bölge, Komut Adı, Hata Kodu| Tümü |Mongo Sorgu Talep Ücreti, Mongo Güncelleme İstek Ücreti, Mongo Silme İstek Ücreti, Mongo Ekle İstek Ücreti, Mongo Kont İstek Ücreti| Bir dakika içinde Mongo kaynak RUs izlemek için kullanılır.|
| Toplam İstek Birimleri (Toplam İstek Birimleri)| Sayı (Toplam) | Tüketilen İstek Birimleri| DatabaseName, CollectionName, Bölge, StatusCode |Tümü| Toplam İstek Birimleri| Toplam RU kullanımını bir dakika parçalı olarak izlemek için kullanılır. Ortalama RU saniyede tüketilen almak için, dakikada Toplam toplama kullanın ve 60'a bölün.|
| ProvisionedThroughput (Sağlanan Girdi)| Sayı (Maksimum) |Konteyner tanecikliliğinde sağlanan iş| DatabaseName, ContainerName| 5 milyon| | Konteyner başına sağlanan iş buzun izlenmesi için kullanılır.|

#### <a name="storage-metrics"></a>Depolama ölçümleri

|Metrik (Metrik Görüntü Adı)|Ünite (Toplama Türü)|Açıklama|Boyutlar| Zaman tanecikleri| Eski metrik eşleme | Kullanım |
|---|---|---|---| ---| ---| ---|
| Kullanılabilir Depolama (Kullanılabilir Depolama) |Bayt (Toplam) | Bölge başına 5 dakikalık parçalı olarak bildirilen toplam kullanılabilir depolama alanı| DatabaseName, CollectionName, Bölge| 5 milyon| Kullanılabilir Depolama| Kullanılabilir depolama kapasitesini izlemek için kullanılır (yalnızca sabit depolama koleksiyonları için geçerlidir) Minimum parçalılık 5 dakika olmalıdır.| 
| DataUsage (Veri Kullanımı) |Bayt (Toplam) |Bölge başına 5 dakikalık parçalı olarak bildirilen toplam veri kullanımı| DatabaseName, CollectionName, Bölge| 5 milyon |Veri boyutu | Kapsayıcı ve bölgede toplam veri kullanımını izlemek için kullanılan minimum taneciklilik 5 dakika olmalıdır.|
| IndexUsage (Dizin Kullanımı) | Bayt (Toplam) |Bölge başına 5 dakikalık parçalı olarak bildirilen Toplam Endeks kullanımı| DatabaseName, CollectionName, Bölge| 5 milyon| Dizin Boyutu| Kapsayıcı ve bölgede toplam veri kullanımını izlemek için kullanılan minimum taneciklilik 5 dakika olmalıdır. |
| BelgeKotası (Belge Kotası) | Bayt (Toplam) | Bölge başına 5 dakikalık parçalı olarak bildirilen toplam depolama kotası.| DatabaseName, CollectionName, Bölge| 5 milyon |Depolama Kapasitesi| Konteyner ve bölgede toplam kotayı izlemek için kullanılan minimum tanecik5 dakika olmalıdır.|
| Belge Sayısı (Belge Sayısı) | Sayı (Toplam) |Bölge başına 5 dakikalık parçalı olarak bildirilen toplam belge sayısı| DatabaseName, CollectionName, Bölge| 5 milyon |Belge Sayısı|Kapsayıcı ve bölgede belge sayısını izlemek için kullanılan minimum taneciklilik 5 dakika olmalıdır.|

#### <a name="latency-metrics"></a>Gecikme ölçümleri

|Metrik (Metrik Görüntü Adı)|Ünite (Toplama Türü)|Açıklama|Boyutlar| Zaman tanecikleri| Kullanım |
|---|---|---|---| ---| ---|
| ReplicationLatency (Çoğaltma Gecikme)| milisaniye (minimum, maksimum, ortalama) | Coğrafi etkin hesap için kaynak ve hedef bölgeler arasında P99 Çoğaltma Gecikmesi| SourceRegion, TargetRegion| Tümü | Coğrafi olarak çoğaltılmış bir hesap için herhangi iki bölge arasındaki P99 çoğaltma gecikmesini izlemek için kullanılır. |
| Sunucu Tarafı GecikmeSi| MiliSaniye (Ortalama) | İsteğin işlenmesi için sunucu tarafından alınan süre. | Koleksiyon Adı, Bağlantı Modu, DatabaseName, OperationType, PublicAPIType, Bölge | Tümü | Azure Cosmos DB sunucusundaki istek gecikmesini izlemek için kullanılır. |



#### <a name="availability-metrics"></a>Kullanılabilirlik ölçümleri

|Metrik (Metrik Görüntü Adı) |Ünite (Toplama Türü)|Açıklama| Zaman tanecikleri| Eski metrik eşleme | Kullanım |
|---|---|---|---| ---| ---|
| Servis Kullanılabilirliği (Servis Kullanılabilirliği)| Yüzde (Minimum, Maksimum) | Hesap, bir saatlik ayrıntılı olarak kullanılabilirlik isteklerini| 1H | Hizmet Kullanılabilirliği | Toplam geçirilen isteklerin yüzdesini temsil eder. Durum kodu 410, 500 veya 503 Saat ayrıntılı hesap kullanılabilirliğini izlemek için kullanılan ise bir istek sistem hatası nedeniyle başarısız olarak kabul edilir. |


#### <a name="cassandra-api-metrics"></a>Cassandra API ölçümleri

|Metrik (Metrik Görüntü Adı)|Ünite (Toplama Türü)|Açıklama|Boyutlar| Zaman tanecikleri| Kullanım |
|---|---|---|---| ---| ---|
| Cassandraİstek (Cassandra İstekleri) | Sayım (Sayım) | Cassandra API isteklerinin sayısı| DatabaseName, CollectionName, ErrorCode, Region, OperationType, ResourceType| Tümü| Cassandra isteklerini bir dakika içinde izlemek için kullanılır. Saniyede ortalama istek almak için, dakikada Sayma toplama kullanın ve 60'a bölün.|
| CassandraRequestCharges (Cassandra İstek Ücretleri) | Sayım (Toplam, Min, Max, Avg) | Cassandra API istekleri tarafından tüketilen İstek Birimleri| DatabaseName, CollectionName, Bölge, İşlem Türü, Kaynak Türü| Tümü| Cassandra API hesabı tarafından dakikada kullanılan RUs'ları izlemek için kullanılır.|
| CassandraConnectionClosures (Cassandra Bağlantı Kapakları) |Sayım (Sayım) |Kapatılan Cassandra Bağlantı Sayısı| KapanışReason, Bölge| Tümü | İstemciler ve Azure Cosmos DB Cassandra API arasındaki bağlantıyı izlemek için kullanılır.|

## <a name="see-also"></a>Ayrıca Bkz.

- Azure Cosmos DB izleme açıklaması için [Azure Cosmos DB'yi](monitor-cosmos-db.md) izleme bölümüne bakın.
- Azure kaynaklarını izleme yle ilgili ayrıntılar için [Azure Kaynaklarını Azure Monitörü ile izleme](../azure-monitor/insights/monitor-azure-resource.md) bilgisine bakın.
