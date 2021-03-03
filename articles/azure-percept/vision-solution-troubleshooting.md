---
title: Azure Percept Vision ve Vision modülleriyle ilgili sorunları giderme
description: Vision AI prototipleme deneyimlerinde bulunan daha yaygın sorunlardan bazıları için sorun giderme ipuçları alın
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: e7351079e7325aa7750dc0d10f0923bc847ccc3c
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101664037"
---
# <a name="vision-solution-troubleshooting"></a>Vision çözüm sorunlarını giderme

Azure Percept Studio 'da kod içermeyen Vision çözümlerinin sorunlarını giderme hakkında bilgi için aşağıdaki kılavuza bakın.

## <a name="delete-a-vision-project"></a>Bir Vision projesini silme

1. https://www.customvision.ai/projects öğesine gidin.

1. Silmek istediğiniz projenin üzerine gelin ve projeyi silmek için çöp kutusu simgesine tıklayın.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-delete-project.png" alt-text="Silme simgesi vurgulanmış Özel Görüntü İşleme Projeler sayfası.":::

## <a name="check-which-modules-are-on-a-device"></a>Bir cihazda hangi modüllerin olduğunu denetleme

1. [Azure Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home) gidin.

1. **IoT Hub** simgesine tıklayın.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-iot-hub-2-inline.png" alt-text="IoT Hub simgesi vurgulanmış şekilde Azure portal giriş sayfası." lightbox= "./media/vision-solution-troubleshooting/vision-iot-hub-2.png":::

1. Hedef cihazınızın bağlı olduğu IoT Hub seçin.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-iot-hub.png" alt-text="IoT Hub 'Ları listesi.":::

1. **IoT Edge** ' yi seçin ve cihaz **kimliği** sekmesinin altındaki cihazınıza tıklayın.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-iot-edge.png" alt-text="IoT Edge giriş sayfası.":::

1. Cihaz modülleriniz **modüller** sekmesinin altında listelenecektir.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-device-modules-inline.png" alt-text="Modüller sekmesi içeriğini gösteren seçili cihaz için IoT Edge sayfası." lightbox= "./media/vision-solution-troubleshooting/vision-device-modules.png":::

## <a name="delete-a-device"></a>Cihazı silme

1. [Azure Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home) gidin.

1. **IoT Hub** simgesine tıklayın.

1. Hedef cihazınızın bağlı olduğu IoT Hub seçin.

1. **IoT Edge** ' yi seçin ve hedef cihaz kimliğinizin yanındaki kutuyu işaretleyin. Cihazınızı silmek için çöp kutusu simgesine tıklayın.

    :::image type="content" source="./media/vision-solution-troubleshooting/vision-delete-device.png" alt-text="IoT Edge giriş sayfasında vurgulanan simgeyi Sil.":::

## <a name="eye-module-troubleshooting-tips"></a>Göz modülü sorun giderme ipuçları

**Webstreammodule** ile ilgili bir sorun varsa, Vision modelinin **ınlada bir şekilde çalıştığı azureeyemodule**'un çalıştığından emin olun. Çalışma zamanı durumunu denetlemek için [Azure Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home) gidin ve **tüm kaynaklar**  ->  **\<your IoT hub>**  ->  **IoT Edge** gidin  ->  **\<your device ID>** . Tüm yüklü modüllerin çalışma zamanı durumunu görmek için **modüller** sekmesine tıklayın.

:::image type="content" source="./media/vision-solution-troubleshooting/over-the-air-iot-edge-device-page-inline.png" alt-text="Cihaz modülü çalışma zamanı durumu ekranı." lightbox= "./media/vision-solution-troubleshooting/over-the-air-iot-edge-device-page.png":::

**Azureeyemodule** çalışma zamanı durumu **çalışıyor** olarak listelenmiyorsa, **modülleri ayarla**  ->  **azureeyemodule**' e tıklayın. **Modül ayarları** sayfasında, **istenen durumu** **çalışıyor** olarak ayarlayın ve **Güncelleştir**' e tıklayın.

 :::image type="content" source="./media/vision-solution-troubleshooting/firmware-desired-status-stopped.png" alt-text="Modül ayarı yapılandırma ekranı.":::

## <a name="view-device-rtsp-video-stream"></a>Cihaz RTSP video akışını görüntüle

Cihazınızın RTSP video akışını [Azure Percept Studio](./how-to-view-video-stream.md) veya [VLC medya yürütücüsü](https://www.videolan.org/vlc/index.html)' nde görüntüleyin.

VLC Media Player 'da RTSP akışını açmak için **medya**  ->  **açık ağ akışı**  ->  **RTSP://[aygıt IP adresi]/Sonuç** sayfasına gidin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Percept DK sorunlarını giderme hakkında daha fazla bilgi için bkz. [genel sorun giderme kılavuzu](./troubleshoot-dev-kit.md) .