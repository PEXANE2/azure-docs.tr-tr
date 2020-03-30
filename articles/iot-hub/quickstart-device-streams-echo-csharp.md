---
title: Azure IoT Hub aygıt akışları yla C#'daki aygıt uygulamasıyla iletişim kurun
description: Bu hızlı başlatmada, IoT Hub üzerinden kurulan bir aygıt akışı üzerinden iletişim kuran iki örnek C# uygulaması çalıştırırsınız.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: 64af62cb6c2c56ca8c7e67e2f1467d4a7e8335a0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78675517"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Quickstart: IoT Hub aygıt akışları (önizleme) üzerinden C# bir aygıt uygulamasına iletişim

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Azure IoT Hub şu anda bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)olarak aygıt akışlarını destekler.

[IoT Hub aygıt akışları,](./iot-hub-device-streams-overview.md) hizmet ve aygıt uygulamalarının güvenli ve güvenlik duvarı dostu bir şekilde iletişim kurmasına olanak tanır. Bu hızlı başlatma, veri ileri geri (yankı) göndermek için aygıt akışlarından yararlanan iki C# uygulaması içerir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Aygıt akışlarının önizlemesi şu anda yalnızca aşağıdaki bölgelerde oluşturulan IoT hub'ları için desteklenir:
  * Orta ABD
  * ORTA ABD EUAP
  * Kuzey Avrupa
  * Güneydoğu Asya

* Bu hızlı başlatmada çalıştırdığınız iki örnek uygulama C# olarak yazılır. .NET Core SDK 2.1.0 veya daha sonra geliştirme makineniz de gerekir.
  * [.NET'ten birden fazla platform için .NET Core SDK'yı](https://www.microsoft.com/net/download/all)indirin.
  * Aşağıdaki komutu kullanarak geliştirme makinenizdeki C#'ın geçerli sürümünü doğrulayın:

   ```
   dotnet --version
   ```

* Aşağıdaki komutu çalıştırarak Azure CLI için Azure IoT Uzantısı'nı Bulut Kabuğu örneğinize ekleyin. IOT Uzantı, Azure CLI'ye IoT Hub, IoT Edge ve IoT Aygıt Sağlama Hizmeti (DPS)'ye özel komutları ekler.

    ```azurecli-interactive
    az extension add --name azure-iot
    ```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

* [Azure IoT C# örneklerini indirin](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) ve ZIP arşivini ayıklayın. Hem cihaz tarafında hem de servis tarafında ihtiyacınız vardır.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu bölümde, simüle edilmiş bir aygıtı kaydetmek için Azure Bulut Su şurunu kullanırsınız.

1. Aygıt kimliğini oluşturmak için Bulut Kabuğu'nda aşağıdaki komutu çalıştırın:

   > [!NOTE]
   > * *YourIoTHubName* yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.
   > * Kaydolmakta olduğunuz aygıtın adı için *MyDevice'ın* gösterildiği gibi kullanılması önerilir. Aygıtınız için farklı bir ad seçerseniz, bu makale boyunca bu adı kullanın ve bunları çalıştırmadan önce örnek uygulamalardaki aygıt adını güncelleştirin.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyDevice
    ```

1. Az önce kaydettiğiniz aygıtın *aygıt bağlantı dizesini* almak için Bulut Kabuğu'nda aşağıdaki komutu çalıştırın:

   > [!NOTE]
   > *YourIoTHubName* yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyDevice --output table
    ```

    Bu hızlı başlatmada daha sonra kullanmak üzere döndürülen aygıt bağlantı dizesini not edin. Aşağıdaki örneğe benzer şekilde görünür:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

3. Ayrıca, hizmet tarafındaki uygulamanın IoT hub'ınıza bağlanmasını ve bir aygıt akışı oluşturmasını sağlamak için IoT hub'ınızdaki *servis bağlantı dizesine* de ihtiyacınız vardır. Aşağıdaki komut IoT hub'ınız için bu değeri alır:

   > [!NOTE]
   > *YourIoTHubName* yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name {YourIoTHubName} --output table
    ```

    Bu hızlı başlatmada daha sonra kullanmak üzere döndürülen hizmet bağlantı dizesini not edin. Aşağıdaki örneğe benzer şekilde görünür:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="communicate-between-the-device-and-the-service-via-device-streams"></a>Cihaz akışları üzerinden cihaz ve hizmet arasında iletişim kurma

Bu bölümde, hem aygıt tarafı uygulamasını hem de hizmet tarafı uygulamasını çalıştırın ve ikisi arasında iletişim kurarsınız.

### <a name="run-the-service-side-application"></a>Hizmet tarafındaki uygulamayı çalıştırma

Yerel bir terminal penceresinde, `iot-hub/Quickstarts/device-streams-echo/service` gün önce gün önce gün önce açılan proje klasörünüzdeki dizine gidin. Aşağıdaki bilgileri kullanışlı tutun:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `ServiceConnectionString` | IoT hub'ınızın servis bağlantısı dizesi. |
| `MyDevice` | Daha önce oluşturduğunuz aygıtın tanımlayıcısı. |

Kodu aşağıdaki komutlarla derle ve çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-echo/service/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run "{ServiceConnectionString}" "MyDevice"

# In Windows
dotnet run {ServiceConnectionString} MyDevice
```
Uygulama, aygıt uygulamasının kullanılabilir hale gelmesini bekler.

> [!NOTE]
> Aygıt tarafındaki uygulama zamanında yanıt vermezse bir zaman ayarı oluşur.

### <a name="run-the-device-side-application"></a>Aygıt tarafındaki uygulamayı çalıştırma

Başka bir yerel terminal penceresinde, sıkılmamış proje klasörünüzdeki dizine `iot-hub/Quickstarts/device-streams-echo/device` gidin. Aşağıdaki bilgileri kullanışlı tutun:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `DeviceConnectionString` | IoT Hub'ınızın aygıt bağlantı dizesi. |

Kodu aşağıdaki komutlarla derle ve çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-echo/device/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run "{DeviceConnectionString}"

# In Windows
dotnet run {DeviceConnectionString}
```

Son adımın sonunda, servis tarafındaki uygulama cihazınıza bir akış başlatır. Akış oluşturulduktan sonra, uygulama akış üzerinden hizmete bir dize arabelleği gönderir. Bu örnekte, servis tarafındaki uygulama, iki uygulama arasında başarılı bir çift yönlü iletişim gösteren aynı verileri aygıta geri verir.

Cihaz tarafında konsol çıkışı:

![Cihaz tarafında konsol çıkışı](./media/quickstart-device-streams-echo-csharp/device-console-output.png)

Hizmet tarafında konsol çıkışı:

![Hizmet tarafında konsol çıkışı](./media/quickstart-device-streams-echo-csharp/service-console-output.png)

Akış üzerinden gönderilen trafik, doğrudan gönderilmek yerine IoT hub'ından tünele işlenir. Sağlanan avantajlar Cihaz [akışlarının avantajlarında](./iot-hub-device-streams-overview.md#benefits)ayrıntılı olarak açıklanmıştır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir IoT hub'ı kurdunuz, bir aygıtı kaydettiniz, aygıt ve hizmet kenarlarındaki C# uygulamaları arasında bir aygıt akışı oluşturdunuz ve uygulamalar arasında veri göndermek için akışı kullandınız.

Aygıt akışları hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Cihaz akışlarına genel bakış](./iot-hub-device-streams-overview.md)
