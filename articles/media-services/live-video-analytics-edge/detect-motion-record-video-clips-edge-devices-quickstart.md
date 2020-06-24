---
title: Hareket & ekran, uç cihazlarda video kaydetme-Azure
description: Bu hızlı başlangıçta, (sanal) bir IP kamerasından canlı video akışını analiz etmek, herhangi bir hareketin mevcut olup olmadığını algılamak ve bu durumda bir MP4 video klibini, sınır cihazında yerel dosya sistemine kaydetmek için IoT Edge 'da canlı video analizinin nasıl kullanılacağı gösterilmektedir.
ms.topic: quickstart
ms.date: 04/27/2020
ms.openlocfilehash: 32f1ae5e9edbdbe522afb39bd56584cd2423dd33
ms.sourcegitcommit: 1383842d1ea4044e1e90bd3ca8a7dc9f1b439a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84817086"
---
# <a name="quickstart-detect-motion-and-record-video-on-edge-devices"></a>Hızlı başlangıç: Edge cihazlarındaki hareketi algılama ve video kaydetme
 
Bu hızlı başlangıçta, (benzetimli) bir IP kamerasından canlı video akışını çözümlemek için IoT Edge 'da canlı video analizinin nasıl kullanılacağı gösterilmektedir. Herhangi bir hareketin mevcut olup olmadığını nasıl tespit etmek gerektiğini gösterir. Bu durumda, sınır cihazında yerel dosya sistemine bir MP4 video klip kaydedin. Hızlı başlangıç, bir Azure VM 'yi IoT Edge bir cihaz olarak kullanır ve ayrıca sanal bir canlı video akışı kullanır. 

Bu makale, C# dilinde yazılan örnek kodu temel alır. Bu BT, [hareket Algıla ve olayları göster](detect-motion-emit-events-quickstart.md) hızlı başlangıç sayfasında oluşturulur. 

## <a name="prerequisites"></a>Ön koşullar

* Etkin aboneliği olan bir Azure hesabı. Henüz bir [hesabınız yoksa ücretsiz olarak bir hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) .
* [Visual Studio Code](https://code.visualstudio.com/), aşağıdaki uzantılara sahip:
    * [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
    * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).
* [Hareketi Algıla ve olayları göster](detect-motion-emit-events-quickstart.md) hızlı başlangıcı ' nı tamamlamadıysanız şu adımları izleyin:
     1. [Azure kaynakları ayarlama](detect-motion-emit-events-quickstart.md#set-up-azure-resources)
     1. [Geliştirme ortamınızı kurma](detect-motion-emit-events-quickstart.md#set-up-your-development-environment)
     1. [IoT Edge dağıtım bildirimini oluşturma ve dağıtma](detect-motion-emit-events-quickstart.md#generate-and-deploy-the-deployment-manifest)
     1. [Olayları izlemeye hazırlanma](detect-motion-emit-events-quickstart.md#prepare-to-monitor-events)

> [!TIP]
> Azure IoT araçları 'nı yüklerken Docker 'ı yüklemeniz istenebilir. İstemi yok saymaktan çekinmeyin.

## <a name="review-the-sample-video"></a>Örnek videoyu gözden geçirin
Bu hızlı başlangıç için Azure kaynaklarını ayarlarken, bir park lotunun kısa bir videosu, Azure 'daki IoT Edge cihaz olarak kullanılan Linux VM 'sine kopyalanır. Bu video dosyası, bu öğretici için canlı bir akışın benzetimini yapmak üzere kullanılacaktır.

[VLC medya oynatıcı](https://www.videolan.org/vlc/)gibi bir uygulama açın, CTRL + N ' ı seçin ve [Bu bağlantıyı](https://lvamedia.blob.core.windows.net/public/lots_015.mkv) Park lotu videosunu seçerek kayıttan yürütmeyi başlatın. 5 saniyelik işaret hakkında bir beyaz araba, Park partisi üzerinden geçer.

Arabasının hareketini algılamak ve 5 saniyelik işaret etrafında bir video klibi kaydetmek için IoT Edge üzerinde canlı video analizlerini kullanmak için aşağıdaki adımları izleyin.

## <a name="overview"></a>Genel Bakış

![genel bakış](./media/quickstarts/overview-qs4.png)

Yukarıdaki diyagramda, sinyallerin bu hızlı başlangıçta nasıl akagösterdiği gösterilmektedir. [Sınır modülü](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) , gerçek zamanlı bir akış protokolü (RTSP) sunucusunu BARıNDıRAN bir IP kamerasına benzetim yapar. Bir [RTSP kaynak](media-graph-concept.md#rtsp-source) düğümü, bu sunucudan video akışını çeker ve [hareket algılama işlemcisi](media-graph-concept.md#motion-detection-processor) düğümüne video çerçeveleri gönderir. RTSP kaynağı bir olay tarafından tetiklenene kadar kapalı kalan bir [sinyal kapısı işlemci](media-graph-concept.md#signal-gate-processor) düğümüne aynı video çerçevelerini gönderir.

Hareket algılama işlemcisi videoda hareket algıladığında, sinyal kapısı işlemci düğümüne bir olay gönderir ve bunu tetikler. Ağ Geçidi, yapılandırılan süre boyunca açılır ve video çerçevelerini [Dosya havuzu](media-graph-concept.md#file-sink) düğümüne gönderir. Bu havuz düğümü, videoyu uç cihazınızın yerel dosya sisteminde bir MP4 dosyası olarak kaydeder. Dosya yapılandırılan konuma kaydedilir.

Bu hızlı başlangıçta şunları yapmanız gerekir:

1. Medya grafiğini oluşturun ve dağıtın.
1. Sonuçları yorumlayın.
1. Kaynakları temizleyin.

## <a name="examine-and-edit-the-sample-files"></a>Örnek dosyaları İnceleme ve düzenleme
Bu hızlı başlangıç önkoşulları kapsamında, örnek kodu bir klasöre indirdiniz. Örnek kodu incelemek ve düzenlemek için bu adımları izleyin.

1. Visual Studio Code, *src/Edge*bölümüne gidin. *. Env* dosyanızı ve birkaç dağıtım şablonu dosyasını görürsünüz.

    Dağıtım şablonu, sınır cihazının bazı özellikler için kullanıldığı dağıtım bildirimini ifade eder. *. Env* dosyası bu değişkenlerin değerlerini içerir.
1. *Src/buluttan cihaza-Console-App* klasörüne gidin. Burada dosya ve diğer birkaç dosya *appsettings.js* görürsünüz:
    * ***C2D-Console-App. csproj*** -Visual Studio Code için proje dosyası.
    * ***operations.json*** -programın çalıştırmasını istediğiniz işlemler listesi.
    * ***Program.cs*** -örnek program kodu. Bu kod:

        * Uygulama ayarlarını yükler.
        * IoT Edge modülündeki canlı video analizinin sunduğu doğrudan yöntemleri çağırır. [Doğrudan yöntemlerini](direct-methods.md)çağırarak canlı video akışlarını çözümlemek için modülünü kullanabilirsiniz. 
        * Pencereyi, **TERMINAL** penceresinde programın çıktısını Incelemenize ve **Çıkış** penceresinde modül tarafından oluşturulan olayları incelemenize olanak sağlamak için duraklar.
        * Kaynakları temizlemek için doğrudan yöntemleri çağırır.

1. Dosyadaki *operations.js* düzenleyin:
    * Grafik topolojisine olan bağlantıyı değiştirin:

        `"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/evr-motion-files/topology.json"`
    * Altında `GraphInstanceSet` , grafik topolojisinin adını önceki bağlantıdaki değerle eşleşecek şekilde düzenleyin:
    
      `"topologyName" : "EVRToFilesOnMotionDetection"`

    * Video dosyasını işaret etmek için RTSP URL 'sini düzenleyin:

        `"value": "rtsp://rtspsim:554/media/lots_015.mkv"`

    * Altında `GraphTopologyDelete` , adı düzenleyin:

        `"name": "EVRToFilesOnMotionDetection"`

## <a name="review---check-the-modules-status"></a>İnceleme-modüllerin durumunu denetleme

[IoT Edge dağıtım bildirimi oluşturma ve dağıtma](detect-motion-emit-events-quickstart.md#generate-and-deploy-the-deployment-manifest) adımında, Visual Studio Code ' de **Azure IoT Hub** altında **LVA-örnek-cihaz** düğümünü genişletin (sol alt bölümde). Aşağıdaki modüllerin dağıtıldığını görmeniz gerekir:

* **Lvaedge** adlı canlı video analizi modülü
* Canlı video akışı kaynağı görevi gören bir RTSP sunucusunun benzetimini yapan **rtspsim** modülü

  ![Modül](./media/quickstarts/lva-sample-device-node.png)


## <a name="review---prepare-for-monitoring-events"></a>İnceleme-izleme olayları için hazırlanma
[Olayları Izlemeye hazırlanmak](detect-motion-emit-events-quickstart.md#prepare-to-monitor-events)için adımları tamamladığınızdan emin olun.

![Yerleşik olay uç noktasını Izlemeye başla](./media/quickstarts/start-monitoring-iothub-events.png)

## <a name="run-the-sample-program"></a>Örnek programı çalıştırma

1. F5 tuşunu seçerek bir hata ayıklama oturumu başlatın. **TERMINAL** penceresinde bazı iletiler yazdırılır.
1. Koddaki *operations.js* doğrudan yöntemleri `GraphTopologyList` ve ' i çağırır `GraphInstanceList` . Önceki hızlı başlangıçlardan sonra kaynakları temizledikten sonra bu işlem boş listeleri döndürür ve sonra duraklatılır. Enter tuşunu seçin.

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

     * Öğesine yapılan bir çağrı `GraphTopologySet``topologyUrl` 
     * Aşağıdaki gövdesini kullanan öğesine yapılan bir çağrı `GraphInstanceSet` :

         ```
         {
           "@apiVersion": "1.0",
           "name": "Sample-Graph",
           "properties": {
             "topologyName": "EVRToFilesOnMotionDetection",
             "description": "Sample graph description",
             "parameters": [
               {
                 "name": "rtspUrl",
                 "value": "rtsp://rtspsim:554/media/lots_015.mkv"
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
1. **TERMINAL** penceresindeki çıktı, tarihinde duraklatılır `Press Enter to continue` . Henüz ENTER ' ı seçmeyin. Çağrdığınız doğrudan yöntemler için JSON yanıtı yüklerini görmek için yukarı kaydırın.
1. Visual Studio Code **Çıkış** penceresine geçin. IoT Edge modülünün canlı video analizinin IoT Hub 'ına gönderdiği iletileri görürsünüz. Bu hızlı başlangıçta aşağıdaki bölümde bu iletiler ele alınmaktadır.

1. Medya grafiği çalışmaya devam eder ve sonuçları yazdırır. RTSP simülatörü kaynak videoyu döngüye sokmaya devam eder. Medya grafiğini durdurmak için, **TERMINAL** penceresine dönün ve ENTER ' u seçin. 

    Sonraki çağrı dizisi kaynakları temizler:
     * `GraphInstanceDeactivate`Grafik örneğini devre dışı bırakmak için bir çağrı.
     * `GraphInstanceDelete`Örneği silme çağrısı.
     * İçin bir çağrı `GraphTopologyDelete` , topolojiyi siler.
     * İçin son çağrı `GraphTopologyList` , listenin artık boş olduğunu gösterir.

## <a name="interpret-results"></a>Sonuçları yorumlama 
Medya grafiğini çalıştırdığınızda, hareket algılayıcısı işlemci düğümündeki sonuçlar, IoT Hub havuz düğümünden IoT Hub 'ına geçer. Visual Studio Code **Çıkış** penceresinde gördüğünüz iletiler bir `body` bölümü ve bir `applicationProperties` bölümü içerir. Daha fazla bilgi için bkz. [IoT Hub Iletileri oluşturma ve okuma](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct).

Aşağıdaki iletilerde, canlı video analizi modülü, uygulama özelliklerini ve gövdenin içeriğini tanımlar.

### <a name="mediasessionestablished-event"></a>Mediasessionkurulduğu olayı

Bir medya grafiği oluşturulduğunda, RTSP kaynak düğümü, rtspsim-live555 kapsayıcısında çalışan RTSP sunucusuna bağlanmaya çalışır. Bağlantı başarılı olursa, aşağıdaki olay yazdırılır.

```
[IoTHubMonitor] [05:37:21 AM] Message received from [lva-sample-device/lvaEdge]:
{  
"body": {
"sdp": "SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 xxx.xxx.xxx.xxx\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets={SPS}\r\na=control:track1\r\n"  
},  
"applicationProperties": {  
    "dataVersion": "1.0",  
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",  
    "subject": "/graphInstances/Sample-Graph-1/sources/rtspSource",  
    "eventType": "Microsoft.Media.MediaGraph.Diagnostics.MediaSessionEstablished",  
    "eventTime": "2020-05-21T05:37:21.398Z",
    }  
}
```

Önceki çıktıda: 

* İleti bir tanılama olayıdır `MediaSessionEstablished` . RTSP kaynak düğümünün (konu) RTSP simülatörü ile bağlantı kurmadığını ve (benzetimli) canlı akışını almaya başladığını gösterir.
* `applicationProperties`' De, `subject` iletinin oluşturulduğu grafik topolojisinde bulunan düğüme başvurur. Bu durumda, ileti RTSP kaynak düğümünden gelmektedir.
* `applicationProperties`' De, `eventType` Bu olayın bir tanılama olayı olduğunu gösterir.
* `eventTime`Değer, olayın gerçekleştiği zaman değeridir.
* `body`Bölüm, tanılama olayı hakkındaki verileri içerir. Bu durumda, veriler [oturum açıklaması Protokolü (SDP)](https://en.wikipedia.org/wiki/Session_Description_Protocol) ayrıntılarını içerir.

### <a name="recordingstarted-event"></a>RecordingStarted olayı

Hareket algılandığında, sinyal kapısı işlemci düğümü etkinleştirilir ve medya grafiğindeki dosya havuzu düğümü bir MP4 dosyası yazmaya başlar. Dosya havuzu düğümü işlemsel bir olay gönderir. , `type` `motion` Hareket algılama işlemcisinden kaynaklanan bir sonuç olduğunu belirtmek için olarak ayarlanır. `eventTime`Değer, hareketin GERÇEKLEŞTIĞI UTC zamanının değeridir. Bu işlem hakkında daha fazla bilgi için bu hızlı başlangıçta [genel bakış](#overview) bölümüne bakın.

Bu iletinin bir örneği aşağıda verilmiştir:

```
[IoTHubMonitor] [05:37:27 AM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "filePath",
    "outputLocation": "/var/media/sampleFilesFromEVR-filesinkOutput-20200521T053726Z.mp4"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",  
    "subject": "/graphInstances/Sample-Graph-1/sinks/fileSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStarted",
    "eventTime": "2020-05-21T05:37:27.713Z",
    "dataVersion": "1.0"
  }
}
```

Önceki iletide: 

* `applicationProperties`' De, `subject` iletinin oluşturulduğu medya grafiğindeki düğüme başvurur. Bu durumda, ileti dosya havuzu düğümünden gelmektedir.
* `applicationProperties`' De, `eventType` Bu olayın işlevsel olduğunu gösterir.
* `eventTime`Değer, olayın gerçekleştiği zaman değeridir. Bu süre, `MediaSessionEstablished` video akışa başladıktan sonra ve sonra 5-6 saniye arasındadır. Bu süre, Araba Park lotuna [taşımaya başladığında](#review-the-sample-video) 5-6 saniyelik bir işaretine karşılık gelir.
* `body`Bölüm işletimsel olayla ilgili verileri içerir. Bu durumda, veriler `outputType` ve içerir `outputLocation` .
* `outputType`Değişkeni, bu bilgilerin dosya yolu hakkında olduğunu gösterir.
* `outputLocation`Değer, sınır MODÜLÜNDEKI MP4 dosyasının konumudur.

### <a name="recordingstopped-and-recordingavailable-events"></a>Recordingdurduruldu ve RecordingAvailable olayları

[Graph topolojisinde](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/evr-motion-files/topology.json)sinyal kapısı işlemci düğümünün özelliklerini incelerseniz etkinleştirme sürelerinin 5 saniyeye ayarlandığını görürsünüz. Bu nedenle, olay alındıktan sonra 5 saniye boyunca şunları `RecordingStarted` alırsınız:

* `RecordingStopped`Kaydın durdurulduğunu belirten bir olay.
* `RecordingAvailable`MP4 dosyasının artık görüntülenmek üzere kullanılabileceğini gösteren bir olay.

İki olay, genellikle birbirleriyle Saniyeler içinde yayılır.

## <a name="play-the-mp4-clip"></a>MP4 klibini oynat

MP4 dosyaları, OUTPUT_VIDEO_FOLDER_ON_DEVICE anahtarını kullanarak *. env* dosyasında yapılandırdığınız Edge cihazında bir dizine yazılır. Varsayılan değeri kullandıysanız, sonuçlar */Home/lvaadmin/Samples/output/* klasöründe olmalıdır.

MP4 klibini oynatmak için:

1. Kaynak grubunuza gidin, VM 'yi bulun ve ardından Azure savunma kullanarak bağlanın.

    ![Kaynak grubu](./media/quickstarts/resource-group.png)
    
    ![VM](./media/quickstarts/virtual-machine.png)

1. [Azure kaynaklarınızı ayarlarken](detect-motion-emit-events-quickstart.md#set-up-azure-resources)oluşturulan kimlik bilgilerini kullanarak oturum açın. 
1. Komut isteminde ilgili dizine gidin. Varsayılan konum */Home/lvaadmin/Samples/output*' dır. MP4 dosyalarını dizinde görmeniz gerekir.

    ![Çıkış](./media/quickstarts/samples-output.png) 

1. Dosyaları yerel makinenize kopyalamak için [Güvenli kopya (SCP)](https://docs.microsoft.com/azure/virtual-machines/linux/copy-files-to-linux-vm-using-scp) kullanın. 
1. [VLC medya oynatıcı](https://www.videolan.org/vlc/) veya başka bir MP4 oynatıcı kullanarak dosyaları yürütün.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıçlarını denemek istiyorsanız, oluşturduğunuz kaynakları saklayın. Aksi takdirde, Azure portal kaynak gruplarınıza gidin, bu hızlı başlangıcı çalıştırdığınız kaynak grubunu seçin ve ardından tüm kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

* Canlı video akışlarına AI uygulamak için [kendi modelinizdeki canlı video analizlerini çalıştırın](use-your-model-quickstart.md) hızlı başlangıç adımlarını izleyin.
* Gelişmiş kullanıcılar için ek güçlükleri gözden geçirin:

    * RTSP simülatörü kullanmak yerine RTSP 'yi destekleyen bir [IP kamera](https://en.wikipedia.org/wiki/IP_camera) kullanın. [ONVIF uyumlu ürünler](https://www.onvif.org/conformant-products) sayfasında RTSP 'YI destekleyen IP kameralarını bulabilirsiniz. Profiller G, S veya T ile uyumlu olan cihazları arayın.
    * Azure 'da Linux VM kullanmak yerine AMD64 veya x64 Linux cihazı kullanın. Bu cihaz, IP kamerası ile aynı ağda olmalıdır. [Linux üzerinde Azure IoT Edge çalışma zamanını Install](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux)içindeki yönergeleri izleyin. Ardından, cihazı Azure IoT Hub 'a kaydetmek için [ilk IoT Edge modülünüzü bir sanal Linux cihazına dağıtma](https://docs.microsoft.com/azure/iot-edge/quickstart-linux) bölümündeki yönergeleri izleyin.
