---
title: OPC Publisher Nedir - Azure | Microsoft Dokümanlar
description: Bu makalede, OPC Publisher'ın özelliklerine genel bir bakış sağlanmaktadır. Kodlanmış JSON telemetri verilerini Bir JSON yükü kullanarak Azure IoT Hub'ına yayımlamanızı sağlar.
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: c77dff4a4f89a78dc0a0d723fbb45fe691246112
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681931"
---
# <a name="what-is-opc-publisher"></a>OPC Publisher nedir?

OPC Publisher nasıl yapılacağını gösteren bir başvuru uygulamasıdır:

- Varolan OPC UA sunucularına bağlanın.
- OPC UA Pub/Alt formatındaki OPC UA sunucularından JSON kodlanmış telemetri verilerini, JSON yükü kullanarak Azure IoT Hub'ına yayımlayın.

Azure IoT Hub istemcisi SDK'nın desteklediği aktarım protokollerinden herhangi birini kullanabilirsiniz: HTTPS, AMQP ve MQTT.

Başvuru uygulaması şunları içerir:

- Ağınızdaki varolan OPC UA sunucularına bağlanmak için bir OPC UA *istemcisi.*
- 62222 bağlantı noktasında yayınlananları yönetmek için kullanabileceğiniz ve IoT Hub'a aynı şeyi yapmak için doğrudan yöntemler sunan bir OPC UA *sunucusu.*

[OPC Publisher başvuru uygulamasını](https://github.com/Azure/iot-edge-opc-publisher) GitHub'dan indirebilirsiniz.

Uygulama .NET Core teknolojisi kullanılarak uygulanır ve .NET Core tarafından desteklenen herhangi bir platformda çalıştırılabilir.

OPC Publisher, belirli sayıda canlı tutma isteklerine yanıt vermeyen uç noktalara bağlantılar kurmak için yeniden deneme mantığını uygular. Örneğin, bir OPC UA sunucusu bir elektrik kesintisi nedeniyle yanıt vermeyi durdurursa.

Bir OPC UA sunucusuna her ayrı yayımlama aralığı için, uygulama üzerinde bu yayımlama aralığı ile tüm düğümleri güncelleştirilen ayrı bir abonelik oluşturur.

OPC Publisher, ağ yükünü azaltmak için IoT Hub'a gönderilen verilerin toplu olarak gruplandırını destekler. Bu toplu işlem, yalnızca yapılandırılan paket boyutuna ulaşıldığında Bir paket ioT Hub'ına gönderir.

Bu uygulama NuGet paketleri olarak OPC Vakfı OPC UA başvuru yığınını kullanır. Lisans [https://opcfoundation.org/license/redistributables/1.3/](https://opcfoundation.org/license/redistributables/1.3/) koşulları için bkz.

### <a name="next-steps"></a>Sonraki adımlar

Şimdi OPC Publisher ne olduğunu öğrendim, önerilen bir sonraki adım [Nasıl OPC Publisher](howto-opc-publisher-configure.md)Yapılandırmaöğrenmektir.
