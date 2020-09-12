---
title: Azure RTOS için güvenlik modülü yerleşik & özelleştirilebilir uyarılar ve öneriler
description: Azure IoT güvenlik modülünü-RTOS kullanarak güvenlik uyarıları ve önerilen düzeltme hakkında bilgi edinin.
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
ms.openlocfilehash: ebb4edf10433cce981b4718835676d0a5d76ba40
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89514944"
---
# <a name="security-module-for-azure-rtos-security-alerts-and-recommendations-preview"></a>Azure RTOS güvenlik uyarıları ve önerileri için güvenlik modülü (Önizleme)

Azure RTOS için güvenlik modülü, olası kötü amaçlı etkinlikler ve şüpheli sistem değişiklikleri konusunda sizi uyarmak için gelişmiş analiz ve tehdit bilgilerini kullanarak IoT çözümünüzü sürekli olarak analiz eder. Ayrıca, beklenen cihaz davranışı ve temelleri hakkında bilginiz temelinde özel uyarılar da oluşturabilirsiniz.

Azure RTOS uyarısı için bir güvenlik modülü olası bir uzlaşmaya ilişkin bir gösterge olarak davranır ve araştırılması ve düzeltilmelidir. Azure RTOS önerisi için bir güvenlik modülü, düzeltilen ve güncellenen zayıf güvenlik duruşunu belirler. 

Bu makalede, varsayılan aralıklara göre tetiklenen yerleşik uyarıların ve önerilerin bir listesini, beklenen veya temel davranışa göre kendi değerlerinizle özelleştirilebilir bir liste bulacaksınız. 

IoT hizmeti için Azure Güvenlik Merkezi 'nde uyarı özelleştirmenin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [özelleştirilebilir uyarılar](concept-customizable-security-alerts.md). Azure RTOS için güvenlik modülünü kullanırken özelleştirme için kullanılabilen belirli uyarılar ve öneriler aşağıdaki tablolarda ayrıntılıdır. 

## <a name="security-module-for-azure-rtos-supported-security-alerts"></a>Azure RTOS tarafından desteklenen güvenlik uyarıları için güvenlik modülü

### <a name="device-related-security-alerts"></a>Cihazla ilgili güvenlik uyarıları

|Cihazla ilgili güvenlik uyarısı etkinliği  |Uyarı adı  |
|---------|---------|
|IP adresi| Şüpheli bir IP adresi ile iletişim algılandı|
|X. 509.440 cihaz sertifikası parmak izi|X. 509.440 cihaz sertifikası parmak izi uyuşmazlığı|
|X. 509.440 sertifikası| X. 509.440 sertifikasının geçerliliği geçildi|
|SAS belirteci| Süre dolsa belirteci|
|SAS belirteci| SAS belirteci imzası geçersiz|

### <a name="iot-hub-related-security-alerts"></a>IoT Hub ilişkili güvenlik uyarıları

|IoT Hub güvenlik uyarısı etkinliği  |Uyarı adı  |
|---------|---------|
|Sertifika ekle    |  IoT Hub sertifika ekleme girişimi başarısız oldu       |
|Tanılama ayarını ekleme veya düzenlememe    | Bir IoT Hub tanılama ayarı ekleme veya düzenleme girişimi algılandı      |
|Sertifikayı silme    |  IoT Hub bir sertifikayı silme girişimi başarısız oldu       |
|Tanılama ayarını Sil    |  IoT Hub bir tanılama ayarını silme girişimi algılandı      |
|Sertifika silindi    | IoT Hub bir sertifikayı silme algılandı        |
|Yeni sertifika     |  Yeni sertifikanın bir IoT Hub eklenmesi algılandı       |

## <a name="security-module-for-azure-rtos-supported-customizable-alerts"></a>Azure RTOS tarafından desteklenen özelleştirilebilir uyarılar için güvenlik modülü

### <a name="device-related-customizable-alerts"></a>Cihazla ilgili özelleştirilebilir uyarılar

|Cihazla ilgili etkinlik |Uyarı adı  |
|---------|---------|
|Etkin bağlantılar|Etkin bağlantı sayısı izin verilen aralıkta değil|
|**MQTT** protokolünde buluttan cihaza iletiler|**MQTT** protokolünde cihaz iletilerinin bulut sayısı izin verilen aralıkta değil|
|Giden bağlantı| İzin verilmeyen bir IP 'ye giden bağlantı|

### <a name="hub-related-customizable-alerts"></a>Hub ile ilgili özelleştirilebilir uyarılar 

|Hub ile ilgili etkinlik  |Uyarı adı  |
|---------|---------|
|Komut sırası temizler     |  Komut kuyruğu sayısı izin verilen aralığın dışında temizler       |
|**MQTT** protokolünde buluttan cihaza iletiler    |  İzin verilen aralığın dışında **MQTT** protokolündeki cihaz Iletilerine yönelik bulut sayısı       |
|**MQTT** protokolünde cihazdan buluta iletiler    | İzin verilen aralığın dışında **MQTT** protokolünde bulunan bulut iletilerine ait cihaz sayısı        |
|Doğrudan yöntem çağırır     |  İzin verilen aralığın dışında doğrudan yöntem çağıran sayısı       |
|**MQTT** protokolündeki buluttan cihaza ileti iletileri reddedildi     |   İzin verilen aralığın dışında **MQTT** protokolündeki cihaz iletilerine reddedilen bulut sayısı      |
|İkizi modülleri güncelleştirmeleri     |  İzin verilen Aralık dışında ikizi modüllerine yönelik güncelleştirme sayısı       |
|Yetkisiz işlemler    |  İzin verilen aralığın dışında yetkisiz işlem sayısı       |

## <a name="security-module-for-azure-rtos-supported-recommendations"></a>Azure RTOS tarafından desteklenen öneriler için güvenlik modülü

### <a name="device-related-recommendations"></a>Cihazla ilgili öneriler

|Cihazla ilgili etkinlik  |Öneri adı |
|---------|---------|
|Kimlik doğrulama kimlik bilgileri    |  Birden çok cihaz tarafından kullanılan özdeş kimlik doğrulama kimlik bilgileri       |

### <a name="hub-related-recommendations"></a>Hub ile ilgili öneriler

|IoT Hub ilgili etkinlik  |Öneri adı |
|---------|---------|
|IP filtresi ilkesi   |  Varsayılan IP filtresi ilkesi **Reddet** olarak ayarlanmalıdır  |
|IP filtre kuralı| IP filtresi kuralı, büyük bir IP aralığı içerir|
|Tanılama günlükleri|IoT Hub tanılama günlüklerini etkinleştirme önerisi|

### <a name="all-azure-security-center-for-iot-alerts-and-recommendations"></a>IoT uyarıları ve önerileri için tüm Azure Güvenlik Merkezi

IoT hizmeti ile ilgili uyarılar ve öneriler için tüm Azure Güvenlik Merkezi listesi için bkz. IoT [güvenlik uyarıları](concept-security-alerts.md), IoT güvenlik [önerileri](concept-recommendations.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlangıç: Azure RTOS için güvenlik modülü](quickstart-azure-rtos-security-module.md)
- [Azure RTOS için güvenlik modülünü yapılandırma ve özelleştirme](how-to-azure-rtos-security-module.md)
- [Azure RTOS API 'Sinin güvenlik modülüne](azure-rtos-security-module-api.md) bakın