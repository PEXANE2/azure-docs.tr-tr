---
title: IoT Edge SSS üzerinde canlı video analizi-Azure
description: Bu konu, IoT Edge SSS üzerinde canlı video analizlerine yanıt verir.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 0a6c1c0f26116227454fa0968264644ea7a43178
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84261327"
---
# <a name="frequently-asked-questions-faqs"></a>Sık sorulan sorular (SSS)

Bu konu, IoT Edge SSS üzerinde canlı video analizlerine yanıt verir.

## <a name="general"></a>Genel

Grafik topolojisi tanımında kullanılabilecek sistem değişkenleri nelerdir?

|Değişken   |Açıklama|
|---|---|
|[System. DateTime](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/linq/system-datetime-methods)|Genellikle günün tarih ve saati olarak ifade edilen bir anlık zamanı temsil eder.|
|System. Graphtopologyıname   |Bir medya grafiği topolojisini temsil eder, grafiğin şeması saklar.|
|System. Graphınstancename|  Bir medya grafiği örneğini temsil eder, parametre değerlerini tutar ve topolojiye başvurur.|

## <a name="configuration-and-deployment"></a>Yapılandırma ve dağıtım

Medya Edge modülünü bir Windows 10 cihazına dağıtabilir miyim?
    * Evet. [Windows 10 ' da Linux kapsayıcılarındaki](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/linux-containers)makaleye bakın.

## <a name="capture-from-ip-camera-and-rtsp-settings"></a>IP kamerası ve RTSP ayarlarından yakala

* Video akışı göndermek için cihazımda belirli bir SDK kullanmam gerekiyor mu?
    * Hayır. IoT Edge üzerindeki canlı video analizi, RTSP video akış protokolü (çoğu IP kameralarından desteklenir) kullanılarak medya yakalamayı destekler.
* IoT Edge RTMP veya kesintisiz (Media Services canlı bir olay gibi) kullanarak canlı video analizlerine medya gönderebilirim miyim?
    * Hayır. LVA yalnızca IP kameralarından video yakalamak için RTSP 'yi destekler.
    * TCP/HTTP üzerinden RTSP akışını destekleyen tüm kamera çalışmalıdır. 
* Grafik örneğinde RTSP kaynak URL’sini sıfırlayabilir veya güncelleştirebilir miyim?
    * Evet, grafik örneği etkin değil durumunda.  
* Test ve geliştirme sırasında kullanılmak üzere kullanılabilir bir RTSP simülatörü var mı?
    * Evet. Hızlı başlar ve öğreticilerde öğrenme sürecini desteklemek için kullanılabilecek bir [RTSP simülatör](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) Edge modülü vardır. Bu modül mümkün olan en iyi çabayla sağlanmıştır ve her zaman kullanılamayabilir. Bunu birkaç saatten uzun bir süre kullanmak kesinlikle önerilir. Üretim dağıtımı için plan yapmadan önce gerçek RTSP kaynağınıza yönelik teste yatırım yapmanız gerekir.
* Uçta IP kameraları için ONVIF bulmayı destekliyor musunuz?
    * Hayır, uçtaki cihazlar için ONVIF bulma desteklenmez.

## <a name="streaming-and-playback"></a>Akış ve kayıttan yürütme

* Ucundan AMS 'ye kaydedilen varlıklar, HLS veya DASH gibi Media Services akış teknolojileri kullanılarak kayıttan çalınabilir mi?
    * Evet. Kayıtlı varlıklar, Azure Media Services başka herhangi bir varlık gibi akışla eklenebilir. İçeriği akışa almak için, oluşturulmuş bir akış uç noktası ve çalışır durumda olmalıdır. Standart akış Konumlandırıcı oluşturma işleminin kullanılması, herhangi bir uyumlu oynatıcı çerçevesine akış için bir HLS veya DASH bildirimine erişmenizi sağlayacaktır. Yayımlama HLS veya DASH bildirimleri oluşturma hakkında ayrıntılı bilgi için bkz. [dinamik paketleme](../latest/dynamic-packaging-overview.md).
* Arşivlenmiş bir varlık üzerinde Media Services standart içerik koruması ve DRM özelliklerini kullanabilir miyim?
    * Evet. Standart dinamik şifreleme içerik koruması ve DRM özelliklerinin hepsi, bir medya grafiğinden kaydedilen varlıklarda kullanılabilir.
* Kayıtlı varlıkların içeriğini görüntülemek için hangi oyuncuları kullanabilirim?
   * Uyumlu Apple HTTP Canlı Akışı (HLS) sürüm 3 veya sürüm 4 ' ü destekleyen tüm standart oyuncular desteklenir. Ayrıca, MPEG-DASH kayıttan yürütme yeteneğine sahip olan her oyuncu da desteklenir.
    Test için önerilen oyuncular şunlardır:

    * [Azure Media Player](../latest/use-azure-media-player.md)
    * [HLS.js](https://hls-js.netlify.app/demo/)
    * [Video.js](https://videojs.com/)
    * [Dash.js](https://github.com/Dash-Industry-Forum/dash.js/wiki)
    * [Shaka oynatıcı](https://github.com/google/shaka-player)
    * [Exoçalar](https://github.com/google/ExoPlayer)
    * [Apple Native HTTP Canlı Akışı](https://developer.apple.com/streaming/)
    * HTML5 video oynatıcı 'da geliştirilmiş Edge, Chrome veya Safari
    * HLS veya DASH oynatmayı destekleyen ticari oyuncular
* Medya grafiği varlığı akışı için sınırlamalar nelerdir?
    * Medya grafiğinden canlı veya kayıtlı bir varlık akışı, medya & eğlence, OTT ve yayın müşterileri için isteğe bağlı ve canlı akış için Media Services desteklediği yüksek ölçekli altyapıyı ve akış uç noktasını kullanır. Bu, Azure CDN, Verizon veya Akamai ' yi hızla ve kolayca etkinleştirebilmeniz için, içeriğinizi birkaç görüntüleyiciye kadar küçük veya en fazla milyonlarca veya senaryonuza bağlı olarak daha az milyonlarca hale getirebileceğinizi gösterir.

    İçerik hem Apple HTTP Canlı Akışı (HLS) hem de MPEG-DASH kullanılarak teslim edilebilir.

## <a name="monitoring-and-metrics"></a>İzleme ve ölçümler

* Event Grid kullanarak kenar üzerinde medya grafiğini izleyebilir miyim?
    * Hayır. Şu anda Event Grid desteklenmiyor.
* Bulutta veya kenarda medya grafiklerimin sistem durumunu, ölçümlerini ve performansını görüntülemek için Azure Izleyicisini kullanabilir miyim?
    * Hayır.
* Media Services IoT Edge modülünü izlemeyi kolaylaştırmak için herhangi bir araç var mı?
    * Visual Studio Code, LVAEdge modül uç noktalarını kolayca izlemenize olanak tanıyan "Azure IoT araçları" uzantısını destekler. Bu aracı, "olaylar" IoT Hub yerleşik uç noktasını izlemeye hızla başlamak ve uç cihazdan buluta yönlendirilen çıkarım iletilerini görmek için kullanabilirsiniz. 

    Ayrıca, bu uzantıyı kullanarak, LVAEdge modülünün modül Ikizi 'yi düzenleyerek medya grafik ayarlarını değiştirebilirsiniz.

Daha fazla bilgi için bkz. [izleme ve günlüğe kaydetme](monitoring-logging.md) makalesi.

## <a name="billing-and-availability"></a>Faturalandırma ve kullanılabilirlik

* IoT Edge LiveVideo analizi nasıl faturalandırılır?
    * Ayrıntılar için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) .

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı başlangıç: IoT Edge kullanmaya başlama-canlı video analizi](get-started-detect-motion-emit-events-quickstart.md)
