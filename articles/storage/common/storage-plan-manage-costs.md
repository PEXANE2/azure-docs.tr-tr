---
title: Azure Blob depolama için maliyetleri planlayın ve yönetin
description: Azure portal ' de maliyet analizini kullanarak Azure Blob depolama için maliyetleri nasıl planlayacağınızı ve yöneteceğinizi öğrenin.
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 08/03/2020
ms.author: normesta
ms.subservice: common
ms.custom: subject-cost-optimization
ms.openlocfilehash: 75b464c140bfda6c3f3559d3bfdbe1e6bc2e7f24
ms.sourcegitcommit: 5a37753456bc2e152c3cb765b90dc7815c27a0a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87760745"
---
# <a name="plan-and-manage-costs-for-azure-blob-storage"></a>Azure Blob depolama için maliyetleri planlayın ve yönetin

Bu makale, Azure Blob depolama için maliyetleri planlayıp yönetmenize yardımcı olur. İlk olarak, Azure fiyatlandırma hesaplayıcısını kullanarak maliyetleri tahmin edin. Depolama hesabınızı oluşturduktan sonra, yalnızca ihtiyacınız olanlar için ödeme yapmak üzere hesabı iyileştirin. Maliyet yönetimi özelliklerini kullanarak bütçeleri ayarlayın ve maliyetleri izleyin. Ayrıca Tahmini maliyetleri gözden geçirebilir ve hareket etmek isteyebileceğiniz yerleri belirlemek için harcama eğilimlerini izleyebilirsiniz.

BLOB depolama maliyetlerinin yalnızca Azure faturanızda bulunan Aylık maliyetlerin yalnızca bir kısmı olduğunu aklınızda bulundurun. Bu makalede, BLOB depolama için maliyetleri tahmin etme ve yönetme işlemleri açıklanmakta olsa da, üçüncü taraf hizmetler de dahil olmak üzere Azure aboneliğiniz için kullanılan tüm Azure hizmetleri ve kaynakları için faturalandırılırsınız. BLOB depolama için maliyetleri yönetme hakkında bilgi sahibi olduktan sonra, aboneliğinizde kullanılan tüm Azure hizmetlerinin maliyetlerini yönetmek için benzer yöntemler uygulayabilirsiniz.

## <a name="estimate-costs"></a>Maliyetleri tahmin etme

Azure depolama hesabına veri aktarmaya başlamadan önce maliyetleri tahmin etmek için [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) ' nı kullanın.

1. [Azure Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/) sayfasında, **depolama hesapları** kutucuğunu seçin.

2. Sayfayı aşağı kaydırın ve tahminlerinizin **depolama hesapları** bölümünü bulun.

3. Açılır listelerden Seçenekler ' i seçin. 

   Bu açılan listelerin değerini değiştirirken maliyet tahmini değişir. Bu tahmin, üst köşede ve tahminin alt kısmında görünür. 
    
   ![Maliyet Analizi bölmesi ile maliyetleri izleme](media/storage-plan-manage-costs/price-calculator-storage-type.png)

   **Tür** açılır listesinin değerini değiştirirken, bu çalışma sayfasında görünen diğer seçenekler de değişir. Her bir seçeneğin ne olduğu ve bu seçeneklerin depolama ile ilgili işlemlerin fiyatını nasıl etkilediği hakkında daha fazla bilgi edinmek için **daha fazla bilgi** bölümündeki bağlantıları kullanın. 

4. Tahmine göre etkilerini görmek için kalan seçenekleri değiştirin.

## <a name="optimize-costs"></a>Maliyetleri iyileştirin

Maliyetleri azaltmak için bu seçenekleri kullanmayı göz önünde bulundurun. 

- Depolama kapasitesini ayır

- Verileri erişim katmanlarında düzenleme

- Erişim katmanları arasında verileri otomatik olarak taşıma

Bu bölüm, her bir seçeneği daha ayrıntılı bir şekilde ele alır. 

#### <a name="reserve-storage-capacity"></a>Depolama kapasitesini ayır

Azure Depolama ayrılmış kapasitesine sahip blob verileri için depolama maliyetlerine para tasarrufu sağlayabilirsiniz. Azure Depolama ayrılmış kapasitesi, bir yıl veya üç yıl için bir ayırmaya kayıt yaptığınızda, blok Blobları için kapasite ve standart depolama hesaplarında Azure Data Lake Storage 2. verileri için bir indirim sağlar. Ayırma, ayırma dönemi için sabit miktarda depolama kapasitesi sağlar. Azure Depolama ayrılmış kapasitesi, blok Blobları ve Azure Data Lake Storage 2. verileri için kapasite maliyetlerinizi önemli ölçüde azaltabilir. 

Daha fazla bilgi için bkz. [ayrılmış kapasiteye sahip BLOB depolama için maliyetleri iyileştirme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-reserved-capacity).

#### <a name="organize-data-into-access-tiers"></a>Verileri erişim katmanlarında düzenleme

En uygun maliyetli erişim katmanlarına blob verileri yerleştirerek maliyetleri azaltabilirsiniz. Veri kullanımı etrafında maliyetlerinizi iyileştirmek için tasarlanan üç katmandan seçim yapın. Örneğin, *sık kullanılan* katmanın depolama maliyeti daha yüksektir, ancak erişim maliyeti düşüktür. Bu nedenle, verilere sık sık erişmeyi planlıyorsanız, etkin katman en uygun maliyetli seçim olabilir. Verilere daha seyrek erişim planlıyorsanız, veri depolama maliyetini azaltırken, *soğuk* veya *Arşiv* katmanı en mantıklı hale gelebilir.    

Daha fazla bilgi edinmek için bkz. [Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers?tabs=azure-portal).

#### <a name="automatically-move-data-between-access-tiers"></a>Erişim katmanları arasında verileri otomatik olarak taşıma

En fazla paradan tasarruf etmek için verileri katmanlar arasında düzenli aralıklarla taşımak için yaşam döngüsü yönetimi ilkelerini kullanın. Bu ilkeler, belirttiğiniz kuralları kullanarak verileri öğesine taşıyabilir. Örneğin, blob 90 gün içinde değiştirilmediyse Blobları arşiv katmanına taşınan bir kural oluşturabilirsiniz. Verilerinizin erişim katmanını ayarlayarak ilkeler oluşturarak, gereksinimlerinize uygun en az maliyetli depolama seçeneklerini tasarlayabilirsiniz.

Daha fazla bilgi için bkz [. Azure Blob depolama yaşam döngüsünü yönetme](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts?tabs=azure-portal)

## <a name="create-budgets"></a>Bütçe oluşturma

Maliyetleri yönetmek için [bütçeler](../../cost-management-billing/costs/tutorial-acm-create-budgets.md) oluşturabilir ve paydaşları harcama anormallikleri ve fazla harcama riskleri ile ilgili otomatik olarak bilgilendiren uyarılar oluşturabilirsiniz. Uyarılar, bütçe ve maliyet eşiklerine kıyasla harcamaya göre belirlenir. Bütçeler ve uyarılar Azure abonelikleri ve kaynak grupları için oluşturulur, bu nedenle genel maliyet izleme stratejisinin bir parçası olarak faydalıdır. Ancak, maliyetleri daha yüksek bir düzeyde izlemek üzere tasarlandıklarından Azure depolama maliyeti gibi ayrı Azure hizmeti maliyetlerini yönetmek için sınırlı işlevlere sahip olabilirler.

## <a name="monitor-costs"></a>Maliyetleri izleme

Azure depolama ile Azure kaynaklarını kullanırken maliyetleriniz vardır. Kaynak kullanımı birim maliyetleri, zaman aralıklarına (saniye, dakika, saat ve gün) veya birim kullanımı (bayt, megabayt vb.) göre farklılık gösterir. Azure depolama 'nın kullanımı başladığı anda maliyetler tahakkuk edilir. Maliyetleri, Azure portal [Maliyet Analizi](../../cost-management-billing/costs/quick-acm-cost-analysis.md) bölmesinde görebilirsiniz.

Maliyet analizini kullandığınızda, Azure depolama maliyetlerini grafik ve tablolardaki farklı zaman aralıkları için görüntüleyebilirsiniz. Bazı örnekler güne, geçerli ve önceki aya ve yıla göre yapılır. Ayrıca bütçeleri ve tahmin edilen maliyetlerden de maliyet görüntüleyebilirsiniz. Zamana göre daha uzun görünümlere geçiş yapmak, harcama eğilimlerini belirlemenize ve fazla harcama oluşmuş olabileceğini görmenizi sağlar. Bütçeleri oluşturduysanız, nerede aşıldığını da kolayca görebilirsiniz.

>[!NOTE]
> Maliyet analizi, farklı türdeki Azure hesaplarını destekler. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](../../cost-management-billing/costs/understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için Azure hesabınızda en azından okuma yetkisine sahip olmanız gerekir. Azure Maliyet Yönetimi verilerine erişim atama hakkında daha fazla bilgi için bkz. [Verilere erişim atama](../../cost-management-billing/costs/assign-access-acm-data.md).

Azure depolama maliyetlerini maliyet analizi 'nde görüntülemek için:

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. **Maliyet yönetimi + faturalandırma** penceresini açın, menüden **maliyet yönetimi** ' ni seçin ve ardından **Maliyet Analizi**' ni seçin. Daha sonra **kapsam** açılan listesinden belirli bir aboneliğin kapsamını değiştirebilirsiniz.

   ![Maliyet Analizi bölmesi ile maliyetleri izleme](./media/storage-plan-manage-costs/cost-analysis-pane.png)

4. Yalnızca Azure Storage maliyetlerini görüntülemek için **Filtre Ekle** ' yi ve ardından **hizmet adı**' nı seçin. Sonra listeden **depolama** ' yı seçin. 

   Yalnızca Azure depolama 'nın maliyetlerini gösteren bir örnek aşağıda verilmiştir:

   ![Maliyet Analizi bölmesi ile depolama maliyetlerini izleme](./media/storage-plan-manage-costs/cost-analysis-pane-storage.png)

Önceki örnekte, hizmet için geçerli maliyeti görürsünüz. Azure bölgeleri (konumlar) ve kaynak grubuna göre maliyetler de görüntülenir. Diğer filtreleri de ekleyebilirsiniz (örneğin: belirli depolama hesaplarının maliyetlerini görmek için bir filtre).

## <a name="next-steps"></a>Sonraki adımlar

[Maliyet Analizi](../../cost-management-billing/costs/quick-acm-cost-analysis.md)ile maliyetleri yönetme hakkında daha fazla bilgi edinin.

Fiyatlandırma 'nın Azure depolama ile nasıl çalıştığı hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure depolama genel bakış fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/)
- [Ayrılmış kapasite ile Blob depolama maliyetlerini iyileştirme](../blobs/storage-blob-reserved-capacity.md)
