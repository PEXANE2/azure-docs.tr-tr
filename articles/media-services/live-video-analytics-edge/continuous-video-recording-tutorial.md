---
title: Bulut öğreticiden bulut ve kayıttan yürütmeye yönelik sürekli video kaydı-Azure
description: Bu öğreticide, canlı video analizinin IoT Edge, buluta sürekli olarak video kaydetmek ve Azure Media Services kullanarak videonun herhangi bir bölümünü akışa almak için nasıl kullanılacağını öğreneceksiniz.
ms.topic: tutorial
ms.date: 05/27/2020
ms.openlocfilehash: a69d3f5db9dd8cbe25bbf79f44921f26258005cc
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84261861"
---
# <a name="tutorial-continuous-video-recording-to-cloud-and-playback-from-cloud"></a>Öğretici: buluta yönelik sürekli video kaydı ve buluttan kayıttan yürütme  

Bu öğreticide, bulutta [sürekli video kaydı](continuous-video-recording-concept.md) (CVR) gerçekleştirmek ve Media Services kullanarak bu videonun herhangi bir bölümünü akışa almak için IoT Edge 'Da canlı video analizinin nasıl kullanılacağını öğreneceksiniz. Bu, güvenlik, uyumluluk ve diğerleri gibi senaryolar için, bir kameradan birden çok gün (veya hafta) için bir film arşivi bulundurmasının gerekli olduğu durumlarda faydalıdır.

> [!div class="checklist"]
> * İlgili kaynakları ayarlayın
> * CVR 'yi gerçekleştiren kodu inceleyin
> * Örnek kodu çalıştırma
> * Sonuçları inceleyin ve videoyu görüntüleyin

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="suggested-pre-reading"></a>Önerilen önceden okuma  

Aşağıdaki belge sayfalarını okumanız önerilir

* [IoT Edge genel bakış üzerinde canlı video analizi](overview.md)
* [IoT Edge terminolojisinde canlı video analizi](terminology.md)
* [Medya grafiği kavramları](media-graph-concept.md) 
* [Sürekli video kayıt senaryoları](continuous-video-recording-concept.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticinin Önkoşulları aşağıdaki gibidir

* [Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) uzantısı ve [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) uzantısı ile geliştirme makinenizde [Visual Studio Code](https://code.visualstudio.com/) .

    > [!TIP]
    > Docker 'ı yüklemek isteyip istemediğiniz sorulabilir. Bu istemi yoksayabilirsiniz.
* Geliştirme makinenizde [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/thank-you/sdk-3.1.201-windows-x64-installer) .
* [Canlı video analizi kaynakları kurulum betiğini](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) doldurun

Yukarıdaki adımların sonunda aşağıdakiler dahil olmak üzere Azure aboneliğine dağıtılan belirli Azure kaynakları olacaktır:

* IoT Hub
* Depolama hesabı
* Azure Media Services hesabı
* Azure 'da [IoT Edge Runtime](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) yüklüyken Linux sanal makinesi

## <a name="concepts"></a>Kavramlar

[Burada](media-graph-concept.md)açıklandığı gibi, bir medya grafiği medyanın nerede yakalanabileceğini, nasıl işleneceğini ve sonuçların nereye teslim edileceğini tanımlamanızı sağlar. CVR 'yi gerçekleştirmek için, bir RTSP özellikli kameradan videoyu yakalamanız ve [Azure Media Services bir varlık](terminology.md#asset)için sürekli olarak kaydetmeniz gerekir. Aşağıdaki diyagramda, bu medya grafiğinin grafik temsili gösterilmektedir.

![Medya grafiği](./media/continuous-video-recording-tutorial/continuous-video-recording-overview.png)

Bu öğreticide, bir RTSP kamerasının benzetimini yapmak için [Live555 medya sunucusu](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) kullanılarak oluşturulan bir Edge modülü kullanacaksınız. Medya grafiğinde, canlı akışı almak için bir [RTSP kaynak](media-graph-concept.md#rtsp-source) düğümü kullanır ve bu videoyu, videoyu bir varlığa kaydedecek [varlık havuzu düğümüne](media-graph-concept.md#asset-sink) gönderebilirsiniz.

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Başlamadan önce, [önkoşullardan](#prerequisites)3. madde işaretini tamamladığınızdan emin olun. Kaynak kurulum betiği bittikten sonra, klasör yapısını göstermek için küme ayraçları üzerine tıklayın. ~/CloudDrive/LVA-Sample dizininde oluşturulmuş birkaç dosya görürsünüz.

![Uygulama ayarları](./media/quickstarts/clouddrive.png)

Bu öğreticide ilgilendiğiniz:

     * ~/clouddrive/lva-sample/edge-deployment/.env  - contains properties that Visual Studio Code uses to deploy modules to an edge device
     * ~/clouddrive/lva-sample/appsettings.json - used by Visual Studio Code for running the sample code

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

## <a name="examine-the-sample-files"></a>Örnek dosyaları inceleyin

Visual Studio Code "src/Edge/Deployment. Template. JSON" öğesini açın. Bu şablon, sınır cihazına (Azure Linux VM) hangi Edge modüllerinin dağıtım olacağını tanımlar. "Modüller" bölümünde aşağıdaki adlarla iki giriş olduğunu unutmayın:

* lvaEdge: IoT Edge modülünde canlı video analizi
* rtspsim: Bu RTSP simülatör

Sonra, "src/buluttan-cihaza-Console-App" klasörüne gidin. Burada, oluşturduğunuz appSettings. json dosyasını ve diğer birkaç dosyayı görürsünüz:

* C2D-Console-App. csproj-Visual Studio Code için proje dosyası.
* Operations. JSON-bu dosya, çalıştırdığınız farklı işlemleri listeler
* Program.cs-aşağıdakileri belirleyen örnek program kodu:
    * Uygulama ayarlarını yükler
    * IoT Edge modülünde canlı video analizi tarafından kullanıma sunulan doğrudan yöntemleri çağırır. [Doğrudan yöntemlerini](direct-methods.md) çağırarak canlı video akışlarını çözümlemek için modülünü kullanabilirsiniz
    * TERMINAL penceresinde programın çıkışını ve çıkış penceresinde modül tarafından oluşturulan olayları incelemek için duraklamalar
    * Kaynakları temizlemek için doğrudan yöntemleri çağırır

## <a name="generate-and-deploy-the-iot-edge-deployment-manifest"></a>IoT Edge dağıtım bildirimini oluşturma ve dağıtma 

Dağıtım bildirimi, bir sınır cihazına hangi modüllerin dağıtıldığını ve bu modüllerin yapılandırma ayarlarını tanımlar. Şablon dosyasından böyle bir bildirim oluşturmak için bu adımları izleyin ve ardından bunu Edge cihazına dağıtın.

1. Visual Studio Code'u başlatma
1. Sol alt köşedeki AZURE ıOT HUB bölmesinin yanındaki "daha fazla eylem" simgesine tıklayarak ıothub bağlantı dizesini ayarlayın. Dizeyi src/Cloud-to-Device-Console-App/appSettings. json dosyasından kopyalayabilirsiniz. 

    ![IOT bağlantı dizesi ayarla](./media/quickstarts/set-iotconnection-string.png)
1. Sonra, "src/Edge/Deployment. Template. JSON" dosyasına sağ tıklayın ve "IoT Edge dağıtım bildirimi oluştur" seçeneğine tıklayın. Visual Studio Code, dağıtım şablonu dosyasında bulunan değişkenleri değiştirmek için. env dosyasındaki değerleri kullanır. Bunun için "Deployment. AMD64. JSON" adlı src/Edge/config klasöründe bir bildirim dosyası oluşturulmalıdır.

   ![IoT Edge dağıtım bildirimi oluştur](./media/quickstarts/generate-iot-edge-deployment-manifest.png)
1. "Src/Edge/config/Deployment. AMD64. JSON" öğesine sağ tıklayın ve "tek cihaz için dağıtım oluştur" seçeneğine tıklayın.

   ![Tek bir cihaz için dağıtım oluşturma](./media/quickstarts/create-deployment-single-device.png)
1. Bundan sonra "bir IoT Hub cihaz seçmeniz" istenir. Açılan listeden LVA-örnek-cihaz ' ı seçin.
1. Yaklaşık 30 saniye içinde, sol alt kısımdaki Azure IoT Hub yenileyin ve Edge cihazında aşağıdaki modüllerin dağıtıldığını görmeniz gerekir:
    * IoT Edge (modül adı "lvaEdge") üzerinde canlı video analizi
    * RTSP simülatör (modül adı "rtspsim")
 
    ![IoT Hub 'ı](./media/continuous-video-recording-tutorial/iot-hub.png)

## <a name="prepare-to-monitor-the-modules"></a>Modülleri izlemeye hazırlanma 

Canlı video akışını kaydetmek için IoT Edge modülde canlı video analizi kullandığınızda, bu işlem, IoT Hub olayları gönderir. Bu olayları görmek için şu adımları izleyin:

1. Visual Studio Code Gezgin bölmesini açın ve sol alt köşedeki Azure IoT Hub arayın.
1. Cihazlar düğümünü genişletin.
1. LVA-Sample-cihaza sağ tıklayın ve "yerleşik olay Izlemeyi Izlemeye başla" seçeneğini seçin.

    ![Yerleşik olay uç noktasını izlemeye başla](./media/quickstarts/start-monitoring-iothub-events.png)

## <a name="run-the-program"></a>Programı çalıştırma 

1. Visual Studio Code, "src/Cloud-to-Device-Console-App/Operations. JSON" öğesine gidin
1. GraphTopologySet düğümü altında aşağıdakileri düzenleyin:

    `"topologyUrl" : "https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/cvr-asset/topology.json" `
1. Ardından, Graphınstanceset ve GraphTopologyDelete düğümleri altında, topologyName değerinin yukarıdaki grafik topolojisinde "Name" özelliğinin değeriyle eşleştiğinden emin olun:

    `"topologyName" : "CVRToAMSAsset"`  
1. [Topolojiyi](https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/cvr-asset/topology.json) bir tarayıcıda açın ve assetNamePattern bölümüne bakın. Benzersiz bir ada sahip bir varlığınızın olduğundan emin olmak için, Operations. JSON dosyasındaki grafik örneği adını değiştirmek isteyebilirsiniz ("Sample-Graph-1" varsayılan değerinden).

    `"assetNamePattern": "sampleAsset-${System.GraphTopologyName}-${System.GraphInstanceName}"`    
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
1. TERMINAL penceresinde "Enter" tuşuna bastığınızda, bir sonraki doğrudan yöntem çağrısı kümesi yapılır
     * Yukarıdaki Topologyıurl 'yi kullanarak Graphtopologyıset öğesine çağrı.
     * Aşağıdaki gövdeyi kullanarak Graphınstanceset öğesine çağrı.
     
     ```
     {
       "@apiVersion": "1.0",
       "name": "Sample-Graph-1",
       "properties": {
         "topologyName": "CVRToAMSAsset",
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
1. Grafik örneği çalışmaya devam edecektir ve videoyu kaydeder; RTSP simülatörü kaynak videoyu döngüye sokacaktır. Kaydı durdurmak için, TERMINAL penceresine dönün ve "Enter" tuşuna basın. Kaynakları temizlemek için sonraki çağrı dizisi yapılır:

     * Grafik örneğini devre dışı bırakmak için Graphınstancedeactivate öğesine çağrı
     * Örneği silmek için Graphınstancedelete öğesine çağrı
     * Topolojiyi silmek için Graphtopologyıdelete çağrısı
     * Listenin artık boş olduğunu göstermek için Graphtopologyılist öğesine yapılan son çağrı

## <a name="interpret-the-results"></a>Sonuçları yorumlama 

Medya grafiğini çalıştırdığınızda IoT Edge modülündeki canlı video analizi, IoT Edge hub 'ına belirli tanılama ve çalışma olayları gönderir. Bu olaylar, "gövde" bölümünü ve "applicationProperties" bölümünü içeren Visual Studio Code çıkış penceresinde gördüğünüz iletilerdir. Bu bölümlerin neyi temsil ettiğini anlamak için [Bu](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct) makaleyi okuyun.

Aşağıdaki iletilerde, uygulama özellikleri ve gövdenin içeriği canlı video analizi modülü tarafından tanımlanır.

## <a name="diagnostic-events"></a>Tanılama olayları 

### <a name="mediasession-established-event"></a>MediaSession tarafından belirlenen olay

Grafik örneği etkinleştirildiğinde, RTSP kaynak düğümü rtspsım modülünde çalışan RTSP sunucusuna bağlanmaya çalışır. Başarılı olursa, bu olayı yazdıracaktır:

```
[IoTHubMonitor] [9:42:18 AM] Message received from [lvaedgesample/lvaEdge]:
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 XXX.XX.XX.XX\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets=XXXXXXXXXXXXXXXXXXXXXX\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "dataVersion": "1.0",
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",
    "subject": "/graphInstances/Sample-Graph-1/sources/rtspSource",
    "eventType": "Microsoft.Media.MediaGraph.Diagnostics.MediaSessionEstablished",
    "eventTime": "2020-04-09T09:42:18.1280000Z"
  }
}
```

* İleti bir tanılama olayıdır, Mediasessionkurulan, RTSP kaynak düğümünün (konu) RTSP simülatörü ile bağlantı kurabildiğini ve (sanal) canlı bir akış almaya başlayabileceğini gösterir.
* ApplicationProperties 'teki "Subject" değeri, iletinin oluşturulduğu grafik topolojisinde bulunan düğüme başvurur. Bu durumda, ileti RTSP kaynak düğümünden gönderilir.
* applicationProperties 'teki "eventType" bir tanılama olayı olduğunu gösterir.
* "eventTime" olayın gerçekleştiği saati gösterir.
* "gövde", bu durumda [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol) ayrıntılarından oluşan tanılama olayı hakkındaki verileri içerir.

## <a name="operational-events"></a>İşletimsel olaylar 

### <a name="recordingstarted-event"></a>RecordingStarted olayı

Varlık havuzu düğümü videoyu kaydetmeye başladığında, Microsoft. Media. Graph. Operational. Recordınstarted türünde bu olayı yayar

```
[IoTHubMonitor] [9:42:38 AM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAsset-CVRToAMSAsset-Sample-Graph-1"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStarted",
    "eventTime": "2020-04-09T09:42:38.1280000Z",
    "dataVersion": "1.0"
  }
}
```

ApplicationProperties 'teki "Subject", bu iletiyi oluşturan grafikteki varlık havuzu düğümüne başvurur.

Gövde, çıkış konumuyla ilgili bilgiler içerir, bu durumda Videonun kaydedildiği Azure Media Service varlığının adıdır. Bu değeri görmeniz gerekir.

### <a name="recordingavailable-event"></a>RecordingAvailable olayı

Adından da anlaşılacağı gibi, kayıt başladığında RecordingStarted olayı gönderilir; ancak video verileri henüz varlığa yüklenmemiş olabilir. Varlık havuzu düğümü, varlığa video verisi yüklemiştir, Microsoft. Media. Graph. Işletimsel. RecordingAvailable türünde bu olayı yayar

```
[IoTHubMonitor] [[9:43:38 AM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAsset-CVRToAMSAsset-Sample-Graph-1"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingAvailable",
    "eventTime": "2020-04-09T09:43:38.1280000Z",
    "dataVersion": "1.0"
  }
}
```

Bu olay, oyuncuların/istemcilerin videonun kayıttan yürütmesini başlatması için varlığa yeterli miktarda veri yazıldığını gösterir.

ApplicationProperties 'teki "Subject" değeri, bu iletiyi oluşturan grafikteki AssetSink düğümüne başvurur.

Gövde, çıkış konumuyla ilgili bilgiler içerir, bu durumda Videonun kaydedildiği Azure Media Service varlığının adıdır.

### <a name="recordingstopped-event"></a>Recordingdurdurulan olay

Grafik örneğini devre dışı bıraktıktan sonra, varlık havuzu düğümü video kaydını kıymete bırakır, Microsoft. Media. Graph. Işletimsel. Recordingdurdurulan türünde bu olayı yayar.

```
[IoTHubMonitor] [11:33:31 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAsset-CVRToAMSAsset-Sample-Graph-1"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStopped",
    "eventTime": "2020-04-10T11:33:31.051Z",
    "dataVersion": "1.0"
  }
}
```

Bu olay, kaydın durdurulduğunu gösterir.

ApplicationProperties 'teki "Subject" değeri, bu iletiyi oluşturan grafikteki AssetSink düğümüne başvurur.

Gövde, çıkış konumuyla ilgili bilgiler içerir, bu durumda Videonun kaydedildiği Azure Media Service varlığının adıdır.

## <a name="media-services-asset"></a>Media Services varlık  

Azure portal oturum açarak ve videoyu görüntüleyerek medya grafiği tarafından oluşturulan Media Services varlığını inceleyebilirsiniz.

1. Web tarayıcınızı açın ve [Azure Portal](https://portal.azure.com/)gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.
1. Aboneliğinizdeki kaynaklar arasında Media Services hesabınızı bulun ve hesap dikey penceresini açın
1. Media Services listesinde varlıklar ' a tıklayın

    ![Varlıklar](./media/continuous-video-recording-tutorial/assets.png)
1. Samplevarlık-CVRToAMSAsset-Sample-Graph-1 adında listelenen bir varlık bulacaksınız. Bu, Graf topolojisi dosyanızda seçilen adlandırma deseninin adıdır.
1. Varlığa tıklayın.
1. Varlık ayrıntıları sayfasında, akış URL 'SI altında **Yeni oluştur** metin kutusunu tıklatın.

    ![Yeni varlık](./media/continuous-video-recording-tutorial/new-asset.png)

1. Açılan sihirbazda, varsayılan seçenekleri kabul edin ve "Ekle" düğmesine basın. Daha fazla bilgi için bkz. [video kayıttan yürütme](video-playback-concept.md).

    > [!TIP]
    > [Akış uç noktanızın çalıştığından](../latest/streaming-endpoint-concept.md)emin olun.
1. Player Videoyu yüklemesi gerekir ve bunu görüntülemek Için **oynatma**> * * tuşuna basabileceksiniz.

> [!NOTE]
> Videonun kaynağı bir kamera akışını taklit eden bir kapsayıcı olduğundan, videodaki zaman damgaları grafik örneğini etkinleştirdiğinizde ve devre dışı bırakıldığında ile ilgilidir. [Bu](playback-multi-day-recordings-tutorial.md) öğreticiyi, çok günlü bir kayda gözatmaya ve bu arşivin bölümlerini görüntülemenize bakabilirsiniz. Bu öğreticide, ekranda görüntülenen videodaki zaman damgalarını de görebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer öğreticileri denemek istiyorsanız, oluşturulan kaynaklarda tutmanız gerekir. Aksi takdirde, Azure portal gidin, Kaynak gruplarınızı inceleyin, bu öğreticiyi çalıştırdığınız kaynak grubunu seçin ve kaynak grubunu silin.

## <a name="next-steps"></a>Sonraki adımlar

* RTSP simülatörü kullanmak yerine RTSP desteğiyle bir [IP kamerası](https://en.wikipedia.org/wiki/IP_camera) kullanın. Profiller G, S veya T ile uyumlu cihazlar ' ı arayarak [ONVIF uyumlu ürünler sayfasında](https://www.onvif.org/conformant-products/) , RTSP desteğiyle IP kameralarını arayabilirsiniz.
* AMD64 veya x64 Linux cihazı kullanın (Azure Linux VM kullanarak). Bu cihaz, IP kamerası ile aynı ağda olmalıdır. [Linux 'ta Azure IoT Edge çalışma zamanını Install](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) ' daki yönergeleri izleyebilir ve sonra Azure IoT Hub 'e kaydetmek için [ilk IoT Edge modülünüzü bir sanal Linux cihaz](https://docs.microsoft.com/azure/iot-edge/quickstart-linux) hızlı başlangıçlarına dağıtma ' daki yönergeleri izleyebilirsiniz.
