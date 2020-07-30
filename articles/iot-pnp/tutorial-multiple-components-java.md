---
title: IoT Tak ve Kullan Preview örnek Java bileşen cihazı kodunu IoT Hub 'ye bağlayın | Microsoft Docs
description: Birden çok bileşen kullanan ve IoT Hub 'ına bağlanan IoT Tak ve Kullan önizlemesi örnek Java cihaz kodu oluşturun ve çalıştırın. Cihaz tarafından hub 'a gönderilen bilgileri görüntülemek için Azure IoT gezgin aracını kullanın.
author: ericmitt
ms.author: ericmitt
ms.date: 07/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 1d16d8c54939c4f659b6a1530e2d360b957a09ad
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87352885"
---
# <a name="tutorial-connect-a-sample-iot-plug-and-play-preview-multiple-component-device-application-to-iot-hub-java"></a>Öğretici: bir örnek IoT Tak ve Kullan önizleme birden çok bileşen cihaz uygulamasını IoT Hub 'ye bağlama (Java)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

Bu öğretici, birden çok bileşen örnek IoT Tak ve Kullan cihaz uygulaması oluşturmayı, IoT Hub 'ınıza bağlamayı ve gönderdiği Telemetriyi görüntülemek için Azure CLı 'yi kullanmayı gösterir. Örnek uygulama Java 'da yazılmıştır ve Java için Azure IoT cihaz SDK 'sına dahildir. Bir çözüm Oluşturucusu, herhangi bir cihaz kodunu görüntülemeye gerek olmadan bir IoT Tak ve Kullan cihazının yeteneklerini anlamak için Azure CLı 'yı kullanabilir.

Bu öğreticide, bileşenler ve kök arabirimiyle örnek IoT Tak ve Kullan cihaz uygulaması oluşturma, IoT Hub 'ınıza bağlama ve Azure IoT gezgin aracını kullanarak hub 'a gönderdiği bilgileri görüntüleme hakkında bilgi verilmektedir. Örnek uygulama Java 'da yazılmıştır ve Java için Azure IoT cihaz SDK 'sına dahildir. Bir çözüm Oluşturucusu, herhangi bir cihaz kodunu görüntülemeye gerek olmadan IoT Tak ve Kullan cihazının yeteneklerini anlamak için Azure IoT gezgin aracını kullanabilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi Windows üzerinde gerçekleştirmek için, yerel Windows ortamınıza aşağıdaki yazılımları yüklersiniz:

* Java SE Development Kit 8. [Azure ve Azure Stack Için Java uzun süreli destek](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable), **uzun süreli destek**altında **Java 8**' i seçin.
* [Apache Maven 3](https://maven.apache.org/download.cgi).

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

[!INCLUDE [iot-pnp-download-models.md](../../includes/iot-pnp-download-models.md)]

## <a name="download-the-code"></a>Kodu indirme

Bu öğreticide, Azure IoT Hub cihaz Java SDK 'sını kopyalamak ve derlemek için kullanabileceğiniz bir geliştirme ortamı hazırlarsınız.

Seçtiğiniz dizinde bir komut istemi açın. [Azure IoT Java SDK 'ları ve kitaplıkları](https://github.com/Azure/azure-iot-sdk-java) GitHub deposunu bu konuma kopyalamak için aşağıdaki komutu yürütün:

```cmd
git clone https://github.com/Azure/azure-iot-sdk-java.git
```

Bu işlemin tamamlanması birkaç dakika sürer.

## <a name="build-the-code"></a>Kodu oluşturma

Windows üzerinde kopyalanmış Java SDK deposunun kök klasörüne gidin. Ardından *\devıceiot-Device-samples\pnp-Device-sample\temsilinebilir ture-Controller-Device-Sample* klasörüne gidin.

Örnek uygulamayı derlemek için aşağıdaki komutu çalıştırın:

```java
mvn clean package
```

## <a name="run-the-device-sample"></a>Cihaz örneğini çalıştırma

Daha önce bir nota yaptığınız cihaz bağlantı dizesini depolamak için **IOTHUB_DEVICE_CONNECTION_STRING** adlı bir ortam değişkeni oluşturun.

Örnek uygulamayı çalıştırmak için aşağıdaki komutu çalıştırın:

```java
mvn exec:java -Dexec.mainClass="samples.com.microsoft.azure.sdk.iot.device.TemperatureController"
```

Cihaz artık komutları ve özellik güncelleştirmelerini almaya hazır ve hub 'a telemetri verileri göndermeye başladı. Sonraki adımları tamamladıktan sonra örneği çalışır durumda tutun.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT Gezginini Kullanma

Cihaz istemcisi örneği başladıktan sonra, çalıştığını doğrulamak için Azure IoT gezgin aracını kullanın.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu örnek, IoT Tak ve Kullan ısı denetleyicisi cihazı uygular. Bu örneğin uyguladığı model [birden çok bileşen](concepts-components.md)kullanır. [Sıcaklık cihazının dijital TWINS tanım dili (DTDL) modeli dosyası](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) , cihazın uyguladığı telemetri, Özellikler ve komutları tanımlar.

Cihaz kodu, `DeviceClient` IoT Hub 'ınıza bağlanmak için standart sınıfını kullanır. Cihaz, bağlantı isteğinde uyguladığı DTDL modelinin model KIMLIĞINI gönderir. Model KIMLIĞI gönderen bir cihaz IoT Tak ve Kullan cihazdır:

```java
private static void initializeDeviceClient() throws URISyntaxException, IOException {
    ClientOptions options = new ClientOptions();
    options.setModelId(MODEL_ID);
    deviceClient = new DeviceClient(deviceConnectionString, protocol, options);

    deviceClient.registerConnectionStatusChangeCallback((status, statusChangeReason, throwable, callbackContext) -> {
        log.debug("Connection status change registered: status={}, reason={}", status, statusChangeReason);

        if (throwable != null) {
            log.debug("The connection status change was caused by the following Throwable: {}", throwable.getMessage());
            throwable.printStackTrace();
        }
    }, deviceClient);

    deviceClient.open();
}
```

Model KIMLIĞI, aşağıdaki kod parçacığında gösterildiği gibi kodda depolanır:

```java
private static final String MODEL_ID = "dtmi:com:example:Thermostat;1";
```

Cihaz IoT Hub 'ınıza bağlandıktan sonra kod, komut işleyicilerini kaydeder.

```java
deviceClient.subscribeToDeviceMethod(new MethodCallback(), null, new MethodIotHubEventCallback(), null);
```

İki termostat bileşenlerinde istenen özellik güncelleştirmelerine yönelik ayrı işleyiciler vardır:

```java
deviceClient.startDeviceTwin(new TwinIotHubEventCallback(), null, new GenericPropertyUpdateCallback(), null);
Map<Property, Pair<TwinPropertyCallBack, Object>> desiredPropertyUpdateCallback = Stream.of(
        new AbstractMap.SimpleEntry<Property, Pair<TwinPropertyCallBack, Object>>(
                new Property(THERMOSTAT_1, null),
                new Pair<>(new TargetTemperatureUpdateCallback(), THERMOSTAT_1)),
        new AbstractMap.SimpleEntry<Property, Pair<TwinPropertyCallBack, Object>>(
                new Property(THERMOSTAT_2, null),
                new Pair<>(new TargetTemperatureUpdateCallback(), THERMOSTAT_2))
).collect(Collectors.toMap(AbstractMap.SimpleEntry::getKey, AbstractMap.SimpleEntry::getValue));

deviceClient.subscribeToTwinDesiredProperties(desiredPropertyUpdateCallback);
```

Örnek kod, her bir termostat bileşeninden telemetri gönderir:

```java
sendTemperatureReading(THERMOSTAT_1);
sendTemperatureReading(THERMOSTAT_2);
```

`sendTemperatureReading`Yöntemi, `PnpHhelper` her bileşen için ileti oluşturmak üzere sınıfını kullanır:

```java
Message message = PnpHelper.createIotHubMessageUtf8(telemetryName, currentTemperature, componentName);
```

`PnpHelper`Sınıfı, birden çok bileşen modeliyle kullanabileceğiniz diğer örnek yöntemleri içerir.

İki termostat bileşeninden telemetri ve özellikleri görüntülemek için Azure IoT gezgin aracını kullanın:

:::image type="content" source="media/tutorial-multiple-components-java/multiple-component.png" alt-text="Azure IoT Gezgini 'nde birden çok bileşen cihazı":::

Ayrıca, iki termostat bileşeninden ya da kök arabirimde komutları çağırmak için Azure IoT gezgin aracını da kullanabilirsiniz.

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IoT Hub 'ına bileşenlerle IoT Tak ve Kullan cihazını bağlamayı öğrendiniz. IoT Tak ve Kullan cihaz modelleri hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [IoT Tak ve Kullan Preview modelleme Geliştirici Kılavuzu](concepts-developer-guide.md)
