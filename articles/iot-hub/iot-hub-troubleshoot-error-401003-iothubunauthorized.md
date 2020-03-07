---
title: Azure IoT Hub hata giderme hatası 401003 Iothubyetkilendirilmemiş
description: 401003 Iothubyetkilendirilmemiş hatasını nasıl düzelteceğinizi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: d0a9f643516af5945037acb3dd1da24b06944171
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78396483"
---
# <a name="401003-iothubunauthorized"></a>401003 IoTHubUnauthorized

Bu makalede, **401003 Iothubyetkilendirilmemiş** hatalara yönelik nedenler ve çözümler açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

### <a name="symptom-1"></a>Belirti 1

Tanılama günlükleri ' nde, **401003 Iothubyetkilendirilmemiş**, sonrasında **404104 Deviceconnectioncloseduzaktan**ve sonra kısa bir süre sonra başarılı bir şekilde bağlantı kurarak cihazların bir modelini görürsünüz.

### <a name="symptom-2"></a>Belirti 2

IoT Hub istekleri aşağıdaki hata iletilerinden biriyle başarısız olur:

* Yetkilendirme üstbilgisi eksik
* Iothub '\*', belirtilen '\*' cihazını içermiyor
* '\*' yetkilendirme kuralı '\*' erişimine izin vermiyor
* Bu cihaz için kimlik doğrulaması başarısız oldu, belirteci veya sertifikayı yenileyip yeniden bağlayın
* Parmak izi yapılandırma ile eşleşmiyor: Parmak Izi: SHA1Hash =\*, SHA2Hash =\*; Yapılandırma: Primaryparmak Izi =\*, Secondaryparmak Izi =\*

## <a name="cause"></a>Nedeni

### <a name="cause-1"></a>Neden 1

MQTT için bazı SDK 'lar, bir kilit süresinin ne zaman yenileneceğini bilmek için SAS belirtecinin süresi dolarsa bağlantıyı kesmek için IoT Hub kullanır. Nedenle 

1. SAS belirtecinin süresi doluyor
1. IoT Hub sona erme süresini fark eder ve cihazın bağlantısını **401003 Iothubyetkilendirilmemiş** ile keser
1. Cihaz, **404104 Deviceconnectioncloseduzaktan** bağlantı kesmeyi tamamlar
1. IoT SDK yeni bir SAS belirteci oluşturur
1. Cihaz başarıyla IoT Hub yeniden bağlanır

### <a name="cause-2"></a>Neden 2

IoT Hub auth üstbilgisinin, kuralın veya anahtarın kimliğini doğrulayamadı.

## <a name="solution"></a>Çözüm

### <a name="solution-1"></a>Çözüm 1

Cihaz bağlantı dizesini kullanarak bağlantı için IoT SDK kullanıyorsanız hiçbir işlem yapmanız gerekmez. IoT SDK, SAS belirteci süre sonuna yeniden bağlanmak için yeni belirteci yeniden oluşturur. 

Hata hacmi bir sorun oluşturacaksa, SAS belirtecini sona ermeden önce yenilediğinde C SDK 'sına geçin. Ayrıca, AMQP için SAS belirtecinin bağlantısı kesilmeden yenilenebilirler.

### <a name="solution-2"></a>Çözüm 2

Genel olarak, sunulan hata iletisinde hatanın nasıl düzeltileceğini açıklamalıdır. Bazı nedenlerle hata iletisi ayrıntısına erişiminiz yoksa şunları yaptığınızdan emin olun:

- Kullandığınız SAS veya diğer güvenlik belirtecinin geçerliliği dolmamıştır. 
- Yetkilendirme kimlik bilgileri, kullandığınız protokol için doğru şekilde biçimlendirilir. Daha fazla bilgi için bkz. [IoT Hub Access Control](iot-hub-devguide-security.md).
- Kullanılan yetkilendirme kuralı istenen işlem için izne sahip.

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub kimlik doğrulamasını kolaylaştırmak için [Azure IoT SDK](iot-hub-devguide-sdks.md)'larını kullanmanızı öneririz.