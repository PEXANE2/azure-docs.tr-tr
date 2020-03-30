---
title: Azure IoT Hub aygıt ikizleri (.NET/.NET) | Microsoft Dokümanlar
description: Etiketler eklemek ve ardından bir IoT Hub sorgusu kullanmak için Azure IoT Hub aygıt ikizleri nasıl kullanılır? Simüle edilen aygıt uygulamasını uygulamak için .NET için Azure IoT aygıtı SDK'yı ve .NET için Azure IoT hizmeti SDK'yı etiketleri ekleyen ve IoT Hub sorgusunu çalıştıran bir hizmet uygulamasını uygulamak için kullanabilirsiniz.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/26/2019
ms.author: robinsh
ms.openlocfilehash: 426430c075cfcb084cfe3238ebd83a19e909369b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110760"
---
# <a name="get-started-with-device-twins-net"></a>Cihaz ikizleri ile başlayın (.NET)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu öğreticide, bu .NET konsol uygulamalarını oluşturursunuz:

* **CreateDeviceIdentity**. Bu uygulama, simüle edilen aygıt uygulamanızı bağlamak için bir aygıt kimliği ve ilişkili güvenlik anahtarı oluşturur.

* **AddtagsandQuery**. Bu arka uç uygulaması etiketleri ekler ve aygıt ikizleri sorgular.

* **ReportConnectivity**. Bu cihaz uygulaması, ioT hub'ınıza daha önce oluşturulan aygıt kimliğiyle bağlanan bir aygıtı simüle eder ve bağlantı durumunu bildirir.

> [!NOTE]
> Azure [IoT SDK'lar](iot-hub-devguide-sdks.md) makalesi, hem aygıt hem de arka uç uygulamaları oluşturmak için kullanabileceğiniz Azure IoT SK'ları hakkında bilgi sağlar.
>

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio.

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>Hizmet uygulamasını oluşturma

Bu bölümde, **myDeviceId**ile ilişkili aygıt ikizine konum meta verileri ekleyen C#'ı kullanan bir .NET konsol uygulaması oluşturursunuz. Daha sonra, IoT hub'ında depolanan aygıt ikizlerini sorgulayarak ABD'de bulunan aygıtları ve hücresel bağlantıyı bildiren aygıtları seçer.

1. Visual Studio'da **yeni bir proje oluştur'u**seçin. **Yeni proje oluştur'da** **Konsol Uygulaması'nı (.NET Framework)** seçin ve **ardından İleri'yi**seçin.

1. **Yeni projeyapıla,** proje **AddTagsAndQuery**adı .

    ![AddTagsAndQuery projenizi yapılandırın](./media/iot-hub-csharp-csharp-twin-getstarted/config-addtagsandquery-app.png)

1. Solution Explorer'da **AddTagsAndQuery** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet'i**seçin.

1. **Gözat'ı** seçin ve **Microsoft.Azure.Devices'ı**arayın ve seçin. **Yükle**’yi seçin.

    ![NuGet Paket Yöneticisi penceresi](./media/iot-hub-csharp-csharp-twin-getstarted/nuget-package-addtagsandquery-app.png)

   Bu adım, [Azure IoT hizmeti SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet paketini ve bağımlılıklarını karşıdan yükler, yükler ve ekler.

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp  
    using Microsoft.Azure.Devices;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. [IoT hub bağlantı dizesini al'da](#get-the-iot-hub-connection-string)kopyaladığınız IoT Hub bağlantı dizesiyle değiştirin. `{iot hub connection string}`

    ```csharp  
    static RegistryManager registryManager;
    static string connectionString = "{iot hub connection string}";
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp  
    public static async Task AddTagsAndQuery()
    {
        var twin = await registryManager.GetTwinAsync("myDeviceId");
        var patch =
            @"{
                tags: {
                    location: {
                        region: 'US',
                        plant: 'Redmond43'
                    }
                }
            }";
        await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);

        var query = registryManager.CreateQuery(
          "SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
        var twinsInRedmond43 = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43: {0}", 
          string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));

        query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
        var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
        Console.WriteLine("Devices in Redmond43 using cellular network: {0}", 
          string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
    }
    ```

    **RegistryManager** sınıfı, hizmetten aygıt ikizleri ile etkileşim kurmak için gereken tüm yöntemleri ortaya çıkarır. Önceki kod önce **registryManager** nesnesini başolarak karşılar, sonra **myDeviceId**için aygıt ikizini alır ve son olarak etiketlerini istenen konum bilgileriyle güncelleştirir.

    Güncelleştirmeden sonra iki sorgu yürütür: ilki **Redmond43** tesisinde bulunan aygıtların yalnızca aygıt ikizlerini seçer ve ikincisi sorguyu yalnızca hücresel ağ üzerinden bağlanan aygıtları seçmek üzere geliştirir.

    Önceki kod, **sorgu** nesnesini oluşturduğunda, döndürülen en fazla sayıda belge belirtir. **Sorgu nesnesi,** tüm sonuçları almak için **GetNextAsTwinAsync** yöntemlerini birden çok kez çağırmak için kullanabileceğiniz bir **HasMoreResults** boolean özelliği içerir. **GetNextAsJson** adlı bir yöntem, aygıt ikizi olmayan sonuçlar için kullanılabilir, örneğin, toplama sorgularının sonuçları.

1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp  
    registryManager = RegistryManager.CreateFromConnectionString(connectionString);
    AddTagsAndQuery().Wait();
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

1. **AddTagsAndQuery** projesine sağ tıklayarak ve **Hata Ayıklama'yı**seçerek bu uygulamayı çalıştırın, ardından yeni örneği **başlatın.** **Redmond43'te** bulunan tüm aygıtları soran sorgunun sonuçlarında bir aygıt görmeniz ve sonuçları hücresel ağ kullanan aygıtlarla sınırlayan sorgu için hiçbir aygıt görmeniz gerekir.

    ![Sonuçları pencerede sorgula](./media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png)

Sonraki bölümde, bağlantı bilgilerini bildiren ve önceki bölümdeki sorgunun sonucunu değiştiren bir aygıt uygulaması oluşturursunuz.

## <a name="create-the-device-app"></a>Cihaz uygulamasını oluşturma

Bu bölümde, hub'ınıza **myDeviceId**olarak bağlanan ve bildirilen özelliklerini güncelleyen bir .NET konsol uygulaması oluşturursunuz ve hücresel ağ kullanarak bağlı olduğu bilgileri içerecek şekilde bildirilen özelliklerini güncelleştirirsiniz.

1. Visual Studio'da **Dosya** > **Yeni** > **Projesi'ni**seçin. **Yeni proje oluştur'da**Konsol **Uygulaması'nı (.NET Framework)** seçin ve **sonra İleri'yi**seçin.

1. **Yeni projenizi yapılandırın,** projeye **ReportConnectivity**adını verin. **Çözüm**için **çözüme ekle'yi**seçin ve ardından **Oluştur'u**seçin.

1. Çözüm Gezgini'nde **ReportConnectivity** projesini sağ tıklatın ve ardından **NuGet Paketlerini Yönet'i**seçin.

1. **Gözat'ı** seçin ve **Microsoft.Azure.Devices.Client'ı**arayın ve seçin. **Yükle**’yi seçin.

   Bu adım, [Azure IoT aygıtı SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet paketini ve bağımlılıklarını karşıdan yükler, yükler ve ekler.

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp  
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. IoT hub'ında yeni bir aygıtı kaydedin'de belirttiğiniz aygıt bağlantı dizesiyle `{device connection string}` [değiştirin.](#register-a-new-device-in-the-iot-hub)

    ```csharp  
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    public static async void InitClient()
    {
        try
        {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
              TransportType.Mqtt);
            Console.WriteLine("Retrieving twin");
            await Client.GetTwinAsync();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

    **İstemci** nesnesi, aygıttaki aygıt ikizleri ile etkileşim de olsa gereken tüm yöntemleri ortaya çıkarır. Yukarıda gösterilen kod **Istemci** nesnesini başharfe alır ve aygıt ikizini **myDeviceId**için alır.

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp  
    public static async void ReportConnectivity()
    {
        try
        {
            Console.WriteLine("Sending connectivity data as reported property");

            TwinCollection reportedProperties, connectivity;
            reportedProperties = new TwinCollection();
            connectivity = new TwinCollection();
            connectivity["type"] = "cellular";
            reportedProperties["connectivity"] = connectivity;
            await Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
    }
    ```

   Yukarıdaki kod, **myDeviceId'in** bildirilen özelliğini bağlantı bilgileriyle güncelleştirir.

1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp
    try
    {
        InitClient();
        ReportConnectivity();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

1. Çözüm Gezgini'nde, çözümünüzün üzerine sağ tıklayın ve **Başlangıç Projeleri Ayarla'yı**seçin.

1. **Ortak Özellikler** > **Başlangıç Projesi'nde,** Birden Çok başlangıç **projesi**seçin. **ReportConnectivity**için **Eylem**olarak **Başlat'ı** seçin. Değişikliklerinizi kaydetmek için **Tamam**’ı seçin.  

1. **ReportConnectivity** projesini sağ tıklayarak ve **Hata Ayıklama'yı**seçerek bu uygulamayı çalıştırın, ardından yeni bir örnek **başlatın.** Uygulamanın ikiz bilgileri aldığını ve ardından bildirilen bir ***özellik***olarak bağlantı gönderdiğini görmeniz gerekir.

    ![Bağlantıyı bildirmek için aygıt uygulamasını çalıştırma](./media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png)

   Aygıt bağlantı bilgilerini bildirdikten sonra her iki sorguda da görünmelidir.

1. **AddTagsAndQuery** projesine sağ tıklayın ve sorguları yeniden çalıştırmak için **Hata Ayıklama** > **Başlat yeni örneğini** seçin. Bu kez, **myDeviceId** her iki sorgu sonuçlarında da görünmelidir.

    ![Aygıt bağlantısı başarıyla raporedildi](./media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Aygıt meta verilerini arka uç uygulamasından etiketler olarak eklediniz ve aygıt ikizindeki aygıt bağlantı bilgilerini bildirmek için simüle edilmiş bir aygıt uygulaması yazdınız. Ayrıca, SQL benzeri IoT Hub sorgu dilini kullanarak bu bilgileri nasıl sorgulayabileceğinizi de öğrendiniz.

Aşağıdaki kaynaklardan daha fazla bilgi edinebilirsiniz:

* Cihazlardan telemetri göndermeyi öğrenmek [için, bir aygıttan IoT hub öğreticisine telemetri gönder'e](quickstart-send-telemetry-dotnet.md) bakın.

* Aygıt ikizinin istediği özellikleri kullanarak aygıtları nasıl yapılandıracağımı öğrenmek [için, aygıtlar](tutorial-device-twins.md) öğreticisini yapılandırmak için istenen özellikleri kullan'a bakın.

* Cihazları kullanıcı kontrollü bir uygulamadan bir fanı açmak gibi etkileşimli olarak nasıl kontrol edilenöğrenmek için, [doğrudan yöntem](quickstart-control-device-dotnet.md) kullan öğreticisine bakın.
