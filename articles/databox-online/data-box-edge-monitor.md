---
title: Azure Veri Kutusu Kenar ı aygıtınızı izleyin | Microsoft Dokümanlar
description: Azure Veri Kutusu Kenarınızı izlemek için Azure portalının ve yerel web Kullanıcı Arabirimi'nin nasıl kullanılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 188f5c6cfbb4650ad1ff767955d064f8e0c3cb70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60756751"
---
# <a name="monitor-your-azure-data-box-edge"></a>Azure Veri Kutusu Kenarınızı İzleyin

Bu makalede, Azure Veri Kutusu Kenarınızın nasıl izlendiği açıklanmaktadır. Cihazınızı izlemek için Azure portalını veya yerel web kullanıcı arabirimi'ni kullanabilirsiniz. Aygıt olaylarını görüntülemek, uyarıları yapılandırmak ve yönetmek ve ölçümleri görüntülemek için Azure portalını kullanın. Çeşitli aygıt bileşenlerinin donanım durumunu görüntülemek için fiziksel aygıtınızdaki yerel web ui'sini kullanın.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Aygıt olaylarını ve ilgili uyarıları görüntüleme
> * Aygıt bileşenlerinin donanım durumunu görüntüleme
> * Cihazınızın kapasite ve işlem ölçümlerini görüntüleme
> * Uyarıları yapılandırma ve yönetme

## <a name="view-device-events"></a>Cihaz olaylarını görüntüleme

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-device-events.md)]

## <a name="view-hardware-status"></a>Donanım durumunu görüntüleme

Aygıt bileşenlerinizin donanım durumunu görüntülemek için yerel web UI'de aşağıdaki adımları izleyin.

1. Cihazınızın yerel web ui'sine bağlanın.
2. Bakım **> Donanım durumuna**gidin. Çeşitli aygıt bileşenlerinin durumunu görüntüleyebilirsiniz.

    ![Donanım durumunu görüntüleme](media/data-box-edge-monitor/view-hardware-status.png)

## <a name="view-metrics"></a>Ölçümleri görüntüle

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-metrics.md)]

## <a name="manage-alerts"></a>Uyarıları yönetme

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-manage-alerts.md)]

## <a name="next-steps"></a>Sonraki adımlar 

[Bant genişliğini yönetmeyi](data-box-edge-manage-bandwidth-schedules.md) öğrenin.