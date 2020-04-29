---
title: Azure IoT Hub cihazdan buluta seçenekleri | Microsoft Docs
description: Geliştirici Kılavuzu-cihazdan buluta iletileri, bildirilen özellikleri veya buluttan cihaza iletişimler için karşıya dosya yükleme işleminin ne zaman kullanılacağı konusunda rehberlik.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: b984ee7ed662bf089e7bcb6fc7e948fb61ed1209
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81733224"
---
# <a name="device-to-cloud-communications-guidance"></a>Cihazdan buluta iletişim Kılavuzu

Cihaz uygulamasından çözüm arka ucuna bilgi gönderirken IoT Hub üç seçenek sunar:

* Zaman serisi telemetrisi ve uyarıları için [cihazdan buluta iletiler](iot-hub-devguide-messages-d2c.md) .

* [Cihaz ikizi](iot-hub-devguide-device-twins.md) , kullanılabilir özellikler, koşullar veya uzun süre çalışan iş akışlarının durumu gibi raporlama cihazı durum bilgilerini raporlamak için bildirilen özelliklerdir. Örneğin, yapılandırma ve yazılım güncelleştirmeleri.

* [Dosya yüklemeleri](iot-hub-devguide-file-upload.md) , ara sıra bağlı cihazlar tarafından yüklenen veya bant genişliği tasarrufu için sıkıştırılan büyük telemetri toplu işleri.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Çeşitli cihazdan buluta iletişim seçeneklerinin ayrıntılı bir karşılaştırması aşağıda verilmiştir.

|  | Cihazdan buluta iletiler | Cihaz ikizi bildirilen özellikleri | Dosya karşıya yüklemeleri |
| ---- | ------- | ---------- | ---- |
| Senaryo | Telemetri süresi serisi ve uyarıları. Örneğin, her 5 dakikada bir 256 KB 'lık algılayıcı veri toplu işi gönderilir. | Kullanılabilir yetenekler ve koşullar. Örneğin, hücresel veya WiFi gibi geçerli cihaz bağlantısı modu. Yapılandırma ve yazılım güncelleştirmeleri gibi uzun süre çalışan iş akışlarını eşitleme. | Medya dosyaları. Büyük (genellikle sıkıştırılmış) telemetri toplu işleri. |
| Depolama ve alma | En fazla 7 güne kadar IoT Hub tarafından geçici olarak depolanır. Yalnızca sıralı okuma. | Cihaz ikizi IoT Hub tarafından depolanır. [IoT Hub sorgu dili](iot-hub-devguide-query-language.md)kullanılarak alınabilir. | Kullanıcı tarafından belirtilen Azure depolama hesabında depolanır. |
| Boyut | En çok 256 KB ileti. | Raporlanan en fazla özellik boyutu 32 KB 'tır. | Azure Blob depolama tarafından desteklenen en büyük dosya boyutu. |
| Frequency | Yüksek. Daha fazla bilgi için bkz. [IoT Hub sınırları](iot-hub-devguide-quotas-throttling.md). | Orta. Daha fazla bilgi için bkz. [IoT Hub sınırları](iot-hub-devguide-quotas-throttling.md). | Düşük. Daha fazla bilgi için bkz. [IoT Hub sınırları](iot-hub-devguide-quotas-throttling.md). |
| Protokol | Tüm protokollerde kullanılabilir. | MQTT veya AMQP kullanılarak kullanılabilir. | Herhangi bir protokol kullanılırken kullanılabilir, ancak cihazda HTTPS gerekir. |

Uygulamanın hem telemetri süresi serisi hem de uyarı olarak bilgi gönderebilmesi ve cihazı cihaz ikizi kullanılabilir hale getirmek gerekebilir. Bu senaryoda, aşağıdaki seçeneklerden birini belirleyebilirsiniz:

* Cihaz uygulaması bir cihazdan buluta ileti gönderir ve bir özellik değişikliğini raporlar.
* Çözüm arka ucu, iletiyi alırken bilgileri Device ikizi 'in etiketlerinde saklayabilir.

Cihazdan buluta iletiler, Device ikizi güncelleştirmelerinden çok daha yüksek bir aktarım hızını olanaklı hale yaptığından, her cihazdan buluta ileti için Device ikizi ' yi güncelleştirmemek bazen tercih edilir.
