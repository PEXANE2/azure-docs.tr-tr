---
title: Azure Olay Idamı'nda coğrafi felaket kurtarma | Microsoft Dokümanlar
description: Azure Olay Idamı'nın coğrafi durum kurtarmayı (GeoDR) otomatik olarak nasıl desteklediğini açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: spelluru
ms.openlocfilehash: 5b5c973a8daa8776efb0909092c569ea46902265
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66307325"
---
# <a name="server-side-geo-disaster-recovery-in-azure-event-grid"></a>Azure Olay Grid'de sunucu tarafı coğrafi felaket kurtarma
Olay Grid şimdi meta-veri otomatik bir jeo felaket kurtarma (GeoDR) sadece yeni, ama tüm mevcut etki alanları, konular ve olay abonelikleri vardır. Tüm Azure bölgesi çökerse, Olay Idamı etkinlikle ilgili tüm altyapı meta verilerinizi eşleştirilmiş bir bölgeyle eşitletirmiş olur. Yeni olaylarınız sizin müdahaleniz olmadan tekrar akmaya başlayacak. 

Olağanüstü durum kurtarma iki ölçümle ölçülür:

- [Kurtarma Noktası Hedefi (RPO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_Point_Objective): kaybedilebilecek dakika veya saat veri.
- [Kurtarma Süresi Hedefi (RTO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_time_objective): hizmetin saat dakikaları kapalı olabilir.

Olay Grid otomatik failover meta veri (olay abonelikleri vb) ve veri (olaylar) için farklı RDO ve RTO'lar vardır. Aşağıdakilerden farklı belirtimler gerekiyorsa, yine de [konu durumu apis kullanarak üzerinde](custom-disaster-recovery.md)kendi istemci tarafı başarısız uygulayabilirsiniz.

## <a name="recovery-point-objective-rpo"></a>Kurtarma noktası hedefi (RPO)
- **Meta veri RPO**: sıfır dakika. Olay Izgara'da bir kaynak oluşturulduğunda, bölgeler arasında anında çoğaltılır. Bir hata oluştuğunda, hiçbir meta veri kaybolmaz.
- **Veri RPO**: Sisteminiz sağlıklıysa ve bölgesel arıza sırasında mevcut trafiğe yakalanmışsa, etkinlikler için RPO yaklaşık 5 dakikadır.

## <a name="recovery-time-objective-rto"></a>Kurtarma süresi hedefi (RTO)
- **Metadata RTO**: Genellikle çok daha hızlı bir şekilde, 60 dakika içinde gerçekleşir rağmen, Olay Grid konular ve abonelikler için arama oluşturma/güncelleme/silme kabul etmeye başlar.
- **Veri RTO**: Meta veriler gibi, genellikle çok daha hızlı olur, ancak 60 dakika içinde, Olay Grid bölgesel bir arıza sonra yeni trafik kabul başlayacak.

> [!NOTE]
> Olay Izgara meta veri GeoDR için maliyet: $0.


## <a name="next-steps"></a>Sonraki adımlar
Kendi istemci tarafı failover mantığını uygulamak istiyorsanız, [bkz.](custom-disaster-recovery.md)
