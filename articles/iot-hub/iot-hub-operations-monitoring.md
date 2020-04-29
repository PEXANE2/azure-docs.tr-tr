---
title: Azure IoT Hub işlemleri izleme (kullanım dışı) | Microsoft Docs
description: IoT Hub 'ınızdaki işlemlerin durumunu gerçek zamanlı olarak izlemek için Azure IoT Hub işlemler izleme 'yi kullanma.
author: nberdy
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: nberdy
ms.custom: amqp
ms.openlocfilehash: edbc3431c860794c7cd1dd8e5011c0d7d11d692d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81732230"
---
# <a name="iot-hub-operations-monitoring-deprecated"></a>IoT Hub işlemleri izleme (kullanım dışı)

IoT Hub işlemler izleme, IoT Hub 'ınızdaki işlemlerin durumunu gerçek zamanlı olarak izlemenize olanak sağlar. IoT Hub birkaç işlem kategorisi içindeki olayları izler. İşlemek için bir veya daha fazla kategoriden bir veya daha fazla kategorinin bir uç noktasına olay göndermeyi tercih edebilirsiniz. Verileri hatalara göre izleyebilir veya veri desenlerine göre daha karmaşık işleme ayarlayabilirsiniz.

>[!NOTE]
>IoT Hub **işlemleri izleme kullanım dışıdır ve 10 mart 2019 ' de IoT Hub kaldırılmıştır**. IoT Hub işlemlerini ve durumunu izlemek için bkz. [Azure IoT Hub sistem durumunu izleme ve sorunları hızlı bir şekilde tanılama](iot-hub-monitor-resource-health.md). Kullanımdan kaldırma zaman çizelgesi hakkında daha fazla bilgi için bkz. Azure [izleme Ile Azure IoT çözümlerinizi izleme ve Azure Kaynak durumu](https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health).

IoT Hub altı olay kategorisini izler:

* Cihaz kimliği işlemleri
* Cihaz telemetrisi
* Buluttan cihaza iletiler
* Bağlantılar
* Dosya karşıya yüklemeleri
* İleti yönlendirme

> [!IMPORTANT]
> IoT Hub işlemler izleme, olayların güvenilir veya sıralı teslimini garanti etmez. IoT Hub temel alınan altyapıya bağlı olarak, bazı olaylar kaybolmuş veya sıra dışında teslim edilebilir. Başarısız bağlantı girişimleri veya belirli cihazlar için yüksek frekanslı bağlantılar gibi hata sinyallerine göre uyarı oluşturmak için işlem izlemeyi kullanın. Cihaz durumu için tutarlı bir mağaza oluşturmak üzere işlem izleme olaylarına güvenmemelisiniz, örn. bir cihazın bağlı veya bağlantısı kesik durumunda olan bir depolama alanı. 

## <a name="how-to-enable-operations-monitoring"></a>İşlem izlemeyi etkinleştirme

1. IoT Hub 'ı oluşturun. [Başlarken](quickstart-send-telemetry-dotnet.md) kılavuzunda IoT Hub 'ı oluşturma yönergelerini bulabilirsiniz.

2. IoT Hub 'ınızın dikey penceresini açın. Buradan, **işlemler izleme**' ye tıklayın.

    ![Portalda erişim işlemleri izleme yapılandırması](./media/iot-hub-operations-monitoring/enable-OM-1.png)

3. İzlemek istediğiniz izleme kategorilerini seçin ve ardından **Kaydet**' e tıklayın. Olaylar, **İzleme ayarlarında**listelenen olay hub 'ı ile uyumlu uç noktadan okumak için kullanılabilir. IoT Hub uç noktası çağrılır `messages/operationsmonitoringevents`.

    ![IoT Hub 'ınızda işlem izlemeyi yapılandırma](./media/iot-hub-operations-monitoring/enable-OM-2.png)

> [!NOTE]
> **Bağlantılar** kategorisi için **ayrıntılı** izleme seçildiğinde IoT Hub ek tanılama iletileri oluşturulmasına neden olur. Tüm diğer kategoriler için, **ayrıntılı** ayar her bir hata iletisindeki IoT Hub dahil edilen bilgi miktarını değiştirir.

## <a name="event-categories-and-how-to-use-them"></a>Olay kategorileri ve bunların nasıl kullanılacağı

Her bir işlem izleme kategorisi IoT Hub ile farklı bir etkileşim türünü izler ve her izleme kategorisinin bu kategorideki olayların nasıl yapılandırıldığını tanımlayan bir şeması vardır.

### <a name="device-identity-operations"></a>Cihaz kimliği işlemleri

Cihaz kimliği işlemler kategorisi, IoT Hub 'ının kimlik kayıt defterinde bir girişi oluşturmaya, güncelleştirmeye veya silmeye çalıştığınızda oluşan hataları izler. Bu kategoriyi izlemek, senaryoları sağlamak için faydalıdır.

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

Cihaz telemetri kategorisi, IoT Hub 'ında oluşan hataları izler ve telemetri işlem hattı ile ilgilidir. Bu kategori telemetri olayları gönderilirken (daraltma gibi) ve telemetri olaylarının (yetkisiz okuyucu gibi) alınması sırasında oluşan hataları içerir. Bu kategori, cihazın kendisinde çalışan kodun neden olduğu hataları yakalayamaz.

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

### <a name="cloud-to-device-commands"></a>Buluttan cihaza komutlar

Buluttan cihaza komutlar kategorisi, IoT Hub 'ında oluşan ve buluttan cihaza ileti işlem hattı ile ilgili hataları izler. Bu kategori, buluttan cihaza iletiler gönderilirken (yetkisiz gönderici gibi), buluttan cihaza iletileri alırken (örneğin, teslim sayısı aşıldığında) ve buluttan cihaza ileti geri bildirimi (örneğin, geri bildirim süre sonu) alırken oluşan hataları içerir. Bu kategori, buluttan cihaza ileti başarıyla teslim edildiğinde, buluttan cihaza iletileri hatalı şekilde işleyen bir cihazdaki hataları yakalamaz.

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

Bağlantılar kategorisi, cihazların bir IoT Hub 'ına bağlanması veya bağlantısı kesildiğinde oluşan hataları izler. Bu kategoriyi izlemek, yetkisiz bağlantı girişimlerini belirlemek ve düşük bağlantı alanlarında cihazlar için bir bağlantı kesildiğinde izlemek için yararlıdır.

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

Karşıya dosya yükleme kategorisi, IoT Hub 'ında oluşan ve karşıya dosya yükleme işlevselliğiyle ilgili hataları izler. Bu kategori şunları içerir:

* Bir cihazın, tamamlanmış bir karşıya yükleme merkezine bildirim göndermeden önce ne zaman dolacağını belirten SAS URI 'SI ile oluşan hatalar.

* Cihaz tarafından raporlanan karşıya yüklemeler başarısız oldu.

* IoT Hub bildirim iletisi oluşturma sırasında depolamada bir dosya bulunamadığında oluşan hatalar.

Bu kategori, cihaz bir dosyayı depoya yüklerken doğrudan oluşan hataları yakalayamaz.

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

İleti yönlendirme kategorisi, IoT Hub tarafından algılanan ileti yolu değerlendirmesi ve uç nokta durumu sırasında oluşan hataları izler. Bu kategori, bir kuralın "tanımsız" olarak değerlendirildiği, IoT Hub bir uç noktayı ölü olarak işaretlediğinde ve bir uç noktadan alınan diğer hatalardan oluşan olayları içerir. Bu kategori, "cihaz telemetrisi" kategorisi altında bildirilen iletiler hakkındaki belirli hataları (cihaz azaltma hataları gibi) içermez.

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

## <a name="connect-to-the-monitoring-endpoint"></a>İzleme uç noktasına Bağlan

IoT Hub 'ınızdaki izleme uç noktası, Olay Hub 'ı ile uyumlu bir uç noktasıdır. Bu uç noktadan izleme iletilerini okumak için, Event Hubs ile birlikte çalışarak herhangi bir mekanizmayı kullanabilirsiniz. Aşağıdaki örnek, yüksek aktarım hızı dağıtımı için uygun olmayan temel bir okuyucu oluşturur. Event Hubs'dan iletilerin nasıl işleneceği hakkında daha fazla bilgi için [Event Hubs ile Çalışmaya Başlama](../event-hubs/event-hubs-csharp-ephcs-getstarted.md) öğreticisine bakın.

İzleme uç noktasına bağlanmak için bir bağlantı dizesi ve uç nokta adı gereklidir. Aşağıdaki adımlarda, portalda gerekli değerleri nasıl bulacağınız gösterilmektedir:

1. Portalda IoT Hub kaynak dikey penceresine gidin.

2. **İşlem izleme**' yi seçin ve **Event hub ile uyumlu adı** ve **Event hub ile uyumlu uç nokta** değerlerini bir yere göz önünde oluşturun:

    ![Olay Hub 'ı ile uyumlu uç nokta değerleri](./media/iot-hub-operations-monitoring/monitoring-endpoint.png)

3. **Paylaşılan erişim ilkeleri**' ni ve ardından **hizmet**' i seçin. **Birincil anahtar** değerini bir yere getirin:

    ![Hizmet paylaşılan erişim ilkesi birincil anahtarı](./media/iot-hub-operations-monitoring/service-key.png)

Aşağıdaki C# kod örneği, Visual Studio **Windows Klasik Masaüstü** C# konsol uygulamasından alınmıştır. Projede **windowsazure. ServiceBus** NuGet paketi yüklüdür.

* Bağlantı dizesi yer tutucusunu, aşağıdaki örnekte gösterildiği gibi, daha önce not ettiğiniz **Olay Hub 'ı ile uyumlu uç noktası** ve hizmet **birincil anahtar** değerlerini kullanan bir bağlantı dizesiyle değiştirin:

    ```csharp
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* İzleme uç noktası adı yer tutucusunu, daha önce not ettiğiniz **Olay Hub 'ı ile uyumlu ad** değeriyle değiştirin.

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

IoT Hub yeteneklerini daha fazla incelemek için bkz.:

* [IoT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)