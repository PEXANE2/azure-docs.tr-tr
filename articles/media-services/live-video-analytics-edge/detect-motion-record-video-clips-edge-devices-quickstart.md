---
title: Hareket algılama, uç cihazlarda video kaydetme-Azure
description: Bu hızlı başlangıçta, (sanal) bir IP kamerasından canlı video akışını analiz etmek, herhangi bir hareketin mevcut olup olmadığını algılamak ve bu durumda bir MP4 video klibini, sınır cihazında yerel dosya sistemine kaydetmek için IoT Edge 'da canlı video analizinin nasıl kullanılacağı gösterilmektedir.
ms.topic: quickstart
ms.date: 04/27/2020
ms.openlocfilehash: d824870ea95922bbbdbf01cf2c95692522936f85
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84262083"
---
# <a name="quickstart-detect-motion-record-video-on-edge-devices"></a>Hızlı başlangıç: hareket algılama, uç cihazlarda video kaydetme
 
Bu hızlı başlangıçta, (sanal) bir IP kamerasından canlı video akışını analiz etmek, herhangi bir hareketin mevcut olup olmadığını algılamak ve bu durumda bir MP4 video klibini, sınır cihazında yerel dosya sistemine kaydetmek için IoT Edge 'da canlı video analizinin nasıl kullanılacağı gösterilmektedir. Bir Azure VM 'yi IoT Edge bir cihaz ve sanal bir canlı video akışı olarak kullanır. Bu makale, C# dilinde yazılan örnek kodu temel alır.

Bu makalede, [Bu](detect-motion-emit-events-quickstart.md) hızlı başlangıç hakkında daha fazla derleme yapılır. 

## <a name="prerequisites"></a>Ön koşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Aşağıdaki uzantılara sahip makinenizde [Visual Studio Code](https://code.visualstudio.com/) :
    * [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
    * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* Sisteminizde yüklü [.NET Core 3,1 SDK 'sı](https://dotnet.microsoft.com/download/dotnet-core/3.1)
* [Bu](detect-motion-emit-events-quickstart.md) hızlı başlangıcı önceden tamamlamadıysanız aşağıdaki adımları uygulayın:
     * [Azure kaynakları ayarlama](detect-motion-emit-events-quickstart.md#set-up-azure-resources)
     * [Geliştirme ortamınızı kurma](detect-motion-emit-events-quickstart.md#set-up-your-development-environment)
     * [IoT Edge dağıtım bildirimini oluşturma ve dağıtma](detect-motion-emit-events-quickstart.md#generate-and-deploy-the-iot-edge-deployment-manifest)
     * [İzleme olaylarını hazırlama](detect-motion-emit-events-quickstart.md#prepare-for-monitoring-events)

> [!TIP]
> Azure IoT araçlarını yüklerken Docker 'ı yüklemeniz istenebilir. Yok saymaktan çekinmeyin.

## <a name="review-the-sample-video"></a>Örnek videoyu gözden geçirin
Azure kaynaklarını kurmak için yukarıdaki adımların bir parçası olarak, bir park lotunun (kısa) bir videosu IoT Edge cihaz olarak kullanılan Azure 'daki Linux VM 'sine kopyalanacaktır. Bu video dosyası, bu öğretici için canlı bir akışın benzetimini yapmak üzere kullanılacaktır.

[VLC Player](https://www.videolan.org/vlc/)gibi bir uygulama kullanabilir, bunu başlatabilir, Control + N tuşuna basın ve [Bu](https://lvamedia.blob.core.windows.net/public/lots_015.mkv) bağlantıyı Park lotu videosuna yapıştırarak kayıttan yürütmeyi başlatın. 5 saniyelik işaret hakkında bir beyaz araba, Park partisi üzerinden geçer.

Aşağıdaki adımları tamamladığınızda, arabasının bu hareketini saptamak için IoT Edge üzerinde canlı video analizi kullandınız ve 5 saniyelik bir işaret üzerinden başlayarak bir video klibini kaydedecaksınız.

## <a name="overview"></a>Genel Bakış

![genel bakış](./media/quickstarts/overview-qs4.png)

Yukarıdaki diyagramda, sinyallerin bu hızlı başlangıçta nasıl akagösterdiği gösterilmektedir. Bir Edge modülü ( [burada](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555)ayrıntılı), bir RTSP sunucusu BARıNDıRAN bir IP kamerasının benzetimini yapar. Bir [RTSP kaynak](media-graph-concept.md#rtsp-source) düğümü, bu sunucudan video akışını çeker ve [hareket algılama işlemcisi](media-graph-concept.md#motion-detection-processor) düğümüne video çerçeveleri gönderir. RTSP kaynağı bir olay tarafından tetiklenene kadar kapalı kalan bir [sinyal kapısı işlemci](media-graph-concept.md#signal-gate-processor) düğümüne aynı video çerçevelerini gönderir.

Hareket algılama işlemcisi videoda bu hareket olduğunu belirlediğinde, sinyal kapısı işlemci düğümüne bir olay gönderir ve bunu tetikler. Ağ Geçidi, yapılandırılan süre boyunca açılır ve video çerçevelerini [Dosya havuzu](media-graph-concept.md#file-sink) düğümüne gönderir. Bu havuz düğümü, videoyu, yapılandırılmış konumda uç cihazınızın yerel dosya sistemine bir MP4 dosyası olarak kaydeder.

Bu hızlı başlangıçta şunları yapmanız gerekir:

1. Medya grafiğini oluşturma ve dağıtma
1. Sonuçları yorumlama
1. Kaynakları temizleme

## <a name="examine-and-edit-the-sample-files"></a>Örnek dosyaları İnceleme ve düzenleme
Önkoşulların bir parçası olarak, örnek kodu bir klasöre indirmiş olursunuz. Visual Studio Code başlatın ve klasörü açın.

1. Visual Studio Code, "src/Edge" e gidin. Oluşturduğunuz. env dosyasını, birkaç dağıtım şablonu dosyası ile birlikte görürsünüz
    * Dağıtım şablonu, kenar cihazının bazı yer tutucu değerleriyle birlikte dağıtım bildirimini ifade eder. . Env dosyası bu değişkenlerin değerlerini içerir.
1. Sonra, "src/buluttan-cihaza-Console-App" klasörüne gidin. Burada, oluşturduğunuz appSettings. json dosyasını ve diğer birkaç dosyayı görürsünüz:
    * C2D-Console-App. csproj-bu, Visual Studio Code için proje dosyasıdır
    * Operations. JSON-bu dosya, programın çalıştırmasını istediğiniz farklı işlemleri listeler
    * Program.cs-Bu örnek program kodudur ve şunları yapar:

        * Uygulama ayarlarını yükler
        * IoT Edge modülünde canlı video analizi tarafından kullanıma sunulan doğrudan yöntemleri çağırır. [Doğrudan yöntemlerini](direct-methods.md) çağırarak canlı video akışlarını çözümlemek için modülünü kullanabilirsiniz 
        * TERMINAL penceresinde programın çıkışını ve çıkış penceresinde modül tarafından oluşturulan olayları incelemek için duraklamalar
        * Kaynakları temizlemek için doğrudan yöntemleri çağırır   

1. Operations. json dosyasında aşağıdaki düzenlemeleri yapın
    * Grafik topolojisine olan bağlantıyı değiştirin:`"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/evr-motion-files/topology.json"`
    * Graphınstanceset altında, grafik topolojisinin adını Yukarıdaki bağlantıdaki değerle eşleşecek şekilde düzenleyin`"topologyName" : "EVRToFilesOnMotionDetection"`
    * Ayrıca, istenen video dosyasına işaret etmek için RTSP URL 'sini düzenleyin`"value": "rtsp://rtspsim:554/media/lots_015.mkv"`
    * Graphtopologyıdelete altında adı düzenleyin`"name": "EVRToFilesOnMotionDetection"`

## <a name="review---check-status-of-the-modules"></a>Gözden geçirme-modüllerin durumunu denetleme
[IoT Edge dağıtım bildirimini oluşturma ve dağıtma](detect-motion-emit-events-quickstart.md#generate-and-deploy-the-iot-edge-deployment-manifest) adımında, VISUAL STUDIO Code, Azure IoT Hub altında "LVA-örnek-cihaz" düğümünü genişletirseniz (alt sol bölümde), aşağıdaki modüllerin dağıtıldığını görmeniz gerekir

    1. "LvaEdge" olarak adlandırılan canlı video analizi modülü
    1. Bir RTSP sunucusuna benzetir ve canlı video akışı kaynağı görevi gören "rtspsim" adlı bir modül

        ![Modül](./media/quickstarts/lva-sample-device-node.png)


## <a name="review---prepare-for-monitoring-events"></a>İnceleme-izleme olayları için hazırlanma
[İzleme olaylarını hazırlamaya](detect-motion-emit-events-quickstart.md#prepare-for-monitoring-events) yönelik adımları tamamladığınızdan emin olun

![Yerleşik olay uç noktasını Izlemeye başla](./media/quickstarts/start-monitoring-iothub-events.png)

## <a name="run-the-sample-program"></a>Örnek programı çalıştırma

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
Medya grafiğini çalıştırdığınızda, hareket algılayıcısı işlemci düğümündeki sonuçlar, IoT Hub IoT Hub havuz düğümü aracılığıyla gönderilir. Visual Studio Code çıkış penceresinde gördüğünüz iletiler bir "gövde" bölümü ve bir "applicationProperties" bölümü içerir. Bu bölümlerin neyi temsil ettiğini anlamak için [Bu](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct) makaleyi okuyun.

Aşağıdaki iletilerde, uygulama özellikleri ve gövdenin içeriği canlı video analizi modülü tarafından tanımlanır.

## <a name="mediasession-established-event"></a>MediaSession tarafından belirlenen olay

Bir medya grafiği oluşturulduğunda, RTSP kaynak düğümü rtspsim-live555 kapsayıcısında çalışan RTSP sunucusuna bağlanmaya çalışır. Başarılı olursa, bu olayı yazdıracaktır:

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

* İleti bir tanılama olayıdır, Mediasessionkurulan, RTSP kaynak düğümünün (konu) RTSP simülatörü ile bağlantı kurabildiğini ve (sanal) canlı bir akış almaya başlayabileceğini gösterir.
* ApplicationProperties 'teki "Subject" değeri, iletinin oluşturulduğu grafik topolojisinde bulunan düğüme başvurur. Bu durumda, ileti RTSP kaynak düğümünden gönderilir.
* applicationProperties 'teki "eventType" bir tanılama olayı olduğunu gösterir.
* "eventTime" olayın gerçekleştiği saati gösterir.
* "gövde", bu durumda [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol) ayrıntılarından oluşan tanılama olayı hakkındaki verileri içerir.


## <a name="recording-started-event"></a>Kayıt başlatıldı olayı

[Burada](#overview)açıklandığı gibi, hareket algılandığında, sinyal kapısı işlemci düğümü etkinleştirilir ve medya grafiğindeki dosya havuzu düğümü bir MP4 dosyası yazmaya başlar. Dosya havuzu düğümü Işlemsel bir olay gönderir. Tür "Motion" olarak ayarlanır ve bu, hareket algılama Işlemcisinden kaynaklanan bir sonuç olduğunu gösterir ve eventTime, size (UTC) hareketin oluştuğunu söyler. Aşağıda bir örnek verilmiştir:

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

* applicationProperties 'teki "Subject", iletinin oluşturulduğu medya grafiğindeki düğüme başvurur. Bu durumda, ileti dosya havuzu düğümünden gönderilir.
* applicationProperties 'teki "eventType" bir Işletimsel olay olduğunu gösterir.
* "eventTime" olayın gerçekleştiği saati gösterir. Mediasessionkurulduğu ve videonun akışa başladıktan sonra bu 5-6 saniye değerini unutmayın. Bu, Araba Park lotuna [taşımaya başladığında](#review-the-sample-video) oluşan 5-6 ikinci işaretine karşılık gelir
* "gövde" işletimsel olay hakkında, bu örnekte "outputType" ve "outputLocation" verileri olan verileri içerir.
* "outputType" Bu bilgilerin dosya yolu hakkında olduğunu belirtir
* "outputLocation", bir MP4 dosyasının konumunu Edge modülünün içinden sağlar

## <a name="recording-stopped-and-available-events"></a>Kayıt durduruldu ve kullanılabilir olaylar

[Graph topolojisinde](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/evr-motion-files/topology.json)sinyal kapısı işlemci düğümünün özelliklerini incelerseniz, etkinleştirme sürelerinin 5 saniyeye ayarlandığını görürsünüz. Bu nedenle, RecordingStarted olayı alındıktan sonra 5 saniye boyunca
* Kaydın durdurulduğunu belirten Recordingdurdurulan olay
* MP4 dosyasının artık görüntülenmek üzere kullanılabileceğini gösteren RecordingAvailable olayı

İki olay, genellikle birbirleriyle saniyeler ile yayılır.

### <a name="playing-back-the-mp4-clip"></a>MP4 klibini kayıttan yürütme

1. MP4 dosyaları, bu anahtar OUTPUT_VIDEO_FOLDER_ON_DEVICE aracılığıyla. env dosyasında yapılandırdığınız Edge cihazında bir dizine yazılır. Bu değeri varsayılan değere bıraktıysanız, sonuçlar/Home/lvaadmin/Samples/output/konumunda olmalıdır.
1. Kaynak grubunuza gidin, sanal makineyi bulun ve savunma kullanarak bağlanın

    ![Kaynak grubu](./media/quickstarts/resource-group.png)
 
    ![VM](./media/quickstarts/virtual-machine.png)
1. Oturum açıldıktan sonra ( [Bu](detect-motion-emit-events-quickstart.md#set-up-azure-resources) adım sırasında oluşturulan kimlik bilgilerini kullanarak), komut isteminde ilgili dizine gidin (varsayılan:/Home/lvaadmin/Samples/output) ve MP4 dosyalarını orada görmeniz gerekir. Dosyaları yerel makinenize [SCP](https://docs.microsoft.com/azure/virtual-machines/linux/copy-files-to-linux-vm-using-scp) yapabilir ve [VLC oynatıcı](https://www.videolan.org/vlc/) veya başka bir MP4 oynatıcı aracılığıyla geri oynatabilirsiniz.

    ![Çıktı](./media/quickstarts/samples-output.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıçlarını denemek istiyorsanız oluşturulan kaynaklarda tutmanız gerekir. Aksi takdirde Azure portal gidin, Kaynak gruplarınızı inceleyin, bu hızlı başlangıcı çalıştırdığınız kaynak grubunu seçin ve tüm kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

* Canlı video akışlarına nasıl AI uygulanacağını gösteren [kendi model hızlı başlangıç Ile canlı video analizi Çalıştır](use-your-model-quickstart.md) ' ı çalıştırın.
* Gelişmiş kullanıcılar için ek güçlükleri gözden geçirin:

    * RTSP simülatörü kullanmak yerine RTSP desteğiyle bir [IP kamerası](https://en.wikipedia.org/wiki/IP_camera) kullanın. Profiller G, S veya T ile uyumlu cihazlar ' ı arayarak [ONVIF uyumlu ürünler](https://en.wikipedia.org/wiki/IP_camera) SAYFASıNDA, RTSP desteğiyle IP kameralarını arayabilirsiniz.
    * AMD64 veya x64 Linux cihazı kullanın (Azure Linux VM kullanarak). Bu cihaz, IP kamerası ile aynı ağda olmalıdır. [Linux 'ta Azure IoT Edge çalışma zamanını Install](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) ' daki yönergeleri izleyebilir ve sonra Azure IoT Hub 'e kaydetmek için [ilk IoT Edge modülünüzü bir sanal Linux cihaz](https://docs.microsoft.com/azure/iot-edge/quickstart-linux) hızlı başlangıçlarına dağıtma ' daki yönergeleri izleyebilirsiniz.
