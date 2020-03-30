---
title: IoT için Azure Güvenlik Merkezi'ni kullanarak verilere erişim| Microsoft Dokümanlar
description: IoT için Azure Güvenlik Merkezi'ni kullanırken güvenlik uyarı ve öneri verilerinize nasıl erişirken nasıl erişirken hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: fbd96ddd-cd9f-48ae-836a-42aa86ca222d
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 3ddd9b2c8373746a65cd78f0a81b60d097cd9f38
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68597186"
---
# <a name="access-your-security-data"></a>Güvenlik verilerinize erişin 

Azure Güvenlik Merkezi ioT, Günlük Analizi çalışma alanınızda güvenlik uyarılarını, önerileri ve ham güvenlik verilerini (kaydetmeyi seçerseniz) saklar.

## <a name="log-analytics"></a>Log Analytics

Hangi Log Analytics çalışma alanının kullanıldığını yapılandırmak için:

1. IoT hub'ınızı açın.
1. **Güvenlik** bölümünün altındaki **Genel Bakış** bıçağına tıklayın
2. **Ayarlar'ı**tıklatın ve Log Analytics çalışma alanı yapılandırmanızı değiştirin.

Yapılandırma sonrası Log Analytics çalışma alanınızda uyarılarınıza ve önerilerinize erişmek için:

1. IoT için Azure Güvenlik Merkezi'nde bir uyarı veya öneri seçin. 
2. **Daha fazla araştırma**yı tıklatın , sonra bu **uyarıya sahip hangi cihazların olduğunu görmek için buraya tıklayın ve DeviceId sütununa bakın.**

Log Analytics'ten veri sorgulama hakkında daha fazla bilgi için, [Log Analytics'teki sorgularla başlayın'](https://docs.microsoft.com//azure/log-analytics/query-language/get-started-queries)a bakın.

## <a name="security-alerts"></a>Güvenlik uyarıları

Güvenlik uyarıları, Azure Güvenlik Merkezi ioT çözümü için yapılandırılan Log Analytics çalışma alanında _AzureSecurityOfThings.SecurityAlert_ tablosunda depolanır.

Güvenlik uyarılarını keşfetmeye başlamanıza yardımcı olacak birkaç yararlı sorgu sağladık.

### <a name="sample-records"></a>Örnek kayıtlar

Birkaç rasgele kayıt seçin

```
// Select a few random records
//
SecurityAlert
| project 
    TimeGenerated, 
    IoTHubId=ResourceId, 
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity, 
    DisplayName,
    Description,
    ExtendedProperties
| take 3
```

| TimeGenerated           | IoTHubId                                                                                                       | DeviceId      | AlertSeverity | DisplayName                           | Açıklama                                             | Genişletilmiş Özellikler                                                                                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2018-11-18T18:10:29.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Yüksek          | Kaba kuvvet saldırısı başarılı oldu           | Cihaza bir Brute kuvvet saldırı başarılı oldu        |    { "Tam Kaynak\"Adresi": "[ 10.165.12.18:\"]",\"\""Kullanıcı Adları": "[ ]", "DeviceId": "IoT-Device-Linux" }                                                                       |
| 2018-11-19T12:40:31.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Yüksek          | Aygıtta başarılı yerel oturum açma      | Aygıta başarılı bir yerel giriş algılandı     | { "Uzak Adres": "?", "Uzak Bağlantı Noktası": "", "Yerel Bağlantı Noktası": "", "Giriş Kabuğu": "/bin/su", "Giriş İşlemid": "28207", "Kullanıcı Adı": "saldırgan", "DeviceId": "IoT-Device-Linux" } |
| 2018-11-19T12:40:31.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Yüksek          | Aygıtta başarısız yerel oturum açma girişimi  | Aygıtta başarısız bir yerel oturum açma girişimi algılandı |  { "Uzak Adres": "?", "Uzak Bağlantı Noktası": "", "Yerel Bağlantı Noktası": "", "Giriş Kabuğu": "/bin/su", "Giriş İşlemid": "22644", "Kullanıcı Adı": "saldırgan", "DeviceId": "IoT-Device-Linux" } |

### <a name="device-summary"></a>Cihaz özeti

Son bir hafta içinde algılanan, IoT Hub, cihaz, uyarı şiddeti, uyarı türüne göre gruplanan farklı güvenlik uyarılarının sayısını alın.

```
// Get the number of distinct security alerts detected in the last week, grouped by 
//   IoT hub, device, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| summarize Cnt=dcount(SystemAlertId) by
    IoTHubId=ResourceId, 
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity,
    DisplayName
```

| IoTHubId                                                                                                       | DeviceId      | AlertSeverity | DisplayName                           | Sayı |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|-----|
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Yüksek          | Kaba kuvvet saldırısı başarılı oldu           | 9   |   
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Orta        | Aygıtta başarısız yerel oturum açma girişimi  | 242 |    
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Yüksek          | Aygıtta başarılı yerel oturum açma      | 31  |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Orta        | Kripto Sikke Madenci                     | 4   |

### <a name="iot-hub-summary"></a>IoT hub özeti

Son bir hafta içinde Uyarıları olan bir dizi farklı aygıtı, IoT Hub, uyarı şiddeti, uyarı türü

```
// Select number of distinct devices which had alerts in the last week, by 
//   IoT hub, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| extend DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"])
| summarize CntDevices=dcount(DeviceId) by
    IoTHubId=ResourceId, 
    AlertSeverity,
    DisplayName
```

| IoTHubId                                                                                                       | AlertSeverity | DisplayName                           | CntCihazlar |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------------------------------|------------|
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Yüksek          | Kaba kuvvet saldırısı başarılı oldu           | 1          |    
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Orta        | Aygıtta başarısız yerel oturum açma girişimi  | 1          | 
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Yüksek          | Aygıtta başarılı yerel oturum açma      | 1          |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Orta        | Kripto Sikke Madenci                     | 1          |

## <a name="security-recommendations"></a>Güvenlik önerileri

Güvenlik önerileri _AzureSecurityOfThings.SecurityRecommendation_ tablosunda, Azure Güvenlik Merkezi ioT çözümü için yapılandırılan Log Analytics çalışma alanında depolanır.

Güvenlik önerilerini keşfetmeye başlamanıza yardımcı olmak için bir dizi yararlı sorgu sağladık.

### <a name="sample-records"></a>Örnek kayıtlar

Birkaç rasgele kayıt seçin

```
// Select a few random records
//
SecurityRecommendation
| project 
    TimeGenerated, 
    IoTHubId=AssessedResourceId, 
    DeviceId,
    RecommendationSeverity,
    RecommendationState,
    RecommendationDisplayName,
    Description,
    RecommendationAdditionalData
| take 2
```
    
| TimeGenerated | IoTHubId | DeviceId | TavsiyeÖnemderecesi | RecommendationState | TavsiyeDisplayName | Açıklama | ÖneriEk Veriler |
|---------------|----------|----------|------------------------|---------------------|---------------------------|-------------|------------------------------|
| 2019-03-22T10:21:06.060 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Orta | Etkin | Giriş zincirinde izin verilen güvenlik duvarı kuralı bulundu | Güvenlik duvarında çok çeşitli IP adresleri veya Bağlantı Noktaları için izin veren bir desen içeren bir kural bulundu | {"Rules":"[{\"Kaynak\"\"\"Adresi\":\"\"\",\"Kaynak\"\"\"Port\":\"\", Hedef\"Adres : , Hedef Port : 1337 }]"} |
| 2019-03-22T10:50:27.237 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Orta | Etkin | Giriş zincirinde izin verilen güvenlik duvarı kuralı bulundu | Güvenlik duvarında çok çeşitli IP adresleri veya Bağlantı Noktaları için izin veren bir desen içeren bir kural bulundu | {"Rules":"[{\"Kaynak\"\"\"Adresi\":\"\"\",\"Kaynak\"\"\"Port\":\"\", Hedef\"Adres : , Hedef Port : 1337 }]"} |

### <a name="device-summary"></a>Cihaz özeti

IoT Hub, aygıt, öneri önem derecesi ve türüne göre gruplanmış farklı etkin güvenlik önerilerinin sayısını alın.

```
// Get the number of distinct active security recommendations, grouped by by 
//   IoT hub, device, recommendation severity and type
//
SecurityRecommendation
| extend IoTHubId=AssessedResourceId
| summarize CurrentState=arg_max(RecommendationState, DiscoveredTimeUTC) by IoTHubId, DeviceId, RecommendationSeverity, RecommendationDisplayName
| where CurrentState == "Active"
| summarize Cnt=count() by IoTHubId, DeviceId, RecommendationSeverity
```

| IoTHubId                                                                                                       | DeviceId      | TavsiyeÖnemderecesi | Sayı |
|----------------------------------------------------------------------------------------------------------------|---------------|------------------------|-----|
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Yüksek          | 2   |    
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Orta        | 1 |  
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Yüksek          | 1  |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Orta        | 4   |


## <a name="next-steps"></a>Sonraki adımlar

- IoT [Genel Bakış](overview.md) için Azure Güvenlik Merkezi'ni okuyun
- IoT [Mimarisi](architecture.md) için Azure Güvenlik Merkezi hakkında bilgi edinin
- [IoT uyarıları için Azure Güvenlik Merkezi'ni](concept-security-alerts.md) anlayın ve keşfedin
- [IoT önerisi için Azure Güvenlik Merkezi'ni](concept-recommendations.md) anlayın ve keşfedin
