---
title: Raspberry Pi'yi buluttaki Azure IoT Hub'ına bağlayın (Node.js)
description: Raspberry Pi'yi bu eğitimde Azure bulut platformuna veri göndermek için Raspberry Pi'yi Azure IoT Hub'ına nasıl kurup bağdatacasüreceğinizi öğrenin.
author: wesmc7777
manager: eliotgra
keywords: azure iot ahududu pi, ahududu pi iot hub, ahududu pi bulut veri göndermek, ahududu pi bulut
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: wesmc
ms.openlocfilehash: 3175956e35603cc4ad3a938f3d316c0af8f2d227
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81640525"
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-nodejs"></a>Raspberry Pi'yi Azure IoT Hub'ına bağlayın (Düğüm.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Bu eğitimde, Raspbian çalışan Raspberry Pi ile çalışmanın temellerini öğrenerek başlar. Daha sonra [Azure IoT Hub'ını](about-iot-hub.md)kullanarak aygıtlarınızı buluta sorunsuz bir şekilde nasıl bağlayacaklarınız öğrenirsiniz. Windows 10 IoT Core örnekleri için [Windows Geliştirme Merkezi'ne](https://www.windowsondevices.com/)gidin.

Henüz bir çantan yok mu? [Raspberry Pi online simülatörü](iot-hub-raspberry-pi-web-simulator-get-started.md)deneyin. Ya da [burada](https://azure.microsoft.com/develop/iot/starter-kits)yeni bir kit satın alın.

## <a name="what-you-do"></a>Ne yaparsınız

* Bir IoT hub'ı oluşturun.

* Pi için bir cihazı IoT hub'ınıza kaydedin.

* Raspberry Pi'yi ayarla.

* Sensör verilerini IoT hub'ınıza göndermek için Pi'de örnek bir uygulama çalıştırın.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Azure IoT hub'ı oluşturma ve yeni aygıt bağlantı dizenizi nasıl edinebilirsiniz?

* Pi'yi BME280 sensörüyle nasıl bağlayabilirsiniz?

* Pi'de örnek bir uygulama çalıştırarak sensör verileri nasıl toplanır?

* Sensör verilerini IoT hub'ınıza nasıl gönderirim?

## <a name="what-you-need"></a>Ne gerekiyor

![Ne gerekiyor](./media/iot-hub-raspberry-pi-kit-node-get-started/0-starter-kit.png)

* A Raspberry Pi 2 veya Raspberry Pi 3 kurulu.

* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

* Pi'ye bağlanan bir monitör, USB klavye ve fare.

* Windows veya Linux çalıştıran bir Mac veya PC.

* İnternet bağlantısı.

* 16 GB veya üzeri microSD kart.

* İşletim sistemi görüntüsünü microSD karta yakmak için bir USB-SD adaptörü veya microSD kart.

* 6-ayak mikro USB kablosu ile 5 volt 2-amp güç kaynağı.

Aşağıdaki öğeler isteğe bağlıdır:

* Monte edilmiş Bir Adafruit BME280 sıcaklık, basınç ve nem sensörü.

* Ekmek tahtası.

* 6 F/M jumper telleri.

* Dağınık 10 mm LED.

> [!NOTE]
> İsteğe bağlı öğeler yoksa, simüle sensör verilerini kullanabilirsiniz.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="set-up-raspberry-pi"></a>Raspberry Pi'yi kur

### <a name="install-the-raspbian-operating-system-for-pi"></a>Pi için Raspbian işletim sistemini kurun

Raspbian görüntüsünün yüklenmesi için microSD kartı hazırlayın.

1. Karşıdan yükleme Raspbian.

   a. Masaüstü (.zip dosyası) [ile Raspbian Buster.](https://www.raspberrypi.org/downloads/raspbian/)

   b. Raspbian görüntüsünü bilgisayarınızdaki bir klasöre ayıklayın.

2. MicroSD karta Raspbian'ı yükleyin.

   a. [Download ve Etcher SD kart brülör programı yükleyin.](https://etcher.io/)

   b. Etcher'ı çalıştırın ve adım 1'de çıkardığınız Raspbian görüntüsünü seçin.

   c. microSD kart sürücüsünü seçin. Etcher doğru sürücüyü seçmiş olabilir.

   d. Raspbian'ı microSD karta yüklemek için Flash'ı tıklatın.

   e. Yükleme tamamlandığında microSD kartı bilgisayarınızdan çıkarın. Etcher tamamlandığında microSD kartı otomatik olarak çıkardığı veya çıkardığı için microSD kartı doğrudan çıkarmak güvenlidir.

   f. MicroSD kartı Pi'ye takın.

### <a name="enable-ssh-and-i2c"></a>SSH ve I2C'yi etkinleştirme

1. Pi'yi monitöre, klavyeye ve fareye bağlayın.

2. Pi'yi başlatın ve ardından kullanıcı `pi` adı ve `raspberry` parola olarak kullanarak Raspbian'da oturum açın.

3. **Tercihleri** > **Raspberry Pi Yapılandırma**> Ahududu simgesini tıklatın.

   ![Raspbian Tercihleri menüsü](./media/iot-hub-raspberry-pi-kit-node-get-started/1-raspbian-preferences-menu.png)

4. **Arabirimler** sekmesinde, **Etkinleştirmek**için **I2C** ve **SSH'yi** ayarlayın ve ardından **Tamam'ı**tıklatın. Fiziksel sensörlerinizi yoksa ve simüle edilmiş sensör verilerini kullanmak istiyorsanız, bu adım isteğe bağlıdır.

   ![Raspberry Pi'de I2C ve SSH'yi etkinleştirin](./media/iot-hub-raspberry-pi-kit-node-get-started/2-enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE]
> SSH ve I2C'yi etkinleştirmek için [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) ve [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c)hakkında daha fazla başvuru belgesi bulabilirsiniz.

### <a name="connect-the-sensor-to-pi"></a>Sensörü Pi'ye bağlayın

Aşağıdaki gibi Pi bir LED ve BME280 bağlamak için ekmek tahtası ve jumper telleri kullanın. Sensör yoksa, bu bölümü [atlayın.](#connect-pi-to-the-network)

![Raspberry Pi ve sensör bağlantısı](./media/iot-hub-raspberry-pi-kit-node-get-started/3-raspberry-pi-sensor-connection.png)

BME280 sensörü sıcaklık ve nem verilerini toplayabilir. Cihaz buluta bir ileti gönderdiğinde LED yanıp söner.

Sensör pimleri için aşağıdaki kabloları kullanın:

| Başlangıç (Sensör & LED)     | End (Pano)            | Kablo Rengi   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 5G)             | 3.3V PWR (Pin 1)       | Beyaz kablo   |
| GND (Pin 7G)             | GND (Pin 6)            | Kahverengi kablo   |
| SDI (Pin 10G)            | I2C1 SDA (Pin 3)       | Kırmızı kablo     |
| SCK (Pin 8G)             | I2C1 SCL (Pin 5)       | Turuncu kablo  |
| LED VDD (Pin 18F)        | GPIO 24 (Pin 18)       | Beyaz kablo   |
| LED GND (Pin 17F)        | GND (Pin 20)           | Siyah kablo   |

Raspberry Pi 2 & referans ınız için [3 pin eşlemesini](/windows/iot-core/learn-about-hardware/pinmappings/pinmappingsrpi) görüntülemek için tıklayın.

BME280'i Raspberry Pi'nize başarıyla bağladıktan sonra, aşağıdaki resim gibi olmalıdır.

![Bağlı Pi ve BME280](./media/iot-hub-raspberry-pi-kit-node-get-started/4-connected-pi.png)

### <a name="connect-pi-to-the-network"></a>Pi'yi ağa bağlayın

Mikro USB kablosunu ve güç kaynağını kullanarak Pi'yi açın. Pi'yi kablolu ağınıza bağlamak için Ethernet kablosunu kullanın veya Pi'yi kablosuz ağınıza bağlamak için [Raspberry Pi Foundation'dan gelen talimatları](https://www.raspberrypi.org/documentation/configuration/wireless/) izleyin. Pi'niz ağa başarıyla bağlandıktan sonra, [Pi'nizin IP adresini](https://www.raspberrypi.org/documentation/remote-access/ip-address.md)not almanız gerekir.

![Kablolu ağa bağlı](./media/iot-hub-raspberry-pi-kit-node-get-started/5-power-on-pi.png)

> [!NOTE]
> Pi'nin bilgisayarınızla aynı ağa bağlı olduğundan emin olun. Örneğin, Pi kablolu bir ağa bağlıyken bilgisayarınız kablosuz ağa bağlıysa, devdisco çıkışında IP adresini göremeyebilirsiniz.

## <a name="run-a-sample-application-on-pi"></a>Pi'de örnek bir uygulama çalıştırma

### <a name="clone-sample-application-and-install-the-prerequisite-packages"></a>Örnek uygulamayı klonla ve ön koşul paketlerini yükleyin

1. Raspberry Pi'nize ana bilgisayardan aşağıdaki SSH istemcilerinden biriyle bağlanın:

   **Windows Kullanıcıları**

   a. Windows için [PuTTY'yi](https://www.putty.org/) indirin ve yükleyin.

   b. Pi'nizin IP adresini Ana Bilgisayar adı (veya IP adresi) bölümüne kopyalayın ve bağlantı türü olarak SSH'yi seçin.

   ![Macun](./media/iot-hub-raspberry-pi-kit-node-get-started/7-putty-windows.png)

   **Mac ve Ubuntu Kullanıcıları**

   Ubuntu veya macOS'ta yerleşik SSH istemcisini kullanın. Pi'yi SSH üzerinden bağlamak için çalıştırmanız `ssh pi@<ip address of pi>` gerekebilir.

   > [!NOTE]
   > Varsayılan kullanıcı adı `pi` ve parola `raspberry`.

2. Pi'nize Node.js ve NPM'yi yükleyin.

   Önce Node.js sürümünü kontrol et.

   ```bash
   node -v
   ```

   Sürüm 10.x'ten düşükse veya Pi'nizde Düğüm.js yoksa, en son sürümü yükleyin.

   ```bash
   curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

3. Örnek uygulamayı klonla.

   ```bash
   git clone https://github.com/Azure-Samples/azure-iot-samples-node.git
   ```

4. Örnek için tüm paketleri yükleyin. Yükleme, Azure IoT aygıt SDK, BME280 Sensör kitaplığı ve Wiring Pi kitaplığını içerir.

   ```bash
   cd azure-iot-samples-node/iot-hub/Tutorials/RaspberryPiApp
   npm install
   ```

   > [!NOTE]
   >Bu yükleme işleminin ağ bağlantınıza bağlı olarak tamamlanması birkaç dakika sürebilir.

### <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

1. Aşağıdaki komutları çalıştırarak config dosyasını açın:

   ```bash
   nano config.json
   ```

   ![Yapılandırma dosyası](./media/iot-hub-raspberry-pi-kit-node-get-started/6-config-file.png)

   Bu dosyada yapılandırabileceğiniz iki öğe vardır. Bunlardan `interval`ilki, buluta gönderilen iletiler arasındaki zaman aralığını (milisaniye cinsinden) tanımlayandır. `simulatedData`İkincisi, simüle sensör verileri kullanılıp kullanılmaması için bir Boolean değeridir.

   Sensör **yoksa,** örnek uygulama oluşturmak `simulatedData` ve `true` simüle sensör verileri kullanmak yapmak için değeri ayarlayın.

   *Not: Bu eğitimde kullanılan i2c adresi varsayılan olarak 0x77'dir. Yapılandırmanıza bağlı olarak 0x76 da olabilir: bir i2c hatasıyla karşılaşırsanız, değeri 118'e değiştirmeyi deneyin ve bunun daha iyi çalışıp çalışmadığını görün. Sensörünüzün hangi adresi kullandığını görmek `sudo i2cdetect -y 1` için, ahududu pi üzerinde bir kabuk içinde çalıştırın*

2. Control-O > Enter > Control-X yazarak kaydedin ve çıkın.

### <a name="run-the-sample-application"></a>Örnek uygulamayı çalıştırın

Aşağıdaki komutu çalıştırarak örnek uygulamayı çalıştırın:

   ```bash
   sudo node index.js '<YOUR AZURE IOT HUB DEVICE CONNECTION STRING>'
   ```

   > [!NOTE]
   > Aygıt bağlantı dizesini tek tırnak içine kopyalayıp yapıştırdığınızdan emin olun.

Sensör verilerinin ve IoT hub’ınıza gönderilen iletilerin gösterildiği aşağıdaki çıkışı görmelisiniz.

![Çıkış - Raspberry Pi'den IoT hub'ınıza gönderilen sensör verileri](./media/iot-hub-raspberry-pi-kit-node-get-started/8-run-output.png)

## <a name="read-the-messages-received-by-your-hub"></a>Hub'ınız tarafından alınan iletileri okuma

IoT hub'ınız tarafından cihazınızdan alınan iletileri izlemenin bir yolu, Görsel Stüdyo Kodu için Azure IoT Araçlarını kullanmaktır. Daha fazla bilgi edinmek [için, cihazınızla IoT Hub arasında ileti gönderip almak için Visual Studio Kodu için Azure IoT Araçlarını Kullanın'a](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md)bakın.

Cihazınız tarafından gönderilen verileri işlemenin daha fazla yolu için bir sonraki bölüme devam edin.

## <a name="next-steps"></a>Sonraki adımlar

Sensör verilerini toplamak ve IoT hub'ınıza göndermek için örnek bir uygulama çalıştırdınız.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
