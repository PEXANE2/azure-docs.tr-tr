---
title: 'Öğretici: Azure bütçesi oluşturma ve yönetme'
description: Bu öğretici, kullandığınız Azure hizmetlerinin maliyetlerini planlayıp hesaba aktarmanıza yardımcı olur.
author: bandersmsft
ms.author: banders
ms.date: 03/09/2021
ms.topic: tutorial
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: adwise
ms.custom: seodec18
ms.openlocfilehash: 2c0853b68e432fa94e661e68486b9e58b21aa845
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103012456"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Öğretici: Azure bütçesi oluşturma ve yönetme

Maliyet Yönetimi hizmetindeki bütçe işlevi, kuruluşunuzda sorumluluk kültürünü planlamanıza ve güçlendirmenize yardımcı olur. Bütçeler, maliyetleri önceden yönetmek için diğer kullanıcıları harcamaları konusunda bilgilendirmenize ve zaman içindeki harcamayı izlemenize yardımcı olur. Harcamalarınızı kurumsal harcama sınırınızı dahilinde olduğundan emin olmak için gerçek maliyetinizi veya tahmin ettiğiniz maliyeti temel alan uyarıları yapılandırabilirsiniz. Oluşturduğunuz bütçe eşikleri aşıldığında yalnızca bildirimler tetiklenir. Kaynaklarınızın hiçbiri etkilenmez ve tüketiminiz durdurulmaz. Bütçeleri kullanarak maliyet analizi sırasında harcamaları karşılaştırabilir ve izleyebilirsiniz.

Maliyet ve kullanım verileri genellikle 8-24 saat içinde kullanıma sunulur ve bütçeler 24 saatte bir bu maliyetlere göre değerlendirilir. [Maliyet ve kullanım verileri güncelleştirmelerinin](./understand-cost-mgt-data.md#cost-and-usage-data-updates-and-retention) özelliklerini öğrendiğinizden emin olun. Bir bütçe eşiği karşılandığında e-posta bildirimleri genellikle değerlendirmeyi izleyen bir saat içinde gönderilir.

Gelecekte bir son kullanma tarihi seçtiğinizde bütçeler, dönem sonunda (ay, üç ay veya yıl) otomatik olarak aynı bütçe tutarına geri döner. Aynı bütçe tutarına geri döndükleri için bütçedeki para birimlerinin ilerleyen dönemlerde farklı olması halinde ayrı bütçeler oluşturmanız gerekir. Bir bütçenin süresi dolmuşsa, otomatik olarak silinir.

Bu öğreticideki örnekler, Azure Kurumsal Anlaşma (EA) aboneliği için bütçe oluşturma ve düzenleme konusunda size yol gösterecektir.

Azure harcamalarınızı izleme amacıyla bütçe oluşturma konusunda bilgi edinmek için [Azure portalını kullanarak aboneliklere bütçe uygulama](https://www.youtube.com/watch?v=UrkHiUx19Po) videosunu izleyin. Diğer videoları izlemek için [Maliyet Yönetimi YouTube kanalını](https://www.youtube.com/c/AzureCostManagement) ziyaret edin.

>[!VIDEO https://www.youtube.com/embed/UrkHiUx19Po]

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalında bütçe oluşturma
> * PowerShell ile bütçe oluşturma ve düzenleme
> * Bir Azure Resource Manager şablonuyla bütçe oluşturma

## <a name="prerequisites"></a>Ön koşullar

Bütçeler, aşağıdaki Azure hesap türleri ve kapsamlar için desteklenir:

- Azure rol tabanlı erişim denetimi (Azure RBAC) kapsamları
    - Yönetim grupları
    - Abonelik
- Kurumsal Anlaşma kapsamları
    - Fatura hesabı
    - Bölüm
    - Kayıt hesabı
- Bireysel anlaşmalar
    - Fatura hesabı
- Microsoft Müşteri Sözleşmesi kapsamları
    - Fatura hesabı
    - Faturalama profili
    - Fatura bölümü
    - Müşteri
- AWS kapsamları
    - Dış hesap
    - Dış abonelik


Bütçeleri görüntülemek için Azure hesabınızda en azından okuma yetkisine sahip olmanız gerekir.

Yeni bir aboneliğiniz varsa hemen bir bütçe oluşturamaz veya diğer Maliyet Yönetimi özelliklerini kullanamazsınız. Maliyet Yönetimi özelliklerini kullanabilmek için 48 saat kadar beklemeniz gerekebilir.

Azure EA aboneliklerinde bütçeleri görüntülemek için okuma yetkisine sahip olmanız gerekir. Bütçe oluşturmak ve yönetmek için katkıda bulunan izninizin olması gerekir.

Aşağıdaki Azure izinleri veya kapsamları abonelik başına kullanıcı ve grup tarafından bütçe oluşturulması için desteklenir. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](understand-work-scopes.md).

- Sahip – Bir abonelik için bütçe oluşturabilir, değiştirebilir veya silebilir.
- Katkıda bulunan ve Maliyet Yönetimi katkıda bulunanı - Kendi bütçelerini oluşturabilir, değiştirebilir veya silebilir. Başkaları tarafından oluşturulan bütçelerin miktarlarını değiştirebilir.
- Okuyucu ve Maliyet Yönetimi okuyucusu - İzin verilen bütçeleri görüntüleyebilir.

Maliyet Yönetimi verilerine izin atama hakkında daha fazla bilgi için bkz. [Maliyet Yönetimi verilerine erişim atama](./assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

- [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-budget-in-the-azure-portal"></a>Azure portalında bütçe oluşturma

Aylık, üç aylık veya yıllık dönem için bir Azure abonelik bütçesi oluşturabilirsiniz.

Bir bütçe oluşturmak veya görüntülemek için, Azure portal bir kapsam açın ve menüdeki **bütçeler** ' ı seçin. Örneğin **Abonelikler**'e gidin, listeden bir abonelik belirleyin ve menüden **Bütçeler**'i seçin. Bütçeler içindeyken yönetim grubu gibi farklı bir kapsama geçiş yapmak için **Kapsam** düğmesini kullanın. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](understand-work-scopes.md).

Bütçeleri oluşturduktan sonra bunlar için geçerli harcamalarınızın basit bir görünümü gösterilir.

**Add (Ekle)** seçeneğini belirleyin.

:::image type="content" source="./media/tutorial-acm-create-budgets/budgets-cost-management.png" alt-text="Zaten oluşturulan bütçelerin listesini gösteren screenthost." lightbox="./media/tutorial-acm-create-budgets/budgets-cost-management.png" :::

**Bütçe oluştur** penceresinde gösterilen kapsamın doğru olduğundan emin olun. Eklemek istediğiniz filtreleri seçin. Filtreler, bir abonelikteki kaynak grupları veya sanal makine hizmetleri gibi belirli maliyetlerle ilgili bütçeler oluşturmanızı sağlar. Maliyet analizinde kullanılan filtreler, bütçelere de uygulanabilir.

Kapsamınızı ve filtrelerinizi tanımladıktan sonra bir bütçe adı yazın. Ardından aylık, üç aylık veya yıllık bütçe sıfırlama dönemi seçin. Sıfırlama süresi, bütçe tarafından çözümlenen zaman penceresini belirler. Bütçe tarafından değerlendirilen maliyet her yeni dönemin başlangıcında sıfırdan başlar. Üç aylık bir bütçe oluşturduğunuzda aylık bütçeyle aynı şekilde çalışır. Aradaki fark, üç aylık bütçe tutarının üç ay arasına eşit olarak bölünmesidir. Yıllık bütçe miktarı, takvim yılının 12 ayına eşit şekilde bölünür.

Kullandıkça Öde, MSDN veya Visual Studio aboneliğiniz varsa faturanızdaki fatura dönemi takvim ayından farklı olabilir. Bu abonelik türleri ve kaynak grupları için fatura döneminize veya takvim aylarına uyan bir bütçe oluşturabilirsiniz. Fatura döneminize uygun bir bütçe oluşturmak için **Faturalama ayı**, **Faturalama üç aylık dönemi** veya **Faturalama yılı** gibi bir sıfırlama dönemi seçin. Takvim ayına uygun bir bütçe oluşturmak için **Aylık**, **Üç Aylık** veya **Yıllık** sıfırlama dönemi seçin.

Ardından bütçenin geçersiz hale geldiği ve maliyetlerinizi değerlendirmeyi durdurduğu sona erme tarihini belirleyebilirsiniz.

Bütçede şu ana kadar seçtiğiniz alanlara bağlı olarak, bütçeniz için kullanılacak eşik değerini seçmenize yardımcı olacak bir grafik gösterilir. Önerilen bütçe, gelecekteki dönemlerde tabi olabileceğiniz en yüksek tahmini maliyeti temel alır. Bütçe tutarını değiştirebilirsiniz.

:::image type="content" source="./media/tutorial-acm-create-budgets/create-monthly-budget.png" alt-text="Aylık maliyet verileriyle bütçe oluşturmayı gösteren ekran görüntüsü." lightbox="./media/tutorial-acm-create-budgets/create-monthly-budget.png" :::

Bütçe tutarını yapılandırdıktan sonra, bütçe uyarılarını gerçek maliyet ve tahmini bütçe uyarıları için yapılandırmak üzere **İleri** ' yi seçin.

## <a name="configure-actual-costs-budget-alerts"></a>Gerçek maliyet bütçe uyarılarını yapılandırma

Bütçeler için en az bir maliyet eşiği (bütçe yüzdesi) ve buna karşılık gelen bir e-posta adresi gerekir. İsteğe bağlı olarak tek bir bütçeye en fazla beş eşik ve beş e-posta adresi ekleyebilirsiniz. Bir bütçe eşiği karşılandığında e-posta bildirimleri genellikle değerlendirmeyi izleyen bir saat içinde gönderilir. Gerçek maliyetler bütçe uyarıları, yapılandırılan bütçe eşikleriyle ilgili olarak tahakkuk ettiğiniz gerçek maliyet için oluşturulur.

## <a name="configure-forecasted-budget-alerts"></a>Tahmin edilen bütçe uyarılarını yapılandırma

Tahmin edilen uyarılar, harcama eğilimlerinizin bütçenize aşmaya karşı gelişmiş bildirim sağlar. Uyarılar [Tahmini maliyet tahminleri](quick-acm-cost-analysis.md#understand-forecast)kullanır. Tahmin edilen maliyet projeksiyonu ayarlanan eşiği aştığında uyarılar oluşturulur. Tahmin edilen eşiği (bütçe yüzdesi) yapılandırabilirsiniz. Tahmin edilen bir bütçe eşiği karşılandığında, bildirimler normalde değerlendirmenin bir saat içinde gönderilir.

Gerçek ve tahmini maliyet uyarısı yapılandırma arasında geçiş yapmak için, `Type` aşağıdaki görüntüde gösterildiği gibi uyarıyı yapılandırırken alanını kullanın.

E-posta almak istiyorsanız, e-postaların önemsiz e-posta klasörünüze gitmemesi için azure-noreply@microsoft.com adresini onaylı gönderenler listenize ekleyin. Bildirimleri hakkında daha fazla bilgi için, bkz. [Maliyeti uyarılarını kullanma](./cost-mgt-alerts-monitor-usage-spending.md).

Aşağıdaki örnekte, bütçenin %90 ' ına ulaşıldığında bir e-posta uyarısı oluşturulur. Bütçeler API'si ile bütçe oluşturduğunuzda kullanıcılara uyarı gönderilmesi için roller de atayabilirsiniz. Kişilere rol atama işlemi Azure portalında gerçekleştirilemez. Azure Bütçeler API'si hakkında daha fazla bilgi için bkz. [Bütçeler API'si](/rest/api/consumption/budgets). Farklı bir dilde yazılmış bir e-posta uyarısı göndermek istiyorsanız bkz. [Bütçe uyarısı e-postaları için desteklenen yerel ayarlar](manage-automation.md#supported-locales-for-budget-alert-emails).

Uyarı limitleri, sağladığınız bütçe eşiğinin% 0,01 ' a kadar %1000 ' e kadar olan bir aralığı destekler.

:::image type="content" source="./media/tutorial-acm-create-budgets/budget-set-alert.png" alt-text="Uyarı koşullarını gösteren ekran görüntüsü." lightbox="./media/tutorial-acm-create-budgets/budget-set-alert.png" :::

Bir bütçe oluşturulduktan sonra maliyet analizi bölümünde gösterilir. Bütçenizin harcama eğilimiyle karşılaştırmalı olarak görüntülenmesi, [maliyetlerinizi ve harcamalarınızı analiz etmeye](./quick-acm-cost-analysis.md) başladığınızda kullanacağınız ilk adımlardan biridir.

:::image type="content" source="./media/tutorial-acm-create-budgets/cost-analysis.png" alt-text="Maliyet Analizi 'nde harcama ile örnek bir bütçe gösteren ekran görüntüsü." lightbox="./media/tutorial-acm-create-budgets/cost-analysis.png" :::

Yukarıdaki örnekte abonelik için bir bütçe oluşturdunuz. Bir kaynak grubu için de bütçe oluşturabilirsiniz. Bir kaynak grubu için bütçe oluşturmak isterseniz **Maliyet Yönetimi + Faturalandırma** &gt; **Abonelikler** &gt; yolunu izleyin, bir abonelik seçin > **Kaynak grupları** > bir kaynak grubu seçin > **Bütçeler** > ve **Ekle**'yi seçerek bir bütçe ekleyin.

### <a name="create-a-budget-for-combined-azure-and-aws-costs"></a>Birleşik Azure ve AWS maliyetlerine yönelik bütçe oluşturma

Bağlayıcınıza, birleştirilmiş ve bağlı hesaplarıyla birlikte bir yönetim grubu atayarak Azure ve AWS maliyetlerinizi birlikte gruplandırabilirsiniz. Azure aboneliklerinizi aynı yönetim grubuna atayın. Daha sonra birleşik maliyetler için bir bütçe oluşturun.

1. Maliyet Yönetimi’nde **Bütçeler**’i seçin.
1. **Add (Ekle)** seçeneğini belirleyin.
1. **Kapsamı değiştir**’i ve sonra yönetim grubunu seçin.
1. Tamamlanana kadar bütçeyi oluşturmaya devam edin.

## <a name="costs-in-budget-evaluations"></a>Bütçe maliyeti değerlendirmeleri

Bütçe maliyeti değerlendirmeleri artık ayrılmış örnek ve satın alma verilerini de içeriyor. Ücretlerin sizin için geçerli olması halinde değerlendirmelerinize dahil ettiğiniz ücretler için uyarılar alabilirsiniz. Bütçe eşiklerinin yeni maliyetler için hesaba uygun şekilde yapılandırıldığını doğrulamak için [Azure Portal](https://portal.azure.com) oturum açın. Faturalandırılan Azure ücretleriniz değişmez. Bütçeler artık maliyetlerinizin daha kapsamlı bir kümesine göre değerlendiriliyor. Ücretler sizin için uygun değilse bütçe davranışlarınız aynı şekilde kalır.

Bütçelerin yalnızca birinci taraf Azure tüketim ücretlerine göre değerlendirilmesi için yeni maliyetleri filtrelemek istiyorsanız, bütçenize aşağıdaki filtreleri ekleyin:

- Yayımcı Türü: Azure
- Ücret Türü: Kullanım

Bütçe maliyet değerlendirmeleri, gerçek maliyeti temel alır. Amorti edilen maliyetler kullanılmaz. Bütçelerde kullanabileceğiniz filtreleme seçenekleri hakkında daha fazla bilgi için bkz. [Gruplandırma ve filtreleme seçeneklerini anlama](group-filter.md).

## <a name="trigger-an-action-group"></a>Bir eylem grubunu tetikleme

Bir abonelik veya kaynak grubu kapsamı için bütçe oluşturma veya düzenleme aşamasında eylem grubu çağıracak şekilde yapılandırabilirsiniz. Bütçe eşiğinize ulaşıldığında eylem grubu çeşitli eylemler gerçekleştirebilir. 

Eylem Grupları şu anda yalnızca abonelik ve kaynak grubu kapsamları için desteklenmektedir. Eylem grupları oluşturma hakkında daha fazla bilgi için bkz. [temel eylem grubu ayarlarını yapılandırma](../../azure-monitor/alerts/action-groups.md#configure-basic-action-group-settings). 

Eylem gruplarıyla bütçe tabanlı otomasyon kullanma hakkında daha fazla bilgi için bkz. [Azure bütçeleri ile maliyetleri yönetme](../manage/cost-management-budget-scenario.md).

Eylem grupları oluşturmak veya güncelleştirmek için, bir bütçe oluştururken veya düzenlenirken **Eylem grubunu yönet** ' i seçin.

:::image type="content" source="./media/tutorial-acm-create-budgets/trigger-action-group.png" alt-text="Eylem gruplarını yönetme işlemini göstermek için bir bütçe oluşturma örneği gösteren ekran görüntüsü." lightbox="./media/tutorial-acm-create-budgets/trigger-action-group.png" :::

Ardından eylem grubunu oluşturmak için **Eylem grubu ekle**'yi seçin.

Bütçelerin eylem gruplarıyla tümleştirilmesi yalnızca ortak uyarı şeması devre dışı bırakılmış olan eylem grupları için kullanılabilir. Şemayı devre dışı bırakma hakkında daha fazla bilgi için bkz. [Ortam uyarı şemasını nasıl devre dışı bırakabilirim?](../../azure-monitor/alerts/alerts-common-schema.md#how-do-i-enable-the-common-alert-schema)

## <a name="create-and-edit-budgets-with-powershell"></a>PowerShell ile bütçe oluşturma ve düzenleme

Bir EA müşterisiyseniz, Azure PowerShell modülünü kullanarak program aracılığıyla bütçeleri oluşturabilir ve düzenleyebilirsiniz. Azure PowerShell'in son sürümünü indirmek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
install-module -name Az
```

Aşağıdaki örnek komutlar bir bütçe oluşturur.

```azurepowershell-interactive
#Sign into Azure Powershell with your account

Connect-AzAccount

#Select a subscription to to monitor with a budget

select-AzSubscription -Subscription "Your Subscription"

#Create an action group email receiver and corresponding action group

$email1 = New-AzActionGroupReceiver -EmailAddress test@test.com -Name EmailReceiver1
$ActionGroupId = (Set-AzActionGroup -ResourceGroupName YourResourceGroup -Name TestAG -ShortName TestAG -Receiver $email1).Id

#Create a monthly budget that sends an email and triggers an Action Group to send a second email. Make sure the StartDate for your monthly budget is set to the first day of the current month. Note that Action Groups can also be used to trigger automation such as Azure Functions or Webhooks.

New-AzConsumptionBudget -Amount 100 -Name TestPSBudget -Category Cost -StartDate 2020-02-01 -TimeGrain Monthly -EndDate 2022-12-31 -ContactEmail test@test.com -NotificationKey Key1 -NotificationThreshold 0.8 -NotificationEnabled -ContactGroup $ActionGroupId
```

## <a name="create-a-budget-with-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonuyla bütçe oluşturma

Bir Azure Resource Manager şablonu kullanarak bütçe oluşturabilirsiniz. Şablonu kullanmak için bkz. [Azure Resource Manager şablonuyla bütçe oluşturma](quick-create-budget-template.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bütçe oluşturduysanız ve artık buna ihtiyacınız yoksa ayrıntılarını görüntüleyin ve silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure portalında bütçe oluşturma
> * PowerShell ile bütçe oluşturma ve düzenleme
> * Bir Azure Resource Manager şablonuyla bütçe oluşturma

Maliyet yönetimi verileriniz için yinelenen dışarı aktarma işlemi oluşturmak üzere sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Dışarı aktarılan verileri oluşturma ve yönetme](tutorial-export-acm-data.md)