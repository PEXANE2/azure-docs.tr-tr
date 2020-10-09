---
title: Güvenlik Aracısı yerel yapılandırması (C)
description: C için aracı yerel yapılandırmalarına yönelik Defender hakkında bilgi edinin.
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
ms.date: 07/26/2019
ms.author: mlottner
ms.openlocfilehash: 46b3009a7a57f726457a767afe501bfb28a06606
ms.sourcegitcommit: b87c7796c66ded500df42f707bdccf468519943c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91843371"
---
# <a name="understanding-the-localconfigurationjson-file---c-agent"></a>LocalConfiguration.json dosyasını anlama - C aracısı

IoT güvenlik Aracısı için Defender, yerel bir yapılandırma dosyasından yapılandırmaları kullanır.
Güvenlik Aracısı, aracıyı başlatma sırasında yapılandırmayı bir kez okur.
Yerel yapılandırma dosyasında bulunan yapılandırma, kimlik doğrulama yapılandırması ve aracıyla ilgili diğer yapılandırmaları içerir.
Dosya, JSON gösteriminde "anahtar-değer" çiftleri içinde yapılandırma içerir ve aracı yüklendiğinde yapılandırma doldurulur.

Varsayılan olarak, dosya şu konumda bulunur:/var/ASCIoTAgent/LocalConfiguration.js

Yapılandırma dosyasındaki değişiklikler aracı yeniden başlatıldığında gerçekleşir.

## <a name="security-agent-configurations-for-c"></a>C için güvenlik Aracısı yapılandırması

| Yapılandırma adı | Olası değerler | Ayrıntılar |
|:-----------|:---------------|:--------|
| AgentId | GUID | Aracı benzersiz tanımlayıcısı |
| TriggerdEventsInterval | ISO8601 dizesi | Tetiklenen olaylar koleksiyonu için Zamanlayıcı aralığı |
| ConnectionTimeout | ISO8601 dizesi | Iothub bağlantısı zaman aşımına uğramadan önce geçen süre |
| Kimlik Doğrulaması | JsonObject | Kimlik doğrulama yapılandırması. Bu nesne, ıothub 'e göre kimlik doğrulaması için gereken tüm bilgileri içerir |
| Kimlik | "DPS", "SecurityModule", "cihaz" | Kimlik doğrulama kimliği-DPS, kimlik doğrulaması, cihaz kimlik bilgileriyle kimlik doğrulama yapılırsa güvenlik modülü kimlik bilgileri veya cihaz aracılığıyla yapılırsa SecurityModule |
| AuthenticationMethod | "SasToken", "SelfSignedCertificate" | kimlik doğrulaması için Kullanıcı parolası-gizli anahtar kullan seçeneği bir simetrik anahtaralıysa, gizli dizi otomatik olarak imzalanan bir sertifika ise, otomatik olarak imzalanan sertifika seçin  |
| Null | Dosya yolu (dize) | Kimlik doğrulama parolasını içeren dosyanın yolu |
| HostName | string | Azure IoT Hub 'ının konak adı. genellikle <-hub>. azure-devices.net |
| DeviceId | string | Cihazın KIMLIĞI (Azure IoT Hub kayıtlı olarak) |
| DPS | JsonObject | DPS ilgili yapılandırma |
| IDScope | string | DPS KIMLIK kapsamı |
| RegistrationId | string  | DPS cihaz kayıt KIMLIĞI |
| Günlüğe Kaydetme | JsonObject | Aracı günlükçüsü ile ilgili yapılandırma |
| Systemloggerminimumönem derecesi | 0 <= sayı <= 4 | Bu önem derecesine sahip ve yukarıdaki günlük iletileri,/var/log/Syslog dizinine kaydedilir (en düşük önem derecesi 0 ' dır) |
| DiagnosticEventMinimumSeverity | 0 <= sayı <= 4 | Bu önem derecesine sahip olan ve yukarıdaki günlük iletileri, tanılama olayları olarak gönderilir (en düşük önem derecesi 0 ' dır) |

## <a name="security-agent-configurations-code-example"></a>Güvenlik Aracısı yapılandırma kodu örneği

```json
{
    "Configuration" : {
        "AgentId" : "b97faf0a-0f57-471f-9dab-46a8e1764946",
        "TriggerdEventsInterval" : "PT2M",
        "ConnectionTimeout" : "PT30S",
        "Authentication" : {
            "Identity" : "Device",
            "AuthenticationMethod" : "SasToken",
            "FilePath" : "/path/to/my/SymmetricKey",
            "DeviceId" : "my-device",
            "HostName" : "my-iothub.azure-devices.net",
            "DPS" : {
                "IDScope" : "",
                "RegistrationId" : ""
            }
        },
        "Logging": {
            "SystemLoggerMinimumSeverity": 0,
            "DiagnoticEventMinimumSeverity": 2
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- IoT hizmetine [genel bakış](overview.md) Için Defender 'ı okuyun
- IoT [mimarisi](architecture.md) için Defender hakkında daha fazla bilgi edinin
- IoT [hizmeti](quickstart-onboard-iot-hub.md) Için Defender 'ı etkinleştirin
- IoT hizmeti için Defender 'ı okuyun [SSS](resources-frequently-asked-questions.md)
- [Ham güvenlik verilerine](how-to-security-data-access.md) erişme hakkında bilgi edinin
- [Önerileri](concept-recommendations.md) anlama
- Güvenlik [uyarılarını](concept-security-alerts.md) anlama
