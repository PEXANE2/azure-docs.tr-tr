---
title: Azure Percept DK 'nin RTSP video akışını görüntüleyin
description: Azure Percept DK 'den RTSP video akışını görüntülemeyi öğrenin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/12/2021
ms.custom: template-how-to
ms.openlocfilehash: 5f77e99dc5c34867fef2b0ac47c709824fa4477d
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102096012"
---
# <a name="view-your-azure-percept-dks-rtsp-video-stream"></a>Azure Percept DK 'nin RTSP video akışını görüntüleyin

Azure Percept Studio içindeki Azure Percept DK 'den RTSP video akışını görüntülemek için bu kılavuzu izleyin. Cihazınıza dağıtılan Vision AI modellerinden ikinci dereceden, Web akışında görünecektir.

## <a name="prerequisites"></a>Önkoşullar

- Azure Percept DK (devkit)
- [Azure aboneliği](https://azure.microsoft.com/free/)
- [Azure PERCEPT dk kurulum deneyimi](./quickstart-percept-dk-set-up.md): devkit 'i bir Wi-Fi ağına bağladınız, bir IoT Hub oluşturdunuz ve devkit 'e IoT Hub

## <a name="view-the-rtsp-video-stream"></a>RTSP video akışını görüntüleme

1. Devkit üzerinde güç.

1. [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819)'ya gidin.

1. Genel Bakış sayfasının sol tarafında, **cihazlar**' a tıklayın.

    :::image type="content" source="./media/how-to-view-video-stream/overview-devices-inline.png" alt-text="Azure Percept Studio 'ya genel bakış ekranı." lightbox="./media/how-to-view-video-stream/overview-devices.png":::

1. Listeden devkit ' i seçin.

    :::image type="content" source="./media/how-to-view-video-stream/select-device.png" alt-text="Azure Percept Studio 'daki kullanılabilir cihazların ekran görüntüsü.":::

1. **Cihaz akışınızı görüntüle**' ye tıklayın.

    :::image type="content" source="./media/how-to-view-video-stream/view-device-stream.png" alt-text="Kullanılabilir Vizyon Projesi eylemlerini gösteren cihaz sayfasının ekran görüntüsü.":::

    Bu, Azure Percept DK 'nizden Canlı Web akışını gösteren ayrı bir sekme açar.

    :::image type="content" source="./media/how-to-view-video-stream/webstream.png" alt-text="Cihaz Web akışının ekran görüntüsü.":::

## <a name="next-steps"></a>Sonraki adımlar

[Azure PERCEPT dk telemetrinizi](./how-to-view-telemetry.md)görüntülemeyi öğrenin.