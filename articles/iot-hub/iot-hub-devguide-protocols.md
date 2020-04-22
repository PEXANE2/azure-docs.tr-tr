---
title: Azure IoT Hub iletişim protokolleri ve bağlantı noktaları | Microsoft Dokümanlar
description: Geliştirici kılavuzu - aygıt-bulut ve bulut-aygıtiletişimi için desteklenen iletişim protokollerini ve açık olması gereken bağlantı noktası numaralarını açıklar.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 3e7f31371a0582a6f4941efbfa0087119278d2d1
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81729127"
---
# <a name="reference---choose-a-communication-protocol"></a>Başvuru - bir iletişim protokolü seçin

IoT Hub cihazların, cihaz tarafındaki iletişimler için şu protokolleri kullanmasına izin verir:

* [MQTT](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf)
* WebSockets üzerinden MQTT
* [AMQP](https://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
* WebSockets üzerinden AMQP
* HTTPS

Bu protokollerin belirli IoT Hub özelliklerini nasıl desteklediği hakkında bilgi için [aygıt-bulut](iot-hub-devguide-d2c-guidance.md) iletişim kılavuzu ve [bulut-aygıt iletişim kılavuzuna](iot-hub-devguide-c2d-guidance.md)bakın.

Aşağıdaki tablo, protokol seçiminiz için üst düzey öneriler sağlar:

| Protokol | Bu protokolü ne zaman seçmeniz gerekir? |
| --- | --- |
| MQTT <br> WebSocket üzerinde MQTT |Aynı TLS bağlantısı üzerinden birden fazla aygıtı (her biri cihaz başına kendi kimlik bilgilerine sahip) bağlaması gerektirmeyen tüm aygıtlarda kullanın. |
| AMQP <br> WebSocket üzerinde AMQP |Aygıtlar arasında bağlantı çoklama dan yararlanmak için alan ve bulut ağ geçitlerini kullanın. |
| HTTPS |Diğer protokolleri destekleyemeyen aygıtlar için kullanın. |

Aygıt tarafı iletişimiçin protokolünüzü seçerken aşağıdaki noktaları göz önünde bulundurun:

* **Bulut-aygıt deseni.** HTTPS sunucu itme uygulamak için etkili bir yolu yoktur. Bu nedenle, HTTPS kullanırken, aygıtlar ioT Hub'ı buluttan aygıta iletiler için yoklar. Bu yaklaşım hem aygıt hem de IoT Hub için verimsizdir. Geçerli HTTPS yönergelerine göre, her cihaz iletileri her 25 dakikada bir veya daha fazla ankete etmelidir. MQTT ve AMQP, buluttan cihaza iletiler alırken sunucu itme sini destekler. IoT Hub'dan aygıta iletilerin anında itilmesini sağlarlar. Teslimat gecikmesi bir sorunsa, MQTT veya AMQP kullanılacak en iyi protokollerdir. Nadiren bağlı aygıtlar için HTTPS de çalışır.

* **Alan ağ geçitleri.** MQTT ve HTTPS, TLS bağlantısı başına yalnızca tek bir aygıt kimliğini (aygıt kimliği artı kimlik bilgileri) destekler. Bu nedenle, bu protokoller, tek bir aygıt kimliği veya IoT Hub'a yukarı akım bağlantıları havuzunda birden çok aygıt kimliği kullanarak çoklama iletileri gerektiren [Alan ağ geçidi senaryoları](iot-hub-devguide-endpoints.md#field-gateways) için desteklenmez. Bu tür ağ geçitleri, akış yukarı trafiği için AMQP gibi bağlantı başına birden çok aygıt kimliği destekleyen bir iletişim kuralı kullanabilir.

* **Düşük kaynak aygıtları.** MQTT ve HTTPS kitaplıkları AMQP kitaplıkları daha küçük bir ayak izi var. Bu nedenle, aygıtın sınırlı kaynakları varsa (örneğin, 1 MB RAM'den az), bu protokoller kullanılabilir tek protokol uygulaması olabilir.

* **Ağ geçişi.** Standart AMQP protokolü 5671 portu kullanır ve MQTT 8883 bağlantı noktasını dinler. Bu bağlantı noktalarının USe'si, HTTPS olmayan protokollere kapalı olan ağlarda sorunlara neden olabilir. Bu senaryoda WebSockets üzerinden MQTT' yi, WebSockets üzerinden AMQP veya HTTPS üzerinden kullanın.

* **Yük boyutu**. MQTT ve AMQP ikili protokollerdir ve bu da HTTPS'den daha fazla kompakt yük sağlar.

> [!WARNING]
> HTTPS kullanırken, her cihaz buluttan cihaza iletileri en fazla 25 dakikada bir anket yapmalıdır. Geliştirme aşamasında, her aygıt istenirse daha sık yoklama yapabilir.

## <a name="port-numbers"></a>Bağlantı noktası numaraları

Aygıtlar çeşitli protokolleri kullanarak Azure'daki IoT Hub ile iletişim kurabilir. Genellikle, protokol seçimi çözümün belirli gereksinimleri tarafından yönlendirilir. Aşağıdaki tablo, bir aygıtın belirli bir protokolü kullanabilmesi için açık olması gereken giden bağlantı noktalarını listeler:

| Protokol | Bağlantı noktası |
| --- | --- |
| MQTT |8883 |
| WebSockets üzerinden MQTT |443 |
| AMQP |5671 |
| WebSockets üzerinden AMQP |443 |
| HTTPS |443 |

Bir Azure bölgesinde bir IoT hub'ı oluşturduktan sonra, IoT hub'ı o IoT hub'ının kullanım ömrü boyunca aynı IP adresini tutar. Ancak, Microsoft hizmet kalitesini korumak için IoT hub'ını farklı bir ölçek birimine taşırsa, yeni bir IP adresi atanır.

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub'ın MQTT protokolünü nasıl uyguladığı hakkında daha fazla bilgi edinmek için, [MQTT protokolünü kullanarak IoT hub'ınızla iletişim kurma'ya](iot-hub-mqtt-support.md)bakın.
