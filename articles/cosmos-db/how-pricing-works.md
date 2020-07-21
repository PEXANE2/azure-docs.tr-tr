---
title: Azure Cosmos DB fiyatlandırma modeli
description: Bu makalede, Azure Cosmos DB fiyatlandırma modeli ve maliyet yönetimi ile maliyet planlamasını nasıl basitleştireceğinizi açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/14/2020
ms.openlocfilehash: d36b4fd433af716ebd97d88d05922d94bd74c309
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523545"
---
# <a name="pricing-model-in-azure-cosmos-db"></a>Azure Cosmos DB’de modeli fiyatlandırma

Azure Cosmos DB’nin fiyatlandırma modeli, maliyet yönetimini ve planlamasını basitleştirir. Azure Cosmos DB ile sağladığınız aktarım hızı ve tükettiğiniz depolama için ücret ödersiniz.

* **Sağlanan aktarım hızı**: [sağlanan aktarım hızı](how-to-choose-offer.md) (Ayrıca, ayrılmış aktarım hızı olarak da anılır), herhangi bir ölçekte yüksek İhtiyaç duyduğunuz aktarım hızını (RU/s) ve yapılandırılan aktarım hızını güvence altına almak için gereken kaynakları ayıran Azure Cosmos DB belirlersiniz. Verilen bir saat için sağlanan maksimum üretilen iş hacmi için saatlik olarak faturalandırılırsınız. El ile üretilen iş sağlamaya veya [Otomatik ölçeklendirmeyi](provision-throughput-autoscale.md)kullanmanıza de ihtiyacınız vardır.

   > [!NOTE]
   > Sağlanan aktarım hızı modeli, kaynakları kapsayıcınıza veya veritabanınıza ayırır, ancak herhangi bir iş yükü çalıştırmasa bile sağlanan aktarım hızı için ücretlendirilirsiniz.

* **Tüketilen depolama alanı**: belirli bir saat için veriler ve dizinler için tüketilen toplam depolama miktarı (GB) için sabit bir ücret faturalandırılırsınız.

Saniye başına [Istek birimi](request-units.md) (ru/s) olarak belirtilen sağlanan aktarım hızı, kapsayıcılardan veya veritabanlarına veri yazmanızı veya bunları yazmanıza izin verir. [Bir veritabanı ya da kapsayıcı üzerinde üretilen iş](set-throughput.md)sağlayabilirsiniz. İş yükünüzün gereksinimlerine bağlı olarak, aktarım hızını dilediğiniz zaman yukarı/aşağı ölçeklendirebilirsiniz. Azure Cosmos DB fiyatlandırması elastik ve bir veritabanı ya da kapsayıcıda yapılandırdığınız aktarım hızına göre orantılıdır. En düşük aktarım hızı ve depolama değerleri ve ölçek artımları, küçük ölçekte büyük ölçekli kapsayıcılara kadar tüm müşterilerin segmentlerine yönelik eksiksiz bir fiyat ve elaze yelpazesi sağlar. Her veritabanı veya kapsayıcı, en az 400 RU/sn ve GB cinsinden tüketilen depolama alanı 100 RU/sn birimlerinde sağlanan aktarım hızı için saatlik olarak faturalandırılır. Sağlanan aktarım hızının aksine, depolama, Tüketim esasına göre faturalandırılır. Diğer bir deyişle, herhangi bir depolamayı önceden ayırmanız gerekmez. Yalnızca kullandığınız depolama alanı için faturalandırılırsınız.

Daha fazla bilgi için [Azure Cosmos DB fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın ve [Azure Cosmos DB faturanızı](understand-your-bill.md)anlayın.

Azure Cosmos DB fiyatlandırma modeli tüm API 'lerde tutarlıdır. Daha fazla bilgi edinmek için bkz. [Azure Cosmos DB fiyatlandırma modelinin müşteriler için uygun maliyetli olması](total-cost-ownership.md). SLA 'Ları sağlamak için bir veritabanında veya kapsayıcıda gereken en düşük aktarım hızı vardır ve her 100 RU/sn için sağlanan aktarım hızını artırabilir veya azaltabilirsiniz.

Azure Cosmos DB hesabınızı ABD 'deki kamu dışı bir bölgeye dağıtırsanız, şu anda hem veritabanı için en düşük fiyat hem de kapsayıcı tabanlı aktarım hızı yaklaşık olarak 24/ay olur. Fiyatlandırma, kullandığınız bölgeye bağlı olarak farklılık gösterir, en son fiyatlandırma bilgileri için [Azure Cosmos DB fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın. İş yükünüz birden çok kapsayıcı kullanıyorsa, veritabanı düzeyinde aktarım hızı veritabanı düzeyinde aktarım hızı kullanılarak maliyet için iyileştirilebilir, çünkü veritabanı düzeyi işleme kapsayıcılar arasındaki aktarım hızını paylaşan bir veritabanında herhangi bir kapsayıcı kullanmanıza izin verir. Aşağıdaki tablo, sağlanan aktarım hızını ve farklı varlıkların maliyetlerini özetler:

|**Varlık**  | **En düşük aktarım hızı** |**Ölçek artışlarını** |**Sağlama kapsamı** |
|---------|---------|---------|-------|
|Veritabanı    | 400 RU/sn    | 100 RU/sn   |İşleme veritabanı için ayrılmıştır ve veritabanı içindeki kapsayıcılar tarafından paylaşılır |
|Kapsayıcı     | 400 RU/sn   | 100 RU/sn  |Aktarım hızı belirli bir kapsayıcı için ayrılmıştır |

Önceki tabloda gösterildiği gibi, Azure Cosmos DB en düşük aktarım hızı yaklaşık $24/aylık bir fiyattan başlar. En düşük aktarım hızı ile başlayıp, üretim iş yüklerinizi desteklemek için zaman içinde ölçeği artırdıysanız, maliyetleriniz, yaklaşık $6/ay artışlarla düzgün bir şekilde artacaktır. Fiyatlandırma, kullandığınız bölgeye bağlı olarak farklılık gösterir, en son fiyatlandırma bilgileri için [Azure Cosmos DB fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın. Azure Cosmos DB fiyatlandırma modeli esnektir ve ölçeği büyütme veya küçültme sırasında fiyata yönelik yumuşak bir artış veya azalma vardır.

## <a name="try-azure-cosmos-db-for-free"></a>Azure Cosmos DB’yi ücretsiz deneyin

Azure Cosmos DB, geliştiriciler için ücretsiz olarak çeşitli seçenekler sunar. Bu seçenekler şunlardır:

* **Azure Cosmos DB ücretsiz katman**: Azure Cosmos DB ücretsiz katman sayesinde çalışmaya başlamak, uygulamalarınızı geliştirmek ve test etmek, hatta küçük üretim iş yüklerini ücretsiz olarak çalıştırmak kolaylaşır. Hesapta ücretsiz katman etkinleştirildiğinde, hesap kullanım ömrü boyunca hesapta ilk 400 RU/sn ve 5 GB depolama alanını ücretsiz olarak alırsınız. Azure aboneliği başına en fazla bir ücretsiz katman hesabınız olabilir ve hesabı oluştururken kabul etmeniz gerekir. Başlamak için, [ücretsiz katman etkinken Azure Portal yeni bir hesap oluşturun](create-cosmosdb-resources-portal.md) veya [ARM şablonu](manage-sql-with-resource-manager.md#free-tier)kullanın.

* **Ücretsiz Azure hesabı**: Azure, 12 ay boyunca ilk 30 gün boyunca Azure kredileri ve sınırlı sayıda ücretsiz hizmet sunan ücretsiz bir $200 [Katman](https://azure.microsoft.com/free/) sunmaktadır. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](../cost-management-billing/manage/avoid-charges-free-account.md). Azure Cosmos DB Azure Ücretsiz hesabının bir parçasıdır. Özellikle Azure Cosmos DB için bu ücretsiz hesap, tüm yıl boyunca sağlanan aktarım hızı için 5 GB depolama ve 400 RU/sn olanakları sunar.

* **Ücretsiz Azure Cosmos DB deneyin**: Azure Cosmos DB ücretsiz hesaplar için TRY Azure Cosmos DB kullanarak zaman sınırlı bir deneyim sunar. Hızlı başlangıçları ve öğreticileri kullanarak bir Azure Cosmos DB hesabı oluşturabilir, veritabanı ve koleksiyonlar oluşturabilir ve örnek bir uygulama çalıştırabilirsiniz. Örnek uygulamayı bir Azure hesabına abone olmadan veya kredi kartınızı kullanarak çalıştırabilirsiniz. Hesabınızı dilediğiniz zaman yenileyebilme olanağı sunan bir ayda [ücretsiz Azure Cosmos DB için Azure Cosmos DB deneyin](https://azure.microsoft.com/try/cosmosdb/) .

* **Azure Cosmos DB öykünücü**: Azure Cosmos DB öykünücü, geliştirme amacıyla Azure Cosmos DB hizmetine taklit eden yerel bir ortam sağlar. Öykünücü, ücretsiz olarak ve bulut hizmeti için yüksek uygunluğa sahip olarak sunulur. Azure Cosmos DB öykünücüsü kullanarak, Azure aboneliği oluşturmadan veya herhangi bir ücret ödemeden uygulamalarınızı yerel olarak geliştirebilir ve test edebilirsiniz. Üretim ortamına geçmeden önce öykünücüyü yerel olarak kullanarak uygulamalarınızı geliştirebilirsiniz. Öykünücüdeki uygulamanın işlevselliğine karşı memnun olduktan sonra, Bulutta Azure Cosmos DB hesabı ile geçiş yapabilir ve maliyeti önemli ölçüde tasarruf edebilirsiniz. Öykünücü hakkında daha fazla bilgi için bkz. [geliştirme ve test için Azure Cosmos DB kullanma](local-emulator.md) , daha fazla ayrıntı için.

## <a name="pricing-with-reserved-capacity"></a>Ayrılmış kapasiteye sahip fiyatlandırma

Azure Cosmos DB [ayrılmış kapasite](cosmos-db-reserved-capacity.md) , bir yıl veya üç yılda Azure Cosmos DB kaynakları için ön ödeme yaparak paradan tasarruf etmenize yardımcı olur. Maliyetlerinizi bir yıllık veya üç yıllık ön taahhütte önemli ölçüde azaltabilir ve normal fiyatlandırmayla karşılaştırıldığında% 20-65 indirimle tasarruf edebilirsiniz. Azure Cosmos DB ayrılmış kapasite, bir yıllık veya üç yıllık bir dönem için sağlanan aktarım hızı (RU/s) için ön ödeme yaparak maliyetleri düşürmenize yardımcı olur ve sağlanan aktarım hızı hakkında bir indirim elde edersiniz. 

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
