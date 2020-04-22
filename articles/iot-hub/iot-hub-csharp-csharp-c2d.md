---
title: Azure IoT Hub (.NET) ile buluttan cihaza iletiler | Microsoft Dokümanlar
description: .NET için Azure IoT SDK'larını kullanarak bir Azure IoT hub'ından bir aygıta buluttan aygıta ileti gönderme. Bir aygıt uygulamasını buluttan cihaza iletiler almak için değiştirirsiniz ve bir arka uç uygulamasını değiştirerek buluttan cihaza iletiler gönderirsiniz.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: robinsh
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 41c29e55f04f9edf06ba375ad4539e5fb3f82c18
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733428"
---
# <a name="send-messages-from-the-cloud-to-your-device-with-iot-hub-net"></a>IoT Hub (.NET) ile buluttan cihazınıza ileti gönderme

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IoT Hub, milyonlarca aygıt ve bir çözüm arka uç arasında güvenilir ve güvenli çift yönlü iletişim sağlamaya yardımcı olan tam olarak yönetilen bir hizmettir. Bir [aygıttan IoT hub'ına hızlı](quickstart-send-telemetry-dotnet.md) bir şekilde gönder telemetrisi, bir IoT hub'ı oluşturmanın, içinde aygıt kimliğini nasıl sağlayabilirsiniz ve aygıttan buluta iletigönderen bir aygıt uygulamasını nasıl kodlar.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici, [bir aygıttan IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-dotnet.md)biryapılsa. Aşağıdaki görevleri nasıl yapacağınızı gösterir:

* Çözümünüzden arka uçtan, IoT Hub üzerinden tek bir cihaza buluttan cihaza iletigönderin.

* Bir aygıttan buluttan aygıta iletiler alın.

* Çözümünüzden arka uçtan, IoT Hub'dan bir cihaza gönderilen iletiler için teslimat bildirimi *(geri bildirim)* isteyin.

[IoT Hub ile D2C ve C2D Mesajlaşma'da](iot-hub-devguide-messaging.md)buluttan cihaza iletiler hakkında daha fazla bilgi bulabilirsiniz.

Bu eğitimin sonunda iki .NET konsol uygulaması çalıştırın.

* **Simüle Cihaz**. Bu uygulama IoT hub'ınıza bağlanır ve buluttan cihaza iletiler alır. Bu uygulama, [bir cihazdan IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-dotnet.md)oluşturulan uygulamanın değiştirilmiş bir sürümüdür.

* **SendCloudtoDevice**. Bu uygulama, IoT Hub üzerinden cihaz uygulamasına buluttan cihaza bir ileti gönderir ve ardından teslim bildirimini alır.

> [!NOTE]
> IoT Hub, [Azure IoT aygıt SDK'ları](iot-hub-devguide-sdks.md)aracılığıyla C, Java, Python ve Javascript gibi birçok aygıt platformu ve dili için SDK desteğine sahiptir. Cihazınızı bu öğreticinin koduna ve genellikle Azure IoT Hub'ına nasıl bağlayacaklarına ilişkin adım adım talimatlar için [IoT Hub geliştirici kılavuzuna](iot-hub-devguide.md)bakın.
>

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="receive-messages-in-the-device-app"></a>Cihaz uygulamasından ileti alma

Bu bölümde, IoT hub'ından buluttan aygıta iletiler almak için telemetri gönder'de oluşturduğunuz aygıt uygulamasını [bir aygıttan IoT](quickstart-send-telemetry-dotnet.md) hub'ına değiştirin.

1. Visual Studio'da, **Simüle Cihaz** projesinde, **Program** sınıfına aşağıdaki yöntemi ekleyin.

   ```csharp
    private static async void ReceiveC2dAsync()
    {
        Console.WriteLine("\nReceiving cloud to device messages from service");
        while (true)
        {
            Message receivedMessage = await s_deviceClient.ReceiveAsync();
            if (receivedMessage == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received message: {0}", 
            Encoding.ASCII.GetString(receivedMessage.GetBytes()));
            Console.ResetColor();

            await s_deviceClient.CompleteAsync(receivedMessage);
        }
    }
   ```

1. **Satırdan** hemen önce `Console.ReadLine()` Ana yönteme aşağıdaki yöntemi ekleyin:

   ```csharp
   ReceiveC2dAsync();
   ```

Yöntem, `ReceiveAsync` alınan iletiyi aygıt tarafından alındığı anda eş zamanlı olarak döndürür. Belirli bir zaman adedinden sonra *null* döndürür. Bu örnekte, bir dakika varsayılan kullanılır. Uygulama *null*aldığında, yeni iletileri beklemeye devam etmelidir. Bu gereksinim `if (receivedMessage == null) continue` satırın nedenidir.

IoT `CompleteAsync()` Hub'a iletinin başarıyla işlendiğini belirten çağrı. İleti aygıt kuyruğundan güvenli bir şekilde kaldırılabilir. Aygıt uygulamasının iletinin işlenmesini tamamlamasını engelleyen bir şey olduysa, IoT Hub bu uygulamayı yeniden teslim eder. Aygıt uygulamasındaki ileti işleme mantığı *idempotent*olmalıdır, böylece aynı iletiyi birden çok kez almak aynı sonucu üretir.

Bir uygulama, ioT hub'ın gelecekteki tüketim için kuyruktaki iletiyi tutmasıyla sonuçlanan bir iletiyi geçici olarak terk edebilir. Veya uygulama, iletiyi kuyruktan kalıcı olarak kaldıran bir iletiyi reddedebilir. Buluttan cihaza ileti yaşam döngüsü hakkında daha fazla bilgi [için, IoT Hub ile D2C ve C2D mesajlaşmasına](iot-hub-devguide-messaging.md)bakın.

   > [!NOTE]
   > Aktarım olarak MQTT veya AMQP yerine `ReceiveAsync` HTTPS kullanırken, yöntem hemen döner. HTTPS ile buluttan aygıta iletiler için desteklenen desen, iletileri seyrek olarak kontrol eden aralıklı olarak bağlanan aygıtlardır (her 25 dakikadan az). Daha fazla HTTPS verilmesi, IoT Hub'ın istekleri azaltması ile sonuç alır. MQTT, AMQP ve HTTPS desteği ve IoT Hub azaltma arasındaki farklar hakkında daha fazla bilgi [için, IoT Hub ile D2C ve C2D mesajlaşma](iot-hub-devguide-messaging.md)bölümüne bakın.
   >

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

Bu makalede, bir aygıttan bir [IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-dotnet.md)oluşturduğunuz IoT hub'ı üzerinden buluttan aygıta iletigöndermek için bir arka uç hizmeti oluşturursunuz. Buluttan cihaza ileti göndermek için hizmetinizin **hizmet bağlantısı** namına ihtiyacı vardır. Varsayılan olarak, her IoT Hub'ı bu izni veren paylaşılan erişim ilkesi adlı **hizmetle** oluşturulur.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="send-a-cloud-to-device-message"></a>Buluttan aygıta ileti gönderme

Şimdi cihaz uygulamasına buluttan cihaza iletigönderen bir .NET konsol uygulaması yazın.

1. Geçerli Visual Studio çözümünde, **Dosya** > **Yeni** > **Proje'yi**seçin. **Yeni bir proje oluştur'da**Konsol **Uygulaması'nı (.NET Framework)** seçin ve ardından **İleri'yi**seçin.

1. Proje *sendCloudToDevice*adı . **Çözüm**altında, **çözüme ekle'yi** seçin ve .NET Framework'ün en son sürümünü kabul edin. Projeyi oluşturmak için **Oluştur**'u seçin.

   ![Visual Studio'da yeni bir proje yapılandırma](./media/iot-hub-csharp-csharp-c2d/sendcloudtodevice-project-configure.png)

1. Çözüm Gezgini'nde yeni çözüme sağ tıklayın ve ardından **NuGet Paketlerini Yönet'i**seçin.

1. **NuGet Paketlerini Yönet'te** **Gözat'ı**seçin ve ardından **Microsoft.Azure.Devices'ı**arayın ve seçin. **Yükle'yi**seçin.

   Bu adım, [Azure IoT hizmeti SDK NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Devices/)indirir, yükler ve ekler.

1. `using` **Program.cs** dosyasının üst kısmında aşağıdaki deyimi ekleyin.

   ``` csharp
   using Microsoft.Azure.Devices;
   ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu [değerini, IoT hub bağlantı dizesini al'da](#get-the-iot-hub-connection-string)daha önce kopyaladığınız IoT hub bağlantı dizesiyle değiştirin.

   ``` csharp
   static ServiceClient serviceClient;
   static string connectionString = "{iot hub connection string}";
   ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin. Aygıtı bir [aygıttan IoT hub'ına gönder telemetrisinde](quickstart-send-telemetry-dotnet.md)aygıtı tanımlarken aygıt adını kullandığınız alete ayarlayın.

   ``` csharp
   private async static Task SendCloudToDeviceMessageAsync()
   {
        var commandMessage = new
         Message(Encoding.ASCII.GetBytes("Cloud to device message."));
        await serviceClient.SendAsync("myFirstDevice", commandMessage);
   }
   ```

   Bu yöntem, aygıta kimlikle yeni bir bulut-aygıt `myFirstDevice`iletisi gönderir. Bu parametreyi yalnızca [bir aygıttan ioT hub'ına telemetri gönder'de kullanılan parametreden](quickstart-send-telemetry-dotnet.md)değiştirdiyseniz değiştirin.

1. Son olarak, **Ana** yönteme aşağıdaki satırları ekleyin.

   ``` csharp
   Console.WriteLine("Send Cloud-to-Device message\n");
   serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

   Console.WriteLine("Press any key to send a C2D message.");
   Console.ReadLine();
   SendCloudToDeviceMessageAsync().Wait();
   Console.ReadLine();
   ```

1. Solutions Explorer'da, çözümünüzü sağ tıklatın ve **Başlangıç Projeleri Ayarla'yı**seçin.

1. **Ortak Özellikler** > **Başlangıç Projesi'nde,** Birden Çok başlangıç **projesi**seçin, ardından **ReadDeviceToCloudMessages**, **SimulatedDevice**ve **SendCloudToDevice**için **Başlat** eylemini seçin. Değişikliklerinizi kaydetmek için **Tamam**’ı seçin.

1. **F5 tuşuna**basın. Her üç uygulama başlamalıdır. **SendCloudToDevice** pencerelerini seçin ve **Enter**tuşuna basın. Cihaz uygulaması tarafından alınan iletiyi görmeniz gerekir.

   ![Uygulama alma iletisi](./media/iot-hub-csharp-csharp-c2d/sendc2d1.png)

## <a name="receive-delivery-feedback"></a>Teslimat geri bildirimi alma

Her buluttan cihaza ileti için IoT Hub'dan teslim (veya son kullanma) bildirimleri talep etmek mümkündür. Bu seçenek, yeniden deneme veya tazminat mantığını kolayca bilgilendirmek için çözümün arka ucunun kolayca bilgilendirilmesini sağlar. Buluttan cihaza geri bildirim hakkında daha fazla bilgi [için, IoT Hub ile D2C ve C2D Mesajlaşma'ya](iot-hub-devguide-messaging.md)bakın.

Bu bölümde, **SendCloudToDevice** uygulamasını geri bildirim istemek ve IoT hub'ından almak için değiştirirsiniz.

1. Visual Studio'da, **SendCloudToDevice** projesinde **Program** sınıfına aşağıdaki yöntemi ekleyin.

   ```csharp
   private async static void ReceiveFeedbackAsync()
   {
        var feedbackReceiver = serviceClient.GetFeedbackReceiver();

        Console.WriteLine("\nReceiving c2d feedback from service");
        while (true)
        {
            var feedbackBatch = await feedbackReceiver.ReceiveAsync();
            if (feedbackBatch == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received feedback: {0}",
              string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
            Console.ResetColor();

            await feedbackReceiver.CompleteAsync(feedbackBatch);
        }
    }
    ```

    Bu alma deseninin, aygıt uygulamasından buluttan cihaza ileti almak için kullanılanmodelle aynı olduğunu unutmayın.

1. **Ana** yöntemde aşağıdaki satırı ekleyin, `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)`hemen sonra .

   ``` csharp
   ReceiveFeedbackAsync();
   ```

1. Buluttan cihaza iletinizin teslimi için geri bildirim istemek için **SendCloudToDeviceMessageAsync** yönteminde bir özellik belirtmeniz gerekir. `var commandMessage = new Message(...);` Satırdan hemen sonra aşağıdaki satırı ekleyin.

   ``` csharp
   commandMessage.Ack = DeliveryAcknowledgement.Full;
   ```

1. **F5**tuşuna basarak uygulamaları çalıştırın. Üç uygulamanın da başladığını görmelisiniz. **SendCloudToDevice** pencerelerini seçin ve **Enter**tuşuna basın. Aygıt uygulaması tarafından alınan iletiyi ve birkaç saniye sonra **SendCloudToDevice** uygulamanız tarafından alınan geri bildirim iletisini görmeniz gerekir.

   ![Uygulama alma iletisi](./media/iot-hub-csharp-csharp-c2d/sendc2d2.png)

> [!NOTE]
> Basitlik için, bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [Geçici hata işlemede](/azure/architecture/best-practices/transient-faults)önerildiği gibi üstel geri tepme gibi yeniden deneme ilkeleri uygulamanız gerekir.
>

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılsa da, buluttan cihaza ileti göndermeyi ve almayı öğrendiniz.

IoT Hub'ı kullanan eksiksiz uçtan uca çözümlerin örneklerini görmek için [Azure IoT Uzaktan İzleme çözüm hızlandırıcısına](https://docs.microsoft.com/azure/iot-suite/)bakın.

IoT Hub ile çözüm geliştirme hakkında daha fazla bilgi edinmek için [IoT Hub geliştirici kılavuzuna](iot-hub-devguide.md)bakın.
