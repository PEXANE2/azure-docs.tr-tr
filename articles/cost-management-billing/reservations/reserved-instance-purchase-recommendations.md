---
title: Azure rezervasyon önerilerinin oluşturulma biçimi
description: Sanal makineler için Azure rezervasyon önerilerinin nasıl oluşturulduğunu öğrenin.
author: banders
ms.reviewer: yashar
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/28/2020
ms.author: banders
ms.openlocfilehash: a43e0c4d86bd47c953b50ab9fb1fd8df00e7df3d
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76851360"
---
# <a name="how-reservation-recommendations-are-created"></a>Rezervasyon önerilerinin oluşturulma biçimi

Ayrılmış örnek (RI) satın alma önerileri, Azure Tüketimi [Rezervasyon Önerisi API'si](/rest/api/consumption/reservationrecommendations) tarafından oluşturulur. API'nin önerileri [Azure Danışmanı](../..//advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs) tarafından da kullanılır. Danışman, önerileri Azure portalında gösterir.

Azure'da çalışan VM'leriniz varsa peşin ödeme yaparak RI'lere özgü indirimli fiyatlardan yararlanabilirsiniz. Microsoft Tüketim önerisi API'si 7, 30 ve 60 gün boyunca kullanımınızı değerlendirerek RI'ler için en uygun yapılandırmaları önerir. RI kullanmadığınızda ödeyeceğiniz tutarı RI ile ödeyeceğiniz tutarla karşılaştırarak tasarruf yapmanızı sağlar.

Azure Danışmanı, 30 günlük kullanımı temel alan öneriler gösterir.

Kolaylık olması açısından aşağıdaki örnekte yedi günlük kullanımı temel alan öneriye ait hesaplamalar gösterilmiştir. 30 veya 60 günlük kullanımı temel alan öneriler için de aynı yöntem kullanılır.

## <a name="calculation-method-example"></a>Hesaplama yöntemi örneği

Belirli bir SKU ve bölge için yedi günü (168 saat) temel alan saatlik Windows VM kullanımınızı düşünelim. Yedi gün içindeki minimum kullanım 65 birim, en yüksek kullanım ise 127 birimdir. Bu örnekte 79. saat 80 VM kullandı ve siz 75 RI satın aldınız.

75 ayrılmış örnek satın alırsanız 79 saat için şu maliyetleri ödersiniz:

- 75 ayrılmış örnek. Bu maliyet, RI satın alma aşamasında peşin ödenir.
- Ayrılmış örnekler, çalışan VM'lerin donanım maliyetini kapsar ve bu nedenle yalnızca yazılım fiyatı üzerinden 75 saat için ödeme yaparsınız.
- 79. saatte kullanım 80 olduğundan beş saatlik Windows ile donanım birleşimi ölçüm fiyatı üzerinden ödeme yaparsınız. Birleşik fiyat, Kurumsal Anlaşmanıza (EA) veya kullandıkça öde fiyatına göre belirlenir.

75 RI satın alırsanız toplam maliyeti hesaplamak için önceki saatlik maliyetleri toplayabilirsiniz. Ayrıca ücretlerinizi kullanarak geçerli maliyetinizi de hesaplayabilirsiniz. Bu örnekte iki tutar arasındaki fark, yedi günde gerçekleştirdiğiniz tasarruftur.

API, belirli kullanım noktalarına özgü hesaplamalar yapar. Ardından en fazla tasarrufun sağlandığı önerilen miktarı döndürür. Aşağıdaki örnekte yer alan grafik, tasarrufun 68 seviyesinde zirve yaptığını göstermektedir. Bunun üzerinde tasarruflar azaldığından API 68 önerisinde bulunmaktadır.

![En yüksek tasarrufu gösteren diyagram](./media/reserved-instance-purchase-recommendations/peak-savings.png)

## <a name="other-expected-api-behavior"></a>Diğer beklenen API davranışı

- [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) kullanıldığında API, Windows için elde edilebilecek olası tasarrufu gösterir. Avantaj kullanılmadığında API önerileri çekirdek Windows maliyetlerine göre oluşturulur. Mümkünse tasarruf oranını artırmak için Azure Hibrit Avantajı'nı kullanabilirsiniz.
- Yedi günlük bir inceleme süresi kullanıldığında, VM'lerin bir günden fazla kapalı kalması durumunda öneri sunulmayabilir.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure rezervasyon indirimini sanal makinelere uygulama](../manage/understand-vm-reservation-charges.md) hakkında bilgi edinin.
