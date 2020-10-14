---
title: IoT Tak ve Kullan cihaz istemcisi oluşturmak için MQTT kullanma | Microsoft Docs
description: Azure IoT cihaz SDK 'larını kullanmadan IoT Tak ve Kullan cihaz istemcisi oluşturmak için doğrudan MQTT protokolünü kullanın
author: ericmitt
ms.author: ericmitt
ms.date: 05/13/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: d0ac0f000b6a096ae3de1f4f00a17b64f1948c1e
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92046290"
---
# <a name="use-mqtt-to-develop-an-iot-plug-and-play-device-client"></a>IoT Tak ve Kullan cihaz istemcisi geliştirmek için MQTT kullanın

Her olabiliyorsa IoT Tak ve Kullan cihaz istemcilerinizi oluşturmak için Azure IoT cihaz SDK 'Lardan birini kullanmanız gerekir. Ancak, bellek kısıtlanmış bir cihaz kullanma gibi senaryolarda IoT Hub 'ınız ile iletişim kurmak için bir MQTT kitaplığı kullanmanız gerekebilir.

Bu öğreticideki örnek, [ıssquıto](http://mosquitto.org/) MQTT kitaplığı ve Visual Studio 'Da Çakışan Küreler kullanır. Bu öğreticideki adımlarda geliştirme makinenizde Windows kullandığınızı varsayalım.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Bu öğreticiyi Windows üzerinde gerçekleştirmek için, yerel Windows ortamınıza aşağıdaki yazılımları yüklersiniz:

* [Visual Studio (Community, Professional veya Enterprise)](https://visualstudio.microsoft.com/downloads/) -Visual Studio 'Yu [yüklerken](/cpp/build/vscpp-step-0-installation?preserve-view=true&view=vs-2019) C++ Iş yüküyle **Masaüstü geliştirmeyi** eklediğinizden emin olun
* [Git](https://git-scm.com/download/)
* [CMake](https://cmake.org/download/)

IoT Hub yeni bir cihaz eklemek için *Azure IoT gezgin* aracını kullanın. IoT Hub 'ınızı, Azure IoT Gezgini aracını, [ıot Tak ve kullan hızlı başlangıç ve öğreticiler için ortamınızı ayarlamayı](set-up-environment.md)tamamladıktan sonra yapılandırdınız:

1. **Azure IoT gezgin** aracını başlatın.
1. **IoT Hub 'ları** sayfasında **Bu hub 'da cihazları görüntüle**' yi seçin.
1. **Cihazlar** sayfasında **+ Yeni**' yi seçin.
1. Otomatik olarak oluşturulmuş bir simetrik anahtar kullanan *My-MQTT-Device* adlı bir cihaz oluşturun.
1. **Cihaz kimliği** SAYFASıNDA, **SAS belirtecine sahip bağlantı dizesi**' ni genişletin.
1. **Simetrik anahtar**olarak kullanılacak **birincil anahtarı** seçin, sona erme süresini 60 dakika olarak ayarlayın ve **Oluştur**' u seçin.
1. Oluşturulan **SAS belirteci bağlantı dizesini**kopyalayın, bu değeri öğreticide daha sonra kullanırsınız.

## <a name="clone-sample-repo"></a>Örnek depoyu Kopyala

Örnek depoyu yerel makinenizde uygun bir konuma kopyalamak için aşağıdaki komutu kullanın:

```cmd
git clone https://github.com/Azure-Samples/IoTMQTTSample.git
```

## <a name="install-mqtt-library"></a>MQTT kitaplığı 'nı yükler

Araç, `vcpkg` ıssquıto kitaplığını indirmek ve derlemek için aracını kullanın.

**Vcpkg** deposunu yerel makinenizde uygun bir konuma kopyalamak için aşağıdaki komutu kullanın:

```cmd
git clone https://github.com/Microsoft/vcpkg.git
```

Ortamı hazırlamak için aşağıdaki komutları kullanın `vcpkg` . ' İ çalıştırdığınızda yükseltilmiş bir komut istemi gerekir `vcpkg integrate install` :

```cmd
cd vcpkg
.\bootstrap-vcpkg.bat
.\vcpkg integrate install
```

Tutulma mosquıto kitaplığını indirmek ve derlemek için aşağıdaki komutu kullanın:

```cmd
.\vcpkg install mosquitto:x64-windows
```

## <a name="migrate-the-sample-to-iot-plug-and-play"></a>Örneği IoT 'ye geçirin Tak ve Kullan

### <a name="review-the-non-iot-plug-and-play-sample-code"></a>IoT olmayan Tak ve Kullan örnek kodunu gözden geçirin

Kodu oluşturup çalıştırmadan önce IoT Hub 'ınızın ve cihazınızın ayrıntıları ile güncelleştirin.

Örnek kodu Visual Studio 'da görüntülemek için, *ıotmqttsample\src\windows* klasöründeki *MQTTWin32. sln* çözüm dosyasını açın.

**Çözüm Gezgini**' de, **TelemetryMQTTWin32** projesine sağ tıklayın ve **Başlangıç projesi olarak ayarla**' yı seçin.

**TelemetryMQTTWin32** projesinde **MQTT_Mosquitto. cpp** kaynak dosyasını açın. Bağlantı bilgileri tanımlarını cihaz ayrıntılarıyla güncelleştirin daha önce bir değişiklik yaptınız. Belirteç dizesi yer tutucularını şu şekilde değiştirin:

* `IOTHUBNAME` IoT Hub 'ınızın adını taşıyan tanımlayıcı.
* `DEVICEID` ile tanımlayıcı `my-mqtt-device` .
* `PWD` cihaz için oluşturduğunuz SAS belirteci bağlantı dizesinin doğru bölümü ile tanımlayıcı. Bağlantı dizesinin bir bölümünü içinden sonuna kadar kullanın `SharedAccessSignature sr=` .

Azure IoT Explorer ' ı başlatarak Telemetriyi dinlemeyi başlatın ve kodun düzgün çalıştığını doğrulayın.

Uygulamayı çalıştırın (CTRL + F5), birkaç saniye sonra aşağıdaki gibi görünen çıktıyı görürsünüz:

:::image type="content" source="media/tutorial-use-mqtt/mqtt-sample-output.png" alt-text="MQTT örnek uygulamasından çıkış":::

Azure IoT Explorer 'da, cihazın IoT Tak ve Kullan cihazı olmadığını görebilirsiniz:

:::image type="content" source="media/tutorial-use-mqtt/non-pnp-iot-explorer.png" alt-text="MQTT örnek uygulamasından çıkış":::

### <a name="make-the-device-an-iot-plug-and-play-device"></a>Cihazı IoT Tak ve Kullan cihazı yapın

IoT Tak ve Kullan cihazı bir dizi basit kuralı izlemelidir. Bir cihaz, bağlandığı sırada bir model KIMLIĞI gönderiyorsa, IoT Tak ve Kullan cihazı haline gelir.

Bu örnekte, MQTT bağlantı paketine bir model KIMLIĞI eklersiniz. Model KIMLIĞINI içinde QueryString parametresi olarak geçirin `USERNAME` ve öğesini `api-version` olarak değiştirin `2020-09-30` :

```c
// computed Host Username and Topic
//#define USERNAME IOTHUBNAME ".azure-devices.net/" DEVICEID "/?api-version=2018-06-30"
#define USERNAME IOTHUBNAME ".azure-devices.net/" DEVICEID "/?api-version=2020-09-30&model-id=dtmi:com:example:Thermostat;1"
#define PORT 8883
#define HOST IOTHUBNAME //".azure-devices.net"
#define TOPIC "devices/" DEVICEID "/messages/events/"
```

Örneği yeniden derleyin ve çalıştırın.

Cihaz ikizi Şu anda model KIMLIĞINI içermektedir:

:::image type="content" source="media/tutorial-use-mqtt/model-id-iot-explorer.png" alt-text="MQTT örnek uygulamasından çıkış":::

Artık IoT Tak ve Kullan bileşenine gidebilirsiniz:

:::image type="content" source="media/tutorial-use-mqtt/components-iot-explorer.png" alt-text="MQTT örnek uygulamasından çıkış":::

Artık, modelinizde tanımlanan telemetri, özellik ve komutları uygulamak için cihaz kodunuzu değiştirebilirsiniz. Mosquıto kitaplığını kullanarak termostat cihazının örnek bir uygulamasını görmek için bkz. GitHub 'da [Windows üzerinde ıOT SDK olmadan Azure ıothub Ile MQTT PNP kullanma](https://github.com/Azure-Samples/IoTMQTTSample/tree/master/src/Windows/PnPMQTTWin32) .

> [!NOTE]
>İstemci, `IoTHubRootCA_Baltimore.pem` bağlandığı IoT Hub 'ının kimliğini doğrulamak için kök sertifika dosyasını kullanır.

### <a name="mqtt-topics"></a>MQTT konuları

Aşağıdaki tanımlar, cihazın IoT Hub 'ına bilgi göndermek için kullandığı MQTT konularına yöneliktir. Daha fazla bilgi için bkz. [MQTT protokolünü kullanarak IoT Hub 'ınız Ile Iletişim kurma](../iot-hub/iot-hub-mqtt-support.md):

* , `DEVICE_CAPABILITIES_MESSAGE` Cihazın uyguladığı arabirimleri raporlamak için kullandığı konuyu tanımlar.
* , `DEVICETWIN_PATCH_MESSAGE` Cihazın IoT Hub 'ına Özellik güncelleştirmelerini raporlamak için kullandığı konuyu tanımlar.
* , `DEVICE_TELEMETRY_MESSAGE` Cihazın IoT Hub 'ınıza telemetri göndermek için kullandığı konuyu tanımlar.

MQTT hakkında daha fazla bilgi için [Azure IoT GitHub deposu Için MQTT örnekleri](https://github.com/Azure-Samples/IoTMQTTSample/) sayfasını ziyaret edin.
  
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IoT Tak ve Kullan kurallarını izlemek için bir MQTT cihaz istemcisini nasıl değiştireceğiniz hakkında daha fazla öğrendiniz. IoT Tak ve Kullan hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Mimari](concepts-architecture.md)

MQTT protokolü için IoT Hub desteği hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [MQTT protokolünü kullanarak IoT Hub 'ınız ile iletişim kurma](../iot-hub/iot-hub-mqtt-support.md)