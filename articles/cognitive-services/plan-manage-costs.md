---
title: Azure bilişsel hizmetler için maliyetleri yönetmeyi planlayın
description: Azure portal maliyet analizini kullanarak Azure bilişsel hizmetler için maliyetleri nasıl planlayacağınızı ve yöneteceğinizi öğrenin.
author: erhopf
ms.author: erhopf
ms.custom: subject-cost-optimization
ms.service: cognitive-services
ms.topic: how-to
ms.date: 12/15/2020
ms.openlocfilehash: 48e111ac8b2b4fd3c1e2fee568e20699896dfff3
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2020
ms.locfileid: "97705793"
---
# <a name="plan-and-manage-costs-for-azure-cognitive-services"></a>Azure bilişsel hizmetler için maliyetleri planlayın ve yönetin

Bu makalede, Azure bilişsel hizmetler için maliyetleri nasıl planlayabileceğinizi ve yönetebileceğiniz açıklanır. İlk olarak, maliyetleri tahmin etmek üzere hizmet için herhangi bir kaynak eklemeden önce bilişsel hizmetler maliyetlerini planlamaya yardımcı olmak üzere Azure Fiyatlandırma hesaplayıcısı ' nı kullanırsınız. Daha sonra, Azure kaynaklarını eklerken Tahmini maliyetleri gözden geçirin. Bilişsel hizmetler kaynaklarını (örneğin, konuşma, Görüntü İşleme, Lua, Metin Analizi, Translator, vb.) kullanmaya başladıktan sonra, bütçe ve izleme maliyetlerini ayarlamak için maliyet yönetimi özelliklerini kullanın. Ayrıca Tahmini maliyetleri gözden geçirebilir ve hareket etmek isteyebileceğiniz yerleri belirlemek için harcama eğilimlerini belirleyebilirsiniz. Bilişsel hizmetler için maliyetler yalnızca Azure faturanızda bulunan Aylık maliyetlerin bir parçasıdır. Bu makalede bilişsel hizmetler için maliyetlerin nasıl planlanacağı ve yönetileceği açıklanmakta olsa da, üçüncü taraf hizmetler de dahil olmak üzere Azure aboneliğinizde kullanılan tüm Azure hizmetleri ve kaynakları için faturalandırılırsınız.

## <a name="prerequisites"></a>Önkoşullar

Maliyet yönetimi 'nde maliyet analizi, çoğu Azure hesap türünü destekler, ancak bunların tümünü kullanmaz. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-cost-mgt-data?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn). Maliyet verilerini görüntülemek için bir Azure hesabı için en azından okuma erişiminizin olması gerekir. Azure Maliyet Yönetimi verilerine erişim atama hakkında daha fazla bilgi için bkz. [Verilere erişim atama](https://docs.microsoft.com/azure/cost-management/assign-access-acm-data?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

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

### <a name="using-monetary-credit-with-cognitive-services"></a>Bilişsel hizmetler ile parasal krediyi kullanma

Bilişsel hizmetler ücretleri için EA parasal taahhüt krediniz ile ödeme yapabilirsiniz. Ancak, Azure Marketi 'nden dahil olmak üzere üçüncü taraf ürün ve hizmetleri için ücret ödeyerek EA parasal taahhüt kredisi kullanamazsınız.

## <a name="create-budgets"></a>Bütçe oluşturma

Maliyetleri yönetmek için [bütçeler](https://docs.microsoft.com/azure/cost-management/tutorial-acm-create-budgets?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) oluşturabilir ve paydaşları harcama anormallikleri ve fazla harcama riskleri ile ilgili otomatik olarak bilgilendiren [uyarılar](https://docs.microsoft.com/azure/cost-management/cost-mgt-alerts-monitor-usage-spending?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) oluşturabilirsiniz. Uyarılar, bütçe ve maliyet eşiklerine kıyasla harcamaya göre belirlenir. Bütçeler ve uyarılar Azure abonelikleri ve kaynak grupları için oluşturulur, bu nedenle genel maliyet izleme stratejisinin bir parçası olarak faydalıdır. 

İzlemekte daha fazla ayrıntı düzeyi istiyorsanız, Azure 'daki belirli kaynaklara veya hizmetlere yönelik filtrelerle bütçeler oluşturulabilir. Filtreler, yanlışlıkla ek para maliyeti sağlayan yeni kaynaklar oluşturmamasını sağlamaya yardımcı olur. Bir bütçe oluştururken filtre seçenekleri hakkında daha fazla bilgi için bkz. [Grup ve filtre seçenekleri](https://docs.microsoft.com/azure/cost-management-billing/costs/group-filter?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

## <a name="export-cost-data"></a>Maliyet verilerini dışarı aktarma

Ayrıca, maliyet verilerinizi bir depolama hesabına [dışarı aktarabilirsiniz](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) . Bu, maliyetler için ek veri analizi yapmanız gerektiğinde veya başkalarının yararlı olması durumunda faydalıdır. Örneğin, finans ekipleri Excel veya Power BI kullanarak verileri analiz edebilir. Maliyetlerinizi günlük, haftalık veya aylık bir zamanlamaya göre dışa aktarabilir ve özel bir tarih aralığı ayarlayabilirsiniz. Maliyet verilerini dışarı aktarmak, maliyet veri kümelerini almak için önerilen yoldur.

<!--
## Other ways to manage and reduce costs for Cognitive Services

Work with Dean to complete this section in 2021.

-->

## <a name="next-steps"></a>Sonraki adımlar

- [Azure maliyet yönetimi ile bulut yatırımınızın nasıl iyileştirileceği hakkında](https://docs.microsoft.com/azure/cost-management-billing/costs/cost-mgt-best-practices?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)bilgi edinin.
- [Maliyet Analizi](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)ile maliyetleri yönetme hakkında daha fazla bilgi edinin.
- [Beklenmeyen maliyetlerin nasıl önleneceği](https://docs.microsoft.com/azure/cost-management-billing/manage/getting-started?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)hakkında bilgi edinin.
- [Maliyet yönetimi](https://docs.microsoft.com/learn/paths/control-spending-manage-bills?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) destekli öğrenme kursuna katılın.
