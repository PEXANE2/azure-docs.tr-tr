---
title: Azure Percept DK 'nin RTSP video akışını görüntüleyin
description: Azure Percept DK 'nin Vision SoM 'dan RTSP video akışını görüntülemeyi öğrenin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/12/2021
ms.custom: template-how-to
ms.openlocfilehash: 20fb8495e17d4294351a50c3bc97436de9f03450
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101663845"
---
# <a name="view-your-azure-percept-dks-rtsp-video-stream"></a>Azure Percept DK 'nin RTSP video akışını görüntüleyin

Azure Percept Studio içindeki Azure Percept DK 'nin Vision SoM 'dan alınan RTSP video akışını görüntülemek için bu kılavuzu izleyin. Cihazınıza dağıtılan Vision AI modellerinden ınırm, webstream 'de görüntülenebilir.

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

    :::image type="content" source="./media/how-to-view-video-stream/select-device.png" alt-text="Azure Percept Studio 'ya genel bakış ekranı.":::

1. **Cihaz akışınızı görüntüle**' ye tıklayın.

    :::image type="content" source="./media/how-to-view-video-stream/view-device-stream.png" alt-text="Azure Percept Studio 'ya genel bakış ekranı.":::

    Bu, Azure Percept DK 'nin Vision SoM 'ınızdan Canlı Web akışını gösteren ayrı bir sekme açar.

    :::image type="content" source="./media/how-to-view-video-stream/webstream.png" alt-text="Azure Percept Studio 'ya genel bakış ekranı.":::

## <a name="next-steps"></a>Sonraki adımlar

[Azure PERCEPT dk telemetrinizi](./how-to-view-telemetry.md)görüntülemeyi öğrenin.