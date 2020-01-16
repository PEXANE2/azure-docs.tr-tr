---
title: Microsoft Azure faturanızı görüntüleme ve indirme
description: Microsoft Azure faturanızı görüntüleme ve indirme işlemleri açıklanır.
keywords: fatura, fatura indirme, azure faturası, azure kullanımı
author: bandersmsft
manager: jureid
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: banders
ms.openlocfilehash: 0f413d38565202d379c81570b5cb169c2ed8effe
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75987830"
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

## <a name="noinvoice"></a> Fatura alamamanızın nedeni nedir?

Bir faturayı görmemenizin birden fazla nedeni olabilir:

- Azure'a abone olmanızın üzerinden 30 gün geçmemiştir.

- Azure sizi fatura döneminizin sonunda faturalar. Bu nedenle fatura henüz oluşturulmamış olabilir. Fatura döneminin sonuna kadar bekleyin.

- Faturaları görüntüleme izniniz yoktur. MCA veya MPA’nız varsa faturalama profilinin Sahibi, Katkıda Bulunanı, Okuyucusu veya Fatura yöneticisi olmanız gerekir. Diğer abonelikler için Hesap Yöneticisi değilseniz eski faturaları göremeyebilirsiniz. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](../manage/manage-billing-access.md).

- Aboneliğinizde Ücretsiz Deneme veya aylık kredi miktarı varsa, yalnızca aylık kredi miktarını aştığınızda faturalandırılırsınız. Microsoft Müşteri Sözleşmeniz veya Microsoft İş Ortağı Sözleşmeniz varsa her zaman fatura alırsınız.

## <a name="download-invoices-in-the-azure-portal"></a>Azure portalında fatura indirme

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
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

Microsoft Müşteri Sözleşmesi için bir faturalandırma hesabınız varsa, faturanızı bir e-posta ile almak için kabul edebilirsiniz. Bir faturalandırma profilinde sahibi, katkıda bulunan, okuyucu veya Fatura Yöneticisi rolüne sahip tüm kullanıcılar, faturasını e-posta ile alır. 

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

1. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Portalda abonelik aramasını gösteren ekran görüntüsü](./media/download-azure-invoice/search-cmb.png)

1. Sol taraftaki **faturalandırma profilleri** ' ni seçin. Faturalandırma profilleri listesinden, e-postadaki faturaları almak için bir faturalandırma profili seçin.

   [Faturalama profili listesini gösteren ekran görüntüsü ![](./media/download-azure-invoice/mca-select-profile.png)](./media/download-azure-invoice/mca-select-profile-zoomed-in.png#lightbox)

1. Sol taraftaki **Özellikler** ' i seçin ve ardından **e-posta faturası tercihini Güncelleştir**' i seçin.

   [Faturalama profili listesini gösteren ekran görüntüsü ![](./media/download-azure-invoice/mca-select-update-email-preferences.png)](./media/download-azure-invoice/mca-select-update-email-preferences.png#lightbox)

1. **Kabul** et ' i seçin ve ardından **Güncelleştir**' e tıklayın.

   [Faturalama profili listesini gösteren ekran görüntüsü ![](./media/download-azure-invoice/mca-select-email-opt-in.png)](./media/download-azure-invoice/mca-select-email-opt-in.png#lightbox)

## <a name="opt-out-of-getting-your-microsoft-customer-agreement-invoices-in-email"></a>Microsoft Müşteri Sözleşmesi faturalarınızı e-postayla almayı geri çevirme

Faturanızı e-postayla almayı devre dışı bırakmak için yukarıdaki adımları izleyin ve **devre dışı**bırak ' a tıklayın. Sahip, katkıda bulunan, okuyucu veya Fatura Yöneticisi rolüne sahip tüm kullanıcılar, faturayı e-posta ile almayı kabul etmedi. 

## <a name="give-others-access-to-your-microsoft-customer-agreement-invoices"></a>Başkalarına Microsoft müşteri anlaşması faturanız için erişim verme

Bir faturalandırma profili için onlara Fatura Yöneticisi rolü atayarak, diğer kullanıcılara faturaları görüntüleme, indirme ve ödeme için erişim izni verebilirsiniz. Faturanızı e-postada almayı tercih ettiyseniz, bu kullanıcılar da e-postadaki faturaları alırlar. 

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

1. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Portalda abonelik aramasını gösteren ekran görüntüsü](./media/download-azure-invoice/search-cmb.png)

1. Sol taraftaki **faturalandırma profilleri** ' ni seçin. Faturalandırma profilleri listesinden, Fatura Yöneticisi rolünü atamak istediğiniz bir faturalandırma profili seçin.

   [Faturalama profili listesini gösteren ekran görüntüsü ![](./media/download-azure-invoice/mca-select-profile.png)](./media/download-azure-invoice/mca-select-profile-zoomed-in.png#lightbox)

1. Sol taraftaki **Access Control (IAM)** seçeneğini belirleyin ve ardından sayfanın en üstünden **Ekle** ' yi seçin.

   [Access Control sayfasını gösteren ![ekran görüntüsü](./media/download-azure-invoice/mca-select-access-control.png)](./media/download-azure-invoice/mca-select-access-control-zoomed-in.png#lightbox)

1. Rol açılan listesinde, **Fatura Yöneticisi**' ni seçin. Erişim izni vermek istediğiniz kullanıcının e-posta adresini girin. Rolü atamak için **Kaydet**’i seçin.

   [bir kullanıcı tarafından Fatura Yöneticisi olarak eklenmesinin gösterildiği ![ekran görüntüsü](./media/download-azure-invoice/mca-added-invoice-manager.png)](./media/download-azure-invoice/mca-added-invoice-manager.png#lightbox)

## <a name="check-your-billing-account-type"></a>Ödeme hesabınızın türünü denetleme
[!INCLUDE [billing-check-account-type](../../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

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
