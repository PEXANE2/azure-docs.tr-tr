---
title: IoT DevKit AZ3166'yı Azure IoT Hub'ına bağlayın
description: Bu eğitimde, Azure bulut platformuna veri gönderebilmek için IoT DevKit AZ3166'yı Azure IoT Hub'ına nasıl ayarlayıp bağlayabileceğinizi öğrenin.
author: wesmc7777
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 06/25/2019
ms.author: wesmc
ms.openlocfilehash: 5d6ba2bd52d8f4af4244e8ce1babeb7bc9d37fe4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235668"
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub"></a>IoT DevKit AZ3166'yı Azure IoT Hub'ına bağlayın

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

[MXChip IoT DevKit'i,](https://microsoft.github.io/azure-iot-developer-kit/) Microsoft Azure hizmetlerinden yararlanan Nesnelerin İnterneti (IoT) çözümlerini geliştirmek ve prototiplemek için kullanabilirsiniz. Bu zengin çevre birimleri ve sensörleri ile Arduino uyumlu bir yönetim kurulu, bir açık kaynak kurulu paketi ve zengin bir [örnek galeri](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/)içerir.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Nasıl bir IoT hub oluşturmak ve MXChip IoT DevKit için bir cihaz kayıt.
* IoT DevKit'i Wi-Fi'a bağlama ve IoT Hub bağlantı dizesini yapılandırma.
* DevKit sensör telemetri verilerini IoT hub'ınıza nasıl gönderirim?
* Geliştirme ortamının nasıl hazırlanacağı ve IoT DevKit için uygulamanın nasıl geliştirilen.

Henüz DevKit'inyok mu? [DevKit simülatörü](https://azure-samples.github.io/iot-devkit-web-simulator/) deneyin veya [bir DevKit satın.](https://aka.ms/iot-devkit-purchase)

Kod [örnekleri galerisinden](https://docs.microsoft.com/samples/browse/?term=mxchip)tüm DevKit öğreticiler için kaynak kodunu bulabilirsiniz.

## <a name="what-you-need"></a>Ne gerekiyor

* Mikro USB kablolu bir MXChip IoT DevKit kartı. [Hemen al.](https://aka.ms/iot-devkit-purchase)
* Windows 10, macOS 10.10+ veya Ubuntu 18.04+ çalıştıran bir bilgisayar.
* Etkin bir Azure aboneliği. [Ücretsiz 30 günlük deneme Microsoft Azure hesabını etkinleştirin.](https://azureinfo.microsoft.com/us-freetrial.html)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]
  
## <a name="prepare-your-hardware"></a>Donanımınızı hazırlayın

Aşağıdaki donanımı bilgisayarınıza bağla:

* DevKit kurulu
* Mikro-USB kablosu

![Gerekli donanım](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

DevKit'i bilgisayarınıza bağlamak için aşağıdaki adımları izleyin:

1. USB ucunu bilgisayarınıza bağlayın.

2. Mikro-USB ucunu DevKit'e bağlayın.

3. Güç için yeşil LED bağlantıyı doğrular.

   ![Donanım bağlantıları](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="quickstart-send-telemetry-from-devkit-to-an-iot-hub"></a>Hızlı başlatma: DevKit'ten Bir IoT Hub'ına telemetri gönderin

Quickstart, telemetriyi IoT Hub'ına göndermek için önceden derlenmiş DevKit firmware'i kullanır. Çalıştırmadan önce bir IoT hub'ı oluşturun ve hub'a bir aygıt kaydettirin.

### <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Aygıt kimliğini oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

   **MyNodeDevice**: Kaydolduğunuz aygıtın adı. **MyNodeDevice'ı** gösterildiği gibi kullanın. Aygıtınız için farklı bir ad seçerseniz, bu makale boyunca bu adı kullanmanız ve bunları çalıştırmadan önce örnek uygulamalardaki aygıt adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyNodeDevice
    ```

   > [!NOTE]
   > Çalışırken `device-identity`bir hata alırsanız, daha fazla ayrıntı [için Azure CLI için Azure IOT Uzantısı'nı](https://github.com/Azure/azure-iot-cli-extension/blob/dev/README.md) yükleyin.
  
1. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

   **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyNodeDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

### <a name="send-devkit-telemetry"></a>DevKit telemetrisini gönder

DevKit, IoT hub'ınızdaki cihaza özel bir uç noktaya bağlanır ve sıcaklık ve nem telemetrisi gönderir.

1. IoT DevKit için [GetStarted firmware'in](https://aka.ms/devkit/prod/getstarted/latest) en son sürümünü indirin.

1. IoT DevKit'in bilgisayarınıza USB üzerinden bağlandığından emin olun. Dosya Gezgini'ni açın **AZ3166**adında bir USB yığın depolama aygıtı vardır.

    ![Windows Gezgini'ni Aç](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/az3166-usb.png)

1. Sürükleyin ve firmware bırakın sadece kitle depolama aygıtına indirilen ve otomatik olarak yanıp söner.

    ![Firmware'i kopyala](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/copy-firmware.png)

1. DevKit'te B **tuşuna**basılı tutun, **Sıfırla** düğmesine basın ve bırakın ve ardından **B**düğmesini bırakın. DevKit'iniz AP moduna girer. Doğrulamak için ekran, DevKit'in hizmet kümesi tanımlayıcısını (SSID) ve yapılandırma portalı IP adresini görüntüler.

    ![Sıfırlama düğmesi, B düğmesi ve SSID](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/wifi-ap.jpg)

    ![AP Modunu Ayarla](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/set-ap-mode.gif)

1. Önceki adımda görüntülenen IoT DevKit SSID'ye bağlanmak için farklı bir Wi-Fi özellikli aygıtta (bilgisayar veya cep telefonu) bir Web tarayıcısı kullanın. Parola isterse, boş bırakın.

    ![SSID'yi bağla](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/connect-ssid.png)

1. Tarayıcıda **192.168.0.1'i** açın. IoT DevKit'in bağlanmasını istediğiniz Wi-Fi'ı seçin, Wi-Fi parolasını yazın ve daha önce not aldığınız aygıt bağlantı dizesini yapıştırın. Daha sonra Kaydet'e tıklayın.

    ![Yapılandırma UI](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/configuration-ui.png)

    > [!NOTE]
    > IoT DevKit sadece 2.4GHz ağını destekler. Daha fazla ayrıntı için [SSS'yi](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#wi-fi-configuration) kontrol edin.

1. Sonuç sayfasını gördüğünüzde Wi-Fi bilgileri ve aygıt bağlantı dizesi IoT DevKit'te depolanır.

    ![Yapılandırma sonucu](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/configuration-ui-result.png)

    > [!NOTE]
    > Wi-Fi yapılandırıldıktan sonra, aygıt fişi çekilmiş olsa bile kimlik bilgileriniz bu bağlantı için aygıtta devam edecektir.

1. IoT DevKit birkaç saniye içinde yeniden başlatılır. DevKit ekranında, DevKit'in IP adresinin, Azure IoT Hub'a gönderilen ileti sayısıyla sıcaklık ve nem değeri de dahil olmak üzere telemetri verilerini izlediğini görürsünüz.

    ![WiFi IP](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/wifi-ip.jpg)

    ![Veri gönderme](media/iot-hub-arduino-devkit-az3166-get-started/quickstarts/sending-data.jpg)

1. Azure'a gönderilen telemetri verilerini doğrulamak için Azure Bulut Bulutu'nda aşağıdaki komutu çalıştırın:

    ```azurecli
    az iot hub monitor-events --hub-name YourIoTHubName --output table
    ```

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Geliştirme ortamını DevKit için hazırlamak için aşağıdaki adımları izleyin:

### <a name="install-visual-studio-code-with-azure-iot-tools-extension-package"></a>Azure IoT Araçları uzantı paketiyle Visual Studio Kodu yükleme

1. [Arduino IDE'yi yükleyin.](https://www.arduino.cc/en/Main/Software) Arduino kodunu derlemek ve yüklemek için gerekli araç zincirini sağlar.
    * **Windows**: Windows Installer sürümünü kullanın. App Store'dan yüklemeyin.
    * **macOS**: Çıkarılan **Arduino.app'ı** `/Applications` sürükleyip klasöre bırakın.
    * **Ubuntu**: Unzip gibi klasöre`$HOME/Downloads/arduino-1.8.8`

2. Güçlü [Visual Studio Code](https://code.visualstudio.com/)intellisense, kod tamamlama ve hata ayıklama desteği yanı sıra zengin uzantıları pazardan yüklenebilir ile bir çapraz platform kaynak kodu editörü yükleyin.

3. VS Kodu başlatın, uzatma pazar **Arduino** arayın ve yükleyin. Bu uzantı, Arduino platformunda geliştirmek için gelişmiş deneyimler sağlar.

    ![Arduino'ya yükleyin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-arduino.png)

4. Uzantılı pazarda [Azure IoT Araçlarını](https://aka.ms/azure-iot-tools) arayın ve yükleyin.

    ![Azure IoT Araçlarını Yükleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-azure-iot-tools.png)

    Veya bu doğrudan bağlantıyı kullanın:
    > [!div class="nextstepaction"]
    > [Azure IoT Araçları uzantı paketini yükleme](vscode:extension/vsciot-vscode.azure-iot-tools)

    > [!NOTE]
    > Azure IoT Araçları uzantı paketi, çeşitli IoT devkit aygıtlarında geliştirme ve hata ayıklama için kullanılan [Azure IoT Aygıt Çalışma Tezgahı](https://aka.ms/iot-workbench) içerir. Azure IoT Araçları uzantı paketiyle birlikte verilen [Azure IoT Hub uzantısı,](https://aka.ms/iot-toolkit)Azure IoT Hub'larını yönetmek ve bunlarla etkileşimde kullanılmak üzere kullanılır.

5. VS Kodunu Arduino ayarlarıyla yapılandırın.

    Visual Studio Code'da **Dosya > Tercihleri > Ayarları'nı** (macOS, **Kod > Tercihleri >** Ayarlar'da) tıklatın. Ardından *Ayarlar* sayfasının sağ üst köşesindeki **Ayarlar 'ı Aç (JSON)** simgesini tıklatın.

    ![Azure IoT Araçlarını Yükleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/user-settings-arduino.png)

    Arduino'u platformunuza bağlı olarak yapılandırmak için aşağıdaki satırları ekleyin: 

    * **Windows**:

        ```json
        "arduino.path": "C:\\Program Files (x86)\\Arduino",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

    * **macOS**:

        ```json
        "arduino.path": "/Applications",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

    * **Ubuntu**:

        Aşağıdaki **{kullanıcı adı}** yer tutucuyu kullanıcı adınız ile değiştirin.

        ```json
        "arduino.path": "/home/{username}/Downloads/arduino-1.8.8",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

6. Komut `F1` paletini açmak için tıklatın, yazın ve **Arduino seçin: Board Manager**. **AZ3166'yı** arayın ve en son sürümü yükleyin.

    ![Devkit SDK'yı Yükleyin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-az3166-sdk.png)

### <a name="install-st-link-drivers"></a>ST-Link sürücülerini yükleme

[ST-Link/V2,](https://www.st.com/en/development-tools/st-link-v2.html) IoT DevKit'in geliştirme makinenizle iletişim kurmak için kullandığı USB arabirimidir. Derlenen aygıt kodunu DevKit'e yanıp salmak için Windows'a yüklemeniz gerekir. Makinenin cihazınıza erişmesine izin vermek için işletim sistemi için özel adımları izleyin.

* **Windows**: [STMicroelectronics web sitesinden](https://www.st.com/en/development-tools/stsw-link009.html)USB sürücüsünü indirin ve kurun.
* **macOS**: macOS için sürücü gerekmez.
* **Ubuntu**: Terminaldeki komutları çalıştırın ve grup değişikliğinin yürürlüğe girmesi için oturum açın:

    ```bash
    # Copy the default rules. This grants permission to the group 'plugdev'
    sudo cp ~/.arduino15/packages/AZ3166/tools/openocd/0.10.0/linux/contrib/60-openocd.rules /etc/udev/rules.d/
    sudo udevadm control --reload-rules

    # Add yourself to the group 'plugdev'
    # Logout and log back in for the group to take effect
    sudo usermod -a -G plugdev $(whoami)
    ```

Şimdi hepiniz geliştirme ortamınızı hazırlama ya da yapılandırma konusunda hazırsınız. Az önce yayınladığınız GetStarted örneğini oluşturalım.

## <a name="build-your-first-project"></a>İlk projenizi oluşturun

### <a name="open-sample-code-from-sample-gallery"></a>Örnek galeriden örnek kodu açma

IoT DevKit, DevKit'i çeşitli Azure hizmetlerine bağlamayı öğrenmek için kullanabileceğiniz zengin bir örnek galerisi içerir.

1. IoT DevKit'inizin bilgisayarınıza **bağlı olmadığından** emin olun. Önce VS Kodu'nu başlatın ve ardından DevKit'i bilgisayarınıza bağlayın.

1. Komut `F1` paletini açmak, Azure **IoT Aygıt Çalışma Tezgahı**yazın ve seçin: Örnekleri Aç... seçeneğini tıklatın. Ardından tahta olarak **IoT DevKit'i** seçin.

1. IoT Çalışma Tezgahı Örnekleri sayfasında, **Başlat'ı** bulun ve **Örnek Aç'ı**tıklatın. Ardından örnek kodu indirmek için varsayılan yolu seçer.

    ![Açık örnek](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/open-sample.png)

### <a name="provision-azure-iot-hub-and-device"></a>Azure IoT Hub ve cihazı sağlama

Azure portalından Azure IoT Hub'ını ve aygıtını sağlamayapmak yerine, geliştirme ortamından çıkmadan VS Kodu'nda yapabilirsiniz.

1. Yeni açılan proje penceresinde, `F1` komut paletini açmak, **Azure IoT Aygıt Çalışma Tezgahı**yazın ve seçin: Azure Hizmetlerini Sağlama... seçeneğini tıklatın. Azure IoT Hub'ınızı sağlama yı ve IoT Hub aygıtını oluşturmayı bitirmek için adım adım kılavuzuizleyin.

    ![Hüküm komutu](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/provision.png)

    > [!NOTE]
    > Azure'da oturum imzalamadıysanız. Oturum açmak için açılan bildirimi izleyin.

1. Kullanmak istediğiniz aboneliği seçin.

    ![Alt seç](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-subscription.png)

1. Ardından yeni bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#terminology)seçin veya oluşturun.

    ![Kaynak grubu seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-resource-group.png)

1. Belirttiğiniz kaynak grubunda, yeni bir Azure IoT Hub'ı seçmek veya oluşturmak için kılavuzu izleyin.

    ![IoT Hub adımlarını seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-provision.png)

    ![IoT Hub'ı seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-iot-hub.png)

    ![Seçilen IoT Hub](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-selected.png)

1. Çıktı penceresinde Azure IoT Hub'ı hazırlanırken görürsünüz.

    ![IoT Hub Sağlanan](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-hub-provisioned.png)

1. Oluşturduğunuz Azure IoT Hub'da yeni bir aygıt seçin veya oluşturun.

    ![IoT Aygıt adımlarını seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/iot-device-provision.png)

    ![IoT Cihazı Nı Seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-iot-device.png)

1. Artık Azure IoT Hub'ı ve içinde aygıt oluşturuldu. Ayrıca aygıt bağlantı dizesi daha sonra IoT DevKit yapılandırmak için VS Kodu kaydedilir.

    ![Yapılan hüküm](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/provision-done.png)

### <a name="configure-and-compile-device-code"></a>Aygıt kodunu yapılandırma ve derleme

1. Sağ alt durum çubuğunda, **MXCHIP AZ3166'nın** seçili tahta olarak gösterildiğini ve **STMicroelectronics'in** kullanıldığı seri bağlantı noktasının kullanıldığını kontrol edin.

    ![Yönetim kurulu ve COM'u seçin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-com.png)

1. Komut `F1` paletini açmak, Azure **IoT Aygıt Çalışma Tezgahı yazın ve seçin: Aygıt Ayarlarını Yapılandır... seçeneğini tıklatın,** ardından **Config Aygıt Bağlantı String > Select IoT Hub Aygıt Bağlantı Dizesini**seçin.

1. DevKit'te **A tuşuna**basılı tutun, **sıfırlama** düğmesine basın ve bırakın ve ardından **A düğmesini**bırakın. DevKit'iniz yapılandırma moduna girer ve bağlantı dizesini kaydeder.

    ![Bağlantı dizesi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connection-string.png)

1. Yeniden `F1` tıklatın, **Azure IoT Aygıt Çalışma Tezgahı**yazın ve seçin: Aygıt Kodunu Yükle. Kodu derlemeye ve DevKit'e yüklemeye başlar.

    ![Arduino yükleme](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/arduino-upload.png)

DevKit yeniden başlatılır ve kodu çalıştırmaya başlar.

> [!NOTE]
> Herhangi bir hata veya kesinti varsa, komutu yeniden çalıştırarak her zaman kurtarabilirsiniz.

## <a name="test-the-project"></a>Projeyi test edin

### <a name="view-the-telemetry-sent-to-azure-iot-hub"></a>Azure IoT Hub'ına gönderilen telemetriyi görüntüleme

Serial Monitor'u açmak için durum çubuğundaki güç fişi simgesine tıklayın:

![Seri monitör](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/serial-monitor.png)

Aşağıdaki sonuçları gördüğünüzde örnek uygulama başarıyla çalışır:

* Seri Monitör, IoT Hub'ına gönderilen iletiyi görüntüler.
* MXChip IoT DevKit'in LED'i yanıp sönüyor.

![Seri monitör çıkışı](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/result-serial-output.png)

### <a name="view-the-telemetry-received-by-azure-iot-hub"></a>Azure IoT Hub tarafından alınan telemetriyi görüntüleme

IoT Hub'da aygıttan buluta (D2C) iletileri izlemek için [Azure IoT Araçlarını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) kullanabilirsiniz.

1. Azure [portalında](https://portal.azure.com/)oturum açın, oluşturduğunuz IoT Hub'ını bulun.

    ![Azure portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-hub-portal.png)

1. Paylaşılan **erişim ilkeleri** bölmesinde, **iothubowner ilkesini**tıklatın ve IoT hub'ınızın Bağlantı dizesini yazın.

    ![Azure IoT Hub bağlantı dizesi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-portal-conn-string.png)

1. VS Kodu'nda `F1`Azure IoT Hub'ı yazın ve **seçin: IoT Hub Bağlantı Dizesini Ayarlayın.** Bağlantı dizesini kopyalayın.

    ![Azure IoT Hub bağlantı dizelerini ayarlama](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/set-iothub-connection-string.png)

1. Sağdaki **AZURE IOT HUB Aygıtları** bölmesini genişletin, oluşturduğunuz aygıt adına sağ tıklayın ve Yerleşik **Etkinlik Bitiş Noktasını İzle'yi**seçin.

    ![D2C Mesajını İzle](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/monitor-d2c.png)

1. **OUTPUT** bölmesinde, IoT Hub'ına gelen D2C iletilerini görebilirsiniz.

    ![D2C iletisi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/d2c-output.png)

## <a name="review-the-code"></a>Kodu gözden geçirin

Ana `GetStarted.ino` Arduino çizim dosyasıdır.

![D2C iletisi](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/code.png)

Aygıt telemetrisinin Azure IoT Hub'ına nasıl `utility.cpp` gönderildiğini görmek için dosyayı aynı klasörde açın. IoT DevKit'te sensörlerin ve çevre birimlerinin nasıl kullanılacağını öğrenmek için [API Referans'ı](https://microsoft.github.io/azure-iot-developer-kit/docs/apis/arduino-language-reference/) görüntüleyin.

Kullanılan, `DevKitMQTTClient` [Microsoft Azure IoT SDK'larından ve C kitaplıklarından azure IoT](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client) Hub ile etkileşimde bulunduğu **iothub_client** bir paketleyicidir.

## <a name="problems-and-feedback"></a>Sorunlar ve geri bildirim

Sorunlarla karşılaşırsanız, [IoT DevKit SSS'de](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) bir çözüm olup olmadığını kontrol edebilir veya [Gitter'den](https://gitter.im/Microsoft/azure-iot-developer-kit)bize ulaşabilirsiniz. Ayrıca bu sayfada bir yorum bırakarak bize geribildirim verebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir MXChip IoT DevKit'i IoT hub'ınıza başarıyla bağladınız ve yakalanan sensör verilerini IoT hub'ınıza gönderdiniz.

[!INCLUDE [iot-hub-get-started-az3166-next-steps](../../includes/iot-hub-get-started-az3166-next-steps.md)]
