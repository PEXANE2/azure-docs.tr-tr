---
title: Raspberry Pi web simülatörünü Azure IoT Hub'ına (Node.js) bağlayın
description: Raspberry Pi web simülatörünü Azure bulutuna veri göndermek için Azure IoT Hub'a bağlayın.
author: wesmc7777
manager: philmea
keywords: ahududu pi simülatörü, azure iot raspberry pi, raspberry pi iot hub, raspberry pi buluta veri göndermek, ahududu pi bulut
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: wesmc
ms.openlocfilehash: efbe41be6c923f3547df86fd6faeb56bff5e0802
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73954524"
---
# <a name="connect-raspberry-pi-online-simulator-to-azure-iot-hub-nodejs"></a>Raspberry Pi çevrimiçi simülatörünü Azure IoT Hub'ına (Node.js) bağlayın

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Bu eğitimde, Raspberry Pi online simülatörü ile çalışmanın temellerini öğrenerek başlar. Daha sonra [Azure IoT Hub'ını](about-iot-hub.md)kullanarak Pi simülatörünü buluta sorunsuz bir şekilde nasıl bağlayabileceğinizi öğrenirsiniz.

Fiziksel cihazlarınız varsa, başlamak [için Raspberry Pi'yi Azure IoT Hub'a](iot-hub-raspberry-pi-kit-node-get-started.md) bağlayın'ı ziyaret edin.

<p>
<div id="diag" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/3-banner.png" alt="Connect Raspberry Pi web simulator to Azure IoT Hub" width="400">
</div>
<p>
<div id="button" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/6-button-default.png" alt="Start Raspberry Pi simulator" width="400" onmouseover="this.src='media/iot-hub-raspberry-pi-web-simulator/5-button-click.png';" onmouseout="this.src='media/iot-hub-raspberry-pi-web-simulator/6-button-default.png';">
</div>

## <a name="what-you-do"></a>Ne yaparsınız

* Raspberry Pi online simülatörü temellerini öğrenin.

* Bir IoT hub'ı oluşturun.

* Pi için bir cihazı IoT hub'ınıza kaydedin.

* IoT hub'ınıza simüle sensör verilerini göndermek için Pi'de örnek bir uygulama çalıştırın.

Simüle edilmiş Raspberry Pi'yi oluşturduğunuz bir IoT hub'ına bağlayın. Sonra sensör verileri oluşturmak için simülatör ile örnek bir uygulama çalıştırın. Son olarak, sensör verilerini IoT hub'ınıza gönderirsiniz.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Azure IoT hub'ı oluşturma ve yeni aygıt bağlantı dizenizi nasıl edinebilirsiniz? Azure hesabınız yoksa, birkaç dakika içinde [ücretsiz bir Azure deneme hesabı oluşturun.](https://azure.microsoft.com/free/)

* Nasıl Raspberry Pi online simülatörü ile çalışmak.

* Sensör verilerini IoT hub'ınıza nasıl gönderirim?

## <a name="overview-of-raspberry-pi-web-simulator"></a>Raspberry Pi web simülatörüne genel bakış

Raspberry Pi çevrimiçi simülatörü başlatmak için düğmeye tıklayın.

> [!div class="button"]
> <a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted" target="_blank">Raspberry Pi Simulator başlatın</a>

Web simülatöründe üç alan vardır.

1. Derleme alanı - Varsayılan devrede Pi bir BME280 sensörü ve bir LED'le bağlanır. Bu alan önizleme sürümünde kilitli olduğundan şu anda özelleştirme yapamazsınız.

2. Kodlama alanı - Raspberry Pi'yi kodlamanız için çevrimiçi bir kod düzenleyicisi. Varsayılan örnek uygulama BME280 sensöründen sensör verileri toplamaya ve bunları Azure IoT Hub'ınıza göndermeye yardımcı olur. Uygulama, asıl Pi cihazlarıyla tamamen uyumludur. 

3. Tümleşik konsol penceresi - Kodunuzun çıkışını gösterir. Bu pencerenin en üstünde üç düğme vardır.

   * **Çalıştır** - Kodlama alanındaki uygulamayı çalıştırın.

   * **Sıfırla** - Kodlama alanını varsayılan örnek uygulamaya sıfırlayın.

   * **Katla/Genişlet** - Sağ tarafta konsol penceresini katlamanız/genişletmeniz için bir düğme yer alır.

> [!NOTE]
> Raspberry Pi web simülatörü artık önizleme sürümünde mevcuttur. [Gitter Sohbet Odası'nda](https://gitter.im/Microsoft/raspberry-pi-web-simulator)sesini duymak istiyoruz. Kaynak kodu [GitHub'da](https://github.com/Azure-Samples/raspberry-pi-web-simulator)herkese açıktır.

![Pi online simülatöre genel bakış](media/iot-hub-raspberry-pi-web-simulator/0-overview.png)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="run-a-sample-application-on-pi-web-simulator"></a>Pi web simülatöründe örnek uygulamayı çalıştırma

1. Kodlama alanında varsayılan örnek uygulamayla çalıştığınızdan emin olun. Satır 15'deki yer tutucuyu Azure IoT hub cihaz bağlantı dizesiyle değiştirin.
1. 
   ![Cihaz bağlantı dizesini değiştirme](media/iot-hub-raspberry-pi-web-simulator/1-connectionstring.png)

2. Uygulamayı **Run** çalıştırmak `npm start` için Çalıştır veya yazın'ı seçin.

Raspberry Pi'den IoT hub'ınıza gönderilen sensör verilerini ve ![IoT hub'ınıza gönderilen iletileri gösteren aşağıdaki çıktıyı görmeniz gerekir](media/iot-hub-raspberry-pi-web-simulator/2-run-application.png)

## <a name="read-the-messages-received-by-your-hub"></a>Hub'ınız tarafından alınan iletileri okuma

IoT hub'ınız tarafından alınan iletileri simüle edilen cihazdan izlemenin bir yolu, Görsel Stüdyo Kodu için Azure IoT Araçlarını kullanmaktır. Daha fazla bilgi edinmek [için, cihazınızla IoT Hub arasında ileti gönderip almak için Visual Studio Kodu için Azure IoT Araçlarını Kullanın'a](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md)bakın.

Cihazınız tarafından gönderilen verileri işlemenin daha fazla yolu için bir sonraki bölüme devam edin.

## <a name="next-steps"></a>Sonraki adımlar

Sensör verilerini toplamak ve IoT hub'ınıza göndermek için örnek bir uygulama çalıştırdınız.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
