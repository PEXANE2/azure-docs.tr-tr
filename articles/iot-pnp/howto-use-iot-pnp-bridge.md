---
title: Linux veya Windows üzerinde çalışan bir IoT Tak ve Kullan köprüsü örneğini bir IoT Hub 'ına bağlama | Microsoft Docs
description: IoT Hub 'ına bağlanan Linux veya Windows üzerinde IoT Tak ve Kullan köprüsü oluşturun ve çalıştırın. Cihaz tarafından hub 'a gönderilen bilgileri görüntülemek için Azure IoT gezgin aracını kullanın.
author: usivagna
ms.author: ugans
ms.date: 09/22/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: eedd19189d1e1ccedd3d505aecf407aca8fca831
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94413369"
---
# <a name="how-to-connect-an--iot-plug-and-play-bridge-sample-running-on-linux-or-windows-to-iot-hub"></a>Linux veya Windows üzerinde çalışan bir IoT Tak ve Kullan köprüsü örneğini IoT Hub 'a bağlama

Bu nasıl yapılır, IoT Tak ve Kullan köprüsünün örnek çevresel bağdaştırıcısının nasıl oluşturulduğunu, IoT Hub 'ınıza nasıl bağlayakullanacağınızı ve gönderdiği Telemetriyi görüntülemek için Azure IoT gezgin aracını nasıl kullanacağınızı gösterir. IoT Tak ve Kullan köprüsü C dilinde yazılır ve C için Azure IoT cihaz SDK 'sını içerir. Bu öğreticinin sonuna kadar IoT Tak ve Kullan köprüsünü çalıştırabiliyor ve Azure IoT Explorer 'da BT rapor telemetrisini görmeniz gerekir: :::image type="content" source="media/concepts-iot-pnp-bridge/iot-pnp-bridge-explorer-telemetry.png" alt-text="ıot Tak ve kullan Bridge 'ten bildirilen telemetri (nem, sıcaklık) tablosu Ile Azure IoT Gezginini gösteren ekran görüntüsü.":::

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı Linux veya Windows üzerinde çalıştırabilirsiniz. Bu nasıl yapılır kılavuzundaki kabuk komutları ' ' yol ayırıcıları için Windows kuralına uyar `\` , Linux üzerinde takip ediyorsanız, bu ayırıcıları ' ' için değiştirmeyi unutmayın `/` .

Önkoşullar işletim sistemine göre farklılık gösterir:

### <a name="linux"></a>Linux

Bu hızlı başlangıç, Ubuntu Linux kullandığınızı varsayar. Bu hızlı başlangıçtaki adımlar Ubuntu 18,04 kullanılarak test edilmiştir.

Bu hızlı başlangıcı Linux üzerinde gerçekleştirmek için, yerel Linux ortamınıza aşağıdaki yazılımı yüklersiniz:

Komutunu kullanarak **GCC** , **Git** , **CMake** ve tüm gerekli bağımlılıkları yükler `apt-get` :

```sh
sudo apt-get update
sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
```

Öğesinin sürümünün `cmake` **2.8.12** üzerinde olduğunu ve **GCC** 'nin sürümünün **4.4.7** 'in üzerinde olduğunu doğrulayın.

```sh
cmake --version
gcc --version
```

### <a name="windows"></a>Windows

Bu hızlı başlangıcı Windows üzerinde gerçekleştirmek için, yerel Windows ortamınıza aşağıdaki yazılımı yüklersiniz:

* [Visual Studio (Community, Professional veya Enterprise)](https://visualstudio.microsoft.com/downloads/) -Visual Studio 'Yu [yüklerken](/cpp/build/vscpp-step-0-installation?preserve-view=true&view=vs-2019) C++ Iş yüküyle **Masaüstü geliştirmeyi** eklediğinizden emin olun.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).

### <a name="azure-iot-explorer"></a>Azure IoT Gezgini

Bu hızlı başlangıç bölümünün ikinci bölümünde örnek cihazla etkileşime geçmek için **Azure IoT gezgin** aracını kullanın. İşletim sisteminiz için [Azure IoT Explorer 'ın en son sürümünü indirin ve yükleyin](./howto-use-iot-explorer.md) .

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub 'ınız için _IoT Hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın. Bu bağlantı dizesini, daha sonra bu hızlı başlangıçta kullanacaksınız:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

> [!TIP]
> IoT Hub bağlantı dizesini bulmak için Azure IoT gezgin aracını da kullanabilirsiniz.

Hub 'a eklediğiniz cihazın _Cihaz bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın. Bu bağlantı dizesini, daha sonra bu hızlı başlangıçta kullanacaksınız:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

## <a name="view-the-model"></a>Modeli görüntüleme

Daha sonraki adımlarda Azure IoT gezginini kullanarak, IoT Hub 'ınıza bağlanırken cihazı görüntüleyebilirsiniz. Azure IoT Explorer 'ın, cihazın gönderdiği **model kimliğiyle** eşleşen bir model dosyasının yerel kopyasına ihtiyacı vardır. Model dosyası, IoT Gezgini 'nin cihazınızın uyguladığı telemetri, Özellikler ve komutları görüntülemesini sağlar.

Kodu aşağıdaki adımda karşıdan yüklediğinizde, klasörü altındaki örnek model dosyalarını içerir `pnpbridge/docs/schema` . Azure IoT Gezgini 'ni sağlamak için:

1. Yerel makinenizde *modeller* adlı bir klasör oluşturun.
1. [EnvironmentalSensor.js](https://aka.ms/iot-pnp-bridge-env-model) GÖRÜNTÜLEYIN ve json dosyasını *modeller* klasörüne kaydedin
1. [RootBridgeSampleDevice.js](https://aka.ms/iot-pnp-bridge-root-model) GÖRÜNTÜLEYIN ve json dosyasını *modeller* klasörüne kaydedin.

## <a name="download-the-code"></a>Kodu indirme

Seçtiğiniz dizinde bir komut istemi açın. [Iot Tak ve Kullan köprüsü](https://aka.ms/iotplugandplaybridge) GitHub deposunu bu konuma kopyalamak için aşağıdaki komutu yürütün:

```cmd
git clone https://github.com/Azure/iot-plug-and-play-bridge.git
```

IoT Tak ve Kullan köprü deposunu makinenize kopyaladıktan sonra, bir yönetici komut istemi açın ve kopyalanmış deponun dizinine gidin:

```cmd
cd pnpbridge
git submodule update --init --recursive
```

Bu işlemin tamamlanması birkaç dakika sürer.

>[!NOTE]
> Git kopyası alt modülü güncelleştirmesi başarısız olduğunda sorunlarla karşılaşırsanız, Windows dosya yollarında ve git 'in bu bilinen bir sorundur: https://github.com/msysgit/git/pull/110 . Sorunu çözmek için aşağıdaki komutu deneyebilirsiniz: `git config --system core.longpaths true`

## <a name="setting-up-the-configuration-json"></a>Yapılandırma JSON 'ı ayarlanıyor

IoT Tak ve Kullan köprü deposunu makinenize kopyaladıktan sonra, `pnpbridge/docs/schema` kopyalanan deponun dizinine gidin ve bu [yapılandırmayı JSON](https://aka.ms/iot-pnp-bridge-env-config) veya `config.json` köprünün çevresel algılayıcı örneği için bulabilirsiniz. [Iot Tak ve kullan köprü kavramları belgesinde](concepts-iot-pnp-bridge.md)yapılandırma dosyaları hakkında daha fazla bilgi edinebilirsiniz.

Alanı için, `root-_interface_model_id` cihazınızın modelini tanımlayan ıot Tak ve kullan MODELI kimliğini kopyalamanız gerekir. Bu örnekte bu değer `dtmi:com:example:SampleDevice;1`’dur. Dosyadaki **pnp_bridge_parameters** düğümü altında aşağıdaki parametreleri değiştirin `config.json` :

* connection_string 
* symmetric_key 

>[!NOTE]
> Symmetric_key bağlantı dizesindeki SAS anahtarıyla aynı olmalıdır.

  ```JSON
    {
      "connection_parameters": {
        "connection_type" : "connection_string",
        "connection_string" : "[CONNECTION STRING]",
        "root_interface_model_id": "[To fill in]",
        "auth_parameters" : {
          "auth_type" : "symmetric_key",
          "symmetric_key" : "[SYMMETRIC KEY]"
        }
      }
    }
  }
  ```

 Bu dosya doldurulduktan sonra, `config.json` şuna benzemelidir:

   ```JSON
    {
      "connection_parameters": {
        "connection_type" : "connection_string",
        "connection_string" : "[CONNECTION STRING]",
        "root_interface_model_id": "dtmi:com:example:SampleDevice;1",
        "auth_parameters" : {
          "auth_type" : "symmetric_key",
          "symmetric_key" : "[SYMMETRIC KEY]"
        }
      }
    }
  }
```

 Köprüyü oluşturduktan sonra, bunu `config.json` köprü ile aynı dizine yerleştirmeniz veya çalıştırıldığında yolunu belirtmeniz gerekir.

## <a name="build-the-iot-plug-and-play-bridge"></a>IoT Tak ve Kullan köprüsü oluşturma

Depo dizinindeki *pnpbridge* klasörüne gidin.

Windows için, [Visual Studio için geliştirici komut istemi](/dotnet/framework/tools/developer-command-prompt-for-vs)aşağıdakileri çalıştırın:

```cmd
cd scripts\windows
build.cmd
```

Linux için benzer şekilde aşağıdakileri çalıştırın:

```bash
cd scripts/linux
./setup.sh
./build.sh
```

>[!TIP]
>Windows 'da, Visual Studio 2019 ' de CMake komutu tarafından oluşturulan çözümü açabilirsiniz. CMake dizinindeki *azure_iot_pnp_bridge. sln* proje dosyasını açın ve *pnpbridge_bin* projesini çözümdeki başlangıç projesi olarak ayarlayın. Artık Visual Studio 'da örneği oluşturabilir ve hata ayıklama modunda çalıştırabilirsiniz.

## <a name="start-the-iot-plug-and-play-bridge"></a>IoT Tak ve Kullan köprüsü 'nü başlatma

 *Pnpbridge* klasörüne giderek ve komut isteminde aşağıdaki komutu çalıştırarak çevresel algılayıcılar için IoT Tak ve kullan köprü örneğini başlatın:

```bash
 cd cmake/pnpbridge_linux/src/adapters/samples/environmental_sensor/
./pnpbridge_environmentalsensor

```

```cmd
REM Windows
cd cmake\pnpbridge_x86\src\adapters\samples\environmental_sensor
Debug\pnpbridge_environmentalsensor.exe
```

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT Gezginini Kullanma

Cihaz istemcisi örneği başladıktan sonra, çalıştığını doğrulamak için Azure IoT gezgin aracını kullanın.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Hub 'ına IoT Tak ve Kullan cihazını bağlamayı öğrendiniz. IoT Tak ve Kullan cihazlarınızla etkileşim kuran bir çözüm oluşturma hakkında daha fazla bilgi edinmek için bkz.:

* [IoT Tak ve Kullan köprüsü nedir?](./concepts-iot-pnp-bridge.md)
* [IoT Tak ve Kullan köprüsü için GitHub geliştirici başvurusuna bakın](https://aka.ms/iot-pnp-bridge-dev-doc)
* [GitHub 'da IoT Tak ve Kullan Köprüsü](https://aka.ms/iotplugandplaybridge)
