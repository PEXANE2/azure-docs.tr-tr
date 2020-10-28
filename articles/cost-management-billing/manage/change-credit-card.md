---
title: Azure için kredi kartınızı değiştirme
description: Azure aboneliğinin ödemesini yapmak için kullanılan kredi kartının nasıl değiştirileceğini açıklar.
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 09/15/2020
ms.author: banders
ms.openlocfilehash: f773c964d900c45a51eac433c3616a6e4b8978ae
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92131148"
---
# <a name="add-or-update-a-credit-card-for-azure"></a>Azure için kredi kartı ekleme veya güncelleştirme

Bu belge Azure için çevrimiçi ortamda kredi kartıyla kaydolan müşterilere yöneliktir.

Azure portalda varsayılan ödeme yönteminizi yeni bir kredi kartı olarak değiştirebilir ve kredi kartı bilgilerinizi güncelleştirebilirsiniz. Bu değişiklikleri yapmak için [Hesap Yöneticisi](../understand/subscription-transfer.md#whoisaa) olmanız gerekir.

Kredi kartını silmek istiyorsanız bkz. [Azure fatura ödeme yöntemini silme](delete-azure-payment-method.md).

Microsoft Azure kredi kartı ve çek/havale ile ödeme yapabilirsiniz. Çek/havale ile ödeme onayı alabilmek için bkz. [Azure abonelikleri için fatura ile ödeme](pay-by-invoice.md).

Microsoft Müşteri Sözleşmesi ile, ödeme yöntemleriniz faturalama profilleriyle ilişkilendirilir. [Microsoft Müşteri Sözleşmesi’ne erişimi denetlemeyi](#check-the-type-of-your-account) öğrenin. MCA'nız varsa [Microsoft Müşteri Sözleşmesi için kredi kartlarını yönetme](#manage-credit-cards-for-a-microsoft-customer-agreement) bölümüne geçin.

<a id="addcard"></a>

## <a name="manage-credit-cards-for-an-azure-subscription"></a>Azure aboneliğindeki kredi kartlarını yönetme

Aşağıdaki bölümler Microsoft Online Services Program ödeme hesabına sahip olan müşteriler için geçerlidir. [Ödeme hesabınızın türünü denetlemeyi](#check-the-type-of-your-account) öğrenin. Ödeme hesabınızın türü Microsoft Online Services Program ise ödeme yöntemleri bireysel Azure abonelikleriyle ilişkilendirilir. Kredi kartını ekledikten sonra hatayla karşılaşırsanız bkz. [Azure’a kaydolurken kredi kartı reddedildi](./troubleshoot-declined-card.md).

### <a name="change-credit-card-for-a-subscription-by-adding-a-new-credit-card"></a>Yeni kredi kartı ekleyerek bir abonelikte kullanılan kredi kartını değiştirme

Azure aboneliğinizin varsayılan kredi kartını yeni bir kredi kartıyla veya Azure portalına kayıtlı kredi kartlarından biriyle değiştirebilirsiniz. Kredi kartını değiştirmek için Hesap Yöneticisi olmanız gerekir. Birden fazla abonelik aynı etkin ödeme yöntemine sahipse aboneliklerden birinde etkin ödeme yöntemi değiştirildiğinde, diğer aboneliklerdeki etkin ödeme yöntemi de güncelleştirilir.

Aboneliğinizin varsayılan kredi kartını değiştirmek için aşağıdaki adımları izleyebilirsiniz:

1. [Azure portalında](https://portal.azure.com) Hesap Yöneticisi olarak oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.  
    ![Aramayı gösteren ekran görüntüsü](./media/change-credit-card/search.png)
1. Kredi kartını eklemek istediğiniz aboneliği seçin.
1. **Ödeme yöntemleri** 'ni seçin.  
    ![Ödeme yöntemlerini yönet seçeneğinin belirlendiğini gösteren ekran görüntüsü](./media/change-credit-card/payment-methods-blade-x.png)
1. Sol üst köşede “+” işaretini seçerek kartı ekleyin. Sağ tarafta bir kredi kartı formu görüntülenir.
1. Kredi kartı ayrıntılarını girin.  
    ![Yeni kart eklemeyi gösteren ekran görüntüsü](./media/change-credit-card/sub-add-new-x.png)
1. Bu kartın etkin ödeme yönteminiz olmasını sağlamak için, formun üst kısmında **Bunu etkin ödeme yöntemim yap** seçeneğinin yanındaki kutuyu işaretleyin. Bu kart, seçilen abonelik olarak aynı kartı kullanan tüm aboneliklerde etkin ödeme aracınız olur.
1. **İleri** ’yi seçin.

### <a name="change-credit-card-for-a-subscription-to-a-previously-saved-credit-card"></a>Abonelikte kullanılan kredi kartını önceden kaydedilen bir kredi kartıyla değiştirme

İsterseniz aşağıdaki adımları izleyerek aboneliğinizin varsayılan kredi kartını hesabınıza kayıtlı olan kredi kartlarından biriyle de değiştirebilirsiniz:

1. [Azure portalında](https://portal.azure.com) Hesap Yöneticisi olarak oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.  
    ![Aramayı gösteren ekran görüntüsü](./media/change-credit-card/search.png)
1. Kredi kartını eklemek istediğiniz aboneliği seçin.
1. **Ödeme yöntemleri** 'ni seçin.
    ![Ödeme yöntemlerini yönet seçeneğinin belirlendiğini gösteren ekran görüntüsü](./media/change-credit-card/payment-methods-blade-x.png)
1. Etkin ödeme yöntemi olmasını istediğiniz kartın yanındaki kutuyu seçin.
1. **Etkin olarak ayarla** ’yı seçin.
    ![Kredi kartının seçili olduğunu ve etkin olarak ayarlandığını gösteren ekran görüntüsü](./media/change-credit-card/sub-change-active-x.png)

### <a name="edit-credit-card-details"></a>Kredi kartı bilgilerini girme

Kredi kartınız yenilenir ve numara aynı kalırsa, son kullanma tarihi gibi mevcut kredi kartı ayrıntılarını güncelleştirin. Kartınız kaybolduğu, çalındığı veya kartınızın süresi dolduğu için kredi kartı numaranız değiştiyse [Ödeme yöntemi olarak kredi kartı ekleme](#addcard) bölümündeki adımları izleyin. CVV’yi güncelleştirmeniz gerekmez.

1. [Azure portalında](https://portal.azure.com) Hesap Yöneticisi olarak oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.
    ![Aramayı gösteren ekran görüntüsü](./media/change-credit-card/search.png)
1. **Ödeme yöntemleri** 'ni seçin.
    ![Ödeme yöntemlerini yönet seçeneğinin belirlendiğini gösteren ekran görüntüsü](./media/change-credit-card/payment-methods-blade-x.png)
1. Düzenlemek istediğiniz kredi kartını seçin. Sağ tarafta bir kredi kartı formu görüntülenir.
    ![Kredi kartının seçili olduğunu gösteren ekran görüntüsü](./media/change-credit-card/edit-card-x.png)
1. Kredi kartı ayrıntılarını güncelleştirin.
1. **Kaydet** ’i seçin.

## <a name="manage-credit-cards-for-a-microsoft-customer-agreement"></a>Microsoft Müşteri Sözleşmesi için kredi kartlarını yönetme

Aşağıdaki bölümler, Microsoft Müşteri Sözleşmesi'ne sahip olan ve Azure'a çevrimiçi olarak kredi kartıyla kaydolmuş müşteriler için geçerlidir. [Microsoft Müşteri Sözleşmesi'ne sahip olup olmadığınızı nasıl denetleyebileceğinizi öğrenin](#check-the-type-of-your-account).

### <a name="change-default-credit-card"></a>Varsayılan kredi kartını değiştirme

Microsoft Müşteri Sözleşmeniz varsa, kredi kartınız bir faturalama profiliyle ilişkilendirilir. Bir faturalama profilinin ödeme yöntemini değiştirmek için, Azure’a kaydolan ve ödeme hesabını oluşturan kişi siz olmalısınız.

Faturalama profilinizin varsayılan ödeme yöntemini çek/havale olarak değiştirmek isterseniz bkz. [Azure abonelikleri için fatura ile ödeme](pay-by-invoice.md).

Kredi kartınızı değiştirmek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.
1. Soldaki menüde **Faturalama profilleri** ’ni seçin.
1. Faturalama profilini seçin.
1. Soldaki menüden **Ödeme yöntemleri** ’ni seçin.  
   ![Menüde ödeme yöntemlerini gösteren ekran görüntüsü](./media/change-credit-card/payment-methods-tab-mca.png)
1. **Varsayılan ödeme yöntemi** bölümünde **Değiştir** ’i seçin.  
    :::image type="content" source="./media/change-credit-card/change-payment-method-mca.png" alt-text="Değiştirme seçeneğini gösteren ekran görüntüsü" :::
1. Sağdaki yeni alanda açılan menüden bir kart seçin veya mavi renkli **Yeni ödeme yöntemi ekleyin** bağlantısını seçerek yeni bir kart ekleyin.

### <a name="edit-a-credit-card"></a>Kredi kartını düzenleme

Azure portaldaki hesap sayfanızdan kredi kartı bilgilerini (son kullanma tarihini güncelleştirme gibi) düzenleyebilirsiniz. 

Kredi kartını düzenlemek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.
1. Soldaki menüde **Faturalama profilleri** ’ni seçin.
1. Faturalama profilini seçin.
1. Soldaki menüden **Ödeme yöntemleri** ’ni seçin.  
   ![Menüde ödeme yöntemlerini gösteren ekran görüntüsü](./media/change-credit-card/payment-methods-tab-mca.png)
1. **Kredi kartlarınız** bölümünde düzenlemek istediğiniz kartı bulun.
1. Satırın sonundaki üç noktayı (`...`) seçin.  
    :::image type="content" source="./media/change-credit-card/edit-delete-credit-card-mca.png" alt-text="Değiştirme seçeneğini gösteren ekran görüntüsü" :::
1. Kredi kartı bilgilerinizi düzenlemek için bağlam menüsünden **Düzenle** 'yi seçin.

## <a name="troubleshooting"></a>Sorun giderme

Azure, sanal veya ön ödemeli kartları desteklemez. Geçerli bir kredi kartı eklerken veya güncelleştirirken hata alıyorsanız tarayıcınızı gizli modda açmayı deneyin.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Aşağıdaki bölümlerde, kredi kartı bilgilerinizi değiştirmeye ilişkin yaygın olarak sorulan soruların yanıtları verilmektedir.

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>Neden "Oturumunuzun süresi doldu. Lütfen tekrar oturum açmak için buraya tıklayın" iletisini almaya devam ediyorum?

Oturumu kapatıp yeniden açtığınız halde bu hata iletisini almaya devam ediyorsanız özel bir gözatma oturumuyla yeniden deneyin.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>Sahip olduğum her abonelik için nasıl farklı bir kart kullanırım?

Ne yazık ki abonelikleriniz zaten aynı kartı kullanıyorsa, bu abonelikleri farklı kartları kullanacak şekilde ayırmak mümkün değildir. Ancak yeni bir aboneliğe kaydolduğunuzda, bu yeni abonelik için yeni bir ödeme yöntemi kullanmayı seçebilirsiniz.

### <a name="how-do-i-make-payments"></a>Nasıl ödeme yaparım?

Ödeme yönteminiz olarak bir kredi kartını ayarlarsanız, her faturalama döneminden sonra otomatik olarak kartınızı ücretlendiririz. Herhangi bir işlem yapmanız gerekmez.

[Fatura ile ödeme yapıyorsanız](pay-by-invoice.md), ödemenizi faturanızın en altında bulunan konuma gönderin.

### <a name="how-do-i-change-the-tax-id"></a>Vergi numarasını nasıl değiştirebilirim?

Vergi numarası eklemek veya güncelleştirmek için [Azure portalda](https://portal.azure.com) profilinizi güncelleştirin ve **Vergi numarası** ’nı seçin. Vergi numarası, vergi muafiyeti hesaplamaları için kullanılır ve faturanızda görünür.

## <a name="check-the-type-of-your-account"></a>Hesabınızın türünü denetleme

[!INCLUDE [billing-check-mca](../../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- Para tasarruf etmenizi sağlayıp sağlayamadığını görmek için [Azure rezervasyonları](../reservations/save-compute-costs-reservations.md) hakkında bilgi edinin.
- Kredi kartını silmek istiyorsanız bkz. [Azure fatura ödeme yöntemini silme](delete-azure-payment-method.md).