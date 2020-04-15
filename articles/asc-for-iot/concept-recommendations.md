---
title: Güvenlik önerileri
description: IoT için Azure Güvenlik Merkezi'nde güvenlik önerileri kavramı ve bunların nasıl kullanıldığı hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 02ced504-d3aa-4770-9d10-b79f80af366c
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2019
ms.author: mlottner
ms.openlocfilehash: 213595ac69efc90ec855b2891641e1f00bd1ba92
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311555"
---
# <a name="security-recommendations"></a>Güvenlik önerileri

Azure Güvenlik Merkezi IoT, Azure kaynaklarınızı ve IoT aygıtlarınızı tarar ve saldırı yüzeyinizi azaltmak için güvenlik önerileri sunar.
Güvenlik önerileri uygulanabilir ve müşterilerin güvenlik en iyi uygulamalarına uymalarına yardımcı olmayı amaçlamaktadır.

Bu makalede, IoT Hub ve/veya IoT aygıtlarınızda tetiklenebilecek önerilerin bir listesini bulacaksınız.

## <a name="recommendations-for-iot-devices"></a>IoT cihazları için öneriler

Cihaz önerileri, cihaz güvenlik duruşunu iyileştirmek için öngörüler ve öneriler sağlar.

| Severity | Adı                                                      | veri kaynağı | Açıklama                                                                                                                                                                                           |
|----------|-----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Orta   | Aygıttaki Bağlantı Noktalarını Aç                                      | Aracı       | Cihazda bir dinleme bitiş noktası bulundu.                                                                                                                                                        |
| Orta   | Zincirlerden birinde bulunan izin verilen güvenlik duvarı ilkesi. | Aracı       | İzin verilen güvenlik duvarı ilkesi bulundu (GIRIŞ/OUTPUT). Güvenlik duvarı ilkesi varsayılan olarak tüm trafiği reddetmeli ve aygıtla/aygıttan gerekli iletişimi sağlamak için kurallar tanımlamalıdır.                               |
| Orta   | Giriş zincirinde izin verilen güvenlik duvarı kuralı bulundu     | Aracı       | Güvenlik duvarında çok çeşitli IP adresleri veya bağlantı noktaları için izin veren bir desen içeren bir kural bulundu.                                                                                    |
| Orta   | Çıkış zincirinde izin verilen güvenlik duvarı kuralı bulundu    | Aracı       | Güvenlik duvarında çok çeşitli IP adresleri veya bağlantı noktaları için izin veren bir desen içeren bir kural bulundu.                                                                                   |
| Orta   | Çalışma sistemi taban çizgisi doğrulaması başarısız oldu           | Aracı       | Cihaz [CIS Linux kriterlerine](https://www.cisecurity.org/cis-benchmarks/)uymaz.                                                                                                        |

### <a name="operational-recommendations-for-iot-devices"></a>IoT cihazları için operasyonel öneriler

Operasyonel öneriler, güvenlik aracıyapılandırmasını geliştirmek için öngörüler ve öneriler sağlar.

| Severity | Adı                                    | veri kaynağı | Açıklama                                                                       |
|----------|-----------------------------------------|-------------|-----------------------------------------------------------------------------------|
| Düşük      | Aracı kullanılmayan iletiler gönderir          | Aracı       | Son 24 saat içinde güvenlik mesajlarının %10'u veya daha fazlası 4 KB'den daha küçüktü.  |
| Düşük      | Güvenlik ikiz yapılandırması en iyi değil | Aracı       | Güvenlik ikiz yapılandırması en iyi değildir.                                        |
| Düşük      | Güvenlik ikiz yapılandırma çakışması    | Aracı       | Çakışmalar güvenlik ikiz yapılandırmasında tespit edildi. |                          |
|

## <a name="recommendations-for-iot-hub"></a>IoT Hub için öneriler

Öneri uyarıları, ortamınızın güvenlik duruşunu iyileştirmek için eylemler için öngörü ve öneriler sağlar.

| Severity | Adı                                                     | veri kaynağı | Açıklama                                                                                                                                                                                                             |
|----------|----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Yüksek     | Birden çok aygıt tarafından kullanılan özdeş kimlik doğrulama kimlik bilgileri | IoT Hub     | IoT Hub kimlik doğrulama kimlik bilgileri birden çok aygıt tarafından kullanılır. Bu, yasal bir aygıtın kimliğine bürünen gayri meşru bir aygıtı gösterebilir. Yinelenen kimlik bilgisi kullanımı, kötü amaçlı bir aktörün aygıt kimliğe bürünme riskini artırır. |
| Orta   | Varsayılan IP filtresi ilkesi reddedilmelidir                  | IoT Hub     | IP filtresi yapılandırması, izin verilen trafik için tanımlanmış kuralları olmalıdır ve varsayılan olarak varsayılan olarak diğer tüm trafiği reddetmelidir.                                                                                                     |
| Orta   | IP filtre kuralı büyük IP aralığı içerir                   | IoT Hub     | İzin ver IP filtresi kural kaynağı IP aralığı çok büyüktür. Aşırı izin verilen kurallar, IoT hub'ınızı kötü niyetli aktörlere maruz bırakabilir.                                                                                       |
| Düşük      | IoT Hub'da tanılama günlüklerini etkinleştirme                       | IoT Hub     | Günlükleri etkinleştirin ve bir yıla kadar saklayın. Günlükleri saklama, bir güvenlik olayı meydana geldiğinde veya ağınız tehlikeye girdiğinde, araştırma amacıyla etkinlik izlerini yeniden oluşturmanıza olanak tanır.                                       |
|

## <a name="next-steps"></a>Sonraki adımlar

- IoT hizmeti için Azure Güvenlik Merkezi [Genel Bakış](overview.md)
- Güvenlik verilerinize nasıl [erişilenleri](how-to-security-data-access.md) öğrenin
- [Bir aygıtı araştırma](how-to-investigate-device.md) hakkında daha fazla bilgi edinin
