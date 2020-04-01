---
title: Bir IoT Tak ve Çalıştır Önizleme cihazı oluşturma ve test etme | Microsoft Dokümanlar
description: Bir aygıt geliştiricisi olarak, bir IoT Tak ve Çalıştır Önizleme aygıtı için yeni bir aygıt yetenek modeli oluşturmak ve test etmek için VS Code'un nasıl kullanılacağı hakkında bilgi edinin.
author: dominicbetts
ms.author: dobett
ms.date: 12/30/2019
ms.topic: tutorial
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: 720b3e56e1dd45bd2940b337adefa6ebdaa2e5a1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76719730"
---
# <a name="tutorial-create-and-test-a-device-capability-model-using-visual-studio-code"></a>Öğretici: Visual Studio Code kullanarak bir aygıt yetenek modeli oluşturma ve test etme

Bu öğretici, bir aygıt geliştiricisi olarak, bir _aygıt yeteneği modeli_oluşturmak için Visual Studio Code'u nasıl kullanacağınızı gösterir. Modeli, buluttaki bir Azure IoT Hub örneğine bağlanan bir aygıtta çalıştırmak için iskelet kodu oluşturmak için kullanabilirsiniz.

Oluşturulan iskelet kodunun nasıl oluşturulabildiğini açıklayan bu öğreticideki bölüm, Windows'u kullandığınızı varsayar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Aygıt yetenek modeli oluşturma
> * Modelden iskelet aygıt kodu oluşturma
> * Oluşturulan koddaki saplamaları uygulama
> * Bir IoT hub'ı ile etkileşimleri test etmek için kodu çalıştırma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticideki aygıt yeteneği modeliyle çalışmak için şunları yapmanız gerekir:

* [Visual Studio Code](https://code.visualstudio.com/download): VS Kodu birden fazla platform için kullanılabilir
* VS Kodu uzantısı paketi [için Azure IoT Araçları.](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) Uzantısı paketini VS Kodu'na yüklemek için aşağıdaki adımları kullanın:

    1. VS Kodu'nda **Uzantılar** sekmesini seçin.
    1. Azure **IoT Araçlarını**arayın.
    1. **Yükle**’yi seçin.

Bu öğreticide Windows'da oluşturulan C kodunu oluşturmak için şunları yapmanız gerekir:

* **C++ oluşturma araçları** ve **NuGet paket yöneticisi bileşen** iş yükleriyle Visual Studio için Araçlar [oluşturun.](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16) Veya visual [studio (Topluluk, Profesyonel veya Kurumsal)](https://visualstudio.microsoft.com/downloads/) 2019, 2017 veya 2015'e sahipseniz ve aynı iş yüklerini yüklüyorsanız.
* [Git](https://git-scm.com/download)
* [CMake](https://cmake.org/download/)

Bu eğitimde cihaz kodunuzu test etmek için şunları yapmanız gerekir:

* [Azure IoT gezgini.](https://github.com/Azure/azure-iot-explorer/releases)
* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="model-your-device"></a>Cihazınızı modelle

Aygıt yeteneği modeli oluşturmak için _dijital ikiz tanımlı dili_ kullanırsınız. Bir model genellikle birden çok _arabirim_ tanımı dosyaları ve tek bir model dosyası oluşur. **VS Kodu için Azure IoT Araçları,** bu JSON dosyalarını oluşturmanıza ve değiştirmenize yardımcı olacak araçlar içerir.

### <a name="create-the-interface-file"></a>Arabirim dosyasını oluşturma

VS Kodu'ndaki IoT aygıtınızın özelliklerini tanımlayan bir arayüz dosyası oluşturmak için:

1. **Aygıt modeli**adlı bir klasör oluşturun.

1. VS Kodunu başlatın ve komut paletini açmak için **Ctrl+Shift+P** kullanın.

1. **Tak ve Çalıştır'ı** girin ve ardından **IoT Fiş & Play: Create Interface** komutunu seçin.

1. Oluşturduğunuz **aygıt modeli** klasörüne göz atın ve seçin.

1. Daha sonra arayüz adı olarak **EnvironmentalSensor'a** girin ve **Enter**tuşuna basın. VS Code **EnvironmentalSensor.interface.json**adlı bir örnek arayüz dosyası oluşturur.

1. Bu dosyanın içeriğini aşağıdaki JSON ile `{your name}` değiştirin ve alandaki benzersiz bir değerle değiştirin. `@id` Yalnızca a-z, A-Z, 0-9 karakterlerini kullanın ve altını çizin. Daha fazla bilgi için [Bkz. Dijital İkiz tanımlayıcı biçimi.](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL#digital-twin-identifier-format) Arabirim kimliği, arabirimi depoya kaydetmek için benzersiz olmalıdır:

    ```json
    {
      "@id": "urn:{your name}:EnvironmentalSensor:1",
      "@type": "Interface",
      "displayName": "Environmental Sensor",
      "description": "Provides functionality to report temperature, humidity. Provides telemetry, commands and read-write properties",
      "comment": "Requires temperature and humidity sensors.",
      "contents": [
        {
          "@type": "Property",
          "displayName": "Device State",
          "description": "The state of the device. Two states online/offline are available.",
          "name": "state",
          "schema": "boolean"
        },
        {
          "@type": "Property",
          "displayName": "Customer Name",
          "description": "The name of the customer currently operating the device.",
          "name": "name",
          "schema": "string",
          "writable": true
        },
        {
          "@type": "Property",
          "displayName": "Brightness Level",
          "description": "The brightness level for the light on the device. Can be specified as 1 (high), 2 (medium), 3 (low)",
          "name": "brightness",
          "writable": true,
          "schema": "long"
        },
        {
          "@type": [
            "Telemetry",
            "SemanticType/Temperature"
          ],
          "description": "Current temperature on the device",
          "displayName": "Temperature",
          "name": "temp",
          "schema": "double",
          "unit": "Units/Temperature/fahrenheit"
        },
        {
          "@type": [
            "Telemetry",
            "SemanticType/Humidity"
          ],
          "description": "Current humidity on the device",
          "displayName": "Humidity",
          "name": "humid",
          "schema": "double",
          "unit": "Units/Humidity/percent"
        },
        {
          "@type": "Telemetry",
          "name": "magnetometer",
          "displayName": "Magnetometer",
          "comment": "This shows a complex telemetry that contains a magnetometer reading.",
          "schema": {
            "@type": "Object",
            "fields": [
              {
                "name": "x",
                "schema": "integer"
              },
              {
                "name": "y",
                "schema": "integer"
              },
              {
                "name": "z",
                "schema": "integer"
              }
            ]
          }
        },
        {
          "@type": "Command",
          "name": "turnon",
          "response": {
            "name": "turnon",
            "schema": "string"
          },
          "comment": "This Commands will turn-on the LED light on the device.",
          "commandType": "synchronous"
        },
        {
          "@type": "Command",
          "name": "turnoff",
          "comment": "This Commands will turn-off the LED light on the device.",
          "response": {
            "name": "turnoff",
            "schema": "string"
          },
          "commandType": "synchronous"
        }
      ],
      "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
    }
    ```

    Bu arabirim, Müşteri **Adı,** **Sıcaklık**gibi telemetri türleri ve **turnon**gibi komutlar gibi aygıt özelliklerini tanımlar.

1. Bu arabirim dosyasının sonunda **yanıp sönme** adı verilen bir komut özelliği ekleyin. Komutu eklemeden önce virgül eklediğinizden emin olun. Intellisense, autocomplete ve doğrulamanın bir arabirim tanımını ne kadar da iyi bir şekilde yeniden oluşturmanıza yardımcı olabileceğini görmek için tanımı yazmayı deneyin:

    ```json
    {
      "@type": "Command",
      "description": "This command will begin blinking the LED for given time interval.",
      "name": "blink",
      "request": {
        "name": "blinkRequest",
        "schema": {
          "@type": "Object",
          "fields": [
            {
              "name": "interval",
              "schema": "long"
            }
          ]
        }
      },
      "response": {
        "name": "blinkResponse",
        "schema": "string"
      },
      "commandType": "synchronous"
    }
    ```

1. Dosyayı kaydedin.

### <a name="create-the-model-file"></a>Model dosyasını oluşturma

Model dosyası, IoT Tak ve Çalıştır cihazınızın uyguladığı arabirimleri belirtir. Bir modelde genellikle en az iki arabirim vardır - cihazınızın belirli özelliklerini tanımlayan bir veya daha fazla arabirim ve tüm IoT Tak ve Çalıştır aygıtlarının uygulaması gereken standart bir arayüz.

IoT Tak ve Çalıştır cihazınızın VS Kodu'nda uyguladığı arabirimleri belirten bir model dosyası oluşturmak için:

1. Komut paletini açmak için **Ctrl+Shift+P'yi** kullanın.

1. **Tak ve Çalıştır'ı** girin ve ardından **IoT Fişi & Play: Yetenek Modeli Oluştur** komutunu seçin. Ardından modelin adı olarak **SensorboxModel'i** girin. VS Code **SensorboxModel.capabilitymodel.json**adlı bir örnek arayüz dosyası oluşturur.

1. Bu dosyanın içeriğini aşağıdaki JSON ile `{your name}` değiştirin `@id` ve `EnvironmentalSensor` alandaki ve arabirimdeki değiştirmeyi **EnvironmentalSensor.interface.json** dosyasında kullandığınız değerle değiştirin. Arabirim kimliği, arabirimi depoya kaydetmek için benzersiz olmalıdır:

    ```json
    {
      "@id": "urn:{your name}:SensorboxModel:1",
      "@type": "CapabilityModel",
      "displayName": "Environmental Sensorbox Model",
      "implements": [
        {
          "schema": "urn:{your name}:EnvironmentalSensor:1",
          "name": "environmentalSensor"
        },
        {
          "schema": "urn:azureiot:DeviceManagement:DeviceInformation:1",
          "name": "deviceinfo"
        }
      ],
      "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
    }
    ```

    Model, **EnvironmentalSensor** arabiriminizi ve standart **DeviceInformation** arabirimini uygulayan bir aygıt tanımlar.

1. Dosyayı kaydedin.

### <a name="download-the-deviceinformation-interface"></a>DeviceInformation arabirimini indirin

Modelden iskelet kodu oluşturmadan önce, *ortak model deposundan* **DeviceInformation'ın** yerel bir kopyasını oluşturmanız gerekir. Ortak model deposu zaten **DeviceInformation** arabirimini içerir.

VS Code kullanarak ortak model deposundan **DeviceInformation** arabirimini indirmek için:

1. Komut paletini açmak için **Ctrl+Shift+P'yi** kullanın.

1. **Tak ve Çalıştır'ı**girin, **Model Deposunu Aç** komutunu seçin ve ardından Ortak Model **Deposu'nu aç'ı**seçin.

1. **Arabirimler'i**seçin, ardından ID `urn:azureiot:DeviceManagement:DeviceInformation:1`ile aygıt bilgi arabirimini seçin ve ardından **İndir'i**seçin.

Artık aygıt yeteneği modelinizi oluşturan üç dosyaya sahipsiniz:

* urn_azureiot_DeviceManagement_DeviceInformation_1.interface.json
* EnvironmentalSensor.interface.json
* SensorboxModel.capabilitymodel.json

## <a name="publish-the-model"></a>Modeli yayımlama

Azure IoT explorer aracının aygıt yeteneği modelinizi okuyabilmek için bunu şirket deponuzda yayımlamanız gerekir. VS Code'dan yayımlamak için şirket deposu için bağlantı dizesi gerekir:

1. [IoT için Azure Sertifikalı portalına](https://aka.ms/ACFI)gidin.

1. Portalda oturum açabilmek için Microsoft _iş hesabınızı_ kullanın.

1. **Şirket deposunu** seçin ve ardından **Bağlantı dizeleri.**

1. Bağlantı dizesini kopyalayın.

Vs Code'da şirket deponuzu açmak için:

1. Komut paletini açmak için **Ctrl+Shift+P'yi** kullanın.

1. **Tak ve Çalıştır'ı** girin ve ardından **IoT Fişi & Oynat: Model Deposu komutunu açın' ı** seçin.

1. Bağlantı dizenizde **Örgütsel Model Deposunu Aç'ı** ve yapıştır'ı seçin.

1. Şirket deponuzu açmak için **Enter** tuşuna basın.

Cihaz yetenek modelinizi ve arayüzlerinizi şirket deponuzda yayımlamak için:

1. Komut paletini açmak için **Ctrl+Shift+P'yi** kullanın.

1. **Tak ve Çalıştır'ı** girin ve ardından **IoT Fiş & Play: Dosyalarını Model Deposu komutuna gönderin'** i seçin.

1. **EnvironmentalSensor.interface.json** ve **SensorboxModel.capabilitymodel.json** dosyalarını seçin ve **Tamam'ı**seçin.

Dosyalarınız artık şirket deponuzda depolanır.

## <a name="generate-code"></a>Kod oluşturma

Modelinizden iskelet C kodu oluşturmak **için VS Kodu için Azure IoT Araçlarını** kullanabilirsiniz. VS Kodu'nda iskelet kodunu oluşturmak için:

1. Komut paletini açmak için **Ctrl+Shift+P'yi** kullanın.

1. **Tak ve Çalıştır'ı** girin ve ardından **IoT Tak'& Oynat: Aygıt Kodu Saplama komutunu oluşturun'u** seçin.

1. **SensorboxModel.capabilitymodel.json** yetenek model dosyanızı seçin.

1. Proje adı olarak **sensorbox_app** girin.

1. Dil olarak **ANSI C'yi** seçin.

1. Bağlanma yolu olarak **Via IoT Hub aygıt bağlantı dizesini** seçin.

1. **Windows'da Proje** Şablonu olarak CMake Project'i seçin.

1. SDK cihazını eklemek için **Via Vcpkg'ı** seçin.

VS Code, c iskeletini oluşturur ve **dosyaları modelkodu** klasöründeki **sensorbox_app** klasöründeki dosyaları kaydeder. VS Code oluşturulan kod dosyalarını içeren yeni bir pencere açar.

## <a name="update-the-generated-code"></a>Oluşturulan kodu güncelleştirme

Kodu oluşturup çalıştırabilmeniz için önce saplanmış özellikleri, telemetriyi ve komutları uygulamanız gerekir.

VS Kodu'nda saplanmış kod için uygulamalar sağlamak için:

1. **SensorboxModel_impl.c** dosyasını açın.

1. Saplı işlevleri uygulamak için kod ekleyin.

1. Yaptığınız değişiklikleri kaydedin.

## <a name="build-the-code"></a>Kodu oluşturma

IoT Tak ve Çalıştır cihazınızı bir Azure IoT hub'ı ile test etmek için kodu çalıştırmadan önce kodu derlemeniz gerekir.

Windows'da kodu oluşturmak ve çalıştırmak için **sensorbox_app** klasöründeki **Readme.md** dosyasındaki yönergeleri izleyin. Aşağıdaki bölümde, aygıt kodunu çalıştırdığınızda kullanılacak bir aygıt bağlantı dizesini alma yönergeleri yer almaktadır.

## <a name="test-the-code"></a>Kodu test edin

Kodu çalıştırdığınızda, IoT Hub'a bağlanır ve örnek telemetri ve özellik değerleri göndermeye başlar. Aygıt ayrıca IoT Hub'dan gönderilen komutlara da yanıt verir. Bu davranışı doğrulamak için:

1. IoT hub'ı oluşturmak için:

    ```azurecli-interactive
    az group create --name environmentalsensorresources --location centralus
    az iot hub create --name {your iot hub name} \
      --resource-group environmentalsensorresources --sku F1
    ```

1. Aygıtı IoT hub'ınıza ekleyin ve bağlantı dizesini alın:

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {your iot hub name} --device-id MyPnPDevice
    az iot hub device-identity show-connection-string --hub-name {your iot hub name} --device-id MyPnPDevice --output table
    ```

    Bağlantı dizesini not edin.

1. Komut isteminde, SDK ve örnekleri oluşturacağınız **azure-iot-sdk-c** klasörüne gidin. Ardından **cmake\\sensorbox_app\\Release** klasörüne gidin.

1. Şu komutu çalıştırın:

    ```cmd
    sensorbox_app.exe {your device connection string}
    ```

1. IoT hub'ınıza bağlı IoT Tak ve Çalıştır aygıtıyla etkileşimkurmak için Azure IoT gezgini aracını kullanın. Daha fazla bilgi için Azure [IoT gezginini yükle'ye](./howto-install-iot-explorer.md)bakın ve kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Artık sertifikaya hazır bir IoT Tak ve Çalıştır oluştursanız, nasıl iş yapılacağını öğrenin:

> [!div class="nextstepaction"]
> [Sertifikasyon için hazır cihaz oluşturma](tutorial-build-device-certification.md)
