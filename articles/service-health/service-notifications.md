---
title: Azure portalını kullanarak hizmet durumu bildirimlerini görüntüleme
description: Hizmet durumu bildirimleri, Microsoft Azure tarafından yayınlanan hizmet durumu iletilerini görüntülemenize olanak tanır.
ms.topic: conceptual
ms.date: 6/27/2019
ms.openlocfilehash: 21416edc95d345eb183030a9b0cccb7529305d6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75748645"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Azure portalını kullanarak hizmet durumu bildirimlerini görüntüleme

Hizmet durumu bildirimleri Azure altyapısı tarafından [Azure etkinlik günlüğüne](../azure-monitor/platform/platform-logs-overview.md)yayınlanır.  Bildirimler, aboneliğiniz altındaki kaynaklar hakkında bilgi içerir. Etkinlik günlüğünde depolanan büyük miktardabilgi göz önüne alındığında, hizmet durumu bildirimlerinde uyarıların görüntülenmesini ve ayarlanın kolaylaştırılabilmek için ayrı bir kullanıcı arabirimi vardır. 

Hizmet durumu bildirimleri, sınıfa bağlı olarak bilgilendirilebilir veya işlem yapılabilir.

Çeşitli hizmet durumu bildirimleri sınıfları hakkında daha fazla bilgi için [Hizmet sistem durumu bildirimleri özelliklerine](service-health-notifications-properties.md)bakın.

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Azure portalında hizmet durumu bildirimlerinizi görüntüleyin

1. Azure [portalında,](https://portal.azure.com) **Monitör'ü**seçin.

    ![Azure portal menüsünün ekran görüntüsü, Monitör seçili](./media/service-notifications/home-monitor.png)

    Azure Monitor, tüm izleme ayarlarınızı ve verilerinizi tek birleştirilmiş görünümde bir araya getirir. İlk olarak **Etkinlik günlüğü** bölümü açılır.

1. **Uyarıları**seçin.

    ![Uyarılar seçili olan Monitör Etkinliği günlüğünün ekran görüntüsü](./media/service-notifications/service-health-summary.png)

1. **+Etkinlik günlüğü uyarısı ekle'yi**seçin ve gelecekteki hizmet bildirimleri için bilgilendirilmenizi sağlamak için bir uyarı ayarlayın. Daha fazla bilgi için [bkz.](../azure-monitor/platform/alerts-activity-log-service-notifications.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Etkinlik günlüğü uyarıları](../azure-monitor/platform/activity-log-alerts.md)hakkında daha fazla bilgi edinin.
