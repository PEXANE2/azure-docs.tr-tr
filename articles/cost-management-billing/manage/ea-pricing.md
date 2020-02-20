---
title: Kuruluşunuzun Azure fiyatlandırmasını görüntüleme ve indirme
description: Kuruluşunuzun fiyatlandırmasıyla maliyetlerin nasıl tahmin edileceğini ve fiyatlandırmanın nasıl görüntülenip indirileceğini öğrenin.
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: 1dd0dfad0c68e18ae272d5d94fedd67d24be9ae1
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77200787"
---
# <a name="view-and-download-your-organizations-azure-pricing"></a>Kuruluşunuzun Azure fiyatlandırmasını görüntüleme ve indirme

Azure Kurumsal Anlaşması (EA), Microsoft Müşteri Sözleşmesi (MCA) veya Microsoft İş Ortağı Sözleşmesi (MPA) olan Azure müşterileri, Azure portalında fiyatlandırmasını görüntüleyip indirebilir. [Ödeme hesabınızın türünü denetlemeyi öğrenin](#check-your-billing-account-type).

## <a name="download-pricing-for-an-enterprise-agreement"></a>Kurumsal Anlaşma için fiyatlandırmayı indirme

Kuruluş Yöneticisi tarafından kuruluşunuz için belirlenen ilkelere bağlı olarak yalnızca belirli yönetici rolleri, kuruluşunuzun Kurumsal Anlaşma fiyatlandırma bilgilerine erişim sağlar. Daha fazla bilgi için bkz. [Azure’daki Azure Kurumsal Anlaşma yönetici rollerini anlama](understand-ea-roles.md).

1. Kuruluş Yöneticisi olarak [Azure portalında](https://portal.azure.com/) oturum açın.
1. *Maliyet Yönetimi + Faturalama* araması yapın.

   ![Azure portalı aramasını gösteren ekran görüntüsü](./media/ea-pricing/portal-cm-billing-search.png)

1. Faturalama hesabı bölümünde **Kullanım + ücretler**’i seçin.

   ![Faturalama bölümünde kullanımı ve ücretleri gösteren ekran görüntüsü](./media/ea-pricing/ea-pricing-usage-charges-nav.png)

1. ![Azure portalı aramasını gösteren ekran görüntüsü](./media/ea-pricing/download-icon.png) Ay için **İndir**’i seçin.

1. **Fiyat Listesi** bölümünde **csv dosyasını indir**’i seçin.

   ![Fiyat listesi csv dosyasını indir düğmesini gösteren ekran görüntüsü](./media/ea-pricing/download-ea-price-sheet.png)

## <a name="download-pricing-for-an-mca-or-mpa-account"></a>MCA veya MPA hesabının fiyatlandırmasını indirme

MCA’nız varsa, fiyatlandırmayı görüntülemek ve indirmek için faturalama profilinin sahibi, katkıda bulunanı, okuyucusu veya fatura yöneticisi olmanız gerekir. MPA’nız varsa, fiyatlandırmayı görüntülemek ve indirmek için iş ortağı kuruluşunda Genel Yönetici veya Yönetici Aracısı rolüne sahip olmanız gerekir.

### <a name="download-price-sheets-for-billed-charges"></a>Faturalanan ücretler için fiyat listelerini indirme

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. *Maliyet Yönetimi + Faturalama* araması yapın.
1. Faturalama profilini seçin. Erişiminize bağlı olarak önce bir ödeme hesabı seçmeniz gerekebilir.
1. **Faturalar**'ı seçin.
1. Fatura kılavuzunda, indirmek istediğiniz fiyat listesine karşılık gelen fatura satırını bulun.
1. Satırın sonundaki üç noktaya (`...`) tıklayın.
![Üç noktanın seçili olduğunu gösteren ekran görüntüsü](./media/ea-pricing/billingprofile-invoicegrid-new.png)

1. Seçilen faturadaki hizmetlere ait fiyatları görmek istiyorsanız **Fatura fiyat listesi** seçeneğini belirleyin.
1. Verilen faturalama dönemine ait tüm Azure hizmetlerinin fiyatlarını görmek istiyorsanız **Azure fiyat listesi**’ni seçin.

![Fiyat listelerini içeren açılır menüyü gösteren ekran görüntüsü](./media/ea-pricing/contextmenu-pricesheet01.png)

### <a name="download-price-sheets-for-the-current-billing-period"></a>Geçerli faturalama dönemi için fiyat listelerini indirme

MCA’nız varsa, geçerli faturalama dönemi için fiyatlandırmayı indirebilirsiniz.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. *Maliyet Yönetimi + Faturalama* araması yapın.
1. Faturalama profilini seçin. Erişiminize bağlı olarak önce bir ödeme hesabı seçmeniz gerekebilir.
1. **Genel Bakış** alanında, ay başından bugüne kadarki ücretlerin altında indirme bağlantılarını bulun.
1. **Azure fiyat listesi**’ni seçin.
![Genel Bakış’tan indirme seçeneğini gösteren ekran görüntüsü](./media/ea-pricing/open-pricing01.png)

## <a name="estimate-costs-with-the-azure-pricing-calculator"></a>Azure ücretlendirme hesaplayıcısı ile maliyetleri tahmin etme

Ayrıca, Azure ücretlendirme hesaplayıcısı ile maliyetleri tahmin etmek için kuruluşunuzun fiyatlandırmasını da kullanabilirsiniz.

1. [Azure ücretlendirme hesaplayıcısı](https://azure.microsoft.com/pricing/calculator)’na gidin.
1. Sağ üst kısımda **Oturum aç**’ı seçin.
1. **Programlar ve Teklif** > **Lisanslama Programı** bölümünde **Kurumsal Anlaşma (EA)** öğesini seçin.
1. **Programlar ve Teklif** > **Seçilen sözleşme** bölümünde **Hiçbiri seçilmedi** öğesini seçin.

    ![Fiyat listesi csv dosyasını indir düğmesini gösteren ekran görüntüsü](./media/ea-pricing/ea-pricing-calculator-estimate.png)

1. Kuruluşu seçin.
1. **Uygula**’yı seçin.
1. Ürünleri arayıp tahmininize ekleyin.
1. Tahmini fiyatlar, seçtiğiniz kuruluşa ait fiyatlandırmaya göre gösterilir.

## <a name="check-your-billing-account-type"></a>Ödeme hesabınızın türünü denetleme
[!INCLUDE [billing-check-account-type](../../../includes/billing-check-account-type.md)]

## <a name="next-steps"></a>Sonraki adımlar

Kurumsal Anlaşma müşterisiyseniz bkz:

- [Azure için Kurumsal Anlaşma faturalama bilgilerinize erişimi yönetme](manage-billing-access.md)
- [Microsoft Azure faturanızı görüntüleme ve indirme](../understand/download-azure-invoice.md)
- [Microsoft Azure kullanımınızı ve ücretlerinizi görüntüleme ve indirme](../understand/download-azure-daily-usage.md)
- [Kurumsal Anlaşma müşterileri için faturanızı anlama](../understand/review-enterprise-agreement-bill.md)

Microsoft Müşteri Sözleşmeniz varsa bkz:

- [Microsoft Müşteri Sözleşmesi için fiyat listenizdeki koşulları anlama](mca-understand-pricesheet.md)
- [Microsoft Azure faturanızı görüntüleme ve indirme](../understand/download-azure-invoice.md)
- [Microsoft Azure kullanımınızı ve ücretlerinizi görüntüleme ve indirme](../understand/download-azure-daily-usage.md)
- [Faturalama profiliniz için vergi belgelerini görüntüleme ve indirme](../understand/mca-download-tax-document.md)
- [Faturalama profilinizin faturasındaki ücretleri anlama](../understand/review-customer-agreement-bill.md)
