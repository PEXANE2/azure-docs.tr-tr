---
title: Azure maliyetlerini ayırma
description: Bu makalede aboneliklerin, kaynak gruplarının veya etiketlerin maliyetini diğerlerine dağıtmak amacıyla maliyet ayırma kurallarının nasıl oluşturulacağı açıklanmaktadır.
author: bandersmsft
ms.author: banders
ms.date: 08/11/2020
ms.topic: how-to
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: benshy
ms.openlocfilehash: 025b9b7e503f38a111bd158f17b7fbeec5b23579
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88684992"
---
# <a name="create-and-manage-azure-cost-allocation-rules-preview"></a>Azure maliyet ayırma kurallarını oluşturma ve yönetme (Önizleme)

Büyük kuruluşlar genelde merkezi olarak yönetilen Azure hizmetlerine veya kaynaklarına sahiptir, ancak bunlar şirket içindeki farklı departmanlar veya iş birimleri tarafından kullanılır. Merkezi olarak yöneten ekip genellikle, paylaşılan hizmetlerin maliyetini, hizmetleri etkin bir şekilde kullanan şirket içi departmanlara veya kuruluşa özgü iş birimlerine yeniden ayırmak ister. Bu makale, Azure Maliyet Yönetimi’ndeki maliyet ayırma özelliğini anlamanıza ve kullanmanıza yardımcı olur.

Maliyet ayırma sayesinde abonelik, kaynak grubu veya etiketlerdeki paylaşılan hizmetlerin maliyetlerini diğer aboneliklere, kaynak gruplarına veya kuruluşunuzdaki etiketlere yeniden atayabilir ya da dağıtabilirsiniz. Maliyet ayırma, paylaşılan hizmetlerin maliyetlerini bu hizmetleri kullanan şirket içi departmanların veya iş birimlerinin sahip olduğu başka aboneliklere, kaynak gruplarına ya da etiketlere kaydırır. Başka bir deyişle maliyet ayırma, _maliyet sorumluluğunu_ yönetmeye ve bir yerden diğerine göstermeye yardımcı olur.

Maliyet ayırma, faturalandırma sürecinizi etkilemez. Faturalandırma sorumlulukları değişmez. Maliyet ayırmanın birincil amacı, maliyetleri başkalarına yansıtmanıza yardımcı olmaktır. Kuruluşunuzdaki tüm ücret yansıtma işlemleri Azure’ın dışında gerçekleşir. Maliyet ayırma, maliyetleri yeniden atanmış veya dağıtılmış şekilde göstererek yansıtmanıza yardımcı olur.

Ayrılan maliyetler maliyet analizinde görüntülenir. Bunlar, bir maliyet ayırma kuralı oluştururken belirttiğiniz hedeflenen aboneliklerle, kaynak gruplarıyla veya etiketlerle ilişkili ek öğeler olarak gösterilir.

> [!NOTE]
> Azure Maliyet Yönetimi’nin maliyet ayırma özelliği şu anda genel önizleme aşamasındadır. Maliyet Yönetimi’ndeki bazı özellikler desteklenmeyebilir veya sınırlı özelliklere sahip olabilir.

## <a name="prerequisites"></a>Ön koşullar

- Maliyet ayırma şu anda yalnızca [Microsoft Müşteri Sözleşmesi](https://azure.microsoft.com/pricing/purchase-options/microsoft-customer-agreement/) veya [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) sahibi olan müşterileri destekler.
- Maliyet ayırma kuralı oluşturup yönetmek için [Kurumsal Anlaşmalara](../manage/understand-ea-roles.md) yönelik bir Kurumsal Yönetici hesabı kullanmanız gerekir. Alternatif olarak, Microsoft Müşteri Sözleşmesi’ne yönelik bir [Ödeme hesabı](../manage/understand-mca-roles.md) sahibi olmanız gerekir.

## <a name="create-a-cost-allocation-rule"></a>Maliyet ayırma kuralı oluşturma

1. [https://portal.azure.com](https://portal.azure.com/) adresinden Azure portalında oturum açın.
2. **Maliyet Yönetimi + Faturalama** > **Maliyet Yönetimi**’ne gidin.
3. **Ayarlar** > **Yapılandırma** bölümünde **Maliyet ayırma (Önizleme)** seçeneğini belirleyin.
4. Doğru EA kaydının veya ödeme hesabının seçili olduğundan emin olun.
5. **+Ekle**’yi seçin.
6. Maliyet ayırma kuralı adı için açıklayıcı bir metin girin.

:::image type="content" source="./media/allocate-costs/rule-name.png" alt-text="Kural adı oluşturmayı gösteren örnek" lightbox="./media/allocate-costs/rule-name.png" :::

Kuralın değerlendirme başlangıç tarihi, önceden doldurulmuş maliyet ayırma yüzdelerini oluşturmak için kullanılır.

1. **Kaynak ekle** seçeneğini belirleyin ve sonra maliyetlerin dağıtılacağı abonelik, kaynak grubu veya etiketleri seçin.
2. **Hedef ekle** seçeneğini belirleyin ve sonra ayrılan maliyetleri alacak olan abonelik, kaynak grubu veya etiketleri seçin.
3. Başka maliyet ayırma kuralları oluşturmanız gerekirse bu işlemi tekrarlayın.

## <a name="configure-the-allocation-percentage"></a>Ayırma yüzdesini yapılandırma

Maliyetlerin belirtilen hedefler arasında orantılı bir şekilde nasıl bölüneceğini belirlemek için ayırma yüzdesini yapılandırın. Ayırma kuralı oluşturmak için tam sayı yüzdelerini el ile tanımlayabilirsiniz. Alternatif olarak, belirtilen hedeflerdeki geçerli işlem, depolama veya ağ kullanımına göre maliyetleri orantılı bir şekilde bölebilirsiniz.

Maliyetleri işlem, depolama veya ağ maliyeti olarak dağıtırken oran yüzdeleri seçili hedeflerin maliyetleri değerlendirilerek türetilir. Maliyetler, geçerli fatura ayının kaynak türüyle ilişkilendirilir.

Maliyetleri toplam maliyete orantılı bir şekilde dağıtırken orantı yüzdesi, geçerli fatura ayındaki seçili hedeflerin toplamı veya toplam maliyetine göre atanır.

:::image type="content" source="./media/allocate-costs/cost-distribution.png" alt-text="Ayırma yüzdesini gösteren örnek" lightbox="./media/allocate-costs/cost-distribution.png" :::

Ayarlandıktan sonra, önceden doldurulmuş tanımlı yüzdeler sabit kalır. Bu yüzdeler, devam eden tüm ayırmalarda kullanılır. Yüzdeler yalnızca kural el ile güncelleştirildiğinde değişir.

1. **Önceden doldurma yüzdesi** listesindeki şu seçeneklerden birini belirleyin.
    - **Eşit dağıt**: Hedeflerin her biri toplam maliyetin eşit bir bölümünü alır.
    - **Toplam maliyet**: Hedeflerin toplam maliyetine göre bir oran oluşturur. Bu oran, seçili kaynakların maliyetlerini dağıtmak için kullanılır.
    - **İşlem maliyeti**: Hedeflerin Azure işlem maliyetlerine (kaynak türleri [Microsoft.Compute](https://docs.microsoft.com/azure/templates/microsoft.compute/allversions) ad alanında belirtilir) göre bir oran oluşturur. Bu oran, seçili kaynakların maliyetlerini dağıtmak için kullanılır.
    - **Depolama maliyeti**: Hedeflerin Azure depolama maliyetlerine (kaynak türleri [Microsoft.Compute](https://docs.microsoft.com/azure/templates/microsoft.storage/allversions) ad alanında belirtilir) göre bir oran oluşturur. Bu oran, seçili kaynakların maliyetlerini dağıtmak için kullanılır.
    - **Ağ maliyeti**: Hedeflerin Azure ağ maliyetlerine (kaynak türleri [Microsoft.Network](https://docs.microsoft.com/azure/templates/microsoft.network/allversions) ad alanında belirtilir) göre bir oran oluşturur. Bu oran, seçili kaynakların maliyetlerini dağıtmak için kullanılır.
    - **Özel**: Bir tam sayı yüzdesinin el ile belirtilmesine olanak verir. Belirtilen toplam %100’e eşit olmalıdır.
1. Kural yapılandırıldığında **Oluştur**’u seçin.

Böylece, ayırma kuralı işlemeye başlar. Kural etkin olduğunda, seçili kaynağın tüm maliyetleri belirtilen hedeflere ayrılır.

> [!NOTE] 
> Yeni kural işlemesinin tamamlanıp etkin olması iki saate kadar sürebilir.

## <a name="verify-the-cost-allocation-rule"></a>Maliyet ayırma kuralını doğrulama

Maliyet ayırma kuralı etkinleştiğinde, seçili kaynaklardaki maliyetler belirtilen ayırma hedeflerine dağıtılır. Bu maliyetin hedeflere düzgün bir şekilde ayrıldığını doğrulamak için aşağıdaki bilgileri kullanın.

### <a name="view-cost-allocation-for-a-subscription"></a>Bir aboneliğin maliyet ayırmasını görüntüleme

Ayırma kuralının etkisini maliyet analizinde görebilirsiniz. Azure portalında [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) sayfasına gidin. Etkin bir maliyet ayırma kuralı tarafından hedeflenen listedeki bir aboneliği seçin. Daha sonra, menüdeki **Maliyet analizi**’ni seçin. Maliyet analizi bölümünde **Gruplandırma ölçütü**’nü ve sonra **Maliyet ayırma**’yı seçin. Elde edilen görünümde, abonelik tarafından oluşturulan hızlı bir maliyet dökümü yer alır. Aboneliğe ayrılan maliyetler de aşağıdaki resimdeki gibi gösterilir.

:::image type="content" source="./media/allocate-costs/cost-breakdown.png" alt-text="Maliyet dökümünü gösteren örnek" lightbox="./media/allocate-costs/cost-breakdown.png" :::

### <a name="view-cost-allocation-for-a-resource-group"></a>Bir kaynak grubunun maliyet ayırmasını görüntüleme

Bir kaynak grubunun maliyet ayırma kuralının etkisi için benzer bir işlemden yararlanın. Azure portalda [Kaynak grupları](https://portal.azure.com/#blade/HubsExtension/BrowseResourceGroups)’na gidin. Etkin bir maliyet ayırma kuralı tarafından hedeflenen listedeki bir kaynak grubunu seçin. Daha sonra, menüdeki **Maliyet analizi**’ni seçin. Maliyet analizi bölümünde **Gruplandırma ölçütü**’nü ve sonra **Maliyet ayırma**’yı seçin. Karşınıza çıkan görünümde, kaynak grubu tarafından oluşturulan hızlı bir maliyet dökümü yer alır. Kaynak grubuna ayrılan maliyetler de gösterilir.

### <a name="view-cost-allocation-for-tags"></a>Etiketlerin maliyet ayırmasını görüntüleme

Azure portalda **Maliyet Yönetimi + Faturalama** > **Maliyet Yönetimi** > **Maliyet analizi**’ne gidin. Maliyet analizinde **Filtre ekle**’yi seçin. **Etiket**’i seçin, etiket anahtarını ve maliyet ayrılmış olan etiket değerlerini seçin.

:::image type="content" source="./media/allocate-costs/tagged-costs.png" alt-text="Etiketli öğelerin maliyetlerini gösteren örnek" lightbox="./media/allocate-costs/tagged-costs.png" :::

## <a name="edit-an-existing-cost-allocation-rule"></a>Mevcut maliyet ayırma kuralını düzenleme

Kaynağı veya hedefi değiştirmek ya da işlem, depolama veya ağ seçeneklerinden birinin önceden doldurulmuş yüzdesini güncelleştirmek için bir maliyet ayırma kuralını düzenleyebilirsiniz. Kuralları, oluşturduğunuz şekilde düzenleyin. Mevcut kurallardaki değişikliğin yeniden işlenmesi iki saate kadar sürebilir.

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

Aşağıdaki bölümlerde insanların maliyet ayırması hakkında sıkça sordukları sorular yer alır.

### <a name="what-are-the-current-limitations-with-cost-allocation-in-public-preview"></a>Genel önizlemede maliyet ayırmanın geçerli sınırlamaları nelerdir?
<a name="limitations"></a>

Şu anda maliyet ayırma, Maliyet Yönetimi’ndeki Maliyet analizi, bütçeler ve tahmin görünümlerinde desteklenmektedir. Ayrılan maliyetler hem abonelikler listesinde hem de Aboneliklere genel bakış sayfasında gösterilir.

Aşağıdakiler şu anda maliyet ayırma genel önizlemesi tarafından desteklenmemektedir:

- Zamanlanan [Dışarı Aktarmalar](tutorial-export-acm-data.md)
- [Kullanım Ayrıntıları](https://docs.microsoft.com/rest/api/consumption/usagedetails/list) API’si tarafından sunulan veriler
- Faturalama abonelikleri alanı
- [Maliyet Yönetimi Power BI Uygulaması](https://appsource.microsoft.com/product/power-bi/costmanagement.azurecostmanagementapp)
- [Power BI Desktop bağlayıcısı](https://docs.microsoft.com/power-bi/connect-data/desktop-connect-azure-cost-management)

### <a name="are-costs-factored-into-budgets-and-forecast-views"></a>Maliyetler, bütçelerde ve tahmin görünümlerinde hesaba katılır mı?
<a name="budgets-forecast"></a>

Evet. Ayrılan maliyetler hem bütçelerde hem de tahmin görünümlerinde hesaba katılır ve bunlar tarafından desteklenir. Bütçe ve tahmin görünümleri, maliyet ayırma kuralları tarafından yapılandırıldığı şekilde kendilerine ayrılan maliyetleri gösterir.

### <a name="if-a-cost-allocation-rule-is-deleted-what-happens"></a>Bir maliyet ayırma kuralı silindiğinde ne olur?
<a name="delete-rule"></a>

Bir maliyet ayırma kuralı silindiğinde, hedeflere ayrılan tüm açık ve geçerli fatura ayı maliyetleri kaldırılır. Maliyet ayırma kuralı birkaç ay boyunca yer aldıysa önceki ayların ayırma verileri, ayırma kuralı tarafından ayarlandığı şekliyle kalır.

### <a name="why-is-an-enrollment-admin-or-a-billing-account-admin-needed-to-create-cost-allocation-rules"></a>Maliyet ayırma kuralları oluşturmak için neden bir kayıt yöneticisi veya ödeme hesabı yöneticisi gerekli?
<a name="why-admin"></a>

Maliyet ayırma kuralları ya kayıt kapsamında (Kurumsal Anlaşma) ya da Faturalandırma hesabı kapsamında (Microsoft Müşteri Sözleşmesi) oluşturulur. Bu kapsamlarda değişiklik yapma izinleri için faturalandırma yöneticisi ayrıcalıkları gerekir.

### <a name="why-are-sources-and-targets-limited-to-25-per-rule"></a>Neden kaynak ve hedefler kural başına 25 ile sınırlıdır?
<a name="source-target-rule-limit"></a>

Bu sınır, iyi bir maliyet ayırma performansı ve ölçeklenebilirlik elde etmek amacıyla koyulan önizleme sınırlamasıdır. Maliyet ayırma genel kullanılabilirlik (GA) aşamasına geçtiğinde sınırlar muhtemelen artacak veya tamamen kaldırılacaktır.

### <a name="what-can-happen-if-cost-allocation-rules-sourcestargets-overlap"></a>Maliyet ayırma kuralları (kaynak/hedef) çakışırsa ne olabilir?
<a name="rule-overlap"></a>

Çakışan kaynaklara veya hedeflere sahip kurallar önerilmez. Maliyet Ayırma kuralları oluşturulma tarihlerine göre uygulanır. Dolayısıyla, ayırma kuralıyla çakışan maliyet ayırma kurallarında en erken oluşturulma tarihine sahip olan öncelik kazanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Maliyet ayırma REST API](https://go.microsoft.com/fwlink/?linkid=2135004)’sini kullanarak ayırma kuralları oluşturma veya bunları güncelleştirme
- [Azure Maliyet Yönetimi ile bulut yatırımınızı iyileştirme](cost-mgt-best-practices.md) hakkında daha fazla bilgi edinin