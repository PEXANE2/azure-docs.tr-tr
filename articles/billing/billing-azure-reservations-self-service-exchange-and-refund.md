---
title: Azure ayırmaları için self servis değişimlerinin ve para iadesi
description: Azure ayırmalarını nasıl değiş tokuş edebilir veya para iadesi yapabileceğinizi öğrenin.
author: yashesvi
manager: yashesvi
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/05/2019
ms.author: banders
ms.openlocfilehash: 5e38684500520d4565835456b94200aea399c938
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68814105"
---
# <a name="self-service-exchanges-and-refunds-for-azure-reservations"></a>Azure ayırmaları için self servis değişimlerinin ve para iadesi

Azure ayırmaları, gelişen ihtiyaçlarınızı karşılamaya yardımcı olma esnekliği sağlar. Bir rezervasyonu aynı türdeki başka bir rezervasyonla değiştirebilirsiniz. İhtiyaç duymadığınız rezervasyonları da iade ederek yılda 50.000 ABD dolarına kadar para iadesi alabilirsiniz.

Self servis değişim ve iptal özelliği US Government Kurumsal Anlaşma müşterileri tarafından kullanılamaz. Kullandıkça öde ve CSP dahil diğer ABD kamu Abonelik türleri desteklenir.

Mevcut bir ayırmayı değiştirmek veya geri ödeme yapmak için rezervasyon sırasında sahip erişiminizin olması gerekir.

## <a name="exchange-an-existing-reserved-instance"></a>Mevcut bir ayrılmış örneği Exchange

[Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade), rezervasyonunuzun üç hızlı adımla Exchange 'i değiştirebilirsiniz.

1. Para iadesi istediğiniz rezervasyonları seçin ve **Exchange**' e tıklayın.  
    ![Döndürülecek ayırmaları gösteren örnek resim](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-return.png)
2. Satın almak istediğiniz VM ürününü seçip miktar girin. Yeni satın alma toplamının, geri dönüş toplamanızdan daha fazla olduğundan emin olun. [Satın almadan önce doğru boyutu saptayın](../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy).  
    ![Bir Exchange ile satın alınabilecek VM ürününün gösterildiği örnek resim](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-select-purchase.png)
3. İşlem inceleyin ve işlemi doldurun.  
    ![Bir Exchange ile satın alma işlemini tamamlamak için VM ürününün gösterildiği örnek resim](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-confirm-exchange.png)

Bir ayırmayı geri ödeme yapmak için **rezervasyon ayrıntıları** ' na gidip **para iadesi**' ne tıklayın.

## <a name="how-transactions-are-processed"></a>İşlemler nasıl işlenir

İlk olarak, Microsoft mevcut ayırmayı iptal eder ve bu rezervasyon için eşit olarak derecelendirilen miktarı para iadesi. Bir Exchange varsa, yeni satın alma işlemi işlenir. Microsoft, hesap türüne ve ödeme yönteminize bağlı olarak aşağıdaki yöntemlerden birini kullanarak para iadelerini işler:

### <a name="enterprise-agreement-customers"></a>Kurumsal Anlaşma müşterileri

Para, orijinal satınalmanın bir tane kullanılarak yapılmışsa, alışverişlerde ve para kararlarının parasal taahhüdüne eklenir. Para taahhüdünün kullanıldığından emin olmak için orijinal satın almalarla bu yana olan tüm fazla mali faturalar yeniden açıldı ve tekrar açıldı. Rezervasyonu kullanan parasal taahhüt dönemi artık etkin değilse, kredi geçerli Kurumsal Sözleşme parasal Taahhüt döneminizin içine eklenir.

İlk satın alma işlemi fazla kullanım olarak yapılmışsa, Microsoft bir kredi faturası yayınlar.

### <a name="pay-as-you-go-invoice-payments-and-csp-program"></a>Kullandıkça Öde fatura ödemeleri ve CSP programı

Orijinal rezervasyon satın alma faturası iptal edilir ve ardından para iadesi için yeni bir fatura oluşturulur. Alışverişlerde, yeni faturada para iadesi ve yeni satın alma gösterilir. İade miktarı satınalmaya göre ayarlanır. Yalnızca bir rezervasyonu iade ediyorsanız, eşit olarak dağıtılan miktar Microsoft ile kalır ve gelecekteki bir rezervasyon satın alımından sonra ayarlanır.

### <a name="pay-as-you-go-credit-card-customers"></a>Kullandıkça Öde kredi kartı müşterileri

Orijinal fatura iptal edilir ve yeni bir fatura oluşturulur. Para, orijinal satın alma için kullanılan kredi kartına iade edildi. Kartınızı değiştirdiyseniz [desteğe başvurun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="cancel-exchange-and-refund-policies"></a>İptal, Exchange ve iade etme ilkeleri

Azure 'da iptaller, alışverişlerde ve para iadesi için aşağıdaki ilkeler bulunur.

**İptal ilkeleri**

- Bir ayırmayı iptal ederseniz,% 12 erken sonlandırma ücreti olabilir.
- İptal için alacağınız para iadesi, kalan Pro-derecelendirilen bakiyeniz eksi% 12 erken sonlandırma ücretinden sona ermiştir. İptal etmek için Azure portal rezervasyonuna gidin ve **para iadesi**' ni seçin.

**Exchange ilkeleri**

- Aynı türde yeni bir ayırma satın almak için birden fazla mevcut ayırma döndürebilirsiniz. Bir türe ait ayırmaları bir tane için alışverişi yapamazsınız. Örneğin, bir SQL ayırması satın almak için bir VM ayırması geri dönemiyoruz.
- Bir Exchange 'i yalnızca rezervasyon sahipleri işleyebilir. [Ayırmayı yönetebilen kullanıcıları nasıl ekleyeceğinizi veya değiştirebileceğinizi öğrenin](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
- Bir Exchange para iadesi ve yeniden oluşturma olarak işlenir – iptal ve yeni satın alma için farklı işlemler oluşturulur. Eşit olarak dağıtılan rezervasyon miktarı, ticaretinizi karşılaştırılan rezervasyonlar için iade edilir. Yeni satın alma için tam olarak ücretlendirilirsiniz. Eşit olarak dağıtılmış rezervasyon miktarı, döndürülmekte olan rezervasyonun günlük eşit eşit olarak eşit değeridir.
- Rezervasyonu satın almak için kullanılan kurumsal sözleşmenin kullanım dışı olması ve yeni bir sözleşme olarak yenilenmesi durumunda bile ayırmaları Exchange veya para iadesi yapabilirsiniz.
- Boyut, bölge, miktar ve dönem gibi herhangi bir rezervasyon özelliğini bir Exchange ile değiştirebilirsiniz.
- Yeni satın alma toplamı, döndürülen tutara eşit veya ondan büyük olmalıdır.
- Exchange kapsamında satın alınan yeni rezervasyon, Exchange zamanından itibaren yeni bir terim içerir.
- Alışverişlerde hiçbir ceza veya yıllık sınır yoktur.

**Para iadesi ilkeleri**
- Toplam para iadesi miktarı 12 aylık bir yuvarlama penceresinde $50.000 ABD Doları aşamaz.
- Para iadesi, satın alma fiyatınızın en düşük fiyatına veya rezervasyonun geçerli fiyatına göre hesaplanır.
- Yalnızca rezervasyon sahipleri bir para iadesi işleyebilir. [Ayırmayı yönetebilen kullanıcıları nasıl ekleyeceğinizi veya değiştirebileceğinizi öğrenin](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
- Microsoft, herhangi bir dönüşte% 12 ceza tutarında ücret ödemesine hak eder, ancak ceza Şu anda ücretlendirilmez.

## <a name="exchange-non-premium-storage-for-premium-storage"></a>Premium Depolama için Premium olmayan Exchange depolaması

Premium depolamayı desteklemeyen bir VM boyutu için satın alınan bir ayırmayı, karşılık gelen bir VM boyutuyla Exchange 'e dönüştürebilirsiniz. Örneğin, bir _F1s_için _F1_ . Exchange oluşturmak için, ayırma ayrıntıları ' na gidin ve **Exchange**' e tıklayın. Exchange, ayrılmış örneğin terimini sıfırlayamaz veya yeni bir işlem oluşturur.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

- Bir ayırmayı yönetme hakkında bilgi edinmek için bkz. [Azure ayırmalarını yönetme](billing-manage-reserved-vm-instance.md).
- Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
    - [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
    - [Azure 'da ayırmaları yönetme](billing-manage-reserved-vm-instance.md)
    - [Rezervasyon iskontosunun nasıl uygulanacağını anlayın](billing-understand-vm-reservation-charges.md)
    - [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage.md)
    - [Kurumsal kaydınız için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage-ea.md)
    - [Windows yazılım maliyetleri rezervasyonlar içermez](billing-reserved-instance-windows-software-costs.md)
    - [Iş Ortağı Merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmaları](/partner-center/azure-reservations)
