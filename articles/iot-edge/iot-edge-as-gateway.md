---
title: Ağ geçitleri için aşağı akış cihazları - Azure IOT Edge | Microsoft Docs
description: Azure IOT Edge, verileri birden çok akış CİHAZDAN buluta gönderiyor veya yerel olarak işleyen bir saydam, opak ya da proxy ağ geçidi cihazı oluşturmak için kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/25/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ec8b6cf61f9fb92f888642d1de7d4d1b9b7ac3df
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74456647"
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway"></a>Bir ağ geçidi olarak IOT Edge cihazının nasıl kullanılabileceğini

IoT Edge çözümlerinde ağ geçitleri, bu yeteneklere sahip olmayan IoT cihazlarına cihaz bağlantısı ve uç analizi sağlar. Azure IOT Edge, bir IOT ağ geçidi bağlantısı, kimlik veya uç analizi ilişkili oldukları bağımsız olarak tüm ihtiyaçlarınızı karşılamak için kullanılabilir. Bu makalede bir ağ geçidi desenleri özellikleri aşağı akış cihaz bağlantısı ve cihaz kimliği için cihaz verilerini ağ geçidi üzerinde nasıl işleneceğini değil yalnızca bakın.

## <a name="patterns"></a>Desenler

Bir ağ geçidi olarak IOT Edge cihazı kullanmak için üç desen vardır: saydam, protokol çevirisi ve kimlik çeviri:
* **Saydam** – teorik olarak IoT Hub bağlantı kurmak için bir ağ geçidi cihazına bağlanabilir cihazlar. Aşağı Akış cihazlar IOT hub'ı kimliklerinin ve MQTT, AMQP veya HTTP protokolünü kullanarak. Ağ geçidi, yalnızca IOT hub'ı ve cihazlar arasında iletişim geçirir. Cihazlar, bulut ile bir ağ geçidi üzerinden iletişim kurdukları farkında değildir ve IoT Hub içindeki cihazlarla etkileşim kuran bir kullanıcı ara ağ geçidi cihazından haberdar değildir. Bu nedenle, ağ geçidi saydamdır. IoT Edge bir cihazı saydam bir ağ geçidi olarak kullanma hakkında daha fazla ayrıntı için [bkz. saydam bir ağ geçidi oluşturma](how-to-create-transparent-gateway.md) .
* **Protokol çevirisi** – donuk ağ geçidi alanı olarak da BILINEN, MQTT, AMQP veya HTTP desteği olmayan cihazlar, verileri adına IoT Hub veri göndermek için bir ağ geçidi cihazı kullanabilir. Ağ Geçidi, aşağı akış cihazları tarafından kullanılan protokolü anlamıştır ve IoT Hub kimliği olan tek cihazdır. Tüm bilgiler, bir CİHAZDAN bir ağ geçidi geliyor gibi görünüyor. Aşağı Akış cihazları ek kimlik bilgileri, bulut uygulamalarını cihaz başına temelinde verileri çözümlemek istiyorsanız, iletilerinde katıştırmanız gerekir. Buna ek olarak, IOT hub'ı temelleri ikizlerini ve gibi yöntemler yalnızca olmayan aşağı akış cihazların ağ geçidi cihazı için kullanılabilir.
* **Kimlik çevirisi** -IoT Hub bağlantı olmayan cihazlar, bunun yerine bir ağ geçidi cihazına bağlanabilir. Ağ geçidi, IOT Hub adına aşağı akış cihazları kimlik ve protokol çevirisi sağlar. Aşağı Akış cihazlar tarafından kullanılan protokol anlamanıza, bunları kimlik sağlama ve IOT hub'ı temelleri çevirmek akıllı geçididir. Aşağı Akış cihazlar IOT Hub'ında birinci sınıf cihaz ikizleri ve yöntemler olarak görünür. Bir kullanıcı, cihazlar IOT hub'ında etkileşim kurabilir ve ara ağ geçidi cihazı farkında değildir.

![Diyagram - saydam, protokolü ve kimlik ağ geçidi desenleri](./media/iot-edge-as-gateway/edge-as-gateway.png)

## <a name="use-cases"></a>Uygulama alanları
Tüm ağ geçidi desenler aşağıdaki avantajları sağlar:
* **Kenarda analiz** : buluta tam uygunlukta telemetri göndermeden aşağı akış cihazlarından gelen verileri işlemek için yerel olarak AI hizmetlerini kullanın. Bul ve yerel olarak ınsights'a react ve yalnızca IOT Hub'ına verilerin bir alt kümesini göndermek. 
* **Aşağı akış cihaz yalıtımı** – ağ geçidi cihazı, tüm aşağı akış cihazlarının internet üzerinden etkilenmasını sağlayabilir. Bağlantı yok. bir OT ağ ile web erişim sağlayan bir BT ağ aralığındaki yerleştirilebilir. 
* **Bağlantı çoğullama** -IoT Edge bir ağ geçidi üzerinden IoT Hub bağlanan tüm cihazlar aynı temel bağlantıyı kullanır.
* **Trafik yumuşatma** -IoT Edge cihaz, trafiği yerel olarak kalıcı hale getiren IoT Hub trafiği kısıtlıyorsa üstel geri alma otomatik olarak uygulanır. Bu Avantajdan çözümünüzü karşı dayanıklı trafiğindeki ani artışları getirir.
* **Çevrimdışı destek** -ağ geçidi cihazı, IoT Hub teslim edilmemiş iletileri ve ikizi güncelleştirmelerini depolar.

Çeviri protokol ağ geçidi, uç analizi, cihaz yalıtımı, trafik düzeltmesi ve var olan cihazların ve kısıtlı kaynak yeni cihazları için çevrimdışı desteği de gerçekleştirebilirsiniz. Birçok mevcut cihaz iş öngörüleri genome veri oluşturduğunu; ancak bunlar bulut bağlantısı aklınızda tasarlanmamıştır. Donuk ağ geçitleri, bu verilerin bir IoT çözümünde kilidinin açık ve kullanılabilir olmasını sağlar.

Kimlik çeviri yapan ağ geçidi protokol çevirisi avantajlarını sağlar ve ayrıca aşağı akış cihazları buluttan tam yönetilebilirlik sağlar. Kullandıkları protokolün bağımsız olarak IOT Hub, IOT çözümünüzün tüm cihazlar gösterilir.

## <a name="cheat-sheet"></a>Kopya kağıdı
IOT hub'ı temelleri saydam kullanırken karşılaştıran bir hızlı bilgi sayfası İşte opak (Protokolü) ve proxy ağ geçitleri.

| &nbsp; | Saydam bir ağ geçidi | Protokol çevirisi | Kimlik çeviri |
|--------|-------------|--------|--------|
| IOT Hub kimlik kayıt defterinde depolanan kimlikleri | Tüm bağlı cihazlarda kimlikleri | Yalnızca ağ geçidi cihazı kimliği | Tüm bağlı cihazlarda kimlikleri |
| Cihaz çifti | Kendi cihaz ikizi bağlı her cihaza sahip | Yalnızca ağ geçidi cihazı ve modül ikizlerini sahiptir. | Kendi cihaz ikizi bağlı her cihaza sahip |
| Doğrudan yöntemler ve bulut-cihaz iletilerini | Buluta bağlı her cihaz tek tek ele | Bulutta yalnızca ağ geçidi cihazı adres | Buluta bağlı her cihaz tek tek ele |
| [IoT Hub kısıtlar ve kotaları](../iot-hub/iot-hub-devguide-quotas-throttling.md) | Her cihaz için geçerlidir | Ağ geçidi cihazı için geçerlidir | Her cihaz için geçerlidir |

Donuk ağ geçidi (Protokol çevirisi) deseni kullanılırken, bu ağ geçidi üzerinden bağlanan tüm cihazlar en fazla 50 iletileri içerebilir aynı bulut-cihaz kuyruk paylaşın. Donuk ağ geçidi düzeni yalnızca birkaç cihaz her alan ağ geçidi üzerinden bağlanıyorsanız ve bunların bulut-cihaz trafiğinin düşük olduğu durumlarda kullanılmalıdır, takip eder.

## <a name="next-steps"></a>Sonraki adımlar

Saydam ağ geçidini ayarlamayı öğrenin: 

* [IoT Edge cihazını saydam ağ geçidi olarak davranacak şekilde yapılandırma](how-to-create-transparent-gateway.md)
* [Azure IoT Hub’da bir aşağı akış cihazının kimliğini doğrulama](how-to-authenticate-downstream-device.md)
* [Aşağı akış cihazını Azure IoT Edge ağ geçidine bağlama](how-to-connect-downstream-device.md)