---
title: Azure IoT Hub izlemek için ölçümleri kullanma | Microsoft Docs
description: IoT Hub 'larınızın genel durumunu değerlendirmek ve izlemek için Azure IoT Hub ölçümlerini kullanma.
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: jlian
ms.openlocfilehash: 49ecf753b1dd547eab251222a6c2709d96e9c89b
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92146525"
---
<!--for build: for each metric, if you understand what it is, it's ok. otw add more info.  -->
# <a name="understand-iot-hub-metrics"></a>IoT Hub ölçümlerini anlayın

IoT Hub ölçümler, Azure aboneliğinizdeki Azure IoT kaynaklarının durumu hakkında bilgi verir. IoT Hub ölçümler, IoT Hub hizmetinin ve ona bağlı cihazların genel sistem durumunu değerlendirmenize yardımcı olur. Bu kullanıcıya yönelik istatistikler, IoT Hub 'ınız ile neler olduğunu görmenizi ve Azure desteği ile iletişim kurmaya gerek kalmadan sorunlar için kök neden analizi gerçekleştirmenize yardımcı olur.

Ölçümler varsayılan olarak etkindir. Azure portal IoT Hub ölçümlerini görüntüleyebilirsiniz.

> [!NOTE]
> IoT Hub bağlı [ıot Tak ve kullan](../iot-pnp/overview-iot-plug-and-play.md) cihazları hakkındaki bilgileri görüntülemek için IoT Hub ölçümlerini kullanabilirsiniz.

## <a name="how-to-view-iot-hub-metrics"></a>IoT Hub ölçümlerini görüntüleme

1. IoT Hub 'ı oluşturun. [Bir cihazdan Telemetriyi nasıl oluşturacağınız hakkında yönergeler için bir cihazdan IoT Hub](quickstart-send-telemetry-dotnet.md) Kılavuzu ' na bakın.

2. IoT Hub 'ınızın dikey penceresini açın. Buradan **ölçümler**' e tıklayın.
   
    ![Ölçüm seçeneğinin IoT Hub kaynak sayfasında olduğunu gösteren ekran görüntüsü](./media/iot-hub-metrics/enable-metrics-1.png)

3. Ölçümler dikey penceresinde IoT Hub 'ınız için ölçümleri görüntüleyebilir ve ölçümlerinizin özel görünümlerini oluşturabilirsiniz. 
   
    ![IoT Hub için ölçüm sayfasını gösteren ekran görüntüsü](./media/iot-hub-metrics/enable-metrics-2.png)
    
4. **Tanılama ayarları**' na ve ardından **Tanılama ayarı Ekle**' ye tıklayarak ölçüm verilerinizi bir Event Hubs uç noktasına veya bir Azure depolama hesabına göndermenizi seçebilirsiniz.

   ![Tanılama Ayarları düğmesinin nerede olduğunu gösteren ekran görüntüsü](./media/iot-hub-metrics/enable-metrics-3.png)

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IoT Hub ölçümleri ve bunların nasıl kullanılacağı

IoT Hub, hub 'ınızın sistem durumuna ve bağlı cihazların toplam sayısına ilişkin bir genel bakış sunmak için çeşitli ölçümler sunar. IoT Hub 'ın durumunun büyük bir resmini boyamak için birden fazla ölçüden bilgi birleştirebilirsiniz. Aşağıdaki tabloda, her bir IoT Hub 'ının izlediği ölçümler ve her bir ölçümün IoT Hub 'ının genel durumuyla nasıl ilişkili olduğu açıklanır.

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Description|Boyutlar|
|---|---|---|---|---|---|
|Routingteslimler | Yönlendirme teslim denemeleri (Önizleme) | Count | Toplam |Bu, yönlendirme teslimi ölçümdür. Belirli bir uç noktanın teslim durumunu veya belirli bir yönlendirme kaynağını belirlemek için boyutları kullanın.| RESOURCEID<br>Sonuç,<br>RoutingSource,<br>EndpointType,<br>FailureReasonCategory,<br>Uçnoktaadı<br>* [**Burada**](#dimensions)Boyutlar hakkında daha fazla ayrıntı*. |
|Routingdeliverygecikme süresi| Yönlendirme gecikmesi (Önizleme) | Mayacak | Ortalama |Bu, yönlendirme teslimi gecikme ölçümdür. Belirli bir uç noktanın gecikmesini veya belirli bir yönlendirme kaynağını belirlemek için boyutları kullanın.| RESOURCEID<br>RoutingSource,<br>EndpointType,<br>Uçnoktaadı<br>* [**Burada**](#dimensions)Boyutlar hakkında daha fazla ayrıntı*.|
|RoutingDataSizeInBytesDelivered| Yönlendirme teslim verilerinin bayt cinsinden boyutu (Önizleme)| Bayt | Toplam |IoT Hub tarafından özel uç noktaya ve yerleşik uç noktaya yönlendirilen toplam bayt sayısı. Belirli bir uç noktaya veya belirli bir yönlendirme kaynağına yönlendirilen veri boyutunu tanımlamak için boyutları kullanın.| RESOURCEID<br>RoutingSource,<br>EndpointType<br>Uçnoktaadı<br>* [**Burada**](#dimensions)Boyutlar hakkında daha fazla ayrıntı*.|
|D2C. telemetri. ınress.<br>allProtocol|Telemetri iletisi gönderme denemeleri|Count|Toplam|IoT Hub 'ınıza gönderilmeye çalışılan cihazdan buluta telemetri iletisi sayısı|Hiçbiri|
|D2C. telemetri. ınress.<br>başarılı|Gönderilen telemetri iletileri|Count|Toplam|IoT Hub 'ınıza başarıyla gönderilen cihazdan buluta telemetri iletisi sayısı|Hiçbiri|
|C2D. Commands. çıkış.<br>tamamlandı. başarılı|C2D ileti teslimleri tamamlandı|Count|Toplam|Cihaz tarafından başarıyla tamamlanan buluttan cihaza ileti teslimleri sayısı|Hiçbiri|
|C2D. Commands. çıkış.<br>Abandon. Success|C2D iletileri bırakıldı|Count|Toplam|Cihaz tarafından bırakılan buluttan cihaza yönelik ileti sayısı|Hiçbiri|
|C2D. Commands. çıkış.<br>Reddet. başarılı|C2D iletileri reddedildi|Count|Toplam|Cihaz tarafından reddedilen buluttan cihaza yönelik ileti sayısı|Hiçbiri|
|C2DMessagesExpired|C2D Iletilerinin süre dolma (Önizleme)|Count|Toplam|Zaman aşımına uğradı buluttan cihaza ileti sayısı|Hiçbiri|
|Devices. totalDevices|Toplam cihaz (kullanım dışı)|Count|Toplam|IoT Hub 'ınıza kayıtlı cihazların sayısı|Hiçbiri|
|cihazlar. connectedDevices.<br>allProtocol|Bağlı cihazlar (kullanım dışı) |Count|Toplam|IoT Hub 'ınıza bağlı cihazların sayısı|Hiçbiri|
|D2C. telemetri. çıkış.<br>başarılı|Yönlendirme: teslim edilen telemetri iletileri|Count|Toplam|IoT Hub yönlendirme kullanılarak iletilerin tüm uç noktalara başarıyla ulaştırılan ileti sayısı. Bir ileti birden çok uç noktaya yönlendirilmemişse, her başarılı teslimat için bu değer bir artar. Bir ileti aynı uç noktaya birden çok kez teslim edildiğinde, her başarılı teslimat için bu değer bir artar.|Hiçbiri|
|D2C. telemetri. çıkış.<br>bırakılmış|Yönlendirme: telemetri iletileri bırakıldı |Count|Toplam|İleti, ölü uç noktalar nedeniyle IoT Hub yönlendirmenin kaç kez bırakılmakta olduğunu. Bu değer, bırakılan iletiler buraya teslim edimediğinden geri dönüş rotasına teslim edilen iletileri saymaz.|Hiçbiri|
|D2C. telemetri. çıkış.<br>kaldı|Yönlendirme: telemetri iletileri yalnız bırakılmış |Count|Toplam|Geri dönüş rotası devre dışı bırakıldığında hiçbir yönlendirme sorgusuyla eşleşmediğinden iletilerin IoT Hub yönlendirme tarafından yalnız bırakılmış olduğu zaman sayısı.|Hiçbiri|
|D2C. telemetri. çıkış.<br>geçersiz|Yönlendirme: telemetri iletileri uyumsuz|Count|Toplam|Uç nokta ile uyumsuzluk nedeniyle IoT Hub yönlendirmenin iletileri teslim etme başarısız olduğu zaman sayısı. IoT Hub, iletiyi bir uç noktaya teslim etmeye çalıştığında ve geçici olmayan bir hata ile başarısız olduğunda bir ileti bir uç nokta ile uyumsuzdur. Geçersiz iletiler yeniden denenmez. Bu değer yeniden denemeler içermez.|Hiçbiri|
|D2C. telemetri. çıkış.<br>dönüş|Yönlendirme: geri dönüşe teslim edilen iletiler|Count|Toplam|Geri dönüş rotası ile ilişkili uç noktaya gönderilen iletileri yönlendirmenin IoT Hub sayısı.|Hiçbiri|
|D2C. endpoints. çıkış.<br>eventHubs|Yönlendirme: Olay Hub 'ına teslim edilen iletiler|Count|Toplam|IoT Hub yönlendirmenin, Olay Hub 'ı türünde özel uç noktalara başarıyla ileti teslim dağıttığı sayı. Bu, yerleşik uç noktaya yönelik ileti yollarını içermez (olaylar).|Hiçbiri|
|D2C. endpoints. Latency.<br>eventHubs|Yönlendirme: Olay Hub 'ı için ileti gecikmesi|Mayacak|Ortalama|Olay Hub 'ının özel uç noktalarına IoT Hub ileti girişi ve ileti girişi arasındaki ortalama gecikme süresi (milisaniye). Bu, yerleşik uç noktaya yönelik ileti yollarını içermez (olaylar).|Hiçbiri|
|D2C. endpoints. çıkış.<br>serviceBusQueues|Yönlendirme: Service Bus kuyruğuna teslim edilen iletiler|Count|Toplam|IoT Hub yönlendirmenin iletileri Service Bus sıra uç noktalarına başarıyla teslim dağıttığı zaman sayısı.|Hiçbiri|
|D2C. endpoints. Latency.<br>serviceBusQueues|Yönlendirme: Service Bus kuyruğu için ileti gecikmesi|Mayacak|Ortalama|IoT Hub ileti girişi ile Service Bus kuyruğu uç noktasına giriş arasındaki ortalama gecikme süresi (milisaniye).|Hiçbiri|
|D2C. endpoints. çıkış.<br>Servicebuskonulardaki konuları|Yönlendirme: Service Bus konuya teslim edilen iletiler|Count|Toplam|IoT Hub yönlendirmenin Service Bus konu uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Hiçbiri|
|D2C. endpoints. Latency.<br>Servicebuskonulardaki konuları|Yönlendirme: Service Bus konusu için ileti gecikmesi|Mayacak|Ortalama|IoT Hub ve ileti girişi arasındaki ortalama gecikme süresi (milisaniye) Service Bus konu uç noktasına giriş.|Hiçbiri|
|D2C. endpoints. çıkış.<br>Yerleşik. olaylar|Yönlendirme: iletilere/olaylara teslim edilen iletiler|Count|Toplam|IoT Hub yönlendirmenin, yerleşik uç noktaya (iletiler/olaylar) ve geri dönüş rotasında iletileri başarıyla teslim eden zaman sayısı.|Hiçbiri|
|D2C. endpoints. Latency.<br>Yerleşik. olaylar|Yönlendirme: iletiler/olaylar için ileti gecikmesi|Mayacak|Ortalama|Yerleşik uç noktaya (iletiler/olaylar) ve geri dönüş rotasında ileti girişi IoT Hub ve ileti girişi arasındaki ortalama gecikme süresi (milisaniye).|Hiçbiri|
|D2C. endpoints. çıkış.<br>depolama|Yönlendirme: depolamaya teslim edilen iletiler|Count|Toplam|IoT Hub yönlendirmenin depolama uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Hiçbiri|
|D2C. endpoints. Latency.<br>depolama|Yönlendirme: depolama için ileti gecikmesi|Mayacak|Ortalama|Bir depolama uç noktasında ileti girişi IoT Hub ve ileti girişi arasındaki ortalama gecikme süresi (milisaniye).|Hiçbiri|
|D2C. endpoints. çıkış.<br>Storage. Bytes|Yönlendirme: depolamaya teslim edilen veriler|Bayt|Toplam|Depolama uç noktalarına teslim edilen veri miktarı (bayt) IoT Hub.|Hiçbiri|
|D2C. endpoints. çıkış.<br>Storage. blob 'ları|Yönlendirme: depolamaya teslim edilen Bloblar|Count|Toplam|IoT Hub blob 'ları depolama uç noktalarına yönlendirmenin kaç kez yönlendirtığı sayısıdır.|Hiçbiri|
|Eventgridteslimler|Event Grid teslimler (Önizleme)|Count|Toplam|Event Grid yayımlanan IoT Hub olaylarının sayısı. Başarılı ve başarısız isteklerin sayısı için sonuç boyutunu kullanın. EventType boyutu olay türünü gösterir ( https://aka.ms/ioteventgrid) .|RESOURCEID<br/>Sonuç,<br/>Olay türü|
|EventGridLatency|Event Grid gecikme süresi (Önizleme)|Mayacak|Ortalama|Olay Event Grid yayımlandığında IoT Hub olayının oluşturulduğu sürenin ortalama gecikme süresi (milisaniye). Bu sayı, tüm olay türleri arasındaki bir ortalama değer. Belirli bir olay türünün gecikmesini görmek için EventType boyutunu kullanın.|RESOURCEID<br/>Olay türü|
|D2C. ikizi. Read. Success|Cihazlardan başarılı ikizi okumaları|Count|Toplam|Tüm başarılı cihaz tarafından başlatılan ikizi okuma sayısı.|Hiçbiri|
|D2C. ikizi. Read. Failure|Cihazlardan başarısız ikizi okumaları|Count|Toplam|Tüm başarısız cihaz tarafından başlatılan ikizi okumaların sayısı.|Hiçbiri|
|D2C. ikizi. Read. size|Cihazlardan gelen ikizi okumaların yanıt boyutu|Bayt|Ortalama|Tüm başarılı cihaz tarafından başlatılan ikizi okuma sayısı.|Hiçbiri|
|D2C. ikizi. Update. Success|Cihazlardan başarılı ikizi güncelleştirmeleri|Count|Toplam|Tüm başarılı cihaz tarafından başlatılan ikizi güncelleştirmelerinin sayısı.|Hiçbiri|
|D2C. ikizi. Update. Failure|Cihazlardan ikizi güncelleştirmeleri başarısız oldu|Count|Toplam|Tüm başarısız cihaz tarafından başlatılan ikizi güncelleştirmelerinin sayısı.|Hiçbiri|
|D2C. ikizi. Update. size|Cihazlardan ikizi güncelleştirmelerinin boyutu|Bayt|Ortalama|Tüm başarılı cihaz tarafından başlatılan ikizi güncelleştirmelerinin toplam boyutu.|Hiçbiri|
|C2D. Methods. Success|Başarılı doğrudan yöntem etkinleştirmeleri|Count|Toplam|Tüm başarılı doğrudan yöntem çağrılarının sayısı.|Hiçbiri|
|C2D. Methods. Failure|Başarısız doğrudan yöntem etkinleştirmeleri|Count|Toplam|Tüm başarısız doğrudan yöntem çağrılarının sayısı.|Hiçbiri|
|C2D. Methods. requestSize|Doğrudan yöntem etkinleştirmeleri istek boyutu|Bayt|Ortalama|Tüm başarılı doğrudan yöntem isteklerinin sayısı.|Hiçbiri|
|C2D. Methods. responseSize|Doğrudan yöntem etkinleştirmeleri yanıt boyutu|Bayt|Ortalama|Tüm başarılı doğrudan yöntem yanıtlarının sayısı.|Hiçbiri|
|C2D. ikizi. Read. Success|Arka uçtan başarılı ikizi okumaları|Count|Toplam|Tüm başarılı arka uç tarafından başlatılan ikizi okumaların sayısı.|Hiçbiri|
|C2D. ikizi. Read. Failure|Arka uçtan başarısız ikizi okumaları|Count|Toplam|Tüm başarısız arka uç tarafından başlatılan ikizi okumaların sayısı.|Hiçbiri|
|C2D. ikizi. Read. size|Arka uçtan gelen ikizi okumaların yanıt boyutu|Bayt|Ortalama|Tüm başarılı arka uç tarafından başlatılan ikizi okumaların sayısı.|Hiçbiri|
|C2D. ikizi. Update. Success|Arka uçtan başarılı ikizi güncelleştirmeleri|Count|Toplam|Tüm başarılı arka uç ile başlatılan ikizi güncelleştirmelerinin sayısı.|Hiçbiri|
|C2D. ikizi. Update. Failure|Arka uçtan başarısız ikizi güncelleştirmeleri|Count|Toplam|Tüm başarısız arka uç başlatılan ikizi güncelleştirmelerinin sayısı.|Hiçbiri|
|C2D. ikizi. Update. size|Arka uçtan ikizi güncelleştirmelerinin boyutu|Bayt|Ortalama|Tüm başarılı arka uç ile başlatılan ikizi güncelleştirmelerinin toplam boyutu.|Hiçbiri|
|twinQueries. Success|Başarılı ikizi sorguları|Count|Toplam|Tüm başarılı ikizi sorgularının sayısı.|Hiçbiri|
|twinQueries. Failure|Başarısız ikizi sorguları|Count|Toplam|Tüm başarısız ikizi sorgularının sayısı.|Hiçbiri|
|twinQueries. resultSize|İkizi sorguları sonuç boyutu|Bayt|Ortalama|Tüm başarılı ikizi sorgularının sonuç boyutunun toplamı.|Hiçbiri|
|Jobs. createTwinUpdateJob.<br>başarılı|İkizi Update işlerinin başarılı oluşturmaları|Count|Toplam|Tüm başarılı ikizi Update işlerinin oluşturma sayısı.|Hiçbiri|
|Jobs. createTwinUpdateJob.<br>hataları|İkizi Update işlerinin başarısız oluşturmaları|Count|Toplam|İkizi Update işlerinin başarısız olan tüm oluşturma sayısı.|Hiçbiri|
|Jobs. createDirectMethodJob.<br>başarılı|Yöntem çağırma işlerinin başarılı oluşturmaları|Count|Toplam|Tüm başarılı doğrudan yöntem çağırma işlerinin oluşturulma sayısı.|Hiçbiri|
|Jobs. createDirectMethodJob.<br>hataları|Yöntem çağırma işlerinin oluşturma işlemi başarısız oldu|Count|Toplam|Doğrudan yöntem çağırma işlerinin tüm başarısız oluşturma sayısı.|Hiçbiri|
|işler. listJobs. başarılı|İşleri listelemek için başarılı çağrılar|Count|Toplam|İşleri listelemek için başarılı olan tüm çağrıların sayısı.|Hiçbiri|
|Jobs. listJobs. hata|İşleri listelemek için başarısız çağrılar|Count|Toplam|Listeleme işleri için başarısız olan tüm çağrıların sayısı.|Hiçbiri|
|Jobs. cancelJob. Success|Başarılı iş iptalleri|Count|Toplam|İşi iptal etmek için tüm başarılı çağrı sayısı.|Hiçbiri|
|Jobs. cancelJob. Failure|Başarısız iş iptalleri|Count|Toplam|İşi iptal etmek için başarısız olan tüm çağrıların sayısı.|Hiçbiri|
|işler. queryJobs. başarılı|Başarılı iş sorguları|Count|Toplam|Sorgu işlerine yapılan tüm başarılı çağrıların sayısı.|Hiçbiri|
|Jobs. queryJobs. hata|Başarısız iş sorguları|Count|Toplam|Sorgu işlerine yapılan tüm başarısız çağrıların sayısı.|Hiçbiri|
|işler. tamamlandı|Tamamlanan İşler|Count|Toplam|Tamamlanan tüm işlerin sayısı.|Hiçbiri|
|işler. başarısız|Başarısız işler|Count|Toplam|Başarısız olan tüm işlerin sayısı.|Hiçbiri|
|D2C. telemetri. ınress.<br>Sendkısıtlaması|Daraltma hatası sayısı|Count|Toplam|Cihaz performansı azaltmasından kaynaklanan azaltma hatası sayısı|Hiçbiri|
|Günlükmessagequotakullanıldı|Kullanılan toplam ileti sayısı|Count|Ortalama|Bugün kullanılan toplam ileti sayısı. Bu, her gün 00:00 UTC tarihinde sıfıra sıfırlanan toplu bir değerdir.|Hiçbiri|
|deviceDataUsage|Toplam cihaz verisi kullanımı|Bayt|Toplam|Iothub 'e bağlı tüm cihazlardan ve bunlardan aktarılan baytlar|Hiçbiri|
|deviceDataUsageV2|Toplam cihaz verisi kullanımı (Önizleme)|Bayt|Toplam|Iothub 'e bağlı tüm cihazlardan ve bunlardan aktarılan baytlar|Hiçbiri|
|totalDeviceCount|Toplam cihaz (Önizleme)|Count|Ortalama|IoT Hub 'ınıza kayıtlı cihazların sayısı|Hiçbiri|
|connectedDeviceCount|Bağlı cihazlar (Önizleme)|Count|Ortalama|IoT Hub 'ınıza bağlı cihazların sayısı|Hiçbiri|
|konfigürasyonları|Yapılandırma ölçümleri|Count|Toplam|Bir hedef cihaz kümesinde cihaz yapılandırması ve IoT Edge dağıtımı için gerçekleştirilen toplam CRUD işlemlerinin sayısı. Bu Ayrıca, bu yapılandırmalardan dolayı cihaz ikizi veya modül ikizi ' i değiştiren işlem sayısını da içerir.|Hiçbiri|

### <a name="dimensions"></a>Boyutlar
Boyutlar ölçümler hakkında daha fazla ayrıntı belirlemenize yardımcı olur. Yönlendirme ölçümlerinden bazıları uç nokta başına bilgi sağlar. Aşağıdaki tabloda bu boyutlar için olası değerler listelenmiştir.

|Boyut|Değerler|
|---|---|
|ResourceID|IoT Hub kaynağınız|
|Sonuç|başarılı<br>hataları|
|Yönlendirme kaynağı|Cihaz Iletileri<br>İkizi değişiklik olayları<br>Cihaz yaşam döngüsü olayları|
|EndpointType|eventHubs<br>serviceBusQueues<br>cosmosDB<br>Servicebuskonulardaki konuları<br>Yerleik<br>blobStorage|
|FailureReasonCategory|geçersiz<br>bırakılmış<br>kaldı<br>null|
|Uçnoktaadı|uç nokta adı|

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub ölçümlerine genel bir bakış gördüğünüze göre, Azure IoT Hub yönetimi hakkında daha fazla bilgi edinmek için bu bağlantıyı izleyin:

* [Tanılama günlükleri ayarlama](iot-hub-monitor-resource-health.md)

* [İleti yönlendirme sorunlarını giderme hakkında bilgi edinin](troubleshoot-message-routing.md)

IoT Hub yeteneklerini daha fazla incelemek için bkz.:

* [IoT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/quickstart-linux.md)