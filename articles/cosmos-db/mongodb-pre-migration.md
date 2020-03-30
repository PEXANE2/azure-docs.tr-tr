---
title: MongoDB için Azure Cosmos DB'nin API'sine veri geçişi için geçiş öncesi adımlar
description: Bu doküman, MongoDB'den Cosmos DB'ye veri geçişi için ön koşullara genel bir bakış sağlar.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 01/09/2020
ms.author: lbosq
ms.openlocfilehash: 73ac1a6ffd5fc2b2d52f169e1e0332044638f9f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75942089"
---
# <a name="pre-migration-steps-for-data-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB'den Azure Cosmos DB'nin MongoDB için API'sine veri geçişleri için geçiş öncesi adımlar

Verilerinizi MongoDB'den (şirket içinde veya bulutta) Azure Cosmos DB'nin MongoDB api'sine geçirmeden önce şunları olmalısınız:

1. [MongoDB için Azure Cosmos DB'nin API'sini kullanma yla ilgili önemli hususları okuyun](#considerations)
2. [Verilerinizi geçirmek için bir seçenek seçin](#options)
3. [İş yükleriniz için gereken iş yükünü tahmin edin](#estimate-throughput)
4. [Verileriniz için en uygun bölüm anahtarını seçin](#partitioning)
5. [Verilerinizde ayarlayabildiğiniz dizin oluşturma ilkesini anlama](#indexing)

Geçiş için yukarıdaki ön koşulları zaten tamamladıysanız, [MongoDB verilerini Azure Veritabanı Geçiş Hizmeti'ni kullanarak MongoDB için Azure Cosmos DB'nin API'sine geçirebilirsiniz.](../dms/tutorial-mongodb-cosmos-db.md) Ayrıca, bir hesap oluşturmadıysanız, hesap oluşturmak için adımları gösteren [Hızlı Başlangıçlar'dan](create-mongodb-dotnet.md) herhangi birini göz atabilirsiniz.

## <a name="considerations-when-using-azure-cosmos-dbs-api-for-mongodb"></a><a id="considerations"></a>MongoDB için Azure Cosmos DB'nin API'si kullanırken dikkat edilmesi gerekenler

Azure Cosmos DB'nin MongoDB için API'si hakkında belirli özellikler şunlardır:

- **Kapasite modeli**: Azure Cosmos DB'deki veritabanı kapasitesi, iş başına dayalı bir modele dayanır. Bu model, bir koleksiyona karşı saniye başına yürütülebilecek veritabanı işlem sayısını temsil eden bir birim olan [saniye başına İstek Birimlerini](request-units.md)temel alır. Bu kapasite [bir veritabanı veya toplama düzeyinde](set-throughput.md)tahsis edilebilir ve bir ayırma modelinde veya Otomatik Pilot [modeli](provision-throughput-autopilot.md)kullanılarak sağlanabilir.

- **İstek Birimleri**: Her veritabanı işleminin Azure Cosmos DB'de ilişkili bir İstek Birimleri (RUs) maliyeti vardır. Yürütüldüğünde, bu, belirli bir saniyedeki kullanılabilir istek birimleri düzeyinden çıkarılır. Bir istek, şu anda tahsis edilen RU/s'den daha fazla RUs gerektiriyorsa, sorunu çözmek için iki seçenek vardır : RUs miktarını artırmak veya bir sonraki ikinci başlayana kadar bekleyin ve işlemi yeniden deneyin.

- **Elastik kapasite**: Belirli bir koleksiyon veya veritabanı nın kapasitesi her zaman değişebilir. Bu, veritabanının iş yükünüzün iş yükü nden elde etme gereksinimlerine esnek bir şekilde uyum sağlamasına olanak tanır.

- **Otomatik parçalama**: Azure Cosmos DB, yalnızca bir parça (veya bölüm anahtarı) gerektiren otomatik bir bölümleme sistemi sağlar. [Otomatik bölümleme mekanizması](partition-data.md) tüm Azure Cosmos DB API'lerinde paylaşılır ve yatay dağıtım boyunca sorunsuz veri ve ölçekleme sağlar.

## <a name="migration-options-for-azure-cosmos-dbs-api-for-mongodb"></a><a id="options"></a>MongoDB için Azure Cosmos DB'nin API'si için geçiş seçenekleri

[Azure Cosmos DB'nin MongoDB için API'si için Azure Veritabanı Geçiş Hizmeti,](../dms/tutorial-mongodb-cosmos-db.md) tam olarak yönetilen bir barındırma platformu, geçiş izleme seçenekleri ve otomatik azaltma işleme sağlayarak veri geçişini kolaylaştıran bir mekanizma sağlar. Seçeneklerin tam listesi şunlardır:

|**Geçiş türü**|**Çözüm**|**Husus -lar**|
|---------|---------|---------|
|Çevrimdışı|[Veri Geçiş Aracı](https://docs.microsoft.com/azure/cosmos-db/import-data)|&bull;Kurulumu kolay ve birden çok kaynağı destekler <br/>&bull;Büyük veri kümeleri için uygun değildir.|
|Çevrimdışı|[Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-cosmos-db)|&bull;Kurulumu kolay ve birden çok kaynağı destekler <br/>&bull;Azure Cosmos DB toplu uygulayıcı kitaplığını kullanır <br/>&bull;Büyük veri kümeleri için uygundur <br/>&bull;Denetim noktasının olmaması, geçiş sırasında ki herhangi bir sorunun tüm geçiş sürecinin yeniden başlatılmasını gerektireceği anlamına gelir<br/>&bull;Ölü harf sırasının olmaması, birkaç hatalı dosyanın tüm geçiş işlemini durdurabileceği anlamına gelir <br/>&bull;Belirli veri kaynakları için okuma iş verisini artırmak için özel kod gerekir|
|Çevrimdışı|[Mevcut Mongo Araçları (mongodump, mongorestore, Studio3T)](https://azure.microsoft.com/resources/videos/using-mongodb-tools-with-azure-cosmos-db/)|&bull;Kurulumu ve entegrasyonu kolay <br/>&bull;Gazlar için özel kullanım ihtiyacı|
|Çevrimiçi|[Azure Veritabanı Geçiş Hizmeti](../dms/tutorial-mongodb-cosmos-db-online.md)|&bull;Tam yönetilen geçiş hizmeti.<br/>&bull;Geçiş görevi için barındırma ve izleme çözümleri sağlar. <br/>&bull;Büyük veri kümeleri için uygundur ve canlı değişiklikleri çoğaltma yıkıyor <br/>&bull;Yalnızca diğer MongoDB kaynaklarıyla çalışır|


## <a name="estimate-the-throughput-need-for-your-workloads"></a><a id="estimate-throughput"></a>İş yüklerinizin iş yükü ihtiyacını tahmin edin

Azure Cosmos DB'de, iş ortası önceden karşılanır ve istek birimlerinde (RU's) saniyede ölçülür. VM'lerin veya şirket içi sunucuların aksine, RUs'ları her zaman ölçeklendirmek kolaydır. Verilen RUS sayısını anında değiştirebilirsiniz. Daha fazla bilgi için Azure [Cosmos DB'deki İstek birimlerine](request-units.md)bakın.

[Azure Cosmos DB Kapasite Hesap](https://cosmos.azure.com/capacitycalculator/) Layıcısı'nı, veritabanı hesabı yapılandırmanıza, veri miktarına, belge boyutuna ve saniyede okuma ve yazma durumuna göre Istek Birimi miktarını belirlemek için kullanabilirsiniz.

Gerekli RUS sayısını etkileyen önemli faktörler şunlardır:
- **Belge boyutu**: Bir öğenin/belgenin boyutu arttıkça, öğe/belgeyi okumak veya yazmak için tüketilen RUS sayısı da artar.

- **Belge özelliği sayısı**:Bir belgeyi oluşturmak veya güncelleştirmek için tüketilen RUS sayısı, özelliklerinin sayısı, karmaşıklığı ve uzunluğuyla ilgilidir. [Dizinlenmiş özellik sayısını sınırlayarak](mongodb-indexing.md)yazma işlemleri için istek birimi tüketimini azaltabilirsiniz.

- **Sorgu desenleri**: Bir sorgunun karmaşıklığı, sorgu tarafından kaç istek biriminin tüketilmesini etkiler. 

Sorguların maliyetini anlamanın en iyi yolu, Azure Cosmos DB'deki örnek verileri kullanmak ve tüketilen RUs `getLastRequestStastistics` sayısını elde etmek için istek ücretini almak için komutu kullanarak [MongoDB Shell'den örnek sorgular çalıştırmaktır:](connect-mongodb-account.md)

`db.runCommand({getLastRequestStatistics: 1})`

Bu komut, aşağıdakilere benzer bir JSON belgesi çıktıracaktır:

```{  "_t": "GetRequestStatisticsResponse",  "ok": 1,  "CommandName": "find",  "RequestCharge": 10.1,  "RequestDurationInMilliSeconds": 7.2}```

Azure Cosmos DB'ye karşı yürütülen sorguların sıklığını ve desenlerini anlamak için [tanılama ayarlarını](cosmosdb-monitor-resource-logs.md) da kullanabilirsiniz. Tanılama günlüklerinin sonuçları bir depolama hesabına, bir EventHub örneğine veya [Azure Günlük Analizi'ne](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)gönderilebilir.  

## <a name="choose-your-partition-key"></a><a id="partitioning"></a>Bölüm anahtarınızı seçin
Sharding olarak da bilinen bölümleme, verileri geçirmeden önce önemli bir göz önünde bulundurulması gereken bir noktadır. Azure Cosmos DB, depolama ve üretim gereksinimlerini karşılamak için veritabanındaki kapasiteyi artırmak için tam yönetilen bölümleme kullanır. Bu özelliğin yönlendirme sunucularının barındırılaması veya yapılandırması gerekmez.   

Benzer şekilde, bölümleme yeteneği otomatik olarak kapasite ekler ve verileri buna göre yeniden dengeler. Verileriniz için doğru bölüm anahtarını seçme yle ilgili ayrıntılar ve öneriler için lütfen [Bölüm Anahtarı seçme makalesine](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey)bakın. 

## <a name="index-your-data"></a><a id="indexing"></a>Verilerinizin dizinini oluşturma
Varsayılan olarak, Azure Cosmos DB eklenen tüm verilerde otomatik dizin oluşturma sağlar. Azure Cosmos DB tarafından sağlanan dizin oluşturma özellikleri arasında bileşik endeksler, benzersiz endeksler ve zaman-canlı (TTL) endeksleri eklenmesi yer almaktadır. Dizin yönetimi arabirimi komuta `createIndex()` eşlenir. [Azure Cosmos DB'nin MongoDB için API'sinde Dizin oluşturma](mongodb-indexing.md)hakkında daha fazla bilgi edinin.

[Azure Veritabanı Geçiş Hizmeti,](../dms/tutorial-mongodb-cosmos-db.md) MongoDB koleksiyonlarını benzersiz dizinlerle otomatik olarak geçirtir. Ancak, benzersiz dizinler geçişten önce oluşturulmalıdır. Azure Cosmos DB, koleksiyonlarınızda zaten veri varken benzersiz dizinlerin oluşturulmasını desteklemez. Daha fazla bilgi için [Azure Cosmos DB'deki Benzersiz anahtarlara](unique-keys.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
* [Veritabanı Geçiş Hizmeti'ni kullanarak MongoDB verilerinizi Cosmos DB'ye geçirin.](../dms/tutorial-mongodb-cosmos-db.md) 
* [Azure Cosmos kapsayıcıları ve veritabanlarında sağlama veri mişi](set-throughput.md)
* [Azure Cosmos DB'de bölümleme](partition-data.md)
* [Azure Cosmos DB'de Küresel Dağıtım](distribute-data-globally.md)
* [Azure Cosmos DB’de dizin oluşturma](index-overview.md)
* [Azure Cosmos DB'de İstek Birimleri](request-units.md)
