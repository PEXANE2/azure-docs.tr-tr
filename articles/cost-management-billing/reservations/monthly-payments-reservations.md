---
title: Peşin veya aylık ödemelerle Azure rezervasyonları satın alma
description: Peşin veya aylık ödemelerle Azure rezervasyonlarını nasıl satın alabileceğinizi öğrenin.
author: bandersmsft
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: banders
ms.openlocfilehash: ede60adc13dadc38e18ee5ade468e01b16523f4f
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80235740"
---
# <a name="purchase-reservations-with-monthly-payments"></a>Aylık ödemelerle rezervasyonlar satın alma

Aylık ödemelerle rezervasyonlar için ödeme yapabilirsiniz. Tüm tutarı ödediğiniz peşin satın almadan farklı olarak aylık ödeme seçeneği, rezervasyonun toplam maliyetini dönemin her ayına eşit olarak böler. Peşin ve aylık rezervasyonların toplam maliyeti aynıdır ve aylık ödemeyi seçtiğinizde ekstra ücret ödemezsiniz.

Rezervasyonunuz Microsoft müşteri sözleşmesi (MCA) kullanılarak satın alındıysa, aylık ödeme tutarınız yerel para biriminiz için güncel ayın döviz kuruna bağlı olarak değişiklik gösterebilir.

Şunlar için aylık ödemeler kullanılamaz: Databricks, SUSE Linux rezervasyonları, Red Hat Planları ve Azure Red Hat OpenShift Compute.

[Azure portalında](https://ms.portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/Docs) rezervasyon satın alın.

![Rezervasyon satın almayı gösteren örnek](./media/monthly-payments-reservations/purchase-reservation.png)

Bir rezervasyon satın alma işlemi yaparken ödeme planını görüntüleyebilirsiniz. **Tam ödeme planını görüntüle** seçeneğine tıklayın.

![Rezervasyon ödeme planını gösteren örnek](./media/monthly-payments-reservations/prepurchase-schedule.png)

Satın alma işleminden sonra ödeme planını görüntülemek için bir rezervasyon seçin, **Rezervasyon sipariş kimliği**‘ne ve sonra **Ödemeler** sekmesine tıklayın.

## <a name="view-payments-made"></a>Yapılan ödemeleri görüntüleme

API’ler, kullanım verileri ve maliyet analizi kullanılarak yapılan ödemeleri görüntüleyebilirsiniz. Aylık olarak ödenen rezervasyonlar için sıklık değeri, kullanım verileri ve Rezervasyon Ücretleri API’sinde **yinelenen** olarak gösterilir. Peşin ödenen rezervasyonlar için değer, **tek seferlik** olarak gösterilir.

Maliyet analizi, varsayılan görünümde aylık satın almaları gösterir. Satın alınan tüm öğeleri görmek istiyorsanız **Masraf türü** için **satın alma** filtresini ve **Sıklık** için **yinelenen** filtresini uygulayın. Yalnızca rezervasyonları görüntülemek istiyorsanız, **Rezervasyon** için bir filtre uygulayın.

![Maliyet analizinde rezervasyon satın alma maliyetlerini gösteren örnek](./media/monthly-payments-reservations/cost-analysis.png)

## <a name="switch-to-monthly-payments-at-renewal"></a>Yenileme sırasında aylık ödemelere geçme

Bir rezervasyonu yenilediğinizde, faturalama sıklığını aylık olarak değiştirebilirsiniz.

## <a name="exchange-and-refunds"></a>Değiştirme ve para iadeleri

Diğer rezervasyonlar gibi, aylık faturalama ile satın alınan rezervasyonları değiştirebilir veya para iadesi alabilirsiniz. 

Aylık olarak ödenen bir rezervasyonu değiştirdiğinizde, yeni satın almanın toplam yaşam süresi maliyeti, iade edilen rezervasyon için iptal edilen kalan ödemelerden fazla olmalıdır. Değişimler için başka bir limit veya ücret yoktur. Aylık olarak faturalandırılan yeni bir rezervasyon satın almak için peşin ödenen bir rezervasyonu değiştirebilirsiniz. Ancak yeni rezervasyonun yaşam süresi değeri, iade edilen rezervasyonun eşit olarak dağıtılan değerinden büyük olmalıdır.

Aylık ödeme yapılan bir rezervasyonu iptal ederseniz, iptal edilen gelecek ödemeleri 50.000 ABD doları para iadesi sınırına kadar tahakkuk ettirilir.

Değişim ve para iadeleri hakkında daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](exchange-and-refund-azure-reservations.md).

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyonlar hakkında daha fazla bilgi edinmek için bkz. [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)
- Rezervasyon satın almadan önce gerçekleştirmeniz gereken görevler hakkında bilgi edinmek için bkz. [Satın almadan önce doğru sanal makine boyutunu belirleme](../../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy)
