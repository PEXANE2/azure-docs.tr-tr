---
title: Bulut öğreticiden buluta ve kayıttan yürütmeye yönelik olay tabanlı video kaydı-Azure
description: Bu öğreticide, bulutta bulut ve kayıttan yürütmeye yönelik olay tabanlı bir video kaydı gerçekleştirmek için IoT Edge canlı video analizinin nasıl kullanılacağını öğreneceksiniz.
ms.topic: tutorial
ms.date: 05/27/2020
ms.openlocfilehash: daab1f06d8950aa7710c7e808ea6362ee3bfd626
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84261933"
---
# <a name="tutorial-event-based-video-recording-to-cloud-and-playback-from-cloud"></a>Öğretici: bulutta buluta ve kayıttan yürütmeye yönelik olay tabanlı video kaydı

Bu öğreticide canlı video analizinin IoT Edge bulutta Media Services üzere seçmeli olarak kaydetmek için nasıl kullanılacağını öğreneceksiniz. Bu kullanım örneği, bu öğreticide [olay tabanlı video kaydı](event-based-video-recording-concept.md) (EVR) olarak adlandırılır. Bunu gerçekleştirmek için, videodaki nesneleri aramak ve video kliplerini yalnızca belirli bir nesne türü algılandığında kaydetmek için bir nesne algılama AI modeli kullanacaksınız. Ayrıca, Media Services kullanarak kayıtlı video kliplerini kayıttan yürütmeyi de öğreneceksiniz. Bu, ilgi çekici bir video klip Arşivi tutmanın gerektiği çeşitli senaryolar için yararlıdır.

> [!div class="checklist"]
> * İlgili kaynakları ayarlayın
> * EVR 'yi gerçekleştiren kodu inceleyin
> * Örnek kodu çalıştırma
> * Sonuçları İnceleme ve videoyu görüntüleme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="suggested-pre-reading"></a>Önerilen önceden okuma  

Aşağıdaki belge sayfalarını okumanız önerilir

* [IoT Edge genel bakış üzerinde canlı video analizi](overview.md)
* [IoT Edge terminolojisinde canlı video analizi](terminology.md)
* [Medya grafiği kavramları](media-graph-concept.md) 
* [Olay tabanlı video kaydı](event-based-video-recording-concept.md)
<!--* [Quickstart: Event-based recording based on motion events]()-->
* [Öğretici: IoT Edge modülünü geliştirme](https://docs.microsoft.com/azure/iot-edge/tutorial-develop-for-linux)
* [Dağıtım. *. Template. JSON nasıl düzenlenir](https://github.com/microsoft/vscode-azure-iot-edge/wiki/How-to-edit-deployment.*.template.json)
* [IoT Edge dağıtım bildiriminde yolların nasıl bildirilemeyeceğini gösteren](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) bölüm

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticinin Önkoşulları aşağıdaki gibidir

* Geliştirme makinenize [Docker](https://docs.docker.com/desktop/) 'yi yüklemeyin
* [Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) uzantısı ve [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) uzantısı ile geliştirme makinenizde [Visual Studio Code](https://code.visualstudio.com/) .

    > [!TIP]
    > Docker 'ı yüklemek isteyip istemediğiniz sorulabilir. Bu istemi yoksayabilirsiniz.
* Geliştirme makinenizde [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/thank-you/sdk-3.1.201-windows-x64-installer) .
* [Canlı video analizi kaynakları kurulum betiğini](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) doldurun ve [ortamı ayarlayın](https://review.docs.microsoft.com/en-us/azure/media-services/live-video-analytics-edge/detect-motion-emit-events-quickstart?branch=release-preview-media-services-lva#set-up-the-environment)

Yukarıdaki adımların sonunda aşağıdakiler dahil olmak üzere Azure aboneliğine dağıtılan belirli Azure kaynakları olacaktır:

* IoT Hub
* Depolama hesabı
* Media Services hesabı
* Bir Linux sanal makinesi

## <a name="concepts"></a>Kavramlar

![Medya grafiği](./media/event-based-video-recording-tutorial/overview.png)

Olay tabanlı video kaydı (EVR), bir olay tarafından tetiklenen videoyu kaydetme sürecini ifade eder. Söz konusu olay, video sinyalinin kendisinden kaynaklanmasıyla (örneğin, videoda hareketli bir nesneyi algılamayla) veya bağımsız bir kaynaktan (örneğin, bir kapı açısına) kaynaklanabilir. Alternatif olarak, yalnızca bir dış ınpoger hizmeti belirli bir olayın oluştuğunu algıladığında kayıt tetikleyebilirsiniz.  Bu öğreticide, bir otobana hareket eden bir eğitim videosu kullanacaksınız ve her kamyon algılandığında video kliplerini kaydedecaksınız.

Yukarıdaki diyagramda bir [medya grafiğinin](media-graph-concept.md) bir resim gösterimi ve istenen senaryoyu gerçekleştiren ek modüller bulunur. Dahil dört IoT Edge modül vardır:

* IoT Edge modülünde canlı video analizi
* [Yolo v3 modeli](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx) kullanılarak oluşturulan bir AI modülü
* Bu öğreticide derleyip dağıtacağınızı nesneleri saymak ve filtrelemek için özel bir modül (Yukarıdaki diyagramda nesne sayacı olarak adlandırılır)
* Bir RTSP kamerasının benzetimini yapmak için bir [RTSP simülatör modülü](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555)
    
Diyagramda gösterildiği gibi, canlı videoyu yakalamak ve bu videoyu iki yola göndermek için medya grafiğinde bir [RTSP kaynak](media-graph-concept.md#rtsp-source) düğümü kullanacaksınız.

* İlk yol, video çerçevelerini belirtilen kare hızında çıkaran bir [kare hızı filtre işlemcisi](media-graph-concept.md#frame-rate-filter-processor) düğümüdür. Bu video çerçeveleri bir HTTP uzantısı düğümüne giriş olarak sunucu. HTTP uzantısı düğümü, AI modülüne (bir nesne algılayıcısı olan YOLO v3) çerçeveler (görüntü olarak) gönderir ve sonuçları alır; bu, model tarafından algılanan nesneler olacaktır. HTTP uzantısı düğümü daha sonra IoT Hub Ileti havuzu aracılığıyla sonuçları IoT Edge hub 'ına yayınlar
* Nesne sayaç modülü, nesne algılama sonuçlarını (trafikte araçlar) içeren IoT Edge hub 'ından ileti alacak şekilde ayarlanır. Belirli bir türdeki nesneleri arayan iletileri denetler (bir ikizi özelliği aracılığıyla yapılandırılır) ve IoT Edge hub 'ına bir ileti verir. Bu iletiler daha sonra medya grafiğinin IoT Hub kaynak düğümüne yönlendirilir. Bir ileti aldıktan sonra medya grafiğindeki IoT Hub kaynak düğümü, ağ geçidini yapılandırılmış bir süre için açmak üzere [sinyal kapısı işlemci](media-graph-concept.md#signal-gate-processor) düğümünü tetikler. Video, o süre için varlık havuzu düğümüne ağ geçidi üzerinden akar. Canlı akışın bu bölümü daha sonra [varlık havuzu](media-graph-concept.md#asset-sink) düğümü aracılığıyla Azure Media Service hesabınızdaki bir [varlığa](terminology.md#asset) kaydedilir.

## <a name="set-up-the-environment"></a>Ortamı ayarlama

1. Depoyu buradan kopyalayın https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp .
2. Visual Studio Code (VSCode) başlatın ve deponun indirileceği klasörü açın.
3. VSCode 'da "src/Cloud-cihaza-Console-App" klasörüne gidin ve "appSettings. JSON" adlı bir dosya oluşturun. Bu dosya, programı çalıştırmak için gereken ayarları içerir.
3. Kaynak kurulum betiğini yürüttükten sonra CloudDrive/LVA-Sample/appSettings. json dosyasının içeriğini kopyalayın. Bkz. [önkoşul #4](event-based-video-recording-tutorial.md#prerequisites). Kaynak kurulum betiği bittikten sonra, klasör yapısını göstermek için küme ayraçları üzerine tıklayın. CloudDrive/LVA-Sample altında oluşturulan üç dosya görürsünüz. Şu anda. env dosyaları ve AppSetting. JSON. Hızlı başlangıçta Visual Studio Code içindeki dosyaları güncelleştirmek için bunlara ihtiyacınız olacaktır. Bunları şimdilik yerel bir dosyaya kopyalamak isteyebilirsiniz.

    ![Uygulama ayarları](./media/quickstarts/clouddrive.png)

    CloudDrive/LVA-Sample/appSettings. JSON dosyasındaki metin şöyle görünmelidir:

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```
1. Sonra, "src/Edge" klasörüne gidin ve ". env" adlı bir dosya oluşturun.
1. İçeriği CloudDrive/LVA-Sample/. env dosyasından kopyalayın. Metin şöyle görünmelidir:

    ```
    SUBSCRIPTION_ID="<Subscription ID>"  
    RESOURCE_GROUP="<Resource Group>"  
    AMS_ACCOUNT="<AMS Account ID>"  
    IOTHUB_CONNECTION_STRING="HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx"  
    AAD_TENANT_ID="<AAD Tenant ID>"  
    AAD_SERVICE_PRINCIPAL_ID="<AAD SERVICE_PRINCIPAL ID>"  
    AAD_SERVICE_PRINCIPAL_SECRET="<AAD SERVICE_PRINCIPAL ID>"  
    INPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"  
    OUTPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"  
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry username>"      
    ```
## <a name="examine-the-template-file"></a>Şablon dosyasını inceleyin 

Ortamı kurulum sırasında Visual Studio Code başlattınız ve örnek kodu içeren klasörü açtınız.

Visual Studio Code, "src/Edge" e gidin. Oluşturduğunuz. env dosyasının yanı sıra birkaç dağıtım şablonu dosyası görürsünüz. Bu şablonlar, Linux VM 'ye hangi kenar modüllerini dağıtacağınızı tanımlar. . Env dosyası, Azure IoT Hub aracılığıyla Edge modüllerine komut göndermenize olanak sağlayan IoT Hub bağlantı dizesi gibi bu şablonlarda kullanılan değişkenlerin değerlerini içerir.

"Src/Edge/Deployment. objectCounter. Template. JSON" öğesini açın. "Modüller" bölümünde listelenen öğelere (kavramlar bölümünde) karşılık gelen dört giriş olduğunu unutmayın:

* lvaEdge: IoT Edge modülünde canlı video analizi
* yolov3 – bu, YOLO v3 modeli kullanılarak oluşturulan çıkarım hizmetidir
* rtspsim: Bu RTSP simülatör
* objectCounter: yolov3 tarafından döndürülen iletilerde belirli nesneler için görünen modüldür

ObjectCounter modülü için, "görüntü" değeri için kullanılan dizeye bakın – bu, IoT Edge bir modül geliştirme öğreticisine dayanır. Visual Studio Code, nesne sayaç modülü için kodun "src/Edge/modules/objectCounter" altında olduğunu otomatik olarak tanır. IoT Edge dağıtım bildiriminde yolları bildirme hakkında bölümünü okuyun ve ardından şablon JSON dosyasındaki yolları inceleyin. Nasıl yapılacağını aklınızda edin:

* LVAToObjectCounter, belirli olayları objectCounter modülündeki belirli bir uç noktaya göndermek için kullanılır
* LvaEdge modülünde bir tetikleyici olayını belirli bir uç noktaya (IoT Hub kaynak düğümü olması gerekir) göndermek için ObjectCounterToLVA kullanılır
* objectCounterToIoTHub, hata ayıklama aracı olarak kullanılır – bu öğreticiyi çalıştırdığınızda objectCounter 'dan çıktıyı görmenizi sağlar

> [!NOTE]
> ObjectCounter modülünün istenen özellikleri: "kamyon" olarak etiketlenen nesneleri en az %50 olan güven düzeyine bakmak üzere ayarlanır.

## <a name="deploy-the-edge-modules"></a>Edge modüllerini dağıtma

Visual Studio Code kullanarak Docker 'da oturum açmak için yönergeleri izleyin ve "çözümü derleyin ve IoT Edge gönderin", ancak src/Edge/Deployment. objectCounter. Template. JSON komutunu bu adım için kullanın.

![IoT Edge çözümü oluşturun ve gönderin](./media/event-based-video-recording-tutorial/build-push.png)

Bu, nesne sayma için objectCounter modülünü oluşturur ve görüntüyü Azure Container Registry (ACR) olarak gönderir

* CONTAINER_REGISTRY_USERNAME_myacr ortam değişkenlerine sahip olup olmadığını ve. env dosyasında CONTAINER_REGISTRY_PASSWORD_myacr tanımlı olduğunu kontrol edin

Yukarıdaki adım src/Edge/config/Deployment. objectCounter. AMD64. JSON konumunda IoT Edge dağıtım bildirimini oluşturacaktır.

Visual Studio Code, src/Edge/config/Deployment. objectCounter. AMD64. JSON dosyasına gidin, dosyaya sağ tıklayın ve "tek cihaz için dağıtım oluştur" ' u seçin. 

IoT Edge, canlı video analiziyle ilgili ilk öğreticeniz varsa, Visual Studio Code ıothub bağlantı dizesini girmek isteyip isteirsiniz. Bunu appSettings. json dosyasından kopyalayabilirsiniz.

Ardından Visual Studio Code bir IoT Hub cihazı seçmenizi ister. IoT Edge cihazınızı seçin ("LVA-örnek-cihaz" olmalıdır).

Bu aşamada, Edge modüllerinin IoT Edge cihazınıza dağıtılması başladı.
Yaklaşık 30 saniye içinde, Visual Studio Code 'in sol alt bölümünde Azure ıOT hub 'ını yenileyin ve dağıtılan 4 modül olduğunu görmeniz gerekir (adları yeniden unutmayın: lvaEdge, rtspsim, yolov3 ve objectCounter)

![4 modül dağıtıldı](./media/event-based-video-recording-tutorial/iot-hub.png)

## <a name="prepare-for-monitoring-events"></a>İzleme olaylarını hazırlama

Sınır cihazına ("LVA-örnek-cihaz") sağ tıklayın ve "yerleşik olay uç noktasını Izlemeye başla" seçeneğine tıklayın. IoT Edge modülündeki canlı video analizi, [işletimsel](#operational-events) ve [Tanılama](#diagnostic-events) olaylarını IoT Edge hub 'ına yayar ve bu olayları Visual Studio Code "çıktı" penceresinde görebilirsiniz.

## <a name="run-the-program"></a>Programı çalıştırma

1. Visual Studio Code, "src/Cloud-to-Device-Console-App/Operations. JSON" öğesine gidin
1. GraphTopologySet düğümü altında şunları ayarlayın: "topologyUrl" https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/evr-hubMessage-assets/topology.json : "" 
1. Ardından, Graphınstanceset düğümünün altında "topologyName" öğesini düzenleyin: "EVRtoAssetsOnObjDetect"
1. "F5" e basın. Bu, hata ayıklama oturumunu başlatacak.
1. TERMINAL penceresinde, program tarafından yapılan [doğrudan yöntem](direct-methods.md) çağrılarının yanıtlarını IoT Edge modülündeki canlı video analizi ' ne görürsünüz.

    1. Graphtopologyılist: varsa, modüle eklenen grafik topolojilerinin bir listesini alır.

        Devam etmek için ENTER tuşuna basın
    1. Graphınstancelist: varsa, oluşturulan grafik örneklerinin bir listesini alır

        Devam etmek için ENTER tuşuna basın
    1. Graphtopologyıset: modüle "EVRtoAssetsOnObjDetect" adlı yukarıdaki topolojiyi ekler
    1. Graphınstanceset: Yukarıdaki topolojinin parametreleri yerine bir örneğini oluşturur
    1. İlgilendiğiniz rtspUrl parametresidir. Linux VM 'ye indirilen MKV dosyasını, RTSP benzeticisinin okuduğu bir konuma işaret eder
    1. Graphınstanceactivate – medya grafiğini başlatır ve videonun akışa geçmesine neden olur
    1. Graphınstancelist: artık, çalıştıran modülde bir örneğe sahip olduğunu göstermek için

        Bu noktada, bunu duraklamalı *ve ENTER tuşuna* basmalısınız
1. Çıkış penceresinde, IoT Edge modülündeki canlı video analizi tarafından IoT Hub gönderilen işletimsel ve tanılama iletilerini görürsünüz
1. Medya grafiği çalışmaya devam eder ve olayları yazdırır; RTSP simülatörü kaynak videoyu döngüye sokacaktır. Medya grafiğini durdurmak için, TERMINAL penceresinde yeniden ENTER tuşuna basabilirsiniz. Program şu şekilde gönderilir:

    1. Graphınstancedeactivate-grafik örneğini durdurmak ve video kaydını durdurmak için
    1. Graphınstancedelete – örneği modülden silmek için
    1. Graphınstancelist: artık modülde örnek olmadığını göstermek için

> [!NOTE]
> Grafik topolojisi silinmedi. Bunu yapmanız gerekirse aşağıdaki JSON gövdesiyle bu adımı çalıştırın:

```
{
    "@apiVersion" : "1.0",
    "name" : "EVRtoAssetsOnObjDetect"
}
```

## <a name="examine-the-output"></a>Çıktıyı inceleme
 
IoT Edge modülündeki canlı video analizi, [işletimsel](#operational-events) ve [tanılama](#diagnostic-events) olaylarını, Visual Studio Code çıkış penceresinde gördüğünüz metin olan IoT Edge hub 'ına yayar, bu IoT Hub tarafından cihazdan buluta iletişimler için kurulan akış mesajlaşma biçimini izler:

* Uygulama özellikleri kümesi. İleti gövdesinin serisini kaldırmak gerekmeden bir uygulamanın tanımlayabilmesine ve erişebileceği dize özelliklerinin bir sözlüğü. IoT Hub bu özellikleri hiçbir şekilde değiştirmeyin
* Donuk ikili gövde

Aşağıdaki iletilerde, uygulama özellikleri ve gövdenin içeriği IoT Edge modülündeki canlı video analizi tarafından tanımlanır. Daha fazla bilgi için bkz. [izleme ve günlüğe kaydetme](monitoring-logging.md). 

## <a name="diagnostic-events"></a>Tanılama olayları

### <a name="mediasessionestablished-event"></a>Mediasessionkurulduğu olayı 

Bir medya grafiği oluşturulduğunda, RTSP kaynak düğümü RTSP simülatör kapsayıcısı üzerinde çalışan RTSP sunucusuna bağlanmaya çalışır. Başarılı olursa, bu olayı yazdıracaktır. Olay türünün Microsoft. Media. MediaGraph. Diagnostics. Mediasessionkurdu olduğunu unutmayın.

```
[IoTHubMonitor] [2:02:54 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1589749373980489 1 IN IP4 172.18.0.4\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.04.12\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets=XXXXXXXXXXX\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sources/rtspSource",
    "eventType": "Microsoft.Media.Graph.Diagnostics.MediaSessionEstablished",
    "eventTime": "2020-05-17T21:02:53.981Z",
    "dataVersion": "1.0"
  }
}
```

Şunlara dikkat edin:

* ApplicationProperties 'teki "Subject" değeri, iletinin oluşturulduğu MediaGraph içindeki düğüme başvurur. Bu durumda, ileti RTSP kaynak düğümünden gönderilir.
* applicationProperties 'teki "eventType" bir tanılama olayı olduğunu belirtiyor
* "eventTime" olayın gerçekleştiği saati gösterir.
* "gövde", tanılama olayı hakkında veriler içeriyor-bu, SDP iletisidir

EventTime 'ı yazın. Bu, trafik videosunun (MKV dosyası), modüle canlı bir akış olarak gelmesi için başladığı süredir.

## <a name="operational-events"></a>İşletimsel olaylar

Medya grafiği bir süre çalıştıktan sonra, sonuç olarak nesne sayacı modülünden bir olay alırsınız. 

```
[IoTHubMonitor] [2:03:21 PM] Message received from [lva-sample-device/objectCounter]:
{
  "body": {
    "count": 2
  },
  "applicationProperties": {
    "eventTime": "2020-05-17T21:03:21.062Z"
  }
}
```

ApplicationProperties yalnızca, modülün YOLO v3 modülünün sonuçlarının ilgilenme nesneleri (structuralks) içerdiğini gözlemlediği zaman olan eventTime ' i içerir.

Videoda diğer structuralks algılandığında bu olaylardan daha fazla bilgi görebilirsiniz.

### <a name="recordingstarted-event"></a>RecordingStarted olayı

Nesne sayacından hemen hemen sonra olayı gönderdikten sonra, Microsoft. Media. Graph. Işletimsel. RecordingStarted türünde bir olay görürsünüz

```
[IoTHubMonitor] [2:03:22 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAssetFromEVR-LVAEdge-20200517T210321Z"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-2/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStarted",
    "eventTime": " 2020-05-17T21:03:22.532Z",
    "dataVersion": "1.0"
  }
}
```

ApplicationProperties 'teki "Subject", bu iletiyi oluşturan grafikteki varlık havuzu düğümüne başvurur.

Gövde, çıkış konumuyla ilgili bilgiler içerir, bu durumda Videonun kaydedildiği Azure Media Service varlığının adıdır. Bu değeri görmeniz gerekir.

### <a name="recordingavailable-event"></a>RecordingAvailable olayı

Varlık havuzu düğümü, varlığa video yüklediğini, Microsoft. Media. Graph. Işletimsel. RecordingAvailable türünde bu olayı yayar

```
[IoTHubMonitor] [2:03:31 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAssetFromEVR-LVAEdge-20200517T210321Z"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-2/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingAvailable",
    "eventTime": "2020-05-17T21:03:31.808Z",
    "dataVersion": "1.0"
  }
}
```

Bu olay, oyuncuların/istemcilerin videonun kayıttan yürütmesini başlatması için varlığa yeterli miktarda veri yazıldığını gösterir.

ApplicationProperties 'teki "Subject" değeri, bu iletiyi oluşturan grafikteki AssetSink düğümüne başvurur.

Gövde, çıkış konumuyla ilgili bilgiler içerir, bu durumda Videonun kaydedildiği Azure Media Service varlığının adıdır.

### <a name="recordingstopped-event"></a>Recordingdurdurulan olay

Topoloji içindeki sinyal kapısı Işlemci düğümü için etkinleştirme ayarlarını (maximumActivationTime) incelerseniz, ağ geçidinin, 30 saniyelik videonun üzerinden kapanması için kurulum olduğunu görürsünüz. Bu nedenle, RecordingStarted olayından yaklaşık 30 saniye sonra, varlık havuzu düğümünün varlık için video kaydını durdurduğunu belirten Microsoft. Media. Graph. Operational. Recordındurdurultüründe bir olay görmeniz gerekir.

```
[IoTHubMonitor] [2:03:52 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAssetFromEVR-LVAEdge-20200517T210321Z"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-2/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStopped",
    "eventTime": "2020-05-17T21:03:52.040Z",
    "dataVersion": "1.0"
  }
}
```

Bu olay, kaydın durdurulduğunu gösterir.

ApplicationProperties 'teki "Subject" değeri, bu iletiyi oluşturan grafikteki AssetSink düğümüne başvurur.

Gövde, çıkış konumuyla ilgili bilgiler içerir, bu durumda Videonun kaydedildiği Azure Media Service varlığının adıdır.

## <a name="media-services-asset"></a>Media Services varlık  

Azure portal oturum açarak ve videoyu görüntüleyerek grafik tarafından oluşturulan Media Services varlığını inceleyebilirsiniz.

1. Web tarayıcınızı açın ve [Azure Portal](https://portal.azure.com/)gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.
1. Aboneliğinizdeki kaynaklar arasında Media Services hesabınızı bulun ve hesap dikey penceresini açın
1. Media Services listesinde varlıklar ' a tıklayın

    ![Varlıklar](./media/continuous-video-recording-tutorial/assets.png)
1. SampleAssetFromCVR-LVAEdge-{DateTime} adında listelenen bir varlık bulacaksınız – bu, medya grafiği topolojisi dosyanızda seçilen adlandırma deseninin adıdır.
1. Varlığa tıklayın.
1. Varlık ayrıntıları sayfasında, akış URL 'SI altında **Yeni oluştur** metin kutusunu tıklatın.

    ![Yeni varlık](./media/continuous-video-recording-tutorial/new-asset.png)

1. Açılan sihirbazda, varsayılan seçenekleri kabul edin ve "Ekle" düğmesine basın. Daha fazla bilgi için bkz. [video kayıttan yürütme](video-playback-concept.md).

    > [!TIP]
    > [Akış uç noktanızın çalıştığından](../latest/streaming-endpoint-concept.md)emin olun.
1. Player Videoyu yüklemesi gerekir ve bunu görüntülemek Için **oynatma**> * * tuşuna basabileceksiniz.

> [!NOTE]
> Videonun kaynağı bir kamera akışını taklit eden bir kapsayıcı olduğundan, videodaki zaman damgaları grafik örneğini etkinleştirdiğinizde ve devre dışı bırakıldığında ile ilgilidir. Daha fazla bilgi için bkz. çok günlü bir kayda nasıl gözatabileceği ve bu arşivin bölümlerini görüntüleme hakkında [çok günlük kayıtları oynatma](playback-multi-day-recordings-tutorial.md) . Bu öğreticide, ekranda görüntülenen videodaki zaman damgalarını de görebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer öğreticileri denemek istiyorsanız, oluşturulan kaynaklarda tutmanız gerekir. Aksi takdirde, Azure portal gidin, Kaynak gruplarınızı inceleyin, bu öğreticiyi çalıştırdığınız kaynak grubunu seçin ve kaynak grubunu silin.

## <a name="next-steps"></a>Sonraki adımlar

* RTSP simülatörü kullanmak yerine RTSP desteğiyle bir [IP kamerası](https://en.wikipedia.org/wiki/IP_camera) kullanın. Profiller G, S veya T ile uyumlu cihazlar ' ı arayarak [ONVIF uyumlu ürünler sayfasında](https://www.onvif.org/conformant-products/) , RTSP desteğiyle IP kameralarını arayabilirsiniz.
* AMD64 veya x64 Linux cihazı kullanın (Azure Linux VM kullanarak). Bu cihaz, IP kamerası ile aynı ağda olmalıdır. [Linux 'ta Azure IoT Edge çalışma zamanını Install](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) ' daki yönergeleri izleyebilir ve sonra Azure IoT Hub 'e kaydetmek için [ilk IoT Edge modülünüzü bir sanal Linux cihaz](https://docs.microsoft.com/azure/iot-edge/quickstart-linux) hızlı başlangıçlarına dağıtma ' daki yönergeleri izleyebilirsiniz.
