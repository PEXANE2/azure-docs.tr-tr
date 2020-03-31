---
title: Azure IoT Hub aygıtı, SSH ve RDP için C# hızlı başlat'ı aktarıyor
description: Bu hızlı başlatmada, Bir IoT Hub aygıt akışı üzerinden SSH ve RDP senaryolarını etkinleştiren iki örnek C# uygulaması çalıştırırsınız.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: c8ef6a87e839e6d8dfb296e7b24f3d0d95843d1c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78675472"
---
# <a name="quickstart-enable-ssh-and-rdp-over-an-iot-hub-device-stream-by-using-a-c-proxy-application-preview"></a>Quickstart: C# proxy uygulaması (önizleme) kullanarak IoT Hub aygıt akışı üzerinden SSH ve RDP'yi etkinleştirme

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IoT Hub şu anda bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)olarak aygıt akışlarını destekler.

[IoT Hub aygıt akışları,](iot-hub-device-streams-overview.md) hizmet ve aygıt uygulamalarının güvenli ve güvenlik duvarı dostu bir şekilde iletişim kurmasına olanak tanır. Bu hızlı başlatma kılavuzu, istemci-sunucu uygulama trafiğinin (Secure Shell [SSH] ve Uzak Masaüstü Protokolü [RDP] gibi bir IoT hub'ı üzerinden kurulan bir aygıt akışı üzerinden gönderilmesini sağlayan iki C# uygulamasını içerir. Kurulumun genel görünümü [için, SSH veya RDP için Yerel proxy uygulama örneğine](iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp)bakın.

Bu makalede, önce SSH (bağlantı noktası 22 kullanarak) için kurulum açıklanır ve daha sonra rdp için kurulum bağlantı noktası değiştirmek için nasıl açıklanır. Aygıt akışları uygulama ve protokol agnostik olduğundan, aynı örnek diğer uygulama trafiği türlerini barındıracak şekilde değiştirilebilir. Bu değişiklik genellikle yalnızca iletişim bağlantı noktasını amaçlanan uygulama tarafından kullanılan bağlantı noktasıyla değiştirmeyi içerir.

## <a name="how-it-works"></a>Nasıl çalışır?

Aşağıdaki şekilde, bu örnekteki aygıt yerel ve hizmet yerel proxy uygulamalarının SSH istemcisi ile SSH daemon işlemleri arasında uçlardan uca bağlantıyı nasıl sağladığı gösteriş tir. Burada, daemon aygıt yerel proxy uygulaması ile aynı cihazda çalıştığını varsayıyoruz.

![Yerel proxy uygulama kurulumu](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg)

1. Hizmet yerel proxy uygulaması IoT hub'ına bağlanır ve hedef aygıta bir aygıt akışı başlatır.

1. Aygıt yerel proxy uygulaması akış başlatma el sıkışmasını tamamlar ve IoT hub'ın akış bitiş noktasından hizmet tarafına uçtan uca bir akış tüneli kurar.

1. Aygıt yerel proxy uygulaması, aygıttaki 22 bağlantı noktasından dinleyen SSH daemon'a bağlanır. Bu ayar, "Aygıt yerel proxy uygulamasını çalıştır" bölümünde açıklandığı gibi yapılandırılabilir.

1. Hizmet yerel proxy uygulaması, bu durumda bağlantı noktası 2222 olan belirlenmiş bir bağlantı noktası üzerinde dinleyerek bir kullanıcıdan yeni SSH bağlantıları bekler. Bu ayar, "Hizmet yerel proxy uygulamasını çalıştır" bölümünde açıklandığı gibi yapılandırılabilir. Kullanıcı SSH istemcisi üzerinden bağlandığında, tünel SSH uygulama trafiğinin SSH istemcisi ile sunucu uygulaması arasında aktarılmasını sağlar.

> [!NOTE]
> Aygıt akışı üzerinden gönderilen SSH trafiği, doğrudan servis ve aygıt arasında gönderilmek yerine IoT hub'ın akış bitiş noktasından tünellenir. Daha fazla bilgi [için, Iot Hub aygıt akışlarını kullanmanın yararlarına](iot-hub-device-streams-overview.md#benefits)bakın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Aygıt akışlarının önizlemesi şu anda yalnızca aşağıdaki bölgelerde oluşturulan IoT hub'ları için desteklenir:

  * Orta ABD
  * ORTA ABD EUAP
  * Güneydoğu Asya
  * Kuzey Avrupa

* Bu hızlı başlatmada çalıştırdığınız iki örnek uygulama C# olarak yazılır. .NET Core SDK 2.1.0 veya daha sonra geliştirme makineniz de gerekir.

  [.NET'ten birden fazla platform için .NET Core SDK'yı](https://www.microsoft.com/net/download/all)indirebilirsiniz.

* Aşağıdaki komutu kullanarak geliştirme makinenizdeki C#'ın geçerli sürümünü doğrulayın:

    ```
    dotnet --version
    ```

* Bulut Kabuğu örneğinize Azure CLI için Azure IoT Uzantısı'nı eklemek için aşağıdaki komutu çalıştırın. IOT Uzantı, Azure CLI'ye IoT Hub, IoT Edge ve IoT Aygıt Sağlama Hizmeti (DPS)'ye özel komutları ekler.

   ```azurecli-interactive
   az extension add --name azure-iot
   ```

   ```azurecli-interactive
   az extension add --name azure-iot
   ```
[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

* [Azure IoT C# örneklerini indirin](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip)ve ZIP arşivini ayıklayın.

* Kullanıcının kimliğini doğrulamak için kullanılan aygıtta (Windows veya Linux) geçerli bir kullanıcı hesabı ve kimlik bilgisi.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlatmada, simüle edilmiş bir aygıtı kaydetmek için Azure Bulut Bulutu'nu kullanırsınız.

1. Aygıt kimliğini oluşturmak için Bulut Kabuğu'nda aşağıdaki komutu çalıştırın:

   > [!NOTE]
   > * *YourIoTHubName* yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.
   > * Kaydolmakta olduğunuz aygıtın adı için *MyDevice'ın* gösterildiği gibi kullanılması önerilir. Aygıtınız için farklı bir ad seçerseniz, bu makale boyunca bu adı kullanın ve bunları çalıştırmadan önce örnek uygulamalardaki aygıt adını güncelleştirin.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyDevice
    ```

1. Az önce kaydettiğiniz aygıtın *aygıt bağlantı dizesini* almak için Bulut Kabuğu'nda aşağıdaki komutları çalıştırın:

   > [!NOTE]
   > *YourIoTHubName* yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyDevice --output table
    ```

    Bu hızlı başlatmada daha sonra kullanmak üzere döndürülen aygıt bağlantı dizesini not edin. Aşağıdaki örneğe benzer şekilde görünür:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

1. IoT hub'ınıza bağlanmak ve bir aygıt akışı oluşturmak için, servis tarafındaki uygulamayı etkinleştirmek için IoT hub'ınızdaki *servis bağlantısı dizesine* de ihtiyacınız vardır. Aşağıdaki komut IoT hub'ınız için bu değeri alır:

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

Yerel bir terminal penceresinde, `device-streams-proxy/device` gün önce gün önce gün önce açılan proje klasörünüzdeki dizine gidin. Aşağıdaki bilgileri kullanışlı tutun:

| Bağımsız değişken adı | Bağımsız değişken değeri |
|----------------|-----------------|
| `DeviceConnectionString` | Daha önce oluşturduğunuz aygıtın aygıt bağlantı dizesi. |
| `targetServiceHostName` | SSH sunucusunun dinlediği IP adresi. Adres, aygıt `localhost` yerel proxy uygulamasının çalıştığı aynı IP olsaydı olurdu. |
| `targetServicePort` | Uygulama protokolünüz tarafından kullanılan bağlantı noktası (SSH için varsayılan olarak, bu bağlantı noktası 22 olacaktır).  |

Kodu aşağıdaki komutlarla derle ve çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run ${DeviceConnectionString} localhost 22

# In Windows
dotnet run {DeviceConnectionString} localhost 22
```

### <a name="run-the-service-local-proxy-application"></a>Hizmet yerel proxy uygulamasını çalıştırma

Başka bir yerel terminal `iot-hub/quickstarts/device-streams-proxy/service` penceresinde, gün açmamış proje klasörünüze gidin. Aşağıdaki bilgileri kullanışlı tutun:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `ServiceConnectionString` | IoT Hub'ınızın servis bağlantısı dizesi. |
| `MyDevice` | Daha önce oluşturduğunuz aygıtın tanımlayıcısı. |
| `localPortNumber` | SSH istemcinizin bağlanacak yerel bir bağlantı noktası. Bu örnekte port 2222'yi kullanıyoruz, ancak diğer rasgele numaraları kullanabilirsiniz. |

Kodu aşağıdaki komutlarla derle ve çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/service/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run ${ServiceConnectionString} MyDevice 2222

# In Windows
dotnet run {ServiceConnectionString} MyDevice 2222
```

### <a name="run-the-ssh-client"></a>SSH istemcisini çalıştırma

Şimdi SSH istemci uygulamanızı kullanın ve bağlantı noktası 2222'deki servis yerel proxy uygulamasına bağlanın (doğrudan SSH daemon yerine).

```
ssh {username}@localhost -p 2222
```

Bu noktada, SSH oturum açma penceresi kimlik bilgilerinizi girmenizi ister.

Hizmet tarafında konsol çıkışı (servis-yerel proxy uygulaması bağlantı noktası 2222 dinler):

![Hizmet-yerel proxy uygulama çıktısı](./media/quickstart-device-streams-proxy-csharp/service-console-output.png)

*IP_address:22'de*SSH daemon'a bağlanan aygıt-yerel proxy uygulamasında konsol çıkışı :

![Aygıt-yerel proxy uygulama çıktısı](./media/quickstart-device-streams-proxy-csharp/device-console-output.png)

SSH istemci uygulamasının konsol çıktısı. SSH istemcisi, hizmet yerel proxy uygulamasının dinlediği bağlantı noktası 22'ye bağlanarak SSH daemon'a iletişim kurar:

![SSH istemci uygulama çıktısı](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png)

## <a name="rdp-to-a-device-via-device-streams"></a>Cihaz akışları üzerinden cihaza RDP

RDP kurulumu SSH kurulumuna benzer (yukarıda açıklanmıştır). Bunun yerine RDP hedef IP ve bağlantı noktası 3389'u ve RDP istemcisini (SSH istemcisi yerine) kullanırsınız.

### <a name="run-the-device-local-proxy-application-rdp"></a>Aygıt yerel proxy uygulamasını (RDP) çalıştırma

Yerel bir terminal penceresinde, `device-streams-proxy/device` gün önce gün önce gün önce açılan proje klasörünüzdeki dizine gidin. Aşağıdaki bilgileri kullanışlı tutun:

| Bağımsız değişken adı | Bağımsız değişken değeri |
|----------------|-----------------|
| `DeviceConnectionString` | Daha önce oluşturduğunuz aygıtın aygıt bağlantı dizesi. |
| `targetServiceHostName` | RDP sunucusunun çalıştığı ana bilgisayar adı veya IP adresi. Adres, aygıt `localhost` yerel proxy uygulamasının çalıştığı aynı IP olsaydı olurdu. |
| `targetServicePort` | Uygulama protokolünüz tarafından kullanılan bağlantı noktası (RDP için varsayılan olarak, bu bağlantı noktası 3389 olacaktır).  |

Kodu aşağıdaki komutlarla derle ve çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device

# Run the application
# In Linux or macOS
dotnet run ${DeviceConnectionString} localhost 3389

# In Windows
dotnet run {DeviceConnectionString} localhost 3389
```

### <a name="run-the-service-local-proxy-application-rdp"></a>Hizmet yerel proxy uygulamasını (RDP) çalıştırma

Başka bir yerel terminal `device-streams-proxy/service` penceresinde, gün açmamış proje klasörünüze gidin. Aşağıdaki bilgileri kullanışlı tutun:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `ServiceConnectionString` | IoT Hub'ınızın servis bağlantısı dizesi. |
| `MyDevice` | Daha önce oluşturduğunuz aygıtın tanımlayıcısı. |
| `localPortNumber` | SSH istemcinizin bağlanacak yerel bir bağlantı noktası. Bu örnekte port 2222'yi kullanıyoruz, ancak bunu diğer rasgele sayılarla değiştirebilirsiniz. |

Kodu aşağıdaki komutlarla derle ve çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/service/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run ${ServiceConnectionString} MyDevice 2222

# In Windows
dotnet run {ServiceConnectionString} MyDevice 2222
```

### <a name="run-rdp-client"></a>RDP istemcisi çalıştırın

Şimdi RDP istemci uygulamanızı kullanın ve bağlantı noktası 2222'deki hizmet yerel proxy uygulamasına bağlanın (bu, daha önce seçtiğiniz rasgele kullanılabilir bir bağlantı noktasıydı).

![RDP, hizmet yerel proxy uygulamasına bağlanır](./media/quickstart-device-streams-proxy-csharp/rdp-screen-capture.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir IoT hub'ı kurdunuz, bir aygıtı kaydettiniz, IoT hub'ı üzerinden bir aygıt akışı oluşturmak için aygıt-yerel ve hizmet yerel proxy uygulamalarını dağıttınız ve proxy uygulamalarını SSH veya RDP trafiğini tünellemek için kullandınız. Aynı paradigma, sunucunun aygıtta çalıştığı diğer istemci-sunucu protokollerini (örneğin, SSH daemon) barındırabilir.

Aygıt akışları hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Cihaz akışlarına genel bakış](./iot-hub-device-streams-overview.md)
