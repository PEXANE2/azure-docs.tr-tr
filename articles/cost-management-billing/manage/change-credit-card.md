---
title: Azure için kredi kartınızı değiştirme
description: Azure aboneliğinin ödemesini yapmak için kullanılan kredi kartının nasıl değiştirileceğini açıklar.
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: banders
ms.openlocfilehash: 0cf0911193cd379da60edb3064d3c6f195cf0190
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77199866"
---
# <a name="add-update-or-remove-a-credit-card-for-azure"></a>Azure için kredi kartı ekleme, güncelleştirme veya kaldırma

Bu belge Azure için çevrimiçi ortamda kredi kartıyla kaydolan müşterilere yöneliktir.

Azure portalında varsayılan ödeme yönteminizi yeni bir kredi kartı olarak değiştirebilir, kredi kartı bilgilerinizi güncelleştirebilir ve kullanmadığınız kredi kartlarını silebilirsiniz. Bu değişiklikleri yapmak için [Hesap Yöneticisi](billing-subscription-transfer.md#whoisaa) olmanız gerekir.

Microsoft Azure kredi kartı ve çek/havale ile ödeme yapabilirsiniz. Çek/havale ile ödeme onayı alabilmek için bkz. [Azure abonelikleri için fatura ile ödeme](pay-by-invoice.md).

Microsoft Müşteri Sözleşmeniz varsa, ödeme yöntemleriniz faturalama profilleriyle ilişkilendirilir. [Microsoft Müşteri Sözleşmesi’ne erişimi denetlemeyi](#check-the-type-of-your-account) öğrenin. MCA'nız varsa [Microsoft Müşteri Sözleşmesi için kredi kartlarını yönetme](#manage-credit-cards-for-a-microsoft-customer-agreement) bölümüne geçin.

<a id="addcard"></a>

## <a name="manage-credit-cards-for-an-azure-subscription"></a>Azure aboneliğindeki kredi kartlarını yönetme

Aşağıdaki bölümler Microsoft Online Services Program ödeme hesabına sahip olan müşteriler için geçerlidir. [Ödeme hesabınızın türünü denetlemeyi](#check-the-type-of-your-account) öğrenin. Ödeme hesabınızın türü Microsoft Online Services Program ise ödeme yöntemleri bireysel Azure abonelikleriyle ilişkilendirilir.

### <a name="change-credit-card-for-a-subscription"></a>Abonelikte kullanılan kredi kartını değiştirme

Azure aboneliğinizin varsayılan kredi kartını yeni bir kredi kartıyla veya Azure portalına kayıtlı kredi kartlarından biriyle değiştirebilirsiniz. Kredi kartını değiştirmek için Hesap Yöneticisi olmanız gerekir. Birden fazla aboneliğiniz aynı etkin ödeme yöntemine sahipse, bu aboneliklerden birinde etkin ödeme yöntemi değiştirildiğinde diğer aboneliklerdeki etkin ödeme yöntemi de güncelleştirilir.


Aboneliğinizin varsayılan kredi kartını değiştirmek için aşağıdaki adımları izleyebilirsiniz:

1. [Azure portalında](https://portal.azure.com) Hesap Yöneticisi olarak oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Aramayı gösteren ekran görüntüsü](./media/change-credit-card/search.png)

1. Kredi kartını eklemek istediğiniz aboneliği seçin.
1. **Ödeme yöntemleri**'ni seçin.

    ![Ödeme yöntemlerini yönet seçeneğinin belirlendiğini gösteren ekran görüntüsü.](./media/change-credit-card/payment-methods-blade-x.png)

1. Sol üst köşede “+” işaretini seçerek kartı ekleyin. Sağ tarafta bir kredi kartı formu görüntülenir.
1. Kredi kartı ayrıntılarını girin.

    ![Yeni kart eklenmesini gösteren ekran görüntüsü.](./media/change-credit-card/sub-add-new-x.png)

1. Bu kartın etkin ödeme yönteminiz olmasını sağlamak için, formun üst kısmında **Bunu etkin ödeme yöntemim yap** seçeneğinin yanındaki kutuyu işaretleyin. Bu kart, seçilen abonelik olarak aynı kartı kullanan tüm aboneliklerde etkin ödeme aracınız olur.

1. **İleri**’yi seçin.

Kredi kartını ekledikten sonra hatayla karşılaşırsanız bkz. [Azure’a kaydolurken kredi kartı reddedildi](../../billing/billing-credit-card-fails-during-azure-sign-up.md).

İsterseniz aşağıdaki adımları izleyerek aboneliğinizin varsayılan kredi kartını hesabınıza kayıtlı olan kredi kartlarından biriyle de değiştirebilirsiniz:

1. [Azure portalında](https://portal.azure.com) Hesap Yöneticisi olarak oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Aramayı gösteren ekran görüntüsü](./media/change-credit-card/search.png)

1. Kredi kartını eklemek istediğiniz aboneliği seçin.
1. **Ödeme yöntemleri**'ni seçin.

    ![Ödeme yöntemlerini yönet seçeneğinin belirlendiğini gösteren ekran görüntüsü.](./media/change-credit-card/payment-methods-blade-x.png)

1. Etkin ödeme yöntemi olmasını istediğiniz kartın yanındaki kutuyu seçin.
1. **Etkin olarak ayarla**’ya tıklayın.
    ![Kredi kartının seçili olduğunu ve etkin olarak ayarlandığını gösteren ekran görüntüsü.](./media/change-credit-card/sub-change-active-x.png)

### <a name="edit-credit-card-details"></a>Kredi kartı bilgilerini girme

Kredi kartınız yenilenir ve numara aynı kalırsa, son kullanma tarihi gibi mevcut kredi kartı ayrıntılarını güncelleştirin. Kartınız kaybolduğu, çalındığı veya kartınızın süresi dolduğu için kredi kartı numaranız değiştiyse [Ödeme yöntemi olarak kredi kartı ekleme](#addcard) bölümündeki adımları izleyin. CVV’yi güncelleştirmeniz gerekmez.

1. [Azure portalında](https://portal.azure.com) Hesap Yöneticisi olarak oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Aramayı gösteren ekran görüntüsü](./media/change-credit-card/search.png)

1. **Ödeme yöntemleri**'ni seçin.

    ![Ödeme yöntemlerini yönet seçeneğinin belirlendiğini gösteren ekran görüntüsü.](./media/change-credit-card/payment-methods-blade-x.png)

1. Düzenlemek istediğiniz kredi kartına tıklayın. Sağ tarafta bir kredi kartı formu görüntülenir.

    ![Kredi kartının seçili olduğunu gösteren ekran görüntüsü.](./media/change-credit-card/edit-card-x.png)

1. Kredi kartı ayrıntılarını güncelleştirin.
1. **Kaydet**’i seçin.

### <a name="delete-a-credit-card-from-the-account"></a>Hesaptaki kredi kartını silme

1. [Azure portalında](https://portal.azure.com) Hesap Yöneticisi olarak oturum açın.
1. Sayfanın sol tarafındaki **Maliyet Yönetimi + Faturalandırma**’yı seçin.

    ![Aramayı gösteren ekran görüntüsü](./media/change-credit-card/search.png)

1. **Faturalama** bölümünde **Ödeme yöntemleri**’ni seçin.

    ![Ödeme yöntemlerini yönet seçeneğinin belirlendiğini gösteren ekran görüntüsü.](./media/change-credit-card/payment-methods-blade-x.png)

1. Kaldırmak istediğiniz kartın yanındaki kutuyu seçin.
1. **Sil**'e tıklayın.

Kredi kartınız herhangi bir Microsoft aboneliğiniz için etkin ödeme yöntemiyse onu Azure hesabınızdan kaldıramazsınız. Bu kredi kartına bağlı tüm abonelikler için etkin ödeme yöntemini değiştirip yeniden deneyin.

## <a name="manage-credit-cards-for-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi için kredi kartlarını yönetme

Aşağıdaki bölümler, Microsoft Müşteri Sözleşmesi'ne sahip olan ve Azure'a çevrimiçi olarak kredi kartıyla kaydolmuş müşteriler için geçerlidir. [Microsoft Müşteri Sözleşmesi'ne sahip olup olmadığınızı nasıl denetleyebileceğinizi öğrenin](#check-the-type-of-your-account).

### <a name="change-default-credit-card"></a>Varsayılan kredi kartını değiştirme
Microsoft Müşteri Sözleşmeniz varsa, kredi kartınız bir faturalama profiliyle ilişkilendirilir. Bir faturalama profilinin ödeme yöntemini değiştirmek için, Azure’a kaydolan ve ödeme hesabını oluşturan kişi siz olmalısınız.

Faturalama profilinizin varsayılan ödeme yöntemini çek/havale olarak değiştirmek isterseniz bkz. [Azure abonelikleri için fatura ile ödeme](pay-by-invoice.md).

Kredi kartınızı değiştirmek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.
1. Soldaki menüde **Faturalama profili**’ne tıklayın.
1. Faturalama profilini seçin.
1. Soldaki menüden **Ödeme yöntemleri**’ni seçin.

   ![Menüde ödeme yöntemlerini gösteren ekran görüntüsü](./media/change-credit-card/payment-methods-tab-mca.png)

1. **Varsayılan ödeme yöntemi** bölümünde **Değiştir**’e tıklayın.

    ![Değiştir düğmesini gösteren ekran görüntüsü](./media/change-credit-card/change-payment-method-mca.png)

1. Sağdaki yeni dikey pencerede açılan menüden bir kart seçin veya mavi renkli "Yeni ödeme yöntemi ekleyin" bağlantısına tıklayarak yeni bir kart ekleyin.

### <a name="edit-or-delete-a-credit-card"></a>Kredi kartlarını düzenleme veya silme

Azure portalındaki hesap sayfanızdan kredi kartı bilgilerini düzenleyebilir (son kullanma tarihini güncelleştirme gibi) ve kredi kartlarını silebilirsiniz. Yalnızca herhangi bir Azure aboneliği veya faturalama profili ile ilişkilendirilmemiş kredi kartlarını silebilirsiniz. Devre dışı bırakılan bir Azure aboneliğiyle ilişkilendirilmiş olan kredi kartlarını silebilmek için aboneliğin silinmesini (iptal edildikten sonra 30-90 gün) beklemeniz gerekir.

Bir kredi kartını düzenlemek veya silmek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.
1. Soldaki menüde **Faturalama profili**’ne tıklayın.
1. Faturalama profilini seçin.
1. Soldaki menüden **Ödeme yöntemleri**’ni seçin.

   ![Menüde ödeme yöntemlerini gösteren ekran görüntüsü](./media/change-credit-card/payment-methods-tab-mca.png)

1. **Kredi kartlarınız** bölümünde düzenlemek veya silmek istediğiniz kartı bulun.
1. Satırın sonundaki üç noktayı (`...`) seçin.

    ![Üç noktayı gösteren ekran görüntüsü](./media/change-credit-card/edit-delete-credit-card-mca.png)

1. Kredi kartı bilgilerinizi düzenlemek için bağlam menüsünden **Düzenle**'yi seçin.
1. Kredi kartınızı silmek için bağlam menüsünden **Sil**'i seçin.

## <a name="troubleshooting"></a>Sorun giderme
Sanal veya ön ödemeli kartları desteklemiyoruz. Geçerli bir kredi kartı eklerken veya güncelleştirirken hata alıyorsanız tarayıcınızı gizli modda açmayı deneyin.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
Aşağıdaki bölümlerde, kredi kartı bilgilerinizi değiştirmeye ilişkin yaygın olarak sorulan soruların yanıtları verilmektedir.

### <a name="my-subscription-is-disabled-why-cant-i-remove-my-credit-card-now"></a>Aboneliğim devre dışı bırakıldı. Neden şimdi kredi kartımı kaldıramıyorum?

Aboneliğiniz devre dışı bırakıldıktan veya iptal edildikten sonra aboneliğinizi kalıcı olarak silmeden önce 90 gün bekleriz. Aboneliğinizi yeniden etkinleştirmeyi istemeniz durumunu göz önünde bulundurarak, saklama süresi boyunca ödeme yönteminizi kayıtlı tutarız. Bu süreden sonra aboneliğiniz kalıcı olarak silinir.

90 günlük saklama süresi sona ermeden önce kredi kartınızı kaldırmanız gerekirse [aboneliğinizi yeniden etkinleştirin](subscription-disabled.md). Yeniden etkinleştiremiyorsanız [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>Neden "Oturumunuzun süresi doldu. Lütfen tekrar oturum açmak için buraya tıklayın" iletisini almaya devam ediyorum?

Oturumu kapatıp yeniden açtığınız halde bu hata iletisini almaya devam ediyorsanız özel bir gözatma oturumuyla yeniden deneyin.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>Sahip olduğum her abonelik için nasıl farklı bir kart kullanırım?

Ne yazık ki abonelikleriniz zaten aynı kartı kullanıyorsa, bu abonelikleri farklı kartları kullanacak şekilde ayırmak mümkün değildir. Ancak yeni bir aboneliğe kaydolduğunuzda, bu yeni abonelik için yeni bir ödeme yöntemi kullanmayı seçebilirsiniz.

### <a name="how-do-i-make-payments"></a>Nasıl ödeme yaparım?

Ödeme yönteminiz olarak bir kredi kartını ayarlarsanız, her faturalama döneminden sonra otomatik olarak kartınızı ücretlendiririz. Herhangi bir işlem yapmanız gerekmez.

[Fatura ile ödeme yapıyorsanız](pay-by-invoice.md), ödemenizi faturanızın en altında bulunan konuma gönderin.

### <a name="how-do-i-change-the-tax-id"></a>Vergi numarasını nasıl değiştirebilirim?

Vergi numarası eklemek veya güncelleştirmek için [Azure Hesap Merkezi](https://account.azure.com/Profile)’nde profilinizi güncelleştirin ve **Vergi numarası**’nı seçin. Vergi numarası, vergi muafiyeti hesaplamaları için kullanılır ve faturanızda görünür.

## <a name="check-the-type-of-your-account"></a>Hesabınızın türünü denetleme
[!INCLUDE [billing-check-mca](../../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- Para tasarruf etmenizi sağlayıp sağlayamadığını görmek için [Azure rezervasyonları](../reservations/save-compute-costs-reservations.md) hakkında bilgi edinin.
