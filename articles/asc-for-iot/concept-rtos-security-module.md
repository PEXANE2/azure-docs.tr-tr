---
title: Azure IoT güvenlik modülünün temel bilgileri hakkında kavramsal açıklama-Azure RTOS
description: Azure RTOS IoT güvenlik modülü kavramları ve iş akışı hakkında temel bilgileri öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2020
ms.author: mlottner
ms.openlocfilehash: f4793aa27dfbb037416d6165ddc659bb864a6647
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89514943"
---
# <a name="security-module-for-azure-rtos-preview"></a>Azure RTOS için güvenlik modülü (Önizleme)

Özellikler ve avantajlar da dahil olmak üzere Azure RTOS güvenlik modülünü ve ilgili yapılandırma ve başvuru kaynaklarının bağlantılarını daha iyi anlamak için bu makaleyi kullanın. 

## <a name="azure-rtos-iot-security-module"></a>Azure RTOS IoT güvenlik modülü

Azure RTOS için güvenlik modülü, NetX Duo sunumunun bir parçası olarak Azure RTOS cihazları için kapsamlı bir güvenlik çözümü sağlar. NetX Duo sunumunda Azure RTOS yerleşik Azure IoT güvenlik modülü ile birlikte gelir ve etkinleştirildikten sonra gerçek zamanlı işletim sistemi cihazlarınızda ortak tehditler için kapsam sağlar. 

Azure RTOS için güvenlik modülü arka planda çalışır ve her bir müşterinin IoT Hub benzersiz bağlantılarını kullanarak güvenlik iletileri gönderirken sorunsuz bir kullanıcı deneyimi sağlar. Azure RTOS için güvenlik modülü varsayılan olarak etkindir.  

## <a name="azure-rtos-netx-duo"></a>Azure RTOS NetX Duo

Azure RTOS NetX Duo, özellikle de gömülü gerçek zamanlı ve IoT uygulamaları için özel olarak tasarlanmış gelişmiş, endüstriyel düzeyde bir TCP/IP ağ yığını. Azure RTOS NetX Duo, güvenlik ve bulut dahil olmak üzere zengin bir protokoller kümesi sağlayan çift IPv4 ve IPv6 ağ yığınıdır. [Azure RTOS NetX Duo](https://aka.ms/netxduo) çözümleri hakkında daha fazla bilgi edinin.

Modül aşağıdaki özellikleri sunar:

- **Kötü amaçlı ağ etkinliklerini Algıla**
- **Özel uyarılara dayanan cihaz davranışı temelleri**
- **Cihaz güvenliğini iyileştirme durumu**

## <a name="security-module-for-azure-rtos-architecture"></a>Azure RTOS mimarisi için güvenlik modülü

Azure RTOS için güvenlik modülü, Azure IoT ara yazılımı platformu tarafından başlatılır ve hub 'a güvenlik telemetri göndermek için IoT Hub istemcileri kullanır.

:::image type="content" source="media/architecture/security-module-state-diagram.png" alt-text="Azure IoT güvenlik modülü durum diyagramı ve bilgi akışı":::

Azure RTOS için güvenlik modülü, aşağıdaki cihaz etkinliğini ve üç toplayıcıyı kullanarak bilgileri izler:
- Cihaz ağı etkinliği **TCP**, **UDP**ve **ICM**
- **Threadx** ve **NETX Duo** sürümleri olarak sistem bilgileri
- Sinyal olayları

Her toplayıcı bir öncelik grubuna bağlanır ve her bir öncelik grubu, olası **düşük**, **Orta**ve **yüksek**değerlerle kendi aralığına sahiptir. Aralıklar, verilerin toplandığı ve gönderildiği zaman aralığını etkiler.

Her zaman aralığı yapılandırılabilir ve çözümünüzü daha fazla [özelleştirmek](how-to-azure-rtos-security-module.md)için IoT bağlayıcıları etkinleştirilebilir ve devre dışı bırakılabilir. 

## <a name="supported-security-alerts-and-recommendations"></a>Desteklenen güvenlik uyarıları ve önerileri

Azure RTOS için güvenlik modülü, belirli güvenlik uyarılarını ve önerilerini destekler. İlk yapılandırmayı tamamladıktan sonra hizmetinizin [ilgili uyarı ve öneri değerlerini incelediğinizden ve özelleştirdiğinizden](concept-rtos-security-alerts-recommendations.md) emin olun.

## <a name="ready-to-begin"></a>Başlamaya hazırlanıyor musunuz?

Azure RTOS için güvenlik modülü, IoT cihazlarınız için ücretsiz bir indirme olarak sunulmaktadır. IoT bulut hizmeti için Azure Güvenlik Merkezi, Azure aboneliği başına 30 günlük deneme sürümü ile kullanılabilir. [Güvenlik modülünü şimdi indirin](https://github.com/azure-rtos/azure-iot-preview/releases) ve kullanmaya başlayın. 

## <a name="next-steps"></a>Sonraki adımlar

- Azure RTOS [Önkoşulları ve kurulumu](quickstart-azure-rtos-security-module.md)Için güvenlik modülünü kullanmaya başlayın.
- Azure RTOS [güvenlik uyarıları ve öneri desteği](concept-rtos-security-alerts-recommendations.md)Için güvenlik modülü hakkında daha fazla bilgi edinin. 
- Azure RTOS [başvuru API 'si](azure-rtos-security-module-api.md)Için güvenlik modülünü kullanın.

