---
title: Azure IoT Hub kotalarını ve azaltmayı anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - IoT Hub'a uygulanan kotaların açıklaması ve beklenen azaltma davranışı.
author: robinsh
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2019
ms.openlocfilehash: 71a5737434e78bc39bccdfeb950e0dbc32ed0052
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284700"
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Referans - IoT Hub kotaları ve azaltma

Bu makalede, bir IoT Hub için kotalar açıklanmaktadır ve azaltmanın nasıl çalıştığını anlamanıza yardımcı olacak bilgiler sağlar.

## <a name="quotas-and-throttling"></a>Kotalar ve azaltma

Her Azure aboneliğinde en fazla 50 IoT hub'ı ve en fazla 1 Ücretsiz hub bulunabilir.

Her IoT hub'ı belirli bir katmanda belirli bir birim sayısıyla sağlanır. Birimlerin katmanı ve sayısı, gönderebileceğiniz en yüksek günlük ileti kotasını belirler. Günlük kotayı hesaplamak için kullanılan ileti boyutu, ücretsiz bir katman hub'ı için 0,5 KB ve diğer tüm katmanlar için 4KB'dir. Daha fazla bilgi için [Azure IoT Hub Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/iot-hub/)bakın.

Katman, IoT Hub'ın tüm işlemlerde uyguladığı azaltma sınırlarını da belirler.

### <a name="iot-plug-and-play"></a>IoT Tak Çalıştır

Genel önizleme sırasında, IoT Tak ve Çalıştır aygıtları arabirim başına ayrı iletiler gönderir ve bu da ileti kotanıza doğru sayılan ileti sayısını artırabilir.

## <a name="operation-throttles"></a>Operasyon gazları

Operasyon azaltmadakika aralıklarında uygulanan ve kötüye kullanımı önlemek için tasarlanmıştır hız sınırlamaları vardır. Ayrıca [trafik şekillendirmetabidir.](#traffic-shaping)

Aşağıdaki tabloda zorlanan azaltmalar gösterilmektedir. Değerler tek bir hub'a başvurur.

| Kısıtlama | Ücretsiz, B1 ve S1 | B2 ve S2 | B3 ve S3 | 
| -------- | ------- | ------- | ------- |
| [Kimlik kayıt işlemleri](#identity-registry-operations-throttle) (oluşturma, alma, listeletmek, güncellemek, silme) | 1.67/sn/ünite (100/dk/birim) | 1.67/sn/ünite (100/dk/birim) | 83.33/sn/ünite (5.000/dk/birim) |
| [Yeni aygıt bağlantıları](#device-connections-throttle) (bu sınır toplam _bağlantı_sayısı için değil, yeni bağlantı hızı için geçerlidir) | 100/sn veya 12/sn/birimden daha yüksek <br/> Örneğin, iki S1 birimi\*2 12 = 24 yeni bağlantı/sn'dir, ancak birimlerinizde en az 100 yeni bağlantı/sn vardır. Dokuz S1 birimiyle, birimlerinizde 108 yeni\*bağlantı/sn (9 12) vardır. | 120 yeni bağlantı/sn/birim | 6.000 yeni bağlantı/sn/birim |
| Cihazdan buluta gönderim | 100'den fazla gönderme işlemi/sn veya 12 gönderme işlemi/sn/birim <br/> Örneğin, iki S1 birimi\*2 12 = 24/sn'dir, ancak birimlerinizde en az 100 gönderme işlemi/sn'dir. Dokuz S1 birimiyle, birimlerinizde 108 gönderme\*işlemi/sn (9 12) vardır. | 120 gönderme işlemleri/sn/birim | 6.000 gönderme işlemleri/sn/birim |
| Bulut-aygıta<sup>1</sup> gönderir | 1.67 gönderme işlemleri/sn/birim (100 mesaj/dk/birim) | 1.67 gönderme işlemleri/sn/birim (100 gönderme işlemleri/dk/birim) | 83.33 operasyon/sn/birim gönderme (5.000 gönderme işlemleri/dk/birim) |
| Bulut-aygıt1 alır<sup>1</sup> <br/> (yalnızca aygıt HTTPS kullandığında)| 16.67 operasyon alma/sn/birim (1.000 alma işlemleri/min/birim) | 16.67 operasyon alma/sn/birim (1.000 alma işlemleri/min/birim) | 833.33 operations/sn/unit (50.000 alma işlemleri/min/birim) |
| Dosya yükleme | 1.67 dosya yükleme başlatmaları/sn/birim (100/dk/birim) | 1.67 dosya yükleme başlatmaları/sn/birim (100/dk/birim) | 83.33 dosya yükleme başlatmaları/sn/birim (5.000/dk/birim) |
| Doğrudan yöntemler<sup>1</sup> | 160KB/sn/ünite<sup>2</sup> | 480KB/sn/ünite<sup>2</sup> | 24MB/sn/ünite<sup>2</sup> | 
| Sorgular | 20/dk/ünite | 20/dk/ünite | 1.000/dk/birim |
| İkiz (cihaz ve modül)<sup>1</sup> okur | 100/sn | 100/sn veya 10/sn/birimden daha yüksek | 500/sn/birim |
| İkiz güncellemeler (cihaz ve modül)<sup>1</sup> | 50/sn | 50/sn veya 5/sn/birimden daha yüksek | 250/sn/ünite |
| İş işleri işlemleri<sup>1</sup> <br/> (oluşturma, güncelleştirme, listeleme, silme) | 1.67/sn/ünite (100/dk/birim) | 1.67/sn/ünite (100/dk/birim) | 83.33/sn/ünite (5.000/dk/birim) |
| İşler cihazı işlemleri<sup>1</sup> <br/> (ikiz güncelleme, doğrudan yöntem çağırmak) | 10/sn | 10/sn veya 1/sn/birimden daha yüksek | 50/sn/ünite |
| Yapılandırmalar ve kenar dağıtımları<sup>1</sup> <br/> (oluşturma, güncelleştirme, listeleme, silme) | 0.33/sn/ünite (20/dk/birim) | 0.33/sn/ünite (20/dk/birim) | 0.33/sn/ünite (20/dk/birim) |
| Cihaz akışı başlatma oranı<sup>1</sup> | 5 yeni akış/sn | 5 yeni akış/sn | 5 yeni akış/sn |
| Aynı anda bağlanan aygıt akışlarının maksimum sayısı<sup>1</sup> | 50 | 50 | 50 |
| Maksimum cihaz akışı veri aktarımı<sup>1</sup> (günlük toplam hacim) | 300 MB | 300 MB | 300 MB |

<sup>1.1.2</sup> Bu özellik, IoT Hub'ın temel katmanında kullanılamaz. Daha fazla bilgi [için, doğru IoT Hub'ını nasıl seçeceğinizi](iot-hub-scaling.md)öğrenin. <br/><sup>2.000</sup> Daraltma metre boyutu 4 KB'dir.

### <a name="throttling-details"></a>Azaltma Ayrıntıları

* Sayaç boyutu, azaltma limitinizin ne kadar artarak tüketilmesini belirler. Doğrudan aramanızın yükü 0 ile 4 KB arasındaysa, 4 KB olarak sayılır. 160 KB/sn/birim sınırına basmadan önce birim başına saniyede 40 arama yapabilirsiniz.

   Benzer şekilde, yükünüz 4 KB ile 8 KB arasındaysa, her arama 8 KB'dir ve maksimum limite basmadan önce birim başına saniyede 20 arama yapabilirsiniz.

   Son olarak, taşıma yükünüz 156KB ile 160 KB arasındaysa, 160 KB/sn/birim sınırına basmadan önce hub'ınızdaki birim başına saniyede yalnızca 1 arama yapabilirsiniz.

*  S2 katmanı için *Jobs aygıt işlemleri (ikizi güncelleştir, doğrudan çağır)* için, 50/sn/birim yalnızca işleri kullanarak yöntemleri çağırdığınızda geçerlidir. Doğrudan yöntemleri doğrudan çağırırsanız, 24 MB/sn/birim (S2 için) orijinal azaltma sınırı uygulanır.

*  **Kota,** hub'ınızda *günlük*gönderebileceğiniz toplam ileti sayısıdır. Hub'ınızın kota [sınırını, IoT Hub fiyatlandırma sayfasındatoplam](https://azure.microsoft.com/pricing/details/iot-hub/) **ileti sayısı /gün** sütununun altında bulabilirsiniz.

*  Buluttan cihaza ve aygıttan buluta azaltmalarınız, ileti gönderme nizin maksimum *hızını* belirler -- 4 KB'den bağımsız olarak ileti sayısı. Her ileti [maksimum ileti boyutu](iot-hub-devguide-quotas-throttling.md#other-limits)256 KB'ye kadar olabilir.

*  Aramalarınızı daraltma limitlerine çarpmamak/aşmamak için daraltmak iyi bir uygulamadır. Sınıra uğrarsa, IoT Hub hata kodu 429 ile yanıt verir ve istemci geri çekilip yeniden denemelidir. Bu sınırlar hub başına (veya bazı durumlarda hub/birim başına) Daha fazla bilgi için bağlantı [ve güvenilir ileti/yeniden deneme desenleri yönet'e](iot-hub-reliability-features-in-sdks.md#retry-patterns)bakın.

### <a name="traffic-shaping"></a>Trafik şekillendirme

Yoğun trafiği karşılamak için, IoT Hub gazın üzerindeki istekleri sınırlı bir süre için kabul eder. Bu isteklerin ilk birkaçı hemen işlenir. Ancak, istek sayısı azaltmayı ihlal etmeye devam ederse, IoT Hub istekleri sıraya yerleştirmeye başlar ve sınır oranında işlenir. Bu etkinin trafik *şekillendirme*denir. Ayrıca, bu sıranın boyutu sınırlıdır. Gaz ihlali devam ederse, sonunda sıra dolur ve IoT Hub istekleri ni `429 ThrottlingException`'ile reddetmeye başlar.

Örneğin, S1 IoT Hub'ınıza saniyede 200 aygıttan buluta ileti göndermek için simüle edilmiş bir aygıt kullanırsınız (100/sn D2C gönderme sınırı vardır). İlk veya iki dakika için iletiler hemen işlenir. Ancak, aygıt gaz sınırından daha fazla ileti göndermeye devam ettiği için, IoT Hub saniyede yalnızca 100 ileti işlemeye başlar ve geri kalanını sıraya koyar. Gecikme gecikmesi arttığını fark etmeye başlarsınız. Sonunda, sıra doldukça almaya `429 ThrottlingException` başlarsınız ve [IoT Hub'ın ölçümlerinde](iot-hub-metrics.md) "gaz hatalarının sayısı" artmaya başlar.

### <a name="identity-registry-operations-throttle"></a>Kimlik kayıt işlemleri azaltma

Aygıt kimlik kayıt işlemleri, aygıt yönetimi ve sağlama senaryolarında çalışma zamanı kullanımı için tasarlanmıştır. Çok sayıda aygıt kimliklerinin okunması veya [güncellenmesi, alma ve dışa aktarma işleri](iot-hub-devguide-identity-registry.md#import-and-export-device-identities)yoluyla desteklenir.

### <a name="device-connections-throttle"></a>Cihaz bağlantıları nın kesilmesi

*Aygıt bağlantıları* azaltma, bir IoT hub'ı ile yeni aygıt bağlantılarının kurulma hızını yönetir. *Aygıt bağlantıları* nın azaltması, aynı anda bağlanan maksimum aygıt sayısını yönetmez. *Aygıt bağlantılarının* hız azaltma oranı, IoT hub'ı için sağlanan birim sayısına bağlıdır.

Örneğin, tek bir S1 birimi satın alırsanız, saniyede 100 bağlantı nın bir gaz ını alırsınız. Bu nedenle, 100.000 aygıtı bağlamak için en az 1.000 saniye (yaklaşık 16 dakika) sürer. Ancak, kimlik kaydınızda kayıtlı aygıtlar kadar aynı anda bağlı aygıta sahip olabilirsiniz.

## <a name="other-limits"></a>Diğer sınırlar

IoT Hub diğer operasyonel sınırları uygular:

| İşlem | Sınır |
| --------- | ----- |
| Cihazlar | Tek bir IoT hub'ına kaydedilebilen toplam aygıt sayısı ve modül sayısı 1.000.000'dir. Bu sınırı artırmanın tek yolu [Microsoft Destek'e](https://azure.microsoft.com/support/options/)başvurmaktır.|
| Dosya karşıya yüklemeleri | Cihaz başına 10 eşzamanlı dosya yüklemesi. |
| İş İlanları<sup>1</sup> | Maksimum eşzamanlı işler 1 (Ücretsiz ve S1 için), 5 (S2 için) ve 10 (S3 için). Ancak, en iyi eşzamanlı [aygıt alma/dışa aktarma işleri](iot-hub-bulk-identity-mgmt.md) tüm katmanlar için 1'dir. <br/>İş geçmişi 30 güne kadar saklanır. |
| Ek uç noktalar | Ücretli SKU hub'larının 10 ek uç noktası olabilir. Ücretsiz SKU hub'larının bir ek bitiş noktası olabilir. |
| İleti yönlendirme sorguları | Ücretli SKU hub'larının 100 yönlendirme sorgusu olabilir. Ücretsiz SKU hub'larının beş yönlendirme sorgusu olabilir. |
| İleti zenginleştirmeleri | Ücretli SKU hub'ları en fazla 10 ileti zenginleştirme sahip olabilir. Ücretsiz SKU hub'ları en fazla 2 mesaj zenginleştirme sahip olabilir.|
| Aygıt-bulut mesajlaşma | Maksimum ileti boyutu 256 KB |
| Bulut-aygıt<sup>mesajlaşma1</sup> | Maksimum ileti boyutu 64 KB. Teslim için bekleyen maksimum ileti, aygıt başına 50'dir. |
| Doğrudan yöntem<sup>1</sup> | Maksimum doğrudan yöntem yük boyutu 128 KB'dir. |
| Otomatik cihaz ve modül konfigürasyonları<sup>1</sup> | Ücretli SKU hub başına 100 yapılandırma. Ücretsiz SKU hub başına 20 yapılandırma. |
| IoT Edge otomatik dağıtımlar<sup>1</sup> | Dağıtım başına 50 modül. Ücretli SKU hub başına 100 dağıtım (katmanlı dağıtımlar dahil). Ücretsiz SKU hub başına 10 dağıtım. |
| İkizler<sup>1</sup> | İstenilen özelliklerin ve bildirilen özelliklerin maksimum boyutu her biri 32 KB'dir. Etiketler bölümünün maksimum boyutu 8 KB'dir. |
| Paylaşılan erişim ilkeleri | Maksimum paylaşılan erişim ilkesi sayısı 16'dır |

<sup>1.1.2</sup> Bu özellik, IoT Hub'ın temel katmanında kullanılamaz. Daha fazla bilgi [için, doğru IoT Hub'ını nasıl seçeceğinizi](iot-hub-scaling.md)öğrenin.

## <a name="increasing-the-quota-or-throttle-limit"></a>Kota veya azaltma limitinin artırılması

Herhangi bir zamanda, [bir IoT hub'ında sağlanan birimlerin sayısını artırarak](iot-hub-upgrade.md)kotaları artırabilir veya azaltma limitlerini artırabilirsiniz.

## <a name="latency"></a>Gecikme süresi

IoT Hub tüm işlemler için düşük gecikme alanı sağlamaya çalışır. Ancak, ağ koşulları ve diğer öngörülemeyen faktörler nedeniyle belirli bir gecikme yi garanti edemez. Çözümünüzü tasarlarken şunları

* Herhangi bir IoT Hub işleminin maksimum gecikme sonu hakkında herhangi bir varsayımda bulunmaktan kaçının.
* IoT hub'ınızı aygıtlarınıza en yakın Azure bölgesinde sağlayın.
* Aygıtta veya aygıta yakın bir ağ geçidinde gecikmeye duyarlı işlemleri gerçekleştirmek için Azure IoT Edge'i kullanmayı düşünün.

Birden çok IoT Hub birimi, daha önce açıklandığı gibi azaltmayı etkiler, ancak ek gecikme avantajları veya garantileri sağlamaz.

Çalışma gecikmesinde beklenmeyen artışlar görürseniz, [Microsoft Destek'e](https://azure.microsoft.com/support/options/)başvurun.

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub azaltma davranışı derinlemesine bir tartışma için, blog yazısı [IoT Hub azaltma bakın ve .](https://azure.microsoft.com/blog/iot-hub-throttling-and-you/)

Bu IoT Hub geliştirici kılavuzundaki diğer başvuru konuları şunlardır:

* [IoT Hub uç noktaları](iot-hub-devguide-endpoints.md)
