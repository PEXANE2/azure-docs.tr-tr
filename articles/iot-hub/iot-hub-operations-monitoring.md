---
title: Azure IoT Hub işlemleri izleme (amortismana uğradı) | Microsoft Dokümanlar
description: IoT hub'ınızdaki işlemlerin durumunu gerçek zamanlı olarak izlemek için Azure IoT Hub işlemleri izleme nasıl kullanılır?
author: nberdy
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: nberdy
ms.custom: amqp
ms.openlocfilehash: edbc3431c860794c7cd1dd8e5011c0d7d11d692d
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732230"
---
# <a name="iot-hub-operations-monitoring-deprecated"></a>IoT Hub operasyonları izleme (amortismana uğradı)

IoT Hub işlemleri izleme, IoT hub'ınızdaki operasyonların durumunu gerçek zamanlı olarak izlemenize olanak tanır. IoT Hub, çeşitli işlem kategorileri boyunca olayları izler. Bir veya daha fazla kategoriden işlenmek üzere IoT hub'ınızın bitiş noktasına etkinlik göndermeyi tercih edebilirsiniz. Verileri hatalar için izleyebilir veya veri desenlerine göre daha karmaşık bir işleme ayarlayabilirsiniz.

>[!NOTE]
>IoT Hub operasyonları izleme işlemi **amortismana kaldırılmış ve 10 Mart 2019 tarihinde IoT Hub'dan kaldırılmıştır.** IoT Hub'ın işlemlerini ve sistem durumunu izlemek için [Azure IoT Hub'ının durumunu izleyin ve sorunları hızlı bir şekilde tanıla.](iot-hub-monitor-resource-health.md) Amortisman zaman çizelgesi hakkında daha fazla bilgi için Azure [IoT çözümlerinizi Azure Monitor ve Azure Kaynak Durumu ile izleyin'e](https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health)bakın.

IoT Hub altı etkinlik kategorisini izler:

* Cihaz kimlik işlemleri
* Cihaz telemetrisi
* Buluttan cihaza iletiler
* Bağlantılar
* Dosya karşıya yüklemeleri
* İleti yönlendirme

> [!IMPORTANT]
> IoT Hub operasyonlarının izlenmesi, olayların güvenilir veya sıralı bir şekilde teslim edildiğini garanti etmez. IoT Hub'ın temel altyapısına bağlı olarak, bazı olaylar kaybolabilir veya sıra dışı teslim edilebilir. Başarısız bağlantı denemeleri veya belirli aygıtlar için yüksek frekanslı bağlantı bağlantıları gibi hata sinyallerine dayalı uyarılar oluşturmak için operasyon izleme yi kullanın. Aygıt durumu için tutarlı bir mağaza oluşturmak için operasyon izleme olaylarına güvenmemelisiniz (örneğin, bir aygıtın bağlı veya bağlantısı zıddı olan bir mağaza izleme durumu). 

## <a name="how-to-enable-operations-monitoring"></a>Operasyon izleme nasıl etkinleştirilir?

1. Bir IoT hub'ı oluşturun. [Başlangıç](quickstart-send-telemetry-dotnet.md) kılavuzunda bir IoT hub'ı oluşturma yla ilgili yönergeleri bulabilirsiniz.

2. IoT hub'ınızın bıçağını açın. Buradan, **Operations izleme'yi**tıklatın.

    ![Portaldaki erişim işlemleri izleme yapılandırması](./media/iot-hub-operations-monitoring/enable-OM-1.png)

3. İzlemek istediğiniz izleme kategorilerini seçin ve ardından **Kaydet'i**tıklatın. Olaylar, **İzleme ayarlarında**listelenen Event Hub uyumlu bitiş noktasından okumak için kullanılabilir. IoT Hub bitiş noktası `messages/operationsmonitoringevents`denir.

    ![IoT hub'ınızda operasyon izleme yi yapılandırma](./media/iot-hub-operations-monitoring/enable-OM-2.png)

> [!NOTE]
> **Bağlantılar** kategorisi için **Verbose** izleme nin seçilmesi, IoT Hub'ın ek tanılama iletileri oluşturmasına neden olur. Diğer tüm kategoriler için **Verbose** ayarı, IoT Hub'ın her hata iletisinde içerdiği bilgi miktarını değiştirir.

## <a name="event-categories-and-how-to-use-them"></a>Etkinlik kategorileri ve bunların nasıl kullanılacağı

Her operasyon izleme kategorisi IoT Hub ile farklı bir etkileşim türünü izler ve her izleme kategorisinde bu kategorideki olayların nasıl yapılandırılacağını tanımlayan bir şema vardır.

### <a name="device-identity-operations"></a>Cihaz kimlik işlemleri

Aygıt kimlik işlemleri kategorisi, IoT hub'ınızın kimlik kayıt defterinde bir giriş oluşturmaya, güncelleştirmeye veya silmeye çalıştığınızda oluşan hataları izler. Bu kategoriyi izlemek, senaryoları sağlamada yararlıdır.

```json
{
    "time": "UTC timestamp",
    "operationName": "create",
    "category": "DeviceIdentityOperations",
    "level": "Error",
    "statusCode": 4XX,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "durationMs": 1234,
    "userAgent": "userAgent",
    "sharedAccessPolicy": "accessPolicy"
}
```

### <a name="device-telemetry"></a>Cihaz telemetrisi

Aygıt telemetri seçimleri kategorisi, IoT hub' ında meydana gelen ve telemetri ardısıyla ilgili hataları izler. Bu kategori, telemetri olayları gönderirken (azaltma gibi) ve telemetri olayları (yetkisiz okuyucu gibi) alırken oluşan hataları içerir. Bu kategori, aygıtın kendisinde çalışan koddan kaynaklanan hataları yakalayamıyor.

```json
{
    "messageSizeInBytes": 1234,
    "batching": 0,
    "protocol": "Amqp",
    "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "DeviceTelemetry",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="cloud-to-device-commands"></a>Bulut-aygıt komutları

Buluttan aygıta komutlar kategorisi, IoT hub'ında oluşan hataları izler ve buluttan aygıta ileti ardışık ardışık olayla ilişkilidir. Bu kategori, buluttan aygıta iletiler gönderirken (yetkisiz gönderen gibi), buluttan cihaza iletiler alırken (teslim sayısı aşıldı) ve buluttan cihaza ileti geri bildirimi (geri bildirim süresi dolmuş gibi) alan hataları içerir. Bu kategori, buluttan aygıta ileti başarıyla teslim edildiyse, buluttan aygıta iletiyi yanlış işleyen bir aygıttan gelen hataları yakalamaz.

```json
{
    "messageSizeInBytes": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "deliveryAcknowledgement": 0,
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "C2DCommands",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="connections"></a>Bağlantılar

Bağlantılar kategorisi, aygıtlar bir IoT hub'ına bağlandığında veya bağlantıkesildiğinda oluşan hataları izler. Bu kategoriyi izlemek, yetkisiz bağlantı girişimlerini tanımlamak ve bağlantının zayıf bağlantı alanlarındaki aygıtlar için kaybedildiğini izlemek için yararlıdır.

```json
{
    "durationMs": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "deviceConnect",
    "category": "Connections",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID"
}
```

### <a name="file-uploads"></a>Dosya karşıya yüklemeleri

Dosya yükleme kategorisi, IoT hub'ında oluşan hataları izler ve dosya yükleme işleviyle ilgilidir. Bu kategori şunları içerir:

* SAS URI ile oluşan hatalar ( örneğin, aygıtın tamamlanmış yükleme nin hub'ını bildiriminde önce süresi dolduğunda olduğu gibi.

* Aygıt tarafından bildirilen başarısız yüklemeler.

* IoT Hub bildirim iletisi oluşturma sırasında depolama alanında bir dosya bulunamadığında oluşan hatalar.

Bu kategori, aygıt bir dosyayı depolama alanına yüklerken doğrudan oluşan hataları yakalayamaz.

```json
{
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "HTTP",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "fileUpload",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "blobUri": "http//bloburi.com",
    "durationMs": 1234
}
```

### <a name="message-routing"></a>İleti yönlendirme

İleti yönlendirme kategorisi, ileti rotası değerlendirmesi ve uç nokta durumu sırasında ioT Hub tarafından algılanan hataları izler. Bu kategori, bir kuralın "tanımsız" olarak değerlendirilmesi, IoT Hub'ın bitiş noktasını ölü olarak işaretlediğinde ve bitiş noktasından alınan diğer hatalar gibi olayları içerir. Bu kategori, "aygıt telemetrisi" kategorisi altında bildirilen iletilerin kendileri (aygıt azaltma hataları gibi) ile ilgili belirli hataları içermez.

```json
{
    "messageSizeInBytes": 1234,
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "routes",
    "level": "Error",
    "deviceId": "device-ID",
    "messageId": "ID of message",
    "routeName": "myroute",
    "endpointName": "myendpoint",
    "details": "ExternalEndpointDisabled"
}
```

## <a name="connect-to-the-monitoring-endpoint"></a>İzleme bitiş noktasına bağlanma

IoT hub'ınızdaki izleme bitiş noktası, Event Hub uyumlu bir bitiş noktasıdır. Bu bitiş noktasından izleme iletilerini okumak için Olay Hub'larıyla çalışan herhangi bir mekanizmayı kullanabilirsiniz. Aşağıdaki örnek, yüksek iş elde dağıtımı için uygun olmayan temel bir okuyucu oluşturur. Event Hubs'dan iletilerin nasıl işleneceği hakkında daha fazla bilgi için [Event Hubs ile Çalışmaya Başlama](../event-hubs/event-hubs-csharp-ephcs-getstarted.md) öğreticisine bakın.

İzleme bitiş noktasına bağlanmak için bir bağlantı dizesi ve bitiş noktası adı gerekir. Aşağıdaki adımlar, portalda gerekli değerleri nasıl bulacağınızı gösterir:

1. Portalda, IoT Hub kaynak bıçağınıza gidin.

2. **Operations izlemeyi**seçin ve Event **Hub uyumlu ad** ve Event Hub uyumlu uç **nokta** değerlerini not edin:

    ![Olay Hub uyumlu uç nokta değerleri](./media/iot-hub-operations-monitoring/monitoring-endpoint.png)

3. **Paylaşılan erişim ilkelerini**seçin, ardından **hizmet**seçin. **Birincil anahtar** değerine dikkat edin:

    ![Hizmet paylaşılan erişim ilkesi birincil anahtar](./media/iot-hub-operations-monitoring/service-key.png)

Aşağıdaki C# kod örneği Visual Studio **Windows Classic Desktop** C# konsol uygulamasından alınmıştır. Proje **windowsazure.ServiceBus** NuGet paketi yüklü vardır.

* Bağlantı dizesini yer tutucuyu, aşağıdaki örnekte gösterildiği gibi daha önce belirttiğiniz **Event Hub uyumlu bitiş noktası** ve hizmet Birincil **anahtar** değerlerini kullanan bir bağlantı dizesiyle değiştirin:

    ```csharp
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* İzleme uç nokta adı yer tutucuyu daha önce belirttiğiniz **Event Hub uyumlu ad** değeriyle değiştirin.

```csharp
class Program
{
    static string connectionString = "{your monitoring endpoint connection string}";
    static string monitoringEndpointName = "{your monitoring endpoint name}";
    static EventHubClient eventHubClient;

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;
        CancellationTokenSource cts = new CancellationTokenSource();
        var tasks = new List<Task>();

        foreach (string partition in d2cPartitions)
        {
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }

        Console.ReadLine();
        Console.WriteLine("Exiting...");
        cts.Cancel();
        Task.WaitAll(tasks.ToArray());
    }

    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested)
            {
                await eventHubReceiver.CloseAsync();
                break;
            }

            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10));

            if (eventData != null)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [IoT Hub geliştirici kılavuzu](iot-hub-devguide.md)

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)