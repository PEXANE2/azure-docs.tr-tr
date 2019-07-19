---
title: Microsoft Azure faturanızı görüntüleme ve indirme
description: Microsoft Azure faturanızı görüntülemeyi ve indirmeyi açıklar.
keywords: Faturalama faturası, fatura indirme, Azure faturası, Azure kullanımı
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
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68321778"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Microsoft Azure faturanızı görüntüleme ve indirme

Çoğu abonelik için faturanızı [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) indirebilir veya e-posta ile gönderilmesini sağlayabilirsiniz. Kurumsal Anlaşma (EA müşterisi) olan bir Azure müşterisiyseniz, kuruluşunuzun faturalarını indiremez. Faturalar kayda gönderilir ve kayıt için faturaları alacak şekilde ayarlanır.

Yalnızca belirli rollerin faturaları görüntüleme izni vardır. Örneğin, hesap yöneticisi veya kuruluş yöneticisi. Faturalandırma bilgilerine erişim elde etme hakkında daha fazla bilgi için bkz. [rolleri kullanarak Azure faturalandırmaya erişimi yönetme](billing-manage-access.md).

Bir [Microsoft Müşteri anlaşmanız](#check-your-access-to-a-microsoft-customer-agreement)varsa, faturalarınızı almak için aşağıdaki rollerden birine sahip olmanız gerekir:

- Faturalama profili sahibi
- Katılımcı
- Okuyucu
- Fatura Yöneticisi

Microsoft Müşteri sözleşmeleri için faturalandırma rolleri hakkında daha fazla bilgi için bkz. [Faturalandırma profili rolleri ve görevleri](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

## <a name="noinvoice"></a>Neden fatura alabilirim?

Bir faturayı görmemenizin birden fazla nedeni olabilir:

- Azure'a abone olmanızın üzerinden 30 gün geçmemiştir.

- Azure, fatura döneminizin sonunda sizi faturalar. Bu nedenle, bir fatura henüz oluşturulmamış olabilir. Fatura döneminin sonuna kadar bekleyin.

- Faturaları görüntüleme izniniz yok. Bir Microsoft Müşteri anlaşmanız varsa, faturalama profili sahibi, katkıda bulunan, okuyucu veya Fatura Yöneticisi olmanız gerekir. Hesap yöneticisi değilseniz, diğer abonelikler için eski faturaları görmeyebilirsiniz. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](billing-manage-access.md).

- Aboneliğiniz ile ücretsiz bir deneme veya aylık kredi miktarınız varsa, yalnızca aylık kredi tutarını aşarsanız bir fatura alırsınız. Bir Microsoft Müşteri anlaşmanız varsa, her zaman bir fatura alırsınız.

## <a name="download-your-azure-invoices-pdf"></a>Azure faturalarınızı indirin (. PDF)

Çoğu abonelik için Azure portal faturanızı indirebilirsiniz. Bir Microsoft Müşteri anlaşmanız varsa bkz. [Microsoft Müşteri Sözleşmesi için fatura indirme](#download-invoices-for-a-microsoft-customer-agreement).

### <a name="download-invoices-for-an-individual-subscription"></a>Tek bir abonelik için faturaları indirin

1. [Faturalara erişimi olan bir Kullanıcı](billing-manage-access.md)olarak Azure Portal [abonelikler sayfasında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) aboneliğinizi seçin.

2. **Faturalar**'ı seçin.

    ![Faturalandırma & kullanım seçeneğini gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png)

3. PDF faturanızda bir kopyasını görüntülemek için **faturayı indir** ' e tıklayın. **Mevcut değilse**, bkz [. son fatura dönemi için neden bir fatura görmüyorum?](#noinvoice)

    ![Her fatura dönemi için faturalandırma dönemlerini, indirme seçeneğini ve toplam ücretleri gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Günlük kullanımınızı faturalandırma dönemi ' ne tıklayarak da görüntüleyebilirsiniz.

Faturanız hakkında daha fazla bilgi için bkz. [Microsoft Azure Faturanızı Anlama](billing-understand-your-bill.md). Maliyetlerinizi yönetme konusunda yardım için bkz. [Azure Faturalandırma ve maliyet yönetimiyle ilgili beklenmedik maliyetleri engelleme](billing-getting-started.md).

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Microsoft Müşteri anlaşmasıyla ilgili faturaları indirin

Faturalar, Microsoft Müşteri anlaşmasındaki her [Faturalandırma profili](billing-mca-overview.md#billing-profiles) için oluşturulur. Azure portal faturaları indirmek için bir faturalandırma profili sahibi, katkıda bulunan, okuyucu veya Fatura Yöneticisi olmanız gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. *Maliyet yönetimi + faturalandırma*için arama yapın.
1. Bir faturalandırma profili seçin. Erişimlerinize bağlı olarak, önce bir faturalandırma hesabı seçmeniz gerekebilir.
1. **Faturalar**'ı seçin.
1. Fatura kılavuzunda, indirmek istediğiniz faturanın satırını bulun.
1. Satırın sonundaki üç nokta`...`() simgesine tıklayın.
    ![Satırın sonundaki üç noktayı gösteren ekran görüntüsü](./media/billing-download-azure-invoice/billingprofile-invoicegrid.png)
1. İndirme bağlam menüsünde **Fatura**' yı seçin.

    ![Bağlam menüsünü gösteren ekran görüntüsü](./media/billing-download-azure-invoice/contextmenu.png)

Son fatura dönemi için bir fatura görmüyorsanız, [son fatura dönemi Için neden bir fatura görmüyorum?](#noinvoice) bölümüne bakın.

## <a name="get-your-invoice-in-email-pdf"></a>Faturanızı e-posta ile (. PDF) alın

Azure faturanızı bir e-postada alacak ek alıcıları kabul edebilir ve yapılandırabilirsiniz. Bu özellik destek teklifleri, kurumsal anlaşmalar veya Open ile Azure gibi belirli abonelikler için kullanılamayabilir. Bir Microsoft Müşteri anlaşmanız varsa, bir sonraki bölüme bakın, [Faturalandırma profili faturanızı e-posta Ile alın](#get-your-subscriptions-invoices-in-email).

### <a name="get-your-subscriptions-invoices-in-email"></a>E-postadaki aboneliğinizin faturalarını alın

1. [Abonelikler sayfasında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)aboneliğinizi seçin. Sahip olduğunuz her abonelik için kabul edin. **Faturalar** ' a tıklayın ve ardından **faturamı gönderin**.

    ![Kabul etme akışını gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. Kabul **et ' e** tıklayın ve koşulları kabul edin.

    ![Katılım akışı adımını gösteren ekran görüntüsü 2](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)

3. Sözleşmeyi kabul ettiğinizde, ek alıcılar yapılandırabilirsiniz. Bir alıcı kaldırıldığında, e-posta adresi artık depolanmaz. Fikrinizi değiştirirseniz, bunları yeniden eklemeniz gerekir.

    ![Katılım akışı adımını gösteren ekran görüntüsü 3](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

Adımları tamamladıktan sonra bir e-posta alamazsanız, [profilinizde iletişim tercihlerinde](https://account.windowsazure.com/profile)e-posta adresinizin doğru olduğundan emin olun.

### <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>E-postadaki aboneliğinizin faturalarını alma işlemini geri çevirme

Faturanızı e-posta ile almayı devre dışı bırakmak için önceki adımları izleyin ve e- **postayla gönderilen faturaların oturumunu kapat**' a tıklayın. Bu seçenek, faturaların e-posta ile gönderilmesi için girilmiş olan e-posta adreslerini kaldırır. Yeniden kabul ederseniz alıcıları yeniden yapılandırabilirsiniz.

 ![Geri çevirme akışını gösteren ekran görüntüsü](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Microsoft müşteri anlaşması faturalarınızı e-posta ile alın

Bir Microsoft Müşteri anlaşmanız varsa, faturanızı bir e-posta ile almak için kabul edebilirsiniz. Tüm faturalama profili sahipleri, katkıda bulunanlar, okuyucular ve fatura yöneticileri, faturayı e-posta ile alır. Okuyucular e-posta faturası tercihini güncelleştiremez.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet yönetimi + faturalandırma**için arama yapın.
1. Bir faturalandırma profili seçin. Erişimlerinize bağlı olarak, önce bir faturalandırma hesabı seçmeniz gerekebilir.
1. **Ayarlar**altında **Özellikler**' i seçin.
1. **E-posta faturası**bölümünde **e-posta faturası tercihini Güncelleştir**' i seçin

    ![E-posta faturası özelliklerini gösteren ekran görüntüsü](./media/billing-download-azure-invoice/billingprofile-email.png)

1. **Kabul et '** i seçin.
1. Tıklayın **güncelleştirme**.

### <a name="opt-out-of-getting-your-microsoft-customer-agreement-invoices-in-email"></a>E-postada Microsoft müşteri anlaşması faturalarınızı alma işlemini geri çevirme

Faturanızı e-postayla almayı devre dışı bırakmak için yukarıdaki adımları izleyin ve **devre dışı**bırak ' a tıklayın. Tüm sahipler, katkıda bulunanlar, okuyucular ve fatura yöneticileri, faturayı e-posta ile almayı kabul etmedi. Bir okuyucunuzu kullanıyorsanız, e-posta faturası tercihini değiştiremezsiniz.



## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesine erişiminizi denetleme
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Faturanızı ve ücretlerinizi hakkında daha fazla bilgi edinmek için bkz.:

- [Microsoft Azure kullanımınızı ve ücretlerinizi görüntüleyin ve indirin](billing-download-azure-daily-usage.md)
- [Microsoft Azure faturanızı anlama](billing-understand-your-bill.md)
- [Azure faturanızda koşulları anlayın](billing-understand-your-invoice.md)
- [Microsoft Azure ayrıntılı kullanımınızın koşullarını anlayın](billing-understand-your-usage.md)
- [Kuruluşunuzun Azure fiyatlandırmasını görüntüleme](billing-ea-pricing.md)

Bir Microsoft Müşteri sözleşmeniz varsa, bkz:

- [Faturalandırma profiliniz için faturadaki ücretleri anlayın](billing-mca-understand-your-bill.md)
- [Faturalandırma profiliniz için faturadaki koşulları anlayın](billing-mca-understand-your-invoice.md)
- [Faturalandırma profiliniz için Azure kullanımı ve ücretler dosyasını anlayın](billing-mca-understand-your-usage.md)
- [Faturalandırma profiliniz için vergi belgelerini görüntüleyin ve indirin](billing-mca-download-tax-document.md)
- [Kuruluşunuzun Azure fiyatlandırmasını görüntüleme](billing-ea-pricing.md)
