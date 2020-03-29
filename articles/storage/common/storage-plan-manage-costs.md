---
title: Azure Depolama için maliyetleri planlama ve yönetme
description: Azure portalında maliyet çözümlemesi kullanarak Azure Depolama için maliyetleri nasıl planlayıp yöneteceklerini öğrenin.
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: normesta
ms.subservice: common
ms.custom: subject-cost-optimization
ms.openlocfilehash: aa0b789b31f50c8b1ccf5450700874a02ad4664c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78304530"
---
# <a name="plan-and-manage-costs-for-azure-storage"></a>Azure Depolama için maliyetleri planlama ve yönetme

Bu makalede, Azure Depolama için maliyetleri nasıl planladığınızı ve yönettiğinizi açıklamaktadır. İlk olarak, herhangi bir kaynak eklemeden önce depolama maliyetlerini planlamaya yardımcı olmak için Azure fiyatlandırma hesaplayıcısını kullanırsınız. Azure Depolama kaynaklarını kullanmaya başladıktan sonra, bütçeleri ayarlamak ve maliyetleri izlemek için maliyet yönetimi özelliklerini kullanın. Ayrıca, hareket etmek isteyebileceğiniz alanları belirlemek için tahmini maliyetleri gözden geçirebilir ve harcama eğilimlerini izleyebilirsiniz.

Azure Depolama maliyetlerinin Azure faturanızdaki aylık maliyetlerin yalnızca bir kısmı olduğunu unutmayın. Bu makalede, Azure Depolama için maliyetleri nasıl planlayıp yönetisiniz açıklansa da, azure aboneliğiniz için kullanılan tüm Azure hizmetleri ve kaynakları (üçüncü taraf hizmetleri de dahil olmak üzere) için faturalandırılırsınız. Azure Depolama'nın maliyetlerini yönetmeyi tanıdıktan sonra, aboneliğinizde kullanılan tüm Azure hizmetlerinin maliyetlerini yönetmek için benzer yöntemler uygulayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Maliyet analizi, farklı türdeki Azure hesaplarını destekler. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](../../cost-management-billing/costs/understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için Azure hesabınızda en azından okuma yetkisine sahip olmanız gerekir. Azure Maliyet Yönetimi verilerine erişim atama hakkında daha fazla bilgi için bkz. [Verilere erişim atama](../../cost-management-billing/costs/assign-access-acm-data.md).

## <a name="estimate-costs-before-creating-an-azure-storage-account"></a>Azure Depolama hesabı oluşturmadan önce maliyetleri tahmin edin

Verileri oluşturmadan ve bir Azure Depolama hesabına aktarmaya başlamadan önce maliyetleri tahmin etmek için [Azure fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

1. Azure [fiyatlandırma hesap makinesi](https://azure.microsoft.com/pricing/calculator/) **sayfasında, Depolama Hesapları** döşemesini seçin.

2. Sayfayı aşağı kaydırın ve tahmininizin **Depolama Hesapları** bölümünü bulun.

3. Açılan listelerden seçenekleri seçin. 

   Bu açılır listelerin değerini değiştirin, maliyet tahmini değişir. Bu tahmin, tahminin alt köşesinde olduğu gibi üst köşede de görünür. 
    
   ![Maliyet Analizi bölmesiyle maliyetleri izleme](media/storage-plan-manage-costs/price-calculator-storage-type.png)

   **Tür** açılır listesinin değerini değiştirin, bu çalışma sayfasında görünen diğer seçenekler de değişir. Her seçeneğin ne anlama geldiğini ve bu seçeneklerin depolamayla ilgili işlemlerin fiyatını nasıl etkilediği hakkında daha fazla bilgi edinmek için **Daha Fazla Bilgi** bölümündeki bağlantıları kullanın. 

4. Kalan seçenekleri, tahmininiz üzerindeki etkilerinin görmek için değiştirin.

## <a name="use-budgets-and-cost-alerts"></a>Bütçe ve maliyet uyarılarını kullanma

Maliyetleri yönetmek için [bütçeler](../../cost-management-billing/costs/tutorial-acm-create-budgets.md) oluşturabilir ve paydaşları harcama anormallikleri ve fazla harcama riskleri ile ilgili otomatik olarak bilgilendiren uyarılar oluşturabilirsiniz. Uyarılar, bütçe ve maliyet eşiklerine kıyasla harcamaya göre belirlenir. Azure abonelikleri ve kaynak grupları için bütçeler ve uyarılar oluşturulur, bu nedenle genel bir maliyet izleme stratejisinin bir parçası olarak yararlıdırlar. Ancak, maliyetleri daha yüksek bir düzeyde izlemek üzere tasarlandıkları için Azure Depolama maliyeti gibi tek tek Azure hizmet maliyetlerini yönetmek için sınırlı işlevleri olabilir.

## <a name="monitor-costs"></a>Maliyetleri izleme

Azure Depolama ile Azure kaynaklarını kullandığınızda, maliyete maruz kalabilirsiniz. Kaynak kullanım birimi maliyetleri zaman aralıklarına (saniye, dakika, saat ve gün) veya birim kullanımına (bayt, megabayt vb.) göre değişir. Azure Depolama'nın kullanımı başlar başlamaz maliyetler tahakkuk ettirilir. Maliyetleri Azure portalında [maliyet analizi](../../cost-management-billing/costs/quick-acm-cost-analysis.md) bölmesinde görebilirsiniz.

Maliyet çözümlemesi kullandığınızda, farklı zaman aralıkları için grafik ve tablolarda Azure Depolama maliyetlerini görüntüleyebilirsiniz. Bazı örnekler gün, geçerli ve önceki ay ve yıl vardır. Maliyetleri bütçelere ve öngörülen maliyetlere göre de görüntüleyebilirsiniz. Zaman içinde daha uzun görünümlere geçmek, harcama eğilimlerini belirlemenize ve aşırı harcamanın nerede oluşmuş olabileceğini görmenize yardımcı olabilir. Bütçeler oluşturduysanız, bunların nerede aşıldığını da kolayca görebilirsiniz.

Maliyet analizinde Azure Depolama maliyetlerini görüntülemek için:

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. Maliyet **Yönetimi + Faturalandırma** penceresini açın, menüden **Maliyet yönetimini** seçin ve ardından **Maliyet çözümlemesi'ni**seçin. Daha sonra **Kapsam** açılır tarihinden belirli bir aboneliğin kapsamını değiştirebilirsiniz.

   ![Maliyet Analizi bölmesiyle maliyetleri izleme](./media/storage-plan-manage-costs/cost-analysis-pane.png)

4. Yalnızca Azure Depolama'nın maliyetlerini görüntülemek için **Filtre Ekle'yi** ve ardından **Hizmet adını**seçin. Ardından, listeden **depolama alanı** seçin. 

   Aşağıda, sadece Azure Depolama'nın maliyetlerini gösteren bir örnek verilmiştir:

   ![Maliyet Analizi bölmesi ile depolama maliyetlerini izleme](./media/storage-plan-manage-costs/cost-analysis-pane-storage.png)

Önceki örnekte, hizmetin geçerli maliyetini görürsünüz. Azure bölgelerine (konumlara) ve kaynak grubuna göre maliyetler de görüntülenir.  

## <a name="next-steps"></a>Sonraki adımlar

[Maliyet analizi](../../cost-management-billing/costs/quick-acm-cost-analysis.md)ile maliyetleri yönetme hakkında daha fazla bilgi edinin.

Azure Depolama ile fiyatlandırmanın nasıl çalıştığı hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Depolama genel değerlendirme fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/)
- [Ayrılmış kapasite ile Blob depolama maliyetlerini iyileştirme](../blobs/storage-blob-reserved-capacity.md)
