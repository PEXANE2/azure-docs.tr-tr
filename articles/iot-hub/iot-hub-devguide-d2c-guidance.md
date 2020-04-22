---
title: Azure IoT Hub aygıt-bulut seçenekleri | Microsoft Dokümanlar
description: Geliştirici kılavuzu - buluttan buluta iletilerin ne zaman kullanılacağı, bildirilen özellikler veya buluttan aygıta iletişimiçin dosya yüklemenin ne zaman kullanılacağı na ilişkin kılavuz.
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
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733224"
---
# <a name="device-to-cloud-communications-guidance"></a>Aygıt-bulut iletişim kılavuzu

IoT Hub, aygıt uygulamasından çözümün arka ucuna bilgi gönderirken üç seçeneği ortaya çıkarır:

* Zaman serisi telemetri ve uyarılar için [aygıttan buluta iletiler.](iot-hub-devguide-messages-d2c.md)

* [Aygıt ikizinin,](iot-hub-devguide-device-twins.md) kullanılabilir özellikler, koşullar veya uzun süren iş akışlarının durumu gibi aygıt durumu bilgilerini raporlamak için bildirdiği özellikler. Örneğin, yapılandırma ve yazılım güncelleştirmeleri.

* Ortam dosyaları ve bant genişliğini kaydetmek için aralıklı olarak bağlanan aygıtlar tarafından yüklenen veya sıkıştırılan büyük telemetri toplu iş için [dosya yüklemeleri.](iot-hub-devguide-file-upload.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Burada çeşitli aygıttan buluta iletişim seçeneklerinin ayrıntılı bir karşılaştırmasI verilmiştir.

|  | Cihazdan buluta iletiler | Aygıt ikizinin bildirilen özellikleri | Dosya karşıya yüklemeleri |
| ---- | ------- | ---------- | ---- |
| Senaryo | Telemetri zaman serisi ve uyarıları. Örneğin, 256-KB sensör veri toplu her 5 dakikada bir gönderildi. | Mevcut yetenekler ve koşullar. Örneğin, hücresel veya WiFi gibi geçerli aygıt bağlantı modu. Yapılandırma ve yazılım güncelleştirmeleri gibi uzun süren iş akışlarını eşitleme. | Medya dosyaları. Büyük (genellikle sıkıştırılmış) telemetri toplu iş. |
| Depolama ve geri alma | IoT Hub tarafından 7 güne kadar geçici olarak saklanır. Sadece sıralı okuma. | IoT Hub tarafından aygıt ikizinde saklanır. [IoT Hub sorgu dilini](iot-hub-devguide-query-language.md)kullanarak alınılabilir. | Kullanıcı tarafından sağlanan Azure Depolama hesabında depolanır. |
| Boyut | 256 KB'ye kadar mesaj. | Bildirilen maksimum özellik boyutu 32 KB'dir. | Azure Blob Depolama tarafından desteklenen maksimum dosya boyutu. |
| Frequency | Yüksek. Daha fazla bilgi için [IoT Hub sınırlarına](iot-hub-devguide-quotas-throttling.md)bakın. | Orta. Daha fazla bilgi için [IoT Hub sınırlarına](iot-hub-devguide-quotas-throttling.md)bakın. | Düşük. Daha fazla bilgi için [IoT Hub sınırlarına](iot-hub-devguide-quotas-throttling.md)bakın. |
| Protokol | Tüm protokollerde mevcuttur. | MQTT veya AMQP kullanılarak kullanılabilir. | Herhangi bir protokol kullanırken kullanılabilir, ancak aygıtta HTTPS gerektirir. |

Bir uygulamanın hem telemetri zaman serisi olarak hem de uyarı olarak bilgi göndermesi ve aygıtın ikizinde kullanılabilir hale getirmesi gerekebilir. Bu senaryoda, aşağıdaki seçeneklerden birini seçebilirsiniz:

* Cihaz uygulaması aygıttan buluta ileti gönderir ve özellik değişikliğini bildirir.
* Çözüm arka uç, iletiyi aldığında bilgileri aygıt ikizinin etiketlerinde depolayabilir.

Aygıttan buluta iletiler, aygıt ikiz güncelleştirmelerinden çok daha yüksek bir iş elde edilmesini sağladığından, bazen aygıt ikizini her aygıttan buluta ileti için güncelleştirmekten kaçınmak istenir.
