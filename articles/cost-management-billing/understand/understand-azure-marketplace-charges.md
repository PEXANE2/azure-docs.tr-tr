---
title: Azure dış hizmet ücretlerinizi anlama
description: Azure’da, eski adıyla Market olarak bilinen dış hizmet ücretlerinin faturalaması hakkında bilgi edinin.
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 08/20/2020
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8da85ec5781ff9575cf380092fea9e41d6af8786
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88686131"
---
# <a name="understand-your-azure-external-services-charges"></a>Azure dış hizmet ücretlerinizi anlama
Dış hizmetler, Azure markette üçüncü taraf yazılım satıcıları tarafından yayımlanır. Örneğin, SendGrid, Azure’da satın alabileceğiniz, ancak Microsoft tarafından yayımlanmayan bir dış hizmettir. Bazı Microsoft ürünleri de Azure Market aracılığıyla satılır.

## <a name="how-external-services-are-billed"></a>Dış hizmetleri faturalandırma

- Microsoft Müşteri Sözleşmeniz (MCA) veya Microsoft İş Ortağı Sözleşmeniz (MPA) varsa üçüncü taraf hizmetleriniz, Azure hizmetlerinizin geri kalanıyla faturalandırılır. MCA veya MPA’ya erişiminizin olup olmadığını görmek için [ödeme hesabınızın türünü denetleyin](#check-billing-account-type).
- MCA veya MPA’nız yoksa dış hizmetleriniz Azure hizmetlerinizden ayrı olarak faturalandırılır. Her faturalama döneminde, biri Azure hizmetleri diğeri de Market’te yaptığınız satın alma işlemleri için olmak üzere iki fatura alırsınız.
- Her dış hizmetin farklı bir faturalama modeli vardır. Bazı hizmetler kullandıkça öde modeliyle faturalanırken bazılarınınsa sabit aylık ücretleri vardır.
- Dış hizmetler için aylık ücretsiz kredileri kullanamazsınız. [Ücretsiz kredileri](https://azure.microsoft.com/pricing/spending-limits/) içeren bir Azure aboneliği kullanıyorsanız bunlar harici hizmetlerin ücretlerine uygulanamaz. Yeni bir dış hizmet veya kaynak sağladığınızda bir uyarı gösterilir:

    ![Market satın alma uyarısı](./media/understand-azure-marketplace-charges/credit-warning.png)

<!-- ## View external service spending and history in the Azure portal
You can view a list of the external services that are on each subscription within the [Azure portal](https://portal.azure.com/):

1. Sign in to the [Azure portal](https://portal.azure.com/) as the account administrator.
2. In the Hub menu, select **Subscriptions**.

    ![Select Subscriptions in the Hub menu](./media/understand-azure-marketplace-charges/sub-button.png)
3. In the **Subscriptions** blade, select the subscription that you want to view, and then select **External services**.

    ![Select a subscription in the billing blade](./media/understand-azure-marketplace-charges/select-sub-external-services.png)
4. You should see each of your external service orders, the publisher name, service tier you bought, name you gave the resource, and the current order status. To see past bills, select an external service.

    ![Select an external service](./media/understand-azure-marketplace-charges/external-service-blade2.png)
5. From here, you can view past bill amounts including the tax breakdown.

    ![View external services billing history](./media/understand-azure-marketplace-charges/billing-overview-blade.png) -->

## <a name="external-spending-for-ea-customers"></a>Kurumsal Anlaşma müşterileri için dış harcama

Kurumsal Anlaşma (EA) müşterileri, EA portalında dış hizmet harcamasını görebilir ve raporları indirebilir. Kullanmaya başlamak için bkz. [Kurumsal Anlaşma Müşterileri için Azure Market](https://ea.azure.com/helpdocs/azureMarketplace).

## <a name="view-and-download-invoices-for-external-services"></a>Dış hizmetler için fatura görüntüleme ve indirme

Microsoft Müşteri Sözleşmeniz (MCA) veya Microsoft İş Ortağı Sözleşmeniz (MPA) varsa, üçüncü taraf hizmetleriniz Azure hizmetlerinizin geri kalanıyla tek bir faturada toplanır. MCA veya MPA’ya erişiminizin olup olmadığını görmek için [ödeme hesabınızın türünü denetleyin](#check-billing-account-type). Erişiminiz varsa, üçüncü taraf ücretlerinizi görmek için bkz. [Azure portalından faturalarınızı görüntüleme ve indirme](download-azure-invoice.md).

MCA veya MPA’nız yoksa üçüncü taraf ücretler için ayrı faturalarınız olur. 

Azure Market ücretleri yerel para biriminiz üzerinden gösterilir.

Şu adımları izleyerek Azure portalından Azure Market faturalarınızı görüntüleyebilir ve indirebilirsiniz:

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.
1. Soldaki menüden **Faturalar**’ı seçin.
1. Abonelik açılan menüsünde Market hizmetlerinizle ilişkili aboneliği seçin.
1. Faturalar listesinde **Tür** sütununu gözden geçirin. Market hizmetlerine yönelik bir faturanın türü **Azure Market ve Rezervasyonlar** olur. 

    ![Fatura kılavuzunda Azure Market türü ekran görüntüsü](./media/understand-azure-marketplace-charges/marketplace-type-twd.png)

1. Yalnızca Azure Market ve Rezervasyonlara yönelik faturaları görmek amacıyla türe göre filtrelemek için **Tür** filtresini seçin. Ardından, açılan menüde **Azure Market ve Rezervasyonlar** seçeneğini belirleyin.

    ![Tür filtresinin seçili olduğunu ve açılan menüden Azure Market ve Rezervasyon seçeneğinin belirlendiğini gösteren ekran görüntüsü](./media/understand-azure-marketplace-charges/type-filter.png)

1. İndirmek istediğiniz fatura için sağ tarafta bulunan indir simgesini seçin.

    ![fatura için seçili olan indir simgesini gösteren ekran görüntüsü](./media/understand-azure-marketplace-charges/download-icon-marketplace.png)

1. **Fatura** seçeneğinin altındaki mavi **İndir** düğmesini belirleyin.

    ![bağlam bölmesinde fatura için indir düğmesini gösteren ekran görüntüsü](./media/understand-azure-marketplace-charges/invoice-download-marketplace.png)

## <a name="pay-for-external-services-in-the-azure-portal"></a>Azure portalında dış hizmetler için ödeme yapma

Microsoft Müşteri Sözleşmeniz (MCA) veya Microsoft İş Ortağı Sözleşmeniz (MPA) varsa üçüncü taraf hizmetleriniz, Azure hizmetlerinizin geri kalanıyla faturalandırılır. MCA veya MPA’ya erişiminizin olup olmadığını görmek için [ödeme hesabınızın türünü denetleyin](#check-billing-account-type). Erişiminiz varsa, [Microsoft Azure faturanızı ödeme](pay-bill.md) bölümündeki adımları izleyerek faturanızın tamamını Azure portalından ödeyebilirsiniz.

MCA veya MPA’nız yoksa, Market faturalarınızı aşağıdaki adımları izleyerek Azure portalından ödeyebilirsiniz:

1. Market faturalarınızı bulmak için önceki bölümdeki ([Dış hizmetlere yönelik faturaları görüntüleme ve indirme](#view-and-download-invoices-for-external-services)) adımları izleyin.
1. Ödemek istediğiniz fatura için mavi **Şimdi ödeyin** bağlantısına tıklayın.

    ![faturalar kılavuzunda seçili Şimdi ödeyin bağlantısını gösteren ekran görüntüsü](./media/understand-azure-marketplace-charges/pay-now-twd.png)

    >[!NOTE]
    > **Şimdi ödeyin** bağlantısı, türü **Azure Market ve Rezervasyonlar** olan ve fatura ödeme durumu vadesi gelmiş veya vadesinde ödenmemiş olan faturalar için gösterilir.

1. Yeni sayfada mavi **Ödeme yöntemini seçin** bağlantısına tıklayın.

    ![ödeme yöntemini seçin bağlantısının seçili olduğu ekran görüntüsü](./media/understand-azure-marketplace-charges/select-payment-method-pay-now-twd.png)

1. Ödeme yöntemi seçtikten sonra sayfanın sol altında bulunan mavi **Şimdi ödeyin** düğmesine tıklayın.
    ![şimdi ödeyin düğmesinin seçili olduğu ekran görüntüsü](./media/understand-azure-marketplace-charges/pay-now-button-twd.png)

## <a name="change-default-payment-for-external-services"></a>Dış hizmetler için varsayılan ödemeyi yönetme

Dış hizmet satın alırken, kaynak için bir Azure aboneliği seçersiniz. Seçilen Azure aboneliğinin ödeme yöntemi, dış hizmetin ödeme yöntemi olur. Bir dış hizmetin ödeme yöntemini değiştirmek için, o dış hizmete bağlı olan [Azure aboneliğinin ödeme yöntemini değiştirmeniz](../manage/change-credit-card.md) gerekir. Aşağıdaki adımları izleyerek dış hizmet siparişinizin hangi aboneliğe bağlı olduğunu öğrenebilirsiniz:

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Sol gezinti menüsünde **Tüm Kaynaklar**’a tıklayın.
     ![tüm kaynaklar menü öğesinin ekran görüntüsü](./media/understand-azure-marketplace-charges/all-resources.png)
1. Dış hizmetinizi arayın.
1. **Abonelik** sütununda aboneliğin adını bulun.
    ![kaynak için abonelik adının ekran görüntüsü](./media/understand-azure-marketplace-charges/sub-selected.png)
1. Abonelik adına tıklayın ve [etkin ödeme yöntemini güncelleştirin](../manage/change-credit-card.md).

## <a name="cancel-an-external-service-order"></a>Dış hizmet siparişini iptal etme

Dış hizmet siparişinizi iptal etmek istiyorsanız, [Azure portalında](https://portal.azure.com) kaynağı silin.

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Sol gezinti menüsünde **Tüm Kaynaklar**’a tıklayın.
    ![Tüm kaynaklar menü öğesinin ekran görüntüsü](./media/understand-azure-marketplace-charges/all-resources.png)
1. Dış hizmetinizi arayın.
1. Silmek istediğiniz kaynağın yanındaki kutuyu işaretleyin.
1. Komut çubuğunda **Sil**’i seçin.
    ![Sil düğmesinin ekran görüntüsü](./media/understand-azure-marketplace-charges/delete-button.png)
1. Onay dikey penceresine *'Evet'* yazın.
    ![Kaynağı Sil](./media/understand-azure-marketplace-charges/delete-resource.PNG)
1. **Sil**'e tıklayın.

## <a name="check-billing-account-type"></a>Ödeme hesabı türünü denetleme
[!INCLUDE [billing-check-account-type](../../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- [Maliyet analizini başlatma](../costs/quick-acm-cost-analysis.md)