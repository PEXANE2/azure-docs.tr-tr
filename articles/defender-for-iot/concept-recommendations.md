---
title: Güvenlik önerileri
description: Güvenlik önerileri kavramı ve bunların IoT için Defender 'da nasıl kullanıldığı hakkında bilgi edinin.
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
ms.openlocfilehash: 0eccab6c3d59ad68ddc8f96c3d84c57dc1bbeeca
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941722"
---
# <a name="security-recommendations"></a>Güvenlik önerileri

IoT için Defender, Azure kaynaklarınızı ve IoT cihazlarınızı tarar ve saldırı yüzeyinizi azaltmak için güvenlik önerileri sağlar.
Güvenlik önerileri, müşterilerin en iyi güvenlik uygulamalarına uymak için eyleme dönüştürülebilir ve hedeflenir.

Bu makalede, IoT Hub ve/veya IoT cihazlarınızda tetiklenebilecek önerilerin bir listesini bulacaksınız.

## <a name="recommendations-for-iot-devices"></a>IoT cihazları için öneriler

Cihaz önerileri, cihaz güvenlik duruşunu geliştirmek için Öngörüler ve öneriler sağlar.

| Önem Derecesi | Ad                                                      | Veri Kaynağı | Açıklama                                                                                                                                                                                           |
|----------|-----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Orta   | Cihazda bağlantı noktalarını açma                                      | Aracı       | Cihazda bir dinleme uç noktası bulundu.                                                                                                                                                        |
| Orta   | Zincirden birinde izin veren güvenlik duvarı ilkesi bulundu. | Aracı       | İzin verilen güvenlik duvarı ilkesi bulundu (GIRIŞ/çıkış). Güvenlik duvarı ilkesi varsayılan olarak tüm trafiği reddetmelidir ve cihaza/cihazdan gerekli iletişime izin vermek için kurallar tanımlar.                               |
| Orta   | Giriş zincirindeki izin veren güvenlik duvarı kuralı bulundu     | Aracı       | Çok sayıda IP adresi veya bağlantı noktası aralığı için izin veren bir model içeren güvenlik duvarındaki bir kural bulundu.                                                                                    |
| Orta   | Çıkış zincirindeki izin veren güvenlik duvarı kuralı bulundu    | Aracı       | Çok sayıda IP adresi veya bağlantı noktası aralığı için izin veren bir model içeren güvenlik duvarındaki bir kural bulundu.                                                                                   |
| Orta   | İşlem sistemi temel doğrulaması başarısız oldu           | Aracı       | Cihaz, [CIS Linux kıyaslamalarıyla](https://www.cisecurity.org/cis-benchmarks/)uyumlu değil.                                                                                                        |

### <a name="operational-recommendations-for-iot-devices"></a>IoT cihazları için işletimsel öneriler

İşletimsel öneriler, güvenlik Aracısı yapılandırmasını geliştirmek için Öngörüler ve öneriler sağlar.

| Önem Derecesi | Ad                                    | Veri Kaynağı | Açıklama                                                                       |
|----------|-----------------------------------------|-------------|-----------------------------------------------------------------------------------|
| Düşük      | Aracı unutilized iletileri gönderiyor          | Aracı       | Son 24 saat boyunca 10 KB veya daha fazla güvenlik iletisi 4 KB 'tan daha küçük.  |
| Düşük      | Güvenlik ikizi yapılandırması en uygun değil | Aracı       | Güvenlik ikizi yapılandırması en uygun değildir.                                        |
| Düşük      | Güvenlik ikizi yapılandırması çakışması    | Aracı       | Güvenlik ikizi yapılandırmasında çakışmalar tanımlandı. |                          |
|

## <a name="recommendations-for-iot-hub"></a>IoT Hub için öneriler

Öneri uyarıları, ortamınızın güvenlik duruşunu iyileştirecek eylemler için Öngörüler ve öneriler sağlar.

| Önem Derecesi | Ad                                                     | Veri Kaynağı | Açıklama                                                                                                                                                                                                             |
|----------|----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Yüksek     | Birden çok cihaz tarafından kullanılan özdeş kimlik doğrulama kimlik bilgileri | IoT Hub     | IoT Hub kimlik doğrulama kimlik bilgileri birden çok cihaz tarafından kullanılır. Bu, meşru bir cihazın kimliğine bürünerek uygun bir cihaz olduğunu gösterebilir. Yinelenen kimlik bilgileri kullanımı, kötü niyetli bir aktör tarafından cihaz kimliğe bürünme riskini artırır. |
| Orta   | Varsayılan IP filtresi ilkesi reddetme olmalıdır                  | IoT Hub     | IP filtresi yapılandırması, izin verilen trafik için tanımlı kurallara sahip olmalıdır ve varsayılan olarak tüm diğer trafiği varsayılan olarak reddeder.                                                                                                     |
| Orta   | IP filtresi kuralı, büyük IP aralığı içeriyor                   | IoT Hub     | Bir izin verme IP filtresi kuralı kaynak IP aralığı çok büyük. Aşırı izin veren kurallar, IoT Hub 'ınızı kötü amaçlı aktörlerin kullanımına açabilir.                                                                                       |
| Düşük      | IoT Hub tanılama günlüklerini etkinleştirme                       | IoT Hub     | Günlükleri etkinleştirin ve bir yıla kadar saklayın. Günlükleri koruma, bir güvenlik olayı gerçekleştiğinde veya ağınızın güvenliği tehlikeye atılırsa araştırma amaçlarıyla etkinlik izlerini yeniden oluşturmayı sağlar.                                       |
|

## <a name="next-steps"></a>Sonraki adımlar

- IoT hizmetine [genel bakış](overview.md) için Defender
- [Güvenlik verilerinize erişme](how-to-security-data-access.md) hakkında bilgi edinin
- [Cihazı araştırma](how-to-investigate-device.md) hakkında daha fazla bilgi edinin
