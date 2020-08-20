---
title: Azure Cosmos DB maliyetlerini planlayın ve yönetin
description: Azure portal maliyet analizini kullanarak Azure Cosmos DB maliyetlerini nasıl planlayacağınızı ve yöneteceğinizi öğrenin.
author: SnehaGunda
ms.author: sngun
ms.custom: subject-cost-optimization
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/19/2020
ms.openlocfilehash: 7f0a8fcb841399eb910f5f043cc75ddad037ee30
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88606866"
---
# <a name="plan-and-manage-costs-for-azure-cosmos-db"></a>Azure Cosmos DB maliyetlerini planlayın ve yönetin

Bu makalede Azure Cosmos DB maliyetlerini nasıl planlayabileceğinizi ve yönetebileceğiniz açıklanmaktadır:

- Herhangi bir kaynak oluşturmadan önce maliyetinizi tahmin edin
- Kaynaklarınızı kullanmaya başladığınızda Tahmini maliyetleri gözden geçirin
- Maliyet yönetimi özelliklerini kullanarak bütçeleri ayarlama ve maliyetleri izleme
- Tahmin edilen maliyetleri gözden geçirin ve hareket etmek isteyebileceğiniz yerleri ortaya çıkarmak için harcama eğilimlerini tanımla

Azure Cosmos DB maliyetlerinin yalnızca Azure faturanızda bulunan Aylık maliyetlerin yalnızca bir kısmı olduğunu anlayın. Diğer Azure hizmetlerini kullanıyorsanız, üçüncü taraf hizmetler de dahil olmak üzere Azure aboneliğinizde kullanılan tüm Azure hizmetleri ve kaynakları için faturalandırılırsınız. Bu makalede, Azure Cosmos DB maliyetlerinin nasıl planlanacağı ve yönetileceği açıklanmaktadır. Azure Cosmos DB maliyetlerini yönetme hakkında bilgi sahibi olduktan sonra, aboneliğinizde kullanılan tüm Azure hizmetlerinin maliyetlerini yönetmek için benzer yöntemler uygulayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Maliyet analizi, farklı türdeki Azure hesaplarını destekler. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](../cost-management-billing/costs/understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için Azure hesabınızda en azından okuma yetkisine sahip olmanız gerekir. Azure Maliyet Yönetimi verilerine erişim atama hakkında daha fazla bilgi için bkz. [Verilere erişim atama](../cost-management-billing/costs/assign-access-acm-data.md).

## <a name="provisioned-throughput-or-serverless"></a>Sağlanan aktarım hızı veya sunucusuz

Azure Cosmos DB iki tür kapasite modunu destekler: [sağlanan aktarım hızı](set-throughput.md) ve [sunucusuz](serverless.md). Azure Cosmos DB kullanımınız için ücretlendirilebilecek şekilde, bu iki mod arasında çok büyük farklılık gösterir. bu sayede iş yükünüz için en uygun olanı seçmeniz önemlidir. Bu seçimi yapma hakkında yönergeler ve öneriler için [sağlanan aktarım hızı ve sunucusuz makale arasında seçim](throughput-serverless.md) yapma bölümüne bakın.

## <a name="estimating-provisioned-throughput-costs-with-capacity-calculator"></a>Kapasite hesaplayıcısı ile sağlanan üretilen iş maliyetleri tahmin ediliyor

Sağlanan aktarım hızı modunda Azure Cosmos DB kullanmayı planlıyorsanız, kaynakları bir Azure Cosmos hesabında oluşturmadan önce maliyetleri tahmin etmek için [Azure Cosmos DB kapasite hesaplayıcısını](https://cosmos.azure.com/capacitycalculator/) kullanın. Kapasite hesaplayıcı, iş yükünüzün gerekli aktarım hızını ve maliyetini tahmin etmek için kullanılır. Azure Cosmos veritabanlarınızı ve Kapsayıcılarınızı, iş yükünüz için doğru sağlanan aktarım hızı veya [Istek birimleri (ru/s)](request-units.md)ile yapılandırarak maliyeti ve performansı iyileştirmek için gereklidir. API türü, bölge sayısı, öğe boyutu, saniye başına okuma/yazma isteği, maliyet tahmini almak için depolanan toplam veri gibi ayrıntıları yazmanız gerekir. Kapasite Hesaplayıcı hakkında daha fazla bilgi için bkz. [tahmin](estimate-ru-with-capacity-planner.md) makalesi.

Aşağıdaki ekran görüntüsünde kapasite Hesaplayıcı kullanılarak aktarım hızı ve maliyet tahmini gösterilmektedir:

:::image type="content" source="./media/plan-manage-costs/capacity-calculator-cost-estimate.png" alt-text="Azure Cosmos DB kapasite hesaplayıcıda maliyet tahmini":::

## <a name="estimating-serverless-costs"></a>Sunucusuz maliyetler tahmin ediliyor

Azure Cosmos DB sunucusuz modda kullanmayı planlıyorsanız, aylık olarak kaç [Istek birimi](request-units.md) ve GB depolama alanı kullanacağınızı tahmin etmeniz gerekir. Bir ayda verilecek veritabanı işlemlerinin sayısını değerlendirerek gereken Istek birimi miktarını tahmin edebilir ve bunların tutarını karşılık gelen RU maliyetlerine göre çarpmanız gerekir. Aşağıdaki tabloda, yaygın veritabanı işlemlerine yönelik tahmini RU ücretleri listelenmektedir:

| İşlem | Tahmini maliyet | Notlar |
| --- | --- | --- |
| Öğe oluşturma | 5 ru | Dizin için 5 ' ten az özelliği olan 1 KB 'lık öğe için Ortalama maliyet |
| Öğeyi güncelleştirme | 10 RU | Dizin için 5 ' ten az özelliği olan 1 KB 'lık öğe için Ortalama maliyet |
| Tek bir öğeyi KIMLIĞE ve bölüm anahtarına göre oku (nokta-oku) | 1 RU | 1 KB 'lık öğe için Ortalama maliyet |
| Öğeyi silme | 5 ru | |
| Sorgu yürütme | 10 RU | [Dizin oluşturmanın](index-overview.md) tam avantajlarından yararlanan ve 100 sonucu veya daha az döndüren bir sorgu için Ortalama maliyet |

> [!IMPORTANT] 
> Yukarıdaki tablodaki notlara dikkat edin. Operasyonlarınızın gerçek maliyetlerinin daha doğru bir şekilde tahmin etmek için [Azure Cosmos öykünücüsünü](local-emulator.md) kullanabilir ve [OPERASYONLARıNıZıN tam ru maliyetini ölçebilirsiniz](find-request-unit-charge.md). Azure Cosmos öykünücüsü sunucusuz 'yi desteklemediğinden, veritabanı işlemleri için standart RU ücreti rapor edebilir ve bu tahmin için kullanılabilir.

Bir ay boyunca tükettiğiniz toplam Istek birimi ve GB depolama alanı sayısını hesapladıktan sonra, aşağıdaki formül maliyet tahmininizi döndürür: **([istek birimi sayısı]/1.000.000 * $0,25) + ([GB depolama] * $0,25)**.

> [!NOTE]
> Önceki örnekte gösterilen maliyetler yalnızca tanıtım amaçlıdır. En son fiyatlandırma bilgileri için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın.

## <a name="review-estimated-costs-from-the-azure-portal"></a>Azure portal Tahmini maliyetleri gözden geçirin

Azure portal Azure Cosmos DB kaynaklarını kullanmaya başladığınızda Tahmini maliyetleri görebilirsiniz. Maliyet tahminini gözden geçirmek için aşağıdaki adımları kullanın:

1. Azure portal oturum açın ve Azure Cosmos hesabınıza gidin.
1. **Genel bakış** bölümüne gidin.
1. Alttaki **Maliyet** grafiğini kontrol edin. Bu grafik, yapılandırılabilir bir dönemdeki geçerli maliyetinizi gösterir:
1. Grafik kapsayıcısı gibi yeni bir kapsayıcı oluşturun.
1. İş yükünüz için gereken aktarım hızını girin. Örneğin, 400 RU/s. Üretilen iş değerini girdikten sonra, fiyatlandırma tahminini aşağıdaki ekran görüntüsünde gösterildiği gibi görebilirsiniz:

   :::image type="content" source="./media/plan-manage-costs/cost-estimate-portal.png" alt-text="Azure portal maliyet tahmini":::

## <a name="use-budgets-and-cost-alerts"></a>Bütçe ve maliyet uyarılarını kullanma

Maliyetleri yönetmek için [bütçeler](../cost-management/tutorial-acm-create-budgets.md) oluşturabilir ve paydaşları harcama anormallikleri ve fazla harcama riskleri ile ilgili otomatik olarak bilgilendiren uyarılar oluşturabilirsiniz. Uyarılar, bütçe ve maliyet eşiklerine kıyasla harcamaya göre belirlenir. Bütçeler ve uyarılar Azure abonelikleri ve kaynak grupları için oluşturulur, bu nedenle genel maliyet izleme stratejisinin bir parçası olarak faydalıdır. Ancak, maliyetleri daha yüksek bir düzeyde izlemek üzere tasarlandıklarından, tek tek Azure hizmeti maliyetlerini Azure Cosmos DB maliyeti gibi ayrı ayrı yönetmek için sınırlı işlevlere sahip olabilirler.

Azure aboneliğinizin harcama limiti varsa, Azure kredi miktarınızdan harcamanıza engel olur. Azure kaynaklarını oluştururken ve kullanırken kredileriniz kullanılır. Kredi sınırınıza ulaştığınızda, dağıttığınız kaynaklar bu fatura döneminin geri kalanı için devre dışı bırakılır. Kredi sınırınızı değiştiremezsiniz, ancak kaldırabilirsiniz. Harcama limitleri hakkında daha fazla bilgi için bkz. [Azure harcama limiti](../billing/billing-spending-limit.md).

## <a name="monitor-costs"></a>Maliyetleri izleme

Azure Cosmos DB ile kaynakları kullanırken maliyetleriniz vardır. Kaynak kullanımı birim maliyetleri, zaman aralıklarına (saniye, dakika, saat ve gün) veya istek birimi kullanımına göre değişir. Azure Cosmos DB kullanımı başladığı anda, maliyetler ücretlendirilir ve bunları Azure portal [Maliyet Analizi](../cost-management/quick-acm-cost-analysis.md) bölmesinde görebilirsiniz.

Maliyet analizini kullandığınızda, grafik ve tablolardaki Azure Cosmos DB maliyetlerini farklı zaman aralıkları için görüntüleyebilirsiniz. Bazı örnekler güne, geçerli, önceki aya ve yıla göre yapılır. Ayrıca bütçeleri ve tahmin edilen maliyetlerden de maliyet görüntüleyebilirsiniz. Zamana göre daha uzun görünümlere geçiş yapmak, harcama eğilimlerini belirlemenize ve fazla harcama oluşmuş olabileceğini görmenizi sağlar. Bütçeleri oluşturduysanız, nerede aşıldığını da kolayca görebilirsiniz. Maliyet analizinde Azure Cosmos DB maliyetlerini görüntülemek için:

1. [Azure portal](https://portal.azure.com) oturum açın.

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