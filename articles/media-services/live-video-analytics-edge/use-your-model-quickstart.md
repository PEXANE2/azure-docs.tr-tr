---
title: Kendi modelinizi kullanarak canlı videoyu çözümleyin-Azure
description: Bu hızlı başlangıçta, (benzetimli) bir IP kamerasından canlı video akışını çözümlemek için bilgisayar vizyonu uygularsınız.
ms.topic: quickstart
ms.date: 04/27/2020
ms.openlocfilehash: dc8c2d1f0620a92a13cb1f4c0b83c2452f964fd6
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87170617"
---
# <a name="quickstart-analyze-live-video-by-using-your-own-model"></a>Hızlı başlangıç: kendi modelinizi kullanarak canlı videoyu çözümleyin

Bu hızlı başlangıçta, (benzetimli) bir IP kamerasından canlı video akışını çözümlemek için IoT Edge 'da canlı video analizinin nasıl kullanılacağı gösterilmektedir. Nesneleri algılamak için bir bilgisayar Vision modelinin nasıl uygulanacağını göreceksiniz. Canlı video akışındaki çerçevelerin bir alt kümesi bir çıkarım hizmetine gönderilir. Sonuçlar IoT Edge hub 'ına gönderilir. 

Bu hızlı başlangıç, bir Azure VM 'yi IoT Edge bir cihaz olarak kullanır ve sanal bir canlı video akışı kullanır. C# dilinde yazılmış örnek koda dayalıdır ve [hareket ve yayma olaylarını Algıla](detect-motion-emit-events-quickstart.md) hızlı başlangıç bölümünde oluşturulur. 

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir abonelik içeren bir Azure hesabı. Henüz bir [hesabınız yoksa ücretsiz olarak bir hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) .
* [Visual Studio Code](https://code.visualstudio.com/), aşağıdaki uzantılara sahip:
    * [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
    * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).
* [Hareket algılama ve olayları](detect-motion-emit-events-quickstart.md) gösterme hızlı başlangıcı ' nı tamamlamadıysanız [Azure kaynaklarını ayarladığınızdan](detect-motion-emit-events-quickstart.md#set-up-azure-resources)emin olun.

> [!TIP]
> Azure IoT araçları 'nı yüklerken Docker 'ı yüklemeniz istenebilir. İstemi yoksayabilirsiniz.

## <a name="review-the-sample-video"></a>Örnek videoyu gözden geçirin
Azure kaynaklarını ayarlarken, Azure 'da IoT Edge cihaz olarak kullandığınız Linux VM 'sine yönelik kısa bir ekran trafiği. Bu hızlı başlangıçta canlı bir akışın benzetimini yapmak için video dosyası kullanılmaktadır.

[VLC medya oynatıcı](https://www.videolan.org/vlc/)gibi bir uygulama açın. CTRL + N ' ı seçin ve ardından kayıttan yürütmeyi başlatmak için [videoya](https://lvamedia.blob.core.windows.net/public/camera-300s.mkv) bir bağlantı yapıştırın. Birçok taşıın otoyol trafiğinden çekilmesine bakabilirsiniz.

Bu hızlı başlangıçta, Araçlar ve kişiler gibi nesneleri algılamak için IoT Edge üzerinde canlı video analizi kullanacaksınız. İlişkili çıkarım olaylarını IoT Edge hub 'a yayımlayacaksınız.

## <a name="overview"></a>Genel Bakış

![Genel Bakış](./media/quickstarts/overview-qs5.png)

Bu diyagramda, sinyallerin bu hızlı başlangıçta nasıl akagösterdiği gösterilmektedir. [Edge modülü](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) , gerçek zamanlı akış protokolü (RTSP) sunucusunu BARıNDıRAN bir IP kamerasına benzetim yapar. Bir [RTSP kaynak](media-graph-concept.md#rtsp-source) düğümü, bu sunucudan video akışını çeker ve [çerçeve hızı filtre işlemcisi](media-graph-concept.md#frame-rate-filter-processor) düğümüne video çerçeveleri gönderir. Bu işlemci, [http uzantısı işlemci](media-graph-concept.md#http-extension-processor) düğümüne ulaşan video akışının kare oranını sınırlandırır. 

HTTP uzantısı düğümü bir ara sunucu rolünü yürütür. Video çerçevelerini belirtilen görüntü türüne dönüştürür. Ardından, görüntüyü REST üzerinden bir HTTP uç noktasının arkasında bulunan bir AI modeli çalıştıran başka bir Edge modülüne geçirir. Bu örnekte, bu Edge modülü, birçok nesne türünü tespit eden [YOLOv3](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx) modeli kullanılarak oluşturulmuştur. HTTP uzantısı işlemci düğümü, algılama sonuçlarını toplar ve olayları [IoT Hub havuz](media-graph-concept.md#iot-hub-message-sink) düğümüne yayımlar. Düğüm daha sonra bu olayları [IoT Edge hub 'ına](../../iot-edge/iot-edge-glossary.md#iot-edge-hub)gönderir.

Bu hızlı başlangıçta şunları yapmanız gerekir:

1. Medya grafiğini oluşturun ve dağıtın.
1. Sonuçları yorumlayın.
1. Kaynakları temizleyin.



## <a name="create-and-deploy-the-media-graph"></a>Medya grafiğini oluşturma ve dağıtma
    
### <a name="examine-and-edit-the-sample-files"></a>Örnek dosyaları İnceleme ve düzenleme

Önkoşulların bir parçası olarak, örnek kodu bir klasöre indirdiniz. Örnek dosyaları incelemek ve düzenlemek için bu adımları izleyin.

1. Visual Studio Code, *src/Edge*bölümüne gidin. *. Env* dosyanızı ve birkaç dağıtım şablonu dosyasını görürsünüz.

    Dağıtım şablonu, Edge cihazının dağıtım bildirimini ifade eder. Bazı yer tutucu değerleri içerir. *. Env* dosyası bu değişkenlerin değerlerini içerir.

1. *Src/buluttan cihaza-Console-App* klasörüne gidin. Burada dosya ve diğer birkaç dosya *appsettings.js* görürsünüz:

    * ***C2D-Console-App. csproj*** -Visual Studio Code için proje dosyası.
    * ***operations.js*** , programın çalıştırmasını istediğiniz işlemlerin bir listesi.
    * ***Program.cs*** -örnek program kodu. Bu kod:

        * Uygulama ayarlarını yükler.
        * IoT Edge modülündeki canlı video analizinin sunduğu doğrudan yöntemleri çağırır. [Doğrudan yöntemlerini](direct-methods.md)çağırarak canlı video akışlarını çözümlemek için modülünü kullanabilirsiniz.
        * Program çıkışını **TERMINAL** penceresinde Incelemenize ve **Çıkış** penceresinde modül tarafından oluşturulan olayları incelemenize olanak tanıyan duraklar.
        * Kaynakları temizlemek için doğrudan yöntemleri çağırır.


1. Dosyadaki *operations.js* düzenleyin:
    * Grafik topolojisine olan bağlantıyı değiştirin:

        `"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/httpExtension/topology.json"`

    * Altında `GraphInstanceSet` , grafik topolojisinin adını önceki bağlantıdaki değerle eşleşecek şekilde düzenleyin:

      `"topologyName" : "InferencingWithHttpExtension"`

    * Altında `GraphTopologyDelete` , adı düzenleyin:

      `"name": "InferencingWithHttpExtension"`

### <a name="generate-and-deploy-the-iot-edge-deployment-manifest"></a>IoT Edge dağıtım bildirimini oluşturma ve dağıtma

1. Dosyasında *src/Edge/deployment.yolov3.template.js* öğesine sağ tıklayın ve ardından **IoT Edge dağıtım bildirimi oluştur**' u seçin.

    ![IoT Edge dağıtım bildirimi oluştur](./media/quickstarts/generate-iot-edge-deployment-manifest-yolov3.png)  

    Bildirim dosyasındaki *deployment.yolov3.amd64.js* *src/Edge/config* klasöründe oluşturulur.

1. [Hareket ve yayma olaylarını Algıla](detect-motion-emit-events-quickstart.md) hızlı başlangıcı ' nı tamamlayıp bu adımı atlayın. 

    Aksi halde, sol alt köşedeki **Azure ıOT hub** bölmesinin yakınında, **diğer eylemler** simgesini ve ardından **IoT Hub bağlantı dizesi ayarla**' yı seçin. Dizeyi dosyadaki *appsettings.js* kopyalayabilirsiniz. Ya da Visual Studio Code içinde doğru IoT Hub 'ı yapılandırdığınızdan emin olmak için, [IoT Hub 'ı Seç komutunu](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Select-IoT-Hub)kullanın.
    
    ![IoT Hub bağlantı dizesi ayarla](./media/quickstarts/set-iotconnection-string.png)

1. *Kaynak/kenar/yapılandırma/deployment.yolov3.amd64.jsüzerinde* sağ tıklayın ve **tek cihaz için dağıtım oluştur**' u seçin. 

    ![Tek cihaz için dağıtım oluştur](./media/quickstarts/create-deployment-single-device.png)

1. IoT Hub bir cihaz seçmeniz istendiğinde, **LVA-örnek-cihaz**' ı seçin.
1. 30 saniye sonra, pencerenin sol alt köşesinde Azure IoT Hub ' yi yenileyin. Edge cihazında artık aşağıdaki dağıtılan modüller gösterilmektedir:

    * **Lvaedge** adlı canlı video analizi modülü
    * Bir RTSP sunucusuna benzetir ve canlı video akışı kaynağı görevi gören **rtspsim** modülü
    * Görüntülere görüntü getiren ve birden çok nesne türü sınıfını döndüren YOLOv3 nesne algılama modeli olan **yolov3** modülü.
 
      ![Sınır cihazında dağıtılan modüller](./media/quickstarts/yolov3.png)

### <a name="prepare-to-monitor-events"></a>Olayları izlemeye hazırlanma

Canlı video analizi cihazına sağ tıklayın ve **Izlemeyi Başlat yerleşik olay uç noktası**' nı seçin. Visual Studio Code **Çıkış** penceresinde IoT Hub olaylarını izlemek için bu adıma ihtiyacınız vardır. 

![İzlemeyi Başlat](./media/quickstarts/start-monitoring-iothub-events.png) 

### <a name="run-the-sample-program"></a>Örnek programı çalıştırma

1. Bir hata ayıklama oturumu başlatmak için F5 tuşunu seçin. **TERMINAL** penceresinde yazdırılan iletileri görürsünüz.
1. Kod *üzerindekioperations.js* doğrudan yöntemlere ve çağrılarıyla başlatılır `GraphTopologyList` `GraphInstanceList` . Önceki hızlı başlangıçlarını tamamladıktan sonra kaynakları temizledikten sonra bu işlem boş listeleri döndürür ve ardından duraklatılır. Devam etmek için Enter tuşunu seçin.

   ```
   --------------------------------------------------------------------------
   Executing operation GraphTopologyList
   -----------------------  Request: GraphTopologyList  --------------------------------------------------
   {
   "@apiVersion": "1.0"
   }
   ---------------  Response: GraphTopologyList - Status: 200  ---------------
   {
   "value": []
   }
   --------------------------------------------------------------------------
   Executing operation WaitForInput
   Press Enter to continue
   ```

    **TERMINAL** penceresinde, bir sonraki doğrudan yöntem çağrısı kümesi gösterilir:

     * Daha önce kullanılan bir çağrısı `GraphTopologySet``topologyUrl`
     * Aşağıdaki gövdesini kullanan öğesine yapılan bir çağrı `GraphInstanceSet` :

         ```
         {
           "@apiVersion": "1.0",
           "name": "Sample-Graph-1",
           "properties": {
             "topologyName": "InferencingWithHttpExtension",
             "description": "Sample graph description",
             "parameters": [
               {
                 "name": "rtspUrl",
                 "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
               },
               {
                 "name": "rtspUserName",
                 "value": "testuser"
               },
               {
                 "name": "rtspPassword",
                 "value": "testpassword"
               }
             ]
           }
         }
         ```

     * `GraphInstanceActivate`Grafik örneğini ve videonun akışını başlatan öğesine yönelik bir çağrı
     * `GraphInstanceList`Grafik örneğinin çalışır durumda olduğunu gösteren ikinci bir çağrı
1. **TERMINAL** penceresindeki çıktı bir istem sırasında duraklatılır `Press Enter to continue` . Henüz ENTER ' ı seçmeyin. Doğrudan çağrdığınız yöntemler için JSON yanıtı yüklerini görmek üzere yukarı kaydırın.
1. Visual Studio Code **Çıkış** penceresine geçin. IoT Edge modülündeki canlı video analizinin IoT Hub 'ına gönderdiğini iletiler görürsünüz. Bu hızlı başlangıçta aşağıdaki bölümde bu iletiler ele alınmaktadır.
1. Medya grafiği çalışmaya devam eder ve sonuçları yazdırır. RTSP simülatörü kaynak videoyu döngüye sokmaya devam eder. Medya grafiğini durdurmak için, **TERMINAL** penceresine dönün ve ENTER ' u seçin. 

    Sonraki çağrı dizisi kaynakları temizler:
      * `GraphInstanceDeactivate`Grafik örneğini devre dışı bırakmak için bir çağrı.
      * `GraphInstanceDelete`Örneği silme çağrısı.
      * İçin bir çağrı `GraphTopologyDelete` , topolojiyi siler.
      * İçin nihai bir çağrı `GraphTopologyList` , listenin boş olduğunu gösterir.

## <a name="interpret-results"></a>Sonuçları yorumlama

Medya grafiğini çalıştırdığınızda, HTTP uzantısı işlemci düğümünün sonuçları IoT Hub 'ına IoT Hub havuz düğümünden geçer. **Çıktı** penceresinde gördüğünüz iletiler bir `body` bölümü ve bir `applicationProperties` bölümü içerir. Daha fazla bilgi için bkz. [IoT Hub Iletileri oluşturma ve okuma](../../iot-hub/iot-hub-devguide-messages-construct.md).

Aşağıdaki iletilerde, canlı video analizi modülü, uygulama özelliklerini ve gövdenin içeriğini tanımlar. 

### <a name="mediasessionestablished-event"></a>Mediasessionkurulduğu olayı

Bir medya grafiği oluşturulduğunda, RTSP kaynak düğümü, rtspsim-live555 kapsayıcısında çalışan RTSP sunucusuna bağlanmaya çalışır. Bağlantı başarılı olursa, aşağıdaki olay yazdırılır. Olay türü `Microsoft.Media.MediaGraph.Diagnostics.MediaSessionEstablished` .

```
[IoTHubMonitor] [9:42:18 AM] Message received from [lvaedgesample/lvaEdge]:
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 nnn.nn.0.6\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets=Z00AKeKQCgC3YC3AQEBpB4kRUA==,aO48gA==\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "dataVersion": "1.0",
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",
    "subject": "/graphInstances/GRAPHINSTANCENAMEHERE/sources/rtspSource",
    "eventType": "Microsoft.Media.MediaGraph.Diagnostics.MediaSessionEstablished",
    "eventTime": "2020-04-09T16:42:18.1280000Z"
  }
}
```

Bu iletide, bu ayrıntılara dikkat edin:

* İleti bir tanılama olayıdır. `MediaSessionEstablished`RTSP kaynak düğümünün (konu) RTSP simülatörü ile bağlandığını ve (benzetimli) canlı akışını almaya başladığını gösterir.
* `applicationProperties`' De, `subject` iletinin medya grafiğindeki RTSP kaynak düğümünden oluşturulduğunu gösterir.
* `applicationProperties`' De, `eventType` Bu olayın bir tanılama olayı olduğunu gösterir.
* , `eventTime` Olayın gerçekleştiği saati gösterir.
* `body`Tanılama olayı hakkındaki verileri içerir. Bu durumda, veriler [oturum açıklaması Protokolü (SDP)](https://en.wikipedia.org/wiki/Session_Description_Protocol) ayrıntılarını içerir.

### <a name="inference-event"></a>Çıkarım olayı

HTTP uzantısı işlemci düğümü, yolov3 modülünden çıkarım sonuçlarını alır. Ardından sonuçları, çıkarım olayları olarak IoT Hub havuz düğümü aracılığıyla yayar. 

Bu olaylarda, türü, `entity` bir otomobil ya da kamyon gibi bir varlık olduğunu belirtmek için olarak ayarlanır. `eventTime`Değer, nesnenin ALGıLANDıĞı UTC zamandır. 

Aşağıdaki örnekte, farklı düzeylerde güvenle aynı video çerçevesinde iki araba algılandı.

```
[IoTHubMonitor] [11:37:17 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "inferences": [
      {
        "entity": {
          "box": {
            "h": 0.0344108157687717,
            "l": 0.5756940841674805,
            "t": 0.5929375966389974,
            "w": 0.04484643936157227
          },
          "tag": {
            "confidence": 0.8714089393615723,
            "value": "car"
          }
        },
        "type": "entity"
      },
      {
        "entity": {
          "box": {
            "h": 0.03960910373263889,
            "l": 0.2750667095184326,
            "t": 0.6102327558729383,
            "w": 0.031027007102966308
          },
          "tag": {
            "confidence": 0.7042660713195801,
            "value": "car"
          }
        },
        "type": "entity"
      }
    ]
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",
    "subject": "/graphInstances/GRAPHINSTANCENAMEHERE/processors/inferenceClient",
    "eventType": "Microsoft.Media.Graph.Analytics.Inference",
    "eventTime": "2020-04-23T06:37:16.097Z"
  }
}
```

İletilerde aşağıdaki ayrıntılara dikkat edin:

* `applicationProperties`' De, `subject` iletinin oluşturulduğu grafik topolojisinde bulunan düğüme başvurur. 
* `applicationProperties`' De, `eventType` Bu olayın bir analiz olayı olduğunu gösterir.
* `eventTime`Değer, olayın gerçekleştiği zaman değeridir.
* `body`Bölüm, analiz olayı hakkındaki verileri içerir. Bu durumda, olay bir çıkarım olayıdır, bu nedenle gövde verileri içerir `inferences` .
* Bölümü, olduğunu `inferences` gösterir `type` `entity` . Bu bölüm, varlıkla ilgili ek verileri içerir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıç yapmayı düşünüyorsanız oluşturduğunuz kaynakları saklayın. Aksi takdirde, Azure portal gidin, kaynak gruplarınıza gidin, bu hızlı başlangıcı çalıştırdığınız kaynak grubunu seçin ve tüm kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

* [YOLOv3 modelinin güvenli bir sürümünü](https://github.com/Azure/live-video-analytics/blob/master/utilities/video-analysis/tls-yolov3-onnx/readme.md) deneyın ve IoT Edge cihazına dağıtın. 

Gelişmiş kullanıcılar için ek güçlükleri gözden geçirin:

* RTSP simülatörü kullanmak yerine RTSP desteği olan bir [IP kamerası](https://en.wikipedia.org/wiki/IP_camera) kullanın. [ONVIF uyumlu](https://www.onvif.org/conformant-products/) ürünler sayfasında RTSP 'YI destekleyen IP kameralarını arayabilirsiniz. Profiller G, S veya T ile uyumlu olan cihazları arayın.
* Azure Linux VM yerine AMD64 veya x64 Linux cihazı kullanın. Bu cihaz, IP kamerası ile aynı ağda olmalıdır. [Linux üzerinde Azure IoT Edge çalışma zamanını Install](../../iot-edge/how-to-install-iot-edge-linux.md)bölümündeki yönergeleri izleyebilirsiniz. Ardından, [ilk IoT Edge modülünüzü bir sanal Linux cihazına dağıtma](../../iot-edge/quickstart-linux.md)konusundaki yönergeleri Izleyerek cihazı Azure IoT Hub kaydettirin.
