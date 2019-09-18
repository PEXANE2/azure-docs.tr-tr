---
title: Microsoft Azure faturanızı görüntüleme ve indirme
description: Microsoft Azure faturanızı görüntüleme ve indirme işlemleri açıklanır.
keywords: fatura, fatura indirme, azure faturası, azure kullanımı
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: banders
ms.openlocfilehash: 4dc5f88f7a5994e5bcd50d71c86bf1ba35b10734
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "68321778"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Microsoft Azure faturanızı görüntüleme ve indirme

Aboneliklerin çoğunda faturanızı [Azure portalından ](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)indirebilir veya e-postayla gönderilmesini sağlayabilirsiniz. Kurumsal Anlaşmalı bir Azure müşterisiyseniz (EA müşterisi), kuruluşunuzun faturalarını indiremezsiniz. Faturalar, kayıt için faturaları alacak kişi olarak ayarlanan kişiye gönderilir.

Yalnızca belirli rollerin fatura görüntüleme izni vardır. Örneğin, Hesap Yöneticisi veya Kuruluş Yöneticisi. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](billing-manage-access.md).

[Microsoft Müşteri Sözleşmeniz](#check-your-access-to-a-microsoft-customer-agreement) varsa faturalarınızı almak için aşağıdaki rollerden birine sahip olmanız gerekir:

- Faturalama profili sahibi
- Katılımcı
- Okuyucu
- Fatura yöneticisi

Microsoft Müşteri Sözleşmelerine ilişkin faturalama rolleri hakkında daha fazla bilgi için bkz. [Faturalama profili rolleri ve görevleri](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

## <a name="noinvoice"></a> Fatura alamamanızın nedeni nedir?

Bir faturayı görmemenizin birden fazla nedeni olabilir:

- Azure'a abone olmanızın üzerinden 30 gün geçmemiştir.

- Azure sizi fatura döneminizin sonunda faturalar. Bu nedenle fatura henüz oluşturulmamış olabilir. Fatura döneminin sonuna kadar bekleyin.

- Faturaları görüntüleme izniniz yoktur. Microsoft Müşteri Sözleşmeniz varsa faturalama profili Sahibi, Katkıda Bulunanı, Okuyucusu veya Fatura yöneticisi olmanız gerekir. Diğer abonelikler için Hesap Yöneticisi değilseniz eski faturaları göremeyebilirsiniz. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](billing-manage-access.md).

- Aboneliğinizde Ücretsiz Deneme veya aylık kredi miktarı varsa, yalnızca aylık kredi miktarını aştığınızda faturalandırılırsınız. Microsoft Müşteri Sözleşmeniz varsa her zaman fatura alırsınız.

## <a name="download-your-azure-invoices-pdf"></a>Azure faturalarınızı indirme (.pdf)

Aboneliklerin çoğunda faturanızı Azure portalından indirebilirsiniz. Microsoft Müşteri Sözleşmeniz varsa bkz. [Microsoft Müşteri Sözleşmesi için faturaları indirme](#download-invoices-for-a-microsoft-customer-agreement).

### <a name="download-invoices-for-an-individual-subscription"></a>Bireysel abonelik için faturaları indirme

1. [Faturalara erişim izni olan bir kullanıcı olarak](billing-manage-access.md) Azure portalındaki[Abonelikler sayfasında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) aboneliğinizi seçin.

2. **Faturalar**'ı seçin.

    ![Faturalama ve kullanım seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png)

3. PDF faturanızın bir kopyasını görüntülemek için **Faturayı İndir**’e tıklayın. **Kullanılamıyor** iletisiyle karşılaşıyorsanız bkz. [Son fatura dönemine ait faturamı neden göremiyorum?](#noinvoice)

    ![Faturalama dönemini, indirme seçeneğini ve her faturalama dönemi için toplam ücretleri gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Günlük kullanımınızı faturalama dönemine tıklayarak da görüntüleyebilirsiniz.

Faturanız hakkında daha fazla bilgi için bkz. [Microsoft Azure faturanızı anlama](billing-understand-your-bill.md). Maliyetlerinizi yönetme konusunda yardım için bkz. [Azure fatura ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing-getting-started.md).

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi için faturaları indirme

Faturalar, Microsoft Müşteri Sözleşmesi’ndeki her [faturalama profili](billing-mca-overview.md#billing-profiles) için oluşturulur. Faturaları Azure portalından indirmek için faturalama profili Sahibi, Katkıda Bulunanı, Okuyucusu veya Fatura yöneticisi olmanız gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. *Maliyet Yönetimi + Faturalama* araması yapın.
1. Faturalama profilini seçin. Erişiminize bağlı olarak önce bir ödeme hesabı seçmeniz gerekebilir.
1. **Faturalar**'ı seçin.
1. Fatura kılavuzunda, indirmek istediğiniz fatura satırını bulun.
1. Satırın sonundaki üç noktaya (`...`) tıklayın.
    ![Satırın sonundaki üç noktayı gösteren ekran görüntüsü](./media/billing-download-azure-invoice/billingprofile-invoicegrid.png)
1. İndirme bağlam menüsünden **Fatura**’yı seçin.

    ![Bağlam menüsünü gösteren ekran görüntüsü](./media/billing-download-azure-invoice/contextmenu.png)

Son faturalama döneminin faturasını görmüyorsanız bkz. [Son fatura dönemine ait faturamı neden göremiyorum?](#noinvoice)

## <a name="get-your-invoice-in-email-pdf"></a>Faturanızı e-postayla alma (.pdf)

Azure faturanızı e-postayla almaları için ek alıcıları kabul edebilir ve yapılandırabilirsiniz. Bu özellik destek teklifleri, Kurumsal Anlaşmalar veya Open ile Azure gibi belirli aboneliklerde kullanılamayabilir. Microsoft Müşteri Sözleşmeniz varsa sonraki bölüme ([Faturalama profili faturalarınızı e-postayla alma](#get-your-subscriptions-invoices-in-email)) bakın.

### <a name="get-your-subscriptions-invoices-in-email"></a>Faturalama profili faturalarınızı e-postayla alma

1. [Abonelikler sayfasında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) aboneliğinizi seçin. Kabul etme işlemini sahip olduğunuz her abonelik için ayrıca gerçekleştirin. **Faturalar**'a ve **Faturamı e-posta ile gönder**’e tıklayın.

    ![Kabul etme akışını gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. **Kabul et**’e tıklayın ve koşulları kabul edin.

    ![Kabul etme akışının 2. adımını gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)

3. Sözleşmeyi kabul ettiğinizde ek alıcıları yapılandırabilirsiniz. Bir alıcı kaldırıldığında artık e-posta adresi depolanmaz. Fikrinizi değiştirirseniz adresi yeniden eklemeniz gerekir.

    ![Kabul etme akışının 3. adımını gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

Adımları izledikten sonra e-posta gönderilmediyse [profilinizin iletişim tercihleri sayfasındaki](https://account.windowsazure.com/profile) e-posta adresinin doğru olduğundan emin olun.

### <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Aboneliğinizin faturalarını e-postayla almayı geri çevirme

Faturanızı e-postayla almayı geri çevirmek için önceki adımları izleyin ve **Faturaların e-posta ile gönderilmesini geri çevir**’e tıklayın. Bu seçenek, faturaların e-posta ile gönderilmesi için girilmiş olan e-posta adreslerini kaldırır. Yeniden e-posta almalarını isterseniz alıcıları yeniden yapılandırabilirsiniz.

 ![Geri çevirme akışını gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Microsoft Müşteri Sözleşmesi faturalarınızı e-postayla alma

Microsoft Müşteri Sözleşmeniz varsa faturanızı e-postayla almayı kabul edebilirsiniz. Tüm faturalama profili Sahipleri, Katkıda Bulunanları, Okuyucuları ve Fatura yöneticileri faturalarını e-postayla alır. Okuyucular e-postayla fatura alma tercihini güncelleştiremez.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.
1. Faturalama profilini seçin. Erişiminize bağlı olarak önce bir ödeme hesabı seçmeniz gerekebilir.
1. **Ayarlar** bölümünde **Özellikler**'i seçin.
1. **Faturayı e-posta ile gönder**’in altında **E-posta fatura tercihini güncelleştir**’i seçin.

    ![Faturayı e-posta ile gönderme özelliklerini gösteren ekran görüntüsü](./media/billing-download-azure-invoice/billingprofile-email.png)

1. **Kabul et**’i seçin.
1. **Güncelleştir**’e tıklayın.

### <a name="opt-out-of-getting-your-microsoft-customer-agreement-invoices-in-email"></a>Microsoft Müşteri Sözleşmesi faturalarınızı e-postayla almayı geri çevirme

Faturanızı e-postayla almayı geri çevirmek için önceki adımları izleyin ve **Geri çevir**’e tıklayın. Tüm Sahipler, Katkıda bulunanlar, Okuyucular ve Fatura yöneticileri de faturalarını e-postayla almayı geri çevirmiş olur. Okuyucuysanız e-postayla fatura alma tercihini değiştiremezsiniz.



## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi’ne erişiminizi denetleme
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Faturanız ve ücretleriniz hakkında daha fazla bilgi edinmek için bkz:

- [Microsoft Azure kullanımınızı ve ücretlerinizi görüntüleme ve indirme](billing-download-azure-daily-usage.md)
- [Microsoft Azure faturanızı anlama](billing-understand-your-bill.md)
- [Azure faturanızdaki terimleri anlama](billing-understand-your-invoice.md)
- [Ayrıntılı Microsoft Azure kullanım bilgilerinizdeki terimleri anlama](billing-understand-your-usage.md)
- [Kuruluşunuzun Azure fiyatlandırmasını görüntüleme](billing-ea-pricing.md)

Microsoft Müşteri Sözleşmeniz varsa bkz:

- [Faturalama profilinizin faturasındaki ücretleri anlama](billing-mca-understand-your-bill.md)
- [Faturalama profilinizin faturasındaki terimleri anlama](billing-mca-understand-your-invoice.md)
- [Faturalama profilinizin Azure kullanımı ve ücretleri dosyasını anlama](billing-mca-understand-your-usage.md)
- [Faturalama profiliniz için vergi belgelerini görüntüleme ve indirme](billing-mca-download-tax-document.md)
- [Kuruluşunuzun Azure fiyatlandırmasını görüntüleme](billing-ea-pricing.md)
