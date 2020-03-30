---
title: Azure MQTT istemci kitaplığını kullanarak bir MQTT sunucusuna ileti gönderme
description: Bir MQTT aracısına ileti göndermek için MQTT İstemci kitaplığını nasıl kullanacağınızı öğrenin. Ayrıca mXChip IoT DevKit'inizi MQTT istemcisi olarak nasıl yapılandırıştırmayı öğrenin.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/02/2018
ms.author: liydu
ms.openlocfilehash: 14feb02fbac800cd37da882a1872beb8269d9938
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73954236"
---
# <a name="send-messages-to-an-mqtt-server"></a>MQTT sunucusuna ileti gönderme

Nesnelerin İnterneti (IoT) sistemleri genellikle aralıklı, kalitesiz veya yavaş internet bağlantılarıyla ilgilenir. MQTT, bu tür zorluklar göz önünde bulundurularak geliştirilen bir makineden makineye (M2M) bağlantı protokolüdür. 

Burada kullanılan MQTT istemci kitaplığı, birden fazla taşıma aracı üzerinde MQTT'yi kullanmak için API'ler sağlayan [Eclipse Paho](https://www.eclipse.org/paho/) projesinin bir parçasıdır.

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu projede şunları öğreniyorsunuz:
- MQTT aracısına ileti göndermek için MQTT İstemci kitaplığını nasıl kullanacağız?
- MXChip Iot DevKit'inizi MQTT istemcisi olarak nasıl yapılandırın.

## <a name="what-you-need"></a>Ne gerekiyor

[Başlarken Kılavuzunu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) bitirin:

* DevKit'inizin Wi-Fi'a bağlanmasını sağlar
* Geliştirme ortamını hazırlama

## <a name="open-the-project-folder"></a>Proje klasörünü açma

1. DevKit zaten bilgisayarınıza bağlıysa, bağlantısını kesin.

2. VS Code'u başlatın.

3. DevKit'i bilgisayarınıza bağlayın.

## <a name="open-the-mqttclient-sample"></a>MQTTClient Örneğini Aç

Sol tarafı **ARDUINO ÖRNEKLERİ** bölümünü genişletin, **MXCHIP AZ3166 > MQTT örneklerine**göz atın ve **MQTTClient'ı**seçin. Yeni bir VS Kodu penceresi içinde bir proje klasörü ile açılır.

> [!NOTE]
> Komut paletinden de örnek açabilirsiniz. Komut `Ctrl+Shift+P` paletini `Cmd+Shift+P`açmak için (macOS: ) kullanın, **Arduino**yazın ve ardından **Arduino'yu**bulup seçin: Örnekler .

## <a name="build-and-upload-the-arduino-sketch-to-the-devkit"></a>Arduino çizimini DevKit'e oluşturun ve yükleyin

Çalıştırmak `Ctrl+P` için yazın `Cmd+P`(macOS: ) . `task device-upload` Yükleme tamamlandıktan sonra DevKit çizimi yeniden başlatır ve çalıştırın.

![cihaz yükleme](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/device-upload.jpg)

> [!NOTE]
> "Hata: AZ3166: Bilinmeyen paket" hata iletisi alabilirsiniz. Bu hata, yönetim kurulu paket dizini doğru şekilde yenilenmediğinde oluşur. Bu hatayı gidermek için [IoT DevKit SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development)geliştirme bölümüne bakın.

## <a name="test-the-project"></a>Projeyi test edin

VS Code'da, Seri Monitör'ü açmak ve kurmak için şu yordamı izleyin:

1. Doğru `COM[X]` COM bağlantı noktasını ayarlamak için durum `STMicroelectronics`çubuğundaki sözcüğü tıklatın: ![set-com-port](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-com-port.jpg)

2. Serial Monitor'u açmak için durum çubuğundaki ![güç fişi simgesine tıklayın: seri monitör](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-monitor.jpg)
  
3. Durum çubuğunda, Baud Oranını temsil eden sayıyı `115200`tıklatın ![ve ayarlayın: set-baud-rate](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-baud-rate.jpg)

Seri Monitör, örnek çizim tarafından gönderilen tüm iletileri görüntüler. Çizim, DevKit'i Wi-Fi'a bağlar. Wi-Fi bağlantısı başarılı olduktan sonra, çizim MQTT aracısına bir ileti gönderir. Bundan sonra, örnek art arda qoS 0 ve QoS 1 kullanarak iki "iot.eclipse.org" iletileri gönderir.

![seri çıktı](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-output.jpg)

## <a name="problems-and-feedback"></a>Sorunlar ve geri bildirim

Sorunlarla karşılaşırsanız, [IoT DevKit SSS'ye](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) bakın veya aşağıdaki kanalları kullanarak bağlanın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="see-also"></a>Ayrıca bkz.

* [IoT DevKit AZ3166'yı buluttaki Azure IoT Hub'ına bağlayın](iot-hub-arduino-iot-devkit-az3166-get-started.md)
* [Shake, Shake bir Tweet için](iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message.md)

## <a name="next-steps"></a>Sonraki adımlar

MXChip Iot DevKit'inizi MQTT istemcisi olarak yapılandırmayı ve MQTT Istemci kitaplığını kullanarak bir MQTT brokerine mesaj göndermeyi öğrendiğinize göre, önerilen sonraki adımlar şunlardır:

* [Azure IoT Uzaktan İzleme çözüm hızlandırıcıya genel bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Bir MXChip IoT DevKit cihazını Azure IoT Merkezi uygulamanıza bağlayın](/azure/iot-central/core/howto-connect-devkit)
