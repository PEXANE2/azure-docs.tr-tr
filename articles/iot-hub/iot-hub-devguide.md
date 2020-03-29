---
title: Azure IoT Hub için geliştirici kılavuzu | Microsoft Dokümanlar
description: Azure IoT Hub geliştirici kılavuzu, uç noktalar, güvenlik, kimlik kayıt defteri, aygıt yönetimi, doğrudan yöntemler, aygıt ikizleri, dosya yüklemeleri, işler, IoT Hub sorgu dili ve mesajlaşma tartışmalarını içerir.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: 1ff7d430edd3f638ad5efcc5a89604e4ed732211
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60400159"
---
# <a name="azure-iot-hub-developer-guide"></a>Azure IoT Hub geliştirici kılavuzu

Azure IoT Hub, milyonlarca aygıt ve bir çözüm arka uç arasında güvenilir ve güvenli çift yönlü iletişim sağlamaya yardımcı olan tam olarak yönetilen bir hizmettir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Azure IoT Hub size şunları sağlar:

* Aygıt başına güvenlik kimlik bilgilerini ve erişim denetimini kullanarak iletişimi güvenli hale getirin.

* Birden çok aygıt-bulut ve bulut-to-aygıt hiper ölçekli iletişim seçenekleri.

* Aygıt başına durum bilgilerinin ve meta verilerin sorgulanabilir depolaması.

* En popüler diller ve platformlar için aygıt kitaplıklarıyla kolay aygıt bağlantısı.

Bu IoT Hub geliştirici kılavuzu aşağıdaki makaleleri içerir:

* [Aygıttan buluta iletişim kılavuzu,](iot-hub-devguide-d2c-guidance.md) aygıttan buluta iletiler, aygıt ikizinin bildirilen özellikleri ve dosya yükleme arasında seçim yapmanızı sağlar.

* [Buluttan cihaza iletişim kılavuzu,](iot-hub-devguide-c2d-guidance.md) doğrudan yöntemler, aygıt ikizinin istediği özellikler ve buluttan aygıta iletiler arasında seçim yapmanızı sağlar.

* [IoT Hub ile aygıt-bulut ve bulut-to-aygıt mesajlaşma, IoT Hub'ın](iot-hub-devguide-messaging.md) ortaya çıkardığı mesajlaşma özelliklerini (aygıt-bulut ve bulut-aygıt) açıklar.

  * [Aygıttan buluta iletileri IoT Hub'ına gönderin.](iot-hub-devguide-messages-d2c.md)

  * [Yerleşik bitiş noktasından aygıttan buluta iletileri okuyun.](iot-hub-devguide-messages-read-builtin.md)

  * [Aygıttan buluta iletiler için özel uç noktaları ve yönlendirme kurallarını kullanın.](iot-hub-devguide-messages-read-custom.md)

  * [IoT Hub'dan buluttan aygıta iletiler gönderin.](iot-hub-devguide-messages-c2d.md)

  * [IoT Hub iletileri oluşturun ve okuyun.](iot-hub-devguide-messages-construct.md)

* [Bir aygıttan dosya yükleme,](iot-hub-devguide-file-upload.md) bir aygıttan nasıl dosya yükleyebileceğinizi açıklar. Makale, yükleme işleminin gönderebileceği bildirimler gibi konular hakkında da bilgiler içerir.

* [IoT Hub'da aygıt kimliklerini yönetme, her IoT](iot-hub-devguide-identity-registry.md)hub'ın kimlik kayıt defteri depolarında hangi bilgileri açıklar. Makalede ayrıca nasıl erişebileceğinizi ve değiştirebileceğiniz açıklanmaktadır.

* [IoT Hub'a erişim denetimi,](iot-hub-devguide-security.md) hem aygıtlar hem de bulut bileşenleri için IoT Hub işlevlerine erişim sağlamak için kullanılan güvenlik modelini açıklar. Makale, belirteçleri ve X.509 sertifikalarının kullanımı yla ilgili bilgileri ve verebileceğiniz izinlerin ayrıntılarını içerir.

* [Durumu senkronize etmek için aygıt ikizlerini kullanın ve yapılandırmaları](iot-hub-devguide-device-twins.md) *aygıt ikiz* kavramını açıklar. Makalede ayrıca, bir aygıtı aygıt ikiziyle senkronize etme gibi işlevsellik aygıtı da açıklanmaktadır. Makale, aygıt ikizinde depolanan veriler le ilgili bilgileri içerir.

* [Aygıtta doğrudan bir yöntem çağırın,](iot-hub-devguide-direct-methods.md) doğrudan bir yöntemin yaşam döngüsünü açıklar. Makalede, arka uç uygulamanızdan bir aygıtta yöntemlerin nasıl çağrılacağı ve cihazınızdaki doğrudan yöntemi nasıl işleyeceğiniz açıklanmaktadır.

* [İşleri birden çok aygıtta zamanlama,](iot-hub-devguide-jobs.md) birden çok cihazdaki işleri nasıl zamanlayabileceğinizi açıklar. Makalede, görevleri gerçekleştiren işlerin doğrudan bir yöntem yürütme, aygıt ikizi kullanarak güncelleştirme olarak nasıl gönderilen açıklar. Ayrıca, bir işin durumunu nasıl sorgulayınız açıklar.

* Başvuru - ioT Hub'ın aygıt iletişimi için desteklediği iletişim protokollerini açıklayan ve açık olması gereken bağlantı noktalarını listeleyen [bir iletişim protokolü seçin.](iot-hub-devguide-protocols.md)

* [Başvuru - IoT Hub uç noktaları, her IoT](iot-hub-devguide-endpoints.md) hub'ın çalışma zamanı ve yönetim işlemleri için ortaya çıkardığı çeşitli uç noktaları açıklar. Makalede ayrıca, IoT hub'ınızda nasıl ek uç noktalar oluşturabileceğiniz ve standart olmayan senaryolarda IoT Hub uç noktalarınıza bağlantı sağlamak için bir alan ağ geçidinin nasıl kullanılacağı da açıklanmaktadır.

* [Başvuru - Aygıt ikizleri, işleri ve ileti yönlendirmesi için IoT Hub sorgu dili,](iot-hub-devguide-query-language.md) aygıtınız ve işleriniz hakkında hub'ınızdan bilgi almanızı sağlayan IoT Hub sorgu dilini açıklar.

* [Başvuru - kotalar ve azaltma,](iot-hub-devguide-quotas-throttling.md) IoT Hub hizmetinde ayarlanan kotaları ve kotayı aştığınızda oluşan azaltmayı özetler.

* [Referans - fiyatlandırma,](iot-hub-devguide-pricing.md) farklı SK'ler ve IoT Hub için fiyatlandırma hakkında genel bilgiler ve çeşitli IoT Hub işlevlerinin IoT Hub tarafından ileti olarak nasıl ölçüldedildiği hakkında ayrıntılar sağlar.

* [Başvuru - Aygıt ve hizmet SDK'ları,](iot-hub-devguide-sdks.md) IoT hub'ınızla etkileşimde bulunan cihaz ve hizmet uygulamaları geliştirmek için Azure IoT SDK'larını listeler. Makale, çevrimiçi API belgelerine bağlantılar içerir.

* [Başvuru - IoT Hub MQTT desteği,](iot-hub-mqtt-support.md) IoT Hub'ın MQTT protokolünü nasıl desteklediği hakkında ayrıntılı bilgi sağlar. Makalede, Azure IoT SDK'larına yerleşik Olarak bulunan MQTT protokolü desteği açıklanır ve MQTT protokolünü doğrudan kullanma hakkında bilgi sağlar.

* IoT Hub ile ilgili yaygın terimlerin listesini [sözlük.](iot-hub-devguide-glossary.md)