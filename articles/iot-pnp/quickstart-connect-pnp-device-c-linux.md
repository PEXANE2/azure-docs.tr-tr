---
title: IoT Tak ve Çalıştır Preview örnek cihaz kodunu IoT Hub'ına (Linux) bağlayın | Microsoft Dokümanlar
description: Linux'ta bir IoT hub'ına bağlanan IoT Tak ve Çalıştır örnek aygıt kodu oluşturun ve çalıştırın. Aygıt tarafından hub'a gönderilen bilgileri görüntülemek için Azure CLI'yi kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 12/23/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 8134c0a97f6350cfa2cf616695c5990618455393
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75531279"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-running-on-linux-to-iot-hub-c-linux"></a>Quickstart: Linux üzerinde çalışan örnek bir IoT Tak ve Çalıştır Önizleme cihazı uygulamasını IoT Hub'ına (C Linux) bağlayın

[!INCLUDE [iot-pnp-quickstarts-2-selector.md](../../includes/iot-pnp-quickstarts-2-selector.md)]

Bu hızlı başlangıç, Linux'ta örnek bir IoT Tak ve Çalıştır aygıtı uygulamasını nasıl oluşturabileceğinizi, IoT bub'unuza nasıl bağlayabileceğinizi ve hub'a gönderdiği bilgileri görüntülemek için Azure CLI'yi nasıl kullanacağınızı gösterir. Örnek uygulama C ile yazılır ve C için Azure IoT aygıt SDK'ya eklenir. Bir çözüm geliştiricisi, herhangi bir aygıt kodunu görüntülemeye gerek kalmadan bir IoT Tak ve Çalıştır aygıtının özelliklerini anlamak için Azure CLI'yi kullanabilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç, Ubuntu Linux kullandığınızı varsayar. Bu öğreticideki adımlar Ubuntu 18.04 kullanılarak test edilmiştir.

Bu hızlı başlatmayı tamamlamak için, yerel Linux makinenize aşağıdaki yazılımı yüklemeniz gerekir:

**Install GCC**, **Git**, **cmake**, `apt-get` ve tüm bağımlılıkları komutunu kullanarak:

```sh
sudo apt-get update
sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
```

`cmake` **2.8.12'nin** üzerinde ve **GCC'nin** sürümünün **4.4.7'nin**üzerinde olduğunu doğrulayın.

```sh
cmake --version
gcc --version
```

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, Azure IoT Hub Aygıtı C SDK'yı klonlamak ve oluşturmak için kullanabileceğiniz bir geliştirme ortamı hazırlayın.

Seçtiğiniz dizinde bir komut istemi açın. [Azure IoT C SDK'larını ve Kitaplıkları](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu bu konuma klonlamak için aşağıdaki komutu uygulayın:

```bash
git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
```

Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

## <a name="build-the-code"></a>Kodu oluşturma

Dahil edilen örnek kodu oluşturmak için SDK aygıtını kullanırsınız. Oluşturduğunuz uygulama, bir IoT hub'ına bağlanan bir aygıtı simüle eder. Uygulama telemetri ve özellikleri gönderir ve komutları alır.

1. Aygıt `cmake` SDK kök klasöründe bir alt dizini oluşturun ve bu klasöre gidin:

    ```bash
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Aygıt SDK ve oluşturulan kod saplama oluşturmak için aşağıdaki komutları çalıştırın:

    ```bash
    cmake ..
    cmake --build .
    ```

## <a name="update-your-model-repository"></a>Model deponuzu güncelleştirme

Örneği çalıştırmadan önce, aygıt yetenek modelini ve arabirim tanımlarını şirket model deponuza ekleyin:

1. Microsoft çalışmaveya okul hesabınızla Azure [Sertifikalı IoT portalında](https://preview.catalog.azureiotsolutions.com) veya hesabınız varsa Microsoft İş Ortağı Kimliğinizle oturum açın.

1. Şirket deposu nu seçin ve ardından Yetenek **modellerini** **seçin.**

1. **Yeni'yi** seçin ve sonra **Yükle'yi**seçin.

1. Aygıt SDK `digitaltwin_client/samples` kök klasöründeki klasördeki dosyayı `SampleDevice.capabilitymodel.json` seçin. Model dosyasını deponuza yüklemek için **Aç** ve ardından **Kaydet'i** seçin.

1. **Şirket deposunu** ve ardından **Arayüzleri**seçin.

1. **Yeni'yi** seçin ve sonra **Yükle'yi**seçin.

1. Aygıt SDK `digitaltwin_client/samples/digitaltwin_sample_environmental_sensor` kök klasöründeki klasördeki dosyayı `EnvironmentalSensor.interface.json` seçin. Arayüz dosyasını deponuza yüklemek için **Aç** ve **ardından Kaydet'i** seçin.

1. **Şirket deposunu** seçin ve ardından **Bağlantı dizeleri.** Bu hızlı başlangıçta daha sonra kullandığınız gibi, ilk _şirket modeli depo bağlantı dizesini_not edin.

## <a name="run-the-device-sample"></a>Aygıt örneğini çalıştırma

IoT hub'ınıza telemetri gönderen bir IoT Tak ve Çalıştır aygıtını simüle etmek için SDK'da örnek bir uygulama çalıştırın. Örnek uygulamayı çalıştırmak için:

1. `cmake` Klasörden yürütülebilir dosyayı içeren klasöre gidin:

    ```bash
    cd digitaltwin_client/samples/digitaltwin_sample_device
    ```

1. Çalıştırılabilir dosyayı çalıştırın:

    ```bash
    ./digitaltwin_sample_device "<YourDeviceConnectionString>"
    ```

Aygıt artık komutları ve özellik güncelleştirmelerini almaya hazır ve hub'a telemetri verileri göndermeye başladı. Sonraki adımları tamamlarken numunenin çalışmasını engelleyin.

### <a name="use-the-azure-iot-cli-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT CLI'yi kullanın

Aygıt istemcisi örneği başladıktan sonra Azure CLI ile çalıştığını doğrulayın.

Örnek aygıtın gönderdiği telemetriyi görüntülemek için aşağıdaki komutu kullanın. Çıktıda herhangi bir telemetri görmeden önce bir veya iki dakika beklemeniz gerekebilir:

```azurecli-interactive
az iot dt monitor-events --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

Aygıt tarafından gönderilen özellikleri görüntülemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az iot dt list-properties --hub-name <YourIoTHubName> --device-id <YourDeviceID> --interface sensor --source private --repo-login "<YourCompanyModelRepositoryConnectionString>"
```
[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir IoT Tak ve Çalıştır aygıtını bir IoT hub'ına nasıl bağlayabileceğinizi öğrendiniz. IoT Tak ve Çalıştır aygıtlarınızla etkileşimedebilen bir çözümü nasıl oluşturabilirsiniz hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Nasıl Yapılsın: Aygıta bağlanma ve aygıtla etkileşim kurma](howto-develop-solution.md)
