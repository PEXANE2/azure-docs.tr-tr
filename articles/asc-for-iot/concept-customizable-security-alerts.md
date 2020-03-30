---
title: IoT için Azure Güvenlik Merkezi için özelleştirilebilir güvenlik uyarı kılavuzu| Microsoft Dokümanlar
description: IoT özellikleri ve hizmetleri için Azure Güvenlik Merkezi'ni kullanarak özelleştirilebilir güvenlik uyarıları ve önerilen düzeltme hakkında bilgi edinin.
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
ms.date: 03/04/2020
ms.author: mlottner
ms.openlocfilehash: 32d79267559480de8ec91e5e66196bfd57e01cf1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299582"
---
# <a name="azure-security-center-for-iot-security-alerts"></a>IoT için Azure Güvenlik Merkezi güvenlik uyarıları

Azure Güvenlik Merkezi IoT, kötü amaçlı etkinlikler konusunda sizi uyarmak için gelişmiş analitik ve tehdit istihbaratını kullanarak IoT çözümünüzü sürekli olarak analiz eder.

Uyarıların benzersiz kuruluş dağıtımınızda ve ortamınızda potansiyel uzlaşmanın en etkili göstergeleri olarak hareket etmesini sağlamak için beklenen aygıt davranışı hakkındaki bilginize dayalı özel uyarılar oluşturmanızı öneririz. 

IoT uyarıları için Azure Güvenlik Merkezi'nin aşağıdaki listesi, beklenen IoT Hub'ınıza ve/veya aygıt davranışınıza bağlı olarak sizin tarafından tanımlanabilir. Her uyarının nasıl özelleştirilenhakkında daha fazla bilgi için [bkz.](quickstart-create-custom-alerts.md)

## <a name="iot-hub-alerts-available-for-customization"></a>Özelleştirme için kullanılabilir IoT Hub uyarıları 



| Severity | Uyarı adı | Veri kaynağı | Açıklama | Önerilen düzeltme|
|---|---|---|---|---|
| Düşük      | Özel uyarı - AMQP protokolündeki aygıt iletilerine bulut sayısı izin verilen aralığın dışındadır          | IoT Hub     | Belirli bir zaman penceresindeki aygıt iletilerine bulut sayısı (AMQP protokolü) şu anda yapılandırılmış ve izin verilen aralığın dışındadır.||
| Düşük      | Özel uyarı - AMQP protokolündeki aygıt iletilerine reddedilen bulut sayısı izin verilen aralığın dışındadır | IoT Hub     | Aygıt tarafından reddedilen aygıt iletilerine bulut sayısı (AMQP protokolü), belirli bir zaman penceresi içinde şu anda yapılandırılmış ve izin verilen aralığın dışındadır.||
| Düşük      | Özel uyarı - AMQP protokolündeki bulut iletilerine aygıt sayısı izin verilen aralığın dışındadır      | IoT Hub     | Belirli bir zaman penceresinde ki iletileri (AMQP protokolü) buluta ayarlayacak aygıt miktarı, şu anda yapılandırılmış ve izin verilen aralığın dışındadır.|   |
| Düşük      | Özel uyarı - doğrudan yöntem çağrılarının sayısı izin verilen aralığın dışında | IoT Hub     | Belirli bir zaman penceresi içinde çağrılan doğrudan yöntemin miktarı, şu anda yapılandırılmış ve izin verilen aralığın dışındadır.||
| Düşük      | Özel uyarı - dosya yükleme sayısı izin verilen aralığın dışında | IoT Hub     | Belirli bir zaman penceresindeki dosya yüklemelerinin miktarı, şu anda yapılandırılmış ve izin verilen aralığın dışındadır.| |
| Düşük      | Özel uyarı - HTTP protokolündeki aygıt iletilerine bulut sayısı izin verilen aralığın dışındadır | IoT Hub     | Bir zaman penceresindeki aygıt iletilerine (HTTP protokolü) bulut miktarı yapılandırılan izin verilen aralıkta değil                                  |
| Düşük      | Özel uyarı - HTTP protokolündeki aygıt iletilerine reddedilen bulut sayısı izin verilen aralıkta değil | IoT Hub     | Belirli bir zaman penceresindeki aygıt iletilerine (HTTP protokolü) bulut miktarı, şu anda yapılandırılmış ve izin verilen aralığın dışındadır. |
| Düşük      | Özel uyarı - HTTP protokolündeki iletileri buluta alan aygıt sayısı izin verilen aralığın dışındadır | IoT Hub| Belirli bir zaman penceresinde iletileri buluta ayarlayacak aygıt miktarı (HTTP protokolü) şu anda yapılandırılmış ve izin verilen aralığın dışındadır.|    |
| Düşük      | Özel uyarı - MQTT protokolündeki aygıt iletilerine bulut sayısı izin verilen aralığın dışındadır | IoT Hub     | Belirli bir zaman penceresindeki aygıt iletilerine (MQTT protokolü) bulut miktarı, şu anda yapılandırılmış ve izin verilen aralığın dışındadır.|   |
| Düşük      | Özel uyarı - MQTT protokolündeki aygıt iletilerine reddedilen bulut sayısı izin verilen aralığın dışındadır | IoT Hub     | Belirli bir zaman penceresi içinde aygıt tarafından reddedilen aygıt iletilerine (MQTT protokolü) bulut miktarı, şu anda yapılandırılan ve izin verilen aralığın dışındadır. |
| Düşük      | Özel uyarı - MQTT protokolündeki bulut iletilerine aygıt sayısı izin verilen aralığın dışındadır          | IoT Hub     | Belirli bir zaman penceresindeki bulut iletilerine (MQTT protokolü) aygıtın miktarı, şu anda yapılandırılmış ve izin verilen aralığın dışındadır.|
| Düşük      | Özel uyarı - komut sırası temizleme sayısı izin verilen aralığın dışında                               | IoT Hub     | Belirli bir zaman penceresindeki komut sırası temizleme miktarı, şu anda yapılandırılmış ve izin verilen aralığın dışındadır.||
| Düşük      | Özel uyarı - modül ikiz güncelleştirmelerinin sayısı izin verilen aralığın dışındadır                                       | IoT Hub     | Belirli bir zaman penceresindeki modül ikiz güncelleştirmelerinin miktarı, şu anda yapılandırılmış ve izin verilen aralığın dışındadır.|
| Düşük      | Özel uyarı - yetkisiz işlem sayısı izin verilen aralığın dışında  | IoT Hub     | Belirli bir zaman penceresindeki yetkisiz işlemlerin miktarı, şu anda yapılandırılmış ve izin verilen aralığın dışındadır.|
|

## <a name="agent-alerts-available-for-customization"></a>Özelleştirme için kullanılabilir aracı uyarıları 

| Severity | Uyarı adı | Veri kaynağı | Açıklama | Önerilen düzeltme|
|---|---|---|---|---|
| Düşük      | Özel uyarı - etkin bağlantı sayısı izin verilen aralığın dışında  | Aracı       | Belirli bir zaman penceresindeki etkin bağlantı sayısı, şu anda yapılandırılmış ve izin verilen aralığın dışındadır.|  Aygıt günlüklerini incele. Bağlantının nereden kaynaklandığını öğrenin ve iyi huylu veya kötü niyetli olup olmadığını belirleyin. Kötü amaçlı yazılım varsa, olası kötü amaçlı yazılımları kaldırın ve kaynağı anlayın. İyi huyluysa, kaynağı izin verilen bağlantı listesine ekleyin.  |
| Düşük      | Özel uyarı - IZIN verilmeyen bir IP'ye oluşturulan giden bağlantı                             | Aracı       | İzin verilen IP listenizin dışında bir IP'ye giden bağlantı oluşturuldu. |Aygıt günlüklerini incele. Bağlantının nereden kaynaklandığını öğrenin ve iyi huylu veya kötü niyetli olup olmadığını belirleyin. Kötü amaçlı yazılım varsa, olası kötü amaçlı yazılımları kaldırın ve kaynağı anlayın. İyi huyluysa, kaynağı izin verilen IP listesine ekleyin.                        |
| Düşük      | Özel uyarı - başarısız yerel giriş sayısı izin verilen aralığın dışında                               | Aracı       | Belirli bir zaman penceresinde ki başarısız yerel girişlerin miktarı, şu anda yapılandırılmış ve izin verilen aralığın dışındadır. |   |
| Düşük      | Özel uyarı - izin verilen kullanıcı listesinde olmayan bir kullanıcının girişi | Aracı       | İzin verilen kullanıcı listenizin dışında, aygıtta oturum açan yerel bir kullanıcı.|  Ham veri kaydediyorsanız, günlük analitiği hesabınıza gidin ve cihazı araştırmak, kaynağı belirlemek ve bu ayarlar için izin verme/engelleme listesini düzeltmek için verileri kullanın. Ham verileri şu anda kaydetmiyorsanız, aygıta gidin ve bu ayarlar için izin/engelleme listesini düzeltin.|
| Düşük      | Özel uyarı - izin verilmeyen bir işlem yürütüldü | Aracı       | Aygıtta izin verilmeyen bir işlem yürütüldü. |Ham veri kaydediyorsanız, günlük analitiği hesabınıza gidin ve cihazı araştırmak, kaynağı belirlemek ve bu ayarlar için izin verme/engelleme listesini düzeltmek için verileri kullanın. Ham verileri şu anda kaydetmiyorsanız, aygıta gidin ve bu ayarlar için izin/engelleme listesini düzeltin.  |
|

## <a name="next-steps"></a>Sonraki adımlar

- Uyarıyı nasıl [özelleştirinizi](quickstart-create-custom-alerts.md) öğrenin
- IoT hizmeti için Azure Güvenlik Merkezi [Genel Bakış](overview.md)
- Güvenlik verilerinize nasıl [erişilenleri](how-to-security-data-access.md) öğrenin
- [Bir aygıtı araştırma](how-to-investigate-device.md) hakkında daha fazla bilgi edinin