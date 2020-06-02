---
title: Medya grafiği kavramı-Azure
description: Medya grafiği, medyanın nerede yakalanabileceğini, nasıl işleneceğini ve sonuçların nereye teslim edileceğini tanımlamanızı sağlar. Bu makale, medya grafiği kavramının ayrıntılı bir açıklamasını vermektedir.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: ee055c34fd37d2d1cc5e7d0bb5147c945dcbff94
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84260970"
---
# <a name="media-graph-concept"></a>Medya grafiği kavramı

## <a name="suggested-pre-reading"></a>Önerilen önceden okuma

* [IoT Edge genel bakış üzerinde canlı video analizi](overview.md)
* [IoT Edge terminolojisinde canlı video analizi](terminology.md)

## <a name="overview"></a>Genel Bakış

Medya grafiği, medyanın nerede yakalanabileceğini, nasıl işleneceğini ve sonuçların nereye teslim edileceğini tanımlamanızı sağlar. Medya grafiği, kaynak, işlemci ve havuz düğümlerinden oluşur. Aşağıdaki diyagramda bir medya grafiğinin grafik temsili verilmiştir.   

![Medya grafiğinin grafik gösterimi](./media/media-graph/overview.png)


* **Kaynak düğümleri** medya grafiğine medya yakalamaya olanak sağlar. Bu bağlamdaki medya, kavramsal olarak bir ses akışı, video akışı, veri akışı veya ses, video ve/veya veri içeren bir akış, tek bir akışta birlikte birleştirilir.
* **İşlemci düğümleri** medya grafiğinde Medya işlemeyi etkinleştirir.
* **Havuz düğümleri** , işleme sonuçlarının medya grafiğinin dışındaki hizmetlere ve uygulamalara teslim edilmesini sağlar.

## <a name="media-graph-topologies-and-instances"></a>Medya grafiği topolojileri ve örnekleri 

IoT Edge üzerindeki canlı video analizi, medya grafiklerini "Graf topolojisi" ve "Graf örneği" olmak üzere iki kavram aracılığıyla yönetmenizi sağlar. Graph topolojisi, parametreler içeren bir grafik şeması tanımlamanızı sağlar ve parametreleri değerler için yer tutucular olarak tanımlar. Topoloji, medya grafiğinde kullanılan düğümleri ve bunların medya grafiğinde nasıl bağlandığını tanımlar. Topolojiye başvuran grafik örnekleri oluşturulurken parametrelerin değerleri belirtilebilir. Bu, aynı topolojiye başvuruda bulunan ancak topolojide belirtilen parametrelere ait farklı değerlerle birden çok örnek oluşturmanıza olanak sağlar. 

## <a name="media-graph-states"></a>Medya grafiği durumları  

Medya grafiği aşağıdaki durumlardan birinde olabilir:

* Etkin değil – bir medya grafiğinin yapılandırıldığı ancak etkin olmadığı durumu temsil eder.
* Etkinleştiriliyor: bir medya grafiğinin örneklendiği (yani, etkin olmayan ve etkin arasındaki geçiş durumu) durum.
* Etkin – bir medya grafiğinin etkin olduğu durumdur. 

    > [!NOTE]
    >  Medya grafiği, veri akışı olmadan etkin hale gelir (örneğin, giriş video kaynağı çevrimdışı olur).
* Devre dışı bırak – bu durum, bir medya grafiğinin etkin durumundan devre dışı durumuna geçme durumdur.

Aşağıdaki diyagramda medya grafiği durum makinesi gösterilmektedir.

![Medya grafiği durum makinesi](./media/media-graph/media-graph-state-machine.png)

## <a name="sources-processors-and-sinks"></a>Kaynaklar, işlemciler ve havuzlar  

IoT Edge canlı video analizi, bir medya grafiğinde aşağıdaki düğüm türlerini destekler:

### <a name="sources"></a>Kaynaklar 

#### <a name="rtsp-source"></a>RTSP kaynağı 

Bir RTSP kaynağı, bir [RTSP](https://tools.ietf.org/html/rfc2326) sunucusundan medya yakalamaya izin vermez. RTSP, bir sunucu ile istemci arasındaki medya oturumlarını oluşturmak ve denetlemek için kullanılır. Medya grafiğindeki RTSP kaynak düğümü istemci olarak davranır ve belirtilen RTSP sunucusuyla bir oturum oluşturabilir. Çoğu [IP kamerada](https://en.wikipedia.org/wiki/IP_camera) gibi birçok cihazın YERLEŞIK bir RTSP sunucusu vardır. [ONVIF](https://www.onvif.org/) MANTARIHLERIN RTSP [, S & T](https://www.onvif.org/wp-content/uploads/2019/12/ONVIF_Profile_Feature_overview_v2-3.pdf) uyumlu cihazların tanımına göre desteklenir. Medya grafiğinde RTSP kaynak düğümü, kimliği doğrulanmış bir bağlantıyı etkinleştirmek için kimlik bilgileriyle birlikte bir RTSP URL 'SI belirtmenizi gerektirir.

#### <a name="iot-hub-message-source"></a>IoT Hub ileti kaynağı 

Diğer [IoT Edge modüller](../../iot-edge/iot-edge-glossary.md#iot-edge-module)gibi, IoT Edge modüldeki canlı video analizi, [IoT Edge hub](../../iot-edge/iot-edge-glossary.md#iot-edge-hub)aracılığıyla iletiler alabilir. Bu iletiler diğer modüllerden veya sınır cihazında ya da buluttan çalışan uygulamalardan gönderilebilir. Bu tür iletiler, modüldeki [adlandırılmış bir girişe](../../iot-edge/module-composition.md#sink) (yönlendirilebilir) teslim edilebilir. IoT Hub bir Ileti kaynağı, bu tür iletilerin bir medya grafiği örneğine yönlendirilmesini sağlar. Bu iletiler veya sinyaller, genellikle sinyal kapılarını etkinleştirmek için medya grafiğinde dahili olarak kullanılabilir (aşağıdaki [sinyal kapısı işlemcisi](#signal-gate-processor) 'ne bakın). 

Örneğin, bir kapı açıldığında ileti üreten bir IoT Edge modüle sahip olabilirsiniz. Bu modülden gelen ileti, daha sonra bir medya grafiğinin IoT Hub 'ı ileti kaynağına yönlendirilebileceği IoT Edge hub 'a yönlendirilebilir. Medya grafiğinde, IoT Hub ileti kaynağı olayı bir sinyal kapısı işlemcisine geçirebilir, bu da daha sonra videonun bir RTSP kaynağından dosyaya kaydedilmesini etkinleştirebilir. 

### <a name="processors"></a>İşlemciler  

#### <a name="motion-detection-processor"></a>Hareket algılama işlemcisi 

Hareket algılama işlemcisi, canlı video 'daki hareketi algılamanıza olanak sağlar. Gelen videoyu inceler ve videoda hareket olup olmadığını belirler. Hareket algılanırsa videoyu aşağı akış düğümüne geçirir ve bir olay yayar. Hareket algılama işlemcisi (diğer medya grafiği düğümleriyle birlikte), algılanan bir işlem olduğunda gelen videonun kaydını tetikleyebilmek için kullanılabilir.

#### <a name="frame-rate-filter-processor"></a>Çerçeve hızı filtre işlemcisi  

Çerçeve hızı filtre işlemcisi, gelen video akışından belirtilen kare ücretine ait çerçeveler örneklemenizi sağlar. Bu, daha fazla işleme için aşağı akış düğümlerine (HTTP uzantısı işlemcisi gibi) gönderilen çerçevelerin sayısını azaltmanıza olanak sağlar.

#### <a name="http-extension-processor"></a>HTTP uzantısı işlemcisi 

HTTP uzantısı işlemcisi, kendi AI 'nizi bir medya grafiğine eklemenize olanak sağlar. HTTP uzantısı işlemcisi, giriş kodu çözülmüş video çerçeveleri olarak alır ve bu çerçeveleri bir HTTP uç noktasına geçirir. İşlemcinin, gerekirse HTTP uç noktası ile kimlik doğrulaması yapma yeteneği vardır. Ayrıca, işlemcinin, video çerçevelerinin öne geçmeden önce ölçeklendirilmesi ve kodlanmasını sağlayan yerleşik görüntü biçimlendiricisi vardır. Ölçeklendirme, resim en boy oranının korunmasıyla ilgili seçeneklere sahiptir, doldurulmuş veya uzatılır, kodlama JPEG, PNG veya BMP gibi farklı görüntü kodlamaları için seçenekler sağlar.

#### <a name="signal-gate-processor"></a>Sinyal kapısı işlemcisi  

Sinyal kapısı işlemcisi, medyanın bir düğümden diğerine koşullu olarak iletilmesini sağlar. Ayrıca, medya ve olay eşitlemesine izin veren bir arabellek görevi görür. Örnek kullanım örneği, bir RTSP kaynağı ve varlık havuzu arasına bir sinyal geçidi işlemcisi eklemek ve ağ geçidini tetiklemek için hareket algılayıcısı işlemcisinin çıkışını kullanmaktır. Böyle bir medya grafiğinde, yalnızca gelen videoda hareket algılandığında medya kaydını tetikleyebilirsiniz. 

### <a name="sinks"></a>Yapma  

#### <a name="asset-sink"></a>Varlık havuzu  

Varlık havuzu, bir ortam grafiğinin bir Azure Media Services varlığına medya (video ve/veya ses) verileri yazmasını sağlar. Varlıklar hakkında daha fazla bilgi için [varlık](terminology.md#asset) bölümüne ve medya kaydetme ve kayıttan yürütme içindeki rolüyle ilgili daha fazla bilgi için bkz..  

#### <a name="file-sink"></a>Dosya havuzu  

Dosya havuzu, medya grafiğinin IoT Edge cihazının yerel dosya sistemindeki bir konuma medya (video ve/veya ses) verileri yazmasını sağlar. Dosya havuzunun bir sinyal kapısı işlemcisinin bir aşağı akış olması gerekir. Bu, çıkış dosyalarının süresini sinyal kapısı işlemci özelliklerinde belirtilen değerlerle sınırlandırır.

#### <a name="iot-hub-message-sink"></a>IoT Hub ileti havuzu  

IoT Hub bir ileti havuzu, olayları IoT Edge hub 'a yayımlamanıza olanak sağlar. Edge hub 'ı daha sonra bu verileri uç cihazdaki diğer modüllere veya uygulamalara yönlendirebilir veya bulutta IoT Hub (dağıtım bildiriminde belirtilen yollar başına). IoT Hub ileti havuzu, bir hareket algılama işlemcisi veya HTTP uzantısı işlemcisi aracılığıyla bir dış çıkarım hizmetinden olayları kabul edebilir.

## <a name="scenarios"></a>Senaryolar

Yukarıda tanımlanan kaynak, işlemci ve havuz düğümlerinin bir birleşimini kullanarak çeşitli senaryolar için medya grafikleri oluşturabilirsiniz. Birkaç örnek senaryo aşağıdaki gibidir

* [Sürekli video kaydı](continuous-video-recording-concept.md)
* [Olay tabanlı video kaydı](event-based-video-recording-concept.md)
* [Video kaydı olmadan canlı video analizi](analyze-live-video-concept.md)

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı başlangıç: canlı video analizinizi kendi modelinizle çalıştırın](use-your-model-quickstart.md) ve canlı video akışında hareket algılamayı nasıl çalıştıracağınızı öğrenin.
