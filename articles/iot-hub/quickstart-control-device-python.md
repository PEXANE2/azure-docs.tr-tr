---
title: Azure IoT Hub’dan bir cihazı denetleme hızlı başlangıcı (Python) | Microsoft Docs
description: Bu hızlı başlangıçta iki örnek Python uygulaması çalıştırırsınız. Bir uygulama, hub’ınıza bağlı cihazları uzaktan denetleyebilen bir arka uç uygulamasıdır. Diğer uygulama, uzaktan denetlenebilen hub’ınıza bağlanan bir cihazın simülasyonunu yapar.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/21/2019
ms.openlocfilehash: 6a3be3733c5041576d5db49256056ac4f0c03a7f
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2019
ms.locfileid: "71003005"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-python"></a>Hızlı Başlangıç: IoT Hub 'ına (Python) bağlı bir cihazı denetleme

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta yüksek hacimlerde telemetri almanızı ve buluttan cihazlarınızı yönetmenizi sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, IoT hub’ınıza bağlı bir simülasyon cihazını denetlemek için *doğrudan yöntem* kullanırsınız. IoT hub’ınıza bağlı bir cihazın davranışını uzaktan değiştirmek için doğrudan yöntemler kullanabilirsiniz.

Hızlı başlangıçta, önceden yazılmış iki Python uygulaması kullanılır:

* Bir arka uç uygulamasından çağrılan doğrudan yöntemlere yanıt veren bir simülasyon cihazı uygulaması. Doğrudan yöntem çağrıları almak için bu uygulama, IoT hub’ınızda aygıta özgü bir uç noktaya bağlanır.

* Simülasyon cihazında doğrudan yöntemler çağıran bir arka uç uygulaması. Bir cihazda doğrudan yöntem çağırmak için bu uygulama, IoT hub’ınızda sunucu tarafı uç noktasına bağlanır.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Azure CLı için Microsoft Azure IoT uzantısını Cloud Shell örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLı 'ye IoT Hub, IoT Edge ve IoT cihaz sağlama hizmeti 'ne (DPS) özel komutlar ekler.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Örnek Python projesini önceden indirmediyseniz https://github.com/Azure-Samples/azure-iot-samples-python/archive/master.zip adresinden indirip ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki [hızlı başlangıcı tamamladıysanız: Bir cihazdan IoT Hub 'ına](quickstart-send-telemetry-python.md)telemetri gönderin, bu adımı atlayabilirsiniz.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Önceki [hızlı başlangıcı tamamladıysanız: Bir cihazdan IoT Hub 'ına](quickstart-send-telemetry-python.md)telemetri gönderin, bu adımı atlayabilirsiniz.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliğini oluşturmak için Azure Cloud Shell aşağıdaki komutu çalıştırın.

    **Youriothubname**: Aşağıdaki yer tutucuyu IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    **Mypythondevice**: Bu, kayıtlı cihaz için verilen addır. Gösterilen MyPythonDevice değerini kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyPythonDevice
    ```

2. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

    **Youriothubname**: Aşağıdaki yer tutucuyu IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyPythonDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

3. Arka uç uygulamasının IoT hub’ınıza bağlanmasına ve iletileri almasına olanak sağlamak için bir _hizmet bağlantı dizesi_ de gerekir. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

    **Youriothubname**: Aşağıdaki yer tutucusunu, IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub show-connection-string \
      --name YourIoTHubName \
      --policy-name service \
      --output table
    ```

    Şu ifadeye benzer şekilde görünen hizmet bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız. Hizmet bağlantı dizesi, cihaz bağlantı dizesinden farklıdır.

## <a name="listen-for-direct-method-calls"></a>Doğrudan yöntem çağrılarını dinleme

Simülasyon cihazı, IoT hub’ınızdaki cihaza özgü bir uç noktaya bağlanır, sanal telemetri gönderir ve hub’ınızdan gelen doğrudan yöntem çağrılarını dinler. Bu hızlı başlangıçta, hub’dan gelen doğrudan yöntem çağrısı, telemetri gönderme aralığını değiştirmesini cihaza bildirir. Sanal cihaz, doğrudan yöntemini yürütmeden sonra hub 'ınıza bir bildirim gönderir.

1. Yerel terminal penceresinde, örnek Python projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\simulated-device-2** klasörüne gidin.

1. **SimulatedDevice.py** dosyasını, istediğiniz bir metin düzenleyicide açın.

    `CONNECTION_STRING` değişkeninin değerini, önceden not ettiğiniz cihaz bağlantı dizesiyle değiştirin. Daha sonra **SimulatedDevice.py** dosyasına değişikliklerinizi kaydedin.

1. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulaması için gerekli kitaplıkları yükleyin:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulamasını çalıştırın:

    ```cmd/sh
    python SimulatedDevice.py
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Simülasyon cihazını çalıştırma](./media/quickstart-control-device-python/SimulatedDevice-1.png)

## <a name="call-the-direct-method"></a>Doğrudan yöntem çağırma

Arka uç uygulaması, IoT Hub’ınızdaki bir hizmet tarafı uç noktasına bağlanır. Uygulama, IoT Hub 'ınız aracılığıyla bir cihaza doğrudan Yöntem çağrıları yapar ve bu bildirimleri dinler. IoT Hub arka uç uygulaması genellikle bulutta çalışır.

1. Başka bir yerel terminal penceresinde, örnek Python projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\back-end-application** klasörüne gidin.

1. **BackEndApplication.py** dosyasını, istediğiniz bir metin düzenleyicide açın.

    `CONNECTION_STRING` değişkeninin değerini, önceden not ettiğiniz hizmet bağlantı dizesiyle değiştirin. Sonra **BackEndApplication.py** dosyasına değişikliklerinizi kaydedin.

1. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulaması için gerekli kitaplıkları yükleyin:

    ```cmd/sh
    pip install azure-iothub-service-client future
    ```

1. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak arka uç uygulamasını çalıştırın:

    ```cmd/sh
    python BackEndApplication.py
    ```

    Aşağıdaki ekran görüntüsünde, uygulamanın cihaza doğrudan yöntem çağrısı yaptığı ve bir bildirim aldığı için çıkış gösterilmektedir:

    ![Arka uç uygulamasını çalıştırma](./media/quickstart-control-device-python/BackEndApplication.png)

    Arka uç uygulamasını çalıştırdıktan sonra, simülasyon cihazını çalıştıran konsol penceresinde bir ileti ve ileti değişikliklerini gönderdiği hızı görürsünüz:

    ![Sanal istemcide değişiklik](./media/quickstart-control-device-python/SimulatedDevice-2.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir arka uç uygulamasındaki cihazda doğrudan yöntem çağırdınız ve simülasyon cihazı uygulamasında doğrudan yöntem çağrısına yanıt verdiniz.

Cihazdan buluta iletileri, buluttaki farklı hedeflere yönlendirmeyi öğrenmek için sonraki öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: İşlem için telemetri farklı uç noktalara yol](tutorial-routing.md)