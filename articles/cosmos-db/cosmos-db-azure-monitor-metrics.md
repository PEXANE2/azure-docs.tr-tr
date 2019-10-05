---
title: Azure Izleyici 'den Azure Cosmos DB ölçümleri alın
description: Azure portal kullanarak Azure Izleyici 'den farklı Azure Cosmos DB ölçüm kategorilerini görüntülemeyi öğrenin.
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/01/2019
ms.openlocfilehash: 905eca99c137af2fd40a1243de8fabd15314477c
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973734"
---
# <a name="monitor-and-debug-azure-cosmos-db-metrics-from-azure-monitor"></a>Azure Izleyici 'de Azure Cosmos DB ölçümleri izleme ve hata ayıklama

Azure Izleyici API 'sinden Azure Cosmos DB ölçümlerini görüntüleyebilirsiniz. Azure Izleyici, Azure portal de dahil olmak üzere ölçülerle etkileşimde bulunmak için çeşitli yollar sağlar, REST API üzerinden bunlara erişim veya PowerShell ya da CLı kullanarak sorgular. Azure Cosmos DB ölçümler, varsayılan olarak tek dakikalık bir sıklıkta toplanan düşük gecikmeli sayısal değerlerdir ve bu ölçümleri de toplayabilirsiniz. Bu ölçümler gerçek zamanlı senaryoları desteklenebilecek bir şekilde kullanılabilir.  

Bu makalede, Azure portal kullanarak Azure Izleyici 'den görüntüleyebileceğiniz farklı Azure Cosmos DB ölçümleri açıklanmaktadır. Yaygın kullanım örnekleri ile ilgileniyorsanız ve bu sorunları çözümlemek ve hatalarını ayıklamak için Azure Cosmos DB ölçümleri yeniden kullanıldığında, [Azure Cosmos DB makalesinde ölçümler Ile izleme ve hata ayıklama](use-metrics.md) bölümüne bakın. Mevcut Azure Cosmos hesaplarınızdan birini kullanacaksınız ve veritabanı, kapsayıcı, bölge, istek ya da işlem düzeylerinde farklı ölçümleri görüntüleyecaksınız. Bu nedenle, örnek veriler içeren bir Azure Cosmos hesabınız olduğundan ve bu verilerde CRUD işlemleri gerçekleştirdiğinizden emin olun.

## <a name="view-metrics-from-azure-portal"></a>Azure portal ölçümleri görüntüleme

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.

1. Sol taraftaki Gezinti çubuğundan **izleyici** ' yi seçin ve **ölçümler**' i seçin.

   ![Azure Izleyici 'de ölçümler bölmesi](./media/cosmos-db-azure-monitor-metrics/monitor-metrics-blade.png)

1. **Ölçümler** bölmesinden > **bir kaynak seçin** > gerekli **aboneliği**ve **kaynak grubunu**seçin. **Kaynak türü**için **Azure Cosmos DB hesapları**' nı seçin, mevcut Azure Cosmos hesaplarınızdan birini seçin ve **Uygula**' yı seçin. 

   ![Ölçümleri görüntülemek için bir Cosmos DB hesabı seçin](./media/cosmos-db-azure-monitor-metrics/select-cosmosdb-account.png)

1. Ardından, kullanılabilir ölçümler listesinden bir ölçüm seçebilirsiniz. Talep birimleri, depolama, gecikme, kullanılabilirlik, Cassandra ve diğer kullanıcılara özgü ölçümleri seçebilirsiniz. Bu listedeki tüm kullanılabilir ölçümler hakkında ayrıntılı bilgi edinmek için bu makalenin [kategoriye göre ölçümler](#metrics-by-category) bölümüne bakın. Bu örnekte, **istek birimlerini** ve toplama değeri olarak **Ort** ' i seçelim. 

   Bu ayrıntılara ek olarak, ölçümlerin **zaman aralığını** ve **zaman parçalı yapısını** da seçebilirsiniz. En fazla, son 30 güne ait ölçümleri görüntüleyebilirsiniz.  Filtreyi uyguladıktan sonra filtreniz temelinde bir grafik görüntülenir. Seçili dönem için dakika başına tüketilen ortalama istek birimi sayısını görebilirsiniz.  

   ![Azure portal bir ölçüm seçin](./media/cosmos-db-azure-monitor-metrics/metric-types.png)

## <a name="add-filters-to-metrics"></a>Ölçümlere filtre ekleme

Ayrıca ölçümleri ve belirli bir **CollectionName**, **DatabaseName**, **OperationType**, **Region**ve **StatusCode**tarafından görüntülenmiş grafik için filtre uygulayabilirsiniz. Ölçümleri filtrelemek için, **Filtre Ekle** ' yi seçin ve **OperationType** gibi gerekli özelliği seçin ve **sorgu**gibi bir değer seçin. Grafik daha sonra seçili dönem için sorgu işlemi için kullanılan istek birimlerini görüntüler. Saklı yordam aracılığıyla yürütülen işlemler, OperationType ölçümü altında kullanılamayacak şekilde günlüğe kaydedilmez.

![Ölçüm ayrıntı düzeyini seçmek için filtre ekleyin](./media/cosmos-db-azure-monitor-metrics/add-metrics-filter.png)

**Bölmeyi Uygula** seçeneğini kullanarak ölçümleri gruplandırabilirsiniz. Örneğin, her işlem türü için istek birimlerini gruplandırabilir ve aşağıdaki görüntüde gösterildiği gibi tüm işlemlerin tek seferde grafiğini görüntüleyebilirsiniz: 

![Uygulama bölme filtresi ekle](./media/cosmos-db-azure-monitor-metrics/apply-metrics-splitting.png)


## <a id="metrics-by-category"></a>Kategoriye göre ölçümler

### <a name="request-metrics"></a>İstek ölçümleri
            
|Ölçüm (ölçüm görünen adı)|Birim (toplama türü) |Açıklama|Boyutlar| Zaman granluğunu| Eski ölçüm eşlemesi | Kullanım |
|---|---|---|---| ---| ---| ---|
| TotalRequests (Toplam Istek) | Sayı (sayı) | Yapılan istek sayısı| DatabaseName, CollectionName, bölge, StatusCode| Tümü | TotalRequests, http 2xx, http 3xx, HTTP 400, http 401, Iç sunucu hatası, hizmet kullanılamıyor, kısıtlanmış Istekler, saniye başına ortalama Istek | Durum kodu başına istekleri, bir dakika ayrıntı düzeyi kapsayıcısını izlemek için kullanılır. Saniye başına ortalama istek almak için dakika olarak sayı toplama kullanın ve 60 ile bölün. |
| MetadataRequests (meta veri Istekleri) |Sayı (sayı) | Meta veri isteklerinin sayısı. Azure Cosmos DB, her hesap için sistem meta veri kapsayıcısını korur, bu da koleksiyonları, veritabanlarını, vb. ve bunların yapılandırmalarının ücretsiz olarak numaralandırılmasını sağlar. | DatabaseName, CollectionName, bölge, StatusCode| Tümü| |Meta veri istekleri nedeniyle kısıtları izlemek için kullanılır.|
| MongoRequests (Mongo Istekleri) | Sayı (sayı) | Yapılan Mongo Isteği sayısı | DatabaseName, CollectionName, Region, CommandName, ErrorCode| Tümü |Mongo sorgu Isteği hızı, Mongo güncelleştirme Isteği hızı, Mongo silme Isteği oranı, Mongo ekleme Isteği hızı, Mongo sayımı Istek hızı| Mongo istek hatalarını izlemek için kullanılır, komut türü başına kullanımlar. |

### <a name="request-unit-metrics"></a>İstek birimi ölçümleri

|Ölçüm (ölçüm görünen adı)|Birim (toplama türü)|Açıklama|Boyutlar| Zaman granluğunu| Eski ölçüm eşlemesi | Kullanım |
|---|---|---|---| ---| ---| ---|
| Mongorequestücret(Mongo Istek ücreti) | Sayı (Toplam) |Tüketilen Mongo Istek birimleri| DatabaseName, CollectionName, Region, CommandName, ErrorCode| Tümü |Mongo sorgu Isteği ücreti, Mongo güncelleştirme Isteği ücreti, Mongo silme Isteği ücreti, Mongo ekleme Isteği ücreti, Mongo sayım Isteği ücreti| Mongo kaynağını bir dakika içinde izlemek için kullanılır.|
| TotalRequestUnits (Toplam Istek birimi)| Sayı (Toplam) | Tüketilen istek birimleri| DatabaseName, CollectionName, bölge, StatusCode |Tümü| TotalRequestUnits| Toplam RU kullanımını bir dakika ayrıntı düzeyinde izlemek için kullanılır. Saniye başına tüketilen ortalama RU 'yu almak için, dakikada toplam toplama kullanın ve 60 ile bölün.|
| Provisionedüretilen iş hacmi (sağlanan aktarım hızı)| Sayı (en fazla) |Kapsayıcı ayrıntı düzeyinde sağlanan aktarım hızı| DatabaseName, ContainerName| 5 dk| | Kapsayıcı başına sağlanan aktarım hızını izlemek için kullanılır.|

### <a name="storage-metrics"></a>Depolama ölçümleri

|Ölçüm (ölçüm görünen adı)|Birim (toplama türü)|Açıklama|Boyutlar| Zaman granluğunu| Eski ölçüm eşlemesi | Kullanım |
|---|---|---|---| ---| ---| ---|
| AvailableStorage (kullanılabilir depolama) |Bayt (Toplam) | Bölge başına 5 dakikalık ayrıntı düzeyinde raporlanan toplam kullanılabilir depolama alanı| DatabaseName, CollectionName, bölgesi| 5 dk| Kullanılabilir depolama alanı| Kullanılabilir depolama kapasitesini (yalnızca sabit depolama koleksiyonları için geçerlidir) izlemek için kullanılan en az ayrıntı düzeyi 5 dakika olmalıdır.| 
| DataUsage (veri kullanımı) |Bayt (Toplam) |Bölge başına 5 dakikalık ayrıntı düzeyinde raporlanan toplam veri kullanımı| DatabaseName, CollectionName, bölgesi| 5 dk |Veri boyutu | Kapsayıcıda ve bölgede toplam veri kullanımını izlemek için kullanılır, en az ayrıntı düzeyi 5 dakika olmalıdır.|
| Indexusage (Dizin kullanımı) | Bayt (Toplam) |Bölge başına 5 dakikalık ayrıntı düzeyinde raporlanan toplam dizin kullanımı| DatabaseName, CollectionName, bölgesi| 5 dk| Dizin boyutu| Kapsayıcıda ve bölgede toplam veri kullanımını izlemek için kullanılır, en az ayrıntı düzeyi 5 dakika olmalıdır. |
| DocumentQuota (belge kotası) | Bayt (Toplam) | Bölge başına 5 dakikalık ayrıntı düzeyinde raporlanan toplam depolama kotası.| DatabaseName, CollectionName, bölgesi| 5 dk |Depolama Kapasitesi| Kapsayıcıda ve bölgede toplam kotayı izlemek için kullanılan minimum ayrıntı düzeyi 5 dakika olmalıdır.|
| DocumentCount (belge sayısı) | Sayı (Toplam) |Bölge başına 5 dakikalık ayrıntı düzeyinde raporlanan toplam belge sayısı| DatabaseName, CollectionName, bölgesi| 5 dk |Belge sayısı|Kapsayıcıda ve bölgede belge sayısını izlemek için kullanılan minimum ayrıntı düzeyi 5 dakika olmalıdır.|

### <a name="latency-metrics"></a>Gecikme süresi ölçümleri

|Ölçüm (ölçüm görünen adı)|Birim (toplama türü)|Açıklama|Boyutlar| Zaman granluğunu| Kullanım |
|---|---|---|---| ---| ---|
| ReplicationLatency (çoğaltma gecikme süresi)| Milisaniye (en düşük, en fazla, Ortalama) | Coğrafi olarak etkinleştirilen hesap için kaynak ve hedef bölgelerde P99 çoğaltma gecikme süresi| SourceRegion, TargetRegion| Tümü | Coğrafi olarak çoğaltılan bir hesabın iki bölgesi arasındaki P99 çoğaltma gecikmesini izlemek için kullanılır. |


### <a name="availability-metrics"></a>Kullanılabilirlik ölçümleri

|Ölçüm (ölçüm görünen adı) |Birim (toplama türü)|Açıklama| Zaman granluğunu| Eski ölçüm eşlemesi | Kullanım |
|---|---|---|---| ---| ---|
| ServiceAvailability (hizmet kullanılabilirliği)| Yüzde (en düşük, en fazla) | Hesap istekleri bir saat ayrıntı düzeyinde kullanılabilirlik| 1 sa | Hizmet kullanılabilirliği | Geçilen toplam istek yüzdesini temsil eder. Durum kodu 410, 500 ya da Tarih ayrıntı düzeyi olarak hesabın kullanılabilirliğini izlemek için kullanılan 503 ise sistem hatası nedeniyle bir istek başarısız olarak kabul edilir. |


### <a name="cassandra-api-metrics"></a>Cassandra API ölçümleri

|Ölçüm (ölçüm görünen adı)|Birim (toplama türü)|Açıklama|Boyutlar| Zaman granluğunu| Kullanım |
|---|---|---|---| ---| ---|
| CassandraRequests (Cassandra Istekleri) | Sayı (sayı) | Yapılan Cassandra API isteği sayısı| DatabaseName, CollectionName, ErrorCode, bölge, OperationType, ResourceType| Tümü| Cassandra isteklerini bir dakikalık ayrıntı düzeyinde izlemek için kullanılır. Saniye başına ortalama istek almak için dakika olarak sayı toplama kullanın ve 60 ile bölün.|
| CassandraRequestCharges (Cassandra Istek ücretleri) | Sayı (Toplam, en az, en fazla, Ortalama) | Cassandra API istekleri tarafından tüketilen istek birimleri| DatabaseName, CollectionName, bölge, OperationType, ResourceType| Tümü| Bir Cassandra API hesabıyla dakikada kullanılan ru 'yi izlemek için kullanılır.|
| Cassandraconnectionkapanışları (Cassandra bağlantı kapanışları) |Sayı (sayı) |Kapatılan Cassandra bağlantısı sayısı| ClosureReason, bölge| Tümü | İstemcilerle Azure Cosmos DB Cassandra API arasındaki bağlantıyı izlemek için kullanılır.|

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB hesabı ölçümleri bölmesinden ölçümleri görüntüleme ve izleme](use-metrics.md)

* [Azure Cosmos DB tanılama günlüğü](logging.md)
