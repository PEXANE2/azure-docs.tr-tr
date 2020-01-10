---
title: Azure portalını kullanarak hizmet durumu bildirimlerini görüntüleme
description: Hizmet durumu bildirimleri Microsoft Azure tarafından yayınlanan hizmet durumu iletilerini görüntülemenize izin verir.
ms.topic: conceptual
ms.date: 6/27/2019
ms.openlocfilehash: 21416edc95d345eb183030a9b0cccb7529305d6d
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75748645"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Azure portalını kullanarak hizmet durumu bildirimlerini görüntüleme

Hizmet durumu bildirimleri Azure altyapı tarafından [Azure etkinlik günlüğüne](../azure-monitor/platform/platform-logs-overview.md)yayımlanır.  Bildirimler, aboneliğinizdeki kaynaklarla ilgili bilgiler içerir. Etkinlik günlüğünde depolanan büyük miktarda bilgi hacmi verildiğinde, hizmet durumu bildirimlerinde uyarıları görüntülemeyi ve ayarlamayı kolaylaştıran ayrı bir kullanıcı arabirimi vardır. 

Hizmet durumu bildirimleri, sınıfa bağlı olarak bilgilendirici veya işlem yapılabilir olabilir.

Çeşitli hizmet sistem durumu bildirimleri hakkında daha fazla bilgi için bkz. [hizmet durumu bildirimleri özellikleri](service-health-notifications-properties.md).

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Hizmet sistem durumu bildirimlerinizi Azure portal görüntüleyin

1. [Azure Portal](https://portal.azure.com), **İzle**' yi seçin.

    ![Izleyici seçiliyken Azure portal menüsünün ekran görüntüsü](./media/service-notifications/home-monitor.png)

    Azure Izleyici, tüm izleme ayarlarınızı ve verilerinizi tek bir birleştirilmiş görünümde birlikte getirir. İlk olarak **Etkinlik günlüğü** bölümü açılır.

1. **Uyarıları**seçin.

    ![Uyarı seçiliyken Izleme etkinlik günlüğü ekran görüntüsü](./media/service-notifications/service-health-summary.png)

1. **+ Etkinlik günlüğü uyarısı Ekle**' yi seçin ve gelecekteki hizmet bildirimleri için bildirim aldığınızdan emin olmak için bir uyarı ayarlayın. Daha fazla bilgi için bkz. [hizmet bildirimlerinde etkinlik günlüğü uyarıları oluşturma](../azure-monitor/platform/alerts-activity-log-service-notifications.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Etkinlik günlüğü uyarıları](../azure-monitor/platform/activity-log-alerts.md)hakkında daha fazla bilgi edinin.
