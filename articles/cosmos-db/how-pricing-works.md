---
title: Azure Cosmos DB fiyatlandırma modeli
description: Bu makalede, Azure Cosmos DB'nin fiyatlandırma modeli ve maliyet yönetimi ve maliyet planlamanızı nasıl basitleştirildiği açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 7efae8fb3c00868e2740eac2d4d5bcb3c82f663a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75977531"
---
# <a name="pricing-model-in-azure-cosmos-db"></a>Azure Cosmos DB’de modeli fiyatlandırma 

Azure Cosmos DB’nin fiyatlandırma modeli, maliyet yönetimini ve planlamasını basitleştirir. Azure Cosmos DB ile sağladığınız aktarım hızı ve tükettiğiniz depolama için ücret ödersiniz.

* **Sağlanan İş İbadet**: Sağlanan iş -iş mesuliyeti (ayrılmış iş olarak da adlandırılır) herhangi bir ölçekte yüksek performansı garanti eder. İhtiyacınız olan iş parçacığı (RU/s) belirtirsiniz ve Azure Cosmos DB, yapılandırılan iş parçacığının güvenliğini sağlamak için gereken kaynakları adatır. Belirli bir saat için sağlanan maksimum verim için saatlik faturalandırılırsınız.

   > [!NOTE]
   > Sağlanan iş parçacığı modeli kaynakları kapsayıcınıza veya veritabanınıza ayırdığından, iş yükü çalışmasanız bile sağlanan iş yükü için ücretlendirilirsiniz.

* **Tüketilen Depolama**: Belirli bir saat için veri ve dizinler için tüketilen toplam depolama miktarı (GBs) için sabit bir oran faturalandırılır.

Saniyede [İstek Birimleri](request-units.md) (RU/s) olarak belirtilen tedarik edilen iş kaynağı, verileri kapsayıcılardan veya veritabanlarından okumanıza veya yazmanıza olanak tanır. Bir [veritabanı veya kapsayıcı üzerinde iş verime sağlayabilir.](set-throughput.md) İş yükü gereksinimlerinize bağlı olarak, iş yükünü istediğiniz zaman yukarı/aşağı ölçeklendirebilirsiniz. Azure Cosmos DB fiyatlandırması elastiktir ve bir veritabanında veya kapsayıcıda yapılandırdığınız iş ile orantılıdır. Minimum iş ve depolama değerleri ve ölçek artışları, küçük ölçekli kapsayıcılardan büyük ölçekli kapsayıcılara kadar müşterilerin tüm segmentlerine tam bir fiyat ve esneklik spektrumu sağlar. Her veritabanı veya bir kapsayıcı, en az 400 RU/s'lik ünitelerde sağlanan iş ve Depolama Alanı'nda tüketilen 100 RU/s'lik birimlerde sağlanan iş için saatlik olarak faturalandırılır. Sağlanan iş girdilerinden farklı olarak, depolama tüketim esasına göre faturalandırılır. Yani, önceden herhangi bir depolama rezerve etmek zorunda değilsiniz. Yalnızca tükettiğiniz depolama alanı için faturalandırılırsınız.

Daha fazla bilgi için [Azure Cosmos DB fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın ve [Azure Cosmos DB faturanızı anlayınız.](understand-your-bill.md)

Azure Cosmos DB'deki fiyatlandırma modeli tüm API'lerde tutarlıdır. Daha fazla bilgi edinmek için [Azure Cosmos DB fiyatlandırma modelinin müşteriler için nasıl uygun maliyetli olduğunu](total-cost-ownership.md)öğrenin. SLA'ları sağlamak için bir veritabanı nda veya kapsayıcıda gereken minimum iş verime vardır ve her 100 RU/s için sağlanan iş bürünmesini 6 ABD doları artırabilir veya azaltabilirsiniz.

Şu anda hem veritabanı hem de kapsayıcı tabanlı iş ortası için minimum fiyat $24/month 'dir (en son bilgiler için [Azure Cosmos DB fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın. İş yükünüz birden çok kapsayıcı kullanıyorsa, veritabanı düzeyinde iş bölümü oluşturma düzeyi kullanılarak maliyet için en iyi duruma getirilebilir, çünkü veritabanı düzeyinde ki iş düzeyi oluşturma, bir veritabanında kapsayıcılar arasında iş düzeyini paylaşan herhangi bir sayıda kapsayıcıya sahip olmanızı sağlar. Aşağıdaki tabloda, farklı varlıklar için sağlanan iş maliyeti ve maliyetleri özetlenmiştir:

|**Varlık**  | **Minimum iş & maliyeti** |**Maliyet & ölçek artışlarını** |**Sağlama Kapsamı** |
|---------|---------|---------|-------|
|Database    | 400 RU/s ($24/ay)    | 100 RU/s ($6/ay)   |İş paylaşımı veritabanı için ayrılmıştır ve veritabanı içindeki kapsayıcılar tarafından paylaşılır |
|Kapsayıcı     | 400 RU/s ($24/ay)    | 100 RU/s ($6/ay)  |İş mesuliyonu belirli bir kapsayıcı için ayrılmıştır |

Önceki tabloda gösterildiği gibi, Azure Cosmos DB'deki minimum iş ortası 24 TL/ay fiyatla başlar. Üretim iş yüklerinizi desteklemek için minimum iş üretimiyle başlar ve zaman içinde ölçeklendirin, maliyetleriniz 6$/ay'lık artışlarla sorunsuz bir şekilde artacaktır. Azure Cosmos DB'deki fiyatlandırma modeli esnektir ve ölçeklendikçe veya küçültttünkçe fiyatta sorunsuz bir artış veya düşüş olur.

## <a name="try-azure-cosmos-db-for-free"></a>Azure Cosmos DB’yi ücretsiz deneyin 

Azure Cosmos DB, geliştiriciler için ücretsiz olarak çeşitli seçenekler sunar. Bu seçenekler şunlardır:

* **Azure ücretsiz hesabı**: Azure, ilk 30 gün boyunca 200 TL Azure kredisi ve 12 ay boyunca sınırlı miktarda ücretsiz hizmet sağlayan ücretsiz bir [katman](https://azure.microsoft.com/free/) sunar. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](../cost-management-billing/manage/avoid-charges-free-account.md). Azure Cosmos DB, Azure ücretsiz hesabının bir parçasıdır. Azure Cosmos DB için özel olarak bu ücretsiz hesap, tüm yıl boyunca 5 GB depolama alanı ve 400 RUs'luk garantili iş ortası sunar. 

* **Azure Cosmos DB'yi ücretsiz olarak deneyin**: Azure Cosmos DB, ücretsiz hesaplar için Azure Cosmos DB'yi deneyin'i kullanarak zaman sınırlı bir deneyim sunar. Bir Azure Cosmos DB hesabı oluşturabilir, veritabanı ve koleksiyonlar oluşturabilir ve Quickstarts ve öğreticileri kullanarak örnek bir uygulama çalıştırabilirsiniz. Örnek uygulamayı bir Azure hesabına abone olmadan veya kredi kartınızı kullanmadan çalıştırabilirsiniz. Azure [Cosmos DB'yi ücretsiz olarak deneyin,](https://azure.microsoft.com/try/cosmosdb/) hesabınızı birçok kez yenileme olanağı sağlayan Azure Cosmos DB tekliflerini bir ay boyunca deneyin.

* **Azure Cosmos DB emülatörü**: Azure Cosmos DB emülatörü, geliştirme amacıyla Azure Cosmos DB hizmetini taklit eden yerel bir ortam sağlar. Emülatör hiçbir ücret ödemeden ve bulut hizmetine yüksek sadakatle sunulur. Azure Cosmos DB emülatörunu kullanarak, bir Azure aboneliği oluşturmadan veya herhangi bir ücret ödemeden uygulamalarınızı yerel olarak geliştirebilir ve test edebilirsiniz. Üretime geçmeden önce emülatörkullanarak uygulamalarınızı yerel olarak geliştirebilirsiniz. Emülatöre karşı uygulamanın işlevselliğinden memnun olduktan sonra, buluttaki Azure Cosmos DB hesabını kullanmaya geçebilir ve maliyetten önemli ölçüde tasarruf edebilirsiniz. Emülatör hakkında daha fazla bilgi için daha fazla ayrıntı için geliştirme ve test makalesi [için Azure Cosmos DB'yi kullanma'ya](local-emulator.md) bakın.

## <a name="pricing-with-reserved-capacity"></a>Ayrılmış kapasite ile fiyatlandırma

Azure Cosmos DB [rezerve etme kapasitesi,](cosmos-db-reserved-capacity.md) Azure Cosmos DB kaynakları için bir yıl veya üç yıl için ön ödeme yaparak tasarruf etmenizi sağlar. Bir yıllık veya üç yıllık peşin taahhütlerle maliyetlerinizi önemli ölçüde azaltabilir ve normal fiyatlandırmaya kıyasla %20-65 arasında indirimden tasarruf edebilirsiniz. Azure Cosmos DB rezerve edilen kapasite, sağlanan iş başına (RU/lar) bir yıl veya üç yıllık bir süre için ön ödeme yaparak maliyetleri düşürmenize yardımcı olur ve sağlanan iş girdilerinde indirim elde elabilirsiniz. 

Ayrılmış kapasite faturalama indirimi sağlar ve Azure Cosmos DB kaynaklarınızın çalışma zamanı durumunu etkilemez. Ayrılmış kapasite, MongoDB, Cassandra, SQL, Gremlin ve Azure Tabloları'nı ve dünya çapındaki tüm bölgeleri içeren tüm API'ler için tutarlı bir şekilde kullanılabilir. Ayrılmış kapasite makalesi yle [Azure Cosmos DB kaynakları için Ön ödeme'de](cosmos-db-reserved-capacity.md) ayrılmış kapasite hakkında daha fazla bilgi edinebilir ve [Azure portalından](https://portal.azure.com/)ayrılmış kapasite satın alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB kaynaklarınızın maliyetlerini optimize etme hakkında daha fazla bilgi edinebilirsiniz:

* Geliştirme [ve test için Optimizasyon](optimize-dev-test.md) hakkında bilgi edinin
* [Azure Cosmos DB faturanızı anlama](understand-your-bill.md) hakkında daha fazla bilgi edinin
* [İş memat maliyetini optimize etme](optimize-cost-throughput.md) hakkında daha fazla bilgi edinin
* [Depolama maliyetini optimize etme](optimize-cost-storage.md) hakkında daha fazla bilgi edinin
* [Okuma ve yazma maliyetini optimize etme](optimize-cost-reads-writes.md) hakkında daha fazla bilgi edinin
* [Sorguların maliyetini optimize etme](optimize-cost-queries.md) hakkında daha fazla bilgi edinin
* [Çok bölgeli Cosmos hesaplarının maliyetini optimize etme](optimize-cost-regions.md) hakkında daha fazla bilgi edinin
* Azure [Cosmos DB rezerve etme kapasitesi](cosmos-db-reserved-capacity.md) hakkında bilgi edinin
* Azure [Cosmos DB Emülatörü](local-emulator.md) hakkında bilgi edinin
