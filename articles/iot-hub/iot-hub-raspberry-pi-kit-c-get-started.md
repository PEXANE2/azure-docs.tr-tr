---
title: Raspberry Pi'yi C kullanarak Azure IoT Hub'ına bağlayın | Microsoft Dokümanlar
description: Azure bulut platformuna veri göndermek için Raspberry Pi'yi Azure IoT Hub'ına nasıl kurup bağlayabilirsiniz öğrenin
author: wesmc7777
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 02/14/2019
ms.author: wesmc
ms.openlocfilehash: 94ac75c4165b11e343ce5c31480a511ebf978a36
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67838769"
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-c"></a>Raspberry Pi'yi Azure IoT Hub'ına (C) bağlayın

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Bu eğitimde, Raspbian çalışan Raspberry Pi ile çalışmanın temellerini öğrenerek başlar. Daha sonra [Azure IoT Hub'ını](about-iot-hub.md)kullanarak aygıtlarınızı buluta sorunsuz bir şekilde nasıl bağlayacaklarınız öğrenirsiniz. Windows 10 IoT Core örnekleri için [Windows Geliştirme Merkezi'ne](https://www.windowsondevices.com/)gidin.

Henüz bir çantan yok mu? [Raspberry Pi online simülatörü](iot-hub-raspberry-pi-web-simulator-get-started.md)deneyin. Ya da [burada](https://azure.microsoft.com/develop/iot/starter-kits)yeni bir kit satın alın.

## <a name="what-you-do"></a>Ne yaparsınız

* Bir IoT hub'ı oluşturun.

* Pi için bir cihazı IoT hub'ınıza kaydedin.

* Kurulum Raspberry Pi.

* Sensör verilerini IoT hub'ınıza göndermek için Pi'de örnek bir uygulama çalıştırın.

Raspberry Pi'yi oluşturduğunuz bir IoT hub'ına bağlayın. Daha sonra Bir BME280 sensöründen sıcaklık ve nem verilerini toplamak için Pi'de örnek bir uygulama çalıştırırsınız. Son olarak, sensör verilerini IoT hub'ınıza gönderirsiniz.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Azure IoT hub'ı oluşturma ve yeni aygıt bağlantı dizenizi nasıl edinebilirsiniz?

* Pi'yi BME280 sensörüyle nasıl bağlayabilirsiniz?

* Pi'de örnek bir uygulama çalıştırarak sensör verileri nasıl toplanır?

* Sensör verilerini IoT hub'ınıza nasıl gönderirim?

## <a name="what-you-need"></a>Ne gerekiyor

![Ne gerekiyor](./media/iot-hub-raspberry-pi-kit-c-get-started/0-starter-kit.png)

* Raspberry Pi 2 veya Raspberry Pi 3 tahtası.

* Etkin bir Azure aboneliği. Azure hesabınız yoksa, birkaç dakika içinde [ücretsiz bir Azure deneme hesabı oluşturun.](https://azure.microsoft.com/free/)

* Pi'ye bağlanan bir monitör, USB klavye ve fare.

* Windows veya Linux çalıştıran bir Mac veya bilgisayar.

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
> Kod örneği simüle sensör verilerini desteklediği için bu öğeler isteğe bağlıdır.
>

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="set-up-raspberry-pi"></a>Raspberry Pi'yi kur

Şimdi Raspberry Pi'yi hazırla.

### <a name="install-the-raspbian-operating-system-for-pi"></a>Pi için Raspbian işletim sistemini kurun

Raspbian görüntüsünün yüklenmesi için microSD kartı hazırlayın.

1. Karşıdan yükleme Raspbian.

   1. [Download Raspbian Stretch Masaüstü ile](https://www.raspberrypi.org/downloads/raspbian/) (.zip dosyası).

   2. Raspbian görüntüsünü bilgisayarınızdaki bir klasöre ayıklayın.

2. MicroSD karta Raspbian'ı yükleyin.

   1. [Download ve Etcher SD kart brülör programı yükleyin.](https://etcher.io/)

   2. Etcher'ı çalıştırın ve adım 1'de çıkardığınız Raspbian görüntüsünü seçin.

   3. microSD kart sürücüsünü seçin. Etcher'ın doğru sürücüyü seçmiş olabileceğini unutmayın.

   4. Raspbian'ı microSD karta yüklemek için Flash'ı tıklatın.

   5. Yükleme tamamlandığında microSD kartı bilgisayarınızdan çıkarın. Etcher tamamlandığında microSD kartı otomatik olarak çıkardığı veya çıkardığı için microSD kartı doğrudan çıkarmak güvenlidir.

   6. MicroSD kartı Pi'ye takın.

### <a name="enable-ssh-and-spi"></a>SSH ve SPI'yi etkinleştirme

1. Pi'yi monitöre, klavyeye ve fareye bağlayın, Pi'yi `pi` başlatın ve `raspberry` kullanıcı adı ve parola olarak kullanarak Raspbian'da oturum açın.
 
2. **Tercihleri** > **Raspberry Pi Yapılandırma**> Ahududu simgesini tıklatın.

   ![Raspbian Tercihleri menüsü](./media/iot-hub-raspberry-pi-kit-c-get-started/1-raspbian-preferences-menu.png)

3. **Arabirimler** sekmesinde, Spi **ve** **SSH'yi** **Etkinleştirmek**için ayarlayın ve ardından **Tamam'ı**tıklatın. Fiziksel sensörlerinizi yoksa ve simüle edilmiş sensör verilerini kullanmak istiyorsanız, bu adım isteğe bağlıdır.

   ![Raspberry Pi'de SPI ve SSH'yi etkinleştirin](./media/iot-hub-raspberry-pi-kit-c-get-started/2-enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE]
> SSH ve SPI'yi etkinleştirmek için [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) ve [RASPI-CONFIG'de](https://www.raspberrypi.org/documentation/configuration/raspi-config.md)daha fazla başvuru belgesi bulabilirsiniz.
>

### <a name="connect-the-sensor-to-pi"></a>Sensörü Pi'ye bağlayın

Aşağıdaki gibi Pi bir LED ve BME280 bağlamak için ekmek tahtası ve jumper telleri kullanın. Sensör yoksa, bu bölümü [atlayın.](#connect-pi-to-the-network)

![Raspberry Pi ve sensör bağlantısı](./media/iot-hub-raspberry-pi-kit-c-get-started/3-raspberry-pi-sensor-connection.png)

BME280 sensörü sıcaklık ve nem verilerini toplayabilir. Cihaz ve bulut arasında bir iletişim varsa LED yanıp söner.

Sensör pimleri için aşağıdaki kabloları kullanın:

| Başlangıç (Sensör & LED)     | End (Pano)            | Kablo Rengi   |
| -----------------------  | ---------------------- | ------------: |
| LED VDD (Pin 5G)         | GPIO 4 (Pin 7)         | Beyaz kablo   |
| LED GND (Pin 6G)         | GND (Pin 6)            | Siyah kablo   |
| VDD (Pin 18F)            | 3.3V PWR (Pin 17)      | Beyaz kablo   |
| GND (Pin 20F)            | GND (Pin 20)           | Siyah kablo   |
| SCK (Pin 21F)            | SPI0 SCLK (Pin 23)     | Turuncu kablo  |
| SDO (Pin 22F)            | SPI0 MISO (Pin 21)     | Sarı kablo  |
| SDI (Pin 23F)            | SPI0 MOSI (Pin 19)     | Yeşil kablo   |
| CS (Pin 24F)             | SPI0 CS (Pin 24)       | Mavi kablo    |

[Raspberry Pi 2 & 3 Pin haritalamalarını](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) görüntülemek için tıklayınız.

BME280'i Raspberry Pi'nize başarıyla bağladıktan sonra, aşağıdaki resim gibi olmalıdır.

![Bağlı Pi ve BME280](./media/iot-hub-raspberry-pi-kit-c-get-started/4-connected-pi.png)

### <a name="connect-pi-to-the-network"></a>Pi'yi ağa bağlayın

Mikro USB kablosunu ve güç kaynağını kullanarak Pi'yi açın. Pi'yi kablolu ağınıza bağlamak için Ethernet kablosunu kullanın veya Pi'yi kablosuz ağınıza bağlamak için [Raspberry Pi Foundation'dan gelen talimatları](https://www.raspberrypi.org/learning/software-guide/wifi/) izleyin. Pi'niz ağa başarıyla bağlandıktan sonra, [Pi'nizin IP adresini](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address)not almanız gerekir.

![Kablolu ağa bağlı](./media/iot-hub-raspberry-pi-kit-c-get-started/5-power-on-pi.png)

## <a name="run-a-sample-application-on-pi"></a>Pi'de örnek bir uygulama çalıştırma

### <a name="sign-into-your-raspberry-pi"></a>Raspberry Pi'nizi kaydedin

1. Raspberry Pi'nize bağlanmak için ana bilgisayarınızdan aşağıdaki SSH istemcilerinden birini kullanın.
   
   **Windows Kullanıcıları**
   1. Windows için [PuTTY'yi](https://www.putty.org/) indirin ve yükleyin. 
   1. Pi'nizin IP adresini Ana Bilgisayar adı (veya IP adresi) bölümüne kopyalayın ve bağlantı türü olarak SSH'yi seçin.
   
   ![Macun](./media/iot-hub-raspberry-pi-kit-node-get-started/7-putty-windows.png)

   **Mac ve Ubuntu Kullanıcıları**

   Ubuntu veya macOS'ta yerleşik SSH istemcisini kullanın. Pi'yi SSH üzerinden bağlamak için çalıştırmanız `ssh pi@<ip address of pi>` gerekebilir.
   > [!NOTE]
   > Varsayılan kullanıcı adı `pi` ve parola `raspberry`.


### <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

1. Aşağıdaki komutu çalıştırarak örnek uygulamayı klonla:

   ```bash
   sudo apt-get install git-core
   git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-client-app.git
   ```

2. Kurulum komut dosyalarını çalıştırın:

   ```bash
   cd ./iot-hub-c-raspberrypi-client-app
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```

   > [!NOTE] 
   > Fiziksel **bir BME280'iniz yoksa,** sıcaklık&nem verilerini simüle etmek için komut satırı parametresi olarak '--simüle edilmiş veri' kullanabilirsiniz. `sudo ./setup.sh --simulated-data`
   >

### <a name="build-and-run-the-sample-application"></a>Örnek uygulamayı oluşturma ve çalıştırma

1. Aşağıdaki komutu çalıştırarak örnek uygulama oluşturun:

   ```bash
   cmake . && make
   ```
   
   ![Üretim oluşturma](./media/iot-hub-raspberry-pi-kit-c-get-started/7-build-output.png)

1. Aşağıdaki komutu çalıştırarak örnek uygulamayı çalıştırın:

   ```bash
   sudo ./app '<DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   > Aygıt bağlantı dizesini tek tırnak içine kopyalayıp yapıştırdığınızdan emin olun.
   >

Sensör verilerinin ve IoT hub’ınıza gönderilen iletilerin gösterildiği aşağıdaki çıkışı görmelisiniz.

![Çıkış - Raspberry Pi'den IoT hub'ınıza gönderilen sensör verileri](./media/iot-hub-raspberry-pi-kit-c-get-started/8-run-output.png)

## <a name="read-the-messages-received-by-your-hub"></a>Hub'ınız tarafından alınan iletileri okuma

IoT hub'ınız tarafından cihazınızdan alınan iletileri izlemenin bir yolu, Görsel Stüdyo Kodu için Azure IoT Araçlarını kullanmaktır. Daha fazla bilgi edinmek [için, cihazınızla IoT Hub arasında ileti gönderip almak için Visual Studio Kodu için Azure IoT Araçlarını Kullanın'a](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md)bakın.

Cihazınız tarafından gönderilen verileri işlemenin daha fazla yolu için bir sonraki bölüme devam edin.

## <a name="next-steps"></a>Sonraki adımlar

Sensör verilerini toplamak ve IoT hub'ınıza göndermek için örnek bir uygulama çalıştırdınız.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
