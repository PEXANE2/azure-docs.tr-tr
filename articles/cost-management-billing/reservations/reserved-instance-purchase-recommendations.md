---
title: Azure rezervasyon önerileri
description: Azure rezervasyon önerileri hakkında bilgi edinin.
author: banders
ms.author: banders
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 09/02/2020
ms.openlocfilehash: 869009d7dd26685842da3c948fbdc058a38feb21
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89398435"
---
# <a name="reservation-recommendations"></a>Rezervasyon önerileri

Azure ayrılmış örnek (RI) satın alma önerileri Azure Tüketimi [Rezervasyon Önerileri API’si](/rest/api/consumption/reservationrecommendations), [Azure Danışmanı](../../advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs) ve Azure portaldaki rezervasyon satın alma deneyimi aracılığıyla sağlanır.

Aşağıdaki adımlar önerilerin nasıl hesaplanacağını tanımlar:

1. Öneri altyapısı son 7, 30 ve 60 gün içinde verilen kapsamdaki kaynaklarınızın saatlik kullanımını değerlendirir.
2. Kullanım verilerine bağlı olarak altyapı, rezervasyonlar ile ve rezervasyonlar olmadan maliyetlerinizin simülasyonunu yapar.
3. Farklı miktarlar için maliyet simülasyonu yapılır ve tasarruf miktarını en üst düzeye çıkaran miktar önerilir.
4. Kaynaklarınız düzenli olarak kapatılıyorsa simülasyon hiçbir tasarruf olanağı bulamaz ve herhangi bir satın alma önerisi sağlanmaz.
5. Önerilerde, isteğe bağlı kullanım oranlarınız üzerinden size sağlanan özel indirimler hesaba katılır.

## <a name="recommendations-in-the-azure-portal"></a>Azure portalındaki öneriler

Satın alma deneyiminde, Azure portalında rezervasyon satın alma önerileri gösterilir. Öneriler, **Önerilen Miktar** ile birlikte gösterilir. Azure'ın önerdiği miktar satın alındığında mümkün olan en yüksek tasarruf elde edilir. İstediğiniz miktarı satın alabilirsiniz ama farklı bir miktar satın alırsanız tasarruflarınız en iyi düzeyde olmaz.

Şimdi bunun nedenini bazı örneklerle gösterelim.

Seçili öneriyi gösteren aşağıdaki örnek resimde Azure 6 adet satın alınmasını öneriyor.

:::image type="content" source="./media/reserved-instance-purchase-recommendations/recommended-quantity.png" alt-text="Rezervasyon satın alma önerisini gösteren örnek" lightbox="./media/reserved-instance-purchase-recommendations/recommended-quantity.png" :::

**Ayrıntılara göz atın** bağlantısını seçtiğinizde öneri hakkında daha fazla bilgi gösterilir. Aşağıdaki resimde öneri hakkındaki ayrıntılar gösterilir. Önerilen miktar, geçmiş kullanımınız temel alınarak mümkün olan en yüksek kullanım için hesaplanır. Kullanımınız tutarsızsa, öneriniz %100 kullanım için hesaplanmamış olabilir. Örnekte kullanımın zamanla dalgalanma gösterdiğine dikkat edin. Rezervasyon maliyeti, olası tasarruflar ve kullanım yüzdesi gösterilir.

:::image type="content" source="./media/reserved-instance-purchase-recommendations/recommended-quantity-details.png" alt-text="Rezervasyon satın alma önerisinin ayrıntılarını gösteren örnek" :::

Rezervasyonu önerilen miktarın üzerine çıkarır veya altına düşürürseniz, grafik ve tahmini değer değişir. Rezervasyon miktarını artırdığınızda tasarruflarınız düşer çünkü sonuçta düşük rezervasyon kullanımıyla karşı karşıya kalırsınız. Diğer bir deyişle tam olarak kullanılmayan rezervasyonlar için ödeme yaparsınız.

Rezervasyon miktarını azaltırsanız tasarruflarınız da azalır. Kullanımınız artsa da, rezervasyonlarınızın kullanımınızı tam olarak karşılamadığı dönemler olacaktır. Rezervasyon miktarınızı aşan kullanımda, daha pahalı olan kullandıkça öde kaynakları kullanılır. Aşağıdaki örnek resimde bu nokta gösterilir. Rezervasyon miktarını kendimiz 4'e düşürdük. Rezervasyon kullanımı artar ama genel olarak tasarruflar azalır çünkü kullandıkça öde maliyetleri söz konusudur.

:::image type="content" source="./media/reserved-instance-purchase-recommendations/recommended-quantity-details-changed.png" alt-text="Değişen rezervasyon satın alma önerisinin ayrıntılarını gösteren örnek" :::

Rezervasyonlardan yapılan tasarrufları en üst düzeye çıkarmak için mümkün olduğunca öneriye yakın miktarda rezervasyon satın almaya çalışın.

## <a name="recommendations-in-azure-advisor"></a>Azure Danışmanı’nda Öneriler

Rezervasyon satın alma önerileri Azure Danışmanı’nda kullanıma sunulmuştur. Aşağıdaki noktaları göz önünde bulundurun:

- Danışman, yalnızca tek abonelik kapsamında öneriler sunar. Faturalama kapsamının (faturalama hesabı veya faturalama profili) tamamı için önerileri görmek istiyorsanız Azure portal > Rezervasyonlar > Ekle’ye gidin ve öneri almak istediğiniz türü seçin.
- Son 30 günlük kullanım eğilimleri hesaba katılarak Danışman’da öneriler sunulur.
- Öneri miktarı ve tasarrufları mümkün olduğunda 3 Yıllık rezervasyona yöneliktir. Hizmet için 3 Yıllık rezervasyon satılmıyorsa, öneri 1 yıllık rezervasyon fiyatı kullanılarak hesaplanır.
- Önerilerde, isteğe bağlı kullanım oranlarınız üzerinden size sağlanan özel indirimler hesaba katılır.
- Paylaşılan kapsam rezervasyonu satın alırsanız, Danışman rezervasyon satın alma önerilerinin kapatılması 5 güne kadar sürebilir.

## <a name="other-expected-api-behavior"></a>Diğer beklenen API davranışı

- Yedi günlük bir inceleme süresi kullanıldığında, VM’lerin bir günden fazla kapalı kalması durumunda öneri sunulmayabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure rezervasyon indirimini sanal makinelere uygulama](../manage/understand-vm-reservation-charges.md) hakkında bilgi edinin.
