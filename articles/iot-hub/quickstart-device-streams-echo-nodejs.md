---
title: Azure IoT Hub aygıt akışları yla Düğüm.js'deki aygıt uygulamasıyla iletişim kurun
description: Bu hızlı başlatmada, bir Aygıt akışı üzerinden bir IoT aygıtıyla iletişim kuran bir Düğüm.js hizmet tarafı uygulaması çalıştırın.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: 0757c5eb8639e4a864b049adc92c97a7cf69adba
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78675501"
---
# <a name="quickstart-communicate-to-a-device-application-in-nodejs-via-iot-hub-device-streams-preview"></a>Quickstart: IoT Hub aygıt akışları (önizleme) üzerinden Node.js bir aygıt uygulamasıile iletişim

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Bu hızlı başlatmada, bir servis tarafında uygulama çalıştırın ve aygıt akışlarını kullanarak bir aygıt ve hizmet arasında iletişim ayarlayın. Azure IoT Hub aygıt akışları, hizmet ve aygıt uygulamalarının güvenli ve güvenlik duvarı dostu bir şekilde iletişim kurmasına olanak tanır. Genel önizleme sırasında, Node.js SDK yalnızca hizmet tarafındaki aygıt akışlarını destekler. Sonuç olarak, bu hızlı başlatma yalnızca hizmet tarafı uygulamasını çalıştırmak için yönergeleri kapsar.

## <a name="prerequisites"></a>Ön koşullar

* [IoT Hub aygıt akışları üzerinden C'deki aygıt uygulamalarına iletişim](./quickstart-device-streams-echo-c.md) kurma veya [IoT Hub aygıt akışları aracılığıyla C#'daki aygıt uygulamalarına iletişim](./quickstart-device-streams-echo-csharp.md)kurma.

* Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

* [Düğüm.js 10+](https://nodejs.org).

* [Örnek bir Düğüm.js projesi.](https://github.com/Azure-Samples/azure-iot-samples-node/archive/streams-preview.zip)

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Node.js sürümünü doğrulayabilirsiniz:

```cmd/sh
node --version
```

Microsoft Azure IoT Hub şu anda bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)olarak aygıt akışlarını destekler.

> [!IMPORTANT]
> Aygıt akışlarının önizlemesi şu anda yalnızca aşağıdaki bölgelerde oluşturulan IoT Hub'ları için desteklenir:
>
> * Orta ABD
> * ORTA ABD EUAP
> * Kuzey Avrupa
> * Güneydoğu Asya

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="add-azure-iot-extension"></a>Azure IoT Uzantısı Ekle

Bulut Kabuğu örneğinize Azure CLI için Microsoft Azure IoT Uzantısı'nı eklemek için aşağıdaki komutu çalıştırın. IoT Uzantı, Azure CLI'ye IoT Hub, IoT Edge ve IoT Aygıt Sağlama Hizmeti (DPS) komutlarını ekler.

```azurecli-interactive
az extension add --name azure-iot
```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-node.md) öğreticisini tamamladıysanız bu adımı atlayabilirsiniz.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Önceki [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-node.md) öğreticisini tamamladıysanız bu adımı atlayabilirsiniz.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Aygıt kimliğini oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

   **MyDevice**: Bu, kaydettiğiniz aygıtın adıdır. Gösterildiği gibi **MyDevice** kullanılması önerilir. Aygıtınız için farklı bir ad seçerseniz, bu makale boyunca bu adı kullanmanız ve bunları çalıştırmadan önce örnek uygulamalardaki aygıt adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyDevice
    ```

2. Arka uç uygulamasının IoT hub’ınıza bağlanmasına ve iletileri almasına olanak sağlamak için bir *hizmet bağlantı dizesi* de gerekir. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

    **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name {YourIoTHubName} --output table
    ```

    Bu hızlı başlatmada daha sonra kullanmak üzere döndürülen hizmet bağlantı dizesini not edin. Aşağıdaki örneğe benzer şekilde görünür:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="communicate-between-device-and-service-via-device-streams"></a>Cihaz akışları üzerinden cihaz ve hizmet arasında iletişim kurma

Bu bölümde, hem aygıt tarafı uygulamasını hem de hizmet tarafı uygulamasını çalıştırın ve ikisi arasında iletişim kurarsınız.

### <a name="run-the-device-side-application"></a>Aygıt tarafındaki uygulamayı çalıştırma

Daha önce de belirtildiği gibi, IoT Hub Node.js SDK yalnızca servis tarafındaki aygıt akışlarını destekler. Aygıt tarafı uygulaması için, aşağıdaki hızlı başlangıçlarda bulunan eşlik eden aygıt programlarından birini kullanın:

* [IoT Hub cihaz akışları aracılığıyla C'deki aygıt uygulamalarıyla iletişim kurun](./quickstart-device-streams-echo-c.md)

* [IoT Hub cihaz akışları aracılığıyla C#'daki aygıt uygulamalarıyla iletişim kurun](./quickstart-device-streams-echo-csharp.md)

Bir sonraki adıma geçmeden önce aygıt tarafındaki uygulamanın çalıştığını sağlayın.

### <a name="run-the-service-side-application"></a>Hizmet tarafındaki uygulamayı çalıştırma

Bu quickstart'taki servis tarafı Düğüm.js uygulaması aşağıdaki işlevlere sahiptir:

* Bir IoT aygıtına aygıt akışı oluşturur.
* Komut satırından girişi okur ve geri yankılanacak aygıt uygulamasına gönderir.

Kod, bir aygıt akışının başlatma işlemini ve veri göndermek ve almak için nasıl kullanılacağını gösterir.

Aygıt tarafındaki uygulamanın çalıştığını varsayarsak, Node.js'deki servis tarafı uygulamasını çalıştırmak için yerel bir terminal penceresinde aşağıdaki adımları izleyin:

* Hizmet kimlik bilgilerinizi ve aygıt kimliğinizi ortam değişkeni olarak sağlayın.
 
   ```cmd/sh
   # In Linux
   export IOTHUB_CONNECTION_STRING="{ServiceConnectionString}"
   export STREAMING_TARGET_DEVICE="MyDevice"

   # In Windows
   SET IOTHUB_CONNECTION_STRING={ServiceConnectionString}
   SET STREAMING_TARGET_DEVICE=MyDevice
   ```
  
   ServiceConnectionString yer tutucuyu servis bağlantı dizenize, **MyDevice'ı** ise sizinkine farklı bir ad verdiyseniz aygıt kimliğinize uyacak şekilde değiştirin.

* `Quickstarts/device-streams-service` Fermuarsız proje klasörünüze gidin ve düğümü kullanarak örneği çalıştırın.

   ```cmd/sh
   cd azure-iot-samples-node-streams-preview/iot-hub/Quickstarts/device-streams-service
    
   # Install the preview service SDK, and other dependencies
   npm install azure-iothub@streams-preview
   npm install

   node echo.js
   ```

Son adımın sonunda, hizmet tarafı programı cihazınıza bir akış başlatacak ve kurulduktan sonra akış üzerinden hizmete bir dize arabelleği gönderir. Bu örnekte, servis tarafındaki program `stdin` yalnızca terminaldeki leri okur ve aygıta gönderir ve bu program tekrar yankılanır. Bu, iki uygulama arasında başarılı çift yönlü iletişim gösterir.

![Servis tarafı konsol çıkışı](./media/quickstart-device-streams-echo-nodejs/service-console-output.png)

Daha sonra yeniden enter tuşuna basarak programı sonlandırabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir IoT hub'ı kurdunuz, bir aygıtı kaydettiniz, aygıt ve hizmet tarafındaki uygulamalar arasında bir aygıt akışı oluşturdunuz ve uygulamalar arasında ileri geri veri göndermek için akışı kullandınız.

Aygıt akışları hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

> [!div class="nextstepaction"]
> [Cihaz akışlarına genel bakış](./iot-hub-device-streams-overview.md) 