---
title: Güvenlik önerileri
description: Güvenlik önerileri kavramı ve bunların IoT için Defender 'da nasıl kullanıldığı hakkında bilgi edinin.
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
ms.date: 01/25/2021
ms.author: shhazam
ms.openlocfilehash: d11154e26e2ffe97a1ea102a11dddb746db9433a
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809278"
---
# <a name="security-recommendations"></a>Güvenlik önerileri

IoT için Defender, Azure kaynaklarınızı ve IoT cihazlarınızı tarar ve saldırı yüzeyinizi azaltmak için güvenlik önerileri sağlar.
Güvenlik önerileri, kullanıcılara en iyi güvenlik uygulamalarıyla uyumlu olarak yardımcı olmak için uygulanabilir ve hedeflenir.

Bu makalede, IoT Hub ve/veya IoT cihazlarınızda tetiklenebilecek önerilerin bir listesini bulacaksınız.

## <a name="agent-based-recommendations"></a>Aracı tabanlı öneriler

Cihaz önerileri, cihaz güvenlik duruşunu geliştirmek için Öngörüler ve öneriler sağlar.

| Önem derecesi | Name | Veri Kaynağı | Description |
|--|--|--|--|
| Orta | Cihazda bağlantı noktalarını açma | Klasik Güvenlik modülü | Cihazda bir dinleme uç noktası bulundu. |
| Orta | Zincirden birinde izin veren güvenlik duvarı ilkesi bulundu. | Klasik Güvenlik modülü | İzin verilen güvenlik duvarı ilkesi bulundu (GIRIŞ/çıkış). Güvenlik duvarı ilkesi varsayılan olarak tüm trafiği reddetmelidir ve cihaza/cihazdan gerekli iletişime izin vermek için kurallar tanımlar. |
| Orta | Giriş zincirindeki izin veren güvenlik duvarı kuralı bulundu | Klasik Güvenlik modülü | Çok sayıda IP adresi veya bağlantı noktası aralığı için izin veren bir model içeren güvenlik duvarındaki bir kural bulundu. |
| Orta | Çıkış zincirindeki izin veren güvenlik duvarı kuralı bulundu | Klasik Güvenlik modülü | Çok sayıda IP adresi veya bağlantı noktası aralığı için izin veren bir model içeren güvenlik duvarındaki bir kural bulundu. |
| Orta | İşlem sistemi temel doğrulaması başarısız oldu | Klasik Güvenlik modülü | Cihaz, [CIS Linux kıyaslamalarıyla](https://www.cisecurity.org/cis-benchmarks/)uyumlu değil. |

### <a name="agent-based-operational-recommendations"></a>Aracı tabanlı işlem önerileri

İşletimsel öneriler, güvenlik Aracısı yapılandırmasını geliştirmek için Öngörüler ve öneriler sağlar.

| Önem derecesi | Name | Veri Kaynağı | Description |
|--|--|--|--|
| Düşük | Aracı unutilized iletileri gönderiyor | Klasik Güvenlik modülü | Son 24 saat boyunca 10 KB veya daha fazla güvenlik iletisi 4 KB 'tan daha küçük. |
| Düşük | Güvenlik ikizi yapılandırması en uygun değil | Klasik Güvenlik modülü | Güvenlik ikizi yapılandırması en uygun değildir. |
| Düşük | Güvenlik ikizi yapılandırması çakışması | Klasik Güvenlik modülü | Güvenlik ikizi yapılandırmasında çakışmalar tanımlandı. |  |


## <a name="built-in-recommendations-in-iot-hub"></a>IoT Hub 'de yerleşik öneriler

Öneri uyarıları, ortamınızın güvenlik duruşunu iyileştirecek eylemler için Öngörüler ve öneriler sağlar.

| Önem derecesi | Name | Veri Kaynağı | Description |
|--|--|--|--|
| Yüksek | Birden çok cihaz tarafından kullanılan özdeş kimlik doğrulama kimlik bilgileri | IoT Hub | IoT Hub kimlik doğrulama kimlik bilgileri birden çok cihaz tarafından kullanılır. Bu işlem, meşru bir cihazın kimliğine bürünerek uygun bir cihaz olduğunu gösterebilir. Yinelenen kimlik bilgileri kullanımı, kötü niyetli bir aktör tarafından cihaz kimliğe bürünme riskini artırır. |
| Orta | Varsayılan IP filtresi ilkesi reddetme olmalıdır | IoT Hub | IP filtresi yapılandırması, izin verilen trafik için tanımlı kurallara sahip olmalıdır ve varsayılan olarak tüm diğer trafiği varsayılan olarak reddeder. |
| Orta | IP filtresi kuralı, büyük IP aralığı içeriyor | IoT Hub | Bir izin verme IP filtresi kuralı kaynak IP aralığı çok büyük. Aşırı izin veren kurallar, IoT Hub 'ınızı kötü amaçlı aktörlerin kullanımına açabilir. |
| Düşük | IoT Hub tanılama günlüklerini etkinleştirme | IoT Hub | Günlükleri etkinleştirin ve bir yıla kadar saklayın. Günlükleri koruma, bir güvenlik olayı gerçekleştiğinde veya ağınızın güvenliği tehlikeye atılırsa araştırma amaçlarıyla etkinlik izlerini yeniden oluşturmayı sağlar. |


## <a name="next-steps"></a>Sonraki adımlar

- IoT hizmetine [genel bakış](overview.md) için Defender
- [Güvenlik verilerinize erişme](how-to-security-data-access.md) hakkında bilgi edinin
- [Cihazı araştırma](how-to-investigate-device.md) hakkında daha fazla bilgi edinin
