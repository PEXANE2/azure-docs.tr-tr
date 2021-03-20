---
title: MySQL için Azure veritabanı-esnek sunucu (Önizleme)-zamanlanmış bakım-Azure portal
description: Azure portal bir MySQL için Azure veritabanı için zamanlanmış bakım ayarlarını yapılandırma hakkında bilgi edinin.
author: niklarin
ms.author: nlarin
ms.service: mysql
ms.topic: how-to
ms.date: 9/21/2020
ms.openlocfilehash: c8251eb2a89a7481ebc981f2b89668c363651b39
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91315025"
---
# <a name="manage-scheduled-maintenance-settings-for-azure-database-for-mysql--flexible-server"></a>MySQL için Azure Veritabanı – Esnek Sunucuda zamanlanmış bakım ayarlarını yönetme
 
Azure aboneliğinizdeki her esnek sunucu için bakım seçeneklerini belirtebilirsiniz. Seçenekler, yaklaşan ve tamamlanmış bakım olaylarının bakım zamanlamasını ve bildirim ayarlarını içerir.

> [!IMPORTANT]
> MySQL için Azure veritabanı-esnek sunucu önizlemededir.
 
## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [MySQL Için Azure veritabanı-esnek sunucu](quickstart-create-server-portal.md)
 
## <a name="specify-maintenance-schedule-options"></a>Bakım zamanlaması seçeneklerini belirtin
 
1. MySQL sunucusu sayfasında, **Ayarlar** başlığı altında **bakım** ' ı seçerek zamanlanmış bakım seçeneklerini açın.
2. Varsayılan (sistem tarafından yönetilen) zamanlama, haftanın rastgele bir günüdür ve 11 PM ile 7 dakikalık yerel sunucu saati arasında bakım başlangıcı için 60 dakikalık bir pencere olur. Bu zamanlamayı özelleştirmek istiyorsanız **özel zamanlama**' yı seçin. Daha sonra haftanın tercih edilen gününü ve bakım başlangıç zamanı için 60 dakikalık bir pencere seçebilirsiniz.
 
## <a name="notifications-about-scheduled-maintenance-events"></a>Zamanlanmış bakım olayları hakkında bildirimler
 
Azure hizmet durumu ' nu kullanarak, esnek sunucunuzda yaklaşan ve gerçekleştirilen zamanlanmış bakımla ilgili [bildirimleri görüntüleyebilirsiniz](../../service-health/service-notifications.md) . Ayrıca, bakım olayları hakkında bildirim almak için Azure hizmet durumu 'nda uyarılar [ayarlayabilirsiniz](../../service-health/resource-health-alert-monitor-guide.md) .
 
## <a name="next-steps"></a>Sonraki adımlar  
 
* [MySQL Için Azure veritabanı – esnek sunucu 'da zamanlanmış bakım](concepts-maintenance.md) hakkında bilgi edinin
* [Azure hizmet durumu](../../service-health/overview.md) hakkında yalın
