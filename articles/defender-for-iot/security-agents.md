---
title: Güvenlik aracısına genel bakış
description: IoT cihazlarınızdaki IoT güvenlik hizmeti aracıları için Azure Defender 'ı anlama, yapılandırma, dağıtma ve kullanma ile çalışmaya başlayın.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 888fdbb2352c4ac972eac8b24c29d22ce973e04d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90941798"
---
# <a name="get-started-with-azure-defender-for-iot-device-security-agents"></a>IoT cihaz güvenlik aracıları için Azure Defender ile çalışmaya başlama

IoT güvenlik aracıları için Defender, uzak bağlantıları, etkin uygulamaları, oturum açma olaylarını ve işletim sistemi yapılandırma en iyi yöntemlerini izleme gibi gelişmiş güvenlik özellikleri sunar. Tek bir hizmetle cihaz alanı tehdit koruması ve güvenlik duruşunuzu denetim altına alın.

Linux ve Windows Güvenlik aracıları için başvuru mimarisi her ikisi de C# ve C içinde sağlanır.

IoT güvenlik aracıları için Defender, cihaz işletim sisteminden Ham olay toplamayı, maliyeti azaltmak için olay toplamayı ve bir cihaz modülü ikizi aracılığıyla yapılandırmayı işler. Güvenlik iletileri, IoT Hub aracılığıyla IoT Analytics hizmetleri için Defender 'a gönderilir.

IoT güvenlik aracılarınız için Defender 'ı dağıtmak ve test etmek üzere aşağıdaki iş akışını kullanın:

1. [IoT Hub IoT hizmeti için Defender 'ı etkinleştirin](quickstart-onboard-iot-hub.md)
1. IoT Hub kayıtlı bir cihaz yoksa, [Yeni bir cihaz kaydedin](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy).
1. Cihazlarınız için [bir azureiotsecurity güvenlik modülü oluşturun](quickstart-create-security-twin.md) .
1. Aracıyı gerçek bir cihaza yüklemek yerine bir Azure sanal cihazına yüklemek için, kullanılabilir bir bölgede [Yeni bir Azure sanal makinesi (VM)](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) çalıştırın.
1. IoT cihazınıza veya yeni VM 'de [IoT güvenlik Aracısı için bir Defender dağıtın](how-to-deploy-linux-cs.md) .
1. Bir saldırının zararsız benzetimini çalıştırmak için [trigger_events](https://aka.ms/iot-security-github-trigger-events) yönergelerini izleyin.
1. Önceki adımda oluşan sanal saldırıya yanıt olarak Defender 'ı IoT uyarıları için doğrulayın. Betiği çalıştırdıktan sonra beş dakika sonra doğrulamaya başlayın.
1. IoT Hub kullanarak [Log Analytics kullanarak](how-to-security-data-access.md) [uyarıları](concept-security-alerts.md), [önerileri](concept-recommendations.md)ve derin ayrıntılarını inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Çözümünüzü](quickstart-configure-your-solution.md) yapılandırın
- [Güvenlik modülleri oluştur](quickstart-create-security-twin.md)
- [Özel uyarıları](quickstart-create-custom-alerts.md) yapılandırma
- [Güvenlik aracısı dağıtma](how-to-deploy-agent.md)
