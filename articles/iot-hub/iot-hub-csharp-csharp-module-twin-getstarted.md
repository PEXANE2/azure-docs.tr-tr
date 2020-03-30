---
title: W/ Azure IoT Hub modülü kimlik & modül ikizi (.NET) başlayın
description: .NET için IoT SDK’larını kullanarak modül kimliği oluşturmayı ve modülü güncelleştirmeyi öğrenin.
author: chrissie926
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: menchi
ms.openlocfilehash: e728d0ef8f52927687d56bd1d4c64f03c53ef401
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73947678"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-net"></a>IoT Hub modülü kimliği ve modül ikizi (.NET) ile başlayın

[!INCLUDE [iot-hub-selector-module-twin-getstarted](../../includes/iot-hub-selector-module-twin-getstarted.md)]

> [!NOTE]
> [Modül kimlikleri ve modül ikizleri](iot-hub-devguide-module-twins.md), Azure IoT Hub cihaz kimliğine ve cihaz ikizine benzer, ancak daha hassas ayrıntı düzeyi sağlar. Azure IoT Hub aygıt kimliği ve aygıt ikizi, arka uç uygulamasının bir aygıtı yapılandırmasını ve aygıtın koşullarında görünürlük sağlamasını sağlarken, modül kimliği ve modül ikizi bu özellikleri bir aygıtın tek tek bileşenleri için sağlar. İşletim sistemi tabanlı aygıtlar veya firmware aygıtları gibi birden fazla bileşene sahip yetenekli cihazlarda, modül kimlikleri ve modül ikizleri her bileşen için yalıtılmış yapılandırma ve koşullarsağlar.

Bu öğreticinin sonunda iki .NET konsol uygulamanız olacak:

* **CreateIdentities**. Bu uygulama, cihazınızı ve modül istemcilerinizi bağlamak için bir aygıt kimliği, modül kimliği ve ilişkili güvenlik anahtarı oluşturur.

* **UpdateModuleTwinReportedProperties**. Bu uygulama, güncelleştirilmiş modül ikiz bildirilen özellikleri IoT hub'ınıza gönderir.

> [!NOTE]
> Hem cihazlarınızda hem de çözüm arka ucunuzda çalıştırılacak uygulamalar oluşturmak için kullanabileceğiniz Azure IoT SDK'ları hakkında bilgi için bkz. [Azure IoT SDK'ları](iot-hub-devguide-sdks.md).

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio.

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.

## <a name="create-a-hub"></a>Hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

[!INCLUDE [iot-hub-howto-module-twin-shared-access-policy-text](../../includes/iot-hub-howto-module-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

[!INCLUDE [iot-hub-get-started-create-module-identity-csharp](../../includes/iot-hub-get-started-create-module-identity-csharp.md)]

## <a name="update-the-module-twin-using-net-device-sdk"></a>.NET cihaz SDK’sını kullanarak modül ikizini güncelleştirme

Bu bölümde, modül ikizi tarafından raporlanan özelliklerini güncelleştiren simülasyon cihazınızda bir .NET konsol uygulaması oluşturursunuz.

Başlamadan önce modül bağlantı dizenizi alın. [Azure portalında](https://portal.azure.com/)oturum açın. Hub'ınıza gidin ve **IoT Aygıtlarını**seçin. **MyFirstDevice'ı**bulun. Açmak için **myFirstDevice'ı** seçin ve ardından açmak için **MyFirstModule'i** seçin. **Modül Kimlik Ayrıntıları'nda,** aşağıdaki yordamda gerektiğinde Bağlantı **dizesini (birincil anahtar)** kopyalayın.

   ![Azure portalı modül ayrıntısı](./media/iot-hub-csharp-csharp-module-twin-getstarted/module-identity-detail.png)

1. Visual Studio'da, **Dosya** > **Yeni** > **Projesi'ni**seçerek çözümünüze yeni bir proje ekleyin. Yeni bir proje oluştur'da **Konsol Uygulaması'nı (.NET Framework)** seçin ve **İleri'yi**seçin.

1. Projeye *UpdateModuleTwinReportedProperties* adını verin. **Çözüm**için , **çözüme ekle'yi**seçin. .NET Framework sürümünün 4.6.1 veya sonraki bir sürüm olduğundan emin olun.

    ![Visual Studio projesi oluşturma](./media/iot-hub-csharp-csharp-module-twin-getstarted/configure-update-twins-csharp1.png)

1. Projenizi oluşturmak için **Oluştur'u** seçin.

1. Visual Studio'da, Open **Tools** > **NuGet Package Manager** > **Çözüm için NuGet Paketlerini Yönetin.** **Gözat** sekmesini seçin.

1. **Microsoft.Azure.Devices.Client'ı**arayın ve seçin ve ardından **Yükle'yi**seçin.

    ![Azure IoT Hub .NET hizmeti SDK geçerli sürümünü yükleme](./media/iot-hub-csharp-csharp-module-twin-getstarted/install-client-sdk.png)

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using System.Threading.Tasks;
    using Newtonsoft.Json;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, modül bağlantı dizesiyle değiştirin.

    ```csharp
    private const string ModuleConnectionString = "<Your module connection string>";
    private static ModuleClient Client = null;
    static void ConnectionStatusChangeHandler(ConnectionStatus status, 
      ConnectionStatusChangeReason reason)
    {
        Console.WriteLine("Connection Status Changed to {0}; the reason is {1}", 
          status, reason);
    }
    ```

1. Aşağıdaki **OnDesiredPropertyChanged** yöntemini **Program** sınıfına ekleyin:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, 
      object userContext)
        {
            Console.WriteLine("desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
            Console.WriteLine("Sending current time as reported property");
            TwinCollection reportedProperties = new TwinCollection
            {
                ["DateTimeLastDesiredPropertyChangeReceived"] = DateTime.Now
            };

            await Client.UpdateReportedPropertiesAsync(reportedProperties).ConfigureAwait(false);
        }
    ```

1. **Ana** yönteme aşağıdaki satırları ekleyin:

    ```csharp
    static void Main(string[] args)
    {
        Microsoft.Azure.Devices.Client.TransportType transport = 
          Microsoft.Azure.Devices.Client.TransportType.Amqp;

        try
        {
            Client = 
              ModuleClient.CreateFromConnectionString(ModuleConnectionString, transport);
            Client.SetConnectionStatusChangesHandler(ConnectionStatusChangeHandler);
            Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();

            Console.WriteLine("Retrieving twin");
            var twinTask = Client.GetTwinAsync();
            twinTask.Wait();
            var twin = twinTask.Result;
            Console.WriteLine(JsonConvert.SerializeObject(twin.Properties)); 

            Console.WriteLine("Sending app start time as reported property");
            TwinCollection reportedProperties = new TwinCollection();
            reportedProperties["DateTimeLastAppLaunch"] = DateTime.Now;

            Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (AggregateException ex)
        {
            Console.WriteLine("Error in sample: {0}", ex);
        }

        Console.WriteLine("Waiting for Events.  Press enter to exit...");
        Console.ReadLine();
        Client.CloseAsync().Wait();
    }
    ```

    Bu kod örneği, AMQP protokolüyle raporlanan özellikleri güncelleştirme ve modül ikizini alma işlemini nasıl yapacağınızı gösterir. Genel önizleme aşamasında, modül ikizi işlemleri için yalnızca AMQP’yi destekleriz.

1. İsteğe bağlı olarak, modülünüzden IoT Hub'a bir olay göndermek için bu ifadeleri **Ana** yönteme ekleyebilirsiniz. Bu satırları `try catch` bloğun altına yerleştirin.

    ```csharp
    Byte[] bytes = new Byte[2];
    bytes[0] = 0;
    bytes[1] = 1;
    var sendEventsTask = Client.SendEventAsync(new Message(bytes));
    sendEventsTask.Wait();
    Console.WriteLine("Event sent to IoT Hub.");
    ```

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Artık uygulamaları çalıştırabilirsiniz.

1. Visual Studio'da, **Solution Explorer'da,** çözümünüzü sağ tıklatın ve ardından Başlangıç **Projeleri'ni ayarlayın'ı**seçin.

1. **Ortak Özellikler**altında Başlangıç Projesi'ni **seçin.**

1. **Birden Çok başlangıç projesi**seçin ve ardından uygulamalar için eylem olarak **Başlat'ı** ve değişikliklerinizi kabul etmek için **Tamam'ı** seçin.

1. Uygulamaları başlatmak için **F5** tuşuna basın.

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [Cihaz yönetimini kullanmaya başlama](iot-hub-node-node-device-management-get-started.md)

* [IoT Edge ile çalışmaya başlama](../iot-edge/tutorial-simulate-device-linux.md)
