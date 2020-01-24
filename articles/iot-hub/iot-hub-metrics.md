---
title: Azure IoT Hub izlemek için ölçümleri kullanma | Microsoft Docs
description: IoT Hub 'larınızın genel durumunu değerlendirmek ve izlemek için Azure IoT Hub ölçümlerini kullanma.
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: jlian
ms.openlocfilehash: ec8a00460b4a750339f929eb6879ac6eb63cac8e
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705743"
---
# <a name="understand-iot-hub-metrics"></a>IoT Hub ölçümlerini anlayın

IoT Hub ölçümler, Azure aboneliğinizdeki Azure IoT kaynaklarının durumu hakkında daha iyi veriler sağlar. IoT Hub ölçümler, IoT Hub hizmetinin ve ona bağlı cihazların genel sistem durumunu değerlendirmenizi sağlar. IoT Hub 'ınız ile neler olduğunu görmenizi ve Azure desteği ile iletişim kurmaya gerek kalmadan köke neden olan sorunları belirlemenize yardımcı olduklarından, kullanıcıya yönelik istatistikler önemlidir.

Ölçümler varsayılan olarak etkindir. Azure portal IoT Hub ölçümlerini görüntüleyebilirsiniz.

> [!NOTE]
> IoT Hub bağlı IoT Tak ve Kullan cihazları hakkındaki bilgileri görüntülemek için IoT Hub ölçümlerini kullanabilirsiniz. IoT Tak ve Kullan cihazları [ıot Tak ve Kullan genel önizlemenin](../iot-pnp/overview-iot-plug-and-play.md)bir parçasıdır.

## <a name="how-to-view-iot-hub-metrics"></a>IoT Hub ölçümlerini görüntüleme

1. IoT Hub 'ı oluşturun. [Bir cihazdan Telemetriyi nasıl oluşturacağınız hakkında yönergeler için bir cihazdan IoT Hub](quickstart-send-telemetry-dotnet.md) Kılavuzu ' na bakın.

2. IoT Hub 'ınızın dikey penceresini açın. Buradan **ölçümler**' e tıklayın.
   
    ![Ölçüm seçeneğinin IoT Hub kaynak sayfasında olduğunu gösteren ekran görüntüsü](./media/iot-hub-metrics/enable-metrics-1.png)

3. Ölçümler dikey penceresinde IoT Hub 'ınız için ölçümleri görüntüleyebilir ve ölçümlerinizin özel görünümlerini oluşturabilirsiniz. 
   
    ![IoT Hub için ölçüm sayfasını gösteren ekran görüntüsü](./media/iot-hub-metrics/enable-metrics-2.png)
    
4. **Tanılama ayarları**' na ve ardından **Tanılama ayarı Ekle** ' ye tıklayarak ölçüm verilerinizi bir Event Hubs uç noktasına veya bir Azure depolama hesabına göndermenizi seçebilirsiniz

   ![Tanılama Ayarları düğmesinin nerede olduğunu gösteren ekran görüntüsü](./media/iot-hub-metrics/enable-metrics-3.png)

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IoT Hub ölçümleri ve bunların nasıl kullanılacağı

IoT Hub, hub 'ınızın sistem durumuna ve bağlı cihazların toplam sayısına ilişkin bir genel bakış sunmak için çeşitli ölçümler sunar. IoT Hub 'ın durumunun büyük bir resmini boyamak için birden fazla ölçüden bilgi birleştirebilirsiniz. Aşağıdaki tabloda, her bir IoT Hub 'ının izlediği ölçümler ve her bir ölçümün IoT Hub 'ının genel durumuyla nasıl ilişkili olduğu açıklanır.

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetri iletisi gönderme denemeleri|Sayı|Toplam|IoT Hub 'ınıza gönderilmeye çalışılan cihazdan buluta telemetri iletisi sayısı|Hiçbiri|
|D2C. telemetri. giriş. başarılı|Gönderilen telemetri iletileri|Sayı|Toplam|IoT Hub 'ınıza başarıyla gönderilen cihazdan buluta telemetri iletisi sayısı|Hiçbiri|
|c2d.commands.egress.complete.success|C2D ileti teslimleri tamamlandı|Sayı|Toplam|Cihaz tarafından başarıyla tamamlanan buluttan cihaza ileti teslimleri sayısı|Hiçbiri|
|c2d.commands.egress.abandon.success|C2D iletileri bırakıldı|Sayı|Toplam|Cihaz tarafından bırakılan buluttan cihaza yönelik ileti sayısı|Hiçbiri|
|c2d.commands.egress.reject.success|C2D iletileri reddedildi|Sayı|Toplam|Cihaz tarafından reddedilen buluttan cihaza yönelik ileti sayısı|Hiçbiri|
|C2DMessagesExpired|C2D Iletilerinin süre dolma (Önizleme)|Sayı|Toplam|Zaman aşımına uğradı buluttan cihaza ileti sayısı|Hiçbiri|
|Devices. totalDevices|Toplam cihaz (kullanım dışı)|Sayı|Toplam|IoT Hub 'ınıza kayıtlı cihazların sayısı|Hiçbiri|
|devices.connectedDevices.allProtocol|Bağlı cihazlar (kullanım dışı) |Sayı|Toplam|IoT Hub 'ınıza bağlı cihazların sayısı|Hiçbiri|
|D2C. telemetri. çıkış. başarılı|Yönlendirme: teslim edilen telemetri iletileri|Sayı|Toplam|IoT Hub yönlendirme kullanılarak iletilerin tüm uç noktalara başarıyla ulaştırılan ileti sayısı. Bir ileti birden çok uç noktaya yönlendirilmemişse, her başarılı teslimat için bu değer bir artar. Bir ileti aynı uç noktaya birden çok kez teslim edildiğinde, her başarılı teslimat için bu değer bir artar.|Hiçbiri|
|D2C. telemetri. çıkış. bırakıldı|Yönlendirme: telemetri iletileri bırakıldı |Sayı|Toplam|İleti, ölü uç noktalar nedeniyle IoT Hub yönlendirmenin kaç kez bırakılmakta olduğunu. Bu değer, bırakılan iletiler buraya teslim edimediğinden geri dönüş rotasına teslim edilen iletileri saymaz.|Hiçbiri|
|d2c.telemetry.egress.orphaned|Yönlendirme: telemetri iletileri yalnız bırakılmış |Sayı|Toplam|Herhangi bir yönlendirme kuralıyla eşleşmediğinden (geri dönüş kuralı dahil) iletilerin IoT Hub yönlendirme tarafından yalnız bırakılmış olduğu zaman sayısı. |Hiçbiri|
|D2C. telemetri. çıkış. geçersiz|Yönlendirme: telemetri iletileri uyumsuz|Sayı|Toplam|Uç nokta ile uyumsuzluk nedeniyle IoT Hub yönlendirmenin iletileri teslim etme başarısız olduğu zaman sayısı. Bu değer yeniden denemeler içermez.|Hiçbiri|
|d2c.telemetry.egress.fallback|Yönlendirme: geri dönüşe teslim edilen iletiler|Sayı|Toplam|Geri dönüş rotası ile ilişkili uç noktaya gönderilen iletileri yönlendirmenin IoT Hub sayısı.|Hiçbiri|
|D2C. endpoints. çıkış. eventHubs|Yönlendirme: Olay Hub 'ına teslim edilen iletiler|Sayı|Toplam|IoT Hub yönlendirmenin Olay Hub 'ı uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Hiçbiri|
|D2C. endpoints. Latency. eventHubs|Yönlendirme: Event Hub için ileti gecikme süresi|Milisaniye|Ortalama|İleti girişi ile bir olay hub 'ı uç noktasına giriş IoT Hub ve ileti girişi arasındaki ortalama gecikme (milisaniye).|Hiçbiri|
|d2c.endpoints.egress.serviceBusQueues|Yönlendirme: Service Bus kuyruğuna teslim edilen iletiler|Sayı|Toplam|IoT Hub yönlendirmenin iletileri Service Bus sıra uç noktalarına başarıyla teslim dağıttığı zaman sayısı.|Hiçbiri|
|d2c.endpoints.latency.serviceBusQueues|Yönlendirme: Service Bus Kuyruğu için ileti gecikme süresi|Milisaniye|Ortalama|İleti girişi IoT Hub ve telemetri iletisi arasındaki ortalama gecikme süresi (milisaniye) Service Bus kuyruğu uç noktasına giriş.|Hiçbiri|
|d2c.endpoints.egress.serviceBusTopics|Yönlendirme: Service Bus konuya teslim edilen iletiler|Sayı|Toplam|IoT Hub yönlendirmenin Service Bus konu uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Hiçbiri|
|d2c.endpoints.latency.serviceBusTopics|Yönlendirme: Service Bus konusu için ileti gecikmesi|Milisaniye|Ortalama|İleti girişi IoT Hub ve telemetri iletisi arasındaki ortalama gecikme süresi (milisaniye) Service Bus konu uç noktasına giriş.|Hiçbiri|
|D2C. endpoints. çıkış. builtIn. Events|Yönlendirme: iletilere/olaylara teslim edilen iletiler|Sayı|Toplam|Yönlendirmenin, yerleşik uç noktaya (iletiler/olaylar) başarıyla ileti teslim IoT Hub sayısı.|Hiçbiri|
|D2C. endpoints. Latency. yerleik. Events|Yönlendirme: iletiler/olaylar için ileti gecikmesi|Milisaniye|Ortalama|Yerleşik uç noktaya (iletiler/olaylar) gelen ileti girişi IoT Hub ve telemetri iletisi arasındaki ortalama gecikme süresi (milisaniye).|Hiçbiri|
|D2C. endpoints. çıkış. Storage|Yönlendirme: depolamaya teslim edilen iletiler|Sayı|Toplam|IoT Hub yönlendirmenin depolama uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Hiçbiri|
|D2C. endpoints. Latency. Storage|Yönlendirme: depolama için ileti gecikmesi|Milisaniye|Ortalama|İleti girişi IoT Hub ve telemetri iletisine bir depolama uç noktasına giriş arasındaki ortalama gecikme süresi (milisaniye).|Hiçbiri|
|D2C. endpoints. çıkış. Storage. Bytes|Yönlendirme: depolama alanına teslim edilen veriler|Bayt|Toplam|Depolama uç noktalarına teslim edilen veri miktarı (bayt) IoT Hub.|Hiçbiri|
|D2C. endpoints. çıkış. Storage. blob 'ları|Yönlendirme: depolama alanına teslim edilen bloblar|Sayı|Toplam|IoT Hub blob 'ları depolama uç noktalarına yönlendirmenin kaç kez yönlendirtığı sayısıdır.|Hiçbiri|
|Eventgridteslimler|Event Grid teslimler (Önizleme)|Sayı|Toplam|Event Grid yayımlanan IoT Hub olaylarının sayısı. Başarılı ve başarısız isteklerin sayısı için sonuç boyutunu kullanın. EventType boyutu olay türünü gösterir (https://aka.ms/ioteventgrid).|RESOURCEID<br/>Kaynaklanan<br/>Olay türü|
|EventGridLatency|Event Grid gecikme süresi (Önizleme)|Milisaniye|Ortalama|Olay Event Grid yayımlandığında IoT Hub olayının oluşturulduğu sürenin ortalama gecikme süresi (milisaniye). Bu sayı, tüm olay türleri arasındaki bir ortalama değer. Belirli bir olay türünün gecikmesini görmek için EventType boyutunu kullanın.|RESOURCEID<br/>Olay türü|
|d2c.twin.read.success|Cihazlardan başarılı ikizi okumaları|Sayı|Toplam|Tüm başarılı cihaz tarafından başlatılan ikizi okuma sayısı.|Hiçbiri|
|d2c.twin.read.failure|Cihazlardan başarısız ikiz okumaları|Sayı|Toplam|Tüm başarısız cihaz tarafından başlatılan ikizi okumaların sayısı.|Hiçbiri|
|D2C. ikizi. Read. size|Cihazlardan ikiz okumaların yanıt boyutu|Bayt|Ortalama|Tüm başarılı cihaz tarafından başlatılan ikizi okumalarının ortalama, en düşük ve en yüksek değeri.|Hiçbiri|
|D2C. ikizi. Update. Success|Cihazlardan başarılı ikizi güncelleştirmeleri|Sayı|Toplam|Tüm başarılı cihaz tarafından başlatılan ikizi güncelleştirmelerinin sayısı.|Hiçbiri|
|d2c.twin.update.failure|Cihazlardan başarısız ikiz güncelleştirmeleri|Sayı|Toplam|Tüm başarısız cihaz tarafından başlatılan ikizi güncelleştirmelerinin sayısı.|Hiçbiri|
|d2c.twin.update.size|Cihazlardan ikizi güncelleştirmelerinin boyutu|Bayt|Ortalama|Tüm başarılı cihaz tarafından başlatılan ikizi güncelleştirmelerinin ortalama, en düşük ve en büyük boyutu.|Hiçbiri|
|c2d.methods.success|Başarılı doğrudan yöntem etkinleştirmeleri|Sayı|Toplam|Tüm başarılı doğrudan yöntem çağrılarının sayısı.|Hiçbiri|
|C2D. Methods. Failure|Başarısız doğrudan yöntem çağrıları|Sayı|Toplam|Tüm başarısız doğrudan yöntem çağrılarının sayısı.|Hiçbiri|
|c2d.methods.requestSize|Doğrudan yöntem çağrılarının istek boyutu|Bayt|Ortalama|Tüm başarılı doğrudan yöntem isteklerinin ortalama, en düşük ve en yüksek değeri.|Hiçbiri|
|c2d.methods.responseSize|Doğrudan yöntem çağrılarının yanıt boyutu|Bayt|Ortalama|Tüm başarılı doğrudan yöntem yanıtlarının ortalama, en düşük ve en yüksek değeri.|Hiçbiri|
|c2d.twin.read.success|Arka uçtan başarılı ikizi okumaları|Sayı|Toplam|Tüm başarılı arka uç tarafından başlatılan ikizi okumaların sayısı.|Hiçbiri|
|C2D. ikizi. Read. Failure|Arka uçtan başarısız ikiz okumaları|Sayı|Toplam|Tüm başarısız arka uç tarafından başlatılan ikizi okumaların sayısı.|Hiçbiri|
|C2D. ikizi. Read. size|Arka uçtan ikiz okumaların yanıt boyutu|Bayt|Ortalama|Tüm başarılı arka uç başlatılan ikizi okumalarının ortalama, en düşük ve en yüksek değeri.|Hiçbiri|
|C2D. ikizi. Update. Success|Arka uçtan başarılı ikiz güncelleştirmeleri|Sayı|Toplam|Tüm başarılı arka uç ile başlatılan ikizi güncelleştirmelerinin sayısı.|Hiçbiri|
|C2D. ikizi. Update. Failure|Arka uçtan başarısız ikiz güncelleştirmeleri|Sayı|Toplam|Tüm başarısız arka uç başlatılan ikizi güncelleştirmelerinin sayısı.|Hiçbiri|
|c2d.twin.update.size|Arka uçtan ikizi güncelleştirmelerinin boyutu|Bayt|Ortalama|Tüm başarılı arka uç ile başlatılan ikizi güncelleştirmelerinin ortalama, en düşük ve en büyük boyutu.|Hiçbiri|
|twinQueries. Success|Başarılı ikizi sorguları|Sayı|Toplam|Tüm başarılı ikizi sorgularının sayısı.|Hiçbiri|
|twinQueries. Failure|Başarısız ikiz sorguları|Sayı|Toplam|Tüm başarısız ikizi sorgularının sayısı.|Hiçbiri|
|twinQueries.resultSize|İkizi sorguları sonuç boyutu|Bayt|Ortalama|Tüm başarılı ikizi sorgularının sonuç boyutunun ortalama, en düşük ve en yüksek değeri.|Hiçbiri|
|jobs.createTwinUpdateJob.success|İkizi Update işlerinin başarılı oluşturmaları|Sayı|Toplam|Tüm başarılı ikizi Update işlerinin oluşturma sayısı.|Hiçbiri|
|jobs.createTwinUpdateJob.failure|İkiz güncelleştirme işlerinin başarısız oluşturmaları|Sayı|Toplam|İkizi Update işlerinin başarısız olan tüm oluşturma sayısı.|Hiçbiri|
|Jobs. createDirectMethodJob. Success|Yöntem çağırma işlerinin başarılı oluşturmaları|Sayı|Toplam|Tüm başarılı doğrudan yöntem çağırma işlerinin oluşturulma sayısı.|Hiçbiri|
|Jobs. createDirectMethodJob. Failure|Yöntem çağrısı işlerinin başarısız oluşturmaları|Sayı|Toplam|Doğrudan yöntem çağırma işlerinin tüm başarısız oluşturma sayısı.|Hiçbiri|
|işler. listJobs. başarılı|İşleri listelemek için başarılı çağrılar|Sayı|Toplam|İşleri listelemek için başarılı olan tüm çağrıların sayısı.|Hiçbiri|
|Jobs. listJobs. hata|İş listeleme için başarısız çağrılar|Sayı|Toplam|Listeleme işleri için başarısız olan tüm çağrıların sayısı.|Hiçbiri|
|Jobs. cancelJob. Success|Başarılı iş iptalleri|Sayı|Toplam|İşi iptal etmek için tüm başarılı çağrı sayısı.|Hiçbiri|
|Jobs. cancelJob. Failure|Başarısız iş iptalleri|Sayı|Toplam|İşi iptal etmek için başarısız olan tüm çağrıların sayısı.|Hiçbiri|
|işler. queryJobs. başarılı|Başarılı iş sorguları|Sayı|Toplam|Sorgu işlerine yapılan tüm başarılı çağrıların sayısı.|Hiçbiri|
|Jobs. queryJobs. hata|Başarısız iş sorguları|Sayı|Toplam|Sorgu işlerine yapılan tüm başarısız çağrıların sayısı.|Hiçbiri|
|işler. tamamlandı|Tamamlanan işler|Sayı|Toplam|Tamamlanan tüm işlerin sayısı.|Hiçbiri|
|işler. başarısız|Başarısız olan işler|Sayı|Toplam|Başarısız olan tüm işlerin sayısı.|Hiçbiri|
|d2c.telemetry.ingress.sendThrottle|Azaltma hatalarının sayısı|Sayı|Toplam|Cihaz performansı azaltmasından kaynaklanan azaltma hatası sayısı|Hiçbiri|
|Günlükmessagequotakullanıldı|Kullanılan toplam ileti sayısı|Sayı|Ortalama|Bugün kullanılan toplam ileti sayısı. Bu, her gün 00:00 UTC tarihinde sıfıra sıfırlanan toplu bir değerdir.|Hiçbiri|
|deviceDataUsage|Toplam cihaz verisi kullanımı|Bayt|Toplam|Iothub 'e bağlı tüm cihazlardan ve bunlardan aktarılan baytlar|Hiçbiri|
|deviceDataUsageV2|Toplam cihaz verisi kullanımı (Önizleme)|Bayt|Toplam|Iothub 'e bağlı tüm cihazlardan ve bunlardan aktarılan baytlar|Hiçbiri|
|totalDeviceCount|Toplam cihaz (Önizleme)|Sayı|Ortalama|IoT Hub 'ınıza kayıtlı cihazların sayısı|Hiçbiri|
|connectedDeviceCount|Bağlı cihazlar (önizleme)|Sayı|Ortalama|IoT Hub 'ınıza bağlı cihazların sayısı|Hiçbiri|
|yapılandırmalar|Yapılandırma Ölçümleri|Sayı|Toplam|Yapılandırma Işlemleri için ölçümler|Hiçbiri|

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub ölçümlerine genel bir bakış gördüğünüze göre, Azure IoT Hub yönetimi hakkında daha fazla bilgi edinmek için bu bağlantıyı izleyin:

* [Tanılama günlüklerini ayarlama](iot-hub-monitor-resource-health.md)

IoT Hub yeteneklerini daha fazla incelemek için bkz.:

* [IoT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)

* [Azure IoT Edge ile uç cihazlara AI dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
