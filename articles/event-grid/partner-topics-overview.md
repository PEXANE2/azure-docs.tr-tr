---
title: Azure Event Grid iş ortağı konuları
description: Üçüncü taraf Event Grid SaaS ve PaaS iş ortaklarından olayları, Azure Event Grid Azure hizmetlerine doğrudan gönderin.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/18/2020
ms.author: babanisa
ms.openlocfilehash: f47d63ce79846e94e992df93af1768aad3c17e67
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170962"
---
# <a name="partner-topics-in-azure-event-grid-preview"></a>Azure Event Grid iş ortağı konuları (Önizleme)
İş ortağı konularını kullanarak, üçüncü taraf olay kaynaklarını doğrudan Azure Event Grid bağlayabilirsiniz. Bu tümleştirme, Azure hizmetlerinden olaylara abone olduğunuz şekilde iş ortaklarından olaylara abone olmanızı sağlar. 

## <a name="available-partners"></a>Kullanılabilir iş ortakları
Event Grid iş ortağı konuları aracılığıyla kullanılabilen ilk iş ortağı Auth0. Auth0 ve Azure hesaplarınızı bağlamak için [Auth0 iş ortağı konusunu](auth0-overview.md) kullanabilirsiniz. Tümleştirme, Auth0 olaylarını gerçek zamanlı olarak tepki vermenize, günlüğe kaydetmenize ve izlemenize olanak tanır.

[Denemek Için](auth0-how-to.md)Auth0 hesabınızda oturum açın ve bir Event Grid tümleştirmesi oluşturun. Auth0 içinde **Oluştur** ' u seçtikten sonra, Azure hesabınızda bekleyen bir Auth0 konusu görürsünüz. **Etkinleştir**' i seçin ve diğer olay kaynakları gibi olayları yönlendirmek, filtrelemek ve sunmak için Event Grid abonelikler oluşturabilirsiniz.

## <a name="pricing"></a>Fiyatlandırma
İş ortağı konuları, sistem konuları ile aynı işlem fiyatı üzerinden ücretlendirilir.

## <a name="limits"></a>Sınırlar
İş ortağı konuları genel önizlemededir. Genel Önizleme sırasında, iş ortağı konuları sistem konuları ve özel konular ile [aynı sınırlara](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#event-grid-limits) tabidir.

## <a name="how-do-i-become-an-event-grid-partner"></a>Nasıl yaparım? Event Grid iş ortağı olacak mı?
Bu başlatmayı desteklemek için oluşturulan altyapı, yeni iş ortaklarının olay yeteneklerini Event Grid ile tümleştirmelerini kolaylaştırır ve hızlı hale getirir. Daha fazla bilgi için bkz. [iş ortağı ekleme belgeleri](partner-onboarding-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Auth0 iş ortağı konusu](auth0-overview.md)
- [Auth0 iş ortağı konusunu kullanma](auth0-how-to.md)
- [Event Grid iş ortağı olun](partner-onboarding-overview.md)