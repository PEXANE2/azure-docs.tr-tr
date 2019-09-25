---
title: SSH ve RDP için C# Azure IoT Hub cihaz akışları hızlı başlangıcı (Önizleme) | Microsoft Docs
description: Bu hızlı başlangıçta, SSH ve RDP senaryolarını C# IoT Hub bir cihaz akışı üzerinden etkinleştiren iki örnek uygulama çalıştırırsınız.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: ab6c381e779ddc19211f183b9bc80e586f58e804
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71261419"
---
# <a name="quickstart-enable-ssh-and-rdp-over-an-iot-hub-device-stream-by-using-a-c-proxy-application-preview"></a>Hızlı Başlangıç: C# Proxy uygulaması (Önizleme) kullanarak IoT Hub cihaz akışı üzerinden SSH ve RDP 'yi etkinleştirme

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IoT Hub Şu anda cihaz akışlarını [Önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)olarak desteklemektedir.

[Cihaz akışları IoT Hub](iot-hub-device-streams-overview.md) hizmet ve cihaz uygulamalarının güvenli ve güvenlik duvarı kolay bir şekilde iletişim kurmasına olanak tanır. Bu hızlı başlangıç kılavuzunda, C# istemci-sunucu uygulama trafiğinin (örneğin, Secure Shell [SSH] ve Uzak Masaüstü Protokolü [RDP]) IoT Hub 'ı aracılığıyla yüklenen bir cihaz akışından gönderilmesi için iki uygulama vardır. Kuruluma genel bakış için bkz. [SSH veya RDP Için yerel proxy uygulama örneği](iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp).

Bu makalede ilk olarak SSH kurulumu (bağlantı noktası 22 kullanılarak) açıklanmakta ve ardından Kurulum 'un RDP için bağlantı noktasının nasıl değiştirileceği açıklanmaktadır. Cihaz akışları uygulama ve protokol belirsiz olduğundan, aynı örnek diğer uygulama trafiği türlerini kapsayacak şekilde değiştirilebilir. Bu değişiklik genellikle iletişim bağlantı noktasını amaçlanan uygulama tarafından kullanılan bir ile değiştirmeyi içerir.

## <a name="how-it-works"></a>Nasıl çalışır?

Aşağıdaki şekilde, bu örnekteki cihaz yerel ve hizmet yerel proxy uygulamalarının SSH istemcisi ile SSH Daemon işlemlerinde uçtan uca bağlantıyı nasıl etkinleştireceğinizi gösterilmektedir. Burada, arka plan programının cihaz yerel ara sunucu uygulamasıyla aynı cihazda çalıştığını varsayalım.

![Yerel proxy uygulama kurulumu](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg)

1. Hizmet yerel proxy uygulaması IoT Hub 'ına bağlanır ve hedef cihaza bir cihaz akışı başlatır.

1. Cihaz yerel proxy uygulaması, akış başlatma anlaşmasını tamamlar ve IoT Hub 'ının akış uç noktası aracılığıyla hizmet tarafına uçtan uca bir akış tüneli oluşturur.

1. Cihaz yerel proxy uygulaması, cihazda 22 numaralı bağlantı noktasında dinleme yapan SSH Daemon 'a bağlanır. Bu ayar, "cihaz-yerel proxy uygulaması çalıştırma" bölümünde açıklandığı gibi yapılandırılabilir.

1. Hizmet yerel proxy uygulaması, bir kullanıcıdan belirli bir bağlantı noktasını dinleyerek, bu durumda 2222 numaralı bağlantı noktası olan yeni SSH bağlantıları bekler. Bu ayar, "hizmet-yerel proxy uygulamasını çalıştırma" bölümünde açıklandığı gibi yapılandırılabilir. Kullanıcı SSH istemcisi aracılığıyla bağlanıyorsa, tünel SSH uygulaması trafiğinin SSH istemcisi ile sunucu uygulaması arasında aktarılmasını sağlar.

> [!NOTE]
> Bir cihaz akışı üzerinden gönderilen SSH trafiği, doğrudan hizmet ve cihaz arasında gönderilmek yerine IoT Hub 'ının akış uç noktası aracılığıyla tünel oluşturulur. Daha fazla bilgi için bkz. [IoT Hub cihaz akışlarını kullanmanın avantajları](iot-hub-device-streams-overview.md#benefits).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Cihaz akışlarının önizlemesi Şu anda yalnızca şu bölgelerde oluşturulan IoT Hub 'lar için desteklenmektedir:

  * Orta ABD
  * Orta ABD EUAP

* Bu hızlı başlangıçta çalıştırdığınız iki örnek uygulama kullanılarak C#yazılmıştır. Geliştirme makinenizde .NET Core SDK 2.1.0 veya üzeri bir sürüm gerekir.

  [.NET Core SDK .net 'ten birden çok platform için](https://www.microsoft.com/net/download/all)indirebilirsiniz.

* Aşağıdaki komutu kullanarak geliştirme makinenizde C# geçerli sürümünü doğrulayın:

    ```
    dotnet --version
    ```

* Azure CLı için Azure IoT uzantısını Cloud Shell örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLı için IoT Hub, IoT Edge ve IoT cihazı sağlama hizmeti 'ne (DPS) özgü komutlar ekler.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

* [Örnek C# PROJEYI indirin](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip)ve ZIP arşivini ayıklayın.

* Kullanıcının kimliğini doğrulamak için kullanılan cihazdaki geçerli bir kullanıcı hesabı ve kimlik bilgisi (Windows veya Linux).

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta, sanal cihazı kaydetmek için Azure Cloud Shell kullanırsınız.

1. Cihaz kimliğini oluşturmak için Cloud Shell ' de aşağıdaki komutu çalıştırın:

   > [!NOTE]
   > * *Youriothubname* yer tutucusunu, IoT Hub 'ınız için seçtiğiniz adla değiştirin.
   > * Gösterilen *Mydevice*' ı kullanın. Kayıtlı cihaz için verilen addır. Cihazınız için farklı bir ad seçerseniz bu adı bu makale boyunca kullanın ve uygulamayı çalıştırmadan önce örnek uygulamalarda cihaz adını güncelleştirin.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

1. Yeni kaydettiğiniz cihazın *Cihaz bağlantı dizesini* almak için Cloud Shell ' de aşağıdaki komutları çalıştırın:

   > [!NOTE]
   > *Youriothubname* yer tutucusunu, IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Bu hızlı başlangıçta kullanılmak üzere cihaz bağlantı dizesini aklınızda yapın. Aşağıdaki örneğe benzer şekilde görünür:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

1. IoT Hub 'ınıza bağlanmak ve bir cihaz akışı oluşturmak için, hizmet tarafı uygulamasını etkinleştirmek üzere IoT Hub 'ınızdaki *hizmet bağlantı dizesine* de ihtiyacınız vardır. Aşağıdaki komut, IoT Hub 'ınız için bu değeri alır:

   > [!NOTE]
   > *Youriothubname* yer tutucusunu, IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Daha sonra bu hızlı başlangıçta kullanmak için döndürülen değeri aklınızda yapın. Aşağıdaki örneğe benzer şekilde görünür:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="ssh-to-a-device-via-device-streams"></a>Cihaz akışları aracılığıyla bir cihaza SSH

Bu bölümde, SSH trafiğini tünele bir uçtan uca akış kurarsınız.

### <a name="run-the-device-local-proxy-application"></a>Cihaz yerel proxy uygulamasını çalıştırma

Sıkıştırılmış proje klasörünüzdeki *cihaz-akışlar-proxy/cihaz* dizinine gidin. Aşağıdaki bilgileri yararlı tutun:

| Bağımsız değişken adı | Bağımsız değişken değeri |
|----------------|-----------------|
| `deviceConnectionString` | Daha önce oluşturduğunuz cihazın bağlantı dizesi. |
| `targetServiceHostName` | SSH sunucusunun dinlediği IP adresi. Bu adres `localhost` , cihaz yerel proxy uygulamasının çalıştığı aynı IP ise olur. |
| `targetServicePort` | Uygulama protokolü tarafından kullanılan bağlantı noktası (SSH için varsayılan olarak, bu bağlantı noktası 22 ' dir).  |

Kodu derleyin ve aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run $deviceConnectionString localhost 22

# In Windows
dotnet run %deviceConnectionString% localhost 22
```

### <a name="run-the-service-local-proxy-application"></a>Hizmet yerel proxy uygulamasını çalıştırma

Sıkıştırılmış proje `device-streams-proxy/service` klasörünüzde öğesine gidin. Aşağıdaki bilgiler yararlı olacaktır:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `iotHubConnectionString` | IoT Hub hizmet bağlantı dizesi. |
| `deviceId` | Daha önce oluşturduğunuz cihazın tanımlayıcısı. |
| `localPortNumber` | SSH istemcinizin bağlanacağı yerel bir bağlantı noktası. Bu örnekte 2222 numaralı bağlantı noktasını kullanıyoruz, ancak diğer rastgele sayılar kullanabilirsiniz. |

Kodu derleyin ve aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/service/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run $serviceConnectionString MyDevice 2222

# In Windows
dotnet run %serviceConnectionString% MyDevice 2222
```

### <a name="run-the-ssh-client"></a>SSH istemcisini çalıştırma

Artık SSH istemci uygulamanızı kullanın ve bağlantı noktası 2222 ' de hizmet yerel ara sunucu uygulamasına bağlanın (doğrudan SSH Daemon yerine).

```
ssh <username>@localhost -p 2222
```

Bu noktada, SSH oturum açma penceresi, kimlik bilgilerinizi girmenizi ister.

Hizmet tarafındaki konsol çıkışı (hizmet yerel proxy uygulaması 2222 numaralı bağlantı noktasını dinler):

![Hizmet-yerel proxy uygulama çıkışı](./media/quickstart-device-streams-proxy-csharp/service-console-output.png)

Cihaz yerel proxy uygulamasında, *IPI: 22*' de ssh daemon 'a bağlanan konsol çıkışı.

![Cihaz-yerel proxy uygulama çıkışı](./media/quickstart-device-streams-proxy-csharp/device-console-output.png)

SSH istemci uygulamasının konsol çıktısı. SSH istemcisi, hizmet yerel proxy uygulamasının dinlediği bağlantı noktası 22 ' ye bağlanarak SSH arka plan programı ile iletişim kurar:

![SSH istemci uygulaması çıkışı](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png)

## <a name="rdp-to-a-device-via-device-streams"></a>Cihaz akışları aracılığıyla bir cihaza RDP

RDP kurulumu, SSH kurulumuna çok benzer (yukarıda açıklanmıştır). Bunun yerine RDP hedef IP 'sini ve bağlantı noktası 3389 ' i kullanın ve RDP istemcisini kullanın (SSH istemcisi yerine).

### <a name="run-the-device-local-proxy-application-rdp"></a>Cihaz yerel proxy uygulaması (RDP) çalıştırma

Sıkıştırılmış proje klasörünüzdeki *cihaz-akışlar-proxy/cihaz* dizinine gidin. Aşağıdaki bilgileri yararlı tutun:

| Bağımsız değişken adı | Bağımsız değişken değeri |
|----------------|-----------------|
| `DeviceConnectionString` | Daha önce oluşturduğunuz cihazın bağlantı dizesi. |
| `targetServiceHostName` | RDP sunucusunun çalıştığı ana bilgisayar adı veya IP adresi. Bu adres `localhost` , cihaz yerel proxy uygulamasının çalıştığı aynı IP ise olur. |
| `targetServicePort` | Uygulama protokolü tarafından kullanılan bağlantı noktası (RDP için varsayılan olarak bu bağlantı noktası 3389 ' dir).  |

Kodu derleyin ve aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device

# Run the application
# In Linux or macOS
dotnet run $DeviceConnectionString localhost 3389

# In Windows
dotnet run %DeviceConnectionString% localhost 3389
```

### <a name="run-the-service-local-proxy-application-rdp"></a>Hizmet yerel proxy uygulamasını (RDP) çalıştırma

Sıkıştırılmış proje `device-streams-proxy/service` klasörünüzde öğesine gidin. Aşağıdaki bilgiler yararlı olacaktır:

| Parametre adı | Parametre değeri |
|----------------|-----------------|
| `iotHubConnectionString` | IoT Hub hizmet bağlantı dizesi. |
| `deviceId` | Daha önce oluşturduğunuz cihazın tanımlayıcısı. |
| `localPortNumber` | SSH istemcinizin bağlanacağı yerel bir bağlantı noktası. Bu örnekte 2222 numaralı bağlantı noktasını kullanıyoruz, ancak bunu diğer rastgele sayılarla değiştirebilirsiniz. |

Kodu derleyin ve aşağıdaki gibi çalıştırın:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/service/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run $serviceConnectionString MyDevice 2222

# In Windows
dotnet run %serviceConnectionString% MyDevice 2222
```

### <a name="run-rdp-client"></a>RDP istemcisini Çalıştır

Şimdi RDP istemci uygulamanızı kullanın ve bağlantı noktası 2222 ' de hizmet yerel ara sunucu uygulamasına bağlanın (Bu, daha önce seçtiğiniz rastgele bir bağlantı noktasıdır).

![RDP, hizmet yerel ara sunucu uygulamasına bağlanır](./media/quickstart-device-streams-proxy-csharp/rdp-screen-capture.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, IoT Hub aracılığıyla bir cihaz akışı oluşturmak için bir IoT Hub 'ı oluşturdunuz, bir cihaz kaydettiniz, cihaz yerel ve hizmet yerel proxy uygulamaları dağıtıldı ve SSH ya da RDP trafiğini tünele ara sunucu uygulamaları kullandınız. Aynı paradigma, sunucunun cihazda çalıştığı diğer istemci-sunucu protokollerine (örneğin, SSH daemon) uyum sağlayabilir.

Cihaz akışları hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Cihaz akışlarına genel bakış](./iot-hub-device-streams-overview.md)
