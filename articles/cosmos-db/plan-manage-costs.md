---
title: Azure Cosmos DB maliyetlerini planlayın ve yönetin
description: Azure portal maliyet analizini kullanarak Azure Cosmos DB maliyetlerini nasıl planlayacağınızı ve yöneteceğinizi öğrenin.
author: SnehaGunda
ms.author: sngun
ms.custom: subject-cost-optimization
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/02/2020
ms.openlocfilehash: 42421f745759d9aee75b285c3fbc6ea7217ba5c0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85112710"
---
# <a name="plan-and-manage-costs-for-azure-cosmos-db"></a>Azure Cosmos DB maliyetlerini planlayın ve yönetin

Bu makalede, Azure Cosmos DB maliyetlerini nasıl planlayabileceğinizi ve yönetebileceğiniz açıklanır. İlk olarak, kaynakları eklemeden önce maliyetleri planlamaya yardımcı olması için Azure Cosmos DB kapasite hesaplayıcısını kullanın. Ardından, Azure kaynaklarını eklerken Tahmini maliyetleri gözden geçirebilirsiniz. Azure Cosmos DB kaynaklarını kullanmaya başladıktan sonra, bütçe ayarlamak ve maliyetleri izlemek için maliyet yönetimi özelliklerini kullanın. Ayrıca tahmin edilen maliyetleri gözden geçirebilir ve hareket etmek isteyebileceğiniz yerleri belirlemek için harcama eğilimlerini belirleyebilirsiniz.

Azure Cosmos DB maliyetlerinin yalnızca Azure faturanızda bulunan Aylık maliyetlerin yalnızca bir kısmı olduğunu anlayın. Diğer Azure hizmetlerini kullanıyorsanız, üçüncü taraf hizmetler de dahil olmak üzere Azure aboneliğinizde kullanılan tüm Azure hizmetleri ve kaynakları için faturalandırılırsınız. Bu makalede, Azure Cosmos DB maliyetlerinin nasıl planlanacağı ve yönetileceği açıklanmaktadır. Azure Cosmos DB maliyetlerini yönetme hakkında bilgi sahibi olduktan sonra, aboneliğinizde kullanılan tüm Azure hizmetlerinin maliyetlerini yönetmek için benzer yöntemler uygulayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Maliyet analizi, farklı türdeki Azure hesaplarını destekler. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](../cost-management-billing/costs/understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için Azure hesabınızda en azından okuma yetkisine sahip olmanız gerekir. Azure Maliyet Yönetimi verilerine erişim atama hakkında daha fazla bilgi için bkz. [Verilere erişim atama](../cost-management-billing/costs/assign-access-acm-data.md).

## <a name="review-estimated-costs-with-capacity-calculator"></a>Kapasite hesaplayıcısı ile Tahmini maliyetleri gözden geçirme

Azure Cosmos hesabında kaynakları oluşturmadan önce maliyetleri tahmin etmek için [Azure Cosmos DB kapasite hesaplayıcısını](https://cosmos.azure.com/capacitycalculator/) kullanın. Kapasite hesaplayıcı, iş yükünüzün gerekli aktarım hızını ve maliyetini tahmin etmek için kullanılır. Azure Cosmos veritabanlarınızı ve Kapsayıcılarınızı, iş yükünüz için doğru sağlanan aktarım hızı veya [Istek birimleri (ru/s)](request-units.md)ile yapılandırarak maliyeti ve performansı iyileştirmek için gereklidir. API türü, bölge sayısı, öğe boyutu, saniye başına okuma/yazma isteği, maliyet tahmini almak için depolanan toplam veri gibi ayrıntıları yazmanız gerekir. Kapasite Hesaplayıcı hakkında daha fazla bilgi için bkz. [tahmin](estimate-ru-with-capacity-planner.md) makalesi.

Aşağıdaki ekran görüntüsünde kapasite Hesaplayıcı kullanılarak aktarım hızı ve maliyet tahmini gösterilmektedir:

:::image type="content" source="./media/plan-manage-costs/capacity-calculator-cost-estimate.png" alt-text="Azure Cosmos DB kapasite hesaplayıcıda maliyet tahmini":::

## <a name="review-estimated-costs-from-the-azure-portal"></a>Azure portal Tahmini maliyetleri gözden geçirin

Azure portal Azure Cosmos DB kaynaklar oluştururken Tahmini maliyetleri görebilirsiniz. Maliyet tahminini gözden geçirmek için aşağıdaki adımları kullanın:

1. Azure portal oturum açın ve Azure Cosmos hesabınıza gidin.
1. **Veri Gezgini**gidin.
1. Grafik kapsayıcısı gibi yeni bir kapsayıcı oluşturun.
1. İş yükünüz için gereken aktarım hızını girin. Örneğin, 400 RU/s. Üretilen iş değerini girdikten sonra, fiyatlandırma tahminini aşağıdaki ekran görüntüsünde gösterildiği gibi görebilirsiniz:

   :::image type="content" source="./media/plan-manage-costs/cost-estimate-portal.png" alt-text="Azure portal maliyet tahmini":::

Azure aboneliğinizin harcama limiti varsa, Azure kredi miktarınızdan harcamanıza engel olur. Azure kaynaklarını oluştururken ve kullanırken kredileriniz kullanılır. Kredi sınırınıza ulaştığınızda, dağıttığınız kaynaklar bu fatura döneminin geri kalanı için devre dışı bırakılır. Kredi sınırınızı değiştiremezsiniz, ancak kaldırabilirsiniz. Harcama limitleri hakkında daha fazla bilgi için bkz. [Azure harcama limiti](../billing/billing-spending-limit.md).

## <a name="use-budgets-and-cost-alerts"></a>Bütçe ve maliyet uyarılarını kullanma

Maliyetleri yönetmek için [bütçeler](../cost-management/tutorial-acm-create-budgets.md) oluşturabilir ve paydaşları harcama anormallikleri ve fazla harcama riskleri ile ilgili otomatik olarak bilgilendiren uyarılar oluşturabilirsiniz. Uyarılar, bütçe ve maliyet eşiklerine kıyasla harcamaya göre belirlenir. Bütçeler ve uyarılar Azure abonelikleri ve kaynak grupları için oluşturulur, bu nedenle genel maliyet izleme stratejisinin bir parçası olarak faydalıdır. Ancak, maliyetleri daha yüksek bir düzeyde izlemek üzere tasarlandıklarından, tek tek Azure hizmeti maliyetlerini Azure Cosmos DB maliyeti gibi ayrı ayrı yönetmek için sınırlı işlevlere sahip olabilirler.

## <a name="monitor-costs"></a>Maliyetleri izleme

Azure Cosmos DB ile kaynakları kullanırken maliyetleriniz vardır. Kaynak kullanımı birim maliyetleri, zaman aralıklarına (saniye, dakika, saat ve gün) veya istek birimi kullanımına göre değişir. Azure Cosmos DB kullanımı başladığı anda, maliyetler ücretlendirilir ve bunları Azure portal [Maliyet Analizi](../cost-management/quick-acm-cost-analysis.md) bölmesinde görebilirsiniz.

Maliyet analizini kullandığınızda, grafik ve tablolardaki Azure Cosmos DB maliyetlerini farklı zaman aralıkları için görüntüleyebilirsiniz. Bazı örnekler güne, geçerli, önceki aya ve yıla göre yapılır. Ayrıca bütçeleri ve tahmin edilen maliyetlerden de maliyet görüntüleyebilirsiniz. Zamana göre daha uzun görünümlere geçiş yapmak, harcama eğilimlerini belirlemenize ve fazla harcama oluşmuş olabileceğini görmenizi sağlar. Bütçeleri oluşturduysanız, nerede aşıldığını da kolayca görebilirsiniz. Maliyet analizinde Azure Cosmos DB maliyetlerini görüntülemek için:

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. **Maliyet yönetimi + faturalandırma** penceresini açın, menüden **maliyet yönetimi** ' ni seçin ve ardından **Maliyet Analizi**' ni seçin. Daha sonra **kapsam** açılan listesinden belirli bir aboneliğin kapsamını değiştirebilirsiniz.

1. Varsayılan olarak, tüm hizmetlerin maliyeti ilk halka grafiğinde gösterilir. Grafikteki "Azure Cosmos DB" etiketli alanı seçin.

1. Azure Cosmos DB gibi tek bir hizmetin maliyetlerini daraltmak için, **Filtre Ekle** ' yi ve ardından **hizmet adı**' nı seçin. Sonra listeden **Azure Cosmos DB** öğesini seçin. Yalnızca Azure Cosmos DB maliyetlerini gösteren bir örnek aşağıda verilmiştir:
 
   :::image type="content" source="./media/plan-manage-costs/cost-analysis-pane.png" alt-text="Maliyet Analizi bölmesi ile maliyetleri izleme":::

Yukarıdaki örnekte, Şubat ayı için geçerli Azure Cosmos DB maliyeti görürsünüz. Grafiklerde Ayrıca konuma ve kaynak grubuna göre Azure Cosmos DB maliyetleri de bulunur.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB ' de fiyatlandırmanın nasıl çalıştığı hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Cosmos DB’de modeli fiyatlandırma](how-pricing-works.md)
* [Azure Cosmos DB’de sağlanan işlem hızını iyileştirme](optimize-cost-throughput.md)
* [Azure Cosmos DB'de sorgu maliyetini iyileştirme](optimize-cost-queries.md)
* [Depolama maliyetini en uygun Azure Cosmos DB](optimize-cost-storage.md)