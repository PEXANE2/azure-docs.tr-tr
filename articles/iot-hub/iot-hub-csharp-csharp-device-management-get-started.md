---
title: Azure IoT Hub cihaz yönetimini kullanmaya başlama (.NET/.NET) | Microsoft Docs
description: Uzak cihaz yeniden başlatma işlemini başlatmak için Azure IoT Hub cihaz yönetimi 'ni kullanma. Doğrudan yöntemini çağıran bir hizmet uygulaması uygulamak üzere .NET için Azure IoT hizmeti SDK 'sını içeren bir sanal cihaz uygulaması uygulamak üzere .NET için Azure IoT cihaz SDK 'sını kullanın.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 09/15/2017
ms.author: robinsh
ms.openlocfilehash: 544c29505d9bf34622963dfac8987b7eae4f8bc5
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668182"
---
# <a name="get-started-with-device-management-net"></a>Cihaz yönetimini kullanmaya başlama (.NET)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Bir IoT Hub oluşturmak ve IoT Hub 'ınızda bir cihaz kimliği oluşturmak için Azure portal kullanın.

* Bu cihazı yeniden yükleyen doğrudan bir yöntem içeren bir sanal cihaz uygulaması oluşturun. Doğrudan Yöntemler buluttan çağrılır.

* IoT Hub 'ınız aracılığıyla sanal cihaz uygulamasındaki önyükleme doğrudan yöntemini çağıran bir .NET konsol uygulaması oluşturun.

Bu öğreticinin sonunda iki .NET konsol uygulamanız olacak:

* Daha önce oluşturulan cihaz kimliğiyle IoT Hub 'ınıza bağlanan **SimulateManagedDevice**, bir yeniden başlatma doğrudan yöntemi alır, fiziksel yeniden başlatmanın benzetimini yapar ve son yeniden başlatmanın zamanını raporlar.

* Sanal cihaz uygulamasında doğrudan bir yöntemi çağıran **Triggerreboot**, yanıtı görüntüler ve güncelleştirilmiş bildirilen özellikleri görüntüler.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio.

* Etkin bir Azure hesabı. (Hesabınız yoksa yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>IoT Hub 'a yeni bir cihaz kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT Hub bağlantı dizesini al

[!INCLUDE [iot-hub-howto-device-management-shared-access-policy-text](../../includes/iot-hub-howto-device-management-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Doğrudan bir yöntem kullanarak cihazda Uzaktan yeniden başlatma tetikleyin

Bu bölümde, doğrudan bir yöntemi kullanarak bir cihazda Uzaktan yeniden başlatma C#işlemini başlatan bir .NET konsol uygulaması (kullanarak) oluşturursunuz. Uygulama, bu cihazın son yeniden başlatma zamanını saptamak için Device ikizi sorgularını kullanır.

1. Visual Studio’da **Konsol Uygulaması (.NET Framework)** proje şablonunu kullanarak yeni bir çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.5.1 veya sonraki bir sürüm olduğundan emin olun. Projeyi **Triggerreboot**olarak adlandırın.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi](./media/iot-hub-csharp-csharp-device-management-get-started/createserviceapp.png)

2. Çözüm Gezgini, **Triggerreboot** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**' e tıklayın.

3. **NuGet Paket Yöneticisi** penceresinde, **Araştır**' ı seçin, Microsoft. **Azure. Devices**' i arayın, **Microsoft. Azure. Devices** paketini yüklemek için, **install** ' ı seçin ve kullanım koşullarını kabul edin. Bu yordam, [Azure IoT hizmeti SDK 'sı](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet paketi ve bağımlılıklarını indirir, yükler ve buna bir başvuru ekler.

    ![NuGet Paket Yöneticisi penceresi](./media/iot-hub-csharp-csharp-device-management-get-started/servicesdknuget.png)

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
   ```csharp
   using Microsoft.Azure.Devices;
   using Microsoft.Azure.Devices.Shared;
   ```
        
5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, [IoT Hub bağlantı dizesini al](#get-the-iot-hub-connection-string)içinde daha önce kopyaladığınız IoT Hub bağlantı dizesiyle değiştirin.
   
   ```csharp
   static RegistryManager registryManager;
   static string connString = "{iot hub connection string}";
   static ServiceClient client;
   static string targetDevice = "myDeviceId";
   ```

6. **Program** sınıfına aşağıdaki yöntemi ekleyin.  Bu kod, yeniden başlatılan cihaz için cihaz ikizi alır ve bildirilen özellikleri verir.
   
   ```csharp
   public static async Task QueryTwinRebootReported()
   {
       Twin twin = await registryManager.GetTwinAsync(targetDevice);
       Console.WriteLine(twin.Properties.Reported.ToJson());
   }
   ```
        
7. **Program** sınıfına aşağıdaki yöntemi ekleyin.  Bu kod, doğrudan bir yöntemi kullanarak cihazda yeniden başlatmayı başlatır.

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

7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
   ```csharp
   registryManager = RegistryManager.CreateFromConnectionString(connString);
   StartReboot().Wait();
   QueryTwinRebootReported().Wait();
   Console.WriteLine("Press ENTER to exit.");
   Console.ReadLine();
   ```

8. Çözümü oluşturun.

> [!NOTE]
> Bu öğreticide, cihazın bildirilen özellikleri için yalnızca tek bir sorgu gerçekleştirilir. Üretim kodunda, bildirilen özelliklerde yapılan değişiklikleri algılamayı yoklamayı öneririz.

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde şunları gerçekleştireceksiniz:

* Bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir .NET konsol uygulaması oluşturun.

* Sanal cihaz yeniden başlatması tetikleyin.

* Cihaz ikizi sorguları cihazları ve en son yeniden başlatıldığı zaman bunları belirlemek için, bildirilen özellikleri kullanın.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Projeyi **SimulateManagedDevice**olarak adlandırın.
   
    ![Yeni Visual C# Windows Klasik cihaz uygulaması](./media/iot-hub-csharp-csharp-device-management-get-started/createdeviceapp.png)
    
2. Çözüm Gezgini, **SimulateManagedDevice** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet...** öğesine tıklayın.

3. **NuGet Paket Yöneticisi** penceresinde, **Araştır** ' ı seçin ve **Microsoft. Azure. Devices. Client**için arama yapın. **Microsoft. Azure. Devices. Client** paketini yüklemek için **Install** ' ı seçin ve kullanım koşullarını kabul edin. Bu yordam, [Azure IoT cihaz SDK 'sı](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet paketi ve bağımlılıklarını indirir, yükler ve buna bir başvuru ekler.
   
    ![NuGet Paket Yöneticisi penceresi Istemci uygulaması](./media/iot-hub-csharp-csharp-device-management-get-started/clientsdknuget.png)
    
4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, önceki bölümde not ettiğiniz cihaz bağlantı dizesiyle değiştirin.

    ```csharp
    static string DeviceConnectionString = 
      "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```
6. Cihaza doğrudan yöntemini uygulamak için aşağıdakileri ekleyin:

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

       string result = "'Reboot started.'";
       return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
   }
   ```

7. Son olarak, aşağıdaki kodu **Main** yöntemine ekleyerek IoT Hub 'ınız bağlantısını açın ve Yöntem dinleyicisini başlatın:

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
        
8. Visual Studio Çözüm Gezgini'nde çözümünüze sağ tıklayın ve ardından **Başlangıç Projelerini Ayarla...** öğesine tıklayın. **Tek bir başlangıç projesi**seçin ve açılan menüden **SimulateManagedDevice** projesini seçin. Çözümü oluşturun.       

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [geçici hata işleme](/azure/architecture/best-practices/transient-faults)makalesinde önerildiği gibi yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. .NET cihaz uygulamasını çalıştırmak için **SimulateManagedDevice**. **SimulateManagedDevice** projesine sağ tıklayın, **Hata Ayıkla**' yı seçin ve ardından **Yeni örnek Başlat**' ı seçin. IoT Hub 'ınızdan Yöntem çağrılarını dinlemeye başlaması gerekir. 

2. Artık cihaz bağlı olduğuna ve Yöntem etkinleştirmeleri beklerken, sanal cihaz uygulamasındaki yeniden başlatma yöntemini çağırmak için .NET **Triggerreboot** uygulamasını çalıştırın. Bunu yapmak için, **Triggerreboot** projesine sağ tıklayın, **Hata Ayıkla**' yı seçin ve ardından **Yeni örnek Başlat**' ı seçin. "Yeniden başlatılıyor!" öğesini görmeniz gerekir **SimulatedManagedDevice** konsolunda yazılır ve cihazın, **triggerreboot** konsoluna yazılan son yeniden başlatma zamanını içeren bildirilen özellikleri.
   
    ![Hizmet ve cihaz uygulaması çalıştırma](./media/iot-hub-csharp-csharp-device-management-get-started/combinedrun.png)

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]
