---
title: ESP8266 to Cloud-geçiş yumuşatma HUZZAH ESP8266 Azure IoT Hub | Microsoft Docs
description: Bu öğreticide Azure Cloud Platform 'a veri göndermek için Adameyve HUZZAH ESP8266 Azure IoT Hub 'yi nasıl ayarlayacağınızı ve bağlayacağınızı öğrenin.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: wesmc
ms.openlocfilehash: 8e97a979c37af8ade51b4ff6ca4b2c5b4eec126e
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68232734"
---
# <a name="connect-adafruit-feather-huzzah-esp8266-to-azure-iot-hub-in-the-cloud"></a>Adameyve geçiş yumuşatma HUZZAH ESP8266 'i Bulutta Azure IoT Hub bağlayın

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![DHT22, geçiş yumuşatma HUZZAH ESP8266 ve IoT Hub arasında bağlantı](./media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a>Yapabilecekleriniz

Adameyve geçiş yumuşatma HUZZAH ESP8266 ' nı oluşturduğunuz bir IoT Hub 'ına bağlayın. Daha sonra, bir DHT22 sensörden sıcaklık ve nem verisi toplamak için ESP8266 üzerinde örnek bir uygulama çalıştırırsınız. Son olarak, algılayıcı verilerini IoT Hub 'ınıza gönderirsiniz.

> [!NOTE]
> Diğer ESP8266 panoları kullanıyorsanız, IoT Hub 'ınıza bağlamak için bu adımları yine de izleyebilirsiniz. Kullanmakta olduğunuz ESP8266 panosuna bağlı olarak, `LED_PIN`' yi yeniden yapılandırmanız gerekebilir. Örneğin, AI-ölçülü ker 'dan ESP8266 kullanıyorsanız, bunu `0` olarak `2`değiştirebilirsiniz. Henüz bir paketi yok mu? [Azure Web sitesinden](https://azure.com/iotstarterkits)alın.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Bir IoT Hub 'ı oluşturma ve bir cihazı geçiş yumuşatma HUZZAH ESP8266 için kaydetme
* Geçiş yumuşatma HUZZAH ESP8266 'nı algılayıcı ve bilgisayarınız ile bağlama
* Geçiş yumuşatma HUZZAH ESP8266 üzerinde örnek uygulama çalıştırarak algılayıcı verilerini toplama
* IoT Hub 'ınıza algılayıcı verileri gönderme

## <a name="what-you-need"></a>Ne gerekiyor

![Öğretici için gereken parçalar](./media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

Bu işlemi gerçekleştirmek için, geçiş yumuşatma HUZZAH ESP8266 Starter Kit ' inizden aşağıdaki bölümlere ihtiyacınız vardır:

* Geçiş yumuşatma HUZZAH ESP8266 panosu
* USB kablosu yazmak için mikro USB

Geliştirme ortamınız için aşağıdaki işlemleri de yapmanız gerekir:

* Etkin bir Azure aboneliği. Azure hesabınız yoksa yalnızca birkaç dakika içinde [ücretsiz bir Azure deneme hesabı oluşturun](https://azure.microsoft.com/free/) .
* Windows veya Ubuntu çalıştıran bir Mac veya bılgısayar.
* [GIT](https://git-scm.com/download)
* [Arduino](https://www.arduino.cc/en/main/software#download)
* [USB için adameyve Huzzah ESP8266 sürücüsü](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide)
* Geçiş yumuşatma HUZZAH ESP8266 için kablosuz ağ bağlantısı.
* Yapılandırma aracını indirmek için Internet bağlantısı.
* [Arduino için Visual Studio Code uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino).

> [!Note]
> Arduino için Visual Studio Code uzantısı tarafından kullanılan Arduino IDE sürümü Version 1.6.8 veya üzeri olmalıdır. Önceki sürümler AzureIoT kitaplığıyla çalışmaz.

Bir sensöre sahip olmadığınız durumlarda aşağıdaki öğeler isteğe bağlıdır. Ayrıca, sanal algılayıcı verisi kullanma seçeneğiniz de vardır.

* Adameyve DHT22 sıcaklığı ve nem algılayıcısı
* Bir enine Pano
* A/y atlatıcı kabloları

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>IoT Hub 'a yeni bir cihaz kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-the-sensor-and-your-computer"></a>Geçiş yumuşatma HUZZAH ESP8266 'nı algılayıcı ve bilgisayarınız ile bağlama

Bu bölümde Sensörlerinizi panonuza bağlayaöğreneceksiniz. Daha sonra daha fazla kullanım için cihazınızı bilgisayarınıza takmaktır.

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-huzzah-esp8266"></a>DHT22 sıcaklık ve nem sensöri geçiş yumuşatma HUZZAH ESP8266 bağlama

Bağlantıyı aşağıdaki şekilde yapmak için enine ve atlatıcı kablolarını kullanın. Bir sensör yoksa, bunun yerine sanal algılayıcı verilerini kullanabileceğiniz için bu bölümü atlayın.

![Bağlantı başvurusu](./media/iot-hub-arduino-huzzah-esp8266-get-started/17_connections_on_breadboard.png)

Algılayıcı PIN 'leri için aşağıdaki kabloyı kullanın:

| Başlat (algılayıcı)           | Bitiş (Pano)            | Kablo rengi   |
| -----------------------  | ---------------------- | ------------  |
| VDD (pin 31F)            | 3V (PIN 58H)           | Kırmızı kablo     |
| VERI (Iğne 32F)           | GPıO 2 (Iğne 46A)       | Mavi kablo    |
| PLAN (Iğne 34F)            | PLAN (Iğne 56ı)          | Siyah kablo   |

Daha fazla bilgi için bkz. [ADAMEYVE DHT22 algılayıcı kurulumu](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) ve [Adameyve geçiş yumuşatma HUZZAH Esp8266](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts)pinsörü.

Artık geçiş yumuşatma Huzzah ESP8266, çalışan bir sensör ile bağlanmalıdır.

![DHT22 ile bağlantı yumuşatma Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-to-your-computer"></a>Geçiş yumuşatma HUZZAH ESP8266 'i bilgisayarınıza bağlayın

Daha sonra gösterildiği gibi, geçiş yumuşatma HUZZAH ESP8266 'i bilgisayarınıza bağlamak için mikro USB 'yi kullanarak bir USB kablosu yazın.

![Huzzah geçiş yumuşatma bilgisayarınıza bağlanır](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Seri bağlantı noktası izinleri ekleme (yalnızca Ubuntu)

Ubuntu kullanırsanız, geçiş yumuşatma HUZZAH ESP8266 USB bağlantı noktasında çalışmak için izinleriniz olduğundan emin olun. Seri bağlantı noktası izinleri eklemek için aşağıdaki adımları izleyin:

1. Bir terminalde aşağıdaki komutları çalıştırın:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Aşağıdaki çıktılardan birini alırsınız:

   * CRW-RW----1 root UUCP xxxxxxxx
   * CRW-RW----1 root dialout xxxxxxxx

   Çıktıda, USB bağlantı noktasının grup `uucp` sahibinin `dialout` adı olduğuna dikkat edin.

2. Aşağıdaki komutu çalıştırarak kullanıcıyı gruba ekleyin:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>`, önceki adımda elde ettiğiniz Grup sahibi adıdır. `<username>`Ubuntu Kullanıcı adınız.

3. Ubuntu oturumunu kapatın ve değişikliğin görünmesi için yeniden oturum açın.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Algılayıcı verilerini toplayın ve IoT Hub 'ınıza gönderin

Bu bölümde, geçiş yumuşatma HUZZAH ESP8266 üzerinde örnek bir uygulama dağıtır ve çalıştırırsınız. Örnek uygulama, geçiş yumuşatma HUZZAH ESP8266 üzerindeki ışığı yanıp sönmesini sağlar ve DHT22 sensörden IoT Hub 'ınıza toplanan sıcaklık ve nem verilerini gönderir.

### <a name="get-the-sample-application-from-github"></a>GitHub 'dan örnek uygulamayı edinme

Örnek uygulama GitHub üzerinde barındırılır. Örnek uygulamayı GitHub 'dan içeren örnek depoyu kopyalayın. Örnek depoyu kopyalamak için aşağıdaki adımları izleyin:

1. Bir komut istemi veya bir Terminal penceresi açın.

2. Örnek uygulamanın depolanmasını istediğiniz klasöre gidin.

3. Şu komutu çalıştırın:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

   Sonra, Visual Studio Code geçiş yumuşatma HUZZAH ESP8266 paketini yükler.

4. Örnek uygulamanın depolandığı klasörü açın.

5. Visual Studio Code App klasöründe app. ino dosyasını açın.

   ![Örnek uygulamayı Visual Studio Code açın](media/iot-hub-arduino-huzzah-esp8266-get-started/10_vscode-open-sample-app.png)

6. Visual Studio Code, girin `F1`.

7. **Arduino** yazın ve Arduino seçin **: Pano Yöneticisi**.

8. **Arduino Pano Yöneticisi** sekmesinde, **ek URL 'ler**' e tıklayın.

   ![VS Code Arduino Pano Yöneticisi](media/iot-hub-arduino-huzzah-esp8266-get-started/11_vscode-arduino-board-manager.png)

9. **Kullanıcı ayarları** penceresinde, dosyanın sonuna şunu kopyalayın ve yapıştırın

   ```json
   "arduino.additionalUrls": "https://arduino.esp8266.com/stable/package_esp8266com_index.json"
   ```

   ![VS Code Arduino paket URL 'sini yapılandırma](media/iot-hub-arduino-huzzah-esp8266-get-started/12_vscode-package-url.png)

10. Dosyayı kaydedin ve **Kullanıcı ayarları** sekmesini kapatın.

11. **Paket dizinlerini Yenile**' ye tıklayın. Yenileme bittikten sonra, **ESP8266**için arama yapın.

12. ESP8266 için **Install** düğmesine tıklayın.

    Panolar Yöneticisi, 2.2.0 veya üzeri bir sürümü olan ESP8266 'in yüklü olduğunu gösterir.

    ![ESP8266 paketi yüklendi](media/iot-hub-arduino-huzzah-esp8266-get-started/13_vscode-esp8266-installed.png)

13. Girip `F1` **Arduino** yazın ve Arduino **seçin: Pano yapılandırması**.

14. **Seçili Pano** için Box ' a tıklayın ve **ESP8266**yazın ve ardından **adameyve HUZZAH ESP8266 (ESP8266)** öğesini seçin.

    ![ESP8266 panosunu seçin](media/iot-hub-arduino-huzzah-esp8266-get-started/14_vscode-select-esp8266.png)

### <a name="install-necessary-libraries"></a>Gerekli kitaplıkları yükler

1. Visual Studio Code, girip `F1` **Arduino** yazın ve Arduino ' ı **seçin: Kitaplık Yöneticisi**.

2. Aşağıdaki kitaplık adlarını tek tek arayın. Bulduğunuz her kitaplık için, **yüklensin**' e tıklayın.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Gerçek bir DHT22 sensör yok mu?

Örnek uygulama, gerçek bir DHT22 sensöre sahip olmadığınız durumlarda sıcaklık ve nem verilerinin benzetimini yapabilir. Örnek uygulamayı sanal verileri kullanmak üzere ayarlamak için aşağıdaki adımları izleyin:

1. `config.h` Dosyasını`app` klasöründe açın.

2. Aşağıdaki kod satırını bulun ve değerini `false` olarak `true`değiştirin:

   ```c
   define SIMULATED_DATA true
   ```

   ![Örnek uygulamayı sanal verileri kullanacak şekilde yapılandırma](media/iot-hub-arduino-huzzah-esp8266-get-started/15_vscode-configure-app-use-simulated-data.png)

3. Dosyayı kaydedin.

### <a name="deploy-the-sample-application-to-feather-huzzah-esp8266"></a>Örnek uygulamayı geçiş yumuşatma HUZZAH ESP8266 'e dağıtma

1. Visual Studio Code, durum çubuğunda  **\<seri bağlantı noktası > Seç** ' e tıklayın ve ardından geçiş yumuşatma HUZZAH ESP8266 için seri bağlantı noktasına tıklayın.

2. Girip `F1` **Arduino** yazın ve Arduino **seçin: Örnek** uygulamayı derlemek ve HUZZAH ESP8266 'e dağıtmak için karşıya yükleyin.

### <a name="enter-your-credentials"></a>Kimlik bilgilerinizi girin

Karşıya yükleme başarıyla tamamlandıktan sonra, kimlik bilgilerinizi girmek için aşağıdaki adımları izleyin:

1. Arduino IDE 'yi açın, **Araçlar** > **seri izleyici**' ye tıklayın.

2. Seri izleyici penceresinde, sağ alt köşedeki iki açılan listeye dikkat edin.

3. Sol aşağı açılan liste için **son satır yok** ' u seçin.

4. Sağ açılan listede **115200 baud** ' yı seçin.

5. Seri İzleyici penceresinin en üstünde bulunan giriş kutusunda, sağlamanız istenirse aşağıdaki bilgileri girin ve ardından **Gönder**' e tıklayın.

   * Wi-Fi SSID
   * Wi-Fi parolası
   * Cihaz bağlantı dizesi

> [!Note]
> Kimlik bilgisi bilgileri, HUZZAH ESP8266 'ın EEPROM 'ı üzerinde depolanır. Geçiş yumuşatma HUZZAH ESP8266 panosunda Sıfırla düğmesine tıklarsanız, örnek uygulama, bilgileri silmek isteyip istemediğinizi sorar. Bilgilerin `Y` silinmesini sağlamak için girin. Bilgileri ikinci bir kez girmeniz istenir.

### <a name="verify-the-sample-application-is-running-successfully"></a>Örnek uygulamanın başarıyla çalıştığını doğrulama

Seri izleyici penceresinde aşağıdaki çıktıyı ve geçiş yumuşatma HUZZAH ESP8266 üzerinde yanıp sönen ışığı görürseniz, örnek uygulama başarıyla çalışıyor demektir.

![Arduino IDE 'de nihai çıkış](media/iot-hub-arduino-huzzah-esp8266-get-started/16_arduino-ide-final-output.png)

## <a name="read-the-messages-received-by-your-hub"></a>Hub 'ınız tarafından alınan iletileri okuyun

Cihazınızdan IoT Hub 'ınız tarafından alınan iletileri izlemenin bir yolu, Visual Studio Code için Azure IoT araçlarını kullanmaktır. Daha fazla bilgi edinmek için bkz. [Visual Studio Code Için Azure IoT araçlarını kullanarak cihazınız ve IoT Hub arasında ileti gönderme ve alma](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

Cihazınız tarafından gönderilen verileri daha fazla şekilde işlemek için bir sonraki bölüme geçin.

## <a name="next-steps"></a>Sonraki adımlar

Bir geçiş yumuşatma HUZZAH ESP8266 'i IoT Hub 'ınıza başarıyla bağladınız ve yakalanan algılayıcı verilerini IoT Hub 'ınıza gönderdiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]