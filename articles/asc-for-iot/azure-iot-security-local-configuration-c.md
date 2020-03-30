---
title: C için aracı yerel yapılandırmaları için Azure Güvenlik Merkezi'ni anlama | Microsoft Dokümanlar
description: C için aracı yerel yapılandırmaları için Azure Güvenlik Merkezi hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2019
ms.author: mlottner
ms.openlocfilehash: 2725a824da26dafcbc215e4c302ec38ad4b5a699
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68600546"
---
# <a name="understanding-the-localconfigurationjson-file---c-agent"></a>LocalConfiguration.json dosyasını anlama - C aracısı

Azure Güvenlik Merkezi ioT güvenlik aracısı için yerel bir yapılandırma dosyasından yapılandırmalar kullanır.
Güvenlik aracısı, aracı nın başlatılmasında yapılandırmayı bir kez okur.
Yerel yapılandırma dosyasında bulunan yapılandırma kimlik doğrulama yapılandırması ve diğer aracıyla ilgili yapılandırmaları içerir.
Dosya, JSON gösterimindeki "Anahtar Değer" çiftlerinde yapılandırmalar içerir ve aracı yüklendiğinde yapılandırmalar doldurulur. 

Varsayılan olarak, dosya şu adreste bulunur: /var/ASCIoTAgent/LocalConfiguration.json

Aracı yeniden başlatıldığında yapılandırma dosyasında değişiklikler gerçekleşir. 

## <a name="security-agent-configurations-for-c"></a>C için güvenlik aracısı yapılandırmaları
| Yapılandırma Adı | Olası değerler | Ayrıntılar | 
|:-----------|:---------------|:--------|
| AgentId | GUID | Aracı Benzersiz tanımlayıcı |
| TriggerdEventsInterval | ISO8601 dize | Tetiklenen olaylar koleksiyonu için zamanlayıcı aralığı |
| Connectiontimeout | ISO8601 dize | IoThub bağlantısızamansız hale gelmeden önceki zaman dilimi |
| Kimlik doğrulaması | JsonNesne | Kimlik doğrulama yapılandırması. Bu nesne, IoTHub'a karşı kimlik doğrulama için gereken tüm bilgileri içerir |
| Kimlik | "DPS", "Güvenlik Modülü", "Aygıt" | Kimlik doğrulama kimliği - DPS üzerinden kimlik doğrulama yapılıyorsa DPS, güvenlik modülü kimlik bilgileri veya aygıt üzerinden kimlik doğrulama sı aygıtla gerçekse Güvenlik Modülü |
| Authenticationmethod | "SasToken", "SelfSignedCertificate" | kimlik doğrulama için kullanıcı sırrı - Kullanım sırrı Simetrik bir anahtarsa SasToken'ı seçin, gizli kendi imzalı bir sertifikaysa kendi imzalı sertifikayı seçin  |
| Filepath | Dosyaya giden yol (dize) | Kimlik doğrulama sırrını içeren dosyaya giden yol |
| Ana bilgisayar adı | string | Azure iot hub'ının ana bilgisayar adı. genellikle <benim-hub>.azure-devices.net |
| DeviceId | string | Aygıtın kimliği (Azure IoT Hub'ında kayıtlı olarak) |
| Dps | JsonNesne | DPS ile ilgili yapılandırmalar |
| IDScope | string | DPS'nin kimlik kapsamı |
| RegistrationId | string  | DPS cihaz kayıt kimliği |
| Günlüğe Kaydetme | JsonNesne | Aracı logger ilgili yapılandırmaları |
| SystemLoggerMinimumŞiddet | 0 <= <sayısı = 4 | günlük iletileri eşit ve bu önemin üzerinde /var/log/syslog 'a kaydedilir (0 en düşük önem derecesidir) |
| TeşhisEventMinimumSeverity | 0 <= <sayısı = 4 | günlük iletileri eşit ve bu önemin üzerinde tanılama olayları olarak gönderilecektir (0 en düşük önem derecesidir) |

## <a name="security-agent-configurations-code-example"></a>Güvenlik aracısı yapılandırmaları kod örneği
```JSON
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
- IoT hizmetine [Genel Bakış](overview.md) için Azure Güvenlik Merkezi'ni okuyun
- IoT [Mimarisi](architecture.md) için Azure Güvenlik Merkezi hakkında daha fazla bilgi edinin
- IoT [hizmeti](quickstart-onboard-iot-hub.md) için Azure Güvenlik Merkezi'ni etkinleştirme
- IoT hizmeti [SSS](resources-frequently-asked-questions.md) için Azure Güvenlik Merkezi'ni okuyun
- Ham güvenlik [verilerine](how-to-security-data-access.md) nasıl erişin gerektiğini öğrenin
- [Önerileri](concept-recommendations.md) anlama
- Güvenlik [uyarılarını](concept-security-alerts.md) anlama