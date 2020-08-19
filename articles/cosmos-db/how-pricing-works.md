---
title: Azure Cosmos DB fiyatlandırma modeli
description: Bu makalede, Azure Cosmos DB fiyatlandırma modeli ve maliyet yönetimi ile maliyet planlamasını nasıl basitleştireceğinizi açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/19/2020
ms.openlocfilehash: a992d240955f42ec030a84c887ba086ce92f9790
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88605252"
---
# <a name="pricing-model-in-azure-cosmos-db"></a>Azure Cosmos DB’de modeli fiyatlandırma

Azure Cosmos DB’nin fiyatlandırma modeli, maliyet yönetimini ve planlamasını basitleştirir. Azure Cosmos DB ile, gerçekleştirdiğiniz işlemler için ve verileriniz tarafından tüketilen depolama alanı için ödeme yaparsınız.

- **Veritabanı işlemleri**: veritabanı işlemlerinizin ücretlendirildiği yol, kullanmakta olduğunuz Azure Cosmos hesabının türüne bağlıdır.

  - **Sağlanan aktarım hızı**: [sağlanan aktarım hızı](set-throughput.md) (Ayrıca, ayrılmış aktarım hızı olarak da anılır), herhangi bir ölçekte yüksek Saniye başına [Istek birimi](request-units.md) (ru/s) cinsinden ihtiyacınız olan aktarım hızını ve yapılandırılan aktarım hızını güvence altına almak için gereken kaynakları ayıran Azure Cosmos DB belirlersiniz. [Bir veritabanı ya da kapsayıcı üzerinde üretilen iş](set-throughput.md)sağlayabilirsiniz. İş yükünüzün gereksinimlerine bağlı olarak, herhangi bir zamanda aktarım hızını yukarı/aşağı ölçeklendirebilir veya [Otomatik ölçeklendirme](provision-throughput-autoscale.md) kullanabilirsiniz (ancak, bir veritabanında veya SLA 'ların garantisi için bir kapsayıcıda gereken en düşük bir aktarım olmasına rağmen). Verilen bir saat için sağlanan maksimum üretilen iş hacmi için saatlik olarak faturalandırılırsınız.

   > [!NOTE]
   > Sağlanan aktarım hızı modeli, kaynakları kapsayıcınıza veya veritabanınıza ayırır, ancak herhangi bir iş yükü çalıştırmazsanız bile sağladığınız aktarım hızı için ücretlendirilirsiniz.

  - **Sunucusuz**: [sunucusuz](serverless.md) modda, Azure Cosmos hesabınızda kaynak oluştururken herhangi bir aktarım hızı sağlamanız gerekmez. Faturalama döneminizin sonunda, veritabanı işlemleriniz tarafından tüketilen Istek birimi miktarına göre faturalandırılırsınız.

- **Depolama**: belirli bir saat için verileriniz ve dizinlerinizin tükettiği toplam depolama miktarı (GB cinsinden) için sabit bir ücret üzerinden faturalandırılırsınız. Depolama, Tüketim esasına göre faturalandırılır, bu nedenle herhangi bir depolamayı önceden ayırmanız gerekmez. Yalnızca kullandığınız depolama alanı için faturalandırılırsınız.

Azure Cosmos DB fiyatlandırma modeli tüm API 'lerde tutarlıdır. Daha fazla bilgi için [Azure Cosmos DB fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/)bakın, [Azure Cosmos DB faturanızı](understand-your-bill.md) ve [Azure Cosmos DB fiyatlandırma modelinin müşteriler için uygun maliyetli olduğunu](total-cost-ownership.md)anlayın.

Azure Cosmos DB hesabınızı ABD 'deki kamu dışı bir bölgeye dağıtırsanız, sağlanan aktarım hızı modunda hem veritabanı hem de kapsayıcı tabanlı aktarım hızı için minimum bir fiyat vardır. Sunucusuz modda minimum fiyat yoktur. Fiyatlandırma, kullandığınız bölgeye bağlı olarak farklılık gösterir, en son fiyatlandırma bilgileri için [Azure Cosmos DB fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın.

## <a name="try-azure-cosmos-db-for-free"></a>Azure Cosmos DB’yi ücretsiz deneyin

Azure Cosmos DB, geliştiriciler için ücretsiz olarak çeşitli seçenekler sunar. Bu seçenekler şunlardır:

* **Azure Cosmos DB ücretsiz katman**: Azure Cosmos DB ücretsiz katman sayesinde çalışmaya başlamak, uygulamalarınızı geliştirmek ve test etmek, hatta küçük üretim iş yüklerini ücretsiz olarak çalıştırmak kolaylaşır. Hesapta ücretsiz katman etkinleştirildiğinde, hesap kullanım ömrü boyunca hesapta ilk 400 RU/sn ve 5 GB depolama alanını ücretsiz olarak alırsınız. Azure aboneliği başına en fazla bir ücretsiz katman hesabınız olabilir ve hesabı oluştururken kabul etmeniz gerekir. Başlamak için, [ücretsiz katman etkinken Azure Portal yeni bir hesap oluşturun](create-cosmosdb-resources-portal.md) veya [ARM şablonu](manage-sql-with-resource-manager.md#free-tier)kullanın.

* **Ücretsiz Azure hesabı**: Azure, 12 ay boyunca ilk 30 gün boyunca Azure kredileri ve sınırlı sayıda ücretsiz hizmet sunan ücretsiz bir $200 [Katman](https://azure.microsoft.com/free/) sunmaktadır. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](../cost-management-billing/manage/avoid-charges-free-account.md). Azure Cosmos DB Azure Ücretsiz hesabının bir parçasıdır. Özellikle Azure Cosmos DB için bu ücretsiz hesap, tüm yıl boyunca sağlanan aktarım hızı için 5 GB depolama ve 400 RU/sn olanakları sunar.

* **Ücretsiz Azure Cosmos DB deneyin**: Azure Cosmos DB ücretsiz hesaplar için TRY Azure Cosmos DB kullanarak zaman sınırlı bir deneyim sunar. Hızlı başlangıçları ve öğreticileri kullanarak bir Azure Cosmos DB hesabı oluşturabilir, veritabanı ve koleksiyonlar oluşturabilir ve örnek bir uygulama çalıştırabilirsiniz. Örnek uygulamayı bir Azure hesabına abone olmadan veya kredi kartınızı kullanarak çalıştırabilirsiniz. Hesabınızı dilediğiniz zaman yenileyebilme olanağı sunan bir ayda [ücretsiz Azure Cosmos DB için Azure Cosmos DB deneyin](https://azure.microsoft.com/try/cosmosdb/) .

* **Azure Cosmos DB öykünücü**: Azure Cosmos DB öykünücü, geliştirme amacıyla Azure Cosmos DB hizmetine taklit eden yerel bir ortam sağlar. Öykünücü, ücretsiz olarak ve bulut hizmeti için yüksek uygunluğa sahip olarak sunulur. Azure Cosmos DB öykünücüsü kullanarak, Azure aboneliği oluşturmadan veya herhangi bir ücret ödemeden uygulamalarınızı yerel olarak geliştirebilir ve test edebilirsiniz. Üretim ortamına geçmeden önce öykünücüyü yerel olarak kullanarak uygulamalarınızı geliştirebilirsiniz. Öykünücüdeki uygulamanın işlevselliğine karşı memnun olduktan sonra, Bulutta Azure Cosmos DB hesabı ile geçiş yapabilir ve maliyeti önemli ölçüde tasarruf edebilirsiniz. Öykünücü hakkında daha fazla bilgi için bkz. [geliştirme ve test için Azure Cosmos DB kullanma](local-emulator.md) , daha fazla ayrıntı için.

## <a name="pricing-with-reserved-capacity"></a>Ayrılmış kapasiteye sahip fiyatlandırma

Azure Cosmos DB [ayrılmış kapasite](cosmos-db-reserved-capacity.md) , bir yıl veya üç yılda Azure Cosmos DB kaynakları için ön ödeme yaparak sağlanan verimlilik modunu kullanırken tasarruf etmenize yardımcı olur. Maliyetlerinizi bir yıllık veya üç yıllık ön taahhütte önemli ölçüde azaltabilir ve normal fiyatlandırmayla karşılaştırıldığında% 20-65 indirimle tasarruf edebilirsiniz. Azure Cosmos DB ayrılmış kapasite, bir yıllık veya üç yıllık bir dönem için sağlanan aktarım hızı (RU/s) için ön ödeme yaparak maliyetleri düşürmenize yardımcı olur ve sağlanan aktarım hızı hakkında bir indirim elde edersiniz. 

Ayrılmış kapasite faturalama indirimi sağlar ve Azure Cosmos DB kaynaklarınızın çalışma zamanı durumunu etkilemez. Ayrılmış kapasite, MongoDB, Cassandra, SQL, Gremlin ve Azure tabloları ile dünya çapındaki tüm bölgeleri içeren tüm API 'Ler için tutarlı bir şekilde kullanılabilir. Ayrılmış kapasite hakkında daha fazla bilgi edinmek [için, ayrılmış kapasite ile Azure Cosmos DB kaynaklar Için ön ödeme](cosmos-db-reserved-capacity.md) yapın ve [Azure Portal](https://portal.azure.com/)ayrılan kapasiteyi satın alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde Azure Cosmos DB kaynaklarınızın maliyetlerini en iyi duruma getirme hakkında daha fazla bilgi edinebilirsiniz:

* [Geliştirme ve test Için iyileştirme](optimize-dev-test.md) hakkında bilgi edinin
* [Azure Cosmos DB Faturanızı Anlama](understand-your-bill.md) hakkında daha fazla bilgi edinin
* [Verimlilik maliyetini iyileştirme](optimize-cost-throughput.md) hakkında daha fazla bilgi edinin
* [Depolama maliyetini iyileştirme](optimize-cost-storage.md) hakkında daha fazla bilgi edinin
* [Okuma ve yazma maliyetlerini iyileştirme](optimize-cost-reads-writes.md) hakkında daha fazla bilgi edinin
* [Sorguların maliyetini En Iyi duruma getirme](optimize-cost-queries.md) hakkında daha fazla bilgi edinin
* [Çok bölgeli Cosmos hesaplarının maliyetini En Iyi duruma getirme](optimize-cost-regions.md) hakkında daha fazla bilgi edinin
* [Ayrılmış Azure Cosmos DB kapasitesi](cosmos-db-reserved-capacity.md) hakkında bilgi edinin
* [Azure Cosmos DB öykünücüsü](local-emulator.md) hakkında bilgi edinin
