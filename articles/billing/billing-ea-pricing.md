---
title: Kuruluşunuzun Azure fiyatlandırmasını görüntüleme ve indirme
description: Kuruluşunuzun fiyatlandırmasıyla maliyetlerin nasıl tahmin edileceğini ve fiyatlandırmanın nasıl görüntülenip indirileceğini öğrenin.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: 486a6b46b6c4590b7f49cd8aba449204cd8f4fac
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71709696"
---
# <a name="view-and-download-your-organizations-azure-pricing"></a>Kuruluşunuzun Azure fiyatlandırmasını görüntüleme ve indirme

Azure Kurumsal Anlaşması (EA) veya [Microsoft Müşteri Sözleşmesi](#check-your-access-to-a-microsoft-customer-agreement) olan Azure müşterileri, Azure portalında fiyatlandırmasını görüntüleyip indirebilir.

## <a name="ea-pricing"></a>Kurumsal Anlaşma fiyatlandırması

Kuruluş Yöneticisi tarafından kuruluşunuz için belirlenen ilkelere bağlı olarak yalnızca belirli yönetici rolleri, kuruluşunuzun Kurumsal Anlaşma fiyatlandırma bilgilerine erişim sağlar. Daha fazla bilgi için bkz. [Azure’daki Azure Kurumsal Anlaşma yönetici rollerini anlama](billing-understand-ea-roles.md).

1. Kuruluş Yöneticisi olarak [Azure portalında](https://portal.azure.com/) oturum açın.
1. *Maliyet Yönetimi + Faturalama* araması yapın.

   ![Azure portalı aramasını gösteren ekran görüntüsü](./media/billing-ea-pricing/portal-cm-billing-search.png)

1. Faturalama hesabı bölümünde **Kullanım + ücretler**’i seçin.

   ![Faturalama bölümünde kullanımı ve ücretleri gösteren ekran görüntüsü](./media/billing-ea-pricing/ea-pricing-usage-charges-nav.png)

1. ![Azure portalı aramasını gösteren ekran görüntüsü](./media/billing-ea-pricing/download-icon.png) Ay için **İndir**’i seçin.

1. **Fiyat Listesi** bölümünde **csv dosyasını indir**’i seçin.

   ![Fiyat listesi csv dosyasını indir düğmesini gösteren ekran görüntüsü](./media/billing-ea-pricing/download-ea-price-sheet.png)

## <a name="microsoft-customer-agreement-pricing"></a>Microsoft Müşteri Sözleşmesi fiyatlandırması

Fiyatlandırmayı görüntülemek ve indirmek için faturalama profilinin sahibi, katkıda bulunanı, okuyucusu veya fatura yöneticisi olmanız gerekir. Microsoft Müşteri Sözleşmelerine ilişkin faturalama rolleri hakkında daha fazla bilgi edinmek için bkz. [Faturalama profili rolleri ve görevleri](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="download-price-sheets-for-the-current-billing-period"></a>Geçerli faturalama dönemi için fiyat listelerini indirme

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. *Maliyet Yönetimi + Faturalama* araması yapın.
1. Faturalama profilini seçin. Erişiminize bağlı olarak önce bir ödeme hesabı seçmeniz gerekebilir.
1. **Genel Bakış** alanında, ay başından bugüne kadarki ücretlerin altında indirme bağlantılarını bulun.
1. **Azure fiyat listesi**’ni seçin.
![Genel Bakış’tan indirme seçeneğini gösteren ekran görüntüsü](./media/billing-ea-pricing/open-pricing.png)

### <a name="download-price-sheets-for-billed-charges"></a>Faturalanan ücretler için fiyat listelerini indirme

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. *Maliyet Yönetimi + Faturalama* araması yapın.
1. Faturalama profilini seçin. Erişiminize bağlı olarak önce bir ödeme hesabı seçmeniz gerekebilir.
1. **Faturalar**'ı seçin.
1. Fatura kılavuzunda, indirmek istediğiniz fiyat listesine karşılık gelen fatura satırını bulun.
1. Satırın sonundaki üç noktaya (`...`) tıklayın.
![Üç noktanın seçili olduğunu gösteren ekran görüntüsü](./media/billing-ea-pricing/billingprofile-invoicegrid.png)

1. Seçilen faturadaki hizmetlere ait fiyatları görmek istiyorsanız **Fatura fiyat listesi** seçeneğini belirleyin.
1. Verilen faturalama dönemine ait tüm Azure hizmetlerinin fiyatlarını görmek istiyorsanız **Azure fiyat listesi**’ni seçin.

![Fiyat listelerini içeren açılır menüyü gösteren ekran görüntüsü](./media/billing-ea-pricing/contextmenu-pricesheet.png)

## <a name="estimate-costs-with-the-azure-pricing-calculator"></a>Azure ücretlendirme hesaplayıcısı ile maliyetleri tahmin etme

Ayrıca, Azure ücretlendirme hesaplayıcısı ile maliyetleri tahmin etmek için kuruluşunuzun fiyatlandırmasını da kullanabilirsiniz.

1. [Azure ücretlendirme hesaplayıcısı](https://azure.microsoft.com/pricing/calculator)’na gidin.
1. Sağ üst kısımda **Oturum aç**’ı seçin.
1. **Programlar ve Teklif** > **Lisanslama Programı** bölümünde **Kurumsal Anlaşma (EA)** öğesini seçin.
1. **Programlar ve Teklif** > **Seçilen sözleşme** bölümünde **Hiçbiri seçilmedi** öğesini seçin.

    ![Fiyat listesi csv dosyasını indir düğmesini gösteren ekran görüntüsü](./media/billing-ea-pricing/ea-pricing-calculator-estimate.png)

1. Kuruluşu seçin.
1. **Uygula**’yı seçin.
1. Ürünleri arayıp tahmininize ekleyin.
1. Tahmini fiyatlar, seçtiğiniz kuruluşa ait fiyatlandırmaya göre gösterilir.

## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi’ne erişiminizi denetleme
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="next-steps"></a>Sonraki adımlar

Kurumsal Anlaşma müşterisiyseniz bkz:

- [Azure için Kurumsal Anlaşma faturalama bilgilerinize erişimi yönetme](billing-manage-access.md)
- [Microsoft Azure faturanızı görüntüleme ve indirme](billing-download-azure-invoice.md)
- [Microsoft Azure kullanımınızı ve ücretlerinizi görüntüleme ve indirme](billing-download-azure-daily-usage.md)
- [Kurumsal Anlaşma müşterileri için faturanızı anlama](billing-understand-your-bill-ea.md)

Microsoft Müşteri Sözleşmeniz varsa bkz:

- [Microsoft Müşteri Sözleşmesi için fiyat listenizdeki koşulları anlama](billing-mca-understand-pricesheet.md)
- [Microsoft Azure faturanızı görüntüleme ve indirme](billing-download-azure-invoice.md)
- [Microsoft Azure kullanımınızı ve ücretlerinizi görüntüleme ve indirme](billing-download-azure-daily-usage.md)
- [Faturalama profiliniz için vergi belgelerini görüntüleme ve indirme](billing-mca-download-tax-document.md)
- [Faturalama profilinizin faturasındaki ücretleri anlama](billing-mca-understand-your-bill.md)
