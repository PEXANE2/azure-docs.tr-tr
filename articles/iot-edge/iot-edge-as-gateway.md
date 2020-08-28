---
title: Aşağı akış cihazları için ağ geçitleri-Azure IoT Edge | Microsoft Docs
description: Birden çok aşağı akış aygıtından buluta veri gönderen veya yerel olarak işleyen bir saydam, donuk veya ara sunucu ağ geçidi cihazı oluşturmak için Azure IoT Edge kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/21/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 0589779de2ddb0bc75dde3b57d6444634b879f86
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89017031"
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway"></a>IoT Edge cihazını ağ geçidi olarak kullanma

IoT Edge çözümlerinde ağ geçitleri, bu yeteneklere sahip olmayan IoT cihazlarına cihaz bağlantısı ve uç analizi sağlar. Azure IoT Edge, bağlantı, kimlik veya uç analizine bağlı olup olmadığına bakılmaksızın IoT Ağ Geçidi ihtiyacını karşılamak için kullanılabilir. Bu makaledeki ağ geçidi desenleri, cihaz verilerinin ağ geçidinde işlenme biçimi değil, yalnızca aşağı akış cihaz bağlantısı ve cihaz kimliği özelliklerine başvurur.

## <a name="patterns"></a>Desenler

Bir IoT Edge cihazını ağ geçidi olarak kullanmanın üç deseni vardır: saydam, protokol çevirisi ve kimlik çevirisi.

Desenler arasındaki önemli bir fark, saydam bir ağ geçidinin iletileri aşağı akış cihazları arasında ve ek işleme gerekmeden IoT Hub geçireceğini. Ancak protokol çevirisi ve kimlik çevirisi, iletişim sağlamak için ağ geçidinde işlem gerektirir.

Herhangi bir ağ geçidi, iletileri aşağı akış cihazlarından IoT Hub geçirmeden önce analiz veya ön işleme gerçekleştirmek için IoT Edge modüllerini kullanabilir.

![Diyagram-saydam, protokol ve kimlik ağ geçidi desenleri](./media/iot-edge-as-gateway/edge-as-gateway.png)

### <a name="transparent-pattern"></a>Saydam kalıp

*Saydam* bir ağ geçidi modelinde, teorik olarak IoT Hub bir ağ geçidi cihazına bağlanabilir cihazlar. Aşağı akış cihazlarının kendi IoT Hub kimlikleri vardır ve MQTT, AMQP veya HTTP protokollerinden herhangi birini kullanır. Ağ geçidi basitçe cihazlarla IoT Hub arasındaki iletişimi geçirir. Her iki cihaz ve IoT Hub aracılığıyla onlarla etkileşim kuran kullanıcılar, bir ağ geçidinin iletişimlerini ortalamalarını farkında değildir. Bu tanıma olmaması, ağ geçidinin *saydam*olarak kabul edildiği anlamına gelir.

IoT Edge çalışma zamanı, saydam ağ geçidi özelliklerini içerir. Daha fazla bilgi için bkz. bir [IoT Edge cihazını saydam bir ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md).

### <a name="protocol-translation-pattern"></a>Protokol çevirisi stili

Saydam ağ geçidi deseninin aksine, bir *protokol çevirisi* ağ geçidi *donuk* ağ geçidi olarak da bilinir. Bu düzende, MQTT, AMQP veya HTTP desteği olmayan cihazlar, verileri adına IoT Hub veri göndermek için bir ağ geçidi cihazı kullanabilir. Ağ geçidi aşağı akış cihazları tarafından kullanılan protokolü anlar ve IoT Hub’da kimliği olan tek cihazdır. Tüm bilgiler bir cihazdan, ağ geçidinden geliyor gibi görünüyor. Bulut uygulamaları verileri cihaz başına analiz etmek istiyorsa, aşağı akış cihazları ek tanımlama bilgilerini iletilerine eklemelidir. Buna ek olarak, ikizler ve yöntemler gibi IoT Hub basit öğeleri yalnızca ağ geçidi cihazı için kullanılabilir, aşağı akış cihazları için kullanılamaz.

IoT Edge çalışma zamanı protokol çevirisi özelliklerini içermez. Bu model, genellikle kullanılan donanım ve protokole özgü özel veya üçüncü taraf modüller gerektirir. [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) , aralarından seçim yapabileceğiniz çeşitli protokol çeviri modüllerini içerir.

### <a name="identity-translation-pattern"></a>Kimlik çevirisi stili

Bir *kimlik çevirisi* ağ geçidi modelinde, IoT Hub bağlantı olmayan cihazlar, bunun yerine bir ağ geçidi cihazına bağlanabilir. Ağ geçidi, aşağı akış cihazları adına IoT Hub kimliği ve protokol çevirisi sağlar. Ağ geçidi aşağı akış cihazları tarafından kullanılan protokolü anlayacak kadar akıllıdır, onlara kimlik sağlar ve IoT Hub basit öğelerini çevirir. Aşağı akış cihazları IoT Hub’da ikizler ve yöntemlerle birinci sınıf cihazlar olarak görüntülenir. Kullanıcı IoT Hub’da cihazlarla etkileşim kurabilir ve aradaki ağ geçidi cihazının farkında olmaz.

IoT Edge çalışma zamanı, kimlik çevirisi özelliklerini içermez. Bu model, genellikle kullanılan donanım ve protokole özgü özel veya üçüncü taraf modüller gerektirir. Kimlik çevirisi modelini kullanan bir örnek için bkz. [Azure IoT Edge LoRaWAN Starter Kit](https://github.com/Azure/iotedge-lorawan-starterkit).

## <a name="use-cases"></a>Uygulama alanları

Tüm ağ geçidi desenleri aşağıdaki avantajları sağlar:

* **Kenarda analiz** : buluta tam uygunlukta telemetri göndermeden aşağı akış cihazlarından gelen verileri işlemek için yerel olarak AI hizmetlerini kullanın. Öngörüleri yerel olarak bulun ve bunlara yanıt verin ve yalnızca IoT Hub bir veri alt kümesini gönderin.
* **Aşağı akış cihaz yalıtımı** – ağ geçidi cihazı, tüm aşağı akış cihazlarının internet üzerinden etkilenmasını sağlayabilir. Bağlantısı olmayan bir OT ağı ile Web 'e erişim sağlayan bir IT ağı arasında yer alabilir.
* **Bağlantı çoğullama** -IoT Edge bir ağ geçidi üzerinden IoT Hub bağlanan tüm cihazlar aynı temel bağlantıyı kullanır.
* **Trafik yumuşatma** -IoT Edge cihaz, trafiği yerel olarak kalıcı hale getiren IoT Hub trafiği kısıtlıyorsa üstel geri alma otomatik olarak uygulanır. Bu avantaj, çözümünüzü trafikte ani artışlar açısından dayanıklı hale getirir.
* **Çevrimdışı destek** -ağ geçidi cihazı, IoT Hub teslim edilmemiş iletileri ve ikizi güncelleştirmelerini depolar.

Protokol çevirisi yapan bir ağ geçidi, var olan cihazları ve kaynak kısıtlanmış yeni cihazları destekleyebilir. Birçok mevcut cihaz, iş içgörülerini destekleyen verileri üretir; Ancak bulut bağlantısı göz önünde bulundurularak tasarlanmamıştır. Donuk ağ geçitleri, bu verilerin bir IoT çözümünde kilidinin açık ve kullanılabilir olmasını sağlar.

Kimlik çevirisi yapan bir ağ geçidi, protokol çevirisi avantajlarından yararlanır ve ayrıca, diğer bir deyişle, tüm aşağı akış cihazlarının bulutlarından tam yönetilebilirlik sağlar. IoT çözümünüzdeki tüm cihazlar, kullandıkları Protokolden bağımsız olarak IoT Hub gösterir.

## <a name="cheat-sheet"></a>Kopya kağıdı

Saydam, donuk (protokol) ve proxy ağ geçitleri kullanırken IoT Hub temel elemanların karşılaştırıldığı hızlı bir şekilde bir sayfa aşağıda verilmiştir.

| Eleman | Saydam ağ geçidi | Protokol çevirisi | Kimlik çevirisi |
|--------|-------------|--------|--------|
| IoT Hub Identity kayıt defterinde depolanan kimlikler | Tüm bağlı cihazların kimlikleri | Yalnızca ağ geçidi cihazının kimliği | Tüm bağlı cihazların kimlikleri |
| Cihaz çifti | Her bağlı cihazın kendi cihazı ikizi vardır | Yalnızca ağ geçidinde bir cihaz ve modül TWINS vardır | Her bağlı cihazın kendi cihazı ikizi vardır |
| Doğrudan Yöntemler ve buluttan cihaza iletiler | Bulut her bağlı cihazı ayrı ayrı ele alabilir | Bulut yalnızca ağ geçidi cihazını adresedebilir | Bulut her bağlı cihazı ayrı ayrı ele alabilir |
| [IoT Hub kısıtlar ve kotaları](../iot-hub/iot-hub-devguide-quotas-throttling.md) | Her cihaza Uygula | Ağ Geçidi cihazına Uygula | Her cihaza Uygula |

Donuk bir ağ geçidi (protokol çevirisi) modelini kullanırken, bu ağ geçidi üzerinden bağlanan tüm cihazlar, en fazla 50 ileti içerebilen aynı buluttan cihaza kuyruğu paylaşır. Donuk ağ geçidi deseninin yalnızca birkaç cihaz, her bir alan ağ geçidi üzerinden bağlanırken kullanılması gerekir ve bunların buluttan cihaza trafiği düşüktür.

## <a name="next-steps"></a>Sonraki adımlar

Saydam bir ağ geçidi ayarlamaya yönelik üç adımı öğrenin:

* [IoT Edge cihazını saydam ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md)
* [Azure IoT Hub’da bir aşağı akış cihazının kimliğini doğrulama](how-to-authenticate-downstream-device.md)
* [Aşağı akış cihazını Azure IoT Edge ağ geçidine bağlama](how-to-connect-downstream-device.md)
