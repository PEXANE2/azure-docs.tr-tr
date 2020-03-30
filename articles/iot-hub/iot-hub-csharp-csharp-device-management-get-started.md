---
title: Azure IoT Hub cihaz yönetimi (.NET/.NET) | Microsoft Dokümanlar
description: Uzak bir aygıtı yeniden başlatmak için Azure IoT Hub aygıt yönetimi nasıl kullanılır? Doğrudan bir yöntem içeren bir simüle aygıt uygulaması uygulamak için .NET için Azure IoT aygıtı SDK'yı ve doğrudan yöntemi çağıran bir hizmet uygulamasını uygulamak için .NET için .NET için Azure IoT hizmeti SDK'yı kullanırsınız.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: robinsh
ms.openlocfilehash: 3b37d7e049e7daabbbb4fe1a7b49feb654e8accc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110249"
---
# <a name="get-started-with-device-management-net"></a>Cihaz yönetimine başlayın (.NET)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IoT hub'ınızda bir IoT hub'ı oluşturmak ve aygıt kimliği oluşturmak için Azure portalını kullanın.

* Bu aygıtı yeniden başlatan doğrudan bir yöntem içeren benzetimli bir aygıt uygulaması oluşturun. Buluttan doğrudan yöntemler çağrılır.

* IoT hub'ınız aracılığıyla simüle edilen cihaz uygulamasında yeniden başlatma yöntemini çağıran bir .NET konsol uygulaması oluşturun.

Bu öğreticinin sonunda iki .NET konsol uygulamanız olacak:

* **Simüle Edilmiş Aygıt**. Bu uygulama, daha önce oluşturulan aygıt kimliğiyle IoT hub'ınıza bağlanır, doğrudan yeniden başlatma yöntemini yeniden başlatın, fiziksel bir yeniden başlatmayı simüle eder ve son yeniden başlatma süresini bildirir.

* **TriggerReboot**. Bu uygulama, benzetilen aygıt uygulamasında doğrudan bir yöntem çağırır, yanıtı görüntüler ve güncellenen bildirilen özellikleri görüntüler.

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio.

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

[!INCLUDE [iot-hub-howto-device-management-shared-access-policy-text](../../includes/iot-hub-howto-device-management-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Doğrudan bir yöntem kullanarak aygıtta uzaktan yeniden başlatmayı tetikleme

Bu bölümde, C#'ı kullanarak doğrudan bir yöntem kullanarak bir aygıtta uzaktan yeniden başlatma başlatan bir .NET konsol uygulaması oluşturursunuz. Uygulama, söz sözleşme nin son yeniden başlatma saatini bulmak için cihaz ikiz sorgularını kullanır.

1. Visual Studio'da **yeni bir proje oluştur'u**seçin.

1. **Yeni bir proje oluştur'da** **Konsol Uygulaması (.NET Framework)** proje şablonunu bulun ve seçin ve ardından **İleri'yi**seçin.

1. **Yeni projenizi yapılandırın,** *project TriggerReboot*adını ve .NET Framework sürümü 4.5.1 veya daha sonrasını seçin. **Oluştur'u**seçin.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi](./media/iot-hub-csharp-csharp-device-management-get-started/create-trigger-reboot-configure.png)

1. **Solution Explorer'da** **TriggerReboot** projesini sağ tıklatın ve ardından **NuGet Paketlerini Yönet'i**seçin.

1. **Gözat'ı**seçin, ardından **Microsoft.Azure.Devices'ı**arayın ve seçin. **Microsoft.Azure.Devices** paketini yüklemek için **Yükle'yi** seçin.

    ![NuGet Paket Yöneticisi penceresi](./media/iot-hub-csharp-csharp-device-management-get-started/create-trigger-reboot-nuget-devices.png)

   Bu adım, [Azure IoT hizmeti SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet paketini ve bağımlılıklarını karşıdan yükler, yükler ve ekler.

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

   ```csharp
   using Microsoft.Azure.Devices;
   using Microsoft.Azure.Devices.Shared;
   ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. `{iot hub connection string}` Yer tutucu [değerini, IoT hub bağlantı dizesini al'da](#get-the-iot-hub-connection-string)daha önce kopyaladığınız IoT Hub bağlantı dizesiyle değiştirin.

   ```csharp
   static RegistryManager registryManager;
   static string connString = "{iot hub connection string}";
   static ServiceClient client;
   static string targetDevice = "myDeviceId";
   ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin.  Bu kod, aygıtı yeniden başlatma aygıtı için ikiz alır ve bildirilen özellikleri çıkar.

   ```csharp
   public static async Task QueryTwinRebootReported()
   {
       Twin twin = await registryManager.GetTwinAsync(targetDevice);
       Console.WriteLine(twin.Properties.Reported.ToJson());
   }
   ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin.  Bu kod, doğrudan bir yöntem kullanarak aygıtta yeniden başlatmayı başlatır.

   ```csharp
   public static async Task StartReboot()
   {
       client = ServiceClient.CreateFromConnectionString(connString);
       CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
       method.ResponseTimeout = TimeSpan.FromSeconds(30);

       CloudToDeviceMethodResult result = await 
         client.InvokeDeviceMethodAsync(targetDevice, method);

       Console.WriteLine("Invoked firmware update on device.");
   }
   ```

1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

   ```csharp
   registryManager = RegistryManager.CreateFromConnectionString(connString);
   StartReboot().Wait();
   QueryTwinRebootReported().Wait();
   Console.WriteLine("Press ENTER to exit.");
   Console.ReadLine();
   ```

1. **Yapı** > **Çözümünü**Seçin.

> [!NOTE]
> Bu öğretici, aygıtın bildirilen özellikleri için yalnızca tek bir sorgu gerçekleştirir. Üretim kodunda, bildirilen özelliklerdeki değişiklikleri algılamak için yoklama yapmanızı öneririz.

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde şunları yapacaksınız:

* Bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir .NET konsol uygulaması oluşturun.

* Benzetimli aygıtıyeniden başlatmayı tetikle.

* Aygıtları tanımlamak için aygıt ikiz sorgularını ve en son ne zaman yeniden başlatıldığını etkinleştirmek için bildirilen özellikleri kullanın.

Benzetimli aygıt uygulamasını oluşturmak için aşağıdaki adımları izleyin:

1. Visual Studio'da, zaten oluşturduğunuz TriggerReboot çözümünde**Yeni** >  **Dosya** > **Projesi'ni**seçin. **Yeni bir proje oluştur'da** **Konsol Uygulaması (.NET Framework)** proje şablonunu bulun ve seçin ve ardından **İleri'yi**seçin.

1. **Yeni projenizi yapılandırın,** *projeyi SimulateManagedDevice*olarak adlandırın ve **Çözüm**için **Çözüme Ekle'yi**seçin. **Oluştur'u**seçin.

    ![Projenizi çözüme adlandırın ve ekleyin](./media/iot-hub-csharp-csharp-device-management-get-started/configure-device-app.png)

1. Çözüm Gezgini'nde, yeni **SimulateManagedDevice** projesini sağ tıklatın ve ardından **NuGet Paketlerini Yönet'i**seçin.

1. **Gözat'ı**seçin, ardından **Microsoft.Azure.Devices.Client'ı**arayın ve seçin. **Yükle**’yi seçin.

    ![NuGet Paket Yöneticisi penceresi İstemci uygulaması](./media/iot-hub-csharp-csharp-device-management-get-started/create-device-nuget-devices-client.png)

   Bu adım, [Azure IoT aygıtı SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet paketini ve bağımlılıklarını karşıdan yükler, yükler ve ekler.

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer `{device connection string}` tutucu [değerini, IoT hub'ında daha](#register-a-new-device-in-the-iot-hub)önce kaydedilen aygıt bağlantı dizesiyle değiştirin.

    ```csharp
    static string DeviceConnectionString = "{device connection string}";
    static DeviceClient Client = null;
    ```

1. Aygıtta doğrudan yöntemi uygulamak için aşağıdakileri ekleyin:

   ```csharp
   static Task<MethodResponse> onReboot(MethodRequest methodRequest, object userContext)
   {
       // In a production device, you would trigger a reboot 
       //   scheduled to start after this method returns.
       // For this sample, we simulate the reboot by writing to the console
       //   and updating the reported properties.
       try
       {
           Console.WriteLine("Rebooting!");

           // Update device twin with reboot time. 
           TwinCollection reportedProperties, reboot, lastReboot;
           lastReboot = new TwinCollection();
           reboot = new TwinCollection();
           reportedProperties = new TwinCollection();
           lastReboot["lastReboot"] = DateTime.Now;
           reboot["reboot"] = lastReboot;
           reportedProperties["iothubDM"] = reboot;
           Client.UpdateReportedPropertiesAsync(reportedProperties).Wait();
       }
       catch (Exception ex)
       {
           Console.WriteLine();
           Console.WriteLine("Error in sample: {0}", ex.Message);
       }

       string result = @"{""result"":""Reboot started.""}";
       return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
   }
   ```

1. Son olarak, IoT hub'ınıza bağlantıyı açmak ve yöntem dinleyicisini başlatmak için **Ana** yönteme aşağıdaki kodu ekleyin:

   ```csharp
   try
   {
       Console.WriteLine("Connecting to hub");
       Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
         TransportType.Mqtt);

       // setup callback for "reboot" method
       Client.SetMethodHandlerAsync("reboot", onReboot, null).Wait();
       Console.WriteLine("Waiting for reboot method\n Press enter to exit.");
       Console.ReadLine();

       Console.WriteLine("Exiting...");

       // as a good practice, remove the "reboot" handler
       Client.SetMethodHandlerAsync("reboot", null, null).Wait();
       Client.CloseAsync().Wait();
   }
   catch (Exception ex)
   {
       Console.WriteLine();
       Console.WriteLine("Error in sample: {0}", ex.Message);
   }
   ```

1. Çözüm Gezgini'nde, çözümünüzü sağ tıklatın ve ardından **Başlangıç Projeleri Ayarla'yı**seçin.

1. **Ortak Özellikler** > **Başlangıç Projesi**için, Tek **başlangıç projesini**seçin ve ardından **Benzetim Aygıtı** projesini seçin. Değişikliklerinizi kaydetmek için **Tamam**’ı seçin.

1. **Yapı** > **Çözümünü**Seçin.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [Geçici hata işlemede](/azure/architecture/best-practices/transient-faults)önerildiği gibi yeniden deneme ilkelerini (üstel geri leme gibi) uygulamanız gerekir.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Artık uygulamaları çalıştırmaya hazırsınız.

1. .NET aygıt uygulaması **SimulateManagedDevice'ı**çalıştırmak için Solution **Explorer'da, SimulateManagedDevice** projesini sağ tıklatın, **Hata Ayıklama'yı**seçin ve ardından yeni örneği **başlat'ı**seçin. Uygulama, IoT hub'ınızdan yapılan yöntem çağrılarını dinlemeye başlamalıdır.

1. Bundan sonra aygıt bağlanır ve yöntem çağrıları için bekliyor, **TriggerReboot** projesi sağ tıklatın, **Hata Ayıklama**seçin ve sonra **yeni örnek başlat'ı**seçin.

   "Yeniden Başlatma"yı görmelisin. **SimulatedManagedDevice** konsolunda ve **TriggerReboot** konsolunda yazılmış son yeniden başlatma süresini içeren aygıtın bildirilen özelliklerinde yazılır.

    ![Servis ve cihaz uygulaması çalıştır](./media/iot-hub-csharp-csharp-device-management-get-started/combinedrun.png)

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]
