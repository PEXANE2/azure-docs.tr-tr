---
title: Azure IoT Hub (.NET/.NET) | Microsoft Dokümanlar
description: Birden çok aygıtta doğrudan bir yöntem çağırmak için Azure IoT Hub işi zamanlama. Simüle edilmiş aygıt uygulamalarını ve işi yürütmek için bir hizmet uygulamasını uygulamak için .NET için Azure IoT cihazı SDK'yı kullanırsınız.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: robinsh
ms.openlocfilehash: 7925ca5c69d01b098764ff744fb832eaa43118d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77108961"
---
# <a name="schedule-and-broadcast-jobs-net"></a>Program ve yayın işleri (.NET)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Milyonlarca aygıtı güncelleştiren işleri zamanlamak ve izlemek için Azure IoT Hub'ı kullanın. İşleri şu şekilde kullanın:

* İstenen özellikleri güncelleştirme

* Etiketleri güncelleştir

* Doğrudan yöntemleri çağırma

Bir iş bu eylemlerden birini sarar ve yürütmeyi aygıt ikiz sorgusu tarafından tanımlanan aygıtlar kümesine karşı izler. Örneğin, bir arka uç uygulaması, aygıtları yeniden başlatan 10.000 aygıtta doğrudan bir yöntem çağırmak için bir işi kullanabilir. Aygıt ikiz sorgusuna sahip aygıt kümesini belirtir ve işi ileriki bir zamanda çalışacak şekilde zamanlarsınız. Her aygıt doğrudan yeniden başlatma yöntemini alır ve çalıştırırken iş ilerlemeyi izler.

Bu yeteneklerin her biri hakkında daha fazla bilgi edinmek için bkz:

* Cihaz ikizi ve özellikleri: [Aygıt ikizleri](iot-hub-csharp-csharp-twin-getstarted.md) ve Öğretici ile [başlayın: Cihaz ikiz özellikleri nasıl kullanılır](tutorial-device-twins.md)

* Doğrudan yöntemler: [IoT Hub geliştirici kılavuzu - doğrudan yöntemler](iot-hub-devguide-direct-methods.md) ve [Öğretici: Doğrudan yöntemleri kullanın](quickstart-control-device-dotnet.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* **LockDoor**adlı doğrudan bir yöntem uygulayan bir aygıt uygulaması oluşturun, arka uç uygulaması tarafından çağrılabilir.

* Birden çok cihazda **LockDoor** doğrudan yöntemini aramak için bir iş oluşturan bir arka uç uygulaması oluşturun. Başka bir iş, istenen özellik güncelleştirmelerini birden çok aygıta gönderir.

Bu eğitimin sonunda iki .NET (C#) konsol uygulamanız vardır:

* **Simüle Cihaz Yöntemleri**. Bu uygulama IoT hub'ınıza bağlanır ve **LockDoor** doğrudan yöntemini uygular.

* **Programİş**. Bu **uygulama, LockDoor** doğrudan yöntemini aramak ve aygıtı birden çok cihazda istenen ikiz özellikleri güncelleştirmek için işleri kullanır.

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio.

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, çözüm arka uç tarafından çağrılan doğrudan bir yönteme yanıt veren bir .NET konsol uygulaması oluşturursunuz.

1. Visual Studio'da **yeni bir proje oluştur'u**seçin ve ardından Konsol Uygulaması **(.NET Framework)** proje şablonu'nu seçin. Devam etmek için **İleri**’yi seçin.

1. **Yeni projenizi yapılandırın,** *projeyi Simüle Eden Aygıt Yöntemleri'ni*adlandırın ve ardından **Oluştur'u**seçin.

    ![Simüle Aygıt Yöntemleri projenizi yapılandırın](./media/iot-hub-csharp-csharp-schedule-jobs/configure-device-app.png)

1. Çözüm Gezgini'nde, **SimulateDeviceMethods** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet'i**seçin.

1. **NuGet Package Manager'da** **Gözat'ı** seçin ve **Microsoft.Azure.Devices.Client'ı**arayın ve seçin. **Yükle**’yi seçin.

    ![NuGet Paket Yöneticisi penceresi İstemci uygulaması](./media/iot-hub-csharp-csharp-schedule-jobs/device-app-nuget.png)

    Bu adım, [Azure IoT aygıtı SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) NuGet paketini ve bağımlılıklarını karşıdan yükler, yükler ve ekler.

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini önceki bölümde belirttiğiniz aygıt bağlantı dizesiyle değiştirin:

    ```csharp
    static string DeviceConnectionString = "<yourDeviceConnectionString>";
    static DeviceClient Client = null;
    ```

1. Aygıtta doğrudan yöntemi uygulamak için aşağıdaki kodu ekleyin:

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

1. Aygıttaki ikiz dinleyiciyi uygulamak için aşağıdaki yöntemi ekleyin:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, 
      object userContext)
    {
        Console.WriteLine("Desired property change:");
        Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
    }
    ```

1. Son olarak, IoT hub'ınıza bağlantıyı açmak ve yöntem dinleyicisini başlatmak için **Ana** yönteme aşağıdaki kodu ekleyin:

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

1. Çalışmanızı kaydedin ve çözümünüzü oluşturun.

> [!NOTE]
> Basit şeyler tutmak için, bu öğretici herhangi bir yeniden deneme ilkeleri uygulamaz. Üretim kodunda, [Geçici hata işlemede](/azure/architecture/best-practices/transient-faults)önerildiği gibi yeniden deneme ilkeleri (bağlantı yeniden denemesi gibi) uygulamanız gerekir.
>

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

[!INCLUDE [iot-hub-howto-schedule-jobs-shared-access-policy-text](../../includes/iot-hub-howto-schedule-jobs-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Doğrudan bir yöntem çağırmak ve aygıt ikiz güncelleştirmeleri göndermek için işleri zamanlama

Bu bölümde, **LockDoor** doğrudan yöntemini aramak ve birden çok cihaza istenen özellik güncelleştirmelerini göndermek için işleri kullanan bir .NET konsol uygulaması (C#'yi kullanarak) oluşturursunuz.

1. Visual Studio'da **Dosya** > **Yeni** > **Projesi'ni**seçin. **Yeni bir proje oluştur'da**Konsol **Uygulaması'nı (.NET Framework)** seçin ve **sonra İleri'yi**seçin.

1. **Yeni projenizi yapılandırın,** project *ScheduleJob*adını adlandırın. **Çözüm**için **çözüme ekle'yi**seçin ve ardından **Oluştur'u**seçin.

    ![Ad ve planyapıla Zamanlamaİş projesi](./media/iot-hub-csharp-csharp-schedule-jobs/config-schedule-job-app.png)

1. Çözüm Gezgini'nde, **ScheduleJob** projesini sağ tıklatın ve ardından **NuGet Paketlerini Yönet'i**seçin.

1. **NuGet Paket**Yöneticisi'nde, **Gözat' ı,** Arama'yı ve **Microsoft.Azure.Devices'ı**seçin, ardından **Yükle'yi**seçin.

   Bu adım, [Azure IoT hizmeti SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices/) NuGet paketini ve bağımlılıklarını karşıdan yükler, yükler ve ekler.

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. Varsayılan ifadelerde zaten yoksa aşağıdaki `using` deyimi ekleyin.

    ```csharp
    using System.Threading;
    using System.Threading.Tasks;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer [tutucuları, IoT hub bağlantı dizesini](#get-the-iot-hub-connection-string) ve cihazınızın adını al'da daha önce kopyaladığınız IoT Hub bağlantı dizesiyle değiştirin.

    ```csharp
    static JobClient jobClient;
    static string connString = "<yourIotHubConnectionString>";
    static string deviceId = "<yourDeviceId>";
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

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

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

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

1. **Program** sınıfına başka bir yöntem ekleyin:

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
    > Sorgu sözdizimi hakkında daha fazla bilgi için [IoT Hub sorgu dili'ne](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-query-language)bakın.
    >

1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

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

1. Çalışmanızı kaydedin ve çözümünüzü oluşturun.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Visual Studio Solution Explorer'da, çözümünüzü sağ tıklatın ve ardından **Başlangıç Projeleri Ayarla'yı**seçin.

1. **Ortak Özellikler** > **Başlangıç Projesi'ni**seçin ve ardından Birden Çok başlangıç **projesi**seçin.

1. Listenin `SimulateDeviceMethods` en üstünde olduğundan emin `ScheduleJob`olun ve ardından . Her iki eylemini **başlat** ve **Tamam'ı**seçecek şekilde ayarlayın.

1. **Başlat'ı** tıklatarak projeleri çalıştırın veya **Hata Ayıklama** menüsüne gidin ve **Hata Ayıklama'yı Başlat'ı**tıklatın.

   Hem aygıttan hem de arka uç uygulamalarından çıktı görürsünüz.

    ![İşleri zamanlamak için uygulamaları çalıştırın](./media/iot-hub-csharp-csharp-schedule-jobs/schedule-jobs-console-results.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir aygıta doğrudan bir yöntem ve aygıt ikizözelliklerinin güncelliğini zamanlamak için bir iş kullandınız.

* IoT Hub ve hava firmware güncellemesi üzerinden uzaktan gibi cihaz yönetimi desenleri ile başlarken devam [etmek için, Öğretici okuyun: Nasıl bir firmware güncelleştirmesi yapmak](tutorial-firmware-update.md).

* Azure IoT Edge'e sahip aygıtlara AI dağıtma hakkında bilgi edinmek için [bkz.](../iot-edge/tutorial-simulate-device-linux.md)
