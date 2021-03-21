---
title: Azure Percept DK 'nin model çıkarım telemetrinizi görüntüleme
description: Azure IoT Explorer 'da Azure Percept DK 'nin Vision model çıkarımı telemetrinizi görüntülemeyi öğrenin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/17/2021
ms.custom: template-how-to
ms.openlocfilehash: 07abbc5f5e85c75b73774d11b6b81dd2085735b7
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102095332"
---
# <a name="view-your-azure-percept-dks-model-inference-telemetry"></a>Azure Percept DK 'nin model çıkarım telemetrinizi görüntüleme

Azure [IoT Explorer](https://github.com/Azure/azure-iot-explorer/releases)'Da Azure Percept dk 'nin Vision model çıkarımı telemetrinizi görüntülemek için bu kılavuzu izleyin.

## <a name="prerequisites"></a>Önkoşullar

- Azure Percept DK (devkit)
- [Azure aboneliği](https://azure.microsoft.com/free/)
- [Azure PERCEPT dk kurulum deneyimi](./quickstart-percept-dk-set-up.md): devkit 'i bir Wi-Fi ağına bağladınız, bir IoT Hub oluşturdunuz ve devkit 'e IoT Hub
- [Vision AI modeli Azure Percept DK 'nize dağıtıldı](./how-to-deploy-model.md)

## <a name="view-telemetry"></a>Telemetri görüntüleme

1. Devkit üzerinde güç.

1. [Azure IoT Gezginini](https://github.com/Azure/azure-iot-explorer/releases)indirin ve yükleyin. Bir Windows kullanıcısı kullanıyorsanız,. msi dosyasını seçin.

    :::image type="content" source="./media/how-to-view-telemetry/azure-iot-explorer-download.png" alt-text="Azure IoT Explorer için indirme ekranı.":::

1. IoT Hub Azure IoT Gezgini 'ne bağlayın:

    1. [Azure Portal](https://portal.azure.com) gidin.

    1. **Tüm kaynaklar**’ı seçin.

        :::image type="content" source="./media/how-to-view-telemetry/azure-portal.png" alt-text="Azure portal giriş sayfası.":::

    1. Azure Percept DK 'nin bağlı olduğu IoT Hub seçin.

        :::image type="content" source="./media/how-to-view-telemetry/iot-hub.png" alt-text="Azure portal liste IoT Hub.":::

    1. IoT Hub sayfanızın sol tarafında, **paylaşılan erişim ilkeleri**' ni seçin.

        :::image type="content" source="./media/how-to-view-telemetry/shared-access-policies.png" alt-text="Paylaşılan erişim ilkelerini gösteren IoT Hub sayfası.":::

    1. **İothubowner** öğesine tıklayın.

        :::image type="content" source="./media/how-to-view-telemetry/iothubowner.png" alt-text="İothubowner vurgulanmış paylaşılan erişim ilkeleri ekranı.":::

    1. **Bağlantı dizesi-birincil anahtar**' ın yanındaki mavi kopya simgesine tıklayın.

        :::image type="content" source="./media/how-to-view-telemetry/connection-string.png" alt-text="bağlantı dizesi kopyalama düğmesi vurgulanmış şekilde ıothubowner penceresi.":::

    1. Azure IoT Gezginini açın ve **+ bağlantı ekle**' ye tıklayın.

    1. Bağlantı dizesini bağlantı dizesi **Ekle** penceresinde **bağlantı dizesi** kutusuna yapıştırın ve **Kaydet**' e tıklayın.

        :::image type="content" source="./media/how-to-view-telemetry/add-connection-string.png" alt-text="Bağlantı dizesini içine yapıştırmaya yönelik Box ile Azure IoT Gezgin penceresi.":::

    1. Model oluşturma için bir nesneye ilişkin bir nesne ile ilgili bir bakış noktası getirin.

    1. **Telemetriyi** seçin.

    1. Cihazdan telemetri olaylarını görüntülemek için **Başlat** ' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
[Azure PERCEPT dk video](./how-to-view-video-stream.md)akışınızı görüntülemeyi öğrenin.