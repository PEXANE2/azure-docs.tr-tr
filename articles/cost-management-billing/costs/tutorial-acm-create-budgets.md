---
title: 'Öğretici: Azure bütçesi oluşturma ve yönetme'
description: Bu öğretici, kullandığınız Azure hizmetlerinin maliyetleriyle ilgili plan yapmanıza yardımcı olmaktadır.
author: bandersmsft
ms.author: banders
ms.date: 07/15/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: adwise
ms.custom: seodec18
ms.openlocfilehash: 3572cbb3f8c4a4f20c0141ac1fae5f0aa6fbd216
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87044994"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Öğretici: Azure bütçesi oluşturma ve yönetme

Maliyet Yönetimi hizmetindeki bütçe işlevi, kuruluşunuzda sorumluluk kültürünü planlamanıza ve güçlendirmenize yardımcı olur. Bütçeleri kullanarak belirli bir dönem içinde kullandığınız veya abone olduğunuz Azure hizmetlerini takip edebilirsiniz. Bütçeler, maliyetleri önceden yönetmek için diğer kullanıcıları harcamaları konusunda bilgilendirmenize ve zaman içindeki harcamayı izlemenize yardımcı olur. Oluşturduğunuz bütçe eşikleri aşıldığında yalnızca bildirimler tetiklenir. Kaynaklarınızın hiçbiri etkilenmez ve tüketiminiz durdurulmaz. Bütçeleri kullanarak maliyet analizi sırasında harcamaları karşılaştırabilir ve izleyebilirsiniz.

Maliyet ve kullanım verileri genellikle 20 saat içinde kullanıma sunulur ve bütçeler 12-14 saatte bir bu maliyetlere göre değerlendirilir. Bir bütçe eşiği karşılandığında e-posta bildirimleri genellikle değerlendirmeyi izleyen bir saat içinde gönderilir.

Gelecekte bir son kullanma tarihi seçtiğinizde bütçeler, dönem sonunda (ay, üç ay veya yıl) otomatik olarak aynı bütçe tutarına geri döner. Aynı bütçe tutarına geri döndükleri için bütçedeki para birimlerinin ilerleyen dönemlerde farklı olması halinde ayrı bütçeler oluşturmanız gerekir.

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

- Azure Rol Tabanlı Erişim Denetimi kapsamları
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

Maliyet Yönetimi verilerine izin atama hakkında daha fazla bilgi için bkz. [Maliyet Yönetimi verilerine erişim atama](../../cost-management/assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

- [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-budget-in-the-azure-portal"></a>Azure portalında bütçe oluşturma

Aylık, üç aylık veya yıllık dönem için bir Azure abonelik bütçesi oluşturabilirsiniz.

Bütçe oluşturmak veya görüntülemek için, Azure portalında istediğiniz kapsamı açın ve menüden **Bütçeler**'i seçin. Örneğin **Abonelikler**'e gidin, listeden bir abonelik belirleyin ve menüden **Bütçeler**'i seçin. Bütçeler içindeyken yönetim grubu gibi farklı bir kapsama geçiş yapmak için **Kapsam** düğmesini kullanın. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](understand-work-scopes.md).

Bütçeleri oluşturduktan sonra bunlar için geçerli harcamalarınızın basit bir görünümü gösterilir.

**Add (Ekle)** seçeneğini belirleyin.

![Önceden oluşturulmuş olan bütçelerin listesini gösteren örnek](./media/tutorial-acm-create-budgets/budgets01.png)

**Bütçe oluştur** penceresinde gösterilen kapsamın doğru olduğundan emin olun. Eklemek istediğiniz filtreleri seçin. Filtreler, bir abonelikteki kaynak grupları veya sanal makine hizmetleri gibi belirli maliyetlerle ilgili bütçeler oluşturmanızı sağlar. Maliyet analizinde kullanılan filtreler, bütçelere de uygulanabilir.

Kapsamınızı ve filtrelerinizi tanımladıktan sonra bir bütçe adı yazın. Ardından aylık, üç aylık veya yıllık bütçe sıfırlama dönemi seçin. Bu sıfırlama dönemi, bütçe tarafından çözümlenen zaman penceresini belirler. Bütçe tarafından değerlendirilen maliyet her yeni dönemin başlangıcında sıfırdan başlar. Üç aylık bir bütçe oluşturduğunuzda aylık bütçeyle aynı şekilde çalışır. Aradaki fark, üç aylık bütçe tutarının üç ay arasına eşit olarak bölünmesidir. Yıllık bütçe miktarı, takvim yılının 12 ayına eşit şekilde bölünür.

Kullandıkça Öde, MSDN veya Visual Studio aboneliğiniz varsa faturanızdaki fatura dönemi takvim ayından farklı olabilir. Bu abonelik türleri ve kaynak grupları için fatura döneminize veya takvim aylarına uyan bir bütçe oluşturabilirsiniz. Fatura döneminize uygun bir bütçe oluşturmak için **Faturalama ayı**, **Faturalama üç aylık dönemi** veya **Faturalama yılı** gibi bir sıfırlama dönemi seçin. Takvim ayına uygun bir bütçe oluşturmak için **Aylık**, **Üç Aylık** veya **Yıllık** sıfırlama dönemi seçin.

Ardından bütçenin geçersiz hale geldiği ve maliyetlerinizi değerlendirmeyi durdurduğu sona erme tarihini belirleyebilirsiniz.

Bütçede şu ana kadar seçtiğiniz alanlara bağlı olarak, bütçeniz için kullanılacak eşik değerini seçmenize yardımcı olacak bir grafik gösterilir. Önerilen bütçe, gelecekteki dönemlerde tabi olabileceğiniz en yüksek tahmini maliyeti temel alır. Bütçe tutarını değiştirebilirsiniz.

![Aylık maliyet verileriyle bütçe oluşturma işlemini gösteren örnek ](./media/tutorial-acm-create-budgets/monthly-budget01.png)

Bütçe tutarını yapılandırdıktan sonra bütçe uyarılarını yapılandırmak için **İleri**'yi seçin. Bütçeler için en az bir maliyet eşiği (bütçe yüzdesi) ve buna karşılık gelen bir e-posta adresi gerekir. İsteğe bağlı olarak tek bir bütçeye en fazla beş eşik ve beş e-posta adresi ekleyebilirsiniz. Bir bütçe eşiği karşılandığında e-posta bildirimleri genellikle değerlendirmeyi izleyen bir saat içinde gönderilir.

E-posta almak istiyorsanız, e-postaların önemsiz e-posta klasörünüze gitmemesi için azure-noreply@microsoft.com adresini onaylı gönderenler listenize ekleyin. Bildirimleri hakkında daha fazla bilgi için, bkz. [Maliyeti uyarılarını kullanma](../../cost-management/cost-mgt-alerts-monitor-usage-spending.md).

Aşağıdaki örnekte, bütçenin %90'ına ulaşıldığında bir e-posta uyarısı oluşturulmuştur. Bütçeler API'si ile bütçe oluşturduğunuzda kullanıcılara uyarı gönderilmesi için roller de atayabilirsiniz. Kişilere rol atama işlemi Azure portalında gerçekleştirilemez. Azure Bütçeler API'si hakkında daha fazla bilgi için bkz. [Bütçeler API'si](/rest/api/consumption/budgets).

Uyarı sınırları, sağladığınız bütçe eşiğinin %0,01’i ile %1000'i arasındaki değerleri destekler.

![Uyarı koşullarını gösteren örnek](./media/tutorial-acm-create-budgets/monthly-budget-alert.png)

Bir bütçe oluşturulduktan sonra maliyet analizi bölümünde gösterilir. Bütçenizin harcama eğilimiyle karşılaştırmalı olarak görüntülenmesi, [maliyetlerinizi ve harcamalarınızı analiz etmeye](../../cost-management/quick-acm-cost-analysis.md) başladığınızda kullanacağınız ilk adımlardan biridir.

![Maliyet analizinde gösterilen örnek bütçe ve harcama](./media/tutorial-acm-create-budgets/cost-analysis.png)

Yukarıdaki örnekte abonelik için bir bütçe oluşturdunuz. Bir kaynak grubu için de bütçe oluşturabilirsiniz. Bir kaynak grubu için bütçe oluşturmak isterseniz **Maliyet Yönetimi + Faturalandırma** &gt; **Abonelikler** &gt; yolunu izleyin, bir abonelik seçin > **Kaynak grupları** > bir kaynak grubu seçin > **Bütçeler** > ve **Ekle**'yi seçerek bir bütçe ekleyin.

### <a name="create-a-budget-for-combined-azure-and-aws-costs"></a>Birleşik Azure ve AWS maliyetlerine yönelik bütçe oluşturma

Bağlayıcınıza, birleştirilmiş ve bağlı hesaplarıyla birlikte bir yönetim grubu atayarak Azure ve AWS maliyetlerinizi birlikte gruplandırabilirsiniz. Azure aboneliklerinizi aynı yönetim grubuna atayın. Daha sonra birleşik maliyetler için bir bütçe oluşturun.

1. Maliyet Yönetimi’nde **Bütçeler**’i seçin.
1. **Add (Ekle)** seçeneğini belirleyin.
1. **Kapsamı değiştir**’i ve sonra yönetim grubunu seçin.
1. Tamamlanana kadar bütçeyi oluşturmaya devam edin.

## <a name="costs-in-budget-evaluations"></a>Bütçe maliyeti değerlendirmeleri

Bütçe maliyeti değerlendirmeleri artık ayrılmış örnek ve satın alma verilerini de içeriyor. Ücretlerin sizin için geçerli olması halinde değerlendirmelerinize dahil ettiğiniz ücretler için uyarılar alabilirsiniz. Bütçe eşiklerinin yeni maliyetleri dikkate alacak şekilde yapılandırıldığını doğrulamak için [Azure portalında](https://portal.azure.com) oturum açmanız önerilir. Faturalandırılan Azure ücretleriniz değişmez. Bütçeler artık maliyetlerinizin daha kapsamlı bir kümesine göre değerlendiriliyor. Ücretler sizin için uygun değilse bütçe davranışlarınız aynı şekilde kalır.

Bütçelerin yalnızca birinci taraf Azure tüketim ücretlerine göre değerlendirilmesi için yeni maliyetleri filtrelemek istiyorsanız, bütçenize aşağıdaki filtreleri ekleyin:

- Yayımcı Türü: Azure
- Ücret Türü: Kullanım

Bütçe maliyet değerlendirmeleri, gerçek maliyeti temel alır. Amorti edilen maliyetler kullanılmaz. Bütçelerde kullanabileceğiniz filtreleme seçenekleri hakkında daha fazla bilgi için bkz. [Gruplandırma ve filtreleme seçeneklerini anlama](group-filter.md).

## <a name="trigger-an-action-group"></a>Bir eylem grubunu tetikleme

Bir abonelik veya kaynak grubu kapsamı için bütçe oluşturma veya düzenleme aşamasında eylem grubu çağıracak şekilde yapılandırabilirsiniz. Bütçe eşiğinize ulaşıldığında eylem grubu çeşitli eylemler gerçekleştirebilir. Eylem Grupları şu anda yalnızca abonelik ve kaynak grubu kapsamları için desteklenmektedir. Eylem Grupları hakkında daha fazla bilgi için bkz. [Azure portalında eylem grubu oluşturma ve yönetme](../../azure-monitor/platform/action-groups.md). Eylem gruplarıyla bütçe tabanlı otomasyon kullanma hakkında daha fazla bilgi için bkz. [Azure bütçeleri ile maliyetleri yönetme](../manage/cost-management-budget-scenario.md).

Eylem grubu oluşturmak veya güncelleştirmek için bütçe oluşturma veya düzenleme aşamasında **Eylem gruplarını yönet**'i seçin.

![Eylem gruplarını yönet seçeneğini gösteren bütçe oluşturma örneği](./media/tutorial-acm-create-budgets/manage-action-groups01.png)

Ardından eylem grubunu oluşturmak için **Eylem grubu ekle**'yi seçin.

![Eylem grubu ekle kutusunun görüntüsü](./media/tutorial-acm-create-budgets/manage-action-groups02.png)

Eylem grubu oluşturulduktan sonra bütçenize geri dönmek için kutuyu kapatın.

Bütçenizi, tek bir eşik karşılandığında eylem grubunuzu kullanacak şekilde yapılandırın. En fazla beş farklı eşik desteklenir.

![Uyarı koşulu için eylem grubu seçimini gösteren örnek](./media/tutorial-acm-create-budgets/manage-action-groups03.png)

Aşağıdaki örnek %50, %75 ve %100 olarak ayarlanmış bütçe eşiklerini göstermektedir. Her biri belirlenen eylem grubundaki belirli eylemleri tetikleyecek şekilde yapılandırılmıştır.

![Farklı eylem grupları ve eylem türleriyle yapılandırılmış uyarı koşullarını gösteren örnek](./media/tutorial-acm-create-budgets/manage-action-groups04.png)

Bütçelerin eylem gruplarıyla tümleştirilmesi yalnızca ortak uyarı şeması devre dışı bırakılmış olan eylem grupları için kullanılabilir. Şemayı devre dışı bırakma hakkında daha fazla bilgi için bkz. [Ortam uyarı şemasını nasıl devre dışı bırakabilirim?](../../azure-monitor/platform/alerts-common-schema.md#how-do-i-enable-the-common-alert-schema)

## <a name="create-and-edit-budgets-with-powershell"></a>PowerShell ile bütçe oluşturma ve düzenleme

EA müşterileri, Azure PowerShell modülünü kullanarak program aracılığıyla bütçe oluşturabilir ve düzenleyebilir.  Azure PowerShell'in son sürümünü indirmek için aşağıdaki komutu çalıştırın:

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

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure portalında bütçe oluşturma
> * PowerShell ile bütçe oluşturma ve düzenleme
> * Bir Azure Resource Manager şablonuyla bütçe oluşturma

Maliyet yönetimi verileriniz için yinelenen dışarı aktarma işlemi oluşturmak üzere sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Dışarı aktarılan verileri oluşturma ve yönetme](tutorial-export-acm-data.md)
