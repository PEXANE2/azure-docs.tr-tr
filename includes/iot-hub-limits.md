---
author: robinsh
ms.author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 34de38e91d47457d215c7ebf65d04ed2dbae5324
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74224538"
---
The following table lists the limits associated with the different service tiers S1, S2, S3, and F1. For information about the cost of each *unit* in each tier, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).

| Kaynak | S1 Standart | S2 Standart | S3 Standart | F1 Ücretsiz |
| --- | --- | --- | --- | --- |
| İleti/gün |400,000 |6,000,000 |300,000,000 |8,000 |
| En fazla birim |200 |200 |10 |1 |

> [!NOTE]
> If you anticipate using more than 200 units with an S1 or S2 tier hub or 10 units with an S3 tier hub, contact Microsoft Support.
> 
> 

The following table lists the limits that apply to IoT Hub resources.

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına en fazla ücretli IoT hub’ı sayısı |100 |
| Azure aboneliği başına en fazla ücretsiz IoT hub’ı sayısı |1 |
| Maximum number of characters in a device ID | 128 |
| En fazla cihaz kimliği sayısı<br/> (tek bir çağrıda döndürülen) |1000 |
| Cihazdan buluta iletiler için IoT Hub en fazla ileti bekletme süresi |7 gün |
| Cihazdan bulut iletinin en büyük boyutu |256 KB |
| Cihazdan buluta toplu işin en büyük boyutu |AMQP and HTTP: 256 KB for the entire batch <br/>MQTT: 256 KB for each message |
| Cihazdan buluta toplu işte en fazla ileti sayısı |500 |
| Buluttan cihaza iletinin en büyük boyutu |64 KB |
| Buluttan cihaza iletiler için en büyük TTL |2 gün |
| Buluttan cihaza iletiler için en fazla teslim <br/> sayısı |100 |
| Maximum cloud-to-device queue depth per device |50 |
| Geri bildirim iletileri için en fazla teslim sayısı <br/> (buluttan cihaza iletiye yanıt olarak) |100 |
| Buluttan cihaza iletiye yanıt olarak <br/> geri bildirim iletileri için en fazla TTL |2 gün |
| [Maximum size of device twin](../articles/iot-hub/iot-hub-devguide-device-twins.md#device-twin-size) | 8 KB for tags section, and 32 KB for desired and reported properties sections each |
| Maximum length of device twin string key | 1 KB |
| Maximum length of device twin string value | 4 KB |
| [Maximum depth of object in device twin](../articles/iot-hub/iot-hub-devguide-device-twins.md#tags-and-properties-format) | 10 |
| Doğrudan yöntem yükünün en büyük boyutu | 128 KB |
| En büyük iş geçmişi bekletme süresi | 30 gün |
| En fazla eşzamanlı iş sayısı | 10 (S3 için), 5 (S2 için), 1 (S1 için) |
| En fazla ek uç nokta sayısı | 10 (for S1, S2, and S3) |
| En fazla ileti yönlendirme kuralı sayısı | 100 (for S1, S2, and S3) |
| Maximum number of concurrently connected device streams | 50 (for S1, S2, S3, and F1 only) |
| Maximum device stream data transfer | 300 MB per day (for S1, S2, S3, and F1 only) |

> [!NOTE]
> If you need more than 100 paid IoT hubs in an Azure subscription, contact Microsoft Support.

> [!NOTE]
> Currently, the total number of devices plus modules that can be registered to a single IoT hub is capped at 1,000,000. If you want to increase this limit, contact [Microsoft Support](https://azure.microsoft.com/support/options/).

IoT Hub throttles requests when the following quotas are exceeded.

| Kısıtlama | Hub başına değer |
| --- | --- |
| Kimlik kayıt defteri işlemleri <br/> (create, retrieve, list, update, and delete), <br/> tek tek veya toplu içeri/dışarı aktarma |83.33/sec/unit (5,000/min/unit) (for S3). <br/> 1.67/sec/unit (100/min/unit) (for S1 and S2). |
| Cihaz bağlantıları |6,000/sec/unit (for S3), 120/sec/unit (for S2), 12/sec/unit (for S1). <br/>En az 100/sn. |
| Cihazdan buluta gönderim |6,000/sec/unit (for S3), 120/sec/unit (for S2), 12/sec/unit (for S1). <br/>En az 100/sn. |
| Buluttan cihaza gönderim | 83.33/sec/unit (5,000/min/unit) (for S3), 1.67/sec/unit (100/min/unit) (for S1 and S2). |
| Buluttan cihaza alım |833.33/sec/unit (50,000/min/unit) (for S3), 16.67/sec/unit (1,000/min/unit) (for S1 and S2). |
| Dosya karşıya yükleme işlemleri |83.33 file upload initiations/sec/unit (5,000/min/unit) (for S3), 1.67 file upload initiations/sec/unit (100/min/unit) (for S1 and S2). <br/> 10,000 SAS URIs can be out for an Azure Storage account at one time.<br/> Tek seferde 10 SAS URI’si/cihaz çıkarılabilir. |
| Doğrudan yöntemler | 24 MB/sec/unit (for S3), 480 KB/sec/unit (for S2), 160 KB/sec/unit (for S1).<br/> Based on 8-KB throttling meter size. |
| Cihaz ikizi okumaları | 500/sec/unit (for S3), Maximum of 100/sec or 10/sec/unit (for S2), 100/sec (for S1) |
| Cihaz ikizi güncelleştirmeleri | 250/sec/unit (for S3), Maximum of 50/sec or 5/sec/unit (for S2), 50/sec (for S1) |
| İş işlemleri <br/> (create, update, list, and delete) | 83.33/sec/unit (5,000/min/unit) (for S3), 1.67/sec/unit (100/min/unit) (for S2), 1.67/sec/unit (100/min/unit) (for S1). |
| İşler işlemleri için cihaz başına aktarım hızı | 50/sec/unit (for S3), maximum of 10/sec or 1/sec/unit (for S2), 10/sec (for S1). |
| Device stream initiation rate | 5 new streams/sec (for S1, S2, S3, and F1 only). |
