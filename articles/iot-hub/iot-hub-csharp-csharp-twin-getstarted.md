---
title: Azure IoT Hub cihaz TWINS (.NET/.NET) ile çalışmaya başlama | Microsoft Docs
description: Azure IoT Hub cihaz ikimlerini kullanarak etiketler ekleyin ve ardından bir IoT Hub sorgusu kullanın. .NET için Azure IoT cihaz SDK 'sını kullanarak sanal cihaz uygulamasını ve Etiketler ekleyen ve IoT Hub sorguyu çalıştıran bir hizmet uygulaması uygulamak üzere .NET için Azure IoT hizmeti SDK 'sını kullanın.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: robinsh
ms.openlocfilehash: 9bf34fd48c3a4a9a9672ac162f63dcce118b2c0a
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668180"
---
# <a name="get-started-with-device-twins-net"></a>Cihaz ikizlerini kullanmaya başlama (.NET)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu öğreticinin sonunda şu .NET konsol uygulamalarına sahip olursunuz:

* Bir cihaz kimliği ve sanal cihaz uygulamanızı bağlamak için ilişkili güvenlik anahtarı oluşturan bir .NET uygulaması olan **Createdeviceıdentity**.

* Etiket ve sorgu cihaz TWINS 'i ekleyen bir .NET arka uç uygulaması olan **Addtagsandquery**.

* Daha önce oluşturulan cihaz kimliğiyle IoT Hub 'ınıza bağlanan bir cihazın benzetimini yapan ve bağlantı koşulunu raporlayan bir .NET cihaz uygulaması olan **Reportconnectivity**.

> [!NOTE]
> [Azure IoT SDK 'ları](iot-hub-devguide-sdks.md) makalesi, hem cihaz hem de arka uç uygulamaları oluşturmak Için kullanabileceğiniz Azure IoT SDK 'ları hakkında bilgi sağlar.
> 

Bu öğreticiyi tamamlayabilmeniz için şunlar gerekir:

* Visual Studio.
* Etkin bir Azure hesabı. (Hesabınız yoksa yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>IoT Hub 'a yeni bir cihaz kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT Hub bağlantı dizesini al

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>Hizmet uygulaması oluşturma

Bu bölümde, **Mydeviceıd**ile ilişkili cihaz ikizi konum meta verilerini C#ekleyen bir .NET konsol uygulaması (kullanarak) oluşturursunuz. Ardından, IoT Hub 'ında depolanan cihaz TWINS 'i, ABD 'de bulunan cihazları ve ardından hücresel bağlantı bildirdikleri sorgular.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Projeyi **Addtagsandquery**olarak adlandırın.
   
    ![Yeni Visual C# Windows Klasik Masaüstü projesi](./media/iot-hub-csharp-csharp-twin-getstarted/createnetapp.png)

2. Çözüm Gezgini, **Addtagsandquery** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet...** öğesine tıklayın.

3. **NuGet Paket Yöneticisi** penceresinde, **Araştır** ' ı seçin ve **Microsoft. Azure. Devices**' i arayın. **Microsoft. Azure. Devices** paketini yüklemek için **yükler** ' i seçin ve kullanım koşullarını kabul edin. Bu yordam, [Azure IoT hizmeti SDK 'sı](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet paketi ve bağımlılıklarını indirir, yükler ve buna bir başvuru ekler.
   
    ![NuGet Paket Yöneticisi penceresi](./media/iot-hub-csharp-csharp-twin-getstarted/servicesdknuget.png)

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp  
    using Microsoft.Azure.Devices;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, [IoT Hub bağlantı dizesini alırken](#get-the-iot-hub-connection-string)daha önce kopyaladığınız IoT Hub bağlantı dizesiyle değiştirin.

    ```csharp  
    static RegistryManager registryManager;
    static string connectionString = "{iot hub connection string}";
    ```

6. **Program** sınıfına aşağıdaki yöntemi ekleyin:

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
   
    **Registrymanager** sınıfı, hizmetten cihaz ikikiyle etkileşimde bulunmak için gereken tüm yöntemleri kullanıma sunar. Önceki kod, ilk olarak **Registrymanager** nesnesini başlatır ve ardından **mydeviceıd**için Device ikizi alır ve son olarak etiketlerini istenen konum bilgileriyle güncelleştirir.
   
    Güncelleştirme sonrasında, iki sorgu yürütülür: İlki yalnızca **Redmond43** tesisinde bulunan cihazların cihaz ikelerini seçer ve ikincisi ise yalnızca hücresel ağ üzerinden de bağlı olan cihazları seçecek şekilde sorguyu iyileştirir.
   
    Önceki kodun **sorgu** nesnesini oluşturduğunda, en fazla döndürülen belge sayısını belirtir. **Sorgu** nesnesi, tüm sonuçları almak Için **Getnextastwınasync** yöntemlerini birden çok kez çağırmak için kullanabileceğiniz bir **HasMoreResults** Boolean özelliği içerir. **Getnextasjson** adlı bir yöntem, Device-wins olmayan sonuçlar (örneğin, toplama sorgularının sonuçları) için kullanılabilir.

7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp  
    registryManager = RegistryManager.CreateFromConnectionString(connectionString);
    AddTagsAndQuery().Wait();
    Console.WriteLine("Press Enter to exit.");
    Console.ReadLine();
    ```

8. Çözüm Gezgini, **Başlangıç projelerini ayarla...** ' yı açın ve **Addtagsandquery** projesi **eyleminin** **başladı**olduğundan emin olun. Çözümü oluşturun.

9. **Addtagsandquery** projesine sağ tıklayıp **Hata Ayıkla**' yı ve ardından **Yeni örnek Başlat**' ı seçerek bu uygulamayı çalıştırın. **Redmond43** ' de bulunan tüm cihazları isteyen sorgunun sonuçlarında bir cihaz görmeniz gerekir ve sonuçları hücresel ağ kullanan cihazlara kısıtlayan sorgu için yok.
   
    ![Penceredeki sorgu sonuçları](./media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png)

Sonraki bölümde, bağlantı bilgilerini raporlayan ve önceki bölümde sorgunun sonucunu değiştiren bir cihaz uygulaması oluşturacaksınız.

## <a name="create-the-device-app"></a>Cihaz uygulaması oluşturma

Bu bölümde, hub 'ınıza **Mydeviceıd**olarak bağlanan bir .NET konsol uygulaması oluşturun ve ardından, bildirilen özelliklerini bir hücresel ağ kullanılarak bağlı olan bilgileri içerecek şekilde günceller.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Projeyi **Reportconnectivity**olarak adlandırın.
   
    ![Yeni Visual C# Windows Klasik cihaz uygulaması](./media/iot-hub-csharp-csharp-twin-getstarted/createdeviceapp.png)
    
2. Çözüm Gezgini, **Reportconnectivity** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet...** öğesine tıklayın.

3. **NuGet Paket Yöneticisi** penceresinde, **Araştır** ' ı seçin ve **Microsoft. Azure. Devices. Client**için arama yapın. **Microsoft. Azure. Devices. Client** paketini yüklemek için **Install** ' ı seçin ve kullanım koşullarını kabul edin. Bu yordam, [Azure IoT cihaz SDK 'sı](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet paketi ve bağımlılıklarını indirir, yükler ve buna bir başvuru ekler.
   
    ![NuGet Paket Yöneticisi penceresi Istemci uygulaması](./media/iot-hub-csharp-csharp-twin-getstarted/clientsdknuget.png)

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp  
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, önceki bölümde not ettiğiniz cihaz bağlantı dizesiyle değiştirin.

    ```csharp  
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;
      DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

6. **Program** sınıfına aşağıdaki yöntemi ekleyin:

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

    **İstemci** nesnesi, cihazdan cihaz ikikiyle etkileşimde bulunmak için gereken tüm yöntemleri kullanıma sunar. Yukarıda gösterilen kod, **istemci** nesnesini başlatır ve ardından **mydeviceıd**için cihaz ikizi alır.

7. **Program** sınıfına aşağıdaki yöntemi ekleyin:

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

   Yukarıdaki kod, **Mydeviceıd**'nin bildirilen özelliğini bağlantı bilgileriyle güncelleştirir.

8. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

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

9. Çözüm Gezgini, **Başlangıç projelerini ayarla...** ' yı açın ve **Reportconnectivity** projesi **eyleminin** **başladı**olduğundan emin olun. Çözümü oluşturun.

10. **Reportconnectivity** projesine sağ tıklayıp **Hata Ayıkla**' yı ve ardından **Yeni örnek Başlat**' ı seçerek bu uygulamayı çalıştırın. İkizi bilgilerini almayı ve sonra *bildirilen bir özellik*olarak bağlantı göndermeyi görmeniz gerekir.
   
    ![Bağlantı raporlamak için cihaz uygulamasını çalıştırma](./media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png)
       
11. Cihaz bağlantı bilgilerini raporladığı için, her iki sorgu da görünmelidir. Sorguları yeniden çalıştırmak için .NET **Addtagsandquery** uygulamasını çalıştırın. **Mydeviceıd** bu kez sorgu sonuçlarında görünmelidir.
   
    ![Cihaz bağlantısı başarıyla bildirildi](./media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Cihaz meta verilerini bir arka uç uygulamasından Etiketler olarak eklediniz ve cihaz ikizi cihaz bağlantı bilgilerini raporlamak için bir sanal cihaz uygulaması yazmış olursunuz. Ayrıca, bu bilgilerin SQL benzeri IoT Hub sorgu dilini kullanarak nasıl sorgulanalınacağını da öğrendiniz.

Aşağıdakilerin nasıl yapılacağını öğrenmek için aşağıdaki kaynakları kullanın:

* [bir cihazdan IoT Hub 'ına telemetri gönder](quickstart-send-telemetry-dotnet.md) öğreticisine sahip cihazlardan telemetri gönderin.

* [cihazları yapılandırmak için istenen özellikleri kullan](tutorial-device-twins.md) öğreticisiyle cihaz ikizi 'nin istenen özelliklerini kullanarak cihazları yapılandırın öğreticisi

* cihazları etkileşimli olarak (kullanıcı denetimli bir uygulamadan bir fanı açmak gibi) [doğrudan Yöntemler](quickstart-control-device-dotnet.md) öğreticisini kullanarak kontrol edin.
