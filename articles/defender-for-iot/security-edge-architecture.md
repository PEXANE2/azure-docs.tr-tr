---
title: IoT Edge için bir IoT güvenlik modülü için Defender
description: IoT Edge için Azure Defender 'ın IoT güvenlik modülü mimarisini ve yeteneklerini anlayın.
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
ms.date: 09/09/2020
ms.author: mlottner
ms.openlocfilehash: 3e653983b39e8eb4ac13ad840ca53e4ab6d1cfc7
ms.sourcegitcommit: 2501fe97400e16f4008449abd1dd6e000973a174
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99820628"
---
# <a name="azure-defender-for-iot-edge-security-module"></a>IoT Edge güvenlik modülü için Azure Defender

[Azure IoT Edge](../iot-edge/index.yml) kenarda iş akışlarını yönetmek ve gerçekleştirmek için güçlü özellikler sunar.
IoT ortamlarında IoT Edge oynadığı anahtar bölüm, kötü amaçlı aktörler için özellikle çekici hale gelir.

IoT güvenlik modülü için Defender, IoT Edge cihazlarınız için kapsamlı bir güvenlik çözümü sağlar.
IoT modülü için Defender, Işletim sisteminizin ve kapsayıcı sisteminizin ham güvenlik verilerini, uygulanabilir güvenlik önerileri ve uyarılarına toplar, toplar ve analiz eder.

IoT cihazlarında IoT güvenlik aracıları için Defender 'a benzer şekilde, IoT Edge modülü için Defender, ikizi modülü aracılığıyla yüksek düzeyde özelleştirilebilir.
Daha fazla bilgi için bkz. [aracınızı yapılandırma](how-to-agent-configuration.md) .

IoT Edge IoT güvenlik modülü için Defender aşağıdaki özellikleri sunmaktadır:

- Temel Işletim sistemi (Linux) ve IoT Edge kapsayıcı sistemlerinden ham güvenlik olayları toplar.

  Kullanılabilir güvenlik veri toplayıcıları hakkında daha fazla bilgi edinmek için bkz. [IoT Aracısı yapılandırması Için Defender](how-to-agent-configuration.md) .

- IoT Edge dağıtım bildirimlerinin analizi.

- [IoT Edge hub](../iot-edge/iot-edge-runtime.md#iot-edge-hub)aracılığıyla gönderilen iletilere ham güvenlik olayları toplar.

- İkizi güvenlik modülünün kullanımıyla yapılandırmayı kaldırın.

  Daha fazla bilgi edinmek için bkz. [IoT Aracısı için bir Defender yapılandırma](how-to-agent-configuration.md) .

IoT Edge için, IoT güvenlik modülü için Defender, IoT Edge altında ayrıcalıklı modda çalışır.
Modülün Işletim sistemini ve diğer IoT Edge modüllerini izlemesine izin vermek için ayrıcalıklı mod gerekir.

## <a name="module-supported-platforms"></a>Modül desteklenen platformlar

IoT Edge için IoT güvenlik modülü için Defender Şu anda yalnızca Linux 'ta kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, IoT Edge için bkz. IoT güvenlik modülü için Defender 'ın mimari ve özellikleri hakkında bilgi edindiniz.

IoT dağıtımı için Defender 'ı kullanmaya devam etmek için aşağıdaki makaleleri kullanın:

- [IoT Edge için güvenlik modülünü](how-to-deploy-edge.md) dağıtma
- [Güvenlik modülünüzü yapılandırmayı](how-to-agent-configuration.md) öğrenin
- IoT için IoT [Defender](resources-manage-proprietary-protocols.md) Için Defender 'ı inceleyin
- [IoT Hub Için Defender 'ı nasıl etkinleştirebileceğinizi](quickstart-onboard-iot-hub.md) öğrenin
- [IoT Için Defender](resources-frequently-asked-questions.md) aracılığıyla hizmet hakkında daha fazla bilgi edinin