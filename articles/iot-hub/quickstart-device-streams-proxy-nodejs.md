---
title: Azure IoT Hub aygıtı SSH ve RDP için Node.js hızlı başlat'ı aktarıyor
description: Bu hızlı başlatmada, IoT Hub aygıt akışları üzerinden SSH ve RDP senaryolarını etkinleştirmek için proxy görevi gören örnek bir Düğüm.js uygulaması çalıştırın.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: c372a0a09fd3143f570aa4b316c9191e617c69e2
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78675455"
---
# <a name="quickstart-enable-ssh-and-rdp-over-an-iot-hub-device-stream-by-using-a-nodejs-proxy-application-preview"></a>Quickstart: Bir Düğüm.js proxy uygulaması (önizleme) kullanarak Bir IoT Hub aygıt akışı üzerinden SSH ve RDP etkinleştirin

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Bu hızlı başlatmada, Güvenli Kabuk (SSH) ve Uzak Masaüstü Protokolü (RDP) trafiğinin aygıta aygıt akışı üzerinden gönderilmesini sağlarsınız. Azure IoT Hub aygıt akışları, hizmet ve aygıt uygulamalarının güvenli ve güvenlik duvarı dostu bir şekilde iletişim kurmasına olanak tanır. Bu hızlı başlatma, hizmet tarafında çalışan bir Düğüm.js proxy uygulamasının yürütülmesini açıklar. Genel önizleme sırasında, Node.js SDK yalnızca hizmet tarafındaki aygıt akışlarını destekler. Sonuç olarak, bu hızlı başlatma yalnızca hizmet yerel proxy uygulamasını çalıştırmak için yönergeleri kapsar.

## <a name="prerequisites"></a>Ön koşullar

* C [proxy uygulaması kullanarak IoT Hub aygıt akışları üzerinden Etkinleştir SSH ve RDP'nin](./quickstart-device-streams-proxy-c.md) tamamlanması veya [C# proxy uygulaması kullanarak IoT Hub aygıt akışları üzerinden SSH ve RDP'yi etkinleştirin.](./quickstart-device-streams-proxy-csharp.md)

* Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

* [Düğüm.js 10+](https://nodejs.org).

* [Örnek bir Düğüm.js projesi.](https://github.com/Azure-Samples/azure-iot-samples-node/archive/streams-preview.zip)

Geliştirme makinenizdeki Node.js'nin geçerli sürümünü aşağıdaki komutu kullanarak doğrulayabilirsiniz:

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

Aşağıdaki komutu çalıştırarak Azure CLI için Azure IoT Uzantısı'nı Bulut Kabuğu örneğinize ekleyin. IoT Uzantı, Azure CLI'ye IoT Hub, IoT Edge ve IoT Aygıt Sağlama Hizmeti (DPS) komutlarını ekler.

```azurecli-interactive
az extension add --name azure-iot
```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-node.md) öğreticisini tamamladıysanız bu adımı atlayabilirsiniz.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

[Quickstart'ı tamamladıysanız: Bir aygıttan IoT hub'ına telemetri gönderin,](quickstart-send-telemetry-node.md)bu adımı atlayabilirsiniz.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu bölümde, simüle edilmiş bir aygıtı kaydetmek için Azure Bulut Su şurunu kullanırsınız.

1. Aygıt kimliğini oluşturmak için Bulut Kabuğu'nda aşağıdaki komutu çalıştırın:

   > [!NOTE]
   > * *YourIoTHubName* yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.
   > * Kaydolmakta olduğunuz aygıtın adı için *MyDevice'ın* gösterildiği gibi kullanılması önerilir. Aygıtınız için farklı bir ad seçerseniz, bu makale boyunca bu adı kullanın ve bunları çalıştırmadan önce örnek uygulamalardaki aygıt adını güncelleştirin.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyDevice
    ```

1. Arka uç uygulamasının IoT hub'ınıza bağlanıp iletileri alabilmesini sağlamak için bir *servis bağlantısı dizesi*de gerekir. Aşağıdaki komut IoT hub'ınız için dize alır:

   > [!NOTE]
   > *YourIoTHubName* yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name {YourIoTHubName} --output table
    ```

   Bu hızlı başlatmada daha sonra kullanmak üzere döndürülen hizmet bağlantı dizesini not edin. Aşağıdaki örneğe benzer şekilde görünür:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="ssh-to-a-device-via-device-streams"></a>Aygıt akışları üzerinden bir cihaza SSH

Bu bölümde, tünel SSH trafiğine uçlardan uca bir akış kurarsınız.

### <a name="run-the-device-local-proxy-application"></a>Aygıt-yerel proxy uygulamasını çalıştırma

Daha önce de belirtildiği gibi, IoT Hub Node.js SDK yalnızca servis tarafındaki aygıt akışlarını destekler. Aygıt yerel uygulaması için, aşağıdaki hızlı başlangıçlardan birinde kullanılabilen bir aygıt proxy uygulaması kullanın:

   * [C proxy uygulaması kullanarak IoT Hub aygıt akışları üzerinden SSH ve RDP'yi etkinleştirme](./quickstart-device-streams-proxy-c.md)
   * [C# proxy uygulaması kullanarak IoT Hub aygıt akışları üzerinden SSH ve RDP'yi etkinleştirme](./quickstart-device-streams-proxy-csharp.md) 

Bir sonraki adıma geçmeden önce, aygıt yerel proxy uygulamasının çalıştığını sağlayın. Kurulumun genel görünümü için [Yerel Proxy Örneği'ne](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp)bakın.

### <a name="run-the-service-local-proxy-application"></a>Hizmet yerel proxy uygulamasını çalıştırma

Bu makalede, SSH için kurulum açıklanır (bağlantı noktası 22 kullanarak) ve sonra RDP için kurulum değiştirmek için nasıl açıklanır (hangi bağlantı noktası 3389 kullanır). Aygıt akışları uygulama ve iletişim-agnostik olduğundan, genellikle iletişim bağlantı noktasını değiştirerek, istemci-sunucu uygulama trafiğinin diğer türlerini karşılamak için aynı örneği değiştirebilirsiniz.

Aygıt-yerel proxy uygulaması çalışırken, Node.js'de yazılmış olan hizmet yerel proxy uygulamasını yerel bir terminal penceresinde aşağıdakileri yaparak çalıştırın:

1. Ortam değişkenleri için, hizmet kimlik bilgilerinizi, SSH daemon'un çalıştığı hedef aygıt kimliğini ve aygıtta çalışan proxy'nin bağlantı noktası numarasını sağlayın.

   ```
   # In Linux
   export IOTHUB_CONNECTION_STRING="{ServiceConnectionString}"
   export STREAMING_TARGET_DEVICE="MyDevice"
   export PROXY_PORT=2222

   # In Windows
   SET IOTHUB_CONNECTION_STRING={ServiceConnectionString}
   SET STREAMING_TARGET_DEVICE=MyDevice
   SET PROXY_PORT=2222
   ```

   ServiceConnectionString yer tutucuyu servis bağlantı dizenize, **MyDevice'ı** ise sizinkine farklı bir ad verdiyseniz aygıt kimliğinize uyacak şekilde değiştirin.

1. Fermuarsız proje klasörünüzdeki dizine `Quickstarts/device-streams-service` gidin. Hizmet yerel proxy uygulamasını çalıştırmak için aşağıdaki kodu kullanın:

   ```
   cd azure-iot-samples-node-streams-preview/iot-hub/Quickstarts/device-streams-service

   # Install the preview service SDK, and other dependencies
   npm install azure-iothub@streams-preview
   npm install

   # Run the service-local proxy application
   node proxy.js
   ```

### <a name="ssh-to-your-device-via-device-streams"></a>Cihaz akışları üzerinden cihazınıza SSH

Linux'ta, bir terminalde kullanarak `ssh $USER@localhost -p 2222` SSH çalıştırın. Windows'da en sevdiğiniz SSH istemcinizi kullanın (örneğin, PuTTY).

SSH oturumu oluşturulduktan sonra hizmet-yerel konsol çıkışı (servis-yerel proxy uygulaması port 2222 dinler):

![SSH terminal çıkışı](./media/quickstart-device-streams-proxy-nodejs/service-console-output.png)

SSH istemci uygulamasının konsol çıkışı (SSH istemcisi, hizmet-yerel proxy uygulamasının dinlediği bağlantı noktası 22'ye bağlanarak SSH daemon'a iletir):

![SSH istemci çıktısı](./media/quickstart-device-streams-proxy-nodejs/ssh-console-output.png)

### <a name="rdp-to-your-device-via-device-streams"></a>Cihaz akışları üzerinden cihazınıza RDP

Şimdi RDP istemci uygulamanızı kullanın ve daha önce seçtiğiniz rasgele bir bağlantı noktası olan 2222 bağlantı noktasındaki servis proxy'sine bağlanın.

> [!NOTE]
> Cihaz proxy'nizin RDP için doğru şekilde yapılandırıldığından ve RDP portu 3389 ile yapılandırıldığından emin olun.

![RDP istemcisi hizmet yerel proxy uygulamasına bağlanır](./media/quickstart-device-streams-proxy-nodejs/rdp-screen-capture.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir IoT hub'ı kurdunuz, bir aygıtı kaydettiniz ve bir IoT aygıtında RDP ve SSH'yi etkinleştirmek için bir hizmet proxy uygulaması dağıttınız. RDP ve SSH trafiği, IoT hub'ı üzerinden bir cihaz akışı üzerinden tünel etüt edilecektir. Bu işlem, aygıta doğrudan bağlantı gereksinimini ortadan kaldırır.

Aygıt akışları hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Cihaz akışlarına genel bakış](./iot-hub-device-streams-overview.md)
