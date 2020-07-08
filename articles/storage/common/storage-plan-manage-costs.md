---
title: Azure depolama için maliyetleri planlayın ve yönetin
description: Azure portal ' de maliyet analizini kullanarak Azure depolama maliyetlerini nasıl planlayacağınızı ve yöneteceğinizi öğrenin.
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: normesta
ms.subservice: common
ms.custom: subject-cost-optimization
ms.openlocfilehash: aa0b789b31f50c8b1ccf5450700874a02ad4664c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78304530"
---
# <a name="plan-and-manage-costs-for-azure-storage"></a>Azure depolama için maliyetleri planlayın ve yönetin

Bu makalede, Azure depolama için maliyetleri nasıl planlayabileceğinizi ve yöneteceğiniz açıklanmaktadır. İlk olarak, kaynak eklemeden önce depolama maliyetlerini planlamaya yardımcı olması için Azure Fiyatlandırma hesaplayıcısı ' nı kullanırsınız. Azure depolama kaynaklarını kullanmaya başladıktan sonra, bütçe ayarlamak ve maliyetleri izlemek için maliyet yönetimi özelliklerini kullanın. Ayrıca tahmin edilen maliyetleri gözden geçirebilir ve harcama eğilimlerini izleyip, işlem yapmak isteyebileceğiniz bölgeleri belirleyebilirsiniz.

Azure depolama için maliyetlerin yalnızca Azure faturanızda bulunan Aylık maliyetlerin yalnızca bir kısmı olduğunu aklınızda bulundurun. Bu makalede, Azure depolama için maliyetlerin nasıl planlanacağı ve yönetileceği açıklanmakta olsa da, üçüncü taraf hizmetler de dahil olmak üzere Azure aboneliğiniz için kullanılan tüm Azure hizmetleri ve kaynakları için faturalandırılırsınız. Azure depolama 'nın maliyetlerini yönetme hakkında bilgi sahibi olduktan sonra, aboneliğinizde kullanılan tüm Azure hizmetlerinin maliyetlerini yönetmek için benzer yöntemler uygulayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Maliyet analizi, farklı türdeki Azure hesaplarını destekler. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](../../cost-management-billing/costs/understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için Azure hesabınızda en azından okuma yetkisine sahip olmanız gerekir. Azure Maliyet Yönetimi verilerine erişim atama hakkında daha fazla bilgi için bkz. [Verilere erişim atama](../../cost-management-billing/costs/assign-access-acm-data.md).

## <a name="estimate-costs-before-creating-an-azure-storage-account"></a>Azure depolama hesabı oluşturmadan önce maliyetleri tahmin etme

Azure depolama hesabına veri aktarmaya başlamadan önce maliyetleri tahmin etmek için [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) ' nı kullanın.

1. [Azure Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/) sayfasında, **depolama hesapları** kutucuğunu seçin.

2. Sayfayı aşağı kaydırın ve tahminlerinizin **depolama hesapları** bölümünü bulun.

3. Açılır listelerden Seçenekler ' i seçin. 

   Bu açılan listelerin değerini değiştirirken maliyet tahmini değişir. Bu tahmin, üst köşede ve tahminin alt kısmında görünür. 
    
   ![Maliyet Analizi bölmesi ile maliyetleri izleme](media/storage-plan-manage-costs/price-calculator-storage-type.png)

   **Tür** açılır listesinin değerini değiştirirken, bu çalışma sayfasında görünen diğer seçenekler de değişir. Her bir seçeneğin ne olduğu ve bu seçeneklerin depolama ile ilgili işlemlerin fiyatını nasıl etkilediği hakkında daha fazla bilgi edinmek için **daha fazla bilgi** bölümündeki bağlantıları kullanın. 

4. Tahmine göre etkilerini görmek için kalan seçenekleri değiştirin.

## <a name="use-budgets-and-cost-alerts"></a>Bütçe ve maliyet uyarılarını kullanma

Maliyetleri yönetmek için [bütçeler](../../cost-management-billing/costs/tutorial-acm-create-budgets.md) oluşturabilir ve paydaşları harcama anormallikleri ve fazla harcama riskleri ile ilgili otomatik olarak bilgilendiren uyarılar oluşturabilirsiniz. Uyarılar, bütçe ve maliyet eşiklerine kıyasla harcamaya göre belirlenir. Bütçeler ve uyarılar Azure abonelikleri ve kaynak grupları için oluşturulur, bu nedenle genel maliyet izleme stratejisinin bir parçası olarak faydalıdır. Ancak, maliyetleri daha yüksek bir düzeyde izlemek üzere tasarlandıklarından Azure depolama maliyeti gibi ayrı Azure hizmeti maliyetlerini yönetmek için sınırlı işlevlere sahip olabilirler.

## <a name="monitor-costs"></a>Maliyetleri izleme

Azure depolama ile Azure kaynaklarını kullanırken maliyetleriniz vardır. Kaynak kullanımı birim maliyetleri, zaman aralıklarına (saniye, dakika, saat ve gün) veya birim kullanımı (bayt, megabayt vb.) göre farklılık gösterir. Azure depolama 'nın kullanımı başladığı anda maliyetler tahakkuk edilir. Maliyetleri, Azure portal [Maliyet Analizi](../../cost-management-billing/costs/quick-acm-cost-analysis.md) bölmesinde görebilirsiniz.

Maliyet analizini kullandığınızda, Azure depolama maliyetlerini grafik ve tablolardaki farklı zaman aralıkları için görüntüleyebilirsiniz. Bazı örnekler güne, geçerli ve önceki aya ve yıla göre yapılır. Ayrıca bütçeleri ve tahmin edilen maliyetlerden de maliyet görüntüleyebilirsiniz. Zamana göre daha uzun görünümlere geçiş yapmak, harcama eğilimlerini belirlemenize ve fazla harcama oluşmuş olabileceğini görmenizi sağlar. Bütçeleri oluşturduysanız, nerede aşıldığını da kolayca görebilirsiniz.

Azure depolama maliyetlerini maliyet analizi 'nde görüntülemek için:

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. **Maliyet yönetimi + faturalandırma** penceresini açın, menüden **maliyet yönetimi** ' ni seçin ve ardından **Maliyet Analizi**' ni seçin. Daha sonra **kapsam** açılan listesinden belirli bir aboneliğin kapsamını değiştirebilirsiniz.

   ![Maliyet Analizi bölmesi ile maliyetleri izleme](./media/storage-plan-manage-costs/cost-analysis-pane.png)

4. Yalnızca Azure Storage maliyetlerini görüntülemek için **Filtre Ekle** ' yi ve ardından **hizmet adı**' nı seçin. Sonra listeden **depolama** ' yı seçin. 

   Yalnızca Azure depolama 'nın maliyetlerini gösteren bir örnek aşağıda verilmiştir:

   ![Maliyet Analizi bölmesi ile depolama maliyetlerini izleme](./media/storage-plan-manage-costs/cost-analysis-pane-storage.png)

Önceki örnekte, hizmet için geçerli maliyeti görürsünüz. Azure bölgeleri (konumlar) ve kaynak grubuna göre maliyetler de görüntülenir.  

## <a name="next-steps"></a>Sonraki adımlar

[Maliyet Analizi](../../cost-management-billing/costs/quick-acm-cost-analysis.md)ile maliyetleri yönetme hakkında daha fazla bilgi edinin.

Fiyatlandırma 'nın Azure depolama ile nasıl çalıştığı hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure depolama genel bakış fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/)
- [Ayrılmış kapasite ile Blob depolama maliyetlerini iyileştirme](../blobs/storage-blob-reserved-capacity.md)
