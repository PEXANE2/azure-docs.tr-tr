---
title: Ön veya aylık ödemeler ile Azure ayırmaları satın alın
description: Azure ayırmalarını ön veya aylık ödemeler ile nasıl satın alabileceğinizi öğrenin.
services: billing
author: bandersmsft
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: banders
ms.openlocfilehash: d211334ad2aa760cd63b98c6827fb2512811a1d3
ms.sourcegitcommit: b8578b14c8629c4e4dea4c2e90164e42393e8064
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70806954"
---
# <a name="purchase-reservations-with-monthly-payments"></a>Aylık ödemeler ile ayırmaları satın alma

Bu aşamada, Azure ayırmaları için ödeme yapılması gerekir. Artık aylık ödemeler içeren rezervasyonlar için ödeme yapabilirsiniz. Tam miktarı ödediğiniz bir ön satın alma işleminden farklı olarak, aylık ödeme seçeneği, ayırmanın toplam maliyetini, dönemin her bir ayda eşit oranda böler. En önde ve aylık rezervasyonların toplam maliyeti aynıdır ve aylık ödeme yapmayı seçtiğinizde fazladan ücret ödemezsiniz.

Aylık ödeme tutarınız, yerel para biriminiz için geçerli aya ait Pazar döviz kuruna bağlı olarak farklılık gösterebilir.

Aylık ödemeler şu şekilde kullanılabilir:

- Sanal makineler
- SQL Veritabanı
- SQL Veri Ambarı
- Cosmos DB
- App Service damga ücreti

[Azure Portal](https://portal.azure.com/?Microsoft_Azure_Reservations_EnableMultiCart=true&amp;paymentPlan=true#blade/Microsoft_Azure_Reservations/CreateBlade)rezervasyonları satın alın.

![Rezervasyon satın almayı gösteren örnek](./media/billing-monthly-payments-reservations/purchase-reservation.png)

Bir rezervasyon satın alma yaparken, ödeme zamanlamasını görüntüleyebilirsiniz. **Tam ödeme zamanlamasını görüntüle**' ye tıklayın.

![Rezervasyon ödeme zamanlamasını gösteren örnek](./media/billing-monthly-payments-reservations/prepurchase-schedule.png)

Satın alma işleminden sonra ödemeler zamanlamasını görüntülemek için bir rezervasyon seçin, **rezervasyon SIPARIŞI kimliğine**ve sonra **ödemeler** sekmesine tıklayın.

## <a name="view-payments-made"></a>Yapılan ödemeleri görüntüleme

API 'Ler, kullanım verileri ve maliyet analizi kullanılarak yapılan ödemeleri görüntüleyebilirsiniz. Aylık olarak ödenen rezervasyonlar için sıklık değeri, kullanım verileri ve rezervasyon ücretleri API 'sinde **yineleniyor** olarak gösterilir. Ödenen rezervasyonlar için, değer **kerelik**olarak gösterilir.

Maliyet analizi, aylık satınalmaları varsayılan görünümde gösterir. Tüm satınalmaları görmek için, **ücretlendirme türü** için **satın alma** filtresini uygulayın ve **Sıklık** için **yinelenir** . Yalnızca rezervasyonları görüntülemek için, **ayırma**için bir filtre uygulayın.

![Maliyet analizinde rezervasyon satın alma maliyetlerini gösteren örnek](./media/billing-monthly-payments-reservations/cost-analysis.png)

## <a name="switch-to-monthly-payments-at-renewal"></a>Yenileme sırasında aylık ödemelere geç

Bir ayırmayı yenilediğinizde faturalandırma sıklığını aylık olarak değiştirebilirsiniz.

## <a name="exchange-and-refunds"></a>Exchange ve para iadesi

Diğer rezervasyonlar gibi, aylık faturalandırma ile satın alınan rezervasyonları veya para iadesi sağlayabilirsiniz. Şu anda, aylık faturalandırma ile satın alınan bir rezervasyon için Exchange veya para iadesi başlatmak üzere bir destek isteği gönderebilirsiniz.

Aylık olarak ödenen bir rezervasyon alışverişi yaptığınızda, yeni satınalmanın toplam ömür maliyeti, döndürülen rezervasyon için iptal edilen soltover ödemelerden daha büyük olmalıdır. Alışverişlerde başka sınırlamalar veya ücretler yoktur. En öne ödenen ve aylık olarak faturalandırılan yeni bir rezervasyon satın almak üzere ödenen bir rezervasyon alışverişi yapabilirsiniz. Ancak, yeni ayırmanın yaşam süresi değeri, döndürülmekte olan rezervasyonun eşit olarak dağıtılmış değerinden büyük olmalıdır.

Aylık olarak ödenen bir rezervasyonu iptal ederseniz Microsoft, iptal edilen daha sonra kaydedilen ödemelere bir iptal ücreti uygulayabilir. Kalan taahhüt edilen ödemeler $50.000 ABD Doları iadesi sınırına doğru tahakkuk eder.

Exchange ve para iadesi hakkında daha fazla bilgi için bkz. [Azure ayırmaları Için self servis değişimlerinin ve para iadesi](billing-azure-reservations-self-service-exchange-and-refund.md).

## <a name="faq"></a>SSS

S. Azure, "kısmi ön ayırmalar" sunuyor mu?<br>
A. Hayır. Önde ve ayda ödenen rezervasyonların maliyetleri aynı olduğundan, Microsoft kısmen ön ödemeleri desteklemez.

S. Microsoft Bulut çözümü sağlayıcısı (CSP) programı için aylık ödemeler kullanılabilir mi?<br>
A. Evet, iş ortakları CSP müşterileri için Azure portal ayırmaları satın alabilir. Aylık faturalandırma ile rezervasyonları satın alma özelliği Iş Ortağı Merkezi 'nde kullanılamaz.

S. BIZE Azure Kamu müşterisi kullanıyorum, rezervasyon satın alma işlemleri için aylık ödeme yapabilir miyim?<br>
A. Şu anda değil.

S. Bir destek bileti oluşturmak yerine Azure portal ne zaman Exchange veya para iadesi yapabilirim?<br>
A. Şu anda değil. Aylık ödemeler ile Exchange ve geri ödeme rezervasyonları için istekler Azure desteği tarafından işlenir.

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyonlar hakkında daha fazla bilgi edinmek için bkz. [Azure rezervasyonları nelerdir?](billing-save-compute-costs-reservations.md)
- Bir ayırmayı satın almadan önce gerçekleştirmeniz gereken görevler hakkında bilgi edinmek için, bkz. [satın almadan önce doğru VM boyutunu belirleme](../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy)
