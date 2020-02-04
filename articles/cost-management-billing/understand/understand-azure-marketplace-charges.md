---
title: Azure dış hizmet ücretlerinizi anlama | Microsoft Docs
description: Azure’da, eski adıyla Market olarak bilinen dış hizmet ücretlerinin faturalaması hakkında bilgi edinin.
author: jureid
manager: jureid
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f94b79e03cf97275ec4dacf17e097e1678e81d10
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "75989429"
---
# <a name="understand-your-azure-external-services-charges"></a>Azure dış hizmet ücretlerinizi anlama
Dış hizmetler, Azure markette üçüncü taraf yazılım satıcıları tarafından yayımlanır. Örneğin, SendGrid, Azure’da satın alabileceğiniz, ancak Microsoft tarafından yayımlanmayan bir dış hizmettir. Bazı Microsoft ürünleri de Azure Market aracılığıyla satılır.

## <a name="how-external-services-are-billed"></a>Dış hizmetleri faturalandırma

- Microsoft Müşteri Sözleşmeniz (MCA) veya Microsoft İş Ortağı Sözleşmeniz (MPA) varsa üçüncü taraf hizmetleriniz, Azure hizmetlerinizin geri kalanıyla faturalandırılır. MCA veya MPA’ya erişiminizin olup olmadığını görmek için [ödeme hesabınızın türünü denetleyin](#check-billing-account-type).
- MCA veya MPA’nız yoksa dış hizmetleriniz Azure hizmetlerinizden ayrı olarak faturalandırılır.
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

## <a name="view-and-download-invoices-for-external-services"></a>Dış hizmetler için fatura görüntüleme ve indirme

Microsoft Müşteri Sözleşmeniz (MCA) veya Microsoft İş Ortağı Sözleşmeniz (MPA) varsa üçüncü taraf hizmetleriniz, Azure hizmetlerinizin geri kalanıyla faturalandırılır. MCA veya MPA’ya erişiminizin olup olmadığını görmek için [ödeme hesabınızın türünü denetleyin](#check-billing-account-type). Erişiminiz varsa, üçüncü taraf ücretlerinizi görmek için bkz. [Azure portalından faturalarınızı görüntüleme ve indirme](download-azure-invoice.md).

MCA veya MPA’nız yoksa üçüncü taraf ücretler için ayrı faturalarınız olur. Şu adımları izleyerek Azure portalından Azure Market faturalarınızı görüntüleyebilir ve indirebilirsiniz:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.
1. Soldaki menüden **Faturalar**’ı seçin.
1. **Azure Market ve Rezervasyonlar** sekmesine tıklayın.  ![Azure market ve rezervasyonlar sekmesinin resmi](./media/understand-azure-marketplace-charges/invoice-tabs.png)
1. Abonelik açılan listesinde, faturalarını görmek istediğiniz dış hizmetleri içeren aboneliği seçin.

## <a name="external-spending-for-ea-customers"></a>Kurumsal Anlaşma müşterileri için dış harcama

Kurumsal Anlaşma (EA) müşterileri, EA portalında dış hizmet harcamasını görebilir ve raporları indirebilir. Kullanmaya başlamak için bkz. [Kurumsal Anlaşma Müşterileri için Azure Market](https://ea.azure.com/helpdocs/azureMarketplace).

## <a name="manage-payment-for-external-services"></a>Dış hizmetler için ödemeyi yönetme

Dış hizmet satın alırken, kaynak için bir Azure aboneliği seçersiniz. Seçilen Azure aboneliğinin ödeme yöntemi, dış hizmetin ödeme yöntemi olur. Bir dış hizmetin ödeme yöntemini değiştirmek için, o dış hizmete bağlı olan [Azure aboneliğinin ödeme yöntemini değiştirmeniz](../manage/change-credit-card.md) gerekir. Aşağıdaki adımları izleyerek dış hizmet siparişinizin hangi aboneliğe bağlı olduğunu öğrenebilirsiniz:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol gezinti menüsünde **Tüm Kaynaklar**’a tıklayın.
     ![tüm kaynaklar menü öğesinin ekran görüntüsü](./media/understand-azure-marketplace-charges/all-resources.png)
1. Dış hizmetinizi arayın.
1. **Abonelik** sütununda aboneliğin adını bulun.
    ![kaynak için abonelik adının ekran görüntüsü](./media/understand-azure-marketplace-charges/sub-selected.png)
1. Abonelik adına tıklayın ve [etkin ödeme yöntemini güncelleştirin](../manage/change-credit-card.md).

<!-- Update your payment methods for external service orders from the [Account Center](https://account.windowsazure.com/).

> [!NOTE]
> If you purchased your subscription with a Work or School account, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to make changes to your payment method.

1. Sign in to the [Account Center](https://account.windowsazure.com/) and [navigate to the **marketplace** tab](https://account.windowsazure.com/Store)

    ![Select marketplace in the account center](./media/understand-azure-marketplace-charges/select-marketplace.png)
2. Select the external service you want to manage

    ![Select the external service you want to manage](./media/understand-azure-marketplace-charges/select-ext-service.png)
3. Click **Change payment method** on the right side of the page. This link brings you to a different portal to manage your payment method.

    ![Order summary](./media/understand-azure-marketplace-charges/change-payment.PNG)
4. Click **Edit info** and follow instructions to update your payment information.

    ![Select edit info](./media/understand-azure-marketplace-charges/edit-info.png) -->

## <a name="cancel-an-external-service-order"></a>Dış hizmet siparişini iptal etme
Dış hizmet siparişinizi iptal etmek istiyorsanız, [Azure portalında](https://portal.azure.com) kaynağı silin.

1. [Azure Portal](https://portal.azure.com) oturum açın.
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
