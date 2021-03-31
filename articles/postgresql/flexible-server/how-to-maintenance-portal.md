---
title: PostgreSQL için Azure veritabanı-esnek sunucu (Önizleme)-zamanlanmış bakım-Azure portal
description: Azure portal bir PostgreSQL için Azure veritabanı-esnek sunucu için zamanlanmış bakım ayarlarını yapılandırmayı öğrenin.
author: niklarin
ms.author: nlarin
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: be6040b8b84a4b86746d62bd2f1c07f0ffea0a3b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "91336301"
---
# <a name="manage-scheduled-maintenance-settings-for-azure-database-for-postgresql--flexible-server"></a>PostgreSQL için Azure veritabanı için zamanlanmış bakım ayarlarını yönetme – esnek sunucu
 
Azure aboneliğinizdeki her esnek sunucu için bakım seçeneklerini belirtebilirsiniz. Seçenekler, yaklaşan ve tamamlanmış bakım olaylarının bakım zamanlamasını ve bildirim ayarlarını içerir.

> [!IMPORTANT]
> PostgreSQL için Azure veritabanı-esnek sunucu önizlemededir.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [PostgreSQL Için Azure veritabanı-esnek sunucu](quickstart-create-server-portal.md)
 
## <a name="specify-maintenance-schedule-options"></a>Bakım zamanlaması seçeneklerini belirtin
 
1. PostgreSQL sunucusu sayfasında, **Ayarlar** başlığı altında **bakım** ' ı seçerek zamanlanmış bakım seçeneklerini açın.
2. Varsayılan (sistem tarafından yönetilen) zamanlama, haftanın rastgele bir günüdür ve 11 PM ile 7 dakikalık yerel sunucu saati arasında bakım başlangıcı için 60 dakikalık bir pencere olur. Bu zamanlamayı özelleştirmek istiyorsanız **özel zamanlama**' yı seçin. Daha sonra haftanın tercih edilen gününü ve bakım başlangıç zamanı için 60 dakikalık bir pencere seçebilirsiniz.
 
## <a name="notifications-about-scheduled-maintenance-events"></a>Zamanlanmış bakım olayları hakkında bildirimler
 
Azure hizmet durumu ' nu kullanarak, esnek sunucunuzda yaklaşan ve gerçekleştirilen zamanlanmış bakımla ilgili [bildirimleri görüntüleyebilirsiniz](../../service-health/service-notifications.md) . Ayrıca, bakım olayları hakkında bildirim almak için Azure hizmet durumu 'nda uyarılar [ayarlayabilirsiniz](../../service-health/resource-health-alert-monitor-guide.md) .
 
## <a name="next-steps"></a>Sonraki adımlar  
 
* [PostgreSQL Için Azure veritabanı – esnek sunucu 'da zamanlanmış bakım](concepts-maintenance.md) hakkında bilgi edinin
* [Azure hizmet durumu](../../service-health/overview.md) hakkında yalın
