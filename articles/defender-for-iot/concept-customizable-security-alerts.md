---
title: Özel güvenlik uyarıları
description: Özelleştirilebilir güvenlik uyarıları ve IoT özellikleri ve hizmeti için Defender 'ı kullanarak önerilen düzeltme hakkında bilgi edinin.
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
ms.date: 03/04/2020
ms.author: mlottner
ms.openlocfilehash: 021b05176da141fad2f4555b0617b5a4a51e453f
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99809295"
---
# <a name="defender-for-iot-custom-security-alerts"></a>IoT için Defender özel güvenlik uyarıları

IoT için Defender, gelişmiş analiz ve tehdit bilgilerini kullanarak IoT çözümünüzü sürekli olarak analiz ederek kötü amaçlı etkinliğe karşı sizi uyarır.

Uyarıların, benzersiz kurumsal dağıtımda ve yatay olarak olası güvenliğin en verimli göstergeleri olarak davranmasını sağlamak için beklenen cihaz davranışı hakkındaki bilginiz temelinde özel uyarılar oluşturmanızı öneririz.

IoT uyarıları için aşağıdaki Defender listeleri, beklenen IoT Hub ve/veya cihaz davranışına göre size göre tanımlanabilir. Her uyarının nasıl özelleştirileceği hakkında daha fazla bilgi için bkz. [özel uyarılar oluşturma](quickstart-create-custom-alerts.md).

## <a name="built-in-custom-alerts-in-the-iot-hub"></a>IoT Hub yerleşik özel uyarılar

| Önem derecesi | Uyarı adı | Veri kaynağı | Description | Önerilen düzeltme |
|--|--|--|--|--|
| Düşük | Özel uyarı-AMQP protokolündeki cihaz iletilerinin bulut sayısı izin verilen aralığın dışında | IoT Hub | Belirli bir zaman penceresi içindeki bulut-cihaz iletileri (AMQP Protokolü) sayısı, şu anda yapılandırılmış ve izin verilen aralığın dışında. |  |
| Düşük | Özel uyarı-AMQP protokolündeki cihaz iletilerine reddedilen bulut sayısı izin verilen aralığın dışında | IoT Hub | Cihaz tarafından reddedilen bulut ve cihaz iletileri (AMQP Protokolü) sayısı, belirli bir zaman penceresi içinde şu anda yapılandırılmış ve izin verilen aralığın dışında. |  |
| Düşük | Özel uyarı-AMQP protokolündeki bulut iletilerine yönelik cihaz sayısı izin verilen aralığın dışında | IoT Hub | Belirli bir zaman penceresi içindeki bulut iletilerine (AMQP Protokolü) cihaz miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında. |  |
| Düşük | Özel uyarı-çağıran doğrudan Yöntem sayısı izin verilen aralığın dışında | IoT Hub | Belirli bir zaman penceresi içinde doğrudan yöntem çağıran miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında. |  |
| Düşük | Özel uyarı-karşıya dosya yükleme sayısı izin verilen aralığın dışında | IoT Hub | Belirli bir zaman penceresi içindeki dosya karşıya yükleme miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında. |  |
| Düşük | Özel uyarı-HTTP protokolündeki cihaz iletilerinin bulut sayısı izin verilen aralığın dışında | IoT Hub | Bir zaman penceresinde bulut-cihaz iletileri (HTTP protokolü) miktarı yapılandırılmış izin verilen aralıkta değil |
| Düşük | Özel uyarı-HTTP protokolündeki reddedilen bulutun cihaz iletileri sayısı izin verilen aralıkta değil | IoT Hub | Belirli bir zaman penceresi içindeki bulut-cihaz iletileri (HTTP protokolü) miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında. |
| Düşük | Özel uyarı-HTTP protokolündeki bulut iletilerine yönelik cihaz sayısı izin verilen aralığın dışında | IoT Hub | Belirli bir zaman penceresi içindeki bulut iletilerine (HTTP protokolü) cihaz miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında. |  |
| Düşük | Özel uyarı-MQTT protokolünde cihaz iletilerinin bulut sayısı izin verilen aralığın dışında | IoT Hub | Belirli bir zaman penceresi içindeki bulut-cihaz iletileri (MQTT Protokolü) miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında. |  |
| Düşük | Özel uyarı-MQTT protokolünde reddedilen bulutun cihaz iletileri sayısı izin verilen aralığın dışında | IoT Hub | Belirli bir zaman penceresi içinde cihaz tarafından reddedilen bulut ve cihaz iletileri (MQTT Protokolü) miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında. |
| Düşük | Özel uyarı-MQTT protokolündeki bulut iletilerine yönelik cihaz sayısı izin verilen aralığın dışında | IoT Hub | Belirli bir zaman penceresi içindeki bulut iletilerine (MQTT Protokolü) cihaz miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında. |
| Düşük | Özel uyarı-izin verilen aralığın dışında kalan komut kuyruğu sayısını temizler | IoT Hub | Komut kuyruğunun miktarı belirli bir zaman penceresi içinde temizler ve şu anda yapılandırılmış ve izin verilen aralığın dışında. |  |
| Düşük | Özel uyarı-modül ikizi Updates sayısı izin verilen aralığın dışında | IoT Hub | Belirli bir zaman penceresi içindeki modül ikizi güncelleştirmelerinin miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında. |
| Düşük | Özel uyarı-yetkisiz işlem sayısı izin verilen aralığın dışında | IoT Hub | Belirli bir zaman penceresi içindeki yetkisiz işlem miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında. |


## <a name="agent-based-security-custom-alerts"></a>Aracı tabanlı güvenlik özel uyarıları

| Önem derecesi | Uyarı adı | Veri kaynağı | Description | Önerilen düzeltme |
|--|--|--|--|--|
| Düşük | Özel uyarı-etkin bağlantı sayısı izin verilen aralığın dışında | Klasik Güvenlik modülü, Azure RTOS | Belirli bir zaman penceresi içindeki etkin bağlantı sayısı, şu anda yapılandırılmış ve izin verilen aralığın dışında. | Cihaz günlüklerini araştırın. Bağlantının nereden geldiğini öğrenin ve zararsız veya kötü amaçlı olup olmadığını öğrenin. Kötü amaçlı, olası kötü amaçlı yazılımları kaldırın ve kaynağı anlayın. Benign ise, kaynağı izin verilen bağlantı listesine ekleyin. |
| Düşük | Özel uyarı-izin verilmeyen bir IP 'ye oluşturulan giden bağlantı | Klasik Güvenlik modülü, Azure RTOS | İzin verilen IP listenizin dışında bir IP 'ye giden bağlantı oluşturuldu. | Cihaz günlüklerini araştırın. Bağlantının nereden geldiğini öğrenin ve zararsız veya kötü amaçlı olup olmadığını öğrenin. Kötü amaçlı, olası kötü amaçlı yazılımları kaldırın ve kaynağı anlayın. Benign ise, kaynağı izin verilen IP listesine ekleyin. |
| Düşük | Özel uyarı-başarısız yerel oturum açma sayısı izin verilen aralığın dışında | Klasik Güvenlik modülü, Azure RTOS | Belirli bir zaman penceresi içindeki başarısız yerel oturum açma sayısı, şu anda yapılandırılmış ve izin verilen aralığın dışında. |  |
| Düşük | Özel uyarı-izin verilen kullanıcı listesinde olmayan bir kullanıcının oturum açması | Klasik Güvenlik modülü, Azure RTOS | İzin verilen Kullanıcı listenizin dışında, cihazda oturum açmış yerel bir kullanıcı. | Ham verileri kaydediyorsanız, Log Analytics hesabınıza gidin ve verileri kullanarak cihazı araştırın, kaynağı tanımlayabilir ve ardından bu ayarlar için izin verme/engelleme listesini onarın. Şu anda ham verileri kaydettiğinizden cihaza gidin ve bu ayarlar için izin verme/engelleme listesini onarın. |
| Düşük | Özel uyarı-izin verilmeyen bir işlem yürütüldü | Klasik Güvenlik modülü, Azure RTOS | Cihazda izin verilmeyen bir işlem yürütüldü. | Ham verileri kaydediyorsanız, Log Analytics hesabınıza gidin ve verileri kullanarak cihazı araştırın, kaynağı tanımlayabilir ve ardından bu ayarlar için izin verme/engelleme listesini onarın. Şu anda ham verileri kaydettiğinizden cihaza gidin ve bu ayarlar için izin verme/engelleme listesini onarın. |
|

## <a name="next-steps"></a>Sonraki adımlar

- [Bir uyarıyı özelleştirmeyi](quickstart-create-custom-alerts.md) öğrenin
- IoT hizmetine [genel bakış](overview.md) için Defender
- [Güvenlik verilerinize erişme](how-to-security-data-access.md) hakkında bilgi edinin
- [Cihazı araştırma](how-to-investigate-device.md) hakkında daha fazla bilgi edinin
