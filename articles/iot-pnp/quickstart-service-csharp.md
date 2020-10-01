---
title: Azure IoT çözümünüze bağlı bir IoT Tak ve Kullan cihazından etkileşim kurma (C#) | Microsoft Docs
description: C# kullanarak Azure IoT çözümünüze bağlı bir IoT Tak ve Kullan cihazına bağlanın ve bunlarla etkileşime geçin.
author: ericmitt
ms.author: ericmitt
ms.date: 09/21/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 1a8155c9c0466184a09bc4ff224c9bdf42c173b9
ms.sourcegitcommit: 06ba80dae4f4be9fdf86eb02b7bc71927d5671d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91612566"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-device-thats-connected-to-your-solution-c"></a>Hızlı başlangıç: çözümünüze bağlı olan IoT Tak ve Kullan cihazla etkileşim kurma (C#)

[!INCLUDE [iot-pnp-quickstarts-service-selector.md](../../includes/iot-pnp-quickstarts-service-selector.md)]

IoT Tak ve Kullan, arka plandaki cihaz uygulamasıyla ilgili bilgi sahibi olmadan bir cihazın özellikleri ile etkileşim kurmanızı sağlayarak IoT 'yi basitleştirir. Bu hızlı başlangıçta, çözümünüze bağlı bir IoT Tak ve Kullan cihazına bağlanmak ve bunları denetlemek Için C# ' nin nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Bu hızlı başlangıcı Windows üzerinde tamamlayabilmeniz için geliştirme makinenizde aşağıdaki yazılımların yüklü olması gerekir:

* [Visual Studio (Community, Professional veya Enterprise)](https://visualstudio.microsoft.com/downloads/).
* [Git](https://git-scm.com/download/).

### <a name="clone-the-sdk-repository-with-the-sample-code"></a>Örnek kodla SDK deposunu kopyalayın

[Hızlı başlangıç: Windows üzerinde çalışan bir örnek ıot Tak ve kullan cihaz uygulamasını IoT Hub 'A bağlama (C#)](quickstart-connect-device-csharp.md), depoyu zaten Klonladığınız.

.NET GitHub deposu için Microsoft Azure IoT SDK ' dan örnekleri kopyalayın. Seçtiğiniz bir klasörde bir komut istemi açın. .NET GitHub deposu [için Microsoft Azure IoT örneklerini](https://github.com/Azure-Samples/azure-iot-samples-csharp) kopyalamak üzere aşağıdaki komutu çalıştırın:

```cmd
git clone https://github.com/Azure-Samples/azure-iot-samples-csharp.git
```

## <a name="run-the-sample-device"></a>Örnek cihazı çalıştırma

Bu hızlı başlangıçta, IoT Tak ve Kullan cihazı olarak C# dilinde yazılmış örnek bir termostat cihazı kullanırsınız. Örnek cihazı çalıştırmak için:

1. *Azure-iot-Samples-csharp\iot-hub\Samples\device\PnpDeviceSamples\Thermostat\Thermostat.csproj* proje dosyasını Visual Studio 2019 ' de açın.

1. Visual Studio 'da, **hata ayıklama > Project > termostat özellikleri**' ne gidin. Ardından projeye aşağıdaki ortam değişkenlerini ekleyin:

    | Name | Değer |
    | ---- | ----- |
    | IOTHUB_DEVICE_SECURITY_TYPE | DPS |
    | IOTHUB_DEVICE_DPS_ENDPOINT | global.azure-devices-provisioning.net |
    | IOTHUB_DEVICE_DPS_ID_SCOPE | [Ortamınızı ayarlamayı](set-up-environment.md) tamamladıktan sonra bir değişiklik yapmış olduğunuz değer |
    | IOTHUB_DEVICE_DPS_DEVICE_ID | My-PNP-cihazım |
    | IOTHUB_DEVICE_DPS_DEVICE_KEY | [Ortamınızı ayarlamayı](set-up-environment.md) tamamladıktan sonra bir değişiklik yapmış olduğunuz değer |


1. Artık Visual Studio 'da örneği oluşturabilir ve hata ayıklama modunda çalıştırabilirsiniz.

1. Cihazın bazı bilgileri gönderdiğini ve onun çevrimiçi olduğunu söyleyen iletiler görürsünüz. Bu iletiler, cihazın hub 'a telemetri verileri göndermeyi başladığını ve artık komut ve özellik güncelleştirmelerini almaya hazır olduğunu gösterir. Bu Visual Studio örneğini kapatmayın, hizmet örneğinin çalıştığını onaylamanız gerekir.

## <a name="run-the-sample-solution"></a>Örnek çözümü çalıştırma

Ortamınızı IoT Hub 'ınıza ve cihazınıza bağlanacak şekilde yapılandırmak için, [ıot Tak ve kullan hızlı başlangıçlarını ve öğreticilerini ayarlama](set-up-environment.md) bölümünde iki ortam değişkeni oluşturdunuz:

* **IOTHUB_CONNECTION_STRING**: IoT Hub bağlantı dizesi daha önce bir nota yaptınız.
* **DEVICE_ID**: `"my-pnp-device"` .

Bu hızlı başlangıçta, yeni ayarladığınız örnek cihazla etkileşim kurmak Için C# ' de örnek bir IoT çözümü kullanırsınız.

1. Visual Studio 'nun başka bir örneğinde, *Azure-iot-Samples-csharp\iot-hub\Samples\service\PnpServiceSamples\Thermostat\Thermostat.csproj* projesini açın.

1. Visual Studio 'da, **hata ayıklama > Project > termostat özellikleri**' ne gidin. Ardından projeye aşağıdaki ortam değişkenlerini ekleyin:

    | Name | Değer |
    | ---- | ----- |
    | DEVICE_ID | My-PNP-cihazım |
    | IOTHUB_CONNECTION_STRING | [Ortamınızı ayarlamayı](set-up-environment.md) tamamladıktan sonra bir değişiklik yapmış olduğunuz değer |

1. Artık Visual Studio 'da örneği oluşturabilir ve hata ayıklama modunda çalıştırabilirsiniz.

### <a name="get-digital-twin"></a>Dijital ikizi al

Aşağıdaki kod parçacığı, hizmet uygulamasının dijital ikizi nasıl alacağını göstermektedir:

```C#
// Get a Twin and retrieves model Id set by Device client
Twin twin = await s_registryManager.GetTwinAsync(s_deviceId);
s_logger.LogDebug($"Model Id of this Twin is: {twin.ModelId}");
```

> [!NOTE]
> Bu örnek, **IoT Hub hizmeti Istemcisinden** **Microsoft. Azure. Devices. Client;** ad alanını kullanır. Model KIMLIĞINI alma hakkında daha fazla bilgi edinmek için bkz. [Geliştirici Kılavuzu](concepts-developer-guide-service.md).

Bu kod aşağıdaki çıktıyı oluşturur:

```cmd
[09/21/2020 11:26:04]dbug: Thermostat.Program[0]
      Model Id of this Twin is: dtmi:com:example:Thermostat;1
```

Aşağıdaki kod parçacığı, dijital ikizi özelliklerini güncelleştirmek için bir *düzeltme ekinin* nasıl kullanılacağını gösterir:

```C#
// Update the twin
var twinPatch = new Twin();
twinPatch.Properties.Desired[PropertyName] = PropertyValue;
await s_registryManager.UpdateTwinAsync(s_deviceId, twinPatch, twin.ETag);
```

Bu kod, hizmet, özelliği güncelleştirdiğinde cihazdan aşağıdaki çıktıyı oluşturur `targetTemperature` :

```cmd
[09/21/2020 11:26:05]dbug: Thermostat.ThermostatSample[0]
      Property: Received - { "targetTemperature": 60°C }.
[09/21/2020 11:26:05]dbug: Thermostat.ThermostatSample[0]
      Property: Update - {"targetTemperature": 60°C } is InProgress.

...

[09/21/2020 11:26:17]dbug: Thermostat.ThermostatSample[0]
      Property: Update - {"targetTemperature": 60°C } is Completed.
```

### <a name="invoke-a-command"></a>Komut çağırma

Aşağıdaki kod parçacığı bir komutun nasıl çağrılacağını göstermektedir:

```csharp
private static async Task InvokeCommandAsync()
{
    var commandInvocation = new CloudToDeviceMethod(CommandName) { ResponseTimeout = TimeSpan.FromSeconds(30) };

    // Set command payload
    string componentCommandPayload = JsonConvert.SerializeObject(s_dateTime);
    commandInvocation.SetPayloadJson(componentCommandPayload);

    CloudToDeviceMethodResult result = await s_serviceClient.InvokeDeviceMethodAsync(s_deviceId, commandInvocation);

    if (result == null)
    {
        throw new Exception($"Command {CommandName} invocation returned null");
    }

    s_logger.LogDebug($"Command {CommandName} invocation result status is: {result.Status}");
}
```

Bu kod, hizmet komutu çağırdığında cihazdan aşağıdaki çıktıyı oluşturur `getMaxMinReport` :

```cmd
[09/21/2020 11:26:05]dbug: Thermostat.ThermostatSample[0]
      Command: Received - Generating max, min and avg temperature report since 21/09/2020 11:25:58.
[09/21/2020 11:26:05]dbug: Thermostat.ThermostatSample[0]
      Command: MaxMinReport since 21/09/2020 11:25:58: maxTemp=32, minTemp=32, avgTemp=32, startTime=21/09/2020 11:25:59, endTime=21/09/2020 11:26:04
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Tak ve Kullan cihazını IoT çözümüne bağlamayı öğrendiniz. IoT Tak ve Kullan cihaz modelleri hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [IoT Tak ve Kullan modelleme Geliştirici Kılavuzu](concepts-developer-guide-device-csharp.md)
