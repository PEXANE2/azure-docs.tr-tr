---
title: Akış aşağı aygıtlar için ağ geçitleri - Azure IoT Edge | Microsoft Dokümanlar
description: Birden çok akış aygıtından buluta veri gönderen veya yerel olarak işleyen saydam, opak veya proxy ağ geçidi aygıtı oluşturmak için Azure IoT Edge'i kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/25/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: cbca0c2509e74a7debf5ba26b361c79b9b208f08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76547124"
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway"></a>IoT Edge cihazını ağ geçidi olarak kullanma

IoT Edge çözümlerindeki ağ geçitleri, aksi takdirde bu özelliklere sahip olmayan IoT aygıtlarına aygıt bağlantısı ve kenar analitiği sağlar. Azure IoT Edge, ister bağlantı, kimlik veya kenar analitiği yle ilgili olsun, bir IoT ağ geçidine yönelik tüm gereksinimleri karşılamak için kullanılabilir. Bu makaledeki ağ geçidi desenleri yalnızca ağ geçidinde aygıt verilerinin nasıl işlenir değil, akış aşağı aygıt bağlantısı ve aygıt kimliğinin özelliklerini ifade eder.

## <a name="patterns"></a>Desenler

Bir IoT Edge aygıtını ağ geçidi olarak kullanmak için üç desen vardır: saydam, protokol çevirisi ve kimlik çevirisi:

* **Saydam** – Teorik olarak IoT Hub'a bağlanabilen aygıtlar bunun yerine bir ağ geçidi aygıtına bağlanabilir. Akış aşağı aygıtlarının kendi IoT Hub kimlikleri vardır ve MQTT, AMQP veya HTTP protokollerinden herhangi birini kullanır. Ağ geçidi yalnızca aygıtlar ve IoT Hub arasındaki iletişimi geçer. Hem aygıtlar hem de IoT Hub üzerinden onlarla etkileşimde olan kullanıcılar, bir ağ geçidinin iletişimlerine aracılık ettiğinin farkında değildir. Bu farkındalık eksikliği, ağ geçidinin *saydam*kabul gördüğü anlamına gelir. IoT Edge aygıtının saydam ağ geçidi olarak kullanılmasıyla ilgili ayrıntılar için saydam bir [ağ geçidi oluşturma](how-to-create-transparent-gateway.md) konusuna bakın.
* **Protokol çevirisi** – Opak ağ geçidi deseni olarak da bilinen, MQTT, AMQP veya HTTP'yi desteklemeyen aygıtlar, ioT Hub'a kendi adlarına veri göndermek için bir ağ geçidi aygıtı kullanabilir. Ağ geçidi, akış aşağı aygıtları tarafından kullanılan protokolü anlar ve IoT Hub'da kimliği olan tek aygıttır. Tüm bilgiler bir cihazdan geliyor gibi görünüyor, ağ geçidinden. Bulut uygulamaları verileri aygıt başına analiz etmek istiyorsa, akış aşağı aygıtları iletilerine ek tanımlayıcı bilgiler katıştırmalıdır. Ayrıca, ikizler ve yöntemler gibi IoT Hub ilkel yalnızca ağ geçidi aygıtı için kullanılabilir, aşağı aygıtlar için değil.
* **Kimlik çevirisi** - IoT Hub'a bağlanamayan aygıtlar bunun yerine bir ağ geçidi aygıtına bağlanabilir. Ağ geçidi, akış aşağı aygıtları adına IoT Hub kimliği ve protokol çevirisi sağlar. Ağ geçidi, akış aşağı aygıtları tarafından kullanılan protokolü anlayacak, onlara kimlik sağlayacak ve IoT Hub ilkellerini çevirecek kadar akıllıdır. Downstream aygıtları IoT Hub'da ikizler ve yöntemleriçeren birinci sınıf aygıtlar olarak görünür. Kullanıcı, IoT Hub'daki aygıtlarla etkileşimkurabilir ve ara ağ geçidi aygıtından habersizdir.

![Diyagram - Saydam, protokol ve kimlik ağ geçidi desenleri](./media/iot-edge-as-gateway/edge-as-gateway.png)

## <a name="use-cases"></a>Uygulama alanları

Tüm ağ geçidi desenleri aşağıdaki avantajları sağlar:

* **Analitiği sınırda** – Buluta tam doğruluk telemetrisi göndermeden akış aşağı aygıtlardan gelen verileri işlemek için AI hizmetlerini yerel olarak kullanın. İstatistikleri yerel olarak bulun ve tepki verin ve yalnızca IoT Hub'a bir veri alt kümesi gönderin.
* **Downstream cihaz yalıtımı** – Ağ geçidi cihazı, tüm akış aşağı aygıtlarını internete maruz kalmaktan koruyabilir. Bağlantıya sahip olmayan bir OT ağı ile web'e erişim sağlayan bir BT ağı arasında oturabilir.
* **Bağlantı çoklama** - IoT Hub'a bir IoT Edge ağ geçidi üzerinden bağlanan tüm aygıtlar aynı temel bağlantıyı kullanır.
* **Trafik yumuşatma** - IoT Hub trafiği daraltırken, iletileri yerel olarak devam ettirirken IoT Edge aygıtı otomatik olarak üstel geri tepme uygular. Bu avantaj, çözümünüzü trafikteki ani artışlara karşı dirençli hale getirir.
* **Çevrimdışı destek** - Ağ geçidi aygıtı, IoT Hub'a teslim edilemeyen iletileri ve ikiz güncelleştirmeleri depolar.

Protokol çevirisi yapan bir ağ geçidi, kenar analitiği, aygıt yalıtımı, trafiği yumuşatma ve kaynak kısıtlı olan mevcut aygıtlara ve yeni aygıtlara çevrimdışı destek de yapabilir. Varolan birçok cihaz, iş öngörülerine güç kaynağı olabilecek veriler üretmektedir; ancak bulut bağlantısı göz önünde bulundurularak tasarlanmadılar. Opak ağ geçitleri, bu verilerin kilidinin açılmasını ve bir IoT çözümünde kullanılmasını sağlar.

Kimlik çevirisi yapan bir ağ geçidi protokol çevirisinin avantajlarını sağlar ve ayrıca buluttan akış aşağı aygıtların tam olarak yönetilebilirliğine olanak tanır. IoT çözümünüzdeki tüm aygıtlar, kullandıkları protokolden bağımsız olarak IoT Hub'da açılır.

## <a name="cheat-sheet"></a>Kopya kağıdı

Burada şeffaf, opak (protokol) ve proxy ağ geçitleri kullanırken IoT Hub ilkel karşılaştırır hızlı bir hile sayfasıdır.

| &nbsp; | Saydam ağ geçidi | Protokol çevirisi | Kimlik çevirisi |
|--------|-------------|--------|--------|
| IoT Hub kimlik kayıt defterinde depolanan kimlikler | Bağlı tüm cihazların kimlikleri | Yalnızca ağ geçidi aygıtının kimliği | Bağlı tüm cihazların kimlikleri |
| Cihaz çifti | Bağlı her cihazın kendi cihazı ikiz | Sadece ağ geçidinde bir cihaz ve modül ikizleri var. | Bağlı her cihazın kendi cihazı ikiz |
| Doğrudan yöntemler ve buluttan cihaza iletiler | Bulut, bağlı her cihaza tek tek hitap edebilir | Bulut yalnızca ağ geçidi aygıtına hitap edebilir | Bulut, bağlı her cihaza tek tek hitap edebilir |
| [IoT Hub azaltma ve kotalar](../iot-hub/iot-hub-devguide-quotas-throttling.md) | Her cihaza uygulayın | Ağ geçidi aygıtına uygulayın | Her cihaza uygulayın |

Opak bir ağ geçidi (protokol çevirisi) deseni kullanırken, bu ağ geçidinden bağlanan tüm aygıtlar, en fazla 50 ileti içerebilen aynı buluttan aygıta sırasını paylaşır. Opak ağ geçidi deseni yalnızca her alan ağ geçidinden birkaç aygıt bağlanırken ve buluttan aygıta trafiğinin düşük olduğu zaman kullanılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Saydam bir ağ geçidi nasıl ayarlayışı nızı öğrenin:

* [IoT Edge cihazını saydam ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md)
* [Azure IoT Hub’da bir aşağı akış cihazının kimliğini doğrulama](how-to-authenticate-downstream-device.md)
* [Aşağı akış cihazını Azure IoT Edge ağ geçidine bağlama](how-to-connect-downstream-device.md)
