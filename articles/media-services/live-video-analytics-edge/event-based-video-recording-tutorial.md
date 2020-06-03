---
title: Bulut öğreticiden buluta ve kayıttan yürütmeye yönelik olay tabanlı video kaydı-Azure
description: Bu öğreticide, bulutta bulut ve kayıttan yürütmeye yönelik olay tabanlı bir video kaydı gerçekleştirmek için IoT Edge canlı video analizinin nasıl kullanılacağını öğreneceksiniz.
ms.topic: tutorial
ms.date: 05/27/2020
ms.openlocfilehash: 92367634a2f5785ecbb102db1e03f3d5f12d744e
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84300850"
---
# <a name="tutorial-event-based-video-recording-to-cloud-and-playback-from-cloud"></a>Öğretici: bulutta buluta ve kayıttan yürütmeye yönelik olay tabanlı video kaydı

Bu öğreticide canlı video analizinin IoT Edge bulutta Media Services üzere seçmeli olarak kaydetmek için nasıl kullanılacağını öğreneceksiniz. Bu kullanım örneği, bu öğreticide [olay tabanlı video kaydı](event-based-video-recording-concept.md) (EVR) olarak adlandırılır. Bunu gerçekleştirmek için, videodaki nesneleri aramak ve video kliplerini yalnızca belirli bir nesne türü algılandığında kaydetmek için bir nesne algılama AI modeli kullanacaksınız. Ayrıca, Media Services kullanarak kayıtlı video kliplerini kayıttan yürütmeyi de öğreneceksiniz. Bu, ilgi çekici bir video klip Arşivi tutan çeşitli senaryolar için yararlıdır.

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
* [Öğretici: IoT Edge modülünü geliştirme](https://docs.microsoft.com/azure/iot-edge/tutorial-develop-for-linux)
* [Dağıtım. *. Template. JSON nasıl düzenlenir](https://github.com/microsoft/vscode-azure-iot-edge/wiki/How-to-edit-deployment.*.template.json)
* [IoT Edge dağıtım bildiriminde yolların nasıl bildirilemeyeceğini gösteren](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) bölüm

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticinin Önkoşulları aşağıdaki gibidir

* [Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) uzantısı ve [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) uzantısı ile geliştirme makinenizde [Visual Studio Code](https://code.visualstudio.com/) .

    > [!TIP]
    > Docker 'ı yüklemek isteyip istemediğiniz sorulabilir. Bu istemi yoksayabilirsiniz.
* Geliştirme makinenizde [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/thank-you/sdk-3.1.201-windows-x64-installer) .
* [Canlı video analizi kaynakları kurulum betiğini](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) doldurun ve [ortamı ayarlayın](https://review.docs.microsoft.com/en-us/azure/media-services/live-video-analytics-edge/detect-motion-emit-events-quickstart?branch=release-preview-media-services-lva#set-up-the-environment)

Yukarıdaki adımların sonunda aşağıdakiler dahil olmak üzere Azure aboneliğine dağıtılan belirli Azure kaynakları olacaktır:

* IoT Hub
* Depolama hesabı
* Azure Media Services hesabı
* Azure 'da [IoT Edge Runtime](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) yüklüyken Linux sanal makinesi

## <a name="concepts"></a>Kavramlar

Olay tabanlı video kaydı (EVR), bir olay tarafından tetiklenen videoyu kaydetme sürecini ifade eder. Bu olay, video sinyalinin içinden (örneğin, videoda hareketli bir nesneyi algılayarak) veya bağımsız bir kaynaktan (örneğin, bir kapısının açılması) oluşturulabilir. Alternatif olarak, yalnızca bir ınpoger hizmeti belirli bir olayın oluştuğunu algıladığında kayıt tetikleyebilirsiniz.  Bu öğreticide, bir otobana hareket eden bir eğitim videosu kullanacaksınız ve her kamyon algılandığında video kliplerini kaydedecaksınız.

![Medya grafiği](./media/event-based-video-recording-tutorial/overview.png)

Yukarıdaki diyagramda bir [medya grafiğinin](media-graph-concept.md) bir resim gösterimi ve istenen senaryoyu gerçekleştiren ek modüller bulunur. Dahil dört IoT Edge modül vardır:

* IoT Edge modülünde canlı video analizi.
* Bir HTTP uç noktasının arkasında bir AI modeli çalıştıran bir Edge modülü. Bu AI modülü, birçok nesne türünü tespit eden [YOLOv3](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx) modelini kullanır.
* Bu öğreticide derleyip dağıtacağınızı nesneleri saymak ve filtrelemek için özel bir modül (Yukarıdaki diyagramda nesne sayacı olarak adlandırılır).
* Bir RTSP kamerasının benzetimini yapmak için bir [RTSP simülatör modülü](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) .
    
    Diyagramda gösterildiği gibi, sanal grafiklerde bir [RTSP kaynak](media-graph-concept.md#rtsp-source) düğümü kullanarak sanal bir canlı videoyu (bir otoyol üzerinde trafik) yakalayabilir ve bu videoyu iki yola gönderebilirsiniz.

* İlk yol, video çerçevelerini belirtilen (azaltılmış) kare hızında çıkaran bir [kare hızı filtre işlemcisi](media-graph-concept.md#frame-rate-filter-processor) düğümüdür. Bu video çerçeveleri bir HTTP uzantısı düğümüne gönderilir ve bu da çerçeveleri (görüntü olarak) AI modülüne (bir nesne algılayıcısı olan YOLO v3 –) geçirir ve sonuçları alır. Bu, model tarafından algılanan nesneler (yani, trafikte araçlar) olacaktır. HTTP uzantısı düğümü daha sonra IoT Hub ileti havuzu düğümü aracılığıyla sonuçları IoT Edge hub 'ına yayınlar.
* Nesne sayaç modülü, nesne algılama sonuçlarını (trafikte araçlar) içeren IoT Edge hub 'ından ileti alacak şekilde ayarlanır. Belirli bir türdeki nesneleri arayan bu iletileri denetler (bir ayar ile yapılandırılır). Böyle bir nesne bulunduğunda bu modül IoT Edge hub 'ına bir ileti gönderir. Bu "nesne bulundu" iletileri daha sonra medya grafiğinin IoT Hub kaynak düğümüne yönlendirilir. Böyle bir ileti alındığında, medya grafiğindeki IoT Hub kaynak düğümü, [sinyal kapısı işlemci](media-graph-concept.md#signal-gate-processor) düğümünü tetikleyip, ikincisinin yapılandırılmış bir süre için açılmasını sağlar. Video, o süre için varlık havuzu düğümüne ağ geçidi üzerinden akar. Canlı akışın bu bölümü daha sonra [varlık havuzu](media-graph-concept.md#asset-sink) düğümü aracılığıyla Azure Media Service hesabınızdaki bir [varlığa](terminology.md#asset) kaydedilir.

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Başlamadan önce, [önkoşullardan](#prerequisites)3. madde işaretini tamamladığınızdan emin olun. Kaynak kurulum betiği bittikten sonra, klasör yapısını göstermek için küme ayraçları üzerine tıklayın. ~/CloudDrive/LVA-Sample dizininde oluşturulmuş birkaç dosya görürsünüz.

![Uygulama ayarları](./media/quickstarts/clouddrive.png)

Bu öğreticide ilgilendiğiniz:

* ~/CloudDrive/LVA-Sample/Edge-Deployment/.exe-Visual Studio Code bir uç cihaza modül dağıtmak için kullandığı özellikleri içerir.
* ~/CloudDrive/LVA-Sample/AppSetting.exe-örnek kodu çalıştırmak için Visual Studio Code tarafından kullanılır.

Aşağıdaki adımlarda bu dosyalara ihtiyacınız olacak.

1. Depoyu buradan kopyalayın https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp .
1. Visual Studio Code başlatın ve depoyu indirdiğiniz klasörü açın.
1. Visual Studio Code ' de, "src/buluttan cihaza-Console-App" klasörüne gidin ve "appSettings. JSON" adlı bir dosya oluşturun. Bu dosya, programı çalıştırmak için gereken ayarları içerir.
1. İçeriği ~/CloudDrive/LVA-Sample/appSettings.json dosyasından kopyalayın. Metin şöyle görünmelidir:

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```

    IoT Hub bağlantı dizesi, Azure IoT Hub aracılığıyla Edge modüllerine komut göndermek için Visual Studio Code kullanmanıza olanak sağlar.
    
1. Sonra, "src/Edge" klasörüne gidin ve ". env" adlı bir dosya oluşturun.
1. İçeriği ~/CloudDrive/LVA-Sample/-env dosyasından kopyalayın. Metin şöyle görünmelidir:

    ```
    SUBSCRIPTION_ID="<Subscription ID>"  
    RESOURCE_GROUP="<Resource Group>"  
    AMS_ACCOUNT="<AMS Account ID>"  
    IOTHUB_CONNECTION_STRING="HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx"  
    AAD_TENANT_ID="<AAD Tenant ID>"  
    AAD_SERVICE_PRINCIPAL_ID="<AAD SERVICE_PRINCIPAL ID>"  
    AAD_SERVICE_PRINCIPAL_SECRET="<AAD SERVICE_PRINCIPAL ID>"  
    INPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"  
    OUTPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/output"  
    APPDATA_FOLDER_ON_DEVICE="/var/local/mediaservices"
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry username>"      
    ```

## <a name="examine-the-template-file"></a>Şablon dosyasını inceleyin 

Önceki adımda Visual Studio Code başlattınız ve örnek kodu içeren klasörü açcaksınız.

Visual Studio Code, "src/Edge" e gidin. Oluşturduğunuz. env dosyasının yanı sıra birkaç dağıtım şablonu dosyası görürsünüz. Bu şablon, sınır cihazına (Azure Linux VM) hangi Edge modüllerinin dağıtım olacağını tanımlar. . Env dosyası, medya hizmeti kimlik bilgileri gibi bu şablonlarda kullanılan değişkenlerin değerlerini içerir.

"Src/Edge/Deployment. objectCounter. Template. JSON" öğesini açın. "Modüller" bölümünde listelenen öğelere (kavramlar bölümünde) karşılık gelen dört giriş olduğunu unutmayın:

* lvaEdge: IoT Edge modülünde canlı video analizi
* yolov3 – bu, YOLO v3 modeli kullanılarak oluşturulan AI modülüdür
* rtspsim: Bu RTSP simülatör
* objectCounter: yolov3 'deki sonuçlarda belirli nesneler için görünen modüldür

ObjectCounter modülü için, "görüntü" değeri için kullanılan dizeye ($ {MODULES. objectCounter}) bakın; bu, IoT Edge bir modül geliştirmeye yönelik [öğreticiye](https://docs.microsoft.com/azure/iot-edge/tutorial-develop-for-linux) dayanır. Visual Studio Code, nesne sayaç modülü için kodun "src/Edge/modules/objectCounter" altında olduğunu otomatik olarak tanır. 

IoT Edge dağıtım bildiriminde yolları bildirme hakkında [Bu](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) bölümü okuyun ve ardından şablon JSON dosyasındaki yolları inceleyin. Nasıl yapılacağını aklınızda edin:

* LVAToObjectCounter, belirli olayları objectCounter modülündeki belirli bir uç noktaya göndermek için kullanılır.
* ObjectCounterToLVA, lvaEdge modülünde belirli bir uç noktaya (IoT Hub kaynak düğümü olması gerekir) bir tetikleyici olayı göndermek için kullanılır.
* objectCounterToIoTHub, bu öğreticiyi çalıştırdığınızda objectCounter 'dan çıktıyı görmenizi sağlayacak bir hata ayıklama aracı olarak kullanılır.

> [!NOTE]
> "Kamyon" olarak etiketlenen nesneleri en az %50 olan bir güvenilirlik düzeyi ile aramak üzere ayarlanan objectCounter modülünün istenen özelliklerini denetleyin.

## <a name="generate-and-deploy-the-iot-edge-deployment-manifest"></a>IoT Edge dağıtım bildirimini oluşturma ve dağıtma 

Dağıtım bildirimi, bir sınır cihazına hangi modüllerin dağıtıldığını ve bu modüllerin yapılandırma ayarlarını tanımlar. Şablon dosyasından böyle bir bildirim oluşturmak için bu adımları izleyin ve ardından bunu Edge cihazına dağıtın.

Visual Studio Code kullanarak, Docker 'da oturum açmak ve "çözümü derlemek ve IoT Edge göndermek" için [Bu](https://docs.microsoft.com/azure/iot-edge/tutorial-develop-for-linux#build-and-push-your-solution) yönergeleri izleyin, ancak bu adım için src/Edge/Deployment. objectcounter. Template. JSON kullanın.

![IoT Edge çözümü oluşturun ve gönderin](./media/event-based-video-recording-tutorial/build-push.png)

Bu, nesne sayma için objectCounter modülünü oluşturur ve görüntüyü Azure Container Registry (ACR) ' e gönderir.

* Ortam değişkenlerinin. env dosyasında tanımlı CONTAINER_REGISTRY_USERNAME_myacr ve CONTAINER_REGISTRY_PASSWORD_myacr sahip olup olmadığını denetleyin.

Yukarıdaki adım src/Edge/config/Deployment. objectCounter. AMD64. JSON konumunda IoT Edge dağıtım bildirimini oluşturacaktır. Bu dosyaya sağ tıklayın ve "tek cihaz için dağıtım oluştur" düğmesine tıklayın.

![Tek bir cihaz için dağıtım oluşturma](./media/quickstarts/create-deployment-single-device.png)

IoT Edge, canlı video analiziyle ilgili ilk öğreticeniz varsa, Visual Studio Code ıothub bağlantı dizesini girmek isteyip isteirsiniz. Bunu appSettings. json dosyasından kopyalayabilirsiniz.

Ardından Visual Studio Code bir IoT Hub cihazı seçmenizi ister. IoT Edge cihazınızı seçin ("LVA-örnek-cihaz" olmalıdır).

Bu aşamada, Edge modüllerinin IoT Edge cihazınıza dağıtılması başladı.
Yaklaşık 30 saniye içinde, Visual Studio Code sol alt bölümünde Azure IoT Hub yenileyin ve dağıtılan 4 modül olduğunu görmeniz gerekir (adları yeniden unutmayın: lvaEdge, rtspsim, yolov3 ve objectCounter).

![4 modül dağıtıldı](./media/event-based-video-recording-tutorial/iot-hub.png)

## <a name="prepare-for-monitoring-events"></a>İzleme olaylarını hazırlama

Nesne sayaç modülünün ve IoT Edge modülündeki canlı video analizinden olayları görmek için şu adımları izleyin:

1. Visual Studio Code Gezgin bölmesini açın ve sol alt köşedeki Azure IoT Hub arayın.
1. Cihazlar düğümünü genişletin.
1. LVA-örnek-cihazında sağ-Clink ve **yerleşik olay Izlemeyi Izlemeye başla**seçeneğini seçti.

![Yerleşik olay uç noktasını izlemeye başla](./media/quickstarts/start-monitoring-iothub-events.png)

## <a name="run-the-program"></a>Programı çalıştırma

1. Visual Studio Code, "src/Cloud-to-Device-Console-App/Operations. JSON" öğesine gidin

1. GraphTopologySet düğümü altında aşağıdakileri düzenleyin:

    `"topologyUrl" : "https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/evr-hubMessage-assets/topology.json"`
    
1. Ardından, Graphınstanceset ve Graphtopologyıdelete, Düzenle, düğümleri altında

    `"topologyName" : "EVRtoAssetsOnObjDetect"`
1. Bir hata ayıklama oturumu başlatın (F5 tuşuna basın). TERMINAL penceresinde yazdırılmış bazı iletileri görmeye başlayacaksınız.

1. Operations. JSON, Graphtopologyılist ve Graphınstancelist çağrıları ile başlatılır. Önceki hızlı başlangıçlardan veya öğreticilerden sonra kaynakları temizlediyseniz, bu, boş listeler döndürür ve ardından ENTER tuşuna, aşağıdaki gibi bir giriş için duraklatırsınız:

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
    1. TERMINAL penceresinde "Enter" tuşuna bastığınızda, bir sonraki doğrudan yöntem çağrısı kümesi yapılır.
     * Yukarıdaki Topologyıurl 'yi kullanarak Graphtopologyıset öğesine çağrı.
     * Aşağıdaki gövdeyi kullanarak Graphınstanceset öğesine çağrı.
     
        ```
        {
          "@apiVersion": "1.0",
          "name": "Sample-Graph-1",
          "properties": {
            "topologyName": "EVRtoAssetsOnObjDetect",
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
     
1. TERMINAL penceresindeki çıktı, şimdi ' devam etmek için ENTER tuşuna basın ' isteminde duraklatılır. Şu an "Enter" tuşuna basmayın. Daha sonra, çağrdığınız doğrudan yöntemler için JSON yanıtı yüklerini görmek üzere yukarı doğru gezinebilirsiniz.

1. Artık Visual Studio Code çıkış penceresine geçiş yaparsanız, IoT Edge modülündeki canlı video analizi tarafından IoT Hub gönderilen iletileri görürsünüz.

     * Bu iletiler aşağıdaki bölümde ele alınmıştır.
     
1. Grafik örneği çalışmaya devam eder ve videoyu kaydeder; RTSP simülatörü kaynak videoyu döngüye sokacaktır. Aşağıdaki bölümde açıklandığı gibi iletileri gözden geçirin ve ardından örneği durdurmak için, TERMINAL penceresine dönün ve "Enter" tuşuna basın. Kaynakları temizlemek için sonraki çağrı dizisi yapılır:

     * Grafik örneğini devre dışı bırakmak için Graphınstancedeactivate öğesine çağrı
     * Örneği silmek için Graphınstancedelete öğesine çağrı
     * Topolojiyi silmek için Graphtopologyıdelete çağrısı
     * Listenin artık boş olduğunu göstermek için Graphtopologyılist öğesine yapılan son çağrı

## <a name="interpret-the-results"></a>Sonuçları yorumlama 

Medya grafiğini çalıştırdığınızda IoT Edge modülündeki canlı video analizi, IoT Edge hub 'ına belirli tanılama ve çalışma olayları gönderir. Bu olaylar, "gövde" bölümünü ve "applicationProperties" bölümünü içeren Visual Studio Code çıkış penceresinde gördüğünüz iletilerdir. Bu bölümlerin neyi temsil ettiğini anlamak için bkz. [IoT Hub Iletileri oluşturma ve okuma](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct).

Aşağıdaki iletilerde, uygulama özellikleri ve gövdenin içeriği canlı video analizi modülü tarafından tanımlanır.

## <a name="diagnostic-events"></a>Tanılama olayları

### <a name="mediasessionestablished-event"></a>Mediasessionkurulduğu olayı 

Bir medya grafiği oluşturulduğunda, RTSP kaynak düğümü RTSP simülatör kapsayıcısı üzerinde çalışan RTSP sunucusuna bağlanmaya çalışır. Başarılı olursa, bu olayı yazdıracaktır. Olay türünün Microsoft. Media. MediaGraph. Diagnostics. Mediasessionkurdu olduğunu unutmayın.

```
[IoTHubMonitor] [5:53:17 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 XXX.XX.XX.XX\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets=XXXXXXXXXXXXXXXXXXXXXX\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sources/rtspSource",
    "eventType": "Microsoft.Media.Graph.Diagnostics.MediaSessionEstablished",
    "eventTime": "2020-05-17T17:53:16.981Z",
    "dataVersion": "1.0"
  }
}
```


* İleti bir tanılama olayıdır, Mediasessionkurulan, RTSP kaynak düğümünün (konu) RTSP simülatörü ile bağlantı kurabildiğini ve (sanal) canlı bir akış almaya başlayabileceğini gösterir.

* ApplicationProperties 'teki "Subject" değeri, iletinin oluşturulduğu grafik topolojisinde bulunan düğüme başvurur. Bu durumda, ileti RTSP kaynak düğümünden gönderilir.

* applicationProperties 'teki "eventType" bir tanılama olayı olduğunu gösterir.

* "eventTime" olayın gerçekleştiği saati gösterir. Bu, trafik videosu (MKV dosyası), bir canlı akış olarak modüle ulaşmak için başlatılır.

* "gövde", bu durumda [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol) ayrıntılarından oluşan tanılama olayı hakkındaki verileri içerir.


## <a name="operational-events"></a>İşletimsel olaylar

Medya grafiği bir süre çalıştıktan sonra, sonuç olarak nesne sayacı modülünden bir olay alırsınız. 

```
[IoTHubMonitor] [5:53:44 PM] Message received from [lva-sample-device/objectCounter]:
{
  "body": {
    "count": 2
  },
  "applicationProperties": {
    "eventTime": "2020-05-17T17:53:44.062Z"
  }
}
```

ApplicationProperties, nesne sayaç modülünün, YOLO v3 modülünün sonuçlarının ilgilenme nesneleri (structuralks) içerdiğini gözlemlediği zaman olan eventTime değerini içerir.

Videoda diğer structuralks algılandığında bu olaylardan daha fazla bilgi görebilirsiniz.

### <a name="recordingstarted-event"></a>RecordingStarted olayı

Nesne sayacı olayı gönderdikten hemen hemen sonra, Microsoft. Media. Graph. Işletimsel. RecordingStarted türünde bir olay görürsünüz

```
[IoTHubMonitor] [5:53:46 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAssetFromEVR-LVAEdge-20200517T175346Z"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStarted",
    "eventTime": " 2020-05-17T17:53:46.132Z",
    "dataVersion": "1.0"
  }
}
```

ApplicationProperties 'teki "Subject", bu iletiyi oluşturan grafikteki varlık havuzu düğümüne başvurur. Gövde, çıkış konumuyla ilgili bilgiler içerir, bu durumda Videonun kaydedildiği Azure Media Service varlığının adıdır. Bu değeri görmeniz gerekir.

### <a name="recordingavailable-event"></a>RecordingAvailable olayı

Varlık havuzu düğümü, varlığa video yüklediğini, Microsoft. Media. Graph. Işletimsel. RecordingAvailable türünde bu olayı yayar

```
[IoTHubMonitor] [5:54:15 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAssetFromEVR-LVAEdge-20200517T175346Z"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingAvailable",
    "eventTime": "2020-05-17T17:54:15.808Z",
    "dataVersion": "1.0"
  }
}
```

Bu olay, oyuncuların/istemcilerin videonun kayıttan yürütmesini başlatması için varlığa yeterli miktarda veri yazıldığını gösterir. ApplicationProperties 'teki "Subject" değeri, bu iletiyi oluşturan grafikteki AssetSink düğümüne başvurur. Gövde, çıkış konumuyla ilgili bilgiler içerir, bu durumda Videonun kaydedildiği Azure Media Service varlığının adıdır.

### <a name="recordingstopped-event"></a>Recordingdurdurulan olay

[Topolojide](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/evr-hubMessage-assets/topology.json)sinyal kapısı işlemci düğümü için etkinleştirme ayarlarını (maximumActivationTime) incelerseniz, bu ağ geçidinin, üzerinden 30 saniyelik video gönderildikten sonra kapatılacak şekilde olduğunu görürsünüz. Bu nedenle, RecordingStarted olayından yaklaşık 30 saniye sonra, varlık havuzu düğümünün varlık için video kaydını durdurduğunu belirten Microsoft. Media. Graph. Operational. Recordındurdurultüründe bir olay görmeniz gerekir.

```
[IoTHubMonitor] [5:54:15 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAssetFromEVR-LVAEdge-20200517T175346Z"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStopped",
    "eventTime": "2020-05-17T17:54:15.040Z",
    "dataVersion": "1.0"
  }
}
```

Bu olay, kaydın durdurulduğunu gösterir. ApplicationProperties 'teki "Subject" değeri, bu iletiyi oluşturan grafikteki AssetSink düğümüne başvurur. Gövde, çıkış konumuyla ilgili bilgiler içerir, bu durumda Videonun kaydedildiği Azure Media Service varlığının adıdır.

## <a name="media-services-asset"></a>Media Services varlık  

Azure portal oturum açarak ve videoyu görüntüleyerek grafik tarafından oluşturulan Media Services varlığını inceleyebilirsiniz.

1. Web tarayıcınızı açın ve [Azure Portal](https://portal.azure.com/)gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.
1. Aboneliğinizdeki kaynaklar arasında Media Services hesabınızı bulun ve hesap dikey penceresini açın
1. Media Services listesinde varlıklar ' a tıklayın.

    ![Varlıklar](./media/continuous-video-recording-tutorial/assets.png)
1. SampleAssetFromEVR-LVAEdge-{DateTime} adıyla listelenmiş bir varlık bulacaksınız. Bu, RecordingStarted olayının outputLocation özelliğinde verilen addır. Topolojideki assetNamePattern, bu adın nasıl oluşturulduğunu belirler.
1. Varlığa tıklayın.
1. Varlık ayrıntıları sayfasında, akış URL 'SI altında **Yeni oluştur** metin kutusunu tıklatın.

    ![Yeni varlık](./media/continuous-video-recording-tutorial/new-asset.png)

1. Açılan sihirbazda, varsayılan seçenekleri kabul edin ve "Ekle" düğmesine basın. Daha fazla bilgi için bkz. [video kayıttan yürütme](video-playback-concept.md).

    > [!TIP]
    > [Akış uç noktanızın çalıştığından](../latest/streaming-endpoint-concept.md)emin olun.
1. Player Videoyu yüklemesi gerekir ve bunu görüntülemek için **Play** 'e ulaşabileceksiniz.

> [!NOTE]
> Videonun kaynağı bir kamera akışını taklit eden bir kapsayıcı olduğundan, videodaki zaman damgaları grafik örneğini etkinleştirdiğinizde ve devre dışı bırakıldığında ile ilgilidir. [Kayıttan yürütme çoklu gün kayıtları](playback-multi-day-recordings-tutorial.md) öğreticisinde yerleşik olarak bulunan kayıttan yürütme denetimlerini kullanıyorsanız, ekranda görüntülenirken görünen zaman damgalarını görebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer öğreticileri denemek istiyorsanız, oluşturulan kaynaklarda tutmanız gerekir. Aksi takdirde, Azure portal gidin, Kaynak gruplarınızı inceleyin, bu öğreticiyi çalıştırdığınız kaynak grubunu seçin ve kaynak grubunu silin.

## <a name="next-steps"></a>Sonraki adımlar

* RTSP simülatörü kullanmak yerine RTSP desteğiyle bir [IP kamerası](https://en.wikipedia.org/wiki/IP_camera) kullanın. Profiller G, S veya T ile uyumlu cihazlar ' ı arayarak [ONVIF uyumlu ürünler sayfasında](https://www.onvif.org/conformant-products/) , RTSP desteğiyle IP kameralarını arayabilirsiniz.
* AMD64 veya x64 Linux cihazı kullanın (Azure Linux VM kullanarak). Bu cihaz, IP kamerası ile aynı ağda olmalıdır. [Linux 'ta Azure IoT Edge çalışma zamanını Install](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) ' daki yönergeleri izleyebilir ve sonra Azure IoT Hub 'e kaydetmek için [ilk IoT Edge modülünüzü bir sanal Linux cihaz](https://docs.microsoft.com/azure/iot-edge/quickstart-linux) hızlı başlangıçlarına dağıtma ' daki yönergeleri izleyebilirsiniz.
