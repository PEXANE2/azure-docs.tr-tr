---
title: IoT Tak ve Çalıştır Preview örnek aygıt kodunu IoT Hub'ına (Windows) bağlayın | Microsoft Dokümanlar
description: Windows'da bir IoT hub'ına bağlanan IoT Tak ve Çalıştır örnek aygıt kodu oluşturun ve çalıştırın. Aygıt tarafından hub'a gönderilen bilgileri görüntülemek için Azure IoT gezgini aracını kullanın.
author: ChrisGMsft
ms.author: chrisgre
ms.date: 12/26/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 1c8b6a5d133d8838c2c7a23f8881092affa424dc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75531221"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-running-on-windows-to-iot-hub-c-windows"></a>Quickstart: Windows'ta çalışan örnek bir IoT Tak ve Çalıştır Önizleme aygıtı uygulamasını IoT Hub'ına (C Windows) bağlayın

[!INCLUDE [iot-pnp-quickstarts-2-selector.md](../../includes/iot-pnp-quickstarts-2-selector.md)]

Bu hızlı başlangıç, örnek bir IoT Tak ve Çalıştır aygıtı uygulamasını nasıl oluşturabileceğinizi, IoT hub'ınıza nasıl bağlayabileceğinizi ve hub'a gönderdiği bilgileri görüntülemek için Azure IoT gezgini aracını nasıl kullanacağınızı gösterir. Örnek uygulama C ile yazılır ve Azure IoT Hub Aygıt C SDK'ya eklenir. Bir çözüm geliştiricisi, herhangi bir aygıt kodunu görüntülemeye gerek kalmadan bir IoT Tak ve Çalıştır aygıtının özelliklerini anlamak için Azure IoT explorer aracını kullanabilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlatmayı tamamlamak için, yerel makinenize aşağıdaki yazılımı yüklemeniz gerekir:

* [Visual Studio (Topluluk, Profesyonel veya Kurumsal)](https://visualstudio.microsoft.com/downloads/) - Visual Studio'yu yüklerken **NuGet paket yöneticisi** bileşenini ve C++ iş **yüküyle Masaüstü Geliştirme'yi** eklediğinizden emin olun.
* [Git.](https://git-scm.com/download/)
* [CMake](https://cmake.org/download/).

### <a name="install-the-azure-iot-explorer"></a>Azure IoT gezginini yükleme

En son güncelleştirme için "Varlıklar" altında .msi dosyasını seçerek aracın [depo](https://github.com/Azure/azure-iot-explorer/releases) sayfasından **Azure IoT explorer'ın** en son sürümünden indirin ve yükleyin.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub'ınız için _IoT hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanmak için not):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, Azure IoT Hub Aygıtı C SDK'yı klonlamak ve oluşturmak için kullanabileceğiniz bir geliştirme ortamı hazırlayın.

Seçtiğiniz dizinde bir komut istemi açın. [Azure IoT C SDK'larını ve Kitaplıkları](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu bu konuma klonlamak için aşağıdaki komutu uygulayın:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
```

Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

## <a name="build-the-code"></a>Kodu oluşturma

Dahil edilen örnek kodu oluşturmak için SDK aygıtını kullanırsınız. Oluşturduğunuz uygulama, bir IoT hub'ına bağlanan bir aygıtı simüle eder. Uygulama telemetri ve özellikleri gönderir ve komutları alır.

1. Aygıt `cmake` SDK kök klasöründe bir alt dizini oluşturun ve bu klasöre gidin:

    ```cmd\sh
    cd <root folder>\azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Aygıt SDK ve oluşturulan kod saplama oluşturmak için aşağıdaki komutları çalıştırın:

    ```cmd\sh
    cmake ..
    cmake --build . -- /m /p:Configuration=Release
    ```

    > [!NOTE]
    > Cmake C++ derleyicinizi bulamazsa, önceki komutu çalıştırdığınızda yapı hataları alırsınız. Bu durumda, [Visual Studio komut istemibu komutu](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs)çalıştırmayı deneyin.

## <a name="run-the-device-sample"></a>Aygıt örneğini çalıştırma

IoT hub'ınıza telemetri gönderen bir IoT Tak ve Çalıştır aygıtını simüle etmek için SDK'da örnek bir uygulama çalıştırın. Örnek uygulamayı çalıştırmak için bu komutları kullanın ve _aygıt bağlantı dizesini_ parametre olarak geçirin.

```cmd\sh
cd digitaltwin_client\samples\digitaltwin_sample_device\Release
copy ..\EnvironmentalSensor.interface.json .
digitaltwin_sample_device.exe "<YourDeviceConnectionString>"
```

Aygıt artık komutları ve özellik güncelleştirmelerini almaya hazır ve hub'a telemetri verileri göndermeye başladı. Sonraki adımları tamamlarken numunenin çalışmasını engelleyin.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT gezginini kullanma

[!INCLUDE [iot-pnp-iot-explorer-1.md](../../includes/iot-pnp-iot-explorer-1.md)]

4. Aracın arabirim modeli tanımlarını cihazınızdan okuyabilmesini sağlamak için **Ayarlar'ı**seçin. Ayarlar menüsünde, **bağlı cihazda** Tak ve Çalıştır yapılandırmalarında zaten görünebilir; yoksa, **+ Ekle modül tanım kaynağını** seçin ve sonra eklemek için bağlı **aygıtta.**

1. **Cihazlara** genel bakış sayfasında, daha önce oluşturduğunuz aygıt kimliğini bulun. Aygıt uygulaması hala komut isteminde çalışırken, Aygıtın Azure IoT gezginindeki **Bağlantı durumunun** _Bağlı_ olarak bildirdiğini kontrol edin (değilse, yenilenene kadar **Yenile'ye** vurun). Daha fazla ayrıntı görüntülemek için aygıtı seçin.

1. IKimlik **vazosu:YOUR_COMPANY_NAME_HERE:EnvironmentalSensor:1** ile arabirimi genişleterek arayüzü ve IoT Plug and Play primitives-properties, commands ve telemetriyi ortaya çıkarın.

[!INCLUDE [iot-pnp-iot-explorer-2.md](../../includes/iot-pnp-iot-explorer-2.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir IoT Tak ve Çalıştır aygıtını bir IoT hub'ına nasıl bağlayabileceğinizi öğrendiniz. IoT Tak ve Çalıştır aygıtlarınızla etkileşimedebilen bir çözümü nasıl oluşturabilirsiniz hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Nasıl Yapılsın: Bir IoT Tak ve Çalıştır Önizleme aygıtına bağlanma ve etkileşimde](howto-develop-solution.md)
