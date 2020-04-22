---
title: .NET ile cihazlardan Azure IoT Hub'ına dosya yükleme | Microsoft Dokümanlar
description: .NET için Azure IoT aygıtı SDK'yı kullanarak bir cihazdan buluta dosya yükleme. Yüklenen dosyalar bir Azure depolama blob kapsayıcısında depolanır.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 07/04/2017
ms.author: robinsh
ms.custom: mqtt
ms.openlocfilehash: 14472e84d425bf03a3c6a0c2dc558d4b8225caec
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733408"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-net"></a>IoT Hub (.NET) ile cihazınızdan buluta dosya yükleme

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Bu öğretici, IoT Hub'ın dosya yükleme yeteneklerini nasıl kullanacağınızı göstermek için [IoT Hub öğreticisiyle buluttan cihaza ileti gönder'deki](iot-hub-csharp-csharp-c2d.md) koda dayalıdır. Nasıl yapılacağını gösterir:

* Bir dosyayı yüklemek için güvenli bir şekilde Azure blob URI içeren bir aygıt sağlayın.

* Uygulamanızın arka ucundaki dosyanın işlenmesini tetiklemek için IoT Hub dosya yükleme bildirimlerini kullanın.

Bir [aygıttan IoT hub'ına](quickstart-send-telemetry-dotnet.md) hızlı başlatma ve [IoT Hub öğreticisiyle buluttan aygıta ileti gönder,IoT](iot-hub-csharp-csharp-c2d.md) Hub'ın temel aygıttan buluta ve buluttan cihaza mesajlaşma işlevselliğini gösterir. [IoT Hub öğreticisiyle Yapılandırılan İleti](tutorial-routing.md) Yönlendirmesi, Aygıttan buluta iletileri Microsoft Azure Blob depolama alanında güvenilir bir şekilde depolamanın bir yolunu açıklar. Ancak, bazı senaryolarda, aygıtlarınızın gönderdiği verileri IoT Hub'ın kabul ettiği nispeten küçük aygıttan buluta iletilerle kolayca eşleyebilirsiniz. Örneğin:

* Resim içeren büyük dosyalar

* Videolar

* Yüksek frekansta örneklenmiş titreşim verileri

* Önceden işlenmiş bir veri biçimi

Bu dosyalar genellikle [Azure Veri Fabrikası](../data-factory/introduction.md) veya [Hadoop](../hdinsight/index.yml) yığını gibi araçlar kullanılarak bulutta toplu olarak işlenir. Bir aygıttan dosya yüklemeniz gerektiğinde, IoT Hub'ın güvenliğini ve güvenilirliğini kullanmaya devam edebilirsiniz.

Bu eğitimin sonunda iki .NET konsol uygulaması çalıştırın:

* **Simüle Cihaz**. Bu uygulama, IoT hub'ınız tarafından sağlanan bir SAS URI kullanarak bir dosyayı depolama alanına yükler. [IoT Hub öğreticisi ile buluttan cihaza ileti gönder iletilerinde](iot-hub-csharp-csharp-c2d.md) oluşturulan uygulamanın değiştirilmiş bir sürümüdür.

* **ReadFileUploadNotification**. Bu uygulama, IoT hub'ınızdan dosya yükleme bildirimleri alır.

> [!NOTE]
> IoT Hub, Azure IoT aygıt SDK'ları aracılığıyla C, Java, Python ve Javascript gibi birçok aygıt platformunu ve dili destekler. Cihazınızı Azure IoT Hub'ına nasıl bağlayacaklarınız hakkında adım adım talimatlar için [Azure IoT Geliştirici Merkezi'ne](https://azure.microsoft.com/develop/iot) bakın.

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Aygıt uygulamasından dosya yükleme

Bu bölümde, [IoT Hub ile buluttan cihaza ileti gönder'de](iot-hub-csharp-csharp-c2d.md) oluşturduğunuz aygıt uygulamasını değiştirerek IoT hub'ından buluttan cihaza iletiler alırsınız.

1. Visual Studio Solution Explorer'da, **Benzetimli Aygıt** projesini sağ tıklatın ve**Varolan Öğeyi** **Ekle'yi** > seçin. Bir resim dosyası bulun ve projenize ekleyin. Bu öğretici, görüntünün `image.jpg`adının .

1. Resme sağ tıklayın ve ardından **Özellikler'i**seçin. **Çıktı Dizinine** Kopyala'nın **her zaman kopyala**olarak ayarlandıklarına emin olun.

    ![Çıktı Dizinine Kopyalama için görüntü özelliğinin nerede güncelleştirileni göster](./media/iot-hub-csharp-csharp-file-upload/image-properties.png)

1. **Program.cs** dosyasında, dosyanın üst kısmında aşağıdaki ifadeleri ekleyin:

    ```csharp
    using System.IO;
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    private static async void SendToBlobAsync()
    {
        string fileName = "image.jpg";
        Console.WriteLine("Uploading file: {0}", fileName);
        var watch = System.Diagnostics.Stopwatch.StartNew();

        using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
        {
            await deviceClient.UploadToBlobAsync(fileName, sourceData);
        }

        watch.Stop();
        Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    Yöntem, `UploadToBlobAsync` yüklenecek dosyanın dosya adını ve akış kaynağını alır ve yüklemeyi depolamaya işler. Konsol uygulaması dosyayı yüklemek için gereken süreyi görüntüler.

1. **Ana** yöntemde aşağıdaki satırı ekleyin, `Console.ReadLine()`hemen önce:

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> Basitlik aşkına, bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [Geçici hata işlemede](/azure/architecture/best-practices/transient-faults)önerildiği gibi üstel geri tepme gibi yeniden deneme ilkeleri uygulamanız gerekir.

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

Bu makalede, [bir aygıttan bir IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-dotnet.md)oluşturduğunuz IoT hub'ından dosya yükleme bildirim iletileri almak için bir arka uç hizmeti oluşturursunuz. Dosya yükleme bildirim iletileri almak için **hizmetinizin bağlanma** iznine ihtiyacı vardır. Varsayılan olarak, her IoT Hub'ı bu izni veren paylaşılan erişim ilkesi adlı **hizmetle** oluşturulur.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="receive-a-file-upload-notification"></a>Dosya yükleme bildirimi alma

Bu bölümde, IoT Hub'dan dosya yükleme bildirim iletileri alan bir .NET konsol uygulaması yazarsınız.

1. Geçerli Visual Studio çözümünde, **Dosya** > **Yeni** > **Proje'yi**seçin. **Yeni bir proje oluştur'da**Konsol **Uygulaması'nı (.NET Framework)** seçin ve ardından **İleri'yi**seçin.

1. Proje *readFileUploadNotification*adı . **Çözüm**altında, **çözüme ekle'yi**seçin. Projeyi oluşturmak için **Oluştur**'u seçin.

    ![Visual Studio'da ReadFileUploadNotification projesini yapılandırın](./media/iot-hub-csharp-csharp-file-upload/read-file-upload-project-configure.png)

1. Solution Explorer'da **ReadFileUploadNotification** projesine sağ tıklayın ve **NuGet Paketlerini Yönet'i**seçin.

1. **NuGet Paket Yöneticisi'nde** **Gözat'ı**seçin. **Microsoft.Azure.Devices'ı**arayın ve seçin ve ardından **Yükle'yi**seçin.

    Bu adım, **ReadFileUploadNotification** projesindeki [Azure IoT hizmeti SDK NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Devices/) indirir, yükler ve ekler.

1. Bu projenin **Program.cs** dosyasına, dosyanın üst kısmında aşağıdaki deyimi ekleyin:

    ```csharp
    using Microsoft.Azure.Devices;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. `{iot hub connection string}` Yer tutucu değerini, IoT hub bağlantı dizesini al'da daha önce kopyaladığınız IoT hub bağlantı [dizesiyle değiştirin:](#get-the-iot-hub-connection-string)

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    private async static void ReceiveFileUploadNotificationAsync()
    {
        var notificationReceiver = serviceClient.GetFileNotificationReceiver();

        Console.WriteLine("\nReceiving file upload notification from service");
        while (true)
        {
            var fileUploadNotification = await notificationReceiver.ReceiveAsync();
            if (fileUploadNotification == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received file upload notification: {0}", 
              string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }
    ```

    Bu alma deseninin, aygıt uygulamasından buluttan cihaza ileti almak için kullanılanmodelle aynı olduğunu unutmayın.

1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter to exit\n");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Solutions Explorer'da, çözümünüzü sağ tıklatın ve **Başlangıç Projeleri Ayarla'yı**seçin.

1. **Ortak Özellikler** > **Başlangıç Projesi'nde,** Birden Çok başlangıç **projesi**seçin, ardından **ReadFileUploadNotification** ve **SimulatedDevice**için **Başlat** eylemini seçin. Değişikliklerinizi kaydetmek için **Tamam**’ı seçin.

1. **F5 tuşuna**basın. Her iki uygulama başlamalıdır. Yüklemenin bir konsol uygulamasında tamamlandığını ve diğer konsol uygulaması tarafından alınan yükleme bildirim iletisini görmeniz gerekir. Yüklenen dosyanın Azure Depolama hesabınızda varlığını kontrol etmek için [Azure portalını](https://portal.azure.com/) veya Visual Studio Server Explorer'ı kullanabilirsiniz.

    ![Çıktı ekranını gösteren ekran görüntüsü](./media/iot-hub-csharp-csharp-file-upload/run-apps1.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, cihazlardan gelen dosya yüklemelerini basitleştirmek için IoT Hub'ın dosya yükleme özelliklerini nasıl kullanacağınızı öğrendiniz. Aşağıdaki makalelerle IoT Hub özelliklerini ve senaryolarını keşfetmeye devam edebilirsiniz:

* [Programlı bir IoT hub'ı oluşturma](iot-hub-rm-template-powershell.md)

* [C SDK'ya Giriş](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK’ları](iot-hub-devguide-sdks.md)

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
