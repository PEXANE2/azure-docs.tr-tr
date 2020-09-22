---
title: Özelleştirilebilir güvenlik uyarıları
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
ms.openlocfilehash: 5a5b9182081e267f8e20cb0818202b9cb5ecd904
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941726"
---
# <a name="defender-for-iot-customizable-security-alerts"></a>IoT özelleştirilebilir güvenlik uyarıları için Defender

IoT için Defender, gelişmiş analiz ve tehdit bilgilerini kullanarak IoT çözümünüzü sürekli olarak analiz ederek kötü amaçlı etkinliğe karşı sizi uyarır.

Uyarıların, benzersiz kurumsal dağıtımda ve yatay olarak olası güvenliğin en verimli göstergeleri olarak davranmasını sağlamak için beklenen cihaz davranışı hakkındaki bilginiz temelinde özel uyarılar oluşturmanızı öneririz.

IoT uyarıları için aşağıdaki Defender listesi, beklenen IoT Hub ve/veya cihaz davranışına göre size göre tanımlanabilir. Her uyarının nasıl özelleştirileceği hakkında daha fazla ayrıntı için bkz. [özel uyarılar oluşturma](quickstart-create-custom-alerts.md).

## <a name="iot-hub-alerts-available-for-customization"></a>IoT Hub uyarılarını özelleştirmek için kullanılabilir

| Önem Derecesi | Uyarı adı | Veri kaynağı | Açıklama | Önerilen düzeltme|
|---|---|---|---|---|
| Düşük      | Özel uyarı-AMQP protokolündeki cihaz iletilerinin bulut sayısı izin verilen aralığın dışında          | IoT Hub     | Belirli bir zaman penceresi içindeki bulut-cihaz iletileri (AMQP Protokolü) sayısı, şu anda yapılandırılmış ve izin verilen aralığın dışında.||
| Düşük      | Özel uyarı-AMQP protokolündeki cihaz iletilerine reddedilen bulut sayısı izin verilen aralığın dışında | IoT Hub     | Cihaz tarafından reddedilen bulut-cihaz iletileri (AMQP Protokolü) sayısı, belirli bir zaman penceresi içinde yapılandırılmış ve izin verilen aralığın dışında.||
| Düşük      | Özel uyarı-AMQP protokolündeki bulut iletilerine yönelik cihaz sayısı izin verilen aralığın dışında      | IoT Hub     | Belirli bir zaman penceresi içindeki bulut iletilerine (AMQP Protokolü) cihaz miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında.|   |
| Düşük      | Özel uyarı-çağıran doğrudan Yöntem sayısı izin verilen aralığın dışında | IoT Hub     | Belirli bir zaman penceresi içinde doğrudan yöntem çağıran miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında.||
| Düşük      | Özel uyarı-dosya yükleme sayısı izin verilen aralığın dışında | IoT Hub     | Belirli bir zaman penceresi içindeki dosya karşıya yükleme miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında.| |
| Düşük      | Özel uyarı-HTTP protokolündeki cihaz iletileri için bulut sayısı izin verilen aralığın dışında | IoT Hub     | Bir zaman penceresinde bulut-cihaz iletileri (HTTP protokolü) miktarı yapılandırılmış izin verilen aralıkta değil                                  |
| Düşük      | Özel uyarı-HTTP protokolündeki cihaz iletilerine reddedilen bulut sayısı izin verilen aralıkta değil | IoT Hub     | Belirli bir zaman penceresi içindeki bulut-cihaz iletileri (HTTP protokolü) miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında. |
| Düşük      | Özel uyarı-HTTP protokolündeki bulut iletilerine yönelik cihaz sayısı izin verilen aralığın dışında | IoT Hub| Belirli bir zaman penceresi içindeki bulut iletilerine (HTTP protokolü) cihaz miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında.|    |
| Düşük      | Özel uyarı-MQTT protokolünde cihaz iletilerine yönelik bulut sayısı izin verilen aralığın dışında | IoT Hub     | Belirli bir zaman penceresi içindeki bulut-cihaz iletileri (MQTT Protokolü) miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında.|   |
| Düşük      | Özel uyarı-MQTT protokolünde cihaz iletilerine reddedilen bulut sayısı izin verilen aralığın dışında | IoT Hub     | Belirli bir zaman penceresi içinde cihaz tarafından reddedilen bulut ve cihaz iletileri (MQTT Protokolü) miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında. |
| Düşük      | Özel uyarı-MQTT protokolünde bulut iletilerine yönelik cihaz sayısı izin verilen aralığın dışında          | IoT Hub     | Belirli bir zaman penceresi içindeki bulut iletilerine (MQTT Protokolü) cihaz miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında.|
| Düşük      | Özel uyarı-komut kuyruğu temizler sayısı izin verilen aralığın dışında                               | IoT Hub     | Komut kuyruğunun miktarı belirli bir zaman penceresi içinde temizler ve şu anda yapılandırılmış ve izin verilen aralığın dışında.||
| Düşük      | Özel uyarı-modül ikizi güncelleştirmelerinin sayısı izin verilen aralığın dışında                                       | IoT Hub     | Belirli bir zaman penceresi içindeki modül ikizi güncelleştirmelerinin miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında.|
| Düşük      | Özel uyarı-yetkisiz işlem sayısı izin verilen aralığın dışında  | IoT Hub     | Belirli bir zaman penceresi içindeki yetkisiz işlem miktarı Şu anda yapılandırılmış ve izin verilen aralığın dışında.|
|

## <a name="agent-alerts-available-for-customization"></a>Özelleştirme için kullanılabilir Aracı uyarıları

| Önem Derecesi | Uyarı adı | Veri kaynağı | Açıklama | Önerilen düzeltme|
|---|---|---|---|---|
| Düşük      | Özel uyarı-etkin bağlantı sayısı izin verilen aralığın dışında  | Aracı       | Belirli bir zaman penceresi içindeki etkin bağlantı sayısı, şu anda yapılandırılmış ve izin verilen aralığın dışında.|  Cihaz günlüklerini araştırın. Bağlantının nereden geldiğini öğrenin ve zararsız veya kötü amaçlı olup olmadığını öğrenin. Kötü amaçlı, olası kötü amaçlı yazılımları kaldırın ve kaynağı anlayın. Benign ise, kaynağı izin verilen bağlantı listesine ekleyin.  |
| Düşük      | Özel uyarı-izin verilmeyen bir IP 'ye giden bağlantı oluşturuldu                             | Aracı       | İzin verilen IP listenizin dışında bir IP 'ye giden bağlantı oluşturuldu. |Cihaz günlüklerini araştırın. Bağlantının nereden geldiğini öğrenin ve zararsız veya kötü amaçlı olup olmadığını öğrenin. Kötü amaçlı, olası kötü amaçlı yazılımları kaldırın ve kaynağı anlayın. Benign ise, kaynağı izin verilen IP listesine ekleyin.                        |
| Düşük      | Özel uyarı-başarısız yerel oturum açma sayısı izin verilen aralığın dışında                               | Aracı       | Belirli bir zaman penceresi içindeki başarısız olan yerel oturum açma miktarı, yapılandırılmış ve izin verilen aralığın dışında. |   |
| Düşük      | Özel uyarı-izin verilen kullanıcı listesinde olmayan bir kullanıcının oturum açması | Aracı       | İzin verilen Kullanıcı listenizin dışında, cihazda oturum açmış yerel bir kullanıcı.|  Ham verileri kaydediyorsanız, Log Analytics hesabınıza gidip cihazı araştırmak için verileri kullanın, kaynağı belirleyip, bu ayarlar için izin verme/engelleme listesini onarın. Şu anda ham verileri kaydettiğinizden cihaza gidin ve bu ayarlar için izin verme/engelleme listesini onarın.|
| Düşük      | Özel uyarı-izin verilmeyen bir işlem yürütüldü | Aracı       | Cihazda izin verilmeyen bir işlem yürütüldü. |Ham verileri kaydediyorsanız, Log Analytics hesabınıza gidip cihazı araştırmak için verileri kullanın, kaynağı belirleyip, bu ayarlar için izin verme/engelleme listesini onarın. Şu anda ham verileri kaydettiğinizden cihaza gidin ve bu ayarlar için izin verme/engelleme listesini onarın.  |
|

## <a name="next-steps"></a>Sonraki adımlar

- [Bir uyarıyı özelleştirmeyi](quickstart-create-custom-alerts.md) öğrenin
- IoT hizmetine [genel bakış](overview.md) için Defender
- [Güvenlik verilerinize erişme](how-to-security-data-access.md) hakkında bilgi edinin
- [Cihazı araştırma](how-to-investigate-device.md) hakkında daha fazla bilgi edinin
