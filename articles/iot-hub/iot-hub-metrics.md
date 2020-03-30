---
title: Azure IoT Hub'ı izlemek için ölçümleri kullanın | Microsoft Dokümanlar
description: IoT hub'larınızın genel durumunu değerlendirmek ve izlemek için Azure IoT Hub ölçümlerini kullanma.
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: jlian
ms.openlocfilehash: ec8a00460b4a750339f929eb6879ac6eb63cac8e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284440"
---
# <a name="understand-iot-hub-metrics"></a>IoT Hub ölçümlerini anlama

IoT Hub ölçümleri, Azure aboneliğinizdeki Azure IoT kaynaklarının durumu hakkında daha iyi veriler sağlar. IoT Hub ölçümleri, IoT Hub hizmetinin ve ona bağlı aygıtların genel durumunu değerlendirmenize olanak tanır. Kullanıcıya yönelik istatistikler önemlidir, çünkü IoT hub'ınızda neler olup bittiğini görmenize ve Azure desteğine başvurmaya gerek kalmadan sorunlara neden olmanıza yardımcı olurlar.

Ölçümler varsayılan olarak etkinleştirilir. Azure portalından IoT Hub ölçümlerini görüntüleyebilirsiniz.

> [!NOTE]
> IoT Hub'ınıza bağlı IoT Tak ve Çalıştır aygıtları hakkındaki bilgileri görüntülemek için IoT Hub ölçümlerini kullanabilirsiniz. IoT Tak ve Çalıştır [aygıtları, IoT Tak ve Genel Önizleme'nin](../iot-pnp/overview-iot-plug-and-play.md)bir parçasıdır.

## <a name="how-to-view-iot-hub-metrics"></a>IoT Hub ölçümlerini görüntüleme

1. Bir IoT hub'ı oluşturun. Bir [aygıttan IoT Hub kılavuzuna gönderme telemetrisinde](quickstart-send-telemetry-dotnet.md) bir IoT hub'ı oluşturma hakkında yönergeler bulabilirsiniz.

2. IoT hub'ınızın bıçağını açın. Buradan, **Ölçümler'i**tıklatın.
   
    ![IoT Hub kaynak sayfasında ölçüm seçeneğinin nerede olduğunu gösteren ekran görüntüsü](./media/iot-hub-metrics/enable-metrics-1.png)

3. Ölçümler bıçak itibaren, IoT hub için ölçümleri görüntülemek ve ölçümleri özel görünümleri oluşturabilirsiniz. 
   
    ![IoT Hub'ın ölçüm sayfasını gösteren ekran görüntüsü](./media/iot-hub-metrics/enable-metrics-2.png)
    
4. Tanılama **ayarlarını**tıklatarak metrik verilerinizi bir Olay Hub'ları bitiş noktasına veya Azure Depolama hesabına göndermeyi seçebilirsiniz , ardından **tanılama ayarı ekleyin**

   ![Tanılama ayarları düğmesinin nerede olduğunu gösteren ekran görüntüsü](./media/iot-hub-metrics/enable-metrics-3.png)

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IoT Hub ölçümleri ve bunların nasıl kullanılacağı

IoT Hub, hub'ınızın durumu ve toplam bağlı aygıt sayısı hakkında genel bir bakış sağlamak için çeşitli ölçümler sağlar. IoT hub'ının durumunun daha büyük bir resmini çizmek için birden çok ölçümdeki bilgileri birleştirebilirsiniz. Aşağıdaki tabloda, her IoT hub'ının izlediği ölçümler ve her ölçümün IoT hub'ının genel durumuyla nasıl ilişkili olduğu açıklanmaktadır.

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|d2c.telemetri.ingress.allProtocol|Telemetri iletisi gönderme girişimleri|Sayı|Toplam|IoT hub'ınıza gönderilmeye çalışılan aygıttan buluta telemetri iletilerinin sayısı|None|
|d2c.telemetri.ingress.success|Gönderilen Telemetri iletileri|Sayı|Toplam|IoT hub'ınıza başarıyla gönderilen aygıttan buluta telemetri iletilerinin sayısı|None|
|c2d.commands.egress.complete.success|C2D ileti teslimatları tamamlandı|Sayı|Toplam|Aygıt tarafından başarıyla tamamlanan buluttan cihaza ileti teslimlerinin sayısı|None|
|c2d.commands.egress.abandon.success|C2D iletileri terk edildi|Sayı|Toplam|Aygıt tarafından terk edilen buluttan aygıta ileti sayısı|None|
|c2d.commands.egress.reject.success|C2D iletileri reddedildi|Sayı|Toplam|Aygıt tarafından reddedilen buluttan aygıta ileti sayısı|None|
|C2DMesajlarSüresi Doldu|C2D İletilerinin Süresi Doldu (önizleme)|Sayı|Toplam|Süresi dolmuş buluttan aygıta ileti sayısı|None|
|devices.totalCihazlar|Toplam aygıtlar (amortismana uğradı)|Sayı|Toplam|IoT hub'ınıza kayıtlı aygıt sayısı|None|
|devices.connectedDevices.allProtocol|Bağlı aygıtlar (amortismana uğradı) |Sayı|Toplam|IoT hub'ınıza bağlı aygıt sayısı|None|
|d2c.telemetry.egress.success|Yönlendirme: telemetri iletileri teslim|Sayı|Toplam|IoT Hub yönlendirmesini kullanarak iletilerin tüm uç noktalara başarıyla teslim ediliş sayısı. İleti birden çok uç noktaya yönlendirilirse, bu değer her başarılı teslimat için bir artar. İleti aynı bitiş noktasına birden çok kez teslim edilirse, bu değer her başarılı teslimat için bir artar.|None|
|d2c.telemetry.egress.düştü|Yönlendirme: telemetri iletileri düştü |Sayı|Toplam|İletilerin, çıkmaz noktalar nedeniyle IoT Hub yönlendirmesi tarafından kaç kez bırakıldığı. Bırakılan iletiler burada teslim edilmedikçe, bu değer geri dönüş rotasına teslim edilen iletileri saymaz.|None|
|d2c.telemetry.egress.orphaned|Yönlendirme: telemetri mesajları öksüz |Sayı|Toplam|İletilerin ioT Hub yönlendirmesi tarafından kaç kez değiştirildikleri, çünkü yönlendirme kurallarına (geri dönüş kuralı dahil) eşleşmedikleri için. |None|
|d2c.telemetry.çıkış.geçersiz|Yönlendirme: telemetri iletileri uyumsuz|Sayı|Toplam|IoT Hub yönlendirmesi, bitiş noktasıyla uyumsuzluk nedeniyle iletileri kaç kez teslim edemedi. Bu değer yeniden denemeleri içermez.|None|
|d2c.telemetry.egress.fallback|Yönlendirme: geri dönüşe teslim edilen iletiler|Sayı|Toplam|IoT Hub yönlendirmesinin geri dönüş rotasıyla ilişkili bitiş noktasına iletileri teslim etme sayısı.|None|
|d2c.endpoints.egress.eventHubs|Yönlendirme: Event Hub'a iletiler|Sayı|Toplam|IoT Hub yönlendirmesinin Olay Hub uç noktalarına iletileri başarıyla teslim etme sayısı.|None|
|d2c.endpoints.latency.eventHubs|Yönlendirme: Olay Hub'ı için ileti gecikmesi|Milisaniye|Ortalama|IoT Hub'ına ileti girişi ile Olay Hub bitiş noktasına ileti girişi arasındaki ortalama gecikme (milisaniye).|None|
|d2c.endpoints.egress.serviceBusQueues|Yönlendirme: Servis Veri Servisi Kuyruğuna gönderilen iletiler|Sayı|Toplam|IoT Hub yönlendirmesinin Hizmet Veri Gönderi sıra uç noktalarına iletileri başarıyla teslim etme sayısı.|None|
|d2c.endpoints.latency.serviceBusQueues|Yönlendirme: Servis Veri Servisi Sırası için ileti gecikmesi|Milisaniye|Ortalama|IoT Hub'a ileti girişi ile Telemetri ileti girişi arasındaki ortalama gecikme (milisaniye) hizmet veri günü sıra bitiş noktasına girer.|None|
|d2c.endpoints.egress.serviceBusTopics|Yönlendirme: Servis Veri Servisi Konusuna iletiler|Sayı|Toplam|IoT Hub yönlendirmesinin Hizmet Veri Gönderi konu uç noktalarına iletileri başarıyla teslim etme sayısı.|None|
|d2c.endpoints.latency.serviceBusTopics|Yönlendirme: Servis Veri Servisi Konusu için ileti gecikmesi|Milisaniye|Ortalama|IoT Hub'a ileti girişi ile Telemetri ileti girişi arasındaki ortalama gecikme (milisaniye) bir Hizmet Veri Merkezi konu bitiş noktasına girer.|None|
|d2c.endpoints.egress.builtIn.events|Yönlendirme: iletilere/etkinliklere iletiler|Sayı|Toplam|IoT Hub yönlendirmesinin yerleşik bitiş noktasına (iletiler/olaylar) iletileri başarıyla teslim etme sayısı.|None|
|d2c.endpoints.latency.builtIn.events|Yönlendirme: iletiler/olaylar için ileti gecikmesi|Milisaniye|Ortalama|IoT Hub'a ileti girişi ile yerleşik bitiş noktasına (ileti/olaylar) telemetri ileti girişi arasındaki ortalama gecikme (milisaniye).|None|
|d2c.endpoints.egress.depolama|Yönlendirme: depolama alanına teslim edilen iletiler|Sayı|Toplam|IoT Hub yönlendirmesinin depolama bitiş noktalarına iletileri başarıyla teslim etme sayısı.|None|
|d2c.endpoints.latency.storage|Yönlendirme: depolama için ileti gecikmesi|Milisaniye|Ortalama|IoT Hub'a ileti girişi ile depolama bitiş noktasına telemetri ileti girişi arasındaki ortalama gecikme (milisaniye).|None|
|d2c.endpoints.egress.storage.bytes|Yönlendirme: depolamaya teslim edilen veriler|Bayt|Toplam|Depolama uç noktalarına teslim edilen veri (bayt) IoT Hub yönlendirme miktarı.|None|
|d2c.endpoints.egress.storage.blobs|Yönlendirme: depoya teslim edilen lekeler|Sayı|Toplam|IoT Hub yönlendirmesinin depolama uç noktalarına blob'lar teslim etme sayısı.|None|
|EventGridDeliveries|Olay Izgara teslimatları(önizleme)|Sayı|Toplam|Olay Grid'de yayınlanan IoT Hub olaylarının sayısı. Başarılı ve başarısız isteklerin sayısı için Sonuç boyutunu kullanın. EventType boyutu olay türünühttps://aka.ms/ioteventgrid)gösterir ( .|Resourceıd<br/>Sonuç<br/>Olay türü|
|EventGridLatency|Olay Izgara gecikmesi (önizleme)|Milisaniye|Ortalama|Iot Hub olayının oluşturulduğu andan Olay Grid'de yayımlandığı zamana kadar olan ortalama gecikme süresi (milisaniye). Bu sayı tüm olay türleri arasında bir ortalamadır. Belirli bir olay türünün gecikmesini görmek için EventType boyutunu kullanın.|Resourceıd<br/>Olay türü|
|d2c.twin.read.success|Cihazlardan başarılı ikiz okuma|Sayı|Toplam|Tüm başarılı cihaz tarafından başlatılan ikiz okuma sayısı.|None|
|d2c.twin.read.failure|Cihazlardan başarısız ikiz okuma|Sayı|Toplam|Tüm başarısız cihaz tarafından başlatılan ikiz okuma sayısı.|None|
|d2c.twin.read.size|Cihazlardan gelen ikiz okumaların yanıt boyutu|Bayt|Ortalama|Tüm başarılı cihaz tarafından başlatılan ikiz okumaların ortalaması, min ve maksimumu.|None|
|d2c.twin.update.success|Cihazlardan başarılı ikiz güncellemeleri|Sayı|Toplam|Tüm başarılı cihaz tarafından başlatılan ikiz güncelleştirmelerinin sayısı.|None|
|d2c.twin.update.failure|Aygıtlardan başarısız ikiz güncelleştirmeleri|Sayı|Toplam|Başarısız olan tüm aygıt tarafından başlatılan ikiz güncelleştirmelerinin sayısı.|None|
|d2c.twin.update.size|Cihazlardan gelen ikiz güncellemelerin boyutu|Bayt|Ortalama|Tüm başarılı cihaz tarafından başlatılan ikiz güncelleştirmelerinin ortalama, min ve maksimum boyutu.|None|
|c2d.methods.success|Başarılı doğrudan yöntem çağrıları|Sayı|Toplam|Tüm başarılı doğrudan yöntem çağrılarının sayısı.|None|
|c2d.methods.failure|Başarısız doğrudan yöntem çağrıları|Sayı|Toplam|Tüm başarısız doğrudan yöntem çağrılarının sayısı.|None|
|c2d.methods.requestSize|Doğrudan yöntem çağrılarının istek boyutu|Bayt|Ortalama|Tüm başarılı doğrudan yöntem isteklerinin ortalaması, min ve maksimumu.|None|
|c2d.methods.responseSize|Doğrudan yöntem çağrılarının yanıt boyutu|Bayt|Ortalama|Tüm başarılı doğrudan yöntem yanıtlarının ortalaması, min ve maksimumu.|None|
|c2d.twin.read.success|Başarılı ikiz arka uçtan okur|Sayı|Toplam|Tüm başarılı arka uç başlatılan ikiz sayısı okur.|None|
|c2d.twin.read.failure|Başarısız ikiz arka uçtan okur|Sayı|Toplam|Tüm başarısız arka uç başlatılan ikiz okuma sayısı.|None|
|c2d.twin.read.size|İkiz okumanın yanıt boyutu arka uçtan okur|Bayt|Ortalama|Tüm başarılı arka uç başlatılan ikiz okumaların ortalaması, min ve max'i.|None|
|c2d.twin.update.success|Arka uçtan başarılı ikiz güncellemeleri|Sayı|Toplam|Tüm başarılı arka uç başlatılan ikiz güncelleştirmelerinin sayısı.|None|
|c2d.twin.update.failure|Arka uçtan başarısız ikiz güncelleştirmeleri|Sayı|Toplam|Tüm başarısız geri uç başlatılan ikiz güncelleştirmelerinin sayısı.|None|
|c2d.twin.update.size|Arka uçtan ikiz güncellemeleriboyutu|Bayt|Ortalama|Tüm başarılı arka uç başlatılan ikiz güncelleştirmelerinin ortalama, min ve maksimum boyutu.|None|
|twinQueries.success|Başarılı ikiz sorgular|Sayı|Toplam|Tüm başarılı ikiz sorguların sayısı.|None|
|twinQueries.failure|Başarısız çift sorgular|Sayı|Toplam|Tüm başarısız ikiz sorguların sayısı.|None|
|twinQueries.resultSize|İkiz sorgular sonuç boyutu|Bayt|Ortalama|Tüm başarılı ikiz sorgularının sonuç boyutunun ortalaması, min ve maksimumu.|None|
|jobs.createTwinUpdateJob.success|İkiz güncelleştirme işlerinin başarılı oluşturmaları|Sayı|Toplam|İkiz güncelleştirme işlerinin tüm başarılı oluşturulmasının sayısı.|None|
|jobs.createTwinUpdateJob.failure|İkiz güncelleştirme işlerinin başarısız oluşturmaları|Sayı|Toplam|İkiz güncelleştirme işlerinin başarısız olan tüm sayısı.|None|
|jobs.createDirectMethodJob.success|Yöntem çağırma işlerinin başarılı oluşturulması|Sayı|Toplam|Doğrudan yöntem çağırma işlerinin tüm başarılı oluşturulmasının sayısı.|None|
|jobs.createDirectMethodJob.failure|Yöntem çağırma işlerinin başarısız oluşturmaları|Sayı|Toplam|Doğrudan yöntem çağırma işlerinin başarısız tüm oluşturma sayısı.|None|
|jobs.listJobs.success|İşleri listelemek için başarılı çağrılar|Sayı|Toplam|İşleri listelemek için yapılan tüm başarılı çağrıların sayısı.|None|
|jobs.listJobs.failure|İşleri listelemek için başarısız çağrılar|Sayı|Toplam|İşleri listelemek için yapılan tüm başarısız çağrıların sayısı.|None|
|jobs.cancelJob.success|Başarılı iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için yapılan tüm başarılı çağrıların sayısı.|None|
|jobs.cancelJob.failure|Başarısız iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için yapılan tüm başarısız çağrıların sayısı.|None|
|jobs.queryJobs.success|Başarılı iş sorguları|Sayı|Toplam|Sorgu işleri için tüm başarılı çağrıların sayısı.|None|
|jobs.queryJobs.failure|Başarısız iş sorguları|Sayı|Toplam|İşleri sorgulamak için başarısız olan tüm çağrıların sayısı.|None|
|jobs.completed|Tamamlanan işler|Sayı|Toplam|Tamamlanan tüm işlerin sayısı.|None|
|jobs.failed|Başarısız işler|Sayı|Toplam|Tüm başarısız işlerin sayısı.|None|
|d2c.telemetri.ingress.sendThrottle|Azaltma hatalarının sayısı|Sayı|Toplam|Cihaz iş letme azaltmalarına bağlı azaltma hatalarının sayısı|None|
|dailyMessageQuotaUsed|Kullanılan toplam ileti sayısı|Sayı|Ortalama|Bugün kullanılan toplam ileti sayısı. Bu, her gün 00:00 UTC'de sıfırlanan kümülatif bir değerdir.|None|
|deviceDataUsage|Toplam cihaz veri kullanımı|Bayt|Toplam|IotHub'a bağlı tüm cihazlara aktarılan baytlar|None|
|cihazDataUsageV2|Toplam cihaz veri kullanımı (önizleme)|Bayt|Toplam|IotHub'a bağlı tüm cihazlara aktarılan baytlar|None|
|totalDeviceCount|Toplam aygıtlar (önizleme)|Sayı|Ortalama|IoT hub'ınıza kayıtlı aygıt sayısı|None|
|bağlıCihaz Sayısı|Bağlı aygıtlar (önizleme)|Sayı|Ortalama|IoT hub'ınıza bağlı aygıt sayısı|None|
|Yapılandırma|Yapılandırma Ölçümleri|Sayı|Toplam|Yapılandırma İşlemleri için Ölçümler|None|

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub ölçümlerine genel bir bakış gördüğünüze göre, Azure IoT Hub'ını yönetme hakkında daha fazla bilgi edinmek için bu bağlantıyı izleyin:

* [Tanılama günlükleri ayarlama](iot-hub-monitor-resource-health.md)

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [IoT Hub geliştirici kılavuzu](iot-hub-devguide.md)

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
