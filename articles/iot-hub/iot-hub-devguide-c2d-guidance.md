---
title: Azure IoT Hub'ın cihazdan cihaza bulut seçenekleri | Microsoft Dokümanlar
description: Geliştirici kılavuzu - doğrudan yöntemlerin ne zaman kullanılacağı, aygıt ikizinin istenen özellikleri veya buluttan cihaza iletişim için buluttan cihaza iletilerin ne zaman kullanılacağına ilişkin kılavuz.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: a2ca9a167d50619ed2963b13515c0a772d712570
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77591277"
---
# <a name="cloud-to-device-communications-guidance"></a>Bulut-aygıtiletişim kılavuzu

IoT Hub, aygıt uygulamalarının işlevselliği bir arka uç uygulamasına maruz bırakmaları için üç seçenek sunar:

* Sonucun hemen onaylanmasını gerektiren iletişim için [doğrudan yöntemler.](iot-hub-devguide-direct-methods.md) Doğrudan yöntemler genellikle fan açmak gibi cihazların etkileşimli kontrolü için kullanılır.

* Uzun süreli komutlar için [Twin'in istenen özellikleri,](iot-hub-devguide-device-twins.md) aygıtı belirli bir istenilen duruma sokmayı amaçlamaktadır. Örneğin, telemetri gönderme aralığını 30 dakikaolarak ayarlayın.

* Cihaz uygulamasına tek yönlü bildirimler için [buluttan cihaza iletiler.](iot-hub-devguide-messages-c2d.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Burada çeşitli buluttan cihaza iletişim seçeneklerinin ayrıntılı bir karşılaştırmasI verilmiştir.

|  | Doğrudan yöntemler | İkiz'in istenilen özellikleri | Buluttan cihaza iletiler |
| ---- | ------- | ---------- | ---- |
| Senaryo | Bir fanı açmak gibi hemen onay gerektiren komutlar. | Aygıtı belirli bir istenilen duruma sokmayı amaçlayan uzun süreli komutlar. Örneğin, telemetri gönderme aralığını 30 dakikaolarak ayarlayın. | Cihaz uygulamasına tek yönlü bildirimler. |
| Veri akışı | Iki yönlü. Cihaz uygulaması yönteme hemen yanıt verebilir. Çözüm arka uç, sonucu isteğe bağlamsal olarak alır. | Tek yönlü. Cihaz uygulaması özellik değişikliği ile ilgili bir bildirim alır. | Tek yönlü. Cihaz uygulaması iletiyi alır
| Dayanıklılık | Bağlantısı kesilen aygıtlar ile bağlantı kesilmez. Çözüm arka uç aygıtın bağlı olmadığı bildirilir. | Özellik değerleri aygıt ikizinde korunur. Aygıt bir sonraki yeniden bağlantıda okuyacaktır. Özellik değerleri [IoT Hub sorgu dili](iot-hub-devguide-query-language.md)ile alınır. | İletiler IoT Hub tarafından 48 saate kadar saklanabilir. |
| Hedefler | **DeviceId**veya [jobs](iot-hub-devguide-jobs.md)kullanan birden fazla cihaz kullanarak tek bir cihaz. | **DeviceId**veya [jobs](iot-hub-devguide-jobs.md)kullanan birden fazla cihaz kullanarak tek bir cihaz. | **DeviceId**ile tek cihaz. |
| Boyut | Maksimum doğrudan yöntem yük boyutu 128 KB'dir. | İstenilen maksimum özellik boyutu 32 KB'dir. | 64 KB'a kadar mesaj. |
| Frequency | Yüksek. Daha fazla bilgi için [IoT Hub sınırlarına](iot-hub-devguide-quotas-throttling.md)bakın. | Orta. Daha fazla bilgi için [IoT Hub sınırlarına](iot-hub-devguide-quotas-throttling.md)bakın. | Düşük. Daha fazla bilgi için [IoT Hub sınırlarına](iot-hub-devguide-quotas-throttling.md)bakın. |
| Protokol | MQTT veya AMQP kullanılarak kullanılabilir. | MQTT veya AMQP kullanılarak kullanılabilir. | Tüm protokollerde mevcuttur. Cihaz HTTPS kullanırken anket yapmalıdır. |

Aşağıdaki öğreticilerde doğrudan yöntemleri, istenen özellikleri ve buluttan aygıta iletileri nasıl kullanacağınızı öğrenin:

* [Doğrudan yöntemler kullanma](quickstart-control-device-node.md)
* [Aygıtları yapılandırmak için istenilen özellikleri kullanma](tutorial-device-twins.md) 
* [Buluttan cihaza iletileri gönderme](iot-hub-node-node-c2d.md)
