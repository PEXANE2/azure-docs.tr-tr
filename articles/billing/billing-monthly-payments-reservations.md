---
title: Peşin veya aylık ödemelerle Azure rezervasyonları satın alma
description: Peşin veya aylık ödemelerle Azure rezervasyonlarını nasıl satın alabileceğinizi öğrenin.
services: billing
author: bandersmsft
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: banders
ms.openlocfilehash: d211334ad2aa760cd63b98c6827fb2512811a1d3
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70806954"
---
# <a name="purchase-reservations-with-monthly-payments"></a>Aylık ödemelerle rezervasyonlar satın alma

Şu ana kadar Azure rezervasyonları için peşin ödeme gerekiyordu. Şimdiyse aylık ödemelerle rezervasyonlar için ödeme yapabilirsiniz. Tüm tutarı ödediğiniz peşin satın almadan farklı olarak aylık ödeme seçeneği, rezervasyonun toplam maliyetini dönemin her ayına eşit olarak böler. Peşin ve aylık rezervasyonların toplam maliyeti aynıdır ve aylık ödemeyi seçtiğinizde ekstra ücret ödemezsiniz.

Aylık ödeme tutarınız, yerel para biriminiz için güncel ayın döviz kuruna bağlı olarak değişiklik gösterebilir.

Şunlar için aylık ödemeler kullanılabilir:

- Sanal makineler
- SQL Veritabanı
- SQL Veri Ambarı
- Cosmos DB
- App Service damga pulu ücreti

[Azure portalında](https://portal.azure.com/?Microsoft_Azure_Reservations_EnableMultiCart=true&amp;paymentPlan=true#blade/Microsoft_Azure_Reservations/CreateBlade) rezervasyon satın alın.

![Rezervasyon satın almayı gösteren örnek](./media/billing-monthly-payments-reservations/purchase-reservation.png)

Bir rezervasyon satın alma işlemi yaparken ödeme planını görüntüleyebilirsiniz. **Tam ödeme planını görüntüle** seçeneğine tıklayın.

![Rezervasyon ödeme planını gösteren örnek](./media/billing-monthly-payments-reservations/prepurchase-schedule.png)

Satın alma işleminden sonra ödeme planını görüntülemek için bir rezervasyon seçin, **Rezervasyon sipariş kimliği**‘ne ve sonra **Ödemeler** sekmesine tıklayın.

## <a name="view-payments-made"></a>Yapılan ödemeleri görüntüleme

API’ler, kullanım verileri ve maliyet analizi kullanılarak yapılan ödemeleri görüntüleyebilirsiniz. Aylık olarak ödenen rezervasyonlar için sıklık değeri, kullanım verileri ve Rezervasyon Ücretleri API’sinde **yinelenen** olarak gösterilir. Peşin ödenen rezervasyonlar için değer, **tek seferlik** olarak gösterilir.

Maliyet analizi, varsayılan görünümde aylık satın almaları gösterir. Tüm satın almaları görmek istiyorsanız, **Masraf türü** için **satın alma** filtresini ve **Sıklık** için **yinelenen** filtresini uygulayın. Yalnızca rezervasyonları görüntülemek istiyorsanız, **Rezervasyon** için bir filtre uygulayın.

![Maliyet analizinde rezervasyon satın alma maliyetlerini gösteren örnek](./media/billing-monthly-payments-reservations/cost-analysis.png)

## <a name="switch-to-monthly-payments-at-renewal"></a>Yenileme sırasında aylık ödemelere geçme

Bir rezervasyonu yenilediğinizde, faturalama sıklığını aylık olarak değiştirebilirsiniz.

## <a name="exchange-and-refunds"></a>Değiştirme ve para iadeleri

Diğer rezervasyonlar gibi, aylık faturalama ile satın alınan rezervasyonları değiştirebilir veya para iadesi alabilirsiniz. Şu anda aylık faturalama ile satın alınan bir rezervasyon için değişim veya para iadesi başlatmak amacıyla bir destek isteği gönderebilirsiniz.

Aylık olarak ödenen bir rezervasyonu değiştirdiğinizde, yeni satın almanın toplam yaşam süresi maliyeti, iade edilen rezervasyon için iptal edilen kalan ödemelerden fazla olmalıdır. Değişimler için başka bir limit veya ücret yoktur. Aylık olarak faturalandırılan yeni bir rezervasyon satın almak için peşin ödenen bir rezervasyonu değiştirebilirsiniz. Ancak yeni rezervasyonun yaşam süresi değeri, iade edilen rezervasyonun eşit olarak dağıtılan değerinden büyük olmalıdır.

Aylık olarak ödenen bir rezervasyon iptal ediliyorsa Microsoft, iptal edilen gelecekteki taahhüt edilen ödemelere bir iptal ücreti uygulayabilir. Kalan taahhüt edilen ödemeler 50.000 ABD Doları para iadesi limitine tahakkuk eder.

Değişim ve para iadeleri hakkında daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](billing-azure-reservations-self-service-exchange-and-refund.md).

## <a name="faq"></a>SSS

S. Azure, “Kısmi peşin rezervasyonlar” sunuyor mu?<br>
A. Hayır. Peşin ve aylık ödenen rezervasyonların maliyetleri aynı olduğundan Microsoft, kısmi peşin ödemeleri desteklemez.

S. Microsoft Bulut Çözümü Sağlayıcısı (CSP) programı için aylık ödeme var mı?<br>
A. Evet, iş ortakları Azure portalında CSP müşterileri için rezervasyonlar satın alabilir. İş ortağı merkezinde aylık faturalamalı rezervasyonlar satın alınamaz.

S. ABD Azure Kamu müşterisiyim, rezervasyon satın alımları için aylık ödeme yapabilir miyim?<br>
A. Şu anda hayır.

S. Destek bileti oluşturmak yerine Azure portalında ne zaman kendim değişim yapabilir veya para iadesi alabilirim?<br>
A. Şu anda hayır. Aylık ödemelerle rezervasyon değiştirme ve para iadesi alma istekleri, Azure desteği tarafından işlenir.

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyonlar hakkında daha fazla bilgi edinmek için bkz. [Azure Ayrılmış Sanal Makine Örnekleri nedir?](billing-save-compute-costs-reservations.md)
- Rezervasyon satın almadan önce gerçekleştirmeniz gereken görevler hakkında bilgi edinmek için bkz. [Satın almadan önce doğru sanal makine boyutunu belirleme](../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy)
