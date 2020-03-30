---
title: Tasarruf etmek için Azure Veri Gezgini işaretlemesi için ön ödeme
description: Azure Veri Gezgini maliyetlerinizden tasarruf etmek için Azure Veri Gezgini'ni nasıl satın alacağınızı öğrenin.
author: avneraa
ms.author: avnera
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/03/2019
ms.openlocfilehash: bbb2108967353b851a0fa0ee610ec30380e3fc50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75969279"
---
# <a name="prepay-for-azure-data-explorer-markup-units-with-azure-data-explorer-reserved-capacity"></a>Azure Veri Gezgini ayrılmış kapasiteye sahip Azure Veri Gezgini biçimlendirme birimleri için ön ödeme

Azure Veri Gezgini ile, çıktıkça öde fiyatlarına kıyasla biçimlendirme birimleri için ön ödeme yaparak tasarruf edin. Azure Veri Gezgini ayrılmış kapasite yle, Azure Veri Gezgini biçimlendirme maliyetlerinde önemli bir indirim elde etmek için bir veya üç yıllık bir süre için Azure Veri Gezgini kullanımı için bir taahhütte bulunabilirsiniz. Azure Veri Gezgini ayrılmış kapasitesatın almak için, yalnızca terimi belirtmeniz gerekir, tüm bölgelerdeki Azure Veri Gezgini'nin tüm dağıtımları için geçerli olacaktır.

Bir rezervasyon satın alarak, bir veya üç yıllık bir süre için işaretleme maliyetleri için ön ödeme yapıyorsunuz. Bir rezervasyon satın alır almaz, rezervasyon öznitelikleriyle eşleşen Azure Veri Gezgini işaretleme ücretleri artık kullandıkça öde fiyatlarıüzerinden ücretlendirilmez. Zaten çalışan Azure Veri Gezgini kümeleri veya yeni dağıtılan kümeler otomatik olarak avantaj dan yararlanır. Bu rezervasyon, kümelerle ilişkili işlem, ağ veya depolama ücretlerini kapsamaz. Bu kaynaklar için ayrılmış kapasitenin ayrıca satın alınması gerekir. Rezervasyon döneminin sonunda, faturalandırma avantajı nın süresi doluyor ve Azure Veri Gezgini biçimlendirme birimleri size ödeme fiyatından faturalandırılır. Rezervasyonlar otomatik olarak yenilemez. Fiyatlandırma bilgileri için [Azure Veri Gezgini fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/data-explorer/)bakın.

Azure veri gezgini için ayrılmış kapasiteyi [Azure portalından](https://portal.azure.com)satın alabilirsiniz. Azure Veri Gezgini'ni satın almak için ayrılmış kapasite:

* En az bir Enterprise veya Pay-As-You-Go aboneliğinin sahibi olmalısınız.
* Kurumsal abonelikler için, [EA portal](https://ea.azure.com)’da **Ayrılmış Örnek Ekle** seçeneği etkinleştirilmelidir. Alternatif olarak, bu ayar devre dışı bırakılmışsa, abonelikte bir EA Yöneticisi olmalısınız.
* Bulut Çözüm Sağlayıcısı (CSP) programı için yalnızca yönetici aracılar veya satış temsilcileri Azure Veri Gezgini ayrılmış kapasite satın alabilir.

Kurumsal müşterilerin ve Ödeme Olarak Ödeme müşterilerinin rezervasyon satın alımları için nasıl ücretlendirildikhakkında ayrıntılı bilgi için bkz:
* [Kurumsal kaydınız için Azure rezervasyon kullanımını anlama](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md) 
* [Kullan-Kullan aboneliğiniz için Azure rezervasyon kullanımını anlayın.](../cost-management-billing/reservations/understand-reserved-instance-usage.md)

## <a name="determine-the-right-markup-usage-before-purchase"></a>Satın almadan önce doğru biçimlendirme kullanımını belirleme

Rezervasyonun boyutu, varolan veya yakında dağıtılacak Azure Veri Gezgini kümeleri tarafından kullanılan toplam Azure Veri Gezgini biçimlendirme birimi sayısına dayanmalıdır. Biçimlendirme birimlerinin sayısı, üretimdeki Azure Veri Gezgini motor küme çekirdeği sayısına eşittir (geliştirme/test SKU'su dahil değildir). 

## <a name="buy-azure-data-explorer-reserved-capacity"></a>Azure Veri Gezgini rezerve edilmiş kapasite satın alın

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. **Tüm Hizmetleri** > Seçin**Rezervasyonları** > **Hemen Satın Alın.** **Ekle**’yi seçin
1. Ürün **Türü Seç** bölmesinde, Azure Veri Gezgini biçimlendirme birimleri için yeni bir rezervasyon satın almak için **Azure Veri Gezgini'ni** seçin. 
1. **Satın Al'ı** Seçin
1. Gerekli alanları doldurun. 

    ![Satın alma sayfası](media/pricing-reserved-capacity/purchase-page.png)

1. **Maliyetler** bölümünde Azure Veri Gezgini biçimlendirmesinin ayrılmış kapasite rezervasyonunun maliyetini gözden geçirin.
1. **Satın al**'ı seçin.
1. Satın alma durumunu görmek için **bu Rezervasyonu Görüntüle'yi** seçin.

## <a name="cancellations-and-exchanges"></a>İptaller ve değişimler

Azure Veri Gezgini ayrılmış kapasite rezervasyonunuzu iptal etmeniz gerekiyorsa, %12 erken sonlandırma ücreti olabilir. İadeler, satın alma fiyatınızın en düşük fiyatına veya rezervasyonun geçerli fiyatına bağlıdır. Para iadesi yıllık 50.000 ABD doları ile sınırlıdır. Yapılacak para iadesi, %12 erken sonlandırma ücreti kesildikten sonra kalan kullanım süresine göre hesaplanır. İptal talebinde bulunmak için Azure portalındaki rezervasyona gidin ve destek isteği oluşturmak için **Geri Ödeme'yi** seçin.

Azure Veri Gezgini rezerve edilmiş kapasite rezervasyonunuzu başka bir terimle değiştirmeniz gerekiyorsa, bu rezervasyonla eşit veya daha değerli başka bir rezervasyonla değiştirebilirsiniz. Yeni ayırma işleminin başlangıç tarihi değiştirilen ayırma işleminin başlangıç tarihiyle aynı olmaz. 1 veya 3 yıllık dönem, yeni rezervasyonu oluşturduğunuz andan itibaren başlar. Değişim isteğinde bulunmak için Azure portalındaki rezervasyona gidin ve destek isteği oluşturmak için **Exchange'i** seçin.

Rezervasyonların nasıl değiş tokuş edilip geri ödenip ödenip ödenip ödenip yapılacağını öğrenmek için [rezervasyon değişimlerine ve geri ödemelere](../cost-management-billing/reservations/exchange-and-refund-azure-reservations.md)bakın.

## <a name="manage-your-reserved-capacity-reservation"></a>Rezerve edilen kapasite rezervasyonunuzu yönetin

Azure Veri Gezgini biçimlendirme birimleri rezervasyon indirimi, Azure Veri Gezgini ayrılmış kapasite rezervasyon kapsamı ve öznitelikleriyle eşleşen biçimlendirme birimi sayısına otomatik olarak uygulanır. 


> [!NOTE]
> * Azure Veri Gezgini rezerve edilmiş kapasite rezervasyonunun kapsamını [Azure portalı](https://portal.azure.com), PowerShell, CLI veya API üzerinden güncelleştirebilirsiniz.
> * Azure Veri Gezgini ayrılmış kapasite rezervasyonlarını nasıl yönetebilirsiniz öğrenmek için Bkz. [Azure Veri Gezgini ayrılmış kapasiteyi yönetin.](../cost-management-billing/reservations/understand-azure-data-explorer-reservation-charges.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Rezervasyonları nedir?](../cost-management-billing/reservations/save-compute-costs-reservations.md)
* [Azure Ayırmalarını yönetme](../cost-management-billing/reservations/manage-reserved-vm-instance.md)
* [Azure Ayrılmış Sanal Makine Örnekleri indirimini anlama](../cost-management-billing/reservations/understand-reservation-charges.md)
* [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](../cost-management-billing/reservations/understand-reserved-instance-usage.md)
* [Kurumsal kaydınız için rezervasyon kullanımını anlama](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md)
* [İş Ortağı Merkezi Bulut Çözümü Sağlayıcısı (CSP) programındaki Azure Ayrılmış Sanal Makine Örnekleri](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız varsa, [bir destek isteği oluşturun.](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)
