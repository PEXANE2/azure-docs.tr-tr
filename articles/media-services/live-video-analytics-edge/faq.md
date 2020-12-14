---
title: IoT Edge SSS üzerinde canlı video analizi-Azure
description: Bu konu, IoT Edge SSS üzerinde canlı video analizlerine yanıt verir.
ms.topic: conceptual
ms.date: 12/01/2020
ms.openlocfilehash: 521cd0e4f5fc8232a000e10520298a979ba1c14c
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97401585"
---
# <a name="frequently-asked-questions-faqs"></a>Sık sorulan sorular (SSS)

Bu konu, IoT Edge SSS üzerinde canlı video analizlerine yanıt verir.

## <a name="general"></a>Genel

### <a name="what-are-the-system-variables-that-can-be-used-in-graph-topology-definition"></a>Grafik topolojisi tanımında kullanılabilecek sistem değişkenleri nelerdir?

|Değişken   |Açıklama|
|---|---|
|[System. DateTime](/dotnet/framework/data/adonet/sql/linq/system-datetime-methods)|Genellikle günün tarih ve saati olarak ifade edilen UTC zamanında bir anlık temsil eder (temel temsili yyyyMMddTHHmmssZ).|
|System. ııisedatetime|ISO8601 dosya uyumlu biçimindeki UTC Tarih saat örneğini milisaniyelik (temel Gösterim yyyyMMddTHHmmss. fffZ) ile temsil eder.|
|System. Graphtopologyıname   |Bir medya grafiği topolojisini temsil eder, grafiğin şeması saklar.|
|System. Graphınstancename|  Bir medya grafiği örneğini temsil eder, parametre değerlerini tutar ve topolojiye başvurur.|

## <a name="configuration-and-deployment"></a>Yapılandırma ve dağıtım

### <a name="can-i-deploy-the-media-edge-module-to-a-windows-10-device"></a>Medya Edge modülünü bir Windows 10 cihazına dağıtabilir miyim?

Evet. [Windows 10 ' da Linux kapsayıcılarındaki](/virtualization/windowscontainers/deploy-containers/linux-containers)makaleye bakın.

## <a name="capture-from-ip-camera-and-rtsp-settings"></a>IP kamerası ve RTSP ayarlarından yakala

### <a name="do-i-need-to-use-a-special-sdk-on-my-device-to-send-in-a-video-stream"></a>Video akışı göndermek için cihazımda belirli bir SDK kullanmam gerekiyor mu?

Hayır. IoT Edge üzerindeki canlı video analizi, RTSP video akış protokolü (çoğu IP kameralarından desteklenir) kullanılarak medya yakalamayı destekler.

### <a name="can-i-push-media-to-live-video-analytics-on-iot-edge-using-rtmp-or-smooth-like-a-media-services-live-event"></a>IoT Edge RTMP veya kesintisiz (Media Services canlı bir olay gibi) kullanarak canlı video analizlerine medya gönderebilirim miyim?

* Hayır. Canlı video analizi yalnızca IP kameralarından video yakalamak için RTSP 'yi destekler.
* TCP/HTTP üzerinden RTSP akışını destekleyen tüm kamera çalışmalıdır. 

### <a name="can-i-reset-or-update-the-rtsp-source-url-on-a-graph-instance"></a>Grafik örneğinde RTSP kaynak URL’sini sıfırlayabilir veya güncelleştirebilir miyim?

Evet, grafik örneği etkin değil durumunda.  

### <a name="is-there-a-rtsp-simulator-available-to-use-during-testing-and-development"></a>Test ve geliştirme sırasında kullanılmak üzere kullanılabilir bir RTSP simülatörü var mı?

Evet. Hızlı başlar ve öğreticilerde öğrenme sürecini desteklemek için kullanılabilecek bir [RTSP simülatör](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) Edge modülü vardır. Bu modül mümkün olan en iyi çabayla sağlanmıştır ve her zaman kullanılamayabilir. Bunu birkaç saatten uzun bir süre kullanmak kesinlikle önerilir. Üretim dağıtımı için plan yapmadan önce gerçek RTSP kaynağınıza yönelik teste yatırım yapmanız gerekir.

### <a name="do-you-support-onvif-discovery-of-ip-cameras-at-the-edge"></a>Uçta IP kameraları için ONVIF bulmayı destekliyor musunuz?

Hayır, uçtaki cihazlar için ONVIF bulma desteklenmez.

## <a name="streaming-and-playback"></a>Akış ve kayıttan yürütme

### <a name="can-assets-recorded-to-ams-from-the-edge-be-played-back-using-media-services-streaming-technologies-like-hls-or-dash"></a>Ucundan AMS 'ye kaydedilen varlıklar, HLS veya DASH gibi Media Services akış teknolojileri kullanılarak kayıttan çalınabilir mi?

Evet. Kayıtlı varlıklar, Azure Media Services başka herhangi bir varlık gibi akışla eklenebilir. İçeriği akışa almak için, oluşturulmuş bir akış uç noktası ve çalışır durumda olmalıdır. Standart akış Konumlandırıcı oluşturma işleminin kullanılması, herhangi bir uyumlu oynatıcı çerçevesine akış için bir HLS veya DASH bildirimine erişmenizi sağlayacaktır. Yayımlama HLS veya DASH bildirimleri oluşturma hakkında ayrıntılı bilgi için bkz. [dinamik paketleme](../latest/dynamic-packaging-overview.md).

### <a name="can-i-use-the-standard-content-protection-and-drm-features-of-media-services-on-an-archived-asset"></a>Arşivlenmiş bir varlık üzerinde Media Services standart içerik koruması ve DRM özelliklerini kullanabilir miyim?

Evet. Standart dinamik şifreleme içerik koruması ve DRM özelliklerinin hepsi, bir medya grafiğinden kaydedilen varlıklarda kullanılabilir.

### <a name="what-players-can-i-use-to-view-content-from-the-recorded-assets"></a>Kayıtlı varlıkların içeriğini görüntülemek için hangi oyuncuları kullanabilirim?

Uyumlu Apple HTTP Canlı Akışı (HLS) sürüm 3 veya sürüm 4 ' ü destekleyen tüm standart oyuncular desteklenir. Ayrıca, MPEG-DASH kayıttan yürütme yeteneğine sahip olan her oyuncu da desteklenir.

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

### <a name="what-are-the-limits-on-streaming-a-media-graph-asset"></a>Medya grafiği varlığı akışı için sınırlamalar nelerdir?

Medya grafiğinden canlı veya kayıtlı bir varlık akışı, medya & eğlence, OTT ve yayın müşterileri için isteğe bağlı ve canlı akış için Media Services desteklediği yüksek ölçekli altyapıyı ve akış uç noktasını kullanır. Bu, Azure CDN, Verizon veya Akamai ' yi hızla ve kolayca etkinleştirebilmeniz için, içeriğinizi birkaç görüntüleyiciye kadar küçük veya en fazla milyonlarca veya senaryonuza bağlı olarak daha az milyonlarca hale getirebileceğinizi gösterir.

İçerik hem Apple HTTP Canlı Akışı (HLS) hem de MPEG-DASH kullanılarak teslim edilebilir.

## <a name="design-your-ai-model"></a>AI modelinizi tasarlama 

### <a name="i-have-multiple-ai-models-wrapped-in-a-docker-container-how-should-i-use-them-with-live-video-analytics"></a>Bir Docker kapsayıcısında Sarmalanan birden çok AI modeli var. Bunları canlı video analiziyle nasıl kullanmalıyım? 

Çözümler, etkin video analiziyle iletişim kurmak için ınation sunucu tarafından kullanılan iletişim protokolüne bağlı olarak farklılık fark edilir. Bunu yapmanın bazı yolları aşağıda verilmiştir.

#### <a name="http-protocol"></a>HTTP protokolü:

* Tek kapsayıcı (tek lvaExtension):  

   İkinci olarak, farklı AI modelleri için tek bir bağlantı noktası, ancak farklı uç noktalar kullanabilirsiniz. Örneğin, bir Python örneği için `route` model başına farklı s 'leri şu şekilde kullanabilirsiniz: 

   ```
   @app.route('/score/face_detection', methods=['POST']) 
   … 
   Your code specific to face detection model… 

   @app.route('/score/vehicle_detection', methods=['POST']) 
   … 
   Your code specific to vehicle detection model 
   … 
   ```

   Ardından, canlı video analizi dağıtımınızda grafikler örneğini oluştururken her bir örnek için çıkarım sunucu URL 'sini şu şekilde ayarlayın: 

   1. örnek: çıkarım sunucu URL 'SI =`http://lvaExtension:44000/score/face_detection`<br/>
   2. örnek: çıkarım sunucu URL 'SI =`http://lvaExtension:44000/score/vehicle_detection`  
    > [!NOTE]
    > Alternatif olarak Ayrıca, farklı bağlantı noktalarında AI modellerinizi kullanıma sunabilir ve grafikler oluşturduğunuzda bunları çağırabilirsiniz.  

* Birden çok kapsayıcı: 

   Her kapsayıcı farklı bir adla dağıtılır. Şu anda, canlı video analizi belge kümesinde, adı: **Lvaextension** olan bir uzantıyı nasıl dağıtacağınızı gösterdik. Artık iki farklı kapsayıcı geliştirebilirsiniz. Her kapsayıcı aynı HTTP arabirimine (yani aynı `/score` uç nokta) sahiptir. Bu iki kapsayıcıyı farklı adlarla dağıtın ve her ikisinin de **farklı bağlantı noktalarında** dinleme olduğundan emin olun. 

   Örneğin, adı olan bir kapsayıcı `lvaExtension1` bağlantı noktasını dinliyor `44000` , adına sahip diğer kapsayıcı `lvaExtension2` bağlantı noktasını dinliyor `44001` . 

   Canlı video analizi topolojinizde, şu şekilde farklı çıkarım URL 'Leri olan iki grafik örnekleyebilirsiniz: 

   İlk örnek: çıkarım sunucu URL 'SI = `http://lvaExtension1:44001/score`    
   İkinci örnek: çıkarım sunucu URL 'SI = `http://lvaExtension2:44001/score`
   
#### <a name="grpc-protocol"></a>GRPC protokolü: 

Canlı video analizi modülü 1,0 ile, gRPC protokolü kullanılırken tek yönlü gRPC sunucusunun farklı bağlantı noktaları aracılığıyla farklı AI modelleri açığa çıkarmasıdır. [Bu örnekte](https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/grpcExtension/topology.json), tüm Yolo modellerini açığa çıkaran 44000 tek bir bağlantı noktası vardır. Teorik olarak, bazı modelleri 44000, diğerleri de 45000 ' de kullanıma sunmak için Yolo gRPC sunucusu yeniden yazılabilir. 

Canlı video analizi modülü 2,0 ile gRPC uzantısı düğümüne yeni bir özellik eklenmiştir. Bu özellik, gRPC sözleşmesinin bir parçası olarak kullanılabilecek isteğe bağlı bir dize olan **Extensionconfiguration** olarak adlandırılır. Tek bir çıkarım sunucusunda paketlenmiş birden çok AI modeli olduğunda, her AI modeli için bir düğüm kullanıma almanız gerekmez. Bunun yerine, bir grafik örneği için uzantı sağlayıcısı (sizin), **Extensionconfiguration** özelliğini kullanarak farklı AI modellerinin nasıl Seçileni tanımlayabilir ve yürütme sırasında, canlı video analizi bu DIZEYI istenen AI modelini çağırmak için bunu kullanan ınsıya geçişli sunucuya iletir. 

### <a name="i-am-building-a-grpc-server-around-an-ai-model-and-want-to-be-able-to-support-being-used-by-multiple-camerasgraph-instances-how-should-i-build-my-server"></a>Bir AI modeli etrafında gRPC sunucusu oluşturuyor ve birden çok kamera/grafik örneği tarafından kullanılmasını destekliyoruz. My Server 'ı nasıl dermalıyım? 

 İlk olarak, sunucunuzun tek seferde birden fazla isteği işleyebileceğinden emin olun. Ya da sunucunuzun paralel iş parçacıklarında çalıştığından emin olun. 

Örneğin, [canlı video analizi GRPC örneklerinden](https://github.com/Azure/live-video-analytics/blob/master/utilities/video-analysis/notebooks/Yolo/yolov3/yolov3-grpc-icpu-onnx/lvaextension/server/server.py)birinde, varsayılan sayıda paralel Kanal kümesi vardır. Bkz. 

```
server = grpc.server(futures.ThreadPoolExecutor(max_workers=3)) 
```

Yukarıdaki gRPC sunucusu örneklemesinde, sunucu her seferinde her kamera için yalnızca üç kanal (grafik topoloji örneği başına) açabilir. Sunucuya üçten fazla örnek bağlamayı denememelisiniz. Üçten fazla kanal açmaya çalışırsanız, istekler mevcut bir düşene kadar bekleyen olacaktır.  

Yukarıdaki gRPC sunucu uygulamasının üzerinde Python örneklerimizde kullanılır. Geliştiriciler kendi sunucularını uygulayabilir veya yukarıdaki varsayılan uygulamada çalışan numarasını video akışını almak için kullanılan kamera sayısına göre artırabilir. 

Birden çok kamera ayarlamak ve kullanmak için, geliştiriciler her bir örneğin aynı veya farklı çıkarım sunucusunu (örneğin, yukarıdaki paragrafta bahsedilen sunucu) işaret eden birden çok Graf topolojisi örneği oluşturabilir. 

### <a name="i-want-to-be-able-to-receive-multiple-frames-from-upstream-before-i-make-an-inferencing-decision-how-can-i-enable-that"></a>Ters sınırlama kararı vermeden önce yukarı akış içinden birden çok kare alabilmesini istiyorum. Bunu nasıl etkinleştirebilirim? 

Geçerli [varsayılan örnekler](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis) "durum bilgisiz" modunda çalışır. Bu örnek, önceki çağrıların durumunu tutulmaz ve (birden çok topoloji örneği aynı çıkarım sunucusunu çağırabilir ve sunucu, çağıran başına kimin çağırılacağını ve durumunu ayırt edemeyecektir) 

#### <a name="http-protocol"></a>HTTP protokolü

HTTP protokolünü kullanırken: 

Durumu tutmak için, her arayan (Graf topoloji örneği), arayan sunucuya, çağıran HTTP sorgu parametresi ile birlikte çağırıcı sunucusunu çağırır. Örneğin, için çıkarım sunucu URL adresi  

Birinci topoloji örneği = `http://lvaExtension:44000/score?id=1`<br/>
2. topoloji örneği = `http://lvaExtension:44000/score?id=2`

… 

Sunucu tarafında, puan rotası kimin çağıran olduğunu bilir. KIMLIK = 1 ise, bu, çağıran (Graf topoloji örneği) için durumu ayrı tutabilir. Daha sonra alınan video çerçevelerini bir arabellekte tutabilirsiniz (örneğin, dizi veya tarih saat anahtarı içeren bir sözlük ve değer çerçevedir) ve ardından x kareleri alındıktan sonra işlemek için sunucuyu (çıkarım) tanımlayabilirsiniz. 

#### <a name="grpc-protocol"></a>GRPC Protokolü 

GRPC protokolü kullanılırken: 

Bir gRPC uzantısıyla her oturum tek bir kamera akışına yöneliktir, bu nedenle KIMLIK sağlanması gerekmez. Artık extensionConfiguration özelliği sayesinde video çerçevelerini bir arabellekte saklayabilir ve x çerçeveleri alındıktan sonra işlenecek sunucuyu (çıkarım) tanımlayabilirsiniz. 

### <a name="do-all-processmediastreams-on-a-particular-container-run-the-same-ai-model"></a>Belirli bir kapsayıcıdaki tüm ProcessMediaStreams aynı AI modelini mi çalıştırsın? 

Hayır.  

Grafik örneğindeki son kullanıcıdan gelen çağrıları Başlat/Durdur işlemi bir oturum oluşturur veya belki de bir kamera bağlantı kesilmesi/yeniden bağlantı olabilir. Hedef, kameranın video akışı olması halinde tek bir oturumu kalıcı hale maktır. 

* İşlenmek üzere video Gönderen iki kamera iki oturum oluşturur. 
* İki gRPCExtension düğümü olan bir grafiğe giden bir kamera iki oturum oluşturur. 

Her oturum, canlı video analizi ile gRPC sunucusu arasında tam çift yönlü bir bağlantıdır ve her oturum farklı bir model/işlem hattına sahip olabilir. 

> [!NOTE]
> Kamera bağlantısının kesilmesi/yeniden bağlanmasına sonra (tolerans sınırları ötesinde bir süre boyunca kamera çevrimdışı duruma geçiyor), canlı video analizi gRPC sunucusuyla yeni bir oturum açar. Sunucunun bu oturumlardaki durumu izlemesi için bir gereksinim yoktur. 

Canlı video analizi, bir grafik örneğindeki tek bir kamera için birden çok gRPC uzantısı desteği de eklenmiştir. Bu gRPC uzantılarını, Birleşik olarak veya paralel olarak ya da her ikisinin birleşimine sahip olan AI işlemeyi yürütmek için kullanabilirsiniz. 

> [!NOTE]
> Birden çok uzantının paralel olarak çalıştırılması, donanım kaynaklarınızı etkiler ve hesaplama gereksinimlerinize uyacak donanımı seçerken bunu aklınızda bulundurmanız gerekir. 

### <a name="what-is-the-max--of-simultaneous-processmediastreams"></a>Eşzamanlı ProcessMediaStreams sayısı üst sınırı nedir? 

Canlı video analizinin uygulandığı bir sınır yoktur.  

### <a name="how-should-i-decide-if-my-inferencing-server-should-use-cpu-or-gpu-or-any-other-hardware-accelerator"></a>Inırma sunucusu 'nun CPU veya GPU ya da başka bir donanım hızlandırıcıyı kullanması gerekip gerekmediğini nasıl karar vermem gerekir? 

Bu, AI modelinin ne kadar karmaşık geliştirildiği ve geliştiricinin CPU ve donanım hızlandırıcılarını nasıl kullanmak istediğini tamamen bağımlıdır. AI modeli geliştirilirken, geliştiriciler hangi kaynakların hangi işlemleri gerçekleştirmek için kullanılması gerektiğini belirtebilir. 

### <a name="how-do-i-store-images-with-bounding-boxes-post-processing"></a>Nasıl yaparım?, sınırlama kutuları olan görüntüleri işleme göndersin mi? 

Bugün, yalnızca çıkarım iletileri olarak sınırlama kutusu koordinatları sunuyoruz. Geliştiriciler, bu iletileri kullanan ve video çerçeveleri üzerinde sınırlayıcı kutuları kaplayan özel bir MJPEG flaması oluşturabilir.  

## <a name="grpc-compatibility"></a>gRPC uyumluluğu 

### <a name="how-will-i-know-what-the-mandatory-fields-for-the-media-stream-descriptor-are"></a>Medya akışı tanımlayıcısı için zorunlu alanların ne olduğunu nasıl anlarım? 

Sağlanan hiçbir alan değerine [gRPC tarafından belirtilen](https://developers.google.com/protocol-buffers/docs/proto3#default)varsayılan değer verilir.  

Canlı video analizi, protokol arabelleği dilinin **proto3** sürümünü kullanır. Canlı video analizi sözleşmeleri tarafından kullanılan tüm protokol arabelleği verileri [burada tanımlanan](https://github.com/Azure/live-video-analytics/tree/master/contracts/grpc)protokol arabelleği dosyalarında mevcuttur. 

### <a name="how-should-i-ensure-that-i-am-using-the-latest-protocol-buffer-files"></a>En son protokol arabelleği dosyalarını kullandığımı nasıl güvence altına almam gerekir? 

En son protokol arabelleği dosyaları [buradan elde](https://github.com/Azure/live-video-analytics/tree/master/contracts/grpc)edilebilir. Sözleşme dosyalarını her güncelleştirtiğimiz zaman, bu konumda görünürler. Protokol dosyalarını güncelleştirmeye yönelik bir plan olmadığından, sürümü bildirmek için dosyaların en üstündeki paket adını arayın. Şunu okumalı: 

```
microsoft.azure.media.live_video_analytics.extensibility.grpc.v1 
```

Bu dosyalardaki tüm güncelleştirmeler, adın sonundaki "v-value" değerini artırır. 

> [!NOTE]
> Canlı video analizi dilin proto3 sürümünü kullandığından, alanlar isteğe bağlıdır ve bu, geri ve ileri doğru şekilde uyumlu hale gelir. 

### <a name="what-grpc-features-are-available-for-me-to-use-with-live-video-analytics-which-features-are-mandatory-and-which-ones-are-optional"></a>Live video Analytics ile kullanmak için hangi gRPC özellikleri kullanılabilir? Hangi özellikler zorunlu ve hangilerinin isteğe bağlıdır? 

Tüm sunucu tarafı gRPC özellikleri kullanılabilir, prototipleme sözleşmesi yerine getirilir. 

## <a name="monitoring-and-metrics"></a>İzleme ve ölçümler

### <a name="can-i-monitor-the-media-graph-on-the-edge-using-event-grid"></a>Event Grid kullanarak kenar üzerinde medya grafiğini izleyebilir miyim?

Evet. Prometheus ölçümlerini kullanabilir ve bunları Event Grid 'de yayımlayabilirsiniz. 

### <a name="can-i-use-azure-monitor-to-view-the-health-metrics-and-performance-of-my-media-graphs-in-the-cloud-or-on-the-edge"></a>Bulutta veya kenarda medya grafiklerimin sistem durumunu, ölçümlerini ve performansını görüntülemek için Azure Izleyicisini kullanabilir miyim?

Evet. Bu desteklenen bir geçiştir. [Azure Izleyici ölçümlerini kullanma](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics)hakkında daha fazla bilgi edinin.

### <a name="are-there-any-tools-to-make-it-easier-to-monitor-the-media-services-iot-edge-module"></a>Media Services IoT Edge modülünü izlemeyi kolaylaştırmak için herhangi bir araç var mı?

Visual Studio Code, LVAEdge modül uç noktalarını kolayca izlemenize olanak tanıyan "Azure IoT araçları" uzantısını destekler. Bu aracı, "olaylar" IoT Hub yerleşik uç noktasını izlemeye hızla başlamak ve uç cihazdan buluta yönlendirilen çıkarım iletilerini görmek için kullanabilirsiniz. 

Ayrıca, bu uzantıyı kullanarak, LVAEdge modülünün modül Ikizi 'yi düzenleyerek medya grafik ayarlarını değiştirebilirsiniz.

Daha fazla bilgi için bkz. [izleme ve günlüğe kaydetme](monitoring-logging.md) makalesi.

## <a name="billing-and-availability"></a>Faturalandırma ve kullanılabilirlik

### <a name="how-is-live-video-analytics-on-iot-edge-billed"></a>IoT Edge için canlı video analizi nasıl faturalandırılır?

Ayrıntılar için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) .

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı başlangıç: IoT Edge kullanmaya başlama-canlı video analizi](get-started-detect-motion-emit-events-quickstart.md)
