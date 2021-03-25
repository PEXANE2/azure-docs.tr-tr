---
title: Zamanlanan Bakım-PostgreSQL için Azure veritabanı-hiper ölçek (Citus)
description: Bu makalede PostgreSQL için Azure veritabanı-hiper ölçek (Citus) içinde zamanlanmış bakım özelliği açıklanır.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/22/2021
ms.openlocfilehash: 0d11ec8815cc0f5082f95c5331321f043e86eece
ms.sourcegitcommit: a8ff4f9f69332eef9c75093fd56a9aae2fe65122
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105027672"
---
# <a name="scheduled-maintenance-in-azure-database-for-postgresql--hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda zamanlanmış bakım – hiper ölçek (Citus)

PostgreSQL için Azure veritabanı-Hyperscale (Citus), yönetilen veritabanınızı güvenli, kararlı ve güncel tutmak için düzenli aralıklarla bakım yapar.  Bakım sırasında, sunucu grubundaki tüm düğümler yeni özellikler, güncelleştirmeler ve düzeltme ekleri alır.

Hiper ölçek için zamanlanmış bakımın (Citus) temel özellikleri şunlardır:

* Güncelleştirmeler sunucu grubundaki tüm düğümlerde aynı anda uygulanır
* Yaklaşan bakımla ilgili bildirimler, Azure hizmet durumu 'na beş gün önceden gönderilir
* Genellikle, bir sunucu grubu için başarılı bakım olayları arasında en az 30 gün vardır

## <a name="notification-about-upcoming-maintenance"></a>Yaklaşan bakım hakkında bildirim

Yaklaşan zamanlanmış bakımla ilgili bildirimler Azure hizmet durumu 'na gönderilir ve şu olabilir:

* Belirli bir adrese e-postayla
* Azure Resource Manager rolüne e-postayla
* Mobil cihazlara kısa mesajda (SMS) gönderilir
* Azure uygulamasına bildirim olarak gönderilerek
* Sesli mesaj olarak teslim edilerek

> [!IMPORTANT]
> Normal olarak, bir sunucu grubu için başarılı zamanlanmış bakım olayları arasında en az 30 gün vardır.
>
> Ancak ciddi bir güvenlik açığı gibi kritik öneme sahip acil bir güncelleştirme söz konusu olduğunda beş günden daha kısa bir süre öncesinde bildirim yapılabilir. Son 30 gün içinde başarılı bir zamanlanmış bakım gerçekleştirilmiş olsa dahi sunucunuza kritik güncelleştirme uygulanabilir.

Bakım başarısız olursa veya iptal edilirse sistem bir bildirim oluşturacaktır.
Geçerli zamanlama ayarlarına göre bakım yapmayı yeniden dener ve sonraki bakım olayından beş gün önce bildirim gönderir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure hizmet durumunu kullanarak [gelecek bakımla ilgili bildirim alma](../service-health/service-notifications.md) hakkında bilgi edinin
* [Yaklaşan zamanlanmış bakım olayları hakkında uyarı ayarlamayı](../service-health/resource-health-alert-monitor-guide.md) öğrenin
