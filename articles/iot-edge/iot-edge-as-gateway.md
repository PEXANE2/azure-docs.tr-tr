---
title: Aşağı akış cihazları için ağ geçitleri-Azure IoT Edge | Microsoft Docs
description: Birden çok aşağı akış aygıtından buluta veri gönderen veya yerel olarak işleyen bir saydam, donuk veya ara sunucu ağ geçidi cihazı oluşturmak için Azure IoT Edge kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/25/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 916eeaa60bc054301af039164ce1c14e77ceb91a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81733523"
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway"></a>IoT Edge cihazını ağ geçidi olarak kullanma

IoT Edge çözümlerinde ağ geçitleri, bu yeteneklere sahip olmayan IoT cihazlarına cihaz bağlantısı ve uç analizi sağlar. Azure IoT Edge, bağlantı, kimlik veya uç analizine bağlı olup olmadığına bakılmaksızın IoT Ağ Geçidi ihtiyacını karşılamak için kullanılabilir. Bu makaledeki ağ geçidi desenleri, cihaz verilerinin ağ geçidinde işlenme biçimi değil, yalnızca aşağı akış cihaz bağlantısı ve cihaz kimliği özelliklerine başvurur.

## <a name="patterns"></a>Desenler

Bir IoT Edge cihazını ağ geçidi olarak kullanmanın üç deseni vardır: saydam, protokol çevirisi ve kimlik çevirisi:

* **Saydam** – teorik olarak IoT Hub bağlantı kurmak için bir ağ geçidi cihazına bağlanabilir cihazlar. Aşağı akış cihazlarının kendi IoT Hub kimlikleri vardır ve MQTT, AMQP veya HTTP protokollerinden herhangi birini kullanıyor. Ağ geçidi yalnızca aygıtlar ve IoT Hub arasındaki iletişimleri geçirir. Her iki cihaz ve IoT Hub aracılığıyla onlarla etkileşim kuran kullanıcılar, bir ağ geçidinin iletişimlerini ortalamalarını farkında değildir. Bu tanıma olmaması, ağ geçidinin *saydam*olarak kabul edildiği anlamına gelir. IoT Edge bir cihazı saydam bir ağ geçidi olarak kullanma hakkında daha fazla ayrıntı için [bkz. saydam bir ağ geçidi oluşturma](how-to-create-transparent-gateway.md) .
* **Protokol çevirisi** – donuk ağ geçidi alanı olarak da BILINEN, MQTT, AMQP veya HTTP desteği olmayan cihazlar, verileri adına IoT Hub veri göndermek için bir ağ geçidi cihazı kullanabilir. Ağ Geçidi, aşağı akış cihazları tarafından kullanılan protokolü anlamıştır ve IoT Hub kimliği olan tek cihazdır. Tüm bilgiler bir cihazdan, ağ geçidinden geliyor gibi görünüyor. Bulut uygulamaları verileri cihaz temelinde çözümlemek istiyorsam, aşağı akış cihazları iletilerine ek tanımlayıcı bilgileri katıştırmalıdır. Ayrıca, TWINS ve yöntemler gibi IoT Hub temel elemanlar yalnızca ağ geçidi cihazında kullanılabilir, akış aşağı cihazlarda kullanılamaz.
* **Kimlik çevirisi** -IoT Hub bağlantı olmayan cihazlar, bunun yerine bir ağ geçidi cihazına bağlanabilir. Ağ Geçidi, aşağı akış cihazları adına IoT Hub kimlik ve protokol çevirisi sağlar. Ağ Geçidi, aşağı akış cihazları tarafından kullanılan protokolü anlamak, bunların kimliklerini sağlamak ve IoT Hub temel öğeleri çevirmek için yeterince akıllı bir temeldir. Aşağı akış cihazları, TWINS ve yöntemleriyle birinci sınıf cihazlar olarak IoT Hub görüntülenir. Kullanıcı IoT Hub cihazlarıyla etkileşime geçebilir ve ara ağ geçidi cihazını farkında değildir.

![Diyagram-saydam, protokol ve kimlik ağ geçidi desenleri](./media/iot-edge-as-gateway/edge-as-gateway.png)

## <a name="use-cases"></a>Uygulama alanları

Tüm ağ geçidi desenleri aşağıdaki avantajları sağlar:

* **Kenarda analiz** : buluta tam uygunlukta telemetri göndermeden aşağı akış cihazlarından gelen verileri işlemek için yerel olarak AI hizmetlerini kullanın. Öngörüleri yerel olarak bulun ve bunlara yanıt verin ve yalnızca IoT Hub bir veri alt kümesini gönderin.
* **Aşağı akış cihaz yalıtımı** – ağ geçidi cihazı, tüm aşağı akış cihazlarının internet üzerinden etkilenmasını sağlayabilir. Bağlantısı olmayan bir OT ağı ile Web 'e erişim sağlayan bir IT ağı arasında yer alabilir.
* **Bağlantı çoğullama** -IoT Edge bir ağ geçidi üzerinden IoT Hub bağlanan tüm cihazlar aynı temel bağlantıyı kullanır.
* **Trafik yumuşatma** -IoT Edge cihaz, trafiği yerel olarak kalıcı hale getiren IoT Hub trafiği kısıtlıyorsa üstel geri alma otomatik olarak uygulanır. Bu avantaj, çözümünüzü trafikte ani artışlar açısından dayanıklı hale getirir.
* **Çevrimdışı destek** -ağ geçidi cihazı, IoT Hub teslim edilmemiş iletileri ve ikizi güncelleştirmelerini depolar.

Protokol çevirisi yapan bir ağ geçidi, uç analizi, cihaz yalıtımı, trafik düzleştirme ve mevcut cihazlara ve kaynak kısıtlanmış yeni cihazlara çevrimdışı destek de gerçekleştirebilir. Birçok mevcut cihaz, iş içgörülerini destekleyen verileri üretir; Ancak bulut bağlantısı göz önünde bulundurularak tasarlanmamıştır. Donuk ağ geçitleri, bu verilerin bir IoT çözümünde kilidinin açık ve kullanılabilir olmasını sağlar.

Kimlik çevirisi yapan bir ağ geçidi, protokol çevirisi avantajlarından yararlanır ve ayrıca, diğer bir deyişle, tüm aşağı akış cihazlarının bulutlarından tam yönetilebilirlik sağlar. IoT çözümünüzdeki tüm cihazlar, kullandıkları Protokolden bağımsız olarak IoT Hub gösterir.

## <a name="cheat-sheet"></a>Kopya kağıdı

Saydam, donuk (protokol) ve proxy ağ geçitleri kullanırken IoT Hub temel elemanların karşılaştırıldığı hızlı bir şekilde bir sayfa aşağıda verilmiştir.

| &nbsp; | Saydam ağ geçidi | Protokol çevirisi | Kimlik çevirisi |
|--------|-------------|--------|--------|
| IoT Hub Identity kayıt defterinde depolanan kimlikler | Tüm bağlı cihazların kimlikleri | Yalnızca ağ geçidi cihazının kimliği | Tüm bağlı cihazların kimlikleri |
| Cihaz çifti | Her bağlı cihazın kendi cihazı ikizi vardır | Yalnızca ağ geçidinde bir cihaz ve modül TWINS vardır | Her bağlı cihazın kendi cihazı ikizi vardır |
| Doğrudan Yöntemler ve buluttan cihaza iletiler | Bulut her bağlı cihazı ayrı ayrı ele alabilir | Bulut yalnızca ağ geçidi cihazını adresedebilir | Bulut her bağlı cihazı ayrı ayrı ele alabilir |
| [IoT Hub kısıtlar ve kotaları](../iot-hub/iot-hub-devguide-quotas-throttling.md) | Her cihaza Uygula | Ağ Geçidi cihazına Uygula | Her cihaza Uygula |

Donuk bir ağ geçidi (protokol çevirisi) modelini kullanırken, bu ağ geçidi üzerinden bağlanan tüm cihazlar, en fazla 50 ileti içerebilen aynı buluttan cihaza kuyruğu paylaşır. Donuk ağ geçidi deseninin yalnızca birkaç cihaz, her bir alan ağ geçidi üzerinden bağlanırken kullanılması gerekir ve bunların buluttan cihaza trafiği düşüktür.

## <a name="next-steps"></a>Sonraki adımlar

Saydam ağ geçidini ayarlamayı öğrenin:

* [IoT Edge cihazını saydam ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md)
* [Azure IoT Hub’da bir aşağı akış cihazının kimliğini doğrulama](how-to-authenticate-downstream-device.md)
* [Aşağı akış cihazını Azure IoT Edge ağ geçidine bağlama](how-to-connect-downstream-device.md)
