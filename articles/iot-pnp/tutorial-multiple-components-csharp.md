---
title: IoT Tak ve Kullan Preview örnek C# Bileşen cihazı kodunu IoT Hub 'ye bağlama | Microsoft Docs
description: Birden çok bileşen kullanan ve IoT Hub 'ına bağlanan IoT Tak ve Kullan Preview örnek C# Cihaz kodunu derleyin ve çalıştırın. Cihaz tarafından hub 'a gönderilen bilgileri görüntülemek için Azure IoT gezgin aracını kullanın.
author: ericmitt
ms.author: ericmitt
ms.date: 07/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 67b71399332fb29a277381a8c2806dbe7fb31d85
ms.sourcegitcommit: 1b2d1755b2bf85f97b27e8fbec2ffc2fcd345120
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87552178"
---
# <a name="tutorial-connect-an-iot-plug-and-play-multiple-component-device-application-running-on-windows-to-iot-hub-c"></a>Öğretici: Windows üzerinde çalışan bir IoT Tak ve Kullan birden çok bileşen cihaz uygulamasını IoT Hub 'ye bağlama (C#)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

Bu öğreticide, bileşenler ve kök arabirimiyle örnek IoT Tak ve Kullan cihaz uygulaması oluşturma, IoT Hub 'ınıza bağlama ve Azure IoT gezgin aracını kullanarak hub 'a gönderdiği bilgileri görüntüleme hakkında bilgi verilmektedir. Örnek uygulama C# dilinde yazılır ve c# için Azure IoT cihaz SDK 'sına dahildir. Bir çözüm Oluşturucusu, herhangi bir cihaz kodunu görüntülemeye gerek olmadan IoT Tak ve Kullan cihazının yeteneklerini anlamak için Azure IoT gezgin aracını kullanabilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi Windows üzerinde gerçekleştirmek için, yerel Windows ortamınıza aşağıdaki yazılımları yüklersiniz:

* [Visual Studio (Community, Professional veya Enterprise)](https://visualstudio.microsoft.com/downloads/).
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).

### <a name="azure-iot-explorer"></a>Azure IoT Gezgini

Bu öğreticinin ikinci bölümünde örnek cihazla etkileşime geçmek için **Azure IoT gezgin** aracını kullanın. İşletim sisteminiz için [Azure IoT Explorer 'ın en son sürümünü indirin ve yükleyin](./howto-use-iot-explorer.md) .

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub 'ınız için _IoT Hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın. Bu bağlantı dizesini, daha sonra bu öğreticide kullanacaksınız.

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

> [!TIP]
> IoT Hub bağlantı dizesini bulmak için Azure IoT gezgin aracını da kullanabilirsiniz.

Hub 'a eklediğiniz cihazın _Cihaz bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın. Bu bağlantı dizesini, daha sonra bu öğreticide kullanacaksınız.

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

[!INCLUDE [iot-pnp-download-models.md](../../includes/iot-pnp-download-models.md)]

## <a name="download-the-code"></a>Kodu indirme

Bu öğreticide, Azure IoT Hub Device C# SDK 'SıNı klonlamak ve derlemek için kullanabileceğiniz bir geliştirme ortamı hazırlarsınız.

Seçtiğiniz dizinde bir komut istemi açın. [Azure IoT C# SDK 'ları ve kitaplıkları](https://github.com/Azure/azure-iot-sdk-csharp) GitHub deposunu bu konuma kopyalamak için aşağıdaki komutu yürütün:

```cmd
git clone https://github.com/Azure/azure-iot-sdk-csharp.git
```

## <a name="build-the-code"></a>Kodu oluşturma

**Azureiot. sln** çözüm dosyasını Visual Studio 2019 ' de açın ve **TemperatureController** projesini başlangıç projesi olarak ayarlayın. **Çözüm Gezgini**, bu proje dosyasını **ıothub > cihaz > örnekleri**içinde bulabilirsiniz.

Artık Visual Studio 'da örneği oluşturabilir ve hata ayıklama modunda çalıştırabilirsiniz.

## <a name="run-the-device-sample"></a>Cihaz örneğini çalıştırma

Daha önce bir nota yaptığınız cihaz bağlantı dizesini depolamak için **IOTHUB_DEVICE_CONNECTION_STRING** adlı bir ortam değişkeni oluşturun.

Windows üzerinde Visual Studio 'da kod yürütmeyi izlemek için, program.cs dosyasındaki işleve bir kesme noktası ekleyin `main` .

Cihaz artık komutları ve özellik güncelleştirmelerini almaya hazır ve hub 'a telemetri verileri göndermeye başladı. Sonraki adımları tamamladıktan sonra örneği çalışır durumda tutun.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT Gezginini Kullanma

Cihaz istemcisi örneği başladıktan sonra, çalıştığını doğrulamak için Azure IoT gezgin aracını kullanın.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu örnek, IoT Tak ve Kullan ısı denetleyicisi cihazı uygular. Bu örneğin uyguladığı model [birden çok bileşen](concepts-components.md)kullanır. [Sıcaklık cihazının dijital TWINS tanım dili (DTDL) modeli dosyası](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) , cihazın uyguladığı telemetri, Özellikler ve komutları tanımlar.

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

Cihaz IoT Hub 'ınıza bağlandıktan sonra kod, komut işleyicilerini kaydeder. `reboot`Komut kök arabirimde tanımlanmıştır. Bu `getMaxMinReport` komut, iki ıterstat bileşeninin her birinde tanımlanmıştır:

```csharp
await s_deviceClient.SetMethodHandlerAsync("reboot", HandleRebootCommandAsync, s_deviceClient);
await s_deviceClient.SetMethodHandlerAsync("thermostat1*getMaxMinReport", HandleMaxMinReportCommandAsync, Thermostat1);
await s_deviceClient.SetMethodHandlerAsync("thermostat2*getMaxMinReport", HandleMaxMinReportCommandAsync, Thermostat2);
```

İki termostat bileşenlerinde istenen özellik güncelleştirmelerine yönelik ayrı işleyiciler vardır:

```csharp
s_desiredPropertyUpdateCallbacks.Add(Thermostat1, TargetTemperatureUpdateCallbackAsync);
s_desiredPropertyUpdateCallbacks.Add(Thermostat2, TargetTemperatureUpdateCallbackAsync);
```

Örnek kod, her bir termostat bileşeninden telemetri gönderir:

```csharp
await SendTemperatureAsync(Thermostat1);
await SendTemperatureAsync(Thermostat2);
```

`SendTemperature`Yöntemi, `PnpHhelper` her bileşen için ileti oluşturmak üzere sınıfını kullanır:

```csharp
Message msg = PnpHelper.CreateIothubMessageUtf8(telemetryName, JsonConvert.SerializeObject(currentTemperature), componentName);
```

`PnpHelper`Sınıfı, birden çok bileşen modeliyle kullanabileceğiniz diğer örnek yöntemleri içerir.

İki termostat bileşeninden telemetri ve özellikleri görüntülemek için Azure IoT gezgin aracını kullanın:

:::image type="content" source="media/tutorial-multiple-components-csharp/multiple-component.png" alt-text="Azure IoT Gezgini 'nde birden çok bileşen cihazı":::

Ayrıca, iki termostat bileşeninden ya da kök arabirimde komutları çağırmak için Azure IoT gezgin aracını da kullanabilirsiniz.

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IoT Hub 'ına bileşenlerle IoT Tak ve Kullan cihazını bağlamayı öğrendiniz. IoT Tak ve Kullan cihaz modelleri hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [IoT Tak ve Kullan Preview modelleme Geliştirici Kılavuzu](concepts-developer-guide.md)
