---
title: Kendi modelinizi kullanarak canlı videoyu çözümleyin-Azure
description: Bu hızlı başlangıçta, (benzetimli) bir IP kamerasından canlı video akışını çözümlemek için bilgisayar vizyonunu uygulayacaksınız.
ms.topic: quickstart
ms.date: 04/27/2020
ms.openlocfilehash: 0b502fb4bcfa3a11890167c5ef297463a3c51cdc
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84261975"
---
# <a name="quickstart-analyze-live-video-with-your-own-model"></a>Hızlı başlangıç: kendi modelinizle canlı videoyu çözümleme

Bu hızlı başlangıçta, nesneleri algılamak üzere bir bilgisayar Vision modeli uygulayarak (benzetimli) bir IP kamerasından canlı video akışını çözümlemek için IoT Edge 'da canlı video analizinin nasıl kullanılacağı gösterilmektedir. Canlı video akışındaki çerçevelerin bir alt kümesi bir çıkarım hizmetine gönderilir ve sonuçları IoT Edge hub 'ına gönderilir. Bir Azure VM 'yi IoT Edge bir cihaz ve sanal bir canlı video akışı olarak kullanır. Bu makale, C# dilinde yazılan örnek kodu temel alır.

Bu makalede, [Bu](detect-motion-emit-events-quickstart.md) hızlı başlangıç hakkında daha fazla derleme yapılır. 

## <a name="prerequisites"></a>Ön koşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Aşağıdaki uzantılara sahip makinenizde [Visual Studio Code](https://code.visualstudio.com/) :
    * [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
    * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* Sisteminizde yüklü [.NET Core 3,1 SDK 'sı](https://dotnet.microsoft.com/download/dotnet-core/3.1)
* [Bu](detect-motion-emit-events-quickstart.md) hızlı başlangıcı önceden tamamlamadıysanız aşağıdaki adımları uygulayın:
     * [Azure kaynakları ayarlama](detect-motion-emit-events-quickstart.md#set-up-azure-resources)

> [!TIP]
> Azure IoT araçlarını yüklerken Docker 'ı yüklemeniz istenebilir. Yok saymaktan çekinmeyin.

## <a name="review-the-sample-video"></a>Örnek videoyu gözden geçirin
Azure kaynaklarını kurmak için yukarıdaki adımların bir parçası olarak, IoT Edge cihaz olarak kullanılan Azure 'daki Linux VM 'ye bir otoyol trafiğinin (kısa) bir videosunu kopyalacaksınız. Bu video dosyası, bu öğretici için canlı bir akışın benzetimini yapmak üzere kullanılacaktır.

[VLC Player](https://www.videolan.org/vlc/)gibi bir uygulama kullanabilir, bunu başlatabilir, Control + N tuşuna basın ve [Bu](https://lvamedia.blob.core.windows.net/public/camera-300s.mkv) bağlantıyı videoya yapıştırarak kayıttan yürütmeyi başlatabilirsiniz. Çekimin trafik üzerinde olduğunu ve bu sayede çok sayıda taşımayı görürsünüz.

Aşağıdaki adımları tamamladığınızda, Araçlar, kişiler vb. gibi nesneleri tespit etmek ve ilişkili çıkarım olaylarını IoT Edge hub 'ına yayımlamak için IoT Edge üzerinde canlı video analizi kullandınız.

## <a name="overview"></a>Genel Bakış

![Genel Bakış](./media/quickstarts/overview-qs5.png)

Yukarıdaki diyagramda, sinyallerin bu hızlı başlangıçta nasıl akagösterdiği gösterilmektedir. Bir Edge modülü ( [burada](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555)ayrıntılı), bir RTSP sunucusu BARıNDıRAN bir IP kamerasının benzetimini yapar. Bir [RTSP kaynak](media-graph-concept.md#rtsp-source) düğümü, bu sunucudan video akışını çeker ve [çerçeve Fate filtre işlemcisi](media-graph-concept.md#frame-rate-filter-processor) düğümüne video çerçeveleri gönderir. Bu işlemci, video akışının [http uzantısı işlemci](media-graph-concept.md#http-extension-processor) düğümüne ulaşan kare hızını sınırlar. 

HTTP uzantısı düğümü, video çerçevelerini belirtilen görüntü türüne dönüştürerek bir ara sunucu rolünü yürütür ve görüntüyü geri kalanı bir HTTP uç noktasının arkasında bulunan bir AI modeli çalıştıran başka bir kenar modülüne geçirerek bir proxy rolü oynar. Bu örnekte, bu Edge modülü, birçok nesne türünü tespit eden [YOLOv3](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx) modeli kullanılarak oluşturulmuştur. HTTP uzantısı işlemci düğümü, algılama sonuçlarını toplar ve olayları [IoT Hub havuz](media-graph-concept.md#iot-hub-message-sink ) düğümüne yayımlar, bu da bu olayı [IoT Edge hub 'ına](../../iot-edge/iot-edge-glossary.md#iot-edge-hub)gönderir.

Bu hızlı başlangıçta şunları yapmanız gerekir:

1. Medya grafiğini oluşturun ve dağıtın.
1. Sonuçları yorumlayın.
1. Kaynakları temizleyin.



## <a name="create-and-deploy-the-media-graph"></a>Medya grafiğini oluşturma ve dağıtma
    
### <a name="examine-and-edit-the-sample-files"></a>Örnek dosyaları İnceleme ve düzenleme

Önkoşulların bir parçası olarak, örnek kodu bir klasöre indirmiş olursunuz. Visual Studio Code başlatın ve klasörü açın.

1. Visual Studio Code, "src/Edge" e gidin. Oluşturduğunuz. env dosyasını, birkaç dağıtım şablonu dosyası ile birlikte görürsünüz.

    * Dağıtım şablonu, kenar cihazının bazı yer tutucu değerleriyle birlikte dağıtım bildirimini ifade eder. . Env dosyası bu değişkenlerin değerlerini içerir.
1. Sonra, "src/buluttan-cihaza-Console-App" klasörüne gidin. Burada, oluşturduğunuz appSettings. json dosyasını ve diğer birkaç dosyayı görürsünüz:

    * C2D-Console-App. csproj-bu Visual Studio Code proje dosyasıdır.
    * Operations. JSON-bu dosya, programın çalıştırmasını istediğiniz farklı işlemleri listeler.
    * Program.cs-Bu örnek program kodudur ve şunları yapar:

        * Uygulama ayarlarını yükler.
        *  IoT Edge modülünde canlı video analizi tarafından kullanıma sunulan doğrudan yöntemleri çağırır. [Doğrudan yöntemlerini](direct-methods.md) çağırarak canlı video akışlarını çözümlemek için modülünü kullanabilirsiniz 
        * TERMINAL penceresinde programın çıkışını ve çıkış penceresinde modül tarafından oluşturulan olayları incelemek için duraklamalar
        * Kaynakları temizlemek için doğrudan yöntemleri çağırır   


1. Operations. json dosyasında aşağıdaki düzenlemeleri yapın
    * Grafik topolojisine olan bağlantıyı değiştirin:`"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/httpExtension/topology.json"`
    * Graphınstanceset altında, grafik topolojisinin adını Yukarıdaki bağlantıdaki değerle eşleşecek şekilde düzenleyin`"topologyName" : "InferencingWithHttpExtension"`
    * Graphtopologyıdelete altında adı düzenleyin`"name": "InferencingWithHttpExtension"`

### <a name="generate-and-deploy-the-iot-edge-deployment-manifest"></a>IoT Edge dağıtım bildirimini oluşturma ve dağıtma

1. "Src/Edge/Deployment. yolov3. Template. JSON" dosyasına sağ tıklayın ve IoT Edge dağıtım bildirimi oluştur ' a tıklayın.

    ![IoT Edge dağıtım bildirimi oluştur](./media/quickstarts/generate-iot-edge-deployment-manifest-yolov3.png)  
1. Bunun için "Deployment. yolov3. AMD64. JSON" adlı src/Edge/config klasöründe bir bildirim dosyası oluşturulmalıdır.
1. Daha önce [hızlı](detect-motion-emit-events-quickstart.md)başlangıcı tamamladıysanız bu adımı atlayın. Aksi takdirde, sol alt köşedeki AZURE ıOT HUB Bölmesi ' nin yanındaki "daha fazla eylem" simgesine tıklayarak ıothub bağlantı dizesini ayarlayın. Dizeyi appSettings. json dosyasından kopyalayabilirsiniz. ( [IoT Hub 'ı Seç komutu](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Select-IoT-Hub)aracılığıyla Visual Studio Code içinde yapılandırılmış uygun IoT Hub sahip olduğunuzdan emin olmak için başka bir önerilen yaklaşım aşağıda verilmiştir.
    
    ![Iothub bağlantı dizesi](./media/quickstarts/set-iotconnection-string.png)
1. Sonra, "src/Edge/config/Deployment. yolov3. AMD64. JSON" öğesine sağ tıklayın ve "tek cihaz için dağıtım oluştur" a tıklayın. 

    ![Tek cihaz için dağıtım oluştur](./media/quickstarts/create-deployment-single-device.png)
1. Bundan sonra IoT Hub bir cihaz seçmeniz istenir. Açılan listeden LVA-örnek-cihaz ' ı seçin.
1. Yaklaşık 30 saniye içinde, sol alt kısımdaki Azure ıOT hub 'ını yenileyin ve aşağıdaki modüllerin dağıtıldığı uç cihazına sahip olmanız gerekir:

    1. "LvaEdge" olarak adlandırılan canlı video analizi modülü.
    1. Bir RTSP sunucusuna benzetir ve canlı video akışı kaynağı olarak davranan "rtspsim" adlı bir modül.
    1. Ad olarak adlandırılan "yolov3" adlı bir modül, görüntülere görüntü görme ve nesne türlerinin birden çok sınıfını döndüren YOLOv3 nesne algılama modelidir.
 
        ![YOLOv3 nesne algılama modeli](./media/quickstarts/yolov3.png)

### <a name="prepare-for-monitoring-events"></a>İzleme olaylarını hazırlama

Canlı video analizi cihazına sağ tıklayın ve "yerleşik olay uç noktasını Izlemeye başla" düğmesine tıklayın. Bu adım IoT Hub olaylarını izlemek ve Visual Studio Code çıkış penceresinde görmek için gereklidir. 

![İzlemeyi Başlat](./media/quickstarts/start-monitoring-iothub-events.png) 

### <a name="run-the-sample-program"></a>Örnek programı çalıştırma

1. Bir hata ayıklama oturumu başlatın (F5 tuşuna basın). TERMINAL penceresinde yazdırılmış bazı iletileri görmeye başlayacaksınız.
1. Operations. JSON doğrudan Yöntemler Graphtopologyılist ve Graphınstancelist çağrıları ile başlatılır. Önceki hızlı başlangıçlardan sonra kaynakları temizlediyseniz, bu, boş listeler döndürür ve ardından ENTER tuşuna basmanız için duraklatıp
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

1. TERMINAL penceresinde "Enter" tuşuna bastığınızda, bir sonraki doğrudan yöntem çağrısı kümesi yapılır
     * Yukarıdaki Topologyıurl 'yi kullanarak Graphtopologyıset çağrısı
     * Aşağıdaki gövdeyi kullanarak Graphınstanceset öğesine çağrı
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
     * Graph örneğini başlatmak ve video akışını başlatmak için Graphınstanceactivate öğesine yapılan çağrı
     * Grafik örneğinin gerçekten çalışır durumda olduğunu göstermek için Graphınstancelist öğesine yapılan ikinci bir çağrı
1. TERMINAL penceresindeki çıktı, şimdi ' devam etmek için ENTER tuşuna basın ' isteminde duraklatılır. Şu an "Enter" tuşuna basmayın. Çağrdığınız doğrudan yöntemler için JSON yanıtı yüklerini görmek üzere yukarı doğru izleyebilirsiniz
1. Artık Visual Studio Code çıkış penceresine geçiş yaparsanız, IoT Edge modülündeki canlı video analizi tarafından IoT Hub gönderilen iletileri görürsünüz.
     * Bu iletiler aşağıdaki bölümde ele alınmıştır
1. Medya grafiği çalışmaya devam eder ve sonuçları yazdırır; RTSP simülatör kaynak videoyu döngüye sokacaktır. Medya grafiğini durdurmak için, TERMINAL penceresine geri dönerek "Enter" tuşuna basın. Kaynakları temizlemek için sonraki çağrı dizisi yapılır:
     * Grafik örneğini devre dışı bırakmak için Graphınstancedeactivate öğesine çağrı
     * Örneği silmek için Graphınstancedelete öğesine çağrı
     * Topolojiyi silmek için Graphtopologyıdelete çağrısı
     * Listenin artık boş olduğunu göstermek için Graphtopologyılist öğesine yapılan son çağrı

## <a name="interpret-results"></a>Sonuçları yorumlama

Medya grafiğini çalıştırdığınızda, http uzantısı işlemci düğümündeki sonuçlar, IoT Hub IoT Hub havuz düğümü aracılığıyla gönderilir. Visual Studio Code çıkış penceresinde gördüğünüz iletiler bir "gövde" bölümü ve bir "applicationProperties" bölümü içerir. Bu bölümlerin neyi temsil ettiğini anlamak için [Bu](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct) makaleyi okuyun.

Aşağıdaki iletilerde, uygulama özellikleri ve gövdenin içeriği canlı video analizi modülü tarafından tanımlanır. 



### <a name="mediasession-established-event"></a>MediaSession tarafından belirlenen olay

Bir medya grafiği oluşturulduğunda, RTSP kaynak düğümü rtspsim-live55 kapsayıcısında çalışan RTSP sunucusuna bağlanmaya çalışır. Başarılı olursa, bu olayı yazdıracaktır. Olay türü Microsoft. Media. MediaGraph. Diagnostics. Mediasessionsetup ' tur.

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

Yukarıdaki iletide aşağıdakileri aklınızda edin:
* İleti bir tanılama olayıdır, Mediasessionkurulan, RTSP kaynak düğümünün (konu) RTSP simülatörü ile bağlantı kurabildiğini ve (sanal) canlı bir akış almaya başlayabileceğini gösterir.
* applicationProperties 'teki "Subject" iletisi, iletinin medya grafiğindeki RTSP kaynak düğümünden oluşturulduğunu gösterir.
* applicationProperties 'teki "eventType", bunun bir tanılama olayı olduğunu gösterir.
* "eventTime" olayın gerçekleştiği saati gösterir.
* "gövde", bu durumda [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol) ayrıntılarından oluşan tanılama olayı hakkındaki verileri içerir.

### <a name="inference-event"></a>Çıkarım olayı

HTTP uzantısı işlemci düğümü, yolov3 modülünden çıkarım sonuçları alır ve bunları çıkarım olayları olarak IoT Hub havuz düğümü aracılığıyla yayar. Bu olaylarda, türü bir otomobil veya kamyonun gibi bir varlık olduğunu göstermek için "varlık" olarak ayarlanır ve eventTime, nesnenin hangi zamanda (UTC) algılandığını size bildirir. Aynı video çerçevesinde farklı düzeylerde güvenle iki araba algılandığı bir örnek aşağıda verilmiştir.

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

Yukarıdaki iletilerde aşağıdakileri göz önünde edin:

* applicationProperties 'teki "Subject", iletinin oluşturulduğu grafik topolojisinde bulunan düğüme başvurur. 
* applicationProperties 'teki "eventType" bir analiz olayı olduğunu gösterir.
* "eventTime" olayın gerçekleştiği saati gösterir.
* "gövde" analiz olayı hakkındaki verileri içerir. Bu durumda, olay bir çıkarım olayıdır ve bu nedenle gövde "ında" verileri içerir.
* "ikinci dereceden" bölümü, "tür" ın "varlık" olduğunu ve "varlık" hakkında ek veriler olduğunu gösterir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıçlarını denemek istiyorsanız oluşturulan kaynaklarda tutmanız gerekir. Aksi takdirde Azure portal gidin, Kaynak gruplarınızı inceleyin, bu hızlı başlangıcı çalıştırdığınız kaynak grubunu seçin ve tüm kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

Gelişmiş kullanıcılar için ek güçlükleri gözden geçirin:

* RTSP simülatörü kullanmak yerine RTSP desteğiyle bir [IP kamerası](https://en.wikipedia.org/wiki/IP_camera) kullanın. Profiller G, S veya T ile uyumlu cihazlar ' ı arayarak [ONVIF uyumlu](https://www.onvif.org/conformant-products/) ürünler SAYFASıNDA, RTSP desteğiyle IP kameralarını arayabilirsiniz.
* AMD64 veya x64 Linux cihazı kullanın (Azure Linux VM kullanarak). Bu cihaz, IP kamerası ile aynı ağda olmalıdır. [Linux üzerinde Azure IoT Edge çalışma zamanı 'Nı Install](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) ' daki yönergeleri izleyebilir ve sonra bu cihazı Azure IoT Hub 'e kaydetmek için [ilk IoT Edge modülünüzü bir sanal Linux cihaz](https://docs.microsoft.com/azure/iot-edge/quickstart-linux) hızlı başlangıca dağıtma ' daki yönergeleri izleyebilirsiniz.

