---
title: Azure Data Box Edge cihazınızı izleme | Microsoft Docs
description: Azure Data Box Edge izlemek için Azure portal ve yerel Web Kullanıcı arabiriminin nasıl kullanılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 188f5c6cfbb4650ad1ff767955d064f8e0c3cb70
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "60756751"
---
# <a name="monitor-your-azure-data-box-edge"></a>Azure Data Box Edge izleyin

Bu makalede Azure Data Box Edge nasıl izleneceği açıklanır. Cihazınızı izlemek için Azure portal veya yerel Web Kullanıcı arabirimi ' ni kullanabilirsiniz. Cihaz olaylarını görüntülemek, uyarıları yapılandırmak ve yönetmek ve ölçümleri görüntülemek için Azure portal kullanın. Çeşitli cihaz bileşenlerinin donanım durumunu görüntülemek için fiziksel cihazınızda yerel Web Kullanıcı arabirimini kullanın.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Cihaz olaylarını ve ilgili uyarıları görüntüleme
> * Cihaz bileşenlerinin donanım durumunu görüntüleme
> * Cihazınız için kapasite ve işlem ölçümlerini görüntüleme
> * Uyarıları yapılandırma ve yönetme

## <a name="view-device-events"></a>Cihaz olaylarını görüntüle

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-device-events.md)]

## <a name="view-hardware-status"></a>Donanım durumunu görüntüleme

Cihaz bileşenlerinizin donanım durumunu görüntülemek için yerel Web Kullanıcı arabiriminde aşağıdaki adımları uygulayın.

1. Cihazınızın yerel Web Kullanıcı arabirimine bağlanın.
2. **Bakım > donanım durumu**' na gidin. Çeşitli cihaz bileşenlerinin sistem durumunu görüntüleyebilirsiniz.

    ![Donanım durumunu görüntüleme](media/data-box-edge-monitor/view-hardware-status.png)

## <a name="view-metrics"></a>Ölçümleri görüntüle

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-view-metrics.md)]

## <a name="manage-alerts"></a>Uyarıları yönetme

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-manage-alerts.md)]

## <a name="next-steps"></a>Sonraki adımlar 

[Bant genişliğini yönetmeyi](data-box-edge-manage-bandwidth-schedules.md) öğrenin.