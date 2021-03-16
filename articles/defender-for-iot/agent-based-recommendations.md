---
title: Aracı tabanlı öneriler
titleSuffix: Azure Defender for IoT
description: Güvenlik önerileri kavramı ve bunların IoT cihazları için Defender için nasıl kullanıldığı hakkında bilgi edinin.
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
ms.date: 02/16/2021
ms.author: shhazam
ms.openlocfilehash: e746f37fdf3b67467c1844ebea9191679d52d6d1
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103491274"
---
# <a name="security-recommendations-for-iot-devices"></a>IoT cihazları için güvenlik önerileri

IoT için Defender, Azure kaynaklarınızı ve IoT cihazlarınızı tarar ve saldırı yüzeyinizi azaltmak için güvenlik önerileri sağlar.
Güvenlik önerileri, kullanıcılara en iyi güvenlik uygulamalarıyla uyumlu olarak yardımcı olmak için uygulanabilir ve hedeflenir.

Bu makalede, IoT cihazlarınızda tetiklenebilecek önerilerin bir listesini bulacaksınız.

## <a name="agent-based-recommendations"></a>Aracı tabanlı öneriler

Cihaz önerileri, cihaz güvenlik duruşunu geliştirmek için Öngörüler ve öneriler sağlar.

| Önem derecesi | Name | Veri Kaynağı | Açıklama |
|--|--|--|--|
| Orta | Cihazda bağlantı noktalarını açma | Klasik Defender-IoT-Micro-Agent| Cihazda bir dinleme uç noktası bulundu. |
| Orta | Zincirden birinde izin veren güvenlik duvarı ilkesi bulundu. | Klasik Defender-IoT-Micro-Agent| İzin verilen güvenlik duvarı ilkesi bulundu (GIRIŞ/çıkış). Güvenlik duvarı ilkesi varsayılan olarak tüm trafiği reddetmelidir ve cihaza/cihazdan gerekli iletişime izin vermek için kurallar tanımlar. |
| Orta | Giriş zincirindeki izin veren güvenlik duvarı kuralı bulundu | Klasik Defender-IoT-Micro-Agent| Çok sayıda IP adresi veya bağlantı noktası aralığı için izin veren bir model içeren güvenlik duvarındaki bir kural bulundu. |
| Orta | Çıkış zincirindeki izin veren güvenlik duvarı kuralı bulundu | Klasik Defender-IoT-Micro-Agent| Çok sayıda IP adresi veya bağlantı noktası aralığı için izin veren bir model içeren güvenlik duvarındaki bir kural bulundu. |
| Orta | İşlem sistemi temel doğrulaması başarısız oldu | Klasik Defender-IoT-Micro-Agent| Cihaz, [CIS Linux kıyaslamalarıyla](https://www.cisecurity.org/cis-benchmarks/)uyumlu değil. |

### <a name="agent-based-operational-recommendations"></a>Aracı tabanlı işlem önerileri

İşletimsel öneriler, güvenlik Aracısı yapılandırmasını geliştirmek için Öngörüler ve öneriler sağlar.

| Önem derecesi | Name | Veri Kaynağı | Açıklama |
|--|--|--|--|
| Düşük | Aracı unutilized iletileri gönderiyor | Klasik Defender-IoT-Micro-Agent| Son 24 saat boyunca 10 KB veya daha fazla güvenlik iletisi 4 KB 'tan daha küçük. |
| Düşük | Güvenlik ikizi yapılandırması en uygun değil | Klasik Defender-IoT-Micro-Agent| Güvenlik ikizi yapılandırması en uygun değildir. |
| Düşük | Güvenlik ikizi yapılandırması çakışması | Klasik Defender-IoT-Micro-Agent| Güvenlik ikizi yapılandırmasında çakışmalar tanımlandı. |  |

## <a name="next-steps"></a>Sonraki adımlar

- IoT hizmetine [genel bakış](overview.md) için Defender
- [Güvenlik verilerinize erişme](how-to-security-data-access.md) hakkında bilgi edinin
- [Cihazı araştırma](how-to-investigate-device.md) hakkında daha fazla bilgi edinin
