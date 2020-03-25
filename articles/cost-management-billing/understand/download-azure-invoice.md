---
title: Microsoft Azure faturanızı görüntüleme ve indirme
description: Microsoft Azure faturanızı görüntüleme ve indirme işlemleri açıklanır.
keywords: fatura, fatura indirme, azure faturası, azure kullanımı
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: banders
ms.openlocfilehash: 691d27acebf238e84265870e8c01976bfc2412b2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77200274"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Microsoft Azure faturanızı görüntüleme ve indirme

Aboneliklerin çoğunda faturanızı [Azure portalından ](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)indirebilir veya e-postayla gönderilmesini sağlayabilirsiniz. Kurumsal Anlaşmalı bir Azure müşterisiyseniz (EA müşterisi), kuruluşunuzun faturalarını indiremezsiniz. Faturalar, kayıt için faturaları alacak kişi olarak ayarlanan kişiye gönderilir.

Yalnızca belirli rollerin fatura görüntüleme izni vardır. Örneğin, Hesap Yöneticisi veya Kuruluş Yöneticisi. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](../manage/manage-billing-access.md).

Microsoft Müşteri Sözleşmeniz (MCA) varsa faturalarınızı almak için aşağıdaki rollerden birine sahip olmanız gerekir:

- Faturalama profili sahibi
- Katılımcı
- Okuyucu
- Fatura yöneticisi

Microsoft İş ortağı Sözleşmeniz (MPA) varsa, Azure faturalarını görüntüleyip indirmek için iş ortağı kuruluşunda Genel Yönetici ve Yönetici Aracısı rolüne sahip olmanız gerekir. Gerekli izinleri öğrenmek için [Ödeme hesabınızın türünü denetleyin](#check-your-billing-account-type).

<!-- For more information about billing roles for Microsoft Customer Agreements, see [Billing profile roles and tasks](../manage/understand-mca-roles.md#billing-profile-roles-and-tasks). -->

## <a name="why-you-might-not-get-an-invoice"></a><a name="noinvoice"></a> Fatura alamamanızın nedeni nedir?

Bir faturayı görmemenizin birden fazla nedeni olabilir:

- Azure'a abone olmanızın üzerinden 30 gün geçmemiştir.

- Azure sizi fatura döneminizin sonunda faturalar. Bu nedenle fatura henüz oluşturulmamış olabilir. Fatura döneminin sonuna kadar bekleyin.

- Faturaları görüntüleme izniniz yoktur. MCA veya MPA’nız varsa faturalama profilinin Sahibi, Katkıda Bulunanı, Okuyucusu veya Fatura yöneticisi olmanız gerekir. Diğer abonelikler için Hesap Yöneticisi değilseniz eski faturaları göremeyebilirsiniz. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](../manage/manage-billing-access.md).

- Aboneliğinizde Ücretsiz Deneme veya aylık kredi miktarı varsa, yalnızca aylık kredi miktarını aştığınızda faturalandırılırsınız. Microsoft Müşteri Sözleşmeniz veya Microsoft İş Ortağı Sözleşmeniz varsa her zaman fatura alırsınız.

## <a name="download-invoices-in-the-azure-portal"></a>Azure portalında fatura indirme

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. *Maliyet Yönetimi + Faturalama* araması yapın.
1. Erişiminize bağlı olarak, bir ödeme hesabı veya faturalama profili seçmeniz gerekebilir.
1. Soldaki menüde **Faturalama** altındaki **Faturalar**’ı seçin.
1. Fatura kılavuzunda, indirmek istediğiniz fatura satırını bulun.
1. İndirme simgesine veya satırın sonundaki üç noktaya (`...`) tıklayın.
1. İndirme menüsünde **Fatura**’yı seçin.

Faturanız hakkında daha fazla bilgi için bkz. [Microsoft Azure faturanızı anlama](review-individual-bill.md). Maliyetlerinizi yönetme konusunda yardım için bkz. [Azure fatura ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](../manage/getting-started.md).

## <a name="get-your-subscriptions-invoices-in-email"></a>Faturalama profili faturalarınızı e-postayla alma

Azure faturanızı e-postayla almaları için ek alıcıları kabul edebilir ve yapılandırabilirsiniz. Bu özellik destek teklifleri, Kurumsal Anlaşmalar veya Open ile Azure gibi belirli aboneliklerde kullanılamayabilir.

1. [Abonelikler sayfasında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) aboneliğinizi seçin. Kabul etme işlemini sahip olduğunuz her abonelik için ayrıca gerçekleştirin. **Faturalar**'a ve **Faturamı e-posta ile gönder**’e tıklayın.

    ![Kabul etme akışını gösteren ekran görüntüsü](./media/download-azure-invoice/invoicesdeeplink01.png)

2. **Kabul et**’e tıklayın ve koşulları kabul edin.

    ![Kabul etme akışının 2. adımını gösteren ekran görüntüsü](./media/download-azure-invoice/invoicearticlestep02.png)

3. Sözleşmeyi kabul ettiğinizde ek alıcıları yapılandırabilirsiniz. Bir alıcı kaldırıldığında artık e-posta adresi depolanmaz. Fikrinizi değiştirirseniz adresi yeniden eklemeniz gerekir.

    ![Kabul etme akışının 3. adımını gösteren ekran görüntüsü](./media/download-azure-invoice/invoicearticlestep03.png)

Adımları izledikten sonra e-posta gönderilmediyse [profilinizin iletişim tercihleri sayfasındaki](https://account.windowsazure.com/profile) e-posta adresinin doğru olduğundan emin olun.

## <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Aboneliğinizin faturalarını e-postayla almayı geri çevirme

Faturanızı e-postayla almayı geri çevirmek için önceki adımları izleyin ve **Faturaların e-posta ile gönderilmesini geri çevir**’e tıklayın. Bu seçenek, faturaların e-posta ile gönderilmesi için girilmiş olan e-posta adreslerini kaldırır. Yeniden e-posta almalarını isterseniz alıcıları yeniden yapılandırabilirsiniz.

 ![Geri çevirme akışını gösteren ekran görüntüsü](./media/download-azure-invoice/invoicearticlestep04.png)

<!-- Does following section apply to MPA too? -->
## <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Microsoft Müşteri Sözleşmesi faturalarınızı e-postayla alma

Microsoft Müşteri Sözleşmesi için ödeme hesabınız varsa faturanızı e-postayla almayı kabul edebilirsiniz. Fatura, faturalama profilinde sahip, katkıda bulunan, okuyucu veya fatura yöneticisi rolüne sahip olan tüm kullanıcılara e-posta ile gönderilir.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Portalda abonelik aramasını gösteren ekran görüntüsü](./media/download-azure-invoice/search-cmb.png)

1. Soldaki **Faturalama profilleri** seçeneğini belirleyin. Faturalama profilleri listesinden faturaların e-posta olarak gönderilmesini istediğiniz faturalama profilini seçin.

   [![Faturalama profili listesini gösteren ekran görüntüsü](./media/download-azure-invoice/mca-select-profile.png)](./media/download-azure-invoice/mca-select-profile-zoomed-in.png#lightbox)

1. Sol taraftan **Özellikler**'i ve ardından **E-posta fatura tercihini güncelleştir**'i seçin.

   [![Faturalama profili listesini gösteren ekran görüntüsü](./media/download-azure-invoice/mca-select-update-email-preferences.png)](./media/download-azure-invoice/mca-select-update-email-preferences.png#lightbox)

1. **Katıl**'ı seçin ve ardından **Güncelleştir**'e tıklayın.

   [![Faturalama profili listesini gösteren ekran görüntüsü](./media/download-azure-invoice/mca-select-email-opt-in.png)](./media/download-azure-invoice/mca-select-email-opt-in.png#lightbox)

## <a name="opt-out-of-getting-your-microsoft-customer-agreement-invoices-in-email"></a>Microsoft Müşteri Sözleşmesi faturalarınızı e-postayla almayı geri çevirme

Faturanızı e-postayla almayı geri çevirmek için önceki adımları izleyin ve **Geri çevir**’e tıklayın. Sahip, katkıda bulunan, okuyucu veya fatura yöneticisi rolüne sahip olan tüm kullanıcılara faturanın e-posta ile gönderilmesi özelliği devre dışı bırakılır.

## <a name="give-others-access-to-your-microsoft-customer-agreement-invoices"></a>Microsoft Müşteri Sözleşmesi faturalarınız için başkalarına erişim verme

Bir faturalama profilinde fatura yöneticisi rolünü atayarak başkalarına faturaları görüntüleme, indirme ve ödeme erişimi verebilirsiniz. Faturanın e-posta ile gönderilmesini kabul ettiyseniz faturalar bu kullanıcılara da e-posta ile gönderilir.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Portalda abonelik aramasını gösteren ekran görüntüsü](./media/download-azure-invoice/search-cmb.png)

1. Soldaki **Faturalama profilleri** seçeneğini belirleyin. Faturalama profilleri listesinden fatura yöneticisi rolünü atamak istediğiniz faturalama profilini seçin.

   [![Faturalama profili listesini gösteren ekran görüntüsü](./media/download-azure-invoice/mca-select-profile.png)](./media/download-azure-invoice/mca-select-profile-zoomed-in.png#lightbox)

1. Sol taraftan **Erişim Denetimi (IAM)** girişini seçtikten sonra sayfanın üst tarafından **Ekle**'yi seçin.

   [![Erişim denetimi sayfasını gösteren ekran görüntüsü](./media/download-azure-invoice/mca-select-access-control.png)](./media/download-azure-invoice/mca-select-access-control-zoomed-in.png#lightbox)

1. Rol açılan listesinde **Fatura Yöneticisi**'ni seçin. Erişim izni vermek istediğiniz kullanıcının e-posta adresini girin. Rolü atamak için **Kaydet**’i seçin.

   [![Bir kullanıcıyı fatura yöneticisi olarak eklemeyi gösteren ekran görüntüsü](./media/download-azure-invoice/mca-added-invoice-manager.png)](./media/download-azure-invoice/mca-added-invoice-manager.png#lightbox)

## <a name="check-your-billing-account-type"></a>Ödeme hesabınızın türünü denetleme
[!INCLUDE [billing-check-account-type](../../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Faturanız ve ücretleriniz hakkında daha fazla bilgi edinmek için bkz:

- [Microsoft Azure kullanımınızı ve ücretlerinizi görüntüleme ve indirme](download-azure-daily-usage.md)
- [Microsoft Azure faturanızı anlama](review-individual-bill.md)
- [Azure faturanızdaki terimleri anlama](understand-invoice.md)
- [Ayrıntılı Microsoft Azure kullanım bilgilerinizdeki terimleri anlama](understand-usage.md)
- [Kuruluşunuzun Azure fiyatlandırmasını görüntüleme](../manage/ea-pricing.md)

Microsoft Müşteri Sözleşmeniz varsa bkz:

- [Faturalama profilinizin faturasındaki ücretleri anlama](review-customer-agreement-bill.md)
- [Faturalama profilinizin faturasındaki terimleri anlama](mca-understand-your-invoice.md)
- [Faturalama profilinizin Azure kullanımı ve ücretleri dosyasını anlama](mca-understand-your-usage.md)
- [Faturalama profiliniz için vergi belgelerini görüntüleme ve indirme](mca-download-tax-document.md)
- [Kuruluşunuzun Azure fiyatlandırmasını görüntüleme](../manage/ea-pricing.md)
