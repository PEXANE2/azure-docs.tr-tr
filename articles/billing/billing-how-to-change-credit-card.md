---
title: Azure için kredi kartınızı değiştirme
description: Bir Azure aboneliği için ödeme yapmak üzere kullanılan kredi kartının nasıl değiştirileceğini açıklar.
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
ms.openlocfilehash: 7719ae83525883a6d3f014dbb99877b7319f2ccd
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68383671"
---
# <a name="add-update-or-remove-a-credit-card-for-azure"></a>Azure için kredi kartı ekleme, güncelleştirme veya kaldırma

Azure portal, yeni bir kredi kartı ekleyebilir, var olan bir kredi kartını güncelleştirebilir veya kullanmıyorsanız bir kredi kartını silebilirsiniz. Bu değişiklikleri yapmak için bir [Hesap Yöneticisi](billing-subscription-transfer.md#whoisaa) olmanız gerekir.

Bir [Microsoft Müşteri anlaşmanız](#check-access-to-a-microsoft-customer-agreement)varsa, ödeme yöntemleriniz faturalandırma profilleriyle ilişkilendirilir. [Bir faturalandırma profili için varsayılan ödeme yöntemini değiştirmeyi](#change-payment-method-for-a-billing-profile)öğrenin. Yalnızca Azure 'a kaydolan Kullanıcı ödeme yöntemini güncelleştirebilir.

**Faturaya (Çek/tel aktarımı) göre ödeme yapmak istiyor musunuz?** Bkz. [faturaya göre Azure abonelikleri Için ödeme](billing-how-to-pay-by-invoice.md).

<a id="addcard"></a>

## <a name="add-a-new-credit-card-to-an-azure-subscription"></a>Azure aboneliğine yeni bir kredi kartı ekleme

1. [Azure Portal](https://portal.azure.com) hesap yöneticisi olarak oturum açın.
1. **Maliyet yönetimi + faturalandırma**için arama yapın.

    ![Aramayı gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/search.png)

1. Kredi kartını eklemek istediğiniz aboneliği seçin.
1. **Ödeme yöntemleri**'ni seçin.

    ![Ödeme yöntemlerini Yönet seçeneğinin seçili olduğunu gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Sol üst köşede “+” işaretini seçerek kartı ekleyin. Sağ tarafta bir kredi kartı formu görüntülenir.
1. Kredi kartı ayrıntılarını girin.

    ![Yeni kart eklemeyi gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/sub-add-new-x.png)

1. Bu kartı etkin ödeme yönteminiz yapmak için formun üzerinde **Bu etkin ödeme yönteminizi yap** ' ın yanındaki kutuyu işaretleyin. Bu kart, seçilen abonelik olarak aynı kartı kullanan tüm aboneliklerde etkin ödeme aracınız olur.

1. **İleri**’yi seçin.

Kredi kartını ekledikten sonra bir hata alırsanız bkz. [Azure kayıt sırasında kredi kartı reddedildi](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="update-existing-credit-card"></a>Mevcut kredi kartını güncelleştirme

Kredi kartınız yenilenirse ve numara aynı kalırsa, mevcut kredi kartı ayrıntılarını sona erme tarihi gibi güncelleştirin. Kartın kaybolması, çalınması veya geçerliliği aşıldığı için kredi kartı numaranız değişirse, [ödeme yöntemi olarak kredi kartı ekleme](#addcard) bölümündeki adımları izleyin. CVV 'yi güncelleştirmeniz gerekmez.

1. [Azure Portal](https://portal.azure.com) hesap yöneticisi olarak oturum açın.
1. **Maliyet yönetimi + faturalandırma**için arama yapın.

    ![Aramayı gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/search.png)

1. **Ödeme yöntemleri**'ni seçin.

    ![Ödeme yöntemlerini Yönet seçeneğinin seçili olduğunu gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Düzenlemek istediğiniz kredi kartına tıklayın. Sağ tarafta bir kredi kartı formu görüntülenir.

    ![Kredi kartının seçili olduğunu gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/edit-card-x.png)

1. Kredi kartı ayrıntılarını güncelleştirin.
1. **Kaydet**’i seçin.

## <a name="use-a-different-credit-card"></a>Farklı bir kredi kartı kullanın

Aboneliklerinizden birden fazlası aynı etkin ödeme yöntemine sahip ise, Bu aboneliklerden herhangi birindeki etkin ödeme yönteminin değiştirilmesi, diğer kullanıcıların da etkin ödeme yöntemini de güncelleştirir.

1. [Azure Portal](https://portal.azure.com) hesap yöneticisi olarak oturum açın.
1. **Maliyet yönetimi + faturalandırma**için arama yapın.

    ![Aramayı gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/search.png)

1. Kredi kartını eklemek istediğiniz aboneliği seçin.
1. **Ödeme yöntemleri**'ni seçin.

    ![Ödeme yöntemlerini Yönet seçeneğinin seçili olduğunu gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Etkin ödeme yöntemini yapmak istediğiniz kartın yanındaki kutuyu seçin.
1. **Etkin ayarla**' ya tıklayın.
    ![Seçilen kredi kartını ve etkin ayarı gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/sub-change-active-x.png)

## <a name="remove-a-credit-card-from-the-account"></a>Bir kredi kartını hesaptan kaldırma

1. [Azure Portal](https://portal.azure.com) hesap yöneticisi olarak oturum açın.
1. Sayfanın sol tarafındaki **maliyet yönetimi + faturalandırma** ' i seçin.

    ![Aramayı gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/search.png)

1. **Faturalandırma**bölümünde **ödeme yöntemleri**' ni seçin.

    ![Ödeme yöntemlerini Yönet seçeneğinin seçili olduğunu gösteren ekran görüntüsü.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Kaldırmak istediğiniz kartın yanındaki kutuyu seçin.
1. Tıklayın **Sil**.

Kredi kartınız Microsoft aboneliklerinizden herhangi biri için etkin ödeme yöntemi ise, Azure hesabınızdan kaldıramazsınız. Bu kredi kartıyla bağlantılı tüm abonelikler için etkin ödeme yöntemini değiştirin ve yeniden deneyin
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

## <a name="change-payment-method-for-a-billing-profile"></a>Faturalandırma profili için ödeme yöntemini değiştir

Bir faturalandırma profili için ödeme yöntemini değiştirmek üzere Azure 'a kaydolan kişi olmanız gerekir.

Varsayılan ödeme yöntemini denetim/tel aktarımına geçirmek isterseniz, [bir faturalandırma profilini denetim/tel aktarım için](billing-how-to-pay-by-invoice.md)nasıl değiştireceğinizi öğrenin.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Arama **maliyet Yönetimi + faturalandırma**.
1. Soldaki menüde **faturalandırma profilleri**' ne tıklayın.

    ![menüdeki faturalama profilini gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/billing-profile.png)

1. Bir faturalandırma profili seçin.
1. Soldaki menüden **ödeme yöntemleri**' ni seçin.

   ![Menüdeki ödeme yöntemlerini gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/billing-profile-payment-methods.png)

1. Varsayılan ödeme yönteminin üzerinde **Değiştir**' e tıklayın.

    ![Değişiklik düğmesini gösteren ekran görüntüsü](./media/billing-how-to-change-credit-card/customer-led-switch-credit-card.png)

1. Varolan bir kartı seçin veya yeni bir kart ekleyin.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
Aşağıdaki bölümlerde, kredi kartı bilgilerinizi değiştirme hakkında sıkça sorulan sorular yanıtlanacaktır.

### <a name="my-subscription-is-disabled-why-cant-i-remove-my-credit-card-now"></a>Aboneliğim devre dışı bırakıldı. Şimdi kredi kartımı neden kaldıramıyorum?

Aboneliğiniz devre dışı bırakıldıktan veya iptal edildikten sonra, aboneliğinizi kalıcı olarak silmeden önce 90 gün bekleyeceğiz. Aboneliği yeniden etkinleştirmek istemeniz durumunda, bekletme süresi boyunca ödeme yönteminizi dosyada tutuyoruz. Bundan sonra, abonelik kalıcı olarak silinir.

90 günlük saklama süresi sona ermeden önce kredi kartınızı kaldırmanız gerekiyorsa, [aboneliğinizi yeniden etkinleştirin](billing-subscription-become-disable.md). Yeniden etkinleştiremeyeceksiniz [Azure desteği 'ne başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>"Oturum açma oturumunuzun süresi doldu. Yeniden oturum açmak için lütfen buraya tıklayın.

Zaten oturumunuzu açmış olsanız ve geri döndüğünüzde bu hata iletisini almaya devam ederseniz, özel bir gözatma oturumu ile yeniden deneyin.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>Nasıl yaparım? her abonelik için farklı bir kart kullanmak mı istiyorsunuz?

Ne yazık ki abonelikleriniz aynı kartı kullanıyorsa, bunları farklı kartlar kullanacak şekilde ayırmak mümkün değildir. Ancak, yeni bir abonelik için kaydolduğunuzda, bu abonelik için yeni bir ödeme yöntemi kullanmayı tercih edebilirsiniz.

### <a name="how-do-i-make-payments"></a>Nasıl yaparım? ödemeler yapılsın mı?

Ödeme yönteminiz olarak bir kredi kartı ayarlarsanız, her faturalandırma döneminden sonra kartınızı otomatik olarak ücretlendiririz. Herhangi bir şey yapmanız gerekmez.

[Fatura ile](billing-how-to-pay-by-invoice.md)ödeme yapıyorsanız faturanızı faturanızda listelenen konuma gönderin.

### <a name="how-do-i-change-the-tax-id"></a>Nasıl yaparım? vergi KIMLIĞI değiştirilsin mi?

Vergi KIMLIĞINI eklemek veya güncelleştirmek için [Azure Hesap Merkezi](https://account.azure.com/Profile)profilinizi güncelleştirin ve ardından **Vergi kaydı**' nı seçin. Vergi numarası, vergi muafiyeti hesaplamaları için kullanılır ve faturanızda görünür.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesine erişimi denetleme
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- [Azure ayırmaları](billing-save-compute-costs-reservations.md) hakkında bilgi edinmek için bkz. Finans, ister paradan tasarruf edebilir.
