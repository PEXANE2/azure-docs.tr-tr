---
title: İşleri Azure IoT Hub zamanlayın (.NET/.NET) | Microsoft Docs
description: Birden çok cihazda doğrudan yöntem çağırmak için bir Azure IoT Hub işi zamanlama. İşi çalıştırmak için sanal cihaz uygulamalarını ve bir hizmet uygulamasını uygulamak üzere .NET için Azure IoT cihaz SDK 'sını kullanın.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/16/2019
ms.author: robinsh
ms.openlocfilehash: 3594828ff3a79242e1cfd4663c415d8de502a329
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69872764"
---
# <a name="schedule-and-broadcast-jobs-net"></a>İşleri zamanlama ve yayınlama (.NET)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Milyonlarca cihazı güncelleştiren işleri zamanlamak ve izlemek için Azure IoT Hub kullanın. İşleri şu şekilde kullan:

* İstenen özellikleri güncelleştirme
* Etiketleri Güncelleştir
* Doğrudan metotları çağır

Bir iş bu eylemlerden birini sarmalar ve yürütmeyi bir cihaz ikizi sorgusu tarafından tanımlanan bir dizi cihaza karşı izler. Örneğin, bir arka uç uygulaması, cihazları yeniden çalıştıran 10.000 cihazda doğrudan bir yöntemi çağırmak için bir iş kullanabilir. Bir cihaz ikizi sorgusu olan cihaz kümesini belirtirsiniz ve işi gelecekteki bir zamanda çalışacak şekilde zamanlayabilirsiniz. Her bir cihazın yeniden başlatma doğrudan metodunu alıp yürütmesi sırasında iş ilerleme durumunu izler.

Bu yeteneklerin her biri hakkında daha fazla bilgi edinmek için bkz.:

* Cihaz ikizi ve özellikleri: [Cihaz](iot-hub-csharp-csharp-twin-getstarted.md) ikizlerini ve [öğreticisini kullanmaya başlayın: Cihaz ikizi özelliklerini kullanma](tutorial-device-twins.md)

* Doğrudan Yöntemler: [IoT Hub Geliştirici Kılavuzu-doğrudan Yöntemler](iot-hub-devguide-direct-methods.md) ve [öğretici: Doğrudan Yöntemler kullanma](quickstart-control-device-dotnet.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Arka uç uygulaması tarafından çağrılabilecek **Lockkapısı** adlı doğrudan bir yöntemi uygulayan bir cihaz uygulaması oluşturun.

* Birden çok cihazda **Lockkapısı** doğrudan yöntemini çağırmak için bir iş oluşturan bir arka uç uygulaması oluşturun. Başka bir iş, istenen özellik güncelleştirmelerini birden çok cihaza gönderir.

Bu öğreticinin sonunda iki .NET (C#) konsol uygulamanız vardır:

IoT Hub 'ınıza bağlanan ve **Lockkapısı** Direct metodunu uygulayan **SimulateDeviceMethods** .

**Lockkapıya** doğrudan yöntemini çağırmak ve cihazı ikizi istenen özellikleri birden çok cihazda güncelleştirmek için Işleri kullanan **schedulejob** .

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio. Bu öğretici, Visual Studio 2017 kullanır.

* Etkin bir Azure hesabı. Hesabınız yoksa yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>IoT Hub 'a yeni bir cihaz kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, çözüm arka ucu tarafından çağrılan doğrudan bir yönteme yanıt veren bir .NET konsol uygulaması oluşturacaksınız.

1. Visual Studio 'da, **konsol uygulaması** proje C# şablonunu kullanarak yeni veya mevcut bir çözüme bir Visual Windows Klasik Masaüstü projesi ekleyin. Projeyi **SimulateDeviceMethods**olarak adlandırın.

    ![Yeni Visual C# Windows Klasik cihaz uygulaması](./media/iot-hub-csharp-csharp-schedule-jobs/create-device-app.png)

2. Çözüm Gezgini, **SimulateDeviceMethods** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet...** seçeneğini belirleyin.

3. **NuGet Paket Yöneticisi** penceresinde, **Araştır** ' ı seçin ve **Microsoft. Azure. Devices. Client**için arama yapın. **Microsoft. Azure. Devices. Client** paketini yüklemek için **Install** ' ı seçin ve kullanım koşullarını kabul edin. Bu yordam, [Azure IoT cihaz SDK 'sı](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet paketi ve bağımlılıklarını indirir, yükler ve buna bir başvuru ekler.

    ![NuGet Paket Yöneticisi penceresi Istemci uygulaması](./media/iot-hub-csharp-csharp-schedule-jobs/device-app-nuget.png)

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, önceki bölümde not ettiğiniz cihaz bağlantı dizesiyle değiştirin:

    ```csharp
    static string DeviceConnectionString = "<yourDeviceConnectionString>";
    static DeviceClient Client = null;
    ```

6. Cihaza doğrudan yöntemini uygulamak için aşağıdakileri ekleyin:

    ```csharp
    static Task<MethodResponse> LockDoor(MethodRequest methodRequest, object userContext)
    {
        Console.WriteLine();
        Console.WriteLine("Locking Door!");
        Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);

        string result = "'Door was locked.'";
        return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
    }
    ```

7. Cihaza cihaz TWINS dinleyicisini uygulamak için aşağıdakileri ekleyin:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, 
      object userContext)
    {
        Console.WriteLine("Desired property change:");
        Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
    }
    ```

8. Son olarak, aşağıdaki kodu **Main** yöntemine ekleyerek IoT Hub 'ınız bağlantısını açın ve Yöntem dinleyicisini başlatın:

    ```csharp
    try
    {
        Console.WriteLine("Connecting to hub");
        Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, 
          TransportType.Mqtt);

        Client.SetMethodHandlerAsync("LockDoor", LockDoor, null);
        Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null);

        Console.WriteLine("Waiting for direct method call and device twin update\n Press enter to exit.");
        Console.ReadLine();

        Console.WriteLine("Exiting...");

        Client.SetMethodHandlerAsync("LockDoor", null, null);
        Client.CloseAsync().Wait();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```

9. Çalışmanızı kaydedin ve çözümünüzü derleyin.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [geçici hata işleme](/azure/architecture/best-practices/transient-faults)makalesinde önerildiği gibi yeniden deneme ilkelerini (bağlantı yeniden denemesi gibi) uygulamalısınız.
>

## <a name="get-the-iot-hub-connection-string"></a>IoT Hub bağlantı dizesini al

[!INCLUDE [iot-hub-howto-schedule-jobs-shared-access-policy-text](../../includes/iot-hub-howto-schedule-jobs-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Doğrudan yöntem çağırmak ve cihaz ikizi güncelleştirmelerini göndermek için işleri zamanlama

Bu bölümde, **Lockkapısı** doğrudan yöntemini çağırmak ve istenen özellik güncelleştirmelerini C#birden çok cihaza göndermek için işleri kullanan bir .NET konsol uygulaması (kullanarak) oluşturursunuz.

1. Visual Studio'da **Konsol Uygulaması** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Projeyi **Schedulejob**olarak adlandırın.

    ![Yeni Visual C# Windows Klasik Masaüstü projesi](./media/iot-hub-csharp-csharp-schedule-jobs/createnetapp.png)

2. Çözüm Gezgini, **Schedulejob** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet...** seçeneğini belirleyin.

3. **NuGet Paket Yöneticisi** penceresinde, **Araştır**' ı seçin, Microsoft. **Azure. Devices**' i arayın, **Microsoft. Azure. Devices** paketini yüklemek için, **install** ' ı seçin ve kullanım koşullarını kabul edin. Bu adım, [Azure IoT hizmeti SDK 'sı](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet paketi ve bağımlılıklarını indirir, yükler ve buna bir başvuru ekler.

    ![NuGet Paket Yöneticisi penceresi](./media/iot-hub-csharp-csharp-schedule-jobs/servicesdknuget.png)

4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

5. Varsayılan deyimlerde `using` henüz yoksa, aşağıdaki deyimi ekleyin.

    ```csharp
    using System.Threading;
    using System.Threading.Tasks;
    ```

6. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucuları, [IoT Hub bağlantı dizesini](#get-the-iot-hub-connection-string) ve cihazınızın adını almak için, daha önce kopyaladığınız IoT Hub bağlantı dizesiyle değiştirin.

    ```csharp
    static JobClient jobClient;
    static string connString = "<yourIotHubConnectionString>";
    static string deviceId = "<yourDeviceId>";
    ```

7. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && 
          (result.Status != JobStatus.Failed));
    }
    ```

8. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = 
          new CloudToDeviceMethod("LockDoor", TimeSpan.FromSeconds(5), 
          TimeSpan.FromSeconds(5));

        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            $"DeviceId IN ['{deviceId}']",
            directMethod,
            DateTime.UtcNow,
            (long)TimeSpan.FromMinutes(2).TotalSeconds);

        Console.WriteLine("Started Method Job");
    }
    ```

9. **Program** sınıfına başka bir yöntem ekleyin:

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        Twin twin = new Twin(deviceId);
        twin.Tags = new TwinCollection();
        twin.Tags["Building"] = "43";
        twin.Tags["Floor"] = "3";
        twin.ETag = "*";

        twin.Properties.Desired["LocationUpdate"] = DateTime.UtcNow;

        JobResponse createJobResponse = jobClient.ScheduleTwinUpdateAsync(
            jobId,
            $"DeviceId IN ['{deviceId}']", 
            twin, 
            DateTime.UtcNow, 
            (long)TimeSpan.FromMinutes(2).TotalSeconds).Result;

        Console.WriteLine("Started Twin Update Job");
    }
    ```

    > [!NOTE]
    > Sorgu sözdizimi hakkında daha fazla bilgi için [IoT Hub sorgu dili](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-query-language)' ne bakın.
    >

10. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp
    Console.WriteLine("Press ENTER to start running jobs.");
    Console.ReadLine();

    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER to run the next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```

11. Çalışmanızı kaydedin ve çözümünüzü derleyin.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Visual Studio Çözüm Gezgini, çözümünüze sağ tıklayın ve **Başlangıç projelerini ayarla**' yı seçin. Ardından **birden çok başlangıç projesi**seçin. **SimulateDeviceMethods** listenin en üstünde ve sonra **schedulejob**' ın altında olduğundan emin olun. Her iki eylemini **Başlangıç** olarak ayarlayın ve **Tamam**' ı seçin.

2. **Başlat** ' i seçerek veya **hata ayıklama** menüsüne gidip **hata ayıklamayı Başlat**' a tıklayarak projeleri çalıştırın.

3. Çıktıyı hem cihazın hem de arka uç uygulamalardan görürsünüz.

    ![İşleri zamanlamak için uygulamaları çalıştırma](./media/iot-hub-csharp-csharp-schedule-jobs/schedulejobs.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir cihaza doğrudan yöntem zamanlamak ve Device ikizi 'in özelliklerinin güncelleştirilmesi için bir iş kullandınız.

AIR üretici yazılımı güncelleştirmesi üzerinden uzak gibi IoT Hub ve cihaz yönetim desenleriyle çalışmaya devam etmek için öğreticiyi okuyun [: Üretici yazılımı güncelleştirmesi](tutorial-firmware-update.md)nasıl yapılır?

Azure IoT Edge ile uç cihazlara AI dağıtma hakkında bilgi edinmek için bkz. [IoT Edge kullanmaya](../iot-edge/tutorial-simulate-device-linux.md)başlama.
