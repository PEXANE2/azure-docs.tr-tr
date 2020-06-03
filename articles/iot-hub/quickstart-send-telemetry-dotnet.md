---
title: Azure IoT Hub’a telemetri gönderme hızlı başlangıcı (C#) | Microsoft Docs
description: Bu hızlı başlangıçta bir IoT hub’a sanal telemetri göndermek ve bulutta işlemek üzere IoT hub’dan gelen telemetriyi okumak için iki örnek C# uygulaması çalıştırırsınız.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom:
- mvc
- mqtt
ms.date: 06/01/2020
ms.openlocfilehash: 2efd2c982fcd4c799a6c9daa1d89fde25e7f2c64
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84307680"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-it-with-a-back-end-application-net"></a>Hızlı başlangıç: bir cihazdan IoT Hub 'ına telemetri gönderme ve arka uç uygulamasıyla okuma (.NET)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta depolama veya işleme amacıyla yüksek hacimlerde telemetri almanızı sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, bir simülasyon cihazı uygulamasından bir arka uç uygulamasına işlenmek üzere IoT Hub aracılığıyla telemetri gönderirsiniz.

Hızlı başlangıçta, biri telemetriyi göndermek için, diğeri de hub’dan telemetriyi okumak için olmak üzere önceden yazılmış iki C# uygulaması kullanılır. Bu iki uygulamayı çalıştırmadan önce bir IoT hub oluşturur ve hub’a bir cihaz kaydedersiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta çalıştırdığınız iki örnek uygulama, C# kullanılarak yazılır. Geliştirme makinenizde .NET Core SDK 3,0 veya üzeri bir sürümü gerekir.

[.NET](https://www.microsoft.com/net/download/all)’ten birden fazla platform için .NET Core SDK’sını indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli C# sürümünü doğrulayabilirsiniz:

```cmd/sh
dotnet --version
```

> [!NOTE]
> Bu hızlı başlangıçta Telemetriyi okumak için kullanılan Event Hubs hizmet kodunu derlemek için .NET Core SDK 3,0 veya üzeri önerilir. Hizmet kodu için dil sürümünü [hub 'ınızdaki Telemetriyi okuma](#read-the-telemetry-from-your-hub) bölümünde belirtildiği gibi Önizleme olarak ayarlarsanız, .NET Core SDK 2,1 ' i kullanabilirsiniz.

Azure CLı için Microsoft Azure IoT uzantısını Cloud Shell örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLı 'ye IoT Hub, IoT Edge ve IoT cihaz sağlama hizmeti 'ne (DPS) özel komutlar ekler.

```azurecli-interactive
az extension add --name azure-iot
```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

Azure IoT C# örneklerini indirin [https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) ve ZIP arşivini ayıklayın.

Güvenlik duvarınızdaki 8883 numaralı bağlantı noktasını açık olduğundan emin olun. Bu hızlı başlangıçta bulunan cihaz örneği, 8883 bağlantı noktası üzerinden iletişim kuran MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağ ortamlarında engellenebilir. Bu sorunu geçici olarak çözmek için daha fazla bilgi ve IoT Hub bkz. [bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliğini oluşturmak için Azure Cloud Shell aşağıdaki komutu çalıştırın.

   **Youriothubname**: aşağıdaki yer tutucuyu IoT Hub 'ınız için seçtiğiniz adla değiştirin.

   **MyDotnetDevice**: Bu, kaydetmekte olduğunuz cihazın adıdır. Gösterildiği gibi **MyDotnetDevice** kullanılması önerilir. Cihazınız için farklı bir ad seçerseniz, bu adı bu makalede da kullanmanız ve bunları çalıştırmadan önce örnek uygulamalarda cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyDotnetDevice
    ```

2. Yeni kaydettiğiniz cihazın _Cihaz bağlantı dizesini_ almak için Azure Cloud Shell ' de aşağıdaki komutu çalıştırın:

   **Youriothubname**: aşağıdaki yer tutucuyu IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyDotnetDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDotnetDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri daha sonra hızlı başlangıçta kullanacaksınız.

3. Ayrıca, arka uç uygulamasının IoT Hub 'ınıza bağlanmasını ve iletileri almanızı sağlamak için IoT Hub 'ınızdaki _Event Hubs uyumlu uç nokta_, _Event Hubs uyumlu yol_ve _hizmet birincil anahtarı_ gerekir. Aşağıdaki komutlar, IoT hub’ınız için şu değerleri alır:

   **Youriothubname**: Bu yer tutucuyu, IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub show --query properties.eventHubEndpoints.events.endpoint --name {YourIoTHubName}

    az iot hub show --query properties.eventHubEndpoints.events.path --name {YourIoTHubName}

    az iot hub policy show --name service --query primaryKey --hub-name {YourIoTHubName}
    ```

    Bu üç değeri bir yere, daha sonra hızlı başlangıçta kullanacaksınız.

## <a name="send-simulated-telemetry"></a>Sanal telemetri gönderme

Simülasyon cihazı uygulaması, IoT hub’ınız üzerindeki cihaza özgü bir uç noktaya bağlanır ve sanal sıcaklık ve nem telemetrisi gönderir.

1. Yerel terminal penceresinde, örnek C# projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\simulated-device** klasörüne gidin.

2. **SimulatedDevice.cs** dosyasını, istediğiniz bir metin düzenleyicide açın.

    Değişkenin değerini, `s_connectionString` daha önce bir değişiklik yaptığınız cihaz bağlantı dizesiyle değiştirin. Sonra **SimulatedDevice.cs**' ye yaptığınız değişiklikleri kaydedin.

3. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulaması için gerekli paketleri yükleyin:

    ```cmd/sh
    dotnet restore
    ```

4. Yerel terminal penceresinde, aşağıdaki komutu çalıştırarak simülasyon cihazı uygulamasını derleyip çalıştırın:

    ```cmd/sh
    dotnet run
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Simülasyon cihazını çalıştırma](media/quickstart-send-telemetry-dotnet/simulated-device.png)

## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

Arka uç uygulaması, IoT Hub’ınızdaki bir hizmet tarafı **Olaylar** uç noktasına bağlanır. Uygulama, simülasyon cihazınızdan gönderilen cihazdan buluta iletileri alır. IoT Hub arka uç uygulaması genellikle cihazdan buluta iletileri alıp işlemek için bulutta çalışır.

1. Başka bir yerel terminal penceresinde, örnek C# projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\read-d2c-messages** klasörüne gidin.

2. **ReadDeviceToCloudMessages.cs** dosyasını istediğiniz bir metin düzenleyicide açın. Aşağıdaki değişkenleri güncelleştirin ve yaptığınız değişiklikleri dosyaya kaydedin.

    | Değişken | Değer |
    | -------- | ----------- |
    | `EventHubsCompatibleEndpoint` | Değişkenin değerini, daha önce bir değişiklik yaptığınız Event Hubs uyumlu uç nokta ile değiştirin. |
    | `EventHubName`                | Değişkenin değerini, daha önce bir değişiklik yaptığınız Event Hubs uyumlu yol ile değiştirin. |
    | `IotHubSasKey`                | Değişkenin değerini, daha önce bir değişiklik yaptığınız hizmet birincil anahtarıyla değiştirin. |

    > [!NOTE]
    > .NET Core SDK 2,1 kullanıyorsanız, kodu derlemek için dil sürümünü önizleme olarak ayarlamanız gerekir. Bunu yapmak için **Read-D2C-messages. csproj** dosyasını açın ve `<LangVersion>` öğesinin değerini olarak ayarlayın `preview` .

3. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak arka uç uygulaması için gerekli kitaplıkları yükleyin:

    ```cmd/sh
    dotnet restore
    ```

4. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak arka uç uygulamasını derleyip çalıştırın:

    ```cmd/sh
    dotnet run
    ```

    Aşağıdaki ekran görüntüsünde, arka uç uygulaması, simülasyon cihazı tarafından hub’a gönderilen telemetriyi aldığında oluşan çıktı gösterilmektedir:

    ![Arka uç uygulamasını çalıştırma](media/quickstart-send-telemetry-dotnet/read-device-to-cloud.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT Hub 'ı ayarlarsınız, bir cihaz kaydettiniz, bir C# uygulaması kullanarak hub 'a sanal telemetri gönderdiniz ve basit bir arka uç uygulaması kullanarak hub 'ın Telemetriyi okuyaöğreneceksiniz.

Bir arka uç uygulamasından simülasyon cihazınızı denetlemeyi öğrenmek için sonraki hızlı başlangıçla devam edin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: IoT hub’a bağlı bir cihazı denetleme](quickstart-control-device-dotnet.md)