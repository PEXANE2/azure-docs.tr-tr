---
title: Hareket ve yayma olaylarını algılama-Azure
description: Bu hızlı başlangıçta, program aracılığıyla doğrudan yöntemleri çağırarak hareket ve yayma olaylarını algılamak için IoT Edge canlı video analizlerinin nasıl kullanılacağı gösterilmektedir.
ms.topic: quickstart
ms.date: 05/29/2020
ms.openlocfilehash: 4986ea13bec5382a8e0ef791e75442e4333e4356
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84262029"
---
# <a name="quickstart-detect-motion-and-emit-events"></a>Hızlı başlangıç: hareket ve yayma olaylarını Algıla

Bu hızlı başlangıç, IoT Edge üzerinde canlı video analizlerini kullanmaya başlamanıza yönelik adımlarda size yol gösterir. Bir Azure VM 'yi IoT Edge bir cihaz ve sanal bir canlı video akışı olarak kullanır. Kurulum adımlarını tamamladıktan sonra, bu akıştaki tüm hareketleri algılayan ve raporlayan bir medya grafiğiyle sanal bir canlı video akışı çalıştırabileceksiniz. Aşağıdaki diyagramda, bu medya grafiğinin grafik temsili gösterilmektedir.

![Hareket algılamayı temel alan canlı video analizi](./media/analyze-live-video/motion-detection.png) 

Bu makale, C# dilinde yazılan [örnek kodu](https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp) temel alır.

## <a name="prerequisites"></a>Ön koşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Aşağıdaki uzantılara sahip makinenizde [Visual Studio Code](https://code.visualstudio.com/) :
    1. [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
    2. [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* Sisteminizde yüklü [.NET Core 3,1 SDK 'sı](https://dotnet.microsoft.com/download/dotnet-core/3.1)

> [!TIP]
> Azure IoT araçları uzantısını yüklerken Docker yüklemeniz istenebilir. Yok saymaktan çekinmeyin.

## <a name="set-up-azure-resources"></a>Azure kaynakları ayarlama

Bu öğretici için aşağıdaki Azure kaynakları gereklidir.

* IoT Hub
* Depolama hesabı
* Azure Media Services hesabı
* Azure 'da [IoT Edge Runtime](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) yüklüyken Linux sanal makinesi

Bu hızlı başlangıçta, Azure aboneliğinizde yukarıda bahsedilen Azure kaynaklarını dağıtmak için [canlı video analizi kaynakları kurulum betiğini](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) kullanmanız önerilir. Bunun için aşağıdaki adımları uygulayın:

1. https://shell.azure.com adresine gidin.
1. Cloud Shell ilk kez kullanıyorsanız, bir depolama hesabı ve Microsoft Azure dosya paylaşımının oluşturulması için bir abonelik seçmeniz istenir. Cloud Shell oturum bilgilerinizi depolamak için bir depolama hesabı oluşturmak üzere "depolama alanı oluştur" seçeneğini belirleyin. Bu depolama hesabı, komut dosyasının Azure Media Services hesabınızla birlikte kullanılmak üzere oluşturulacağı bir ayrıdır.
1. Kabuk penceresinin sol tarafındaki açılan menüdeki ortamınız olarak "Bash" i seçin.

    ![Ortam Seçicisi](./media/quickstarts/env-selector.png)

1. Aşağıdaki komutu çalıştırın

    ```
    bash -c "$(curl -sL https://aka.ms/lva-edge/setup-resources-for-samples)"
    ```

    Betik başarıyla tamamlanırsa, aboneliğinizde yukarıda bahsedilen tüm kaynakları görmeniz gerekir.

1. Betik tamamlandıktan sonra, klasör yapısını göstermek için küme ayraçları üzerine tıklayın. ~/CloudDrive/LVA-Sample dizininde oluşturulmuş birkaç dosya görürsünüz. Bu hızlı başlangıçta ilgilendiğiniz:

     * ~/CloudDrive/LVA-Sample/Edge-Deployment/.exe-Visual Studio Code bir uç cihaza modül dağıtmak için kullandığı özellikleri içerir
     * ~/CloudDrive/LVA-Sample/AppSetting.exe-örnek kodu çalıştırmak için Visual Studio Code tarafından kullanılır
     
Hızlı başlangıçta Visual Studio Code içindeki dosyaları güncelleştirmek için bunlara ihtiyacınız olacaktır. Bunları şimdilik yerel bir dosyaya kopyalamak isteyebilirsiniz.


 ![Uygulama ayarları](./media/quickstarts/clouddrive.png)

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

1. Depoyu buradan kopyalayın https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp .
1. Visual Studio Code başlatın ve deponun indirildiği klasörü açın.
1. Visual Studio Code ' de, "src/buluttan cihaza-Console-App" klasörüne gidin ve "appSettings. JSON" adlı bir dosya oluşturun. Bu dosya, programı çalıştırmak için gereken ayarları içerir.
1. Önceki bölümde oluşturulan ~/CloudDrive/LVA-Sample/appSettings.JSON dosyasındaki içeriği kopyalayın (bkz. 5. adım)

    Metin şöyle görünmelidir:

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```
1. Sonra, "src/Edge" klasörüne gidin ve ". env" adlı bir dosya oluşturun.
1. İçeriği "/CloudDrive/LVA-Sample/Edge-Deployment/.exe ' dan kopyalayın. Metin şöyle görünmelidir:

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
    APPDATA_FOLDER_ON_DEVICE="/var/local/mediaservices"
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry username>"      
    ```

## <a name="examine-the-sample-files"></a>Örnek dosyaları inceleyin

1. Visual Studio Code, "src/Edge" e gidin. Oluşturduğunuz. env dosyasını, birkaç dağıtım şablonu dosyası ile birlikte görürsünüz.

    Dağıtım şablonu, kenar cihazının bazı yer tutucu değerleriyle birlikte dağıtım bildirimini ifade eder. . Env dosyası bu değişkenlerin değerlerini içerir.
1. Sonra, "src/buluttan-cihaza-Console-App" klasörüne gidin. Burada, oluşturduğunuz appSettings. json dosyasını ve diğer birkaç dosyayı görürsünüz:

    * C2D-Console-App. csproj-Visual Studio Code için proje dosyası.
    * Operations. JSON-bu dosya, programın çalıştırmasını istediğiniz farklı işlemleri listeler.
    * Program.cs-örnek program kodu, şunları yapar:
    
        * Uygulama ayarlarını yükler
        * IoT Edge modülünde canlı video analizi tarafından kullanıma sunulan doğrudan yöntemleri çağırır. [Doğrudan yöntemlerini](direct-methods.md) çağırarak canlı video akışlarını çözümlemek için modülünü kullanabilirsiniz 
        * TERMINAL penceresinde programın çıkışını ve çıkış penceresinde modül tarafından oluşturulan olayları incelemek için duraklamalar
        * Kaynakları temizlemek için doğrudan yöntemleri çağırır   

## <a name="generate-and-deploy-the-iot-edge-deployment-manifest"></a>IoT Edge dağıtım bildirimini oluşturma ve dağıtma

Dağıtım bildirimi, bir sınır cihazına hangi modüllerin dağıtıldığını ve bu modüllerin yapılandırma ayarlarını tanımlar. Şablon dosyasından böyle bir bildirim oluşturmak için bu adımları izleyin ve ardından bunu Edge cihazına dağıtın.

1. Visual Studio Code açın
1. Sol alt köşedeki AZURE ıOT HUB bölmesinin yanındaki "daha fazla eylem" simgesine tıklayarak ıothub bağlantı dizesini ayarlayın. Dizeyi src/Cloud-to-Device-Console-App/appSettings. json dosyasından kopyalayabilirsiniz. 

    ![IOT bağlantı dizesi ayarla](./media/quickstarts/set-iotconnection-string.png)
1. Sonra, "src/Edge/Deployment. Template. JSON" dosyasına sağ tıklayın ve "IoT Edge dağıtım bildirimi oluştur" seçeneğine tıklayın.
    ![IoT Edge dağıtım bildirimi oluştur](./media/quickstarts/generate-iot-edge-deployment-manifest.png)

    Bunun için "Deployment. AMD64. JSON" adlı src/Edge/config klasöründe bir bildirim dosyası oluşturulmalıdır.
1. "Src/Edge/config/Deployment. AMD64. JSON" öğesine sağ tıklayın ve "tek cihaz için dağıtım oluştur" seçeneğine tıklayın ve uç cihazınızın adını seçin.

    ![Tek bir cihaz için dağıtım oluşturma](./media/quickstarts/create-deployment-single-device.png)
1. Bundan sonra "bir IoT Hub cihaz seçmeniz" istenir. Açılan listeden LVA-örnek-cihaz ' ı seçin.
1. Yaklaşık 30 saniye içinde, sol alt kısımdaki Azure ıOT hub 'ını yenileyin ve Edge cihazında aşağıdaki modüllerin dağıtıldığını görmeniz gerekir:

    * IoT Edge (modül adı "lvaEdge") üzerinde canlı video analizi
    * RTSP simülatör (modül adı "rtspsim")

RTSP simülatör modülü, [canlı video analizi kaynakları kurulum betiğini](https://github.com/Azure/live-video-analytics/tree/master/edge/setup)çalıştırdığınızda Edge cihazınıza kopyalanmış olan bir video dosyası kullanarak canlı bir video akışının benzetimini yapar. Bu aşamada, modüller dağıtıldı, ancak hiçbir medya grafiği etkin değil.

## <a name="prepare-for-monitoring-events"></a>İzleme olaylarını hazırlama

IoT Edge modüldeki canlı video analizinden yararlanarak gelen canlı video akışındaki hareketleri algılayabilir ve olayları IoT Hub gönderebilirsiniz. Bu olayları görmek için şu adımları izleyin:

1. Visual Studio Code Gezgin bölmesini açın ve sol alt köşedeki Azure IoT Hub arayın.
1. Cihazlar düğümünü genişletin.
1. LVA-örnek-cihazındaki sağ-Clink ve "yerleşik olay Izlemeyi Izlemeye başla" seçeneğini seçti.

    ![Yerleşik olay uç noktasını izlemeye başla](./media/quickstarts/start-monitoring-iothub-events.png)

## <a name="run-the-sample-program"></a>Örnek programı çalıştırma

Örnek kodu çalıştırmak için aşağıdaki adımları izleyin.
1. Visual Studio Code, "src/Cloud-to-Device-Console-App/Operations. JSON" öğesine gidin.
1. GraphTopologySet düğümünün altında aşağıdakilerden emin olun:

    ` "topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/motion-detection/topology.json"`
1. Ardından, Graphınstanceset ve GraphTopologyDelete düğümleri altında, topologyName değerinin yukarıdaki grafik topolojisinde "Name" özelliğinin değeriyle eşleştiğinden emin olun:

    `"topologyName" : "MotionDetection"`
    
1. Bir hata ayıklama oturumu başlatın (F5 tuşuna basın). TERMINAL penceresinde yazdırılmış bazı iletileri görmeye başlayacaksınız.
1. Operations. JSON, Graphtopologyılist ve Graphınstancelist çağrıları ile başlatılır. Önceki hızlı başlangıçlardan sonra kaynakları temizlediyseniz, bu, boş listeler döndürür ve ardından ENTER tuşuna basmanız için duraklatırsınız.

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
         "topologyName": "MotionDetection",
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
     
     * Graph örneğini başlatmak için Graphınstanceactivate çağrısı yapın ve video akışını başlatın.
     * Grafik örneğinin gerçekten çalışır durumda olduğunu göstermek için Graphınstancelist öğesine yapılan ikinci bir çağrı.
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
[IoTHubMonitor] [9:42:18 AM] Message received from [lvaedgesample/lvaEdge]:  
{  
"body": {
"sdp": "SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 xxx.xxx.xxx.xxx\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets={SPS}\r\na=control:track1\r\n"  
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

* İleti bir tanılama olayıdır, Mediasessionkurulan, RTSP kaynak düğümünün (konu) RTSP simülatörü ile bağlantı kurabildiğini ve (sanal) canlı bir akış almaya başlayabileceğini gösterir.
* ApplicationProperties 'teki "Subject" değeri, iletinin oluşturulduğu grafik topolojisinde bulunan düğüme başvurur. Bu durumda, ileti RTSP kaynak düğümünden gönderilir.
* applicationProperties 'teki "eventType" bir tanılama olayı olduğunu gösterir.
* "eventTime" olayın gerçekleştiği saati gösterir.
* "gövde", bu durumda [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol) ayrıntılarından oluşan tanılama olayı hakkındaki verileri içerir.


## <a name="motion-detection-event"></a>Hareket algılama olayı

Hareket algılandığında, canlı video analizi uç modülü bir çıkarım olayı gönderir. Tür "Motion" olarak ayarlanır ve bu, hareket algılama Işlemcisinden kaynaklanan bir sonuç olduğunu gösterir ve eventTime, size (UTC) hareketin oluştuğunu söyler. Aşağıda bir örnek verilmiştir:

```
  {  
  "body": {  
    "timestamp": 142843967343090,
    "inferences": [  
      {  
        "type": "motion",  
        "motion": {  
          "box": {  
            "l": 0.573222,  
            "t": 0.492537,  
            "w": 0.141667,  
            "h": 0.074074  
          }  
        }  
      }  
    ]  
  },  
  "applicationProperties": {  
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",  
    "subject": "/graphInstances/GRAPHINSTANCENAME/processors/md",  
    "eventType": "Microsoft.Media.Graph.Analytics.Inference",  
    "eventTime": "2020-04-17T20:26:32.7010000Z",
    "dataVersion": "1.0"  
  }  
}  
```

* applicationProperties 'teki "Subject", iletinin oluşturulduğu medya grafiğindeki düğüme başvurur. Bu durumda, ileti hareket algılama işlemcisi düğümünden gönderilir.
* applicationProperties 'teki "eventType" bir analiz olayı olduğunu gösterir.
* "eventTime" olayın gerçekleştiği saati gösterir.
"gövde" analiz olayı hakkındaki verileri içerir. Bu durumda, olay bir çıkarım olayıdır ve bu nedenle gövde "timestamp" ve "ında" verilerini içerir.
* "ında" verileri "tür" ın "Motion" olduğunu ve bu "Motion" olayı hakkında ek verilere sahip olduğunu gösterir.
* "Box" bölümü, hareketli nesnenin çevresindeki bir sınırlayıcı kutunun koordinatlarını içerir. Değerler, videonun piksel genişliği ve yüksekliğine göre normalleştirilir (örn. 1920 genişliği ve 1080 yüksekliği).

    ```
    l - distance from left of image
    t - distance from top of image
    w - width of bounding box
    h - height of bounding box
    ```
    
## <a name="cleanup-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıçlarını denemek istiyorsanız oluşturulan kaynaklarda tutmanız gerekir. Aksi takdirde Azure portal gidin, Kaynak gruplarınızı inceleyin, bu hızlı başlangıcı çalıştırdığınız kaynak grubunu seçin ve tüm kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

Canlı video akışındaki bir nesneyi algılama gibi diğer hızlı başlangıçlarını çalıştırın.        
