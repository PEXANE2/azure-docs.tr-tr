---
title: IoT Tak ve Çalıştır Preview örnek cihaz kodunu IoT Hub'ına bağlayın | Microsoft Dokümanlar
description: C# (.NET) kullanarak, bir IoT hub'ına bağlanan IoT Tak ve Çalıştır Preview örnek aygıt kodu oluşturun ve çalıştırın. Aygıt tarafından hub'a gönderilen bilgileri görüntülemek için Azure IoT gezgini aracını kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 12/27/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 90d39635ac6302f816f39ca19cc00a39cfbbf850
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77121022"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-to-iot-hub-c"></a>Quickstart: Örnek bir IoT Tak ve Çalıştır Önizleme aygıtı uygulamasını IoT Hub'ına (C#) bağlayın

[!INCLUDE [iot-pnp-quickstarts-2-selector.md](../../includes/iot-pnp-quickstarts-2-selector.md)]

Bu hızlı başlangıç, örnek bir IoT Tak ve Çalıştır aygıtı uygulamasını nasıl oluşturabileceğinizi, IoT hub'ınıza nasıl bağlayabileceğinizi ve hub'a gönderdiği bilgileri görüntülemek için Azure IoT gezgini aracını nasıl kullanacağınızı gösterir. Örnek uygulama C# (.NET ile) olarak yazılır ve C# (.NET) koleksiyonu için Azure IoT Örnekleri'nin bir parçası olarak sağlanır. Bir çözüm geliştiricisi, herhangi bir aygıt kodunu görüntülemeye gerek kalmadan bir IoT Tak ve Çalıştır aygıtının özelliklerini anlamak için Azure IoT explorer aracını kullanabilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için geliştirme makinenize .NET Core 3.0 yüklemeniz gerekir. .NET Core SDK'nın bu sürümünü download [.NET Core 3.0'dan](https://dotnet.microsoft.com/download/dotnet-core/3.0)birden fazla platform için indirebilirsiniz.

Geliştirme makinenizdeki .NET sürümünü yerel bir terminal penceresinde aşağıdaki komutu çalıştırarak doğrulayabilirsiniz: 

```cmd/sh
dotnet --version
```

### <a name="install-the-azure-iot-explorer"></a>Azure IoT gezginini yükleme

En son güncelleştirme için "Varlıklar" altında .msi dosyasını seçerek aracın [depo](https://github.com/Azure/azure-iot-explorer/releases) sayfasından **Azure IoT explorer'ın** en son sürümünden indirin ve yükleyin.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub'ınız için _IoT hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanmak için not):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, C# (.NET) için Azure IoT Örneklerini klonlamak ve oluşturmak için kullanabileceğiniz bir geliştirme ortamı hazırlayın.

Seçtiğiniz dizinde bir komut istemi açın. [C# (.NET)](https://github.com/Azure-Samples/azure-iot-samples-csharp) GitHub deposunu bu konuma klonlamak için aşağıdaki komutu uygulayın:

```cmd/sh
git clone https://github.com/Azure-Samples/azure-iot-samples-csharp
```

Bu işlemin tamamlanması birkaç dakika sürebilir.

## <a name="run-the-device-sample"></a>Aygıt örneğini çalıştırma

Bir IoT hub'ına bağlanan bir aygıtı taklit eden bir uygulama oluşturmak için klonlanmış örnek kodunu kullanırsınız. Uygulama telemetri ve özellikleri gönderir ve komutları alır.

1. Yerel bir terminal penceresinde, klonlanmış deponuzun klasörüne gidin ve **azure-iot-samples-csharp/digitaltwin/Samples/device/EnvironmentalSensorSample** klasörüne gidin. 

1. _Aygıt bağlantı dizesini_yapılandır:

    ```cmd/sh
    set DIGITAL_TWIN_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. IoT hub'ınıza telemetri gönderen bir IoT Tak ve Çalıştır aygıtını simüle etmek için örnek bir uygulama çalıştırın. Aynı terminal penceresinde, gerekli paketleri oluşturmak ve örnek uygulamayı çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd\sh
    dotnet run --framework=netcoreapp3.0
    ```

Aygıtın başarıyla kaydolduğunu ve buluttan güncelleştirmeler beklediğini belirten iletiler görürsünüz. Bu, aygıtın artık komutları ve özellik güncelleştirmelerini almaya hazır olduğunu ve hub'a telemetri verilerini göndermeye başladığını gösterir. Sonraki adımları tamamlarken numunenin çalışmasını engelleyin.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT gezginini kullanma

[!INCLUDE [iot-pnp-iot-explorer-1.md](../../includes/iot-pnp-iot-explorer-1.md)]

4. Aracın arabirim modeli tanımlarını cihazınızdan okuyabilmesini sağlamak için **Ayarlar'ı**seçin. Ayarlar menüsünde, **bağlı cihazda** Tak ve Çalıştır yapılandırmalarında zaten görünebilir; yoksa, **+ Ekle modül tanım kaynağını** seçin ve sonra eklemek için bağlı **aygıtta.**

1. **Cihazlara** genel bakış sayfasında, daha önce oluşturduğunuz aygıt kimliğini bulun. Aygıt uygulaması hala komut isteminde çalışırken, Aygıtın Azure IoT gezginindeki **Bağlantı durumunun** _Bağlı_ olarak bildirdiğini kontrol edin (değilse, yenilenene kadar **Yenile'ye** vurun). Daha fazla ayrıntı görüntülemek için aygıtı seçin.

1. IKimlik **vazosu:csharp_sdk_sample:EnvironmentalSensor:1** ile arabirimi genişleterek arayüzü ve IoT Plug and Play primitives-properties, commands ve telemetriyi ortaya çıkarın.

[!INCLUDE [iot-pnp-iot-explorer-2.md](../../includes/iot-pnp-iot-explorer-2.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir IoT Tak ve Çalıştır aygıtını bir IoT hub'ına nasıl bağlayabileceğinizi öğrendiniz. IoT Tak ve Çalıştır aygıtlarınızla etkileşimedebilen bir çözümü nasıl oluşturabilirsiniz hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Nasıl Yapılsın: Bir IoT Tak ve Çalıştır Önizleme aygıtına bağlanma ve etkileşimde](howto-develop-solution.md)
