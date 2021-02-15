---
title: Güvenlik aracıları
description: IoT cihazlarınızdaki IoT güvenlik hizmeti aracıları için Azure Defender 'ı anlama, yapılandırma, dağıtma ve kullanma ile çalışmaya başlayın.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/24/2021
ms.author: shhazam
ms.openlocfilehash: a40b64dd3b8f898c961863c1d78a2a56642c44b9
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100521620"
---
# <a name="get-started-with-azure-defender-for-iot-device-micro-agents"></a>IoT cihaz mikro aracıları için Azure Defender ile çalışmaya başlama

IoT güvenlik aracıları için Defender, işletim sistemi yapılandırma en iyi yöntemlerini izleme gibi gelişmiş güvenlik özellikleri sunar. Tek bir hizmetle cihaz alanı tehdit koruması ve güvenlik duruşunuzu denetim altına alın.

IoT güvenlik aracılarına yönelik savunma, cihaz işletim sisteminden Ham olay toplamayı, maliyeti azaltmak için olay toplamayı ve bir cihaz modülü ikizi aracılığıyla yapılandırmayı işler. Güvenlik iletileri, IoT Hub aracılığıyla IoT Analytics hizmetleri için Defender 'a gönderilir.

IoT güvenlik aracılarınız için Defender 'ı dağıtmak ve test etmek üzere aşağıdaki iş akışını kullanın:

1. [IoT hizmetini IoT Hub Için Defender 'ı etkinleştirin](quickstart-onboard-iot-hub.md).

1. IoT Hub kayıtlı bir cihaz yoksa, [Yeni bir cihaz kaydedin](../iot-accelerators/iot-accelerators-device-simulation-overview.md).

1. Cihazlarınız için [bir savunma Deriotmikro aracı modülü Ikizi oluşturun](quickstart-create-micro-agent-module-twin.md) .

1. Aracıyı gerçek bir cihaza yüklemek yerine bir Azure sanal cihazına yüklemek için, kullanılabilir bir bölgede [Yeni bir Azure sanal makinesi (VM)](../virtual-machines/linux/quick-create-portal.md) çalıştırın.

1. IoT cihazınıza veya yeni VM 'de [IoT güvenlik Aracısı için bir Defender dağıtın](how-to-deploy-linux-cs.md) .

1. Bir işletim sistemi temel olayı çalıştırmak için [trigger_events](https://aka.ms/iot-security-github-trigger-events) yönergelerini izleyin.

1. Önceki adımda sanal işletim sistemi taban çizgisi denetim hatasına yanıt olarak Defender 'ı IoT önerileri için doğrulayın. Betiği çalıştırdıktan sonra 30 dakika sonra doğrulamaya başlayın.

## <a name="next-steps"></a>Sonraki adımlar

Çözümünüzü yapılandırma [](quickstart-configure-your-solution.md) 
 [güvenlik modülleri oluşturma](quickstart-create-security-twin.md) [özel uyarıları](quickstart-create-custom-alerts.md)yapılandırma 
 [Güvenlik aracısı dağıtma](how-to-deploy-agent.md)
