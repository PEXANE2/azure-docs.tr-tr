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
ms.openlocfilehash: 2f7b09c14553fdb5d642080d286ce123176b997f
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76991057"
---
# <a name="how-reservation-recommendations-are-created"></a>Rezervasyon önerilerinin oluşturulma biçimi
Azure ayrılmış örnek (RI) satın alma önerileri Azure Tüketimi [Rezervasyon Önerileri API’si](/rest/api/consumption/reservationrecommendations), [Azure Danışmanı](../..//advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs) ve Azure portaldaki rezervasyon satın alma deneyimi aracılığıyla sağlanır.

Aşağıdaki adımlar önerilerin nasıl hesaplanacağını tanımlar: 
1. Öneri altyapısı son 7, 30 ve 60 gün içinde verilen kapsamdaki kaynaklarınızın saatlik kullanımını değerlendirir.
2. Kullanım verilerine bağlı olarak altyapı, rezervasyonlar ile ve rezervasyonlar olmadan maliyetlerinizin simülasyonunu yapar.
3. Farklı miktarlar için maliyet simülasyonu yapılır ve tasarruf miktarını en üst düzeye çıkaran miktar önerilir.
4. Kaynaklarınız düzenli olarak kapatılıyorsa simülasyon hiçbir tasarruf olanağı bulamaz ve herhangi bir satın alma önerisi sağlanmaz.

## <a name="recommendations-in-azure-advisor"></a>Azure Danışmanı’nda Öneriler
Sanal makineler için rezervasyon satın alma önerileri Azure Danışmanı’nda kullanıma sunuldu. Aşağıdaki noktaları göz önünde bulundurun: 
- Danışman, yalnızca tek abonelik kapsamında öneriler sunar.
- 30 günlük inceleme süresi ile hesaplanan öneriler Danışman’da kullanıma sunuldu.
- Paylaşılan kapsam rezervasyonu satın alırsanız, Danışman rezervasyon satın alma önerilerinin kapatılması 30 güne kadar sürebilir.

## <a name="other-expected-api-behavior"></a>Diğer beklenen API davranışı
- Yedi günlük bir inceleme süresi kullanıldığında, VM’lerin bir günden fazla kapalı kalması durumunda öneri sunulmayabilir.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure rezervasyon indirimini sanal makinelere uygulama](../manage/understand-vm-reservation-charges.md) hakkında bilgi edinin.
