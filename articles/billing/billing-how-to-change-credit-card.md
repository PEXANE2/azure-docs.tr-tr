---
title: Azure için kredi kartınızı değiştirme
description: Azure aboneliğinin ödemesini yapmak için kullanılan kredi kartının nasıl değiştirileceğini açıklar.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: banders
ms.openlocfilehash: 05b1fe3d061e735322e6eb6c65e60f63d8adb4d6
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70933859"
---
# <a name="add-update-or-remove-a-credit-card-for-azure"></a>Azure için kredi kartı ekleme, güncelleştirme veya kaldırma

Bu belge Azure için çevrimiçi ortamda kredi kartıyla kaydolan müşterilere yöneliktir.

Azure portalında yeni kredi kartı ekleme, var olan kredi kartını güncelleştirme veya kullanılmayan kredi kartlarını silme işlemlerini gerçekleştirebilirsiniz. Bu değişiklikleri yapmak için [Hesap Yöneticisi](billing-subscription-transfer.md#whoisaa) olmanız gerekir.

[Microsoft Müşteri Sözleşmeniz](#check-access-to-a-microsoft-customer-agreement) varsa, ödeme yöntemleriniz faturalama profilleriyle ilişkilendirilir. [Faturalama profili için varsayılan ödeme yöntemini değiştirme](#change-payment-method-for-a-billing-profile) hakkında bilgi edinin. Yalnızca Azure’a kaydolan kullanıcı ödeme yöntemini güncelleştirebilir.

**Fatura ile ödemeye (çek/havale) geçiş yapmak mı istiyorsunuz?** Bkz. [Azure abonelikleri için fatura ile ödeme](billing-how-to-pay-by-invoice.md).

<a id="addcard"></a>

## <a name="add-a-new-credit-card-to-an-azure-subscription"></a>Azure aboneliğine yeni bir kredi kartı ekleme

1. [Azure portalında](https://portal.azure.com) Hesap Yöneticisi olarak oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Aramayı gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/search.png)

1. Kredi kartını eklemek istediğiniz aboneliği seçin.
1. **Ödeme yöntemleri**'ni seçin.

    ![Ödeme yöntemlerini yönet seçeneğinin belirlendiğini gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Sol üst köşede “+” işaretini seçerek kartı ekleyin. Sağ tarafta bir kredi kartı formu görüntülenir.
1. Kredi kartı ayrıntılarını girin.

    ![Yeni kart eklenmesini gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/sub-add-new-x.png)

1. Bu kartın etkin ödeme yönteminiz olmasını sağlamak için, formun üst kısmında **Bunu etkin ödeme yöntemim yap** seçeneğinin yanındaki kutuyu işaretleyin. Bu kart, seçilen abonelik olarak aynı kartı kullanan tüm aboneliklerde etkin ödeme aracınız olur.

1. **İleri**’yi seçin.

Kredi kartını ekledikten sonra hatayla karşılaşırsanız bkz. [Azure’a kaydolurken kredi kartı reddedildi](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="update-existing-credit-card"></a>Mevcut kredi kartını güncelleştirme

Kredi kartınız yenilenir ve numara aynı kalırsa, son kullanma tarihi gibi mevcut kredi kartı ayrıntılarını güncelleştirin. Kartınız kaybolduğu, çalındığı veya kartınızın süresi dolduğu için kredi kartı numaranız değiştiyse [Ödeme yöntemi olarak kredi kartı ekleme](#addcard) bölümündeki adımları izleyin. CVV’yi güncelleştirmeniz gerekmez.

1. [Azure portalında](https://portal.azure.com) Hesap Yöneticisi olarak oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Aramayı gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/search.png)

1. **Ödeme yöntemleri**'ni seçin.

    ![Ödeme yöntemlerini yönet seçeneğinin belirlendiğini gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Düzenlemek istediğiniz kredi kartına tıklayın. Sağ tarafta bir kredi kartı formu görüntülenir.

    ![Kredi kartının seçili olduğunu gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/edit-card-x.png)

1. Kredi kartı ayrıntılarını güncelleştirin.
1. **Kaydet**’i seçin.

## <a name="use-a-different-credit-card"></a>Farklı bir kredi kartı kullanma

Birden fazla aboneliğiniz aynı etkin ödeme yöntemine sahipse, bu aboneliklerden birinde etkin ödeme yöntemi değiştirildiğinde diğer aboneliklerdeki etkin ödeme yöntemi de güncelleştirilir.

1. [Azure portalında](https://portal.azure.com) Hesap Yöneticisi olarak oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Aramayı gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/search.png)

1. Kredi kartını eklemek istediğiniz aboneliği seçin.
1. **Ödeme yöntemleri**'ni seçin.

    ![Ödeme yöntemlerini yönet seçeneğinin belirlendiğini gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Etkin ödeme yöntemi olmasını istediğiniz kartın yanındaki kutuyu seçin.
1. **Etkin olarak ayarla**’ya tıklayın.
    ![Kredi kartının seçili olduğunu ve etkin olarak ayarlandığını gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/sub-change-active-x.png)

## <a name="remove-a-credit-card-from-the-account"></a>Hesaptan kredi kartını kaldırma

1. [Azure portalında](https://portal.azure.com) Hesap Yöneticisi olarak oturum açın.
1. Sayfanın sol tarafındaki **Maliyet Yönetimi + Faturalandırma**’yı seçin.

    ![Aramayı gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/search.png)

1. **Faturalama** bölümünde **Ödeme yöntemleri**’ni seçin.

    ![Ödeme yöntemlerini yönet seçeneğinin belirlendiğini gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Kaldırmak istediğiniz kartın yanındaki kutuyu seçin.
1. **Sil**'e tıklayın.

Kredi kartınız herhangi bir Microsoft aboneliğiniz için etkin ödeme yöntemiyse onu Azure hesabınızdan kaldıramazsınız. Bu kredi kartına bağlı tüm abonelikler için etkin ödeme yöntemini değiştirip yeniden deneyin.
<!-- # Add, update, or remove a credit card for Azure

In the Account Center, you can add a new credit card, update an existing credit card, or delete a credit card that you don't use. You must be an [Account Administrator](billing-subscription-transfer.md#whoisaa) to make these changes.

**Want to switch to pay by invoice?** See [Pay for Azure subscriptions by invoice](billing-how-to-pay-by-invoice.md).

<a id="addcard"></a>

## Add a new credit card

1. Sign in to the [Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select a subscription.
1. On the right side of the page, select **Manage payment methods**.

    ![Screenshot that shows Manage payment methods option selected.](./media/billing-how-to-change-credit-card/changesub_new.png)
1. Select “+” to add a card.

    ![Screenshot that shows the edit option next to the payment method.](./media/billing-how-to-change-credit-card/editcard_new.png)
1. Enter credit card details.
1. Select **Save**.

If you get an error after you add the credit card, see [Credit card declined at Azure sign-up](billing-credit-card-fails-during-azure-sign-up.md).

## Update existing credit card

If your credit card gets renewed and the number remains the same, update the existing credit card details like the expiration date. If your credit card number changes because the card is lost, stolen, or expired, follow the steps in the [Add a credit card as a payment method](#addcard) section. You don't need to update the CVV.

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
1. Select **Manage payment methods**.
1. Select **Edit** next to the card you want to update.
1. Update the credit card details.
1. Select **Save**.

## Use a different credit card for the Azure subscription

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
1. On the right side of the page, select **Manage payment methods**.
1. Click **Use Instead** next to the card that you want to use. This also updates any other subscriptions currently associated with this card.

## Remove a credit card from the account

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
3. On the right side of the page, select **Manage payment methods**.
4. Click **Delete** for the credit card that you want to delete.

If your credit card is associated with other active Microsoft subscriptions, you can't remove it from your Azure account. Remove the credit card from all active subscriptions that you have with Microsoft and try again. -->

## <a name="change-payment-method-for-a-billing-profile"></a>Faturalama profili için ödeme yöntemini değiştirme

Bir faturalama profilinin ödeme yöntemini değiştirmek için, Azure’a kaydolan kişi siz olmalısınız.

Varsayılan ödeme yöntemini çek/havale olarak değiştirmek istiyorsanız, [faturalama profilini çek/havale olarak değiştirme](billing-how-to-pay-by-invoice.md) hakkında bilgi edinin.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.
1. Soldaki menüde **Faturalama profili**’ne tıklayın.

    ![menüde faturalama profilini gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/billing-profile.png)

1. Faturalama profilini seçin.
1. Soldaki menüden **Ödeme yöntemleri**’ni seçin.

   ![Menüde ödeme yöntemlerini gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/billing-profile-payment-methods.png)

1. Varsayılan ödeme yönteminin yukarısında **Değiştir**’e tıklayın.

    ![Değiştir düğmesini gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/customer-led-switch-credit-card.png)

1. Mevcut bir kartı seçin veya yeni bir kart ekleyin.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
Aşağıdaki bölümlerde, kredi kartı bilgilerinizi değiştirmeye ilişkin yaygın olarak sorulan soruların yanıtları verilmektedir.

### <a name="my-subscription-is-disabled-why-cant-i-remove-my-credit-card-now"></a>Aboneliğim devre dışı bırakıldı. Neden şimdi kredi kartımı kaldıramıyorum?

Aboneliğiniz devre dışı bırakıldıktan veya iptal edildikten sonra aboneliğinizi kalıcı olarak silmeden önce 90 gün bekleriz. Aboneliğinizi yeniden etkinleştirmeyi istemeniz durumunu göz önünde bulundurarak, saklama süresi boyunca ödeme yönteminizi kayıtlı tutarız. Bu süreden sonra aboneliğiniz kalıcı olarak silinir.

90 günlük saklama süresi sona ermeden önce kredi kartınızı kaldırmanız gerekirse [aboneliğinizi yeniden etkinleştirin](billing-subscription-become-disable.md). Yeniden etkinleştiremiyorsanız [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>Neden "Oturumunuzun süresi doldu. Lütfen tekrar oturum açmak için buraya tıklayın" iletisini almaya devam ediyorum?

Oturumu kapatıp yeniden açtığınız halde bu hata iletisini almaya devam ediyorsanız özel bir gözatma oturumuyla yeniden deneyin.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>Sahip olduğum her abonelik için nasıl farklı bir kart kullanırım?

Ne yazık ki abonelikleriniz zaten aynı kartı kullanıyorsa, bu abonelikleri farklı kartları kullanacak şekilde ayırmak mümkün değildir. Ancak yeni bir aboneliğe kaydolduğunuzda, bu yeni abonelik için yeni bir ödeme yöntemi kullanmayı seçebilirsiniz.

### <a name="how-do-i-make-payments"></a>Nasıl ödeme yaparım?

Ödeme yönteminiz olarak bir kredi kartını ayarlarsanız, her faturalama döneminden sonra otomatik olarak kartınızı ücretlendiririz. Herhangi bir işlem yapmanız gerekmez.

[Fatura ile ödeme yapıyorsanız](billing-how-to-pay-by-invoice.md), ödemenizi faturanızın en altında bulunan konuma gönderin.

### <a name="how-do-i-change-the-tax-id"></a>Vergi numarasını nasıl değiştirebilirim?

Vergi numarası eklemek veya güncelleştirmek için [Azure Hesap Merkezi](https://account.azure.com/Profile)’nde profilinizi güncelleştirin ve **Vergi numarası**’nı seçin. Vergi numarası, vergi muafiyeti hesaplamaları için kullanılır ve faturanızda görünür.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi’ne erişimi denetleme
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- Para tasarruf etmenizi sağlayıp sağlayamadığını görmek için [Azure rezervasyonları](billing-save-compute-costs-reservations.md) hakkında bilgi edinin.
