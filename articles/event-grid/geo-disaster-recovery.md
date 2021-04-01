---
title: Azure Event Grid coğrafi olağanüstü durum kurtarma | Microsoft Docs
description: Azure Event Grid coğrafi olağanüstü durum kurtarmayı (GeoDR) otomatik olarak nasıl desteklediğini açıklar.
ms.topic: conceptual
ms.date: 11/19/2020
ms.openlocfilehash: 10beaf0ae25f3ed9b7bcda5961a89494b18b84d9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94980871"
---
# <a name="server-side-geo-disaster-recovery-in-azure-event-grid"></a>Azure Event Grid 'de sunucu tarafı coğrafi olağanüstü durum kurtarma
Event Grid artık yalnızca yeni, ancak var olan tüm etki alanları, konular ve olay abonelikleri için değil, meta verilere yönelik otomatik coğrafi bir olağanüstü durum kurtarma (GeoDR) vardır. Bir Azure bölgesinin tamamı kapalıysa Event Grid, tüm olayla ilgili altyapı meta verilerlerinizin eşleştirilmiş bir bölgeyle eşitlenmesi zaten olur. Yeni olaylarınız, sizin tarafınızdan araya girmeden yeniden akışa sunulacaktır. 

Olağanüstü durum kurtarma iki ölçüm ile ölçülür:

- Kurtarma noktası hedefi (RPO): kaybolmuş olabilecek verilerin dakikası veya saatleri.
- Kurtarma süresi hedefi (RTO): hizmetin dakika veya saat sayısı.

Event Grid otomatik yük devretmede, meta verileriniz (olay abonelikleri vb.) ve verileriniz (olaylar) için farklı RPOs ve RTOs vardır. Aşağıdakilerden farklı belirtilere ihtiyacınız varsa, [Konu durumu API 'lerini kullanarak kendi istemci tarafı yük devrelerinizi](custom-disaster-recovery.md)de uygulayabilirsiniz.

## <a name="recovery-point-objective-rpo"></a>Kurtarma noktası hedefi (RPO)
- **Meta veri RPO 'su**: sıfır dakika. Event Grid her zaman bir kaynak oluşturulduğunda, bölgeler arasında anında çoğaltılır. Yük devretme gerçekleştiğinde, hiçbir meta veri kaybolmaz.
- **Veri RPO 'su**: sistem sağlıklı ve bölgesel yük devretme sırasında mevcut trafiğe girerse, olaylar için RPO yaklaşık 5 dakikadır.

## <a name="recovery-time-objective-rto"></a>Kurtarma süresi hedefi (RTO)
- **Meta veri RTO**: genellikle 60 dakika içinde çok daha hızlı bir şekilde gerçekleşse de Event Grid konular ve abonelikler için oluşturma/güncelleştirme/silme çağrılarını kabul edecek şekilde başlayacaktır.
- **Veri RTO**: meta veriler gibi genellikle çok daha hızlı bir şekilde gerçekleşir, ancak 60 dakika içinde Event Grid, bölgesel bir yük devretmeden sonra yeni trafiği kabul etmeye başlayacaktır.

> [!NOTE]
> Event Grid meta veri GeoDR maliyeti: $0.


## <a name="next-steps"></a>Sonraki adımlar
İstemci tarafı yük devretme mantığınızı uygulamak istiyorsanız, bkz. [Event Grid özel konular için kendi olağanüstü durum kurtarmayı oluşturma](custom-disaster-recovery.md)
