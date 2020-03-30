---
title: Azure Cosmos DB için maliyetleri planlama ve yönetme
description: Azure portalında maliyet çözümlemesi kullanarak Azure Cosmos DB'nin maliyetlerini nasıl planlayıp yöneteceklerini öğrenin.
author: SnehaGunda
ms.author: sngun
ms.custom: subject-cost-optimization
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/02/2020
ms.openlocfilehash: cf259dfa0b00393cf0fee259b199527569ba1792
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77152593"
---
# <a name="plan-and-manage-costs-for-azure-cosmos-db"></a>Azure Cosmos DB için maliyetleri planlama ve yönetme

Bu makalede, Azure Cosmos DB'nin maliyetlerini nasıl planlayıp yönetebileceğiniz açıklanmaktadır. İlk olarak, herhangi bir kaynak eklemeden önce maliyetlerin planlanmasına yardımcı olmak için Azure Cosmos DB kapasite hesaplayıcısını kullanırsınız. Ardından, Azure kaynaklarını ekledikçe, tahmini maliyetleri gözden geçirebilirsiniz. Azure Cosmos DB kaynaklarını kullanmaya başladıktan sonra, bütçeleri ayarlamak ve maliyetleri izlemek için maliyet yönetimi özelliklerini kullanın. Ayrıca, öngörülen maliyetleri gözden geçirebilir ve harekete geçirmek isteyebileceğiniz alanları belirlemek için harcama eğilimlerini belirleyebilirsiniz.

Azure Cosmos DB maliyetlerinin Azure faturanızdaki aylık maliyetlerin yalnızca bir kısmı olduğunu anlayın. Diğer Azure hizmetlerini kullanıyorsanız, üçüncü taraf hizmetleri de dahil olmak üzere Azure aboneliğinizde kullanılan tüm Azure hizmetleri ve kaynakları için faturalandırılırsınız. Bu makalede, Azure Cosmos DB maliyetlerinin nasıl planlanıp yönetilen bir şekilde planlanması ve yönetilmesi açıklanmaktadır. Azure Cosmos DB'nin maliyetlerini yönetmeyi tanıdıktan sonra, aboneliğinizde kullanılan tüm Azure hizmetlerinin maliyetlerini yönetmek için benzer yöntemler uygulayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Maliyet analizi, farklı türdeki Azure hesaplarını destekler. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](../cost-management-billing/costs/understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için Azure hesabınızda en azından okuma yetkisine sahip olmanız gerekir. Azure Maliyet Yönetimi verilerine erişim atama hakkında daha fazla bilgi için bkz. [Verilere erişim atama](../cost-management-billing/costs/assign-access-acm-data.md).

## <a name="review-estimated-costs-with-capacity-calculator"></a>Kapasite hesaplayıcısı ile tahmini maliyetleri gözden geçirme

Azure Cosmos hesabında kaynakları oluşturmadan önce maliyetleri tahmin etmek için [Azure Cosmos DB kapasite hesaplayıcısını](https://cosmos.azure.com/capacitycalculator/) kullanın. Kapasite hesaplayıcısı, iş yükünüzün gerekli iş maliyeti ve maliyetinin tahminini almak için kullanılır. Azure Cosmos veritabanlarınızı ve kapsayıcılarınızı doğru miktarda sağlanan iş yükü veya [İstek Birimleri (RU/lar)](request-units.md)ile iş yükünüz için yapılandırmak, maliyet ve performansı optimize etmek için gereklidir. Maliyet tahmini almak için API türü, bölge sayısı, madde boyutu, saniyede okuma/yazma istekleri, depolanan toplam veri gibi ayrıntıları girmeniz gerekir. Kapasite hesaplayıcısı hakkında daha fazla bilgi edinmek için [tahmin](estimate-ru-with-capacity-planner.md) makalesine bakın.

Aşağıdaki ekran görüntüsü, kapasite hesaplayıcısını kullanarak iş ve maliyet tahminini gösterir:

![Azure Cosmos DB kapasite hesaplayıcısında maliyet tahmini](./media/plan-manage-costs/capacity-calculator-cost-estimate.png)

## <a name="review-estimated-costs-from-the-azure-portal"></a>Azure portalındaki tahmini maliyetleri gözden geçirme

Azure portalından Azure Cosmos DB kaynaklarını oluştururken, tahmini maliyetleri görebilirsiniz. Maliyet tahminini gözden geçirmek için aşağıdaki adımları kullanın:

1. Azure portalında oturum açın ve Azure Cosmos hesabınıza gidin.
1. **Veri Gezgini'ne**gidin.
1. Grafik kapsayıcısı gibi yeni bir kapsayıcı oluşturun.
1. 400 RU/s gibi iş yükünüz için gereken iş buzunu girdi. Girdi değerini girdikten sonra, fiyatlandırma tahminini aşağıdaki ekran görüntüsünde gösterildiği gibi görebilirsiniz:

   ![Azure portalında maliyet tahmini](./media/plan-manage-costs/cost-estimate-portal.png)

Azure aboneliğinizin bir harcama sınırı varsa, Azure kredi tutarınızın üzerinde harcama nızı engeller. Azure kaynaklarını oluşturur ken ve kullanırken, kredileriniz kullanılır. Kredi limitinize ulaştığında, dağıttığınız kaynaklar bu fatura döneminin geri kalanı için devre dışı bırakılır. Kredi limitinizi değiştiremezsiniz, ancak kaldırabilirsiniz. Harcama sınırları hakkında daha fazla bilgi için [Azure harcama sınırına](../billing/billing-spending-limit.md)bakın.

## <a name="use-budgets-and-cost-alerts"></a>Bütçe ve maliyet uyarılarını kullanma

Maliyetleri yönetmek için [bütçeler](../cost-management/tutorial-acm-create-budgets.md) oluşturabilir ve paydaşları harcama anormallikleri ve fazla harcama riskleri ile ilgili otomatik olarak bilgilendiren uyarılar oluşturabilirsiniz. Uyarılar, bütçe ve maliyet eşiklerine kıyasla harcamaya göre belirlenir. Azure abonelikleri ve kaynak grupları için bütçeler ve uyarılar oluşturulur, bu nedenle genel bir maliyet izleme stratejisinin bir parçası olarak yararlıdırlar. Ancak, maliyetleri daha yüksek bir düzeyde izlemek üzere tasarlandıkları için Azure Cosmos DB maliyeti gibi tek tek Azure hizmet maliyetlerini yönetmek için sınırlı işlevleri olabilir.

## <a name="monitor-costs"></a>Maliyetleri izleme

Azure Cosmos DB ile kaynakları kullandığınızda, maliyete maruz kalmaktadırsınız. Kaynak kullanım birimi maliyetleri zaman aralıklarına (saniye, dakika, saat ve gün) veya istek birimi kullanımına göre değişir. Azure Cosmos DB'nin kullanımı başlar başlamaz, maliyetler tahakkuk ettirilir ve bunları Azure portalındaki [maliyet analizi](../cost-management/quick-acm-cost-analysis.md) bölmesinde görebilirsiniz.

Maliyet çözümlemesi kullandığınızda, farklı zaman aralıkları için grafiklerde ve tablolarda Azure Cosmos DB maliyetlerini görüntüleyebilirsiniz. Bazı örnekler güne, geçerliye, önceki aya ve yıla göre dir. Maliyetleri bütçelere ve öngörülen maliyetlere göre de görüntüleyebilirsiniz. Zaman içinde daha uzun görünümlere geçmek, harcama eğilimlerini belirlemenize ve aşırı harcamanın nerede oluşmuş olabileceğini görmenize yardımcı olabilir. Bütçeler oluşturduysanız, bunların nerede aşıldığını da kolayca görebilirsiniz. Maliyet analizinde Azure Cosmos DB maliyetlerini görüntülemek için:

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Maliyet **Yönetimi + Faturalandırma** penceresini açın, menüden **Maliyet yönetimini** seçin ve ardından **Maliyet çözümlemesi'ni**seçin. Daha sonra **Kapsam** açılır tarihinden belirli bir aboneliğin kapsamını değiştirebilirsiniz.

1. Varsayılan olarak, tüm hizmetlerin maliyeti ilk donut grafiğinde gösterilir. Grafikte "Azure Cosmos DB" etiketli alanı seçin.

1. Azure Cosmos DB gibi tek bir hizmetin maliyetlerini daraltmak için **Filtre Ekle'yi** ve ardından **Hizmet adını**seçin. Ardından, listeden **Azure Cosmos DB'yi** seçin. Aşağıda, sadece Azure Cosmos DB'nin maliyetlerini gösteren bir örnek verilmiştir:
 
   ![Maliyet Analizi bölmesiyle maliyetleri izleme](./media/plan-manage-costs/cost-analysis-pane.png)

Önceki örnekte, Azure Cosmos DB'nin Şubat ayı için geçerli maliyetini görürsünüz. Grafikler ayrıca konuma ve kaynak grubuna göre Azure Cosmos DB maliyetlerini de içerir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de fiyatlandırmanın nasıl çalıştığı hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Cosmos DB’de modeli fiyatlandırma](how-pricing-works.md)
* [Azure Cosmos DB’de sağlanan işlem hızını iyileştirme](optimize-cost-throughput.md)
* [Azure Cosmos DB'de sorgu maliyetini iyileştirme](optimize-cost-queries.md)
* [Azure Cosmos DB'de depolama maliyetini optimize edin](optimize-cost-storage.md)