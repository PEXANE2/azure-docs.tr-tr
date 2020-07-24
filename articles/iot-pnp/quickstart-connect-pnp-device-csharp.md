---
title: IoT Tak ve Kullan önizleme kodunu IoT Hub-C# ' a bağlama | Microsoft Docs
description: C# (.net) kullanarak IoT Hub 'ına bağlanan IoT Tak ve Kullan önizlemesi örnek cihaz kodu oluşturun ve çalıştırın. Cihaz tarafından hub 'a gönderilen bilgileri görüntülemek için Azure IoT gezgin aracını kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 12/27/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: d598bbb792c071db281ebdc562402ba0198305ab
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87044186"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-to-iot-hub-c"></a>Hızlı başlangıç: örnek bir IoT Tak ve Kullan Preview cihaz uygulamasını IoT Hub 'ye bağlama (C#)

[!INCLUDE [iot-pnp-quickstarts-2-selector.md](../../includes/iot-pnp-quickstarts-2-selector.md)]

Bu hızlı başlangıçta, örnek bir IoT Tak ve Kullan cihaz uygulamasının nasıl oluşturulacağı, IoT Hub 'ınıza nasıl bağlanacağı ve hub 'a gönderdiği bilgileri görüntülemek için Azure IoT gezgin aracının nasıl kullanılacağı gösterilmektedir. Örnek uygulama C# dilinde yazılmıştır (.NET ile) ve c# için Azure IoT örnekleri (.net) koleksiyonunun bir parçası olarak sağlanır. Bir çözüm geliştiricisi, herhangi bir cihaz kodunu görüntülemeye gerek olmadan IoT Tak ve Kullan cihazının yeteneklerini anlamak için Azure IoT gezgin aracını kullanabilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için geliştirme makinenize .NET Core 3,0 ' i yüklemeniz gerekir. Birden çok platform için .NET Core SDK bu sürümünü [.NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0)' i indirin.

Bir yerel Terminal penceresinde aşağıdaki komutu çalıştırarak geliştirme makinenize ait .NET sürümünü doğrulayabilirsiniz: 

```cmd/sh
dotnet --version
```

### <a name="install-the-azure-iot-explorer"></a>Azure IoT Gezginini yüklemeyi

En son güncelleştirme için "varlıklar" altında. msi dosyasını seçerek aracın [Depo](https://github.com/Azure/azure-iot-explorer/releases) sayfasından **Azure IoT Explorer** 'ın en son sürümünü indirin ve yükleyin.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub 'ınız için _IoT Hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanmak üzere):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, C# için Azure IoT örnekleri (.net) kopyalamak ve derlemek için kullanabileceğiniz bir geliştirme ortamı hazırlarsınız.

Seçtiğiniz dizinde bir komut istemi açın. [C# (.net) GitHub deposunu Azure IoT örnekleri](https://github.com/Azure-Samples/azure-iot-samples-csharp) ' ni bu konuma kopyalamak için aşağıdaki komutu yürütün:

```cmd/sh
git clone https://github.com/Azure-Samples/azure-iot-samples-csharp
```

Bu işlemin tamamlanması birkaç dakika sürebilir.

## <a name="run-the-device-sample"></a>Cihaz örneğini çalıştırma

Bir IoT Hub 'ına bağlanan bir cihaza benzetim yapan bir uygulama oluşturmak için klonlanan örnek kodu kullanırsınız. Uygulama telemetri ve Özellikler gönderir ve komutları alır.

1. Yerel bir Terminal penceresinde, klonlanmış deponuzdaki klasöre gidin ve **Azure-IoT-Samples-CSharp/digitaltwin/Samples/Device/EnvironmentalSensorSample** klasörüne gidin. 

1. _Cihaz bağlantı dizesini_yapılandırın:

    ```cmd/sh
    set DIGITAL_TWIN_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. IoT Hub 'ınıza telemetri gönderen bir IoT Tak ve Kullan cihazının benzetimini yapmak için örnek bir uygulama çalıştırın. Aynı Terminal penceresinde, gerekli paketleri oluşturmak ve örnek uygulamayı çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd\sh
    dotnet run --framework=netcoreapp3.0
    ```

Cihazın başarıyla kaydedildiğini ve buluttan güncelleştirme beklediğini belirten iletiler görürsünüz. Bu, cihazın artık komutları ve özellik güncelleştirmelerini almaya hazır olduğunu ve hub 'a telemetri verileri göndermeye başlamış olduğunu gösterir. Sonraki adımları tamamladıktan sonra örneği çalışır durumda tutun.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT gezginini kullanın

[!INCLUDE [iot-pnp-iot-explorer-1.md](../../includes/iot-pnp-iot-explorer-1.md)]

4. Aracın, cihazınızdan arabirim modeli tanımlarını okuyacağına emin olmak için **Ayarlar**' ı seçin. Ayarlar menüsünde, **bağlı cihazdaki** Tak ve kullan yapılandırmalarda zaten görünebilir. değilse, **+ modül tanımı kaynağı Ekle** ' yi seçin ve ardından **bağlı cihazda** ekleyin.

1. **Cihazların** genel bakış sayfasına geri döndüğünüzde, daha önce oluşturduğunuz cihaz kimliğini bulun. Cihaz uygulaması komut isteminde çalışmaya devam ettiğinden, cihazın Azure IoT Explorer 'daki **bağlantı durumunun** _bağlı_ olarak raporlandığından emin olun (Aksi takdirde, olana kadar **Yenile** ' ye basın). Daha fazla ayrıntı görüntülemek için cihazı seçin.

1. Arabirim ve IoT Tak ve Kullan temelleri (özellikler, komutlar ve telemetri) göstermek için ID **urn: csharp_sdk_sample: Environmentalalgılayıcı: 1** olan arabirimi genişletin.

[!INCLUDE [iot-pnp-iot-explorer-2.md](../../includes/iot-pnp-iot-explorer-2.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Hub 'ına IoT Tak ve Kullan cihazını bağlamayı öğrendiniz. IoT Tak ve Kullan cihazlarınızla etkileşim kuran bir çözüm oluşturma hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Nasıl yapılır: IoT Tak ve Kullan önizleme cihazına bağlanma ve bunlarla etkileşim kurma](howto-develop-solution.md)
