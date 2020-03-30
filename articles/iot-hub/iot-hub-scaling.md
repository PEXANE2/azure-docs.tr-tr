---
title: Azure IoT Hub ölçekleme | Microsoft Dokümanlar
description: IoT hub'ınızı, beklenen ileti iş inizi ve istediğiniz özellikleri desteklemek için nasıl ölçeklendirilir? Her katman için desteklenen iş tamsayısının bir özetini ve parçalama seçenekleriiçerir.
author: wesmc7777
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: wesmc
ms.openlocfilehash: ecc1ae5138fe5a1e42fed9be2e31b5afa8b6d5b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79497509"
---
# <a name="choose-the-right-iot-hub-tier-for-your-solution"></a>Çözümünüz için doğru IoT Hub katmanını seçin

Her IoT çözümü farklıdır, bu nedenle Azure IoT Hub fiyatlandırma ve ölçeğe dayalı çeşitli seçenekler sunar. Bu makale, IoT Hub gereksinimlerinizi değerlendirmenize yardımcı olmak içindir. IoT Hub katmanları hakkında fiyatlandırma bilgileri için [IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub)fiyatlandırması'na bakın.

Hangi IoT Hub katmanının çözümünüz için doğru olduğuna karar vermek için kendinize iki soru sorun:

**Hangi özellikleri kullanmayı planlıyorum?**

Azure IoT Hub, destekledikleri özellik sayısında farklılık gösteren temel ve standart olmak üzere iki katman sunar. IoT çözümünüz aygıtlardan veri toplama ve merkezi olarak analiz etme üzerine kuruluysa, temel katman muhtemelen sizin için doğru olacaktır. IoT aygıtlarını uzaktan denetlemek veya iş yüklerinizi aygıtlara dağıtmak için daha gelişmiş yapılandırmalar kullanmak istiyorsanız, standart katmanı göz önünde bulundurmalısınız. Özellikleri her katmanda yer alan ayrıntılı bir dökümü için [Temel ve standart katmanlara](#basic-and-standard-tiers)devam edin.

**Günlük ne kadar veri taşımayı planlıyorum?**

Her IoT Hub katmanı, herhangi bir günde ne kadar veri veri verisi işleyebilir lerine bağlı olarak üç boyutta kullanılabilir. Bu boyutlar sayısal olarak 1, 2 ve 3 olarak tanımlanır. Örneğin, düzey 1 IoT hub'ının her birimi günde 400 bin ileti işleyebilirken, düzey 3 birimi 300 milyon iletiişleyebilir. Veri yönergeleri hakkında daha fazla bilgi için, İleti iş bilgili olmaya devam [edin.](#message-throughput)

## <a name="basic-and-standard-tiers"></a>Temel ve standart katmanlar

IoT Hub'ın standart katmanı tüm özellikleri etkinleştirir ve çift yönlü iletişim özelliklerinden yararlanmak isteyen tüm IoT çözümleri için gereklidir. Temel katman, özelliklerin alt kümesini kullanıma sunar ve yalnızca cihazlardan buluta tek yönlü iletişim ihtiyacı duyan IoT çözümlerine yöneliktir. İki katman da aynı güvenlik ve kimlik doğrulaması özelliklerini sunar.

IoT Hub başına bir katman içinde yalnızca bir [baskı](https://azure.microsoft.com/pricing/details/iot-hub/) türü seçilebilir. Örneğin, birden çok S1 birimine sahip bir IoT Hub'ı oluşturabilirsiniz, ancak S1 ve S2 gibi farklı sürümlerden birimlerin karışımıyla oluşturamaz.

| Özellik | Temel katman | Ücretsiz/Standart katman |
| ---------- | ---------- | ------------- |
| [Cihazdan buluta telemetri](iot-hub-devguide-messaging.md) | Evet | Evet |
| [Cihaz başına kimlik](iot-hub-devguide-identity-registry.md) | Evet | Evet |
| [İleti yönlendirme,](iot-hub-devguide-messages-read-custom.md) [ileti zenginleştirmeleri](iot-hub-message-enrichments-overview.md)ve [Olay Izgara tümleştirmesi](iot-hub-event-grid.md) | Evet | Evet |
| [HTTP, AMQP ve MQTT protokolleri](iot-hub-devguide-protocols.md) | Evet | Evet |
| [Cihaz Sağlama Hizmeti](../iot-dps/about-iot-dps.md) | Evet | Evet |
| [İzleme ve tanılama](iot-hub-monitor-resource-health.md) | Evet | Evet |
| [Bulut-aygıt mesajlaşma](iot-hub-devguide-c2d-guidance.md) |   | Evet |
| [Cihaz ikizleri](iot-hub-devguide-device-twins.md), [Modül ikizleri](iot-hub-devguide-module-twins.md)ve [Cihaz yönetimi](iot-hub-device-management-overview.md) |   | Evet |
| [Aygıt akışları (önizleme)](iot-hub-device-streams-overview.md) |   | Evet |
| [Azure IoT Edge](../iot-edge/about-iot-edge.md) |   | Evet |
| [IoT Tak ve Çalıştır Önizleme](../iot-pnp/overview-iot-plug-and-play.md) |   | Evet |

IoT Hub ayrıca test ve değerlendirme için direnen ücretsiz bir katman da sunar. Standart katmanın tüm özelliklerine sahiptir, ancak mesajlaşma izinleri sınırlıdır. Ücretsiz katmandan temel veya standart yükseltme yapamazsınız.

## <a name="partitions"></a>Bölümler

Azure IoT Hub'ları, [Bölümler](../event-hubs/event-hubs-features.md#partitions)de dahil olmak üzere [Azure Etkinlik Hub'larının](../event-hubs/event-hubs-features.md)birçok temel bileşenini içerir. IoT Hub'ları için olay akışları genellikle çeşitli IoT aygıtları tarafından bildirilen gelen telemetri verileriyle doldurulur. Olay akışının bölümlemi, olay akışlarına aynı anda okuma ve yazma yaparken oluşan çekişmeleri azaltmak için kullanılır.

IoT Hub oluşturulduğunda bölüm sınırı seçilir ve değiştirilemez. Temel katman IoT Hub'ı ve standart katman IoT Hub'ı için maksimum bölüm sınırı 32'dir. Çoğu IoT hub'ı yalnızca 4 bölüme ihtiyaç duyar. Bölümleri belirleme hakkında daha fazla bilgi için Olay Hub'ları SSS'ye bakın [kaç bölüme ihtiyacım var?](../event-hubs/event-hubs-faq.md#how-many-partitions-do-i-need)

## <a name="tier-upgrade"></a>Katman yükseltme

IoT hub'ınızı oluşturduktan sonra, mevcut işlemlerinizi kesintiye uğratmadan temel katmandan standart katmana yükseltebilirsiniz. Daha fazla bilgi için [IoT hub'ınızı nasıl yükselteceğimize](iot-hub-upgrade.md)bakın.

Temel katmandan standart katmana geçiş yaptığınızda bölüm yapılandırması değişmeden kalır.

> [!NOTE]
> Ücretsiz katman temel veya standart yükseltme desteklemez.

## <a name="iot-hub-rest-apis"></a>IoT Hub REST API'leri

Desteklenen özelliklerarasındaki IoT Hub'ın temel ve standart katmanları arasındaki fark, bazı API çağrılarının temel katman hub'larıyla çalışmadığı anlamına gelir. Aşağıdaki tablo, hangi API'lerin kullanılabildiği gösterir:

| API | Temel katman | Ücretsiz/Standart katman |
| --- | ---------- | ------------- |
| [Cihazı silme](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/deletedevice) | Evet | Evet |
| [Cihazı alın](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/getdevice) | Evet | Evet |
| [Modülü silme](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/deletemodule) | Evet | Evet |
| [Modülü alın](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/getmodule) | Evet | Evet |
| [Kayıt defteri istatistiklerini alın](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/getdevicestatistics) | Evet | Evet |
| [Hizmet istatistiklerini alın](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/getservicestatistics) | Evet | Evet |
| [Aygıt oluşturma veya güncelleme](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/createorupdatedevice) | Evet | Evet |
| [Modül oluşturma veya güncelleme](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/createorupdatemodule) | Evet | Evet |
| [IoT Hub sorgula](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/queryiothub) | Evet | Evet |
| [Dosya yüklemesi SAS URI oluşturma](https://docs.microsoft.com/rest/api/iothub/device/createfileuploadsasuri) | Evet | Evet |
| [Aygıta bağlı bildirim alma](https://docs.microsoft.com/rest/api/iothub/device/receivedeviceboundnotification) | Evet | Evet |
| [Aygıt olayını gönderme](https://docs.microsoft.com/rest/api/iothub/device/senddeviceevent) | Evet | Evet |
| Modül olayını gönder | Yalnızca AMQP ve MQTT | Yalnızca AMQP ve MQTT |
| [Dosya yükleme durumunu güncelleştir](https://docs.microsoft.com/rest/api/iothub/device/updatefileuploadstatus) | Evet | Evet |
| [Toplu cihaz çalışması](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/bulkdevicecrud) | Evet, IoT Edge özellikleri hariç | Evet |
| [Alma dışa aktarma işini iptal etme](https://docs.microsoft.com/rest/api/iothub/service/jobclient/cancelimportexportjob) | Evet | Evet |
| [Alma dışa aktarma işi oluşturma](https://docs.microsoft.com/rest/api/iothub/service/jobclient/createimportexportjob) | Evet | Evet |
| [İthalat ihracat işi alın](https://docs.microsoft.com/rest/api/iothub/service/jobclient/getimportexportjob) | Evet | Evet |
| [İthalat ihracat işleri alın](https://docs.microsoft.com/rest/api/iothub/service/jobclient/getimportexportjobs) | Evet | Evet |
| [Temizleme komut sırası](https://docs.microsoft.com/rest/api/iothub/service/registrymanager/purgecommandqueue) |   | Evet |
| [Aygıt ikizini alın](https://docs.microsoft.com/rest/api/iothub/service/twin/getdevicetwin) |   | Evet |
| [Modül ikizi alın](https://docs.microsoft.com/rest/api/iothub/service/twin/getmoduletwin) |   | Evet |
| [Aygıt yöntemini çağırma](https://docs.microsoft.com/rest/api/iothub/service/devicemethod/invokedevicemethod) |   | Evet |
| [Aygıt ikizini güncelleştir](https://docs.microsoft.com/rest/api/iothub/service/twin/updatedevicetwin) |   | Evet |
| [Modül ikizini güncelleştir](https://docs.microsoft.com/rest/api/iothub/service/twin/updatemoduletwin) |   | Evet |
| [Aygıt alıtosı bildirimini terk etme](https://docs.microsoft.com/rest/api/iothub/device/abandondeviceboundnotification) |   | Evet |
| [Cihaza bağlı bildirimi tamamlayın](https://docs.microsoft.com/rest/api/iothub/device/completedeviceboundnotification) |   | Evet |
| [İşi iptal etme](https://docs.microsoft.com/rest/api/iothub/service/jobclient/canceljob) |   | Evet |
| [İş oluştur](https://docs.microsoft.com/rest/api/iothub/service/jobclient/createjob) |   | Evet |
| [İş bul](https://docs.microsoft.com/rest/api/iothub/service/jobclient/getjob) |   | Evet |
| [İşleri sorgula](https://docs.microsoft.com/rest/api/iothub/service/jobclient/queryjobs) |   | Evet |

## <a name="message-throughput"></a>İleti girişi

IoT Hub çözümlerini boyutlandırmanın en iyi yolu trafiği birim başına değerlendirmektir. Özellikle, aşağıdaki işlem kategorileri için gerekli tepe iş artışını göz önünde bulundurun:

* Cihazdan buluta iletiler
* Buluttan cihaza iletiler
* Kimlik kayıt defteri işlemleri

Trafik, IoT hub'ınız için birim başına olarak ölçülür. Bir IoT hub'ı oluşturduğunuzda, katmanını ve sürümünü seçer ve kullanılabilir birim sayısını ayarlarsınız. B1, B2, S1 veya S2 sürümü için en fazla 200 adet veya B3 veya S3 sürümü için en fazla 10 birim satın alabilirsiniz. IoT hub'ınız oluşturulduktan sonra, sürümünde bulunan birim sayısını değiştirebilir, katmanındaki sürümler arasında yükseltebilir veya düşürebilir veya temel katmandan (B1'den S1'e) mevcut işlemlerinizi kesintiye uğratmadan yükseltebilirsiniz. Daha fazla bilgi için [IoT hub'ınızı nasıl yükselteceğimize](iot-hub-upgrade.md)bakın.  

Her kademenin trafik yeteneklerine bir örnek olarak, aygıttan buluta iletiler aşağıdaki sürekli iş ortası yönergelerini izler:

| Katman sürümü | Sürekli iş artışı | Sürekli gönderme oranı |
| --- | --- | --- |
| B1, S1 |Birim başına 1111 KB/dakika'ya kadar<br/>(1,5 GB/gün/birim) |Birim başına ortalama 278 ileti/dakika<br/>(Birim başına 400.000 mesaj/gün) |
| B2, S2 |Birim başına 16 MB/dakikaya kadar<br/>(22,8 GB/gün/birim) |Birim başına ortalama 4.167 ileti/dakika<br/>(Birim başına 6 milyon mesaj/gün) |
| B3, S3 |Birim başına 814 MB/dakikaya kadar<br/>(1144,4 GB/gün/birim) |Birim başına ortalama 208.333 ileti/dakika<br/>(Birim başına 300 milyon ileti/gün) |

Aygıttan buluta iş hacmi, bir IoT çözümü tasarlarken göz önünde bulundurmanız gereken ölçümlerden yalnızca biridir. Daha kapsamlı bilgi için [IoT Hub kotaları ve azaltmalarına](iot-hub-devguide-quotas-throttling.md)bakın.

### <a name="identity-registry-operation-throughput"></a>Kimlik kayıt işlemi işletilmesi

IoT Hub kimlik kayıt işlemleri çoğunlukla aygıt sağlama ile ilgili olduğundan, çalışma zamanı işlemleri olması gerekmez.

Belirli seri performans numaraları için [IoT Hub kotaları ve azaltmalara](iot-hub-devguide-quotas-throttling.md)bakın.

## <a name="auto-scale"></a>Otomatik ölçeklendirme

IoT hub'ınızda izin verilen ileti sınırına yaklaşıyorsanız, aynı IoT Hub katmanında bir IoT Hub birimini otomatik olarak ölçeklendirmek için bu [adımları](https://azure.microsoft.com/resources/samples/iot-hub-dotnet-autoscale/) kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* IoT Hub özellikleri ve performans ayrıntıları hakkında daha fazla bilgi [için, IoT Hub fiyatlandırmaveya](https://azure.microsoft.com/pricing/details/iot-hub) [IoT Hub kotaları ve azaltma bölümüne](iot-hub-devguide-quotas-throttling.md)bakın.

* IoT Hub katmanınızı değiştirmek [için, IoT hub'ınızı Yükselt'teki](iot-hub-upgrade.md)adımları izleyin.
