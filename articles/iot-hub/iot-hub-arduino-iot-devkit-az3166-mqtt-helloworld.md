---
title: Azure MQTT istemci kitaplığı 'nı kullanarak bir MQTT sunucusuna ileti gönderme
description: MQTT Istemci kitaplığını kullanarak bir MQTT aracısına ileti gönderme hakkında bilgi edinin. Ayrıca, Mxyonga IoT DevKit 'i MQTT istemcisi olarak nasıl yapılandıracağınızı öğrenin.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/02/2018
ms.author: liydu
ms.custom: mqtt
ms.openlocfilehash: 600e64ef5bc3329f0116359066bdcdaf42c13e2e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81733485"
---
# <a name="send-messages-to-an-mqtt-server"></a>Bir MQTT sunucusuna ileti gönderme

Nesnelerin İnterneti (IoT) sistemleri genellikle aralıklı, düşük kaliteli veya yavaş İnternet bağlantılarıyla ilgilenir. MQTT, göz önünde bulundurularak geliştirilen bir makineden makineye ('U M2M) bağlantı protokolüdür. 

Burada kullanılan MQTT istemci kitaplığı, birden çok taşıma yöntemi üzerinden MQTT kullanmaya yönelik API 'Ler sağlayan, [tutulma PAHO](https://www.eclipse.org/paho/) projesinin bir parçasıdır.

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu projede şunları öğrenirsiniz:
- MQTT Istemci kitaplığını kullanarak bir MQTT aracısına ileti gönderme.
- Mxyongaıot DevKit 'i MQTT istemcisi olarak yapılandırma.

## <a name="what-you-need"></a>Ne gerekiyor

[Başlarken Kılavuzunu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) şu şekilde sona erdirin:

* DevKit 'in Wi-Fi ' e bağlanmasını sağlama
* Geliştirme ortamını hazırlama

## <a name="open-the-project-folder"></a>Proje klasörünü açın

1. DevKit zaten bilgisayarınıza bağlanuysa bağlantıyı kesin.

2. VS Code'u başlatın.

3. DevKit 'i bilgisayarınıza bağlayın.

## <a name="open-the-mqttclient-sample"></a>MQTTClient örneğini açın

Sol taraf **Arduino örnekleri** bölümünü GENIŞLETIN, **mxyongaAZ3166 > MQTT örneklerine**gidin ve **mqttclient**' ı seçin. İçindeki bir proje klasörüyle yeni bir VS Code penceresi açılır.

> [!NOTE]
> Ayrıca, komut paletinden örnek de açabilirsiniz. Komut `Ctrl+Shift+P` paletini açmak için ( `Cmd+Shift+P`MacOS:) kullanın, **Arduino**yazın ve **Arduino: örnekleri**bulun ve seçin.

## <a name="build-and-upload-the-arduino-sketch-to-the-devkit"></a>Arduino taslağı oluşturun ve DevKit 'e yükleyin

Çalıştırılacak `Ctrl+P` `task device-upload`tür (MacOS `Cmd+P`:). Karşıya yükleme tamamlandıktan sonra DevKit yeniden başlatılır ve taslağı çalıştırır.

![cihaz-karşıya yükle](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/device-upload.jpg)

> [!NOTE]
> "Hata: AZ3166: bilinmeyen paket" hata iletisi alabilirsiniz. Pano paket dizini doğru yenilenmediğinde bu hata oluşur. Bu hatayı çözmek için [IoT DevKit SSS konusunun geliştirme bölümüne](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development)bakın.

## <a name="test-the-project"></a>Projeyi test etme

VS Code ' de, seri Izleyiciyi açmak ve ayarlamak için şu yordamı izleyin:

1. Doğru com `COM[X]` bağlantı noktasını ayarlamak için durum çubuğundaki sözcüğe tıklayın `STMicroelectronics`: ![set-com-Port](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-com-port.jpg)

2. Durum çubuğundaki güç tak simgesine tıklayarak seri izleyici 'yi açın: ![seri izleyici](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-monitor.jpg)
  
3. Durum çubuğunda, baud hızını temsil eden sayıya tıklayın ve şu şekilde `115200`ayarlayın: ![set-baud-rate](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-baud-rate.jpg)

Seri Izleyici, örnek taslak tarafından gönderilen tüm iletileri görüntüler. Taslak, DevKit 'i Wi-Fi ' a bağlar. Wi-Fi bağlantısı başarılı olduktan sonra, taslak MQTT aracısına bir ileti gönderir. Bundan sonra örnek, sırasıyla QoS 0 ve QoS 1 kullanarak iki "iot.eclipse.org" iletisi gönderir.

![seri çıkış](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-output.jpg)

## <a name="problems-and-feedback"></a>Sorunlar ve geri bildirim

Sorunlarla karşılaşırsanız [IoT DevKit SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) sayfasına bakın veya aşağıdaki kanalları kullanarak bağlanın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="see-also"></a>Ayrıca bkz.

* [IoT DevKit AZ3166 'i Bulutta Azure IoT Hub bağlama](iot-hub-arduino-iot-devkit-az3166-get-started.md)
* [Tweet için sallayın, sallayın](iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message.md)

## <a name="next-steps"></a>Sonraki adımlar

Mxyonunuzu IoT DevKit 'i MQTT istemcisi olarak yapılandırma ve MQTT Istemci kitaplığını kullanarak bir MQTT aracısına ileti gönderme hakkında öğrendiğinize göre, önerilen sonraki adımlar aşağıda verilmiştir:

* [Azure IoT uzaktan Izleme çözüm hızlandırıcısının genel bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Bir Mxyonga IoT DevKit cihazını Azure IoT Central uygulamanıza bağlama](/azure/iot-central/core/howto-connect-devkit)
