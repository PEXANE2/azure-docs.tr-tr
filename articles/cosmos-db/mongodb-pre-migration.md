---
title: MongoDB için Azure Cosmos DB API 'sine veri geçişinin geçiş öncesi adımları
description: Bu belge, MongoDB 'den Cosmos DB 'e veri geçişinin önkoşulları hakkında genel bakış sağlar.
author: roaror
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: roaror
ms.openlocfilehash: 4dc7038d0ff5180f15a43268fd3f3aa0cbb0c7a0
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75445192"
---
# <a name="pre-migration-steps-for-data-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB 'den Azure Cosmos DB MongoDB için API 'sine veri geçişleri için geçiş öncesi adımlar

Verilerinizi MongoDB 'ye (Şirket içinde veya bulutta (IaaS)) MongoDB için API Azure Cosmos DB geçirmeden önce şunları yapmalısınız:

1. [Azure Cosmos DB hesabı oluşturma](#create-account)
2. [İş yükleriniz için gereken aktarım hızını tahmin etme](#estimate-throughput)
3. [Verileriniz için en iyi bölüm anahtarını seçin](#partitioning)
4. [Verilerinize ayarlayabileceğiniz dizin oluşturma ilkesini anlayın](#indexing)

Geçiş için yukarıdaki önkoşulları zaten tamamladıysanız, gerçek veri geçiş yönergeleri için [MongoDB verilerini MongoDB için Azure Cosmos DB GEÇIRME API 'sine](../dms/tutorial-mongodb-cosmos-db.md) bakın. Aksi takdirde, bu belge, bu önkoşulların işlenmesi için yönergeler sağlar. 

## <a id="create-account"></a>Azure Cosmos DB hesabı oluşturma 

Geçişe başlamadan önce, [MongoDB için Azure Cosmos DB API 'sini kullanarak bir Azure Cosmos hesabı oluşturmanız](create-mongodb-dotnet.md)gerekir. 

Hesap oluşturma sırasında, verilerinizi [Global olarak dağıtmak](distribute-data-globally.md) için ayarlar ' ı seçebilirsiniz. Ayrıca, her bir bölgenin hem yazma hem de okuma bölgesi olmasına izin veren çok bölgeli yazmaları (veya çok yöneticili yapılandırma) etkinleştirme seçeneğiniz de vardır.

![Hesap-oluşturma](./media/mongodb-pre-migration/account-creation.png)

## <a id="estimate-throughput"></a>İş yükleriniz için işleme gereksinimini tahmin etme

[Veritabanı geçiş hizmeti 'ni (DMS)](../dms/dms-overview.md)kullanarak Geçişe başlamadan önce, Azure Cosmos veritabanları ve koleksiyonlarınız için sağlamak üzere üretilen iş miktarını tahmin etmelisiniz.

Üretilen iş, aşağıdakilerden biri ile sağlanabilir:

- Koleksiyon

- Veritabanı

> [!NOTE]
> Ayrıca, bir veritabanındaki bazı koleksiyonların adanmış sağlanmış işleme sahip olabileceği ve başkalarının üretilen işi paylaştığı bir birleşimi de olabilir. Ayrıntılar için lütfen [bir veritabanı ve kapsayıcı için aktarım hızı ayarlama](set-throughput.md) sayfasına bakın.
>

İlk olarak, veritabanı veya koleksiyon düzeyinde üretilen iş sağlama veya her ikisinin birleşimini belirlemek istediğinize karar vermeniz gerekir. Genel olarak, koleksiyon düzeyinde adanmış bir üretilen iş hacmi yapılandırmanız önerilir. Veritabanı düzeyinde sağlama performansı, veritabanınızdaki koleksiyonların sağlanan aktarım hızını paylaşmasını sağlar. Ancak, paylaşılan verimlilik sayesinde, her bir koleksiyonda belirli bir aktarım hızı garantisi yoktur ve belirli bir koleksiyonda tahmin edilebilir bir performans kalmaz.

Her bir koleksiyona ne kadar üretilen iş ayrıldığınızdan emin değilseniz, veritabanı düzeyinde aktarım hızı ' nı seçebilirsiniz. Azure Cosmos veritabanınızda yapılandırılan üretilen iş verimini, bir MongoDB VM 'sinin veya fiziksel sunucunun işlem kapasitesinden mantıksal bir eşdeğer olarak düşünebilirsiniz, ancak Esnek ölçek özelliği sayesinde daha uygun maliyetli bir işlem yapabilirsiniz. Daha fazla bilgi için bkz. [Azure Cosmos kapsayıcılarında ve veritabanlarında Işleme sağlama](set-throughput.md).

Veritabanı düzeyinde üretilen iş sağlamak istiyorsanız, bu veritabanı içinde oluşturulan tüm koleksiyonlar bir bölüm/parça anahtarıyla oluşturulmalıdır. Bölümlendirme hakkında daha fazla bilgi için, bkz. [Azure Cosmos DB bölümlendirme ve yatay ölçeklendirme](partition-data.md). Geçiş sırasında bir bölüm/parça anahtarı belirtmezseniz, Azure veritabanı geçiş hizmeti, parça anahtarı alanını otomatik olarak her belge için oluşturulan bir *_id* özniteliğiyle doldurur.

### <a name="optimal-number-of-request-units-rus-to-provision"></a>Sağlanacak en iyi Istek birimi (ru) sayısı

Azure Cosmos DB, verimlilik önceden sağlanır ve saniye başına Istek birimleri (RU) cinsinden ölçülür. Bir VM 'de veya şirket içinde MongoDB çalıştıran iş yükleriniz varsa, RU 'nın fiziksel kaynaklar için bir VM veya şirket içi sunucu boyutu ve sahip oldukları kaynakların (örneğin, bellek, CPU, IOPS) basit bir soyutlama olarak olduğunu düşünün. 

Sanal makinelerin veya şirket içi sunucuların aksine, ru 'nın ölçeği dilediğiniz zaman daha kolay ve daha düşük. Sağlanan ru sayısını Saniyeler içinde değiştirebilir ve yalnızca belirli bir saatlik dönem için sağladığınız en fazla ru sayısı için faturalandırılırsınız. Daha fazla bilgi için [Azure Cosmos DB istek birimleri](request-units.md)bölümüne bakın.

Aşağıdakiler, gerekli RUs sayısını etkileyen önemli faktörlerdir:
- **Öğe (örn. belge) boyutu**: bir öğe/belge boyutu arttıkça, öğe/belge okuma veya yazma için tüketilen ru sayısı da artar.
- **Öğe özelliği sayısı**: tüm özelliklerde [varsayılan dizin oluşturma](index-overview.md) varsayılıyor, öğe yazmak için tüketilen ru sayısı, öğe özelliği sayısı arttıkça artar. [Dizin oluşturma özelliklerinin sayısını sınırlayarak](index-policy.md)yazma işlemleri için istek birimi tüketimini azaltabilirsiniz.
- **Eşzamanlı işlemler**: tüketilen istek birimleri ayrıca farklı CRUD işlemlerinin (yazma, okuma, güncelleştirme, silme işlemleri gibi) ve daha karmaşık sorguların yürütüldüğü sıklığa bağlıdır. [Mongostat](https://docs.mongodb.com/manual/reference/program/mongostat/) 'ı, geçerli MongoDB verilerinizin eşzamanlılık ihtiyaçlarını karşılamak için kullanabilirsiniz.
- **Sorgu desenleri**: sorgunun karmaşıklığı, sorgu tarafından kaç istek biriminin tüketildiğini etkiler.

[Mongoexport](https://docs.mongodb.com/manual/reference/program/mongoexport/) kullanarak JSON dosyalarını dışa aktardığınızda ve saniye başına gerçekleşen yazma, okuma, güncelleştirme ve silme işlemlerinin sayısını anlamak için [Azure Cosmos DB kapasite planlayıcısı](https://www.documentdb.com/capacityplanner) ' nı kullanarak, sağlanacak ilk ru sayısını tahmin edebilirsiniz. Kapasite planlayıcısı, daha karmaşık sorguların maliyetinde etken yapmaz. Bu nedenle, verilerinizde karmaşık sorgular varsa, ek RUs kullanılır. Hesap makinesi ayrıca tüm alanların dizine alındığı ve oturum tutarlılığının kullanıldığı varsayılır. Sorguların maliyetini anlamanın en iyi yolu, Azure Cosmos DB için verilerinizi (veya örnek verileri) geçirmektir, [Cosmos DB uç noktasına bağlanıp](connect-mongodb-account.md) MongoDB kabu`getLastRequestStastistics` ğundan bir örnek sorgu çalıştırarak, tüketilen ru sayısını gösteren istek ücreti elde eder:

`db.runCommand({getLastRequestStatistics: 1})`

Bu komut, aşağıdakine benzer bir JSON belgesinin çıktısını alacak:

```{  "_t": "GetRequestStatisticsResponse",  "ok": 1,  "CommandName": "find",  "RequestCharge": 10.1,  "RequestDurationInMilliSeconds": 7.2}```

Bir sorgu tarafından tüketilen RUs sayısını ve bu sorguya yönelik eşzamanlılık ihtiyaçlarını anladıktan sonra, sağlanan ru sayısını ayarlayabilirsiniz. Ru 'yi iyileştirmek tek seferlik bir olay değildir. ağır bir iş yüküne veya veri içeri aktarmaya bağlı olarak, ağır bir trafik beklenmediğinden bağımsız olarak sağlanan ru 'yı sürekli olarak iyileştirmeli veya ölçeklendirmelisiniz.

## <a id="partitioning"></a>Bölüm anahtarınızı seçin
Bölümlendirme, Azure Cosmos DB gibi küresel olarak dağıtılan bir veritabanına geçmeden önce önemli bir noktadır. Azure Cosmos DB, uygulamanızın ölçeklenebilirlik ve performans ihtiyaçlarını karşılamak üzere bir veritabanındaki ayrı kapsayıcıları ölçeklendirmek için bölümleme kullanır. Bölümleme bölümünde, bir kapsayıcıdaki öğeler mantıksal bölümler adlı farklı alt kümelere bölünür. Verileriniz için doğru bölüm anahtarını seçmeye ilişkin ayrıntılar ve öneriler için lütfen [bölüm anahtarı seçme bölümüne](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey)bakın. 

## <a id="indexing"></a>Verilerinizin dizinini oluştur
Varsayılan olarak, Azure Cosmos DB tüm veri alanlarınızı alma sırasında dizinleyerek. Azure Cosmos DB [Dizin oluşturma ilkesini](index-policy.md) dilediğiniz zaman değiştirebilirsiniz. Aslında, verileri geçirirken Dizin oluşturmayı kapatmanız ve veriler zaten Cosmos DB olduğunda yeniden açmanız önerilir. Dizin oluşturma hakkında daha fazla ayrıntı için [Azure Cosmos DB bölümünde Dizin oluşturma](index-overview.md) ' da daha fazla bilgi edinebilirsiniz. 

[Azure veritabanı geçiş hizmeti](../dms/tutorial-mongodb-cosmos-db.md) , MongoDB koleksiyonlarını benzersiz dizinlerle otomatik olarak geçirir. Ancak, geçiş işleminden önce benzersiz dizinlerin oluşturulması gerekir. Azure Cosmos DB, Koleksiyonlarınızdan zaten veri olduğunda benzersiz dizinlerin oluşturulmasını desteklemez. Daha fazla bilgi için bkz. [Azure Cosmos DB Içindeki benzersiz anahtarlar](unique-keys.md).

## <a name="next-steps"></a>Sonraki adımlar
* [MongoDB verilerinizi veritabanı geçiş hizmetini kullanarak Cosmos DB geçirin.](../dms/tutorial-mongodb-cosmos-db.md) 
* [Azure Cosmos kapsayıcılarında ve veritabanlarında üretilen iş sağlama](set-throughput.md)
* [Azure Cosmos DB'de bölümleme](partition-data.md)
* [Azure Cosmos DB genel dağıtım](distribute-data-globally.md)
* [Azure Cosmos DB 'de dizin oluşturma](index-overview.md)
* [Azure Cosmos DB'de İstek birimleri](request-units.md)
