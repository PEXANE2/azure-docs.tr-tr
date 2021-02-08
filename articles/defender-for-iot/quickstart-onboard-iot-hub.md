---
title: IoT Aracısı tabanlı çözüm için Defender 'a ekleme
description: Azure IoT Hub IoT güvenlik hizmeti için Defender 'ı ekleme ve etkinleştirme hakkında bilgi edinin.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/20/2021
ms.author: shhazam
ms.openlocfilehash: 127e439a7740cb97cbe126071aaaa5245cd85782
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809142"
---
# <a name="onboard-to-defender-for-iot-agent-based-solution"></a>IoT Aracısı tabanlı çözüm için Defender 'a ekleme

Bu makalede, mevcut IoT Hub IoT hizmeti için Defender 'ın nasıl etkinleştirileceği açıklanır. Şu anda bir IoT Hub yoksa, kullanmaya başlamak için [Azure Portal kullanarak IoT Hub oluşturma](../iot-hub/iot-hub-create-through-portal.md) makalesine bakın.

IoT için Defender 'daki IoT Hub IoT güvenlerinizi yönetebilirsiniz. IoT Hub bulunan yönetim portalı aşağıdakileri yapmanıza olanak sağlar: 

- IoT Hub güvenliğini yönetin.

- IoT Hub telemetri temelinde bir aracı yüklemeden IoT cihazının güvenliğinin temel yönetimi. 

- Mikro aracıya dayalı bir IoT cihazının güvenliği için gelişmiş yönetim.

> [!NOTE]
> IoT için Defender Şu anda yalnızca Standart katman IoT Hub 'Larını desteklemektedir.

## <a name="onboard-to-defender-for-iot-in-iot-hub"></a>IoT Hub 'da IoT için Defender 'a ekleme

Tüm yeni IoT Hub 'ları için, IoT için Defender varsayılan olarak **Açık** olarak ayarlanmıştır. IoT Hub oluşturma işlemi sırasında IoT için Defender 'ın **Açık** olduğunu doğrulayabilirsiniz.

Değiştirme seçeneğinin **Açık** olarak ayarlandığını doğrulamak için:

1. Azure portalına gidin.

1. Azure hizmetleri listesinden **IoT Hub** seçin.

1. **Oluştur**’u seçin.

    :::image type="content" source="media/quickstart-onboard-iot-hub/create-iot-hub.png" alt-text="Üstteki araç çubuğundan Oluştur düğmesini seçin." lightbox="media/quickstart-onboard-iot-hub/create-iot-hub-expanded.png":::

1. **Yönetim** sekmesini seçin ve **IoT Için Defender** 'ın **Açık** olarak ayarlandığını doğrulayın.

    :::image type="content" source="media/quickstart-onboard-iot-hub/management-tab.png" alt-text="IoT geçiş için Defender 'ın açık olarak ayarlandığından emin olun.":::

## <a name="onboard-defender-for-iot-to-an-existing-iot-hub"></a>IoT için Defender 'ı mevcut bir IoT Hub ekleme

Cihaz kimlik yönetimi, cihazınızı buluta ve buluttan cihaza iletişim desenlerine izleyebilirsiniz ve hizmeti başlatmak için aşağıdakileri yapın: 

1. IoT Hub gidin. 

1.  **Güvenlik genel bakış**   menüsünü seçin. 

1. IoT çözümünüzü güvenli hale getirin ve ekleme formunu doldurun. 


## <a name="next-steps"></a>Sonraki adımlar

Çözümünüzü yapılandırmak için sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Bir Defender IoT mikro aracı modülü oluşturma ikizi (Önizleme)](quickstart-create-micro-agent-module-twin.md)
