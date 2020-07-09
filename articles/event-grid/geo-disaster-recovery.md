---
title: Azure Event Grid coğrafi olağanüstü durum kurtarma | Microsoft Docs
description: Azure Event Grid coğrafi olağanüstü durum kurtarmayı (GeoDR) otomatik olarak nasıl desteklediğini açıklar.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: ccb16971020a65932daa8f9adf4b7cd9008a9253
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86105855"
---
# <a name="server-side-geo-disaster-recovery-in-azure-event-grid"></a>Azure Event Grid 'de sunucu tarafı coğrafi olağanüstü durum kurtarma
Event Grid artık yalnızca yeni, ancak var olan tüm etki alanları, konular ve olay abonelikleri için değil, meta verilere yönelik otomatik coğrafi bir olağanüstü durum kurtarma (GeoDR) vardır. Bir Azure bölgesinin tamamı kapalıysa Event Grid, tüm olayla ilgili altyapı meta verilerlerinizin eşleştirilmiş bir bölgeyle eşitlenmesi zaten olur. Yeni olaylarınız, sizin tarafınızdan araya girmeden yeniden akışa sunulacaktır. 

Olağanüstü durum kurtarma iki ölçüm ile ölçülür:

- [Kurtarma noktası hedefi (RPO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_Point_Objective): kaybolmuş olabilecek verilerin dakikası veya saatleri.
- [Kurtarma süresi hedefi (RTO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_time_objective): hizmetin dakika cinsinden bir süre olması olabilir.

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
