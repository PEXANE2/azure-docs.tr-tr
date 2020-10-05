---
title: IoT Tak ve Kullan örnek C# Cihaz kodunu IoT Hub bağlama | Microsoft Docs
description: IoT Hub 'ına bağlanan Windows üzerinde IoT Tak ve Kullan örnek cihaz kodu oluşturun ve çalıştırın. Cihaz tarafından hub 'a gönderilen bilgileri görüntülemek için Azure IoT gezgin aracını kullanın.
author: ericmitt
ms.author: ericmitt
ms.date: 07/14/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: d1deac1c7932a8f3cec06d9c264ba401f7f1341d
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91577042"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-device-application-running-on-windows-to-iot-hub-c"></a>Hızlı başlangıç: Windows 'ta IoT Hub için çalışan bir örnek IoT Tak ve Kullan cihaz uygulaması bağlama (C#)

[!INCLUDE [iot-pnp-quickstarts-device-selector.md](../../includes/iot-pnp-quickstarts-device-selector.md)]

Bu hızlı başlangıçta örnek bir IoT Tak ve Kullan cihaz uygulaması oluşturma, IoT Hub 'ınıza bağlama ve Azure IoT Explorer aracını kullanarak gönderdiği Telemetriyi görüntüleme gösterilmektedir. Örnek uygulama, CSharp dilinde yazılmıştır ve C# için Azure IoT cihaz SDK 'sına dahildir. Bir çözüm Oluşturucusu, herhangi bir cihaz kodunu görüntülemeye gerek olmadan IoT Tak ve Kullan cihazının yeteneklerini anlamak için Azure IoT gezgin aracını kullanabilir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Bu hızlı başlangıcı Windows üzerinde tamamlayabilmeniz için geliştirme makinenizde aşağıdaki yazılımların yüklü olması gerekir:

* [Visual Studio (Community, Professional veya Enterprise)](https://visualstudio.microsoft.com/downloads/).
* [Git](https://git-scm.com/download/).

## <a name="download-the-code"></a>Kodu indirme

Bu hızlı başlangıçta, Azure IoT Hub Device C# SDK 'SıNı klonlamak ve derlemek için kullanabileceğiniz bir geliştirme ortamı hazırlarsınız.

Seçtiğiniz bir klasörde bir komut istemi açın. .NET GitHub deposunun [Microsoft Azure IoT örneklerini](https://github.com/Azure-Samples/azure-iot-samples-csharp) bu konuma kopyalamak için aşağıdaki komutu çalıştırın:

```cmd
git clone  https://github.com/Azure-Samples/azure-iot-samples-csharp.git
```

## <a name="build-the-code"></a>Kodu oluşturma

Artık Visual Studio 'da örneği oluşturabilir ve hata ayıklama modunda çalıştırabilirsiniz.

1. *Azure-iot-Samples-csharp\iot-hub\Samples\device\PnpDeviceSamples\Thermostat\Thermostat.csproj* proje dosyasını Visual Studio 2019 ' de açın.

1. Visual Studio 'da, **hata ayıklama > Project > termostat özellikleri**' ne gidin. Ardından projeye aşağıdaki ortam değişkenlerini ekleyin:

    | Name | Değer |
    | ---- | ----- |
    | IOTHUB_DEVICE_SECURITY_TYPE | DPS |
    | IOTHUB_DEVICE_DPS_ENDPOINT | global.azure-devices-provisioning.net |
    | IOTHUB_DEVICE_DPS_ID_SCOPE | [Ortamınızı ayarlamayı](set-up-environment.md) tamamladıktan sonra bir değişiklik yapmış olduğunuz değer |
    | IOTHUB_DEVICE_DPS_DEVICE_ID | My-PNP-cihazım |
    | IOTHUB_DEVICE_DPS_DEVICE_KEY | [Ortamınızı ayarlamayı](set-up-environment.md) tamamladıktan sonra bir değişiklik yapmış olduğunuz değer |

Artık Visual Studio 'da örneği oluşturabilir ve hata ayıklama modunda çalıştırabilirsiniz.

## <a name="run-the-device-sample"></a>Cihaz örneğini çalıştırma

Windows üzerinde Visual Studio 'da kod yürütmeyi izlemek için, program.cs dosyasındaki işleve bir kesme noktası ekleyin `main` .

Cihaz artık komutları ve özellik güncelleştirmelerini almaya hazır ve hub 'a telemetri verileri göndermeye başladı. Sonraki adımları tamamladıktan sonra örneği çalışır durumda tutun.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT Gezginini Kullanma

Cihaz istemcisi örneği başladıktan sonra, çalıştığını doğrulamak için Azure IoT gezgin aracını kullanın.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu örnek, basit bir IoT Tak ve Kullan termostat cihazı uygular. Bu örneğin uyguladığı model IoT Tak ve Kullan [bileşenleri](concepts-components.md)kullanmaz. [Termostat cihazının dijital TWINS tanım dili (DTDL) modeli dosyası](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json) , cihazın uyguladığı telemetri, özellik ve komutları tanımlar.

Cihaz kodu, standart yöntemi kullanarak IoT Hub 'ınıza bağlanır `CreateFromConnectionString` . Cihaz, bağlantı isteğinde uyguladığı DTDL modelinin model KIMLIĞINI gönderir. Model KIMLIĞI gönderen bir cihaz IoT Tak ve Kullan cihazdır:

```csharp
private static void InitializeDeviceClientAsync()
{
  var options = new ClientOptions
  {
    ModelId = ModelId,
  };
  s_deviceClient = DeviceClient.CreateFromConnectionString(s_deviceConnectionString, TransportType.Mqtt, options);
  s_deviceClient.SetConnectionStatusChangesHandler((status, reason) =>
  {
     s_logger.LogDebug($"Connection status change registered - status={status}, reason={reason}.");
  });
}
```

Model KIMLIĞI, aşağıdaki kod parçacığında gösterildiği gibi kodda depolanır:

```csharp
private const string ModelId = "dtmi:com:example:Thermostat;1";
```

Özellikleri güncelleştiren, komutları işleyen ve telemetri gönderen kod, IoT Tak ve Kullan kurallarını kullanmayan bir cihazın koduyla aynıdır.

Örnek, IoT Hub 'ınızdan gönderilen yüklerden JSON nesnelerini ayrıştırmak için bir JSON kitaplığı kullanır:

```csharp
using Newtonsoft.Json;

...

DateTime since = JsonConvert.DeserializeObject<DateTime>(request.DataAsJson);
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Hub 'ına IoT Tak ve Kullan cihazını bağlamayı öğrendiniz. IoT Tak ve Kullan cihazlarınızla etkileşim kuran bir çözüm oluşturma hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Nasıl yapılır: bir cihaza bağlanma ve cihazla etkileşim kurma](howto-develop-solution.md)
