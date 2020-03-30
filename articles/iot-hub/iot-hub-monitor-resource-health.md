---
title: Azure IoT Hub'ınızın sistem durumunu izleyin | Microsoft Dokümanlar
description: IoT Hub'ınızı izlemek ve sorunları hızlı bir şekilde tanılamak için Azure Monitörü ve Azure Kaynak Durumu'nun kullanımını kullanın
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: kgremban
ms.openlocfilehash: f801abc40caf273c28a0c01dedf9735f5198c2af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271089"
---
# <a name="monitor-the-health-of-azure-iot-hub-and-diagnose-problems-quickly"></a>Azure IoT Hub durumunu izleyin ve sorunları hızla tanılayın

Azure IoT Hub'ını uygulayan işletmeler kaynaklarından güvenilir performans bekler. IoT Hub, işlemlerinizi yakından izlemenize yardımcı olmak için [Azure Monitor](../azure-monitor/index.yml) ve [Azure Kaynak Durumu](../service-health/resource-health-overview.md)ile tamamen entegre edilmiştir. Bu iki hizmet, IoT çözümlerinizi sağlıklı bir durumda çalışır durumda tutmak için ihtiyacınız olan verileri sağlamak için çalışır.

Azure Monitor, tüm Azure hizmetleriniz için tek bir izleme ve günlüğe kaydetme kaynağıdır. Azure Monitor'un oluşturduğu tanı günlüklerini özel işleme için Azure Monitor günlüklerine, Etkinlik Hub'larına veya Azure Depolama'ya gönderebilirsiniz. Azure Monitor'un ölçüm ve tanılama ayarları, kaynaklarınızın performansında görünürlük sağlar. IoT hub'ınızla Azure Monitör'ü nasıl [kullanacağınızı](#use-azure-monitor) öğrenmek için bu makaleyi okumaya devam edin. 

> [!IMPORTANT]
> Azure Monitor tanı günlüklerini kullanarak IoT Hub hizmeti tarafından yayılan olayların güvenilir veya düzenli olduğu garanti edilmez. Bazı olaylar kaybolabilir veya sıra dışı teslim edilebilir. Tanılama günlüklerinin gerçek zamanlı olması da beklenmiyor ve olayların seçtiğiniz hedefe kaydedilmesi birkaç dakika sürebilir.

Azure Kaynak Durumu, bir Azure sorunu kaynaklarınızı etkilediğinde tanılamanıza ve destek almanıza yardımcı olur. Pano, IoT hub'larınızın her biri için geçerli ve geçmiş sistem durumu durumu sağlar. IoT hub'ınızla Azure Kaynak Durumu'nu nasıl [kullanacağınızı](#use-azure-resource-health) öğrenmek için bu makalenin altındaki bölüme devam edin. 

IoT Hub, IoT kaynaklarınızın durumunu anlamak için kullanabileceğiniz kendi ölçümlerini de sağlar. Daha fazla bilgi için [Bkz. IoT Hub ölçümlerini anlayın.](iot-hub-metrics.md)

## <a name="use-azure-monitor"></a>Azure İzleyici’yi kullanma

Azure Monitor, Azure kaynakları için tanılama bilgileri sağlar, bu da IoT hub'ınızda gerçekleşen işlemleri izleyebileceğiniz anlamına gelir.

Azure Monitor'un tanılama ayarları, IoT Hub işlemleri monitörünyerini alır. Şu anda işlem izleme kullanıyorsanız, iş akışlarınızı geçirmeniz gerekir. Daha fazla bilgi için bkz: [Operasyon izlemeden tanılama ayarlarına geçir.](iot-hub-migrate-to-diagnostics-settings.md)

Azure Monitor'un izlediği belirli ölçümler ve etkinlikler hakkında daha fazla bilgi edinmek için Azure Monitor ve [Desteklenen hizmetler, şemalar ve Azure Tanı Günlükleri için kategorileriçeren](../azure-monitor/platform/diagnostic-logs-schema.md) [Desteklenen ölçümlere](../azure-monitor/platform/metrics-supported.md) bakın.

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="understand-the-logs"></a>Günlükleri anlama

Azure Monitor, IoT Hub'da gerçekleşen farklı işlemleri izler. Her kategoride, bu kategorideki olayların nasıl raporlandığını tanımlayan bir şema vardır.

#### <a name="connections"></a>Bağlantılar

Bağlantılar kategorisi, aygıt bağlantısını izler ve olayları bir IoT hub'ından ve hataların bağlantısını keser. Bu kategori, yetkisiz bağlantı girişimlerini tanımlamak ve aygıtlara bağlantınızı kaybettiğinde uyarmak için yararlıdır.

> [!NOTE]
> Aygıtların güvenilir bağlantı durumu için [Aygıt sinyalatışını](iot-hub-devguide-identity-registry.md#device-heartbeat)kontrol edin.

```json
{
   "records":
   [
        {
            "time": " UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "deviceConnect",
            "category": "Connections",
            "level": "Information",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"maskedIpAddress\":\"<maskedIpAddress>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="cloud-to-device-commands"></a>Bulut-aygıt komutları

Buluttan aygıta komutlar kategorisi, IoT hub'ında oluşan hataları izler ve buluttan aygıta ileti ardışık ardışık olayla ilişkilidir. Bu kategori, şunlardan kaynaklanan hataları içerir:

* Buluttan aygıta ileti gönderme (yetkisiz gönderen hataları gibi),
* Buluttan aygıta iletiler alma (teslim sayısı aşıldı hataları gibi) ve
* Buluttan cihaza ileti geri bildirimi alma (süresi dolmuş geri bildirim hataları gibi).

Bu kategori, buluttan aygıta ileti başarılı bir şekilde teslim edildiğinde hataları yakalamaz, ancak aygıt tarafından yanlış şekilde işlenir.

```json
{
    "records":
    [
        {
            "time": " UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "messageExpired",
            "category": "C2DCommands",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"deviceId\":\"<deviceId>\",\"messageId\":\"<messageId>\",\"messageSizeInBytes\":\"<messageSize>\",\"protocol\":\"Amqp\",\"deliveryAcknowledgement\":\"<None, NegativeOnly, PositiveOnly, Full>\",\"deliveryCount\":\"0\",\"expiryTime\":\"<timestamp>\",\"timeInSystem\":\"<timeInSystem>\",\"ttl\":<ttl>, \"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\", \"maskedIpAddress\": \"<maskedIpAddress>\", \"statusCode\": \"4XX\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-identity-operations"></a>Cihaz kimlik işlemleri

Aygıt kimlik işlemleri kategorisi, IoT hub'ınızın kimlik kayıt defterinde bir giriş oluşturmaya, güncelleştirmeye veya silmeye çalıştığınızda oluşan hataları izler. Bu kategoriyi izlemek, senaryoları sağlamada yararlıdır.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "get",
            "category": "DeviceIdentityOperations",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"maskedIpAddress\":\"<maskedIpAddress>\",\"deviceId\":\"<deviceId>\", \"statusCode\":\"4XX\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="routes"></a>Yollar

İleti yönlendirme kategorisi, ileti rotası değerlendirmesi ve uç nokta durumu sırasında ioT Hub tarafından algılanan hataları izler. Bu kategori, şunlar gibi olayları içerir:

* Bir kural "tanımsız" olarak değerlendirir,
* IoT Hub ölü olarak bir bitiş noktası işaretler, ya da
* Bitiş noktasından alınan hatalar. 

Bu kategori, "aygıt telemetrisi" kategorisi altında bildirilen iletilerin kendileri (aygıt azaltma hataları gibi) ile ilgili belirli hataları içermez.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "endpointUnhealthy",
            "category": "Routes",
            "level": "Error",
            "properties": "{\"deviceId\": \"<deviceId>\",\"endpointName\":\"<endpointName>\",\"messageId\":<messageId>,\"details\":\"<errorDetails>\",\"routeName\": \"<routeName>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-telemetry"></a>Cihaz telemetrisi

Aygıt telemetri seçimleri kategorisi, IoT hub' ında meydana gelen ve telemetri ardısıyla ilgili hataları izler. Bu kategori, telemetri olayları gönderirken (azaltma gibi) ve telemetri olayları (yetkisiz okuyucu gibi) alırken oluşan hataları içerir. Bu kategori, aygıtın kendisinde çalışan koddan kaynaklanan hataları yakalayamıyor.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "ingress",
            "category": "DeviceTelemetry",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"deviceId\":\"<deviceId>\",\"batching\":\"0\",\"messageSizeInBytes\":\"<messageSizeInBytes>\",\"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\",\"partitionId\":\"1\"}", 
            "location": "Resource location"
        }
    ]
}
```

#### <a name="file-upload-operations"></a>Dosya karşıya yükleme işlemleri

Dosya yükleme kategorisi, IoT hub'ında oluşan hataları izler ve dosya yükleme işleviyle ilgilidir. Bu kategori şunları içerir:

* SAS URI ile oluşan hatalar ( örneğin, aygıtın tamamlanmış yükleme nin hub'ını bildiriminde önce süresi dolduğunda olduğu gibi.

* Aygıt tarafından bildirilen başarısız yüklemeler.

* IoT Hub bildirim iletisi oluşturma sırasında depolama alanında bir dosya bulunamadığında oluşan hatalar.

Bu kategori, aygıt bir dosyayı depolama alanına yüklerken doğrudan oluşan hataları yakalayamaz.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "ingress",
            "category": "FileUploadOperations",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"blobUri\":\"http//bloburi.com\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="cloud-to-device-twin-operations"></a>Bulut-aygıtikiz işlemleri

Bulut-aygıta çift işlemler kategorisi, aygıt ikizleri üzerinde hizmet tarafından başlatılan olayları izler. Bu işlemler ikiz alma, güncelleştirme veya etiketleri değiştirme ve istenen özellikleri güncelleştirmeyi veya değiştirmeyi içerebilir.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "read",
            "category": "C2DTwinOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-to-cloud-twin-operations"></a>Aygıt-buluta ikiz işlemleri

Aygıt-buluta ikiz işlemler kategorisi, aygıt ikizleri üzerinde aygıt tarafından başlatılan olayları izler. Bu işlemler ikiz almak, bildirilen özellikleri güncelleştirmek ve istenen özelliklere abone içerebilir.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "update",
            "category": "D2CTwinOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authenticationType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="twin-queries"></a>İkiz sorgular

İkiz sorgular kategorisi, bulutta başlatılan aygıt ikizlerine yönelik sorgu isteklerini bildirir.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "query",
            "category": "TwinQueries",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"query\":\"<twin query>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\",\"pageSize\":\"<pageSize>\", \"continuation\":\"<true, false>\", \"resultSize\":\"<resultSize>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="jobs-operations"></a>İş işlemleri

İş işlemleri kategorisi, aygıt ikizlerini güncelleştirmek veya birden çok cihazda doğrudan yöntemler çağırmak için iş isteklerini bildirir. Bu istekler bulutta başlatılır.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "jobCompleted",
            "category": "JobsOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"jobId\":\"<jobId>\", \"sdkVersion\": \"<sdkVersion>\",\"messageSize\": <messageSize>,\"filter\":\"DeviceId IN ['1414ded9-b445-414d-89b9-e48e8c6285d5']\",\"startTimeUtc\":\"Wednesday, September 13, 2017\",\"duration\":\"0\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="direct-methods"></a>Doğrudan Yöntemler

Doğrudan yöntemler kategorisi, tek tek aygıtlara gönderilen istek-yanıt etkileşimlerini izler. Bu istekler bulutta başlatılır.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "send",
            "category": "DirectMethods",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":<messageSize>, \"RequestSize\": 1, \"ResponseSize\": 1, \"sdkVersion\": \"2017-07-11\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="distributed-tracing-preview"></a>Dağıtılmış İzleme (Önizleme)

Dağıtılmış izleme kategorisi, izleme bağlamı üstbilgisini taşıyan iletilerin korelasyon iliklerini izler. Bu günlükleri tam olarak etkinleştirmek için, istemci tarafı kodu Çözümle'yi izleyerek güncelleştirilmeli [ve IoT Hub dağıtılmış izleme (önizleme) ile IoT uygulamalarını uçtan uca](iot-hub-distributed-tracing.md)tanılamalıdır.

`correlationId` [W3C İzleme Bağlamı](https://github.com/w3c/trace-context) önerisine uygun olduğunu unutmayın, burada `trace-id` `span-id`bir de .

##### <a name="iot-hub-d2c-device-to-cloud-logs"></a>IoT Hub D2C (aygıt-bulut) günlükleri

IoT Hub, geçerli izleme özellikleri içeren bir ileti IoT Hub'a geldiğinde bu günlüğü kaydeder.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubD2C",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-0144d2590aacd909-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Receive message success",
            "durationMs": "",
            "properties": "{\"messageSize\": 1, \"deviceId\":\"<deviceId>\", \"callerLocalTimeUtc\": : \"2017-02-22T03:27:28.633Z\", \"calleeLocalTimeUtc\": \"2017-02-22T03:27:28.687Z\"}",
            "location": "Resource location"
        }
    ]
}
```

Burada, `durationMs` IoT Hub'ın saati aygıt saati ile eşitlenmemiş olabileceğinden hesaplanmaz ve böylece bir süre hesaplaması yanıltıcı olabilir. Aygıt-bulut gecikmesi ndeki ani artışları yakalamak için `properties` bölümdeki zaman damgalarını kullanarak mantık yazmanızı öneririz.

| Özellik | Tür | Açıklama |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **messageBoyut** | Tamsayı | Baytlarda aygıttan buluta iletinin boyutu |
| **deviceId** | ASCII 7-bit alfasayısal karakter dizesi | Aygıtın kimliği |
| **arayanLocalTimeUtc** | UTC zaman damgası | Aygıt yerel saat tarafından bildirilen iletinin oluşturma süresi |
| **calleeLocalTimeUtc** | UTC zaman damgası | IoT Hub hizmet yan saati tarafından bildirilen IoT Hub'ın ağ geçidine ileti varış saati |

##### <a name="iot-hub-ingress-logs"></a>IoT Hub giriş günlükleri

IoT Hub, geçerli izleme özellikleri içeren ileti dahili veya yerleşik Olay Hub'ına yazdığında bu günlüğü kaydeder.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubIngress",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-349810a9bbd28730-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Ingress message success",
            "durationMs": "10",
            "properties": "{\"isRoutingEnabled\": \"true\", \"parentSpanId\":\"0144d2590aacd909\"}",
            "location": "Resource location"
        }
    ]
}
```

`properties` Bölümde, bu günlük ileti girişi hakkında ek bilgiler içerir.

| Özellik | Tür | Açıklama |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **isRoutingEtkin** | Dize | Doğru veya yanlış, IoT Hub'ında ileti yönlendirmesinin etkin olup olmadığını gösterir |
| **ebeveynSpanId** | Dize | Bu durumda D2C iletisi izleme olacak ana iletinin [yayılma kimliği](https://w3c.github.io/trace-context/#parent-id) |

##### <a name="iot-hub-egress-logs"></a>IoT Hub çıkış günlükleri

Yönlendirme [etkinleştirildiğinde](iot-hub-devguide-messages-d2c.md) ve ileti [bitiş noktasına](iot-hub-devguide-endpoints.md)yazıldığında IoT Hub bu günlüğü kaydeder. Yönlendirme etkin değilse, IoT Hub bu günlüğü kaydetmez.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubEgress",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-98ac3578922acd26-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Egress message success",
            "durationMs": "10",
            "properties": "{\"endpointType\": \"EventHub\", \"endpointName\": \"myEventHub\", \"parentSpanId\":\"349810a9bbd28730\"}",
            "location": "Resource location"
        }
    ]
}
```

`properties` Bölümde, bu günlük ileti girişi hakkında ek bilgiler içerir.

| Özellik | Tür | Açıklama |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **endpointName** | Dize | Yönlendirme bitiş noktasının adı |
| **endpointType** | Dize | Yönlendirme bitiş noktasının türü |
| **ebeveynSpanId** | Dize | Bu durumda IoT Hub giriş iletisi izleme olacak üst iletinin [yayılma kimliği](https://w3c.github.io/trace-context/#parent-id) |

#### <a name="configurations"></a>Yapılandırmalar

IoT Hub yapılandırma günlükleri, Otomatik Aygıt Yönetimi özellik kümesi için olayları ve hataları izler.

```json
{
    "records":
    [
         {
             "time": "2019-09-24T17:21:52Z",
             "resourceId": "Resource Id",
             "operationName": "ReadManyConfigurations",
             "category": "Configurations",
             "resultType": "",
             "resultDescription": "",
             "level": "Information",
             "durationMs": "17",
             "properties": "{\"configurationId\":\"\",\"sdkVersion\":\"2018-06-30\",\"messageSize\":\"0\",\"statusCode\":null}",
             "location": "southcentralus"
         }
    ]
}
```

### <a name="device-streams-preview"></a>Aygıt Akışları (Önizleme)

Aygıt akışı kategorisi, tek tek aygıtlara gönderilen istek-yanıt etkileşimlerini izler.

```json
{
    "records":
    [
         {
             "time": "2019-09-19T11:12:04Z",
             "resourceId": "Resource Id",
             "operationName": "invoke",
             "category": "DeviceStreams",
             "resultType": "",
             "resultDescription": "",    
             "level": "Information",
             "durationMs": "74",
             "properties": "{\"deviceId\":\"myDevice\",\"moduleId\":\"myModule\",\"sdkVersion\":\"2019-05-01-preview\",\"requestSize\":\"3\",\"responseSize\":\"5\",\"statusCode\":null,\"requestName\":\"myRequest\",\"direction\":\"c2d\"}",
             "location": "Central US"
         }
    ]
}
```

### <a name="read-logs-from-azure-event-hubs"></a>Azure Etkinlik Hub'larından günlükleri okuma

Tanılama ayarları nda olay günlüğe kaydetmeyi ayarladıktan sonra, günlükleri okuyan uygulamalar oluşturabilir, böylece bu ayarlardaki bilgilere göre işlem yapabilirsiniz. Bu örnek kod, bir olay merkezinden günlükleri alır:

```csharp
class Program
{ 
    static string connectionString = "{your AMS eventhub endpoint connection string}";
    static string monitoringEndpointName = "{your AMS event hub endpoint name}";
    static EventHubClient eventHubClient;
    //This is the Diagnostic Settings schema
    class AzureMonitorDiagnosticLog
    {
        string time { get; set; }
        string resourceId { get; set; }
        string operationName { get; set; }
        string category { get; set; }
        string level { get; set; }
        string resultType { get; set; }
        string resultDescription { get; set; }
        string durationMs { get; set; }
        string callerIpAddress { get; set; }
        string correlationId { get; set; }
        string identity { get; set; }
        string location { get; set; }
        Dictionary<string, string> properties { get; set; }
    };

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformationAsync().PartitionIds;
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
                var deserializer = new JavaScriptSerializer();
                //deserialize json data to azure monitor object
                AzureMonitorDiagnosticLog message = new JavaScriptSerializer().Deserialize<AzureMonitorDiagnosticLog>(result);
            }
        }
    }
}
```

## <a name="use-azure-resource-health"></a>Azure Kaynak Durumu'nun kullanımı

IoT hub'ınızın çalışır durumda olup olmadığını izlemek için Azure Kaynak Durumu'nun kullanımını kullanın. Bölgesel bir kesintinin IoT hub'ınızın sağlığını etkileyip etkilemediğini de öğrenebilirsiniz. Azure IoT Hub'ınızın sistem durumu yla ilgili belirli ayrıntıları anlamak için [Azure Monitörü kullanmanızı](#use-azure-monitor)öneririz.

Azure IoT Hub,sağlık durumunu bölgesel düzeyde gösterir. Bölgesel bir kesinti IoT hub'ınızı etkiliyorsa, sistem durumu **Bilinmiyor**olarak gösterir. Daha fazla bilgi edinmek için [Azure kaynak sağlığında Kaynak türleri ve sistem durumu denetimlerine](../service-health/resource-health-checks-resource-types.md)bakın.

IoT hub'larınızın durumunu kontrol etmek için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. Hizmet **Durumu** > **Kaynak durumu**için gidin.

3. Açılan kutulardan aboneliğinizi seçin ve kaynak türü olarak **IoT Hub'ı** seçin.

Sistem durumu verilerini nasıl yorumladığınız hakkında daha fazla bilgi edinmek için [Azure kaynak durumu genel bakışı'na](../service-health/resource-health-overview.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [IoT Hub ölçümlerini anlama](iot-hub-metrics.md)
* [IoT hub'ınızı ve posta kutunuzu bağlayan Azure Logic Apps ile IoT uzaktan izleme ve bildirimler](iot-hub-monitoring-notifications-with-azure-logic-apps.md)
