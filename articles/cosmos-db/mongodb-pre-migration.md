---
title: MongoDB için Azure Cosmos DB API 'sine veri geçişinin geçiş öncesi adımları
description: Bu belge, MongoDB 'den Cosmos DB 'e veri geçişinin önkoşulları hakkında genel bakış sağlar.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 01/09/2020
ms.author: lbosq
ms.openlocfilehash: ef3d56b4ec7e4dbe5f6f4097fdd5d8d125b074dc
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75932282"
---
# <a name="pre-migration-steps-for-data-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB 'den Azure Cosmos DB MongoDB için API 'sine veri geçişleri için geçiş öncesi adımlar

Verilerinizi MongoDB 'ye (Şirket içi veya buluttaki) MongoDB için Azure Cosmos DB API 'sine geçirmeden önce şunları yapmalısınız:

1. [MongoDB için Azure Cosmos DB API 'sini kullanma hakkındaki önemli konuları okuyun](#considerations)
2. [Verilerinizi geçirmek için bir seçenek belirleyin](#options)
3. [İş yükleriniz için gereken aktarım hızını tahmin etme](#estimate-throughput)
4. [Verileriniz için en iyi bölüm anahtarını seçin](#partitioning)
5. [Verilerinize ayarlayabileceğiniz dizin oluşturma ilkesini anlayın](#indexing)

Geçiş için yukarıdaki önkoşulları zaten tamamladıysanız, [MongoDB verilerini Azure veritabanı geçiş hizmeti 'ni kullanarak MongoDB için Azure Cosmos DB API 'Sine geçirebilirsiniz](../dms/tutorial-mongodb-cosmos-db.md). Ayrıca, bir hesap oluşturmadıysanız, [hızlı](create-mongodb-dotnet.md)başlangıçlardan birine gidebilirsiniz.

## <a id="considerations"></a>MongoDB için Azure Cosmos DB API 'SI kullanılırken başlıca önemli noktalar

Aşağıda, MongoDB için Azure Cosmos DB API 'SI hakkında özel özellikler verilmiştir:
- **Kapasite modeli**: Azure Cosmos DB veritabanı kapasitesi, verimlilik tabanlı bir modeli temel alır. Bu model, saniye [başına Istek birimlerini](request-units.md)temel alır. bu birim, saniye başına bir koleksiyonda yürütülen veritabanı işlemlerinin sayısını temsil eden bir birimdir. Bu kapasite [bir veritabanı veya koleksiyon düzeyinde](set-throughput.md)ayrılabilir ve bir ayırma modelinde veya [Autopilot modeli](provision-throughput-autopilot.md)kullanılarak sağlanabilir.
- **Istek birimleri**: her veritabanı işleminin, Azure Cosmos DB Ilişkili bir istek birimleri (ru) maliyeti vardır. Yürütüldüğünde, bu, verilen bir saniyede kullanılabilir istek birimleri düzeyinden çıkarılır. Bir istek, şu anda ayrılmış olandan daha fazla RUs gerektiriyorsa, iki seçenek de ru miktarını artırır veya bir sonraki ikinci olana kadar bekleyip işlemi yeniden denemeyi sürdürecek.
- **Esnek kapasite**: belirli bir koleksiyon veya veritabanının kapasitesi herhangi bir zamanda değişebilir. Bu, veritabanının iş yükünüzün verimlilik gereksinimlerini esnek sağlamasına izin verir.
- **Otomatik**parçalama: Azure Cosmos DB yalnızca parça (veya bölümlendirme) anahtarı gerektiren bir otomatik bölümlendirme sistemi sağlar. [Otomatik bölümleme mekanizması](partition-data.md) tüm Azure Cosmos DB API 'lerinde paylaşılır ve sorunsuz verilere ve yatay dağıtım aracılığıyla ölçeklendirilmesine olanak tanır.

## <a id="options"></a>MongoDB için Azure Cosmos DB API 'SI için geçiş seçenekleri

[MongoDB için Azure Cosmos DB API 'si Için Azure veritabanı geçiş hizmeti](../dms/tutorial-mongodb-cosmos-db.md) , tam olarak yönetilen bir barındırma platformu, geçiş izleme seçenekleri ve otomatik azaltma işleme sağlayarak veri geçişini kolaylaştıran bir mekanizma sağlar. Seçeneklerin tam listesi şunlardır:

|**Geçiş türü**|**Çözüm**|**Dikkat edilmesi gerekenler**|
|---------|---------|---------|
|Offline|[Veri Geçişi Aracı](https://docs.microsoft.com/azure/cosmos-db/import-data)|birden çok kaynağı kolayca ayarlamayı ve desteklemenin &bull; <br/>büyük veri kümeleri için uygun &bull;.|
|Offline|[Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-cosmos-db)|birden çok kaynağı kolayca ayarlamayı ve desteklemenin &bull; <br/>&bull; Azure Cosmos DB toplu yürütücü kitaplığı 'nın kullanımını sağlar <br/>büyük veri kümeleri için uygun &bull; <br/>checksize &bull; olmaması, geçiş işlemi sırasında herhangi bir sorunun tüm geçiş sürecinin yeniden başlatılmasını gerektirmeyeceği anlamına gelir.<br/>sahipsiz bir sıra &bull; olmaması, birkaç hatalı dosyanın tüm geçiş sürecini durduramaması anlamına gelir. <br/>&bull; belirli veri kaynakları için okuma aktarım hızını artırmak üzere özel koda Ihtiyaç duyuyor|
|Offline|[Mevcut Mongo araçları (mongodump, mongorestore, Studio3T)](https://azure.microsoft.com/resources/videos/using-mongodb-tools-with-azure-cosmos-db/)|&bull; kolayca ayarlama ve Tümleştirme <br/>&bull;, kısıtlar için özel işlemeye Ihtiyaç duyuyor|
|Çevrimiçi|[Azure Veritabanı Geçiş Hizmeti](../dms/tutorial-mongodb-cosmos-db-online.md)|Tam olarak yönetilen geçiş hizmeti &bull;.<br/>&bull;, geçiş görevi için barındırma ve izleme çözümleri sağlar. <br/>&bull;, büyük veri kümeleri için uygun ve canlı değişiklikleri çoğaltma işlemini gerçekleştirir <br/>&bull; yalnızca diğer MongoDB kaynaklarıyla kullanılabilir|


## <a id="estimate-throughput"></a>İş yükleriniz için işleme gereksinimini tahmin etme

Azure Cosmos DB, verimlilik önceden sağlanır ve saniye başına Istek birimleri (RU) cinsinden ölçülür. Sanal makinelerin veya şirket içi sunucuların aksine, ru 'nın ölçeği dilediğiniz zaman daha kolay ve daha düşük. Sağlanan RUs sayısını anında değiştirebilirsiniz. Daha fazla bilgi için [Azure Cosmos DB istek birimleri](request-units.md)bölümüne bakın.

Veritabanı hesabı yapılandırmanıza, veri miktarına, belge boyutuna ve saniye başına gerekli okuma ve yazma verilerine göre Istek birimi miktarını öğrenmek için [Azure Cosmos DB kapasite Hesaplayıcı](https://cosmos.azure.com/capacitycalculator/) 'ı kullanabilirsiniz.

Aşağıdakiler, gerekli RUs sayısını etkileyen önemli faktörlerdir:
- **Belge boyutu**: bir öğe/belge boyutu arttıkça, öğe/belge okuma veya yazma Için tüketilen Rus sayısı da artar.
- **Belge özelliği sayısı**: bir belge oluşturmak veya güncelleştirmek Için tüketilen Rus sayısı, özelliklerinin sayısı, karmaşıklığı ve uzunluğu ile ilgilidir. [Dizin oluşturma özelliklerinin sayısını sınırlayarak](mongodb-indexing.md)yazma işlemleri için istek birimi tüketimini azaltabilirsiniz.
- **Sorgu desenleri**: sorgunun karmaşıklığı, sorgu tarafından kaç istek biriminin tüketildiğini etkiler. 

Sorguların maliyetini anlamanın en iyi yolu, Azure Cosmos DB örnek verileri kullanmak ve `getLastRequestStastistics` komutunu kullanarak [MongoDB kabuğundan örnek sorgular çalıştırıp](connect-mongodb-account.md) , tüketilen ru sayısını gösteren istek ücreti elde eder:

`db.runCommand({getLastRequestStatistics: 1})`

Bu komut, aşağıdakine benzer bir JSON belgesinin çıktısını alacak:

```{  "_t": "GetRequestStatisticsResponse",  "ok": 1,  "CommandName": "find",  "RequestCharge": 10.1,  "RequestDurationInMilliSeconds": 7.2}```

[Tanılama ayarlarını](cosmosdb-monitor-resource-logs.md) , Azure Cosmos DB karşı yürütülen sorguların sıklığını ve desenlerini anlamak için de kullanabilirsiniz. Tanılama günlüklerinin sonuçları bir depolama hesabına, bir EventHub örneğine veya [Azure Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)gönderilebilir.  

## <a id="partitioning"></a>Bölüm anahtarınızı seçin
Parça olarak da bilinen bölümlendirme, verileri geçirmeden önce önemli bir noktadır. Azure Cosmos DB, depolama ve aktarım hızı gereksinimlerini karşılamak üzere bir veritabanındaki kapasiteyi artırmak için tam olarak yönetilen bölümlendirme kullanır. Bu özellik, yönlendirme sunucularının barındırılmasına veya yapılandırılmasına gerek yoktur.   

Benzer şekilde bölümlendirme özelliği, kapasiteyi otomatik olarak ekler ve verileri uygun şekilde yeniden dengeler. Verileriniz için doğru bölüm anahtarını seçmeye ilişkin ayrıntılar ve öneriler için lütfen [bölüm anahtarı seçme makalesine](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey)bakın. 

## <a id="indexing"></a>Verilerinizin dizinini oluştur
Varsayılan olarak Azure Cosmos DB, ekli tüm verilerde otomatik dizin oluşturma sağlar. Azure Cosmos DB tarafından sunulan dizin oluşturma özellikleri, bileşik dizinler, benzersiz dizinler ve yaşam süresi (TTL) dizinlerinin eklenmesini içerir. Dizin yönetimi arabirimi `createIndex()` komutuna eşlenir. [Azure Cosmos DB MongoDB IÇIN API 'Sinde dizin oluşturma](mongodb-indexing.md)hakkında daha fazla bilgi edinin.

[Azure veritabanı geçiş hizmeti](../dms/tutorial-mongodb-cosmos-db.md) , MongoDB koleksiyonlarını benzersiz dizinlerle otomatik olarak geçirir. Ancak, geçiş işleminden önce benzersiz dizinlerin oluşturulması gerekir. Azure Cosmos DB, Koleksiyonlarınızdan zaten veri olduğunda benzersiz dizinlerin oluşturulmasını desteklemez. Daha fazla bilgi için bkz. [Azure Cosmos DB Içindeki benzersiz anahtarlar](unique-keys.md).

## <a name="next-steps"></a>Sonraki adımlar
* [MongoDB verilerinizi veritabanı geçiş hizmetini kullanarak Cosmos DB geçirin.](../dms/tutorial-mongodb-cosmos-db.md) 
* [Azure Cosmos kapsayıcılarında ve veritabanlarında üretilen iş sağlama](set-throughput.md)
* [Azure Cosmos DB'de bölümleme](partition-data.md)
* [Azure Cosmos DB genel dağıtım](distribute-data-globally.md)
* [Azure Cosmos DB 'de dizin oluşturma](index-overview.md)
* [Azure Cosmos DB'de İstek birimleri](request-units.md)
