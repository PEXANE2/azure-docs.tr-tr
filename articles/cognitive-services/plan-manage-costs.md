---
title: Azure bilişsel hizmetler için maliyetleri yönetmeyi planlayın
description: Azure portal maliyet analizini kullanarak Azure bilişsel hizmetler için maliyetleri nasıl planlayacağınızı ve yöneteceğinizi öğrenin.
author: erhopf
ms.author: erhopf
ms.custom: subject-cost-optimization
ms.service: cognitive-services
ms.topic: how-to
ms.date: 12/15/2020
ms.openlocfilehash: db99fa5caff27a24aa04e4780b25ade3f7c25496
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101699938"
---
# <a name="plan-and-manage-costs-for-azure-cognitive-services"></a>Azure bilişsel hizmetler için maliyetleri planlayın ve yönetin

Bu makalede, Azure bilişsel hizmetler için maliyetleri nasıl planlayabileceğinizi ve yönetebileceğiniz açıklanır. İlk olarak, maliyetleri tahmin etmek üzere hizmet için herhangi bir kaynak eklemeden önce bilişsel hizmetler maliyetlerini planlamaya yardımcı olmak üzere Azure Fiyatlandırma hesaplayıcısı ' nı kullanırsınız. Daha sonra, Azure kaynaklarını eklerken Tahmini maliyetleri gözden geçirin. Bilişsel hizmetler kaynaklarını (örneğin, konuşma, Görüntü İşleme, Lua, Metin Analizi, Translator, vb.) kullanmaya başladıktan sonra, bütçe ve izleme maliyetlerini ayarlamak için maliyet yönetimi özelliklerini kullanın. Ayrıca Tahmini maliyetleri gözden geçirebilir ve hareket etmek isteyebileceğiniz yerleri belirlemek için harcama eğilimlerini belirleyebilirsiniz. Bilişsel hizmetler için maliyetler yalnızca Azure faturanızda bulunan Aylık maliyetlerin bir parçasıdır. Bu makalede bilişsel hizmetler için maliyetlerin nasıl planlanacağı ve yönetileceği açıklanmakta olsa da, üçüncü taraf hizmetler de dahil olmak üzere Azure aboneliğinizde kullanılan tüm Azure hizmetleri ve kaynakları için faturalandırılırsınız.

## <a name="prerequisites"></a>Önkoşullar

Maliyet yönetimi 'nde maliyet analizi, çoğu Azure hesap türünü destekler, ancak bunların tümünü kullanmaz. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](../cost-management-billing/costs/understand-cost-mgt-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn). Maliyet verilerini görüntülemek için bir Azure hesabı için en azından okuma erişiminizin olması gerekir. Azure Maliyet Yönetimi verilerine erişim atama hakkında daha fazla bilgi için bkz. [Verilere erişim atama](../cost-management-billing/costs/assign-access-acm-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

<!--Note for Azure service writer: If you have other prerequisites for your service, insert them here -->

## <a name="estimate-costs-before-using-cognitive-services"></a>Bilişsel hizmetler 'i kullanmadan önce maliyetleri tahmin etme

Bilişsel hizmetler eklemeden önce maliyetleri tahmin etmek için [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) ' nı kullanın.

:::image type="content" source="media/cognitive-services-pricing-calculator.png" alt-text="Bilişsel hizmetler için Azure Fiyatlandırma Hesaplayıcı" border="true":::

Çalışma alanınıza yeni kaynaklar eklerken bu Hesaplayıcı 'a dönün ve maliyet tahminlerinizi güncelleştirmek için aynı kaynağı buraya ekleyin.

Daha fazla bilgi için bkz. Azure bilişsel [Hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/).

## <a name="understand-the-full-billing-model-for-cognitive-services"></a>Bilişsel hizmetler için tam faturalandırma modelini anlayın

Bilişsel hizmetler, yeni kaynağı dağıtırken [maliyeti tahakkuk](https://azure.microsoft.com/pricing/details/cognitive-services/) eden Azure altyapısında çalışır. Ek altyapının maliyeti tahakkuk edebileceğini anlamak önemlidir. Dağıtılan kaynaklarda değişiklik yaptığınızda bu maliyeti yönetmeniz gerekir. 

### <a name="costs-that-typically-accrue-with-cognitive-services"></a>Genellikle bilişsel hizmetler ile tahakkuk eden maliyetler

Genellikle, bir Azure kaynağı dağıttıktan sonra ücretler fiyatlandırma katmanınız ve uç noktanıza yaptığınız API çağrıları tarafından belirlenir. Kullandığınız hizmetin bir taahhüt katmanı varsa, katmanınızda ayrılan çağrıların üzerine gidip fazla kullanım ücreti uygulanabilir.

Bu hizmetler kullanılırken ek maliyetler tahakkuk edebilir:

#### <a name="qna-maker"></a>Soru-Cevap Oluşturucu

Soru-Cevap Oluşturma için kaynak oluşturduğunuzda, diğer Azure hizmetlerine yönelik kaynaklar da oluşturulabilir. Bunlara aşağıdakiler dahildir:

- [Azure App Service (çalışma zamanı için)](https://azure.microsoft.com/pricing/details/app-service/)
- [Azure Bilişsel Arama (veriler için)](https://azure.microsoft.com/pricing/details/search/)
 
### <a name="costs-that-might-accrue-after-resource-deletion"></a>Kaynak silme işleminden sonra tahakkuk olabilecek maliyetler

#### <a name="qna-maker"></a>Soru-Cevap Oluşturucu

Soru-Cevap Oluşturma kaynaklarını sildikten sonra, aşağıdaki kaynaklar mevcut olmaya devam edebilir. Bunlar silinene kadar maliyetleri tahakkuk ettirmeye devam eder.

- [Azure App Service (çalışma zamanı için)](https://azure.microsoft.com/pricing/details/app-service/)
- [Azure Bilişsel Arama (veriler için)](https://azure.microsoft.com/pricing/details/search/)

### <a name="using-azure-prepayment-credit-with-cognitive-services"></a>Bilişsel hizmetler ile Azure ön ödeme kredisi kullanma

Bilişsel hizmetler ücretlendirmesinin Azure ön ödeme (daha önce parasal taahhüt olarak adlandırılır) kredisi ile ödeme yapabilirsiniz. Ancak Azure ön ödeme kredisi, Azure Marketi 'nden dahil olan üçüncü taraf ürün ve hizmetlerine yönelik ücretler için ödeme yapmak üzere kullanamazsınız.

## <a name="monitor-costs"></a>Maliyetleri izleme

<!-- Note to Azure service writer: Modify the following as needed for your service. Replace example screenshots with ones taken for your service. If you need assistance capturing screenshots, ask banders for help. -->

Bilişsel hizmetler ile Azure kaynaklarını kullanırken maliyetleriniz vardır. Azure Kaynak kullanımı birim maliyetleri zaman aralıklarına (saniye, dakika, saat ve gün) veya birim kullanımı (bayt, megabayt vb.) göre değişir. Bilişsel hizmet hizmeti (veya bilişsel hizmetler) başladığında maliyetler ücretlendirilir ve [Maliyet analizinde](../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)maliyetleri görebilirsiniz.

Maliyet analizini kullandığınızda, farklı zaman aralıkları için grafiklerde ve tablolarda bilişsel hizmetler maliyetlerini görüntüleyebilirsiniz. Bazı örnekler güne, geçerli ve önceki aya ve yıla göre yapılır. Ayrıca bütçeleri ve tahmin edilen maliyetleri de görüntüleyebilirsiniz. Zamana göre daha uzun görünümlere geçiş yapmak harcama eğilimlerini belirlemenize yardımcı olabilir. Ve fazla harcama oluşmuş olabileceğini görürsünüz. Bütçeleri oluşturduysanız, nerede aşıldığınızı da kolayca görebilirsiniz.

Bilişsel hizmetler maliyetlerini maliyet analizi 'nde görüntülemek için:

1. Azure Portal’da oturum açın.
2. Azure portal kapsamı açın ve menüden **Maliyet Analizi** ' ni seçin. Örneğin, **abonelikler**' e gidin, listeden bir abonelik seçin ve ardından menüde  **Maliyet Analizi** ' ni seçin. Maliyet analizinde farklı bir kapsama geçiş yapmak için **kapsam** ' ı seçin.
3. Varsayılan olarak, hizmetlerin maliyeti ilk halka grafiğinde gösterilir. Bilişsel hizmetler etiketli grafikteki alanı seçin.

Maliyet analizini ilk kez açtığınızda gerçek aylık maliyetler gösterilir. İşte tüm aylık kullanım maliyetlerini gösteren bir örnek.

:::image type="content" source="./media/cost-management/all-costs.png" alt-text="Bir aboneliğin birikmiş maliyetlerini gösteren örnek":::

- Bilişsel hizmetler gibi tek bir hizmetin maliyetlerini daraltmak için **Filtre Ekle** ' yi ve ardından **hizmet adı**' nı seçin. Ardından bilişsel **Hizmetler**' i seçin.

Yalnızca bilişsel hizmetler için maliyetleri gösteren bir örnek aşağıda verilmiştir.

:::image type="content" source="./media/cost-management/cognitive-services-costs.png" alt-text="Bilişsel hizmetler için birikmiş maliyetleri gösteren örnek":::

Önceki örnekte, hizmet için geçerli maliyeti görürsünüz. Kaynak grubuna göre Azure bölgeleri (konumlar) ve bilişsel hizmetler maliyetlerine göre maliyetler de gösterilir. Buradan, maliyetlerinizi kendiniz inceleyebilirsiniz.

## <a name="create-budgets"></a>Bütçe oluşturma

Maliyetleri yönetmek için [bütçeler](../cost-management-billing/costs/tutorial-acm-create-budgets.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) oluşturabilir ve paydaşları harcama anormallikleri ve fazla harcama riskleri ile ilgili otomatik olarak bilgilendiren [uyarılar](../cost-management-billing/costs/cost-mgt-alerts-monitor-usage-spending.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) oluşturabilirsiniz. Uyarılar, bütçe ve maliyet eşiklerine kıyasla harcamaya göre belirlenir. Bütçeler ve uyarılar Azure abonelikleri ve kaynak grupları için oluşturulur, bu nedenle genel maliyet izleme stratejisinin bir parçası olarak faydalıdır. 

İzlemekte daha fazla ayrıntı düzeyi istiyorsanız, Azure 'daki belirli kaynaklara veya hizmetlere yönelik filtrelerle bütçeler oluşturulabilir. Filtreler, yanlışlıkla ek para maliyeti sağlayan yeni kaynaklar oluşturmamasını sağlamaya yardımcı olur. Bir bütçe oluştururken filtre seçenekleri hakkında daha fazla bilgi için bkz. [Grup ve filtre seçenekleri](../cost-management-billing/costs/group-filter.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

## <a name="export-cost-data"></a>Maliyet verilerini dışarı aktarma

Ayrıca, maliyet verilerinizi bir depolama hesabına [dışarı aktarabilirsiniz](../cost-management-billing/costs/tutorial-export-acm-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) . Bu, maliyetler için ek veri analizi yapmanız gerektiğinde veya başkalarının yararlı olması durumunda faydalıdır. Örneğin, finans ekipleri Excel veya Power BI kullanarak verileri analiz edebilir. Maliyetlerinizi günlük, haftalık veya aylık bir zamanlamaya göre dışa aktarabilir ve özel bir tarih aralığı ayarlayabilirsiniz. Maliyet verilerini dışarı aktarmak, maliyet veri kümelerini almak için önerilen yoldur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure maliyet yönetimi ile bulut yatırımınızın nasıl iyileştirileceği hakkında](../cost-management-billing/costs/cost-mgt-best-practices.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)bilgi edinin.
- [Maliyet Analizi](../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)ile maliyetleri yönetme hakkında daha fazla bilgi edinin.
- [Beklenmeyen maliyetlerin nasıl önleneceği](../cost-management-billing/cost-management-billing-overview.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)hakkında bilgi edinin.
- [Maliyet yönetimi](/learn/paths/control-spending-manage-bills?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) destekli öğrenme kursuna katılın.