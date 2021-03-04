---
title: Azure Güvenlik Merkezi 'nde iş akışı Otomasyonu | Microsoft Docs
description: Azure Güvenlik Merkezi 'nde iş akışları oluşturmayı ve otomatikleştirmeyi öğrenin
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 03/04/2021
ms.author: memildin
ms.openlocfilehash: 6268ff6cfb3d3e856edcd8f84af930d52f4cf9d3
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102096182"
---
# <a name="automate-responses-to-security-center-triggers"></a>Güvenlik Merkezi tetikleyicilerine yönelik yanıtları otomatikleştirin

Her güvenlik programı, olay yanıtı için birden çok iş akışı içerir. Bu işlemler ilgili hissedarları bildirmeyi, bir değişiklik yönetimi işlemini başlatmayı ve belirli düzeltme adımlarını uygulamayı içerebilir. Güvenlik uzmanları, yaptığınız gibi bu yordamların pek çok adımını otomatikleştirmenizi önerir. Otomasyon ek yükü azaltır. Ayrıca işlem adımlarının hızlı, tutarlı bir şekilde ve önceden tanımlanmış gereksinimlerinize göre hızla yapılmasını sağlayarak güvenliği geliştirebilir.

Bu makalede, Azure Güvenlik Merkezi 'nin iş akışı Otomasyonu özelliği açıklanır. Bu özellik güvenlik uyarıları, öneriler ve mevzuat uyumluluğuyla ilgili değişiklikler Logic Apps tetiklenebilir. Örneğin, bir uyarı oluştuğunda Güvenlik Merkezi 'nin belirli bir kullanıcıyı e-posta ile e-posta olarak istemeniz gerekebilir. Ayrıca, [Azure Logic Apps](../logic-apps/logic-apps-overview.md)kullanarak Logic Apps oluşturmayı öğreneceksiniz.


## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel kullanılabilirlik (GA)|
|Fiyat|Ücretsiz|
|Gerekli roller ve izinler:|Kaynak grubundaki **Güvenlik Yöneticisi rolü** veya **sahibi**<br>Ayrıca hedef kaynak için yazma izinlerine sahip olmalıdır<br><br>Azure Logic Apps iş akışlarıyla çalışmak için aşağıdaki Logic Apps rollere/izinlere de sahip olmanız gerekir:<br> - [Mantıksal uygulama işleci](../role-based-access-control/built-in-roles.md#logic-app-operator) izinleri gerekiyor veya mantıksal uygulama okuma/tetikleme erişimi (Bu rol, mantıksal uygulamalar oluşturamaz veya düzenleyemez; yalnızca var olanları *Çalıştır* )<br> - Mantıksal uygulama oluşturma ve değiştirme için [mantıksal uygulama katılımcısı](../role-based-access-control/built-in-roles.md#logic-app-contributor) izinleri gereklidir<br>Logic App bağlayıcıları kullanmak istiyorsanız, ilgili hizmetlerinde oturum açmak için ek kimlik bilgilerine ihtiyacınız olabilir (örneğin, Outlook/takımlar/bolluk örnekleri)|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Yes](./media/icons/yes-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||



## <a name="create-a-logic-app-and-define-when-it-should-automatically-run"></a>Bir mantıksal uygulama oluşturun ve ne zaman otomatik olarak çalışacağını tanımlayın 

1. Güvenlik Merkezi 'nin kenar çubuğundan **Iş akışı Otomasyonu**' nu seçin.

    :::image type="content" source="./media/workflow-automation/list-of-workflow-automations.png" alt-text="İş akışı otomasyonları listesi":::

    Bu sayfadan yeni otomasyon kuralları oluşturabilir, Ayrıca var olanları etkinleştirebilir, devre dışı bırakabilir veya silebilirsiniz.

1. Yeni bir iş akışı tanımlamak için **iş akışı Otomasyonu Ekle**' ye tıklayın. 

    Yeni otomasyonuna yönelik seçenekleri içeren bir bölme açılır. Buraya şunu girebilirsiniz:
    1. Otomasyon için bir ad ve açıklama.
    1. Bu otomatik iş akışını başlatacak Tetikleyiciler. Örneğin, "SQL" içeren bir güvenlik uyarısı oluşturulduğunda mantıksal uygulamanızın çalışmasını isteyebilirsiniz.

        > [!NOTE]
        > Tetikleyiciniz "alt öneriler" içeren bir öneridir, örneğin **SQL veritabanlarınızdaki güvenlik açığı değerlendirmesi düzeltilmelidir**, mantıksal uygulama her yeni güvenlik bulma işlemi için tetiklemez; yalnızca ana önerinin durumu değiştiğinde.

    1. Tetikleyici koşullarınız karşılandığında çalıştırılacak mantıksal uygulama. 

        :::image type="content" source="./media/workflow-automation/add-workflow.png" alt-text="İş akışı otomatiklamalar bölmesi Ekle":::

1. Mantıksal uygulama oluşturma işlemini başlatmak için Eylemler bölümünden **Yeni bir tane oluştur ' a** tıklayın.

    Azure Logic Apps için yönlendirilirsiniz.

    [![Yeni bir mantıksal uygulama oluşturma](media/workflow-automation/logic-apps-create-new.png)](media/workflow-automation/logic-apps-create-new.png#lightbox)

1. Bir ad, kaynak grubu ve konum girin ve **Oluştur**' a tıklayın.

1. Yeni mantıksal uygulamanızda, güvenlik kategorisinden yerleşik, önceden tanımlanmış şablonlar arasından seçim yapabilirsiniz. Ya da bu işlem tetiklendiğinde gerçekleşecek özel olay akışını tanımlayabilirsiniz.

    > [!TIP]
    > Bazen bir mantıksal uygulamada, parametreler bağlayıcıya bir dizenin parçası olarak dahil edilir ve kendi alanları içinde değildir. Parametrelerin nasıl ayıklanacağını gösteren bir örnek için bkz. [Azure Güvenlik Merkezi iş akışı tahminleri oluştururken Logic App parametreleriyle çalışma](https://techcommunity.microsoft.com/t5/azure-security-center/working-with-logic-app-parameters-while-building-azure-security/ba-p/1342121)#14 adım adım.

    Logic App Designer, bu güvenlik merkezi tetikleyicilerini destekler:

    - **Bir Azure Güvenlik Merkezi önerisi oluşturulduğunda veya tetiklendiğinde** -mantıksal uygulamanız kullanım dışı veya değiştirilmiş bir öneriyi kullanıyorsa, Otomasyon çalışmayı durdurur ve tetikleyiciyi güncelleştirmeniz gerekir. Önerilerle ilgili değişiklikleri izlemek için bkz. [Azure Güvenlik Merkezi sürüm notları](release-notes.md).

    - **Bir Azure Güvenlik Merkezi uyarısı oluşturulduğunda veya tetiklendiğinde** , tetikleyiciyi yalnızca ilgilendiğiniz önem düzeyleri ile ilgili uyarılarla ilişkili olacak şekilde özelleştirebilirsiniz.
    
    - **Bir güvenlik merkezi mevzuat uyumluluk değerlendirmesi oluşturulduğunda veya tetiklendiğinde,** mevzuat uyumluluk değerlendirmelerine yönelik güncelleştirmelere dayalı olarak otomasyon tetiklemeleri.

    > [!NOTE]
    > Eski tetikleyiciyi kullanıyorsanız "bir Azure Güvenlik Merkezi uyarısıyla bir yanıt tetiklendiğinde", Logic Apps Iş akışı Otomasyonu özelliği tarafından başlatılmaz. Bunun yerine, yukarıda bahsedilen tetikleyicilerden birini kullanın. 

    [![Örnek mantıksal uygulama](media/workflow-automation/sample-logic-app.png)](media/workflow-automation/sample-logic-app.png#lightbox)

1. Mantıksal uygulamanızı tanımladıktan sonra, iş akışı Otomasyon tanımı bölmesine ("iş akışı Otomasyonu Ekle") dönün. Yeni mantıksal uygulamanızın seçim için kullanılabilir olduğundan emin olmak için **Yenile** ' ye tıklayın.

    ![Yenile](media/workflow-automation/refresh-the-list-of-logic-apps.png)

1. Mantıksal uygulamanızı seçin ve Otomasyonu kaydedin. Mantıksal uygulama açılan listesinin yalnızca yukarıda bahsedilen destekleyici Güvenlik Merkezi bağlayıcıları olan Logic Apps gösterdiğini unutmayın.


## <a name="manually-trigger-a-logic-app"></a>Mantıksal uygulamayı el ile tetikleme

Ayrıca, herhangi bir güvenlik uyarısını veya öneriyi görüntülerken Logic Apps el ile de çalıştırabilirsiniz.

Bir mantıksal uygulamayı el ile çalıştırmak için bir uyarı veya öneri açın ve **mantıksal uygulamayı Tetikle**' ye tıklayın:

[![Mantıksal uygulamayı el ile tetikleme](media/workflow-automation/manually-trigger-logic-app.png)](media/workflow-automation/manually-trigger-logic-app.png#lightbox)


## <a name="configure-workflow-automation-at-scale-using-the-supplied-policies"></a>Sağlanan ilkeleri kullanarak iş akışı Otomasyonu 'nu ölçekte yapılandırma

Kuruluşunuzun izleme ve olay yanıtı süreçlerini otomatik hale getirmek, güvenlik olaylarını araştırmak ve azaltmak için gereken süreyi büyük ölçüde iyileştirebilir.

Otomasyon yapılandırmalarınızı kuruluşunuz genelinde dağıtmak için, aşağıda açıklanan Azure Ilkesi ' DeployIfNotExist ' ilkelerini kullanarak iş akışı Otomasyonu yordamlarını oluşturun ve yapılandırın.

[İş akışı Otomasyonu şablonları](https://github.com/Azure/Azure-Security-Center/tree/master/Workflow%20automation)ile çalışmaya başlayın.

Bu ilkeleri uygulamak için:

1. Aşağıdaki tablodan, uygulamak istediğiniz ilkeyi seçin:

    |Hedef  |İlke  |İlke KIMLIĞI  |
    |---------|---------|---------|
    |Güvenlik uyarıları için iş akışı Otomasyonu|[Azure Güvenlik Merkezi uyarıları için İş Akışı Otomasyonu dağıtımı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2ff1525828-9a90-4fcf-be48-268cdd02361e)|f1525828-9a90-4fcf-be48-268cdd02361e|
    |Güvenlik önerileri için iş akışı Otomasyonu|[Azure Güvenlik Merkezi önerileri için İş Akışı Otomasyonu dağıtımı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f73d6ab6c-2475-4850-afd6-43795f3492ef)|73d6ab6c-2475-4850-afd6-43795f3492ef|
    |Yasal uyumluluk değişiklikleri için iş akışı Otomasyonu|[Azure Güvenlik Merkezi mevzuatı uyumluluğu için Iş akışı Otomasyonu dağıtma](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f73d6ab6c-509122b9-ddd9-47ba-a5f1-d0dac20be63c)|509122b9-ddd9-47ba-A5F1-d0dac20be63c|
    ||||

    > [!TIP]
    > Ayrıca, Azure Ilkesi 'ni arayarak bunları bulabilirsiniz:
    > 1. Azure Ilkesini açın.
    > :::image type="content" source="./media/continuous-export/opening-azure-policy.png" alt-text="Azure Ilkesine erişme":::
    > 2. Azure Ilke menüsünden **tanımlar** ' ı seçin ve adları ada göre arayın. 

1. İlgili Azure Ilkesi sayfasında **ata**' yı seçin.
    :::image type="content" source="./media/workflow-automation/export-policy-assign.png" alt-text="Azure Ilkesi atama":::

1. Her sekmeyi açın ve parametreleri istediğiniz şekilde ayarlayın:
    1. **Temel bilgiler** sekmesinde, ilke kapsamını ayarlayın. Merkezi Yönetim 'i kullanmak için, ilkeyi iş akışı Otomasyonu yapılandırmasını kullanacak olan abonelikleri içeren yönetim grubuna atayın. 
    1. **Parametreler** sekmesinde, kaynak grubu ve veri türü ayrıntılarını ayarlayın. 
        > [!TIP]
        > Her parametrenin size sunulan seçenekleri açıklayan bir araç ipucu vardır.
        >
        > Azure Ilkesinin Parametreler sekmesi (1), güvenlik merkezi 'nin iş akışı Otomasyonu sayfası (2) olarak benzer yapılandırma seçeneklerine erişim sağlar.
        > :::image type="content" source="./media/workflow-automation/azure-policy-next-to-workflow-automation.png" alt-text="Azure Ilkesiyle iş akışı otomasyonunda parametreleri karşılaştırma" lightbox="./media/workflow-automation/azure-policy-next-to-workflow-automation.png":::

    1. İsteğe bağlı olarak, bu atamayı mevcut aboneliklere uygulamak için **Düzeltme** sekmesini açın ve bir düzeltme görevi oluşturma seçeneğini belirleyin.

1. Özet sayfasını gözden geçirin ve **Oluştur**' u seçin.


## <a name="data-types-schemas"></a>Veri türleri şemaları

Mantıksal uygulama örneğine geçirilen güvenlik uyarıları veya öneriler olaylarının Ham olay şemalarını görüntülemek için, [Iş akışı Otomasyonu veri türleri şemalarını](https://aka.ms/ASCAutomationSchemas)ziyaret edin. Bu, yukarıda bahsedilen güvenlik merkezi 'nin yerleşik Logic Apps bağlayıcılarını kullanmadığınız, ancak mantıksal uygulamanın genel HTTP bağlayıcısını kullanan durumlarda yararlı olabilir-olay JSON şemasını, uygun gördüğünüz şekilde el ile ayrıştırmanız için kullanabilirsiniz.


## <a name="faq-for-workflow-automation"></a>İş akışı otomasyonu hakkında SSS

### <a name="does-workflow-automation-support-any-business-continuity-or-disaster-recovery-bcdr-scenarios"></a>İş akışı Otomasyonu, iş sürekliliği veya olağanüstü durum kurtarma (BCDR) senaryolarını destekler mi?

Ortamınızı, hedef kaynağın bir kesinti veya başka bir olağanüstü durum yaşayan BCDR senaryolarında hazırlarken, Azure Event Hubs, Log Analytics çalışma alanı ve mantıksal uygulama yönergelerine göre yedeklemeler kurarak, kuruluşun veri kaybını önleme sorumluluğu vardır.

Her etkin otomasyon için özdeş (devre dışı) bir Otomasyon oluşturmanızı ve farklı bir konumda depolamanızı öneririz. Bir kesinti olduğunda, bu yedekleme önerilerini etkinleştirebilir ve normal işlemleri koruyabilirsiniz.

[Azure Logic Apps Için iş sürekliliği ve olağanüstü durum kurtarma](../logic-apps/business-continuity-disaster-recovery-guidance.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Logic Apps oluşturma, güvenlik merkezi 'nde yürütmesini otomatikleştirme ve bunları el ile çalıştırma hakkında bilgi edindiniz.

İlgili malzemeler için bkz.: 

- [Bir güvenlik yanıtını otomatikleştirmek için iş akışı Otomasyonu 'nu kullanma hakkında Microsoft Learn modülü](/learn/modules/resolve-threats-with-azure-security-center/)
- [Azure Güvenlik Merkezi'nde güvenlik önerileri](security-center-recommendations.md)
- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](security-center-alerts-overview.md)
- [Azure Logic Apps hakkında](../logic-apps/logic-apps-overview.md)
- [Azure Logic Apps için Bağlayıcılar](../connectors/apis-list.md)
- [İş akışı otomasyonu veri türleri şemaları](https://aka.ms/ASCAutomationSchemas)