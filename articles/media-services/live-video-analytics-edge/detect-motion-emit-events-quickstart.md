---
title: Hareket ve yayma olaylarını algılama-Azure
description: Bu hızlı başlangıçta, program aracılığıyla doğrudan yöntemleri çağırarak hareket ve yayma olaylarını algılamak için IoT Edge canlı video analizlerinin nasıl kullanılacağı gösterilmektedir.
ms.topic: quickstart
ms.date: 05/29/2020
ms.openlocfilehash: fca773d0583bee3bef4e7254bcca95866b2205e9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87091928"
---
# <a name="quickstart-detect-motion-and-emit-events"></a>Hızlı başlangıç: hareket ve yayma olaylarını Algıla

Bu hızlı başlangıç, IoT Edge üzerinde canlı video analizlerini kullanmaya başlamanıza yönelik adımlarda size yol gösterir. IoT Edge cihazı olarak bir Azure sanal makinesi ve canlı video akış simülasyonu kullanır. Kurulum adımlarını tamamladıktan sonra, bu akıştaki tüm hareketleri algılayan ve raporlayan bir medya grafiğiyle sanal bir canlı video akışı çalıştırabileceksiniz. Aşağıdaki diyagramda, bu medya grafiğinin grafik temsili gösterilmektedir.

![Hareket algılamayı temel alan canlı video analizi](./media/analyze-live-video/motion-detection.png) 

Bu makale, C# dilinde yazılan [örnek kodu](https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp) temel alır.

## <a name="prerequisites"></a>Önkoşullar

* Etkin aboneliği olan bir Azure hesabı. Henüz bir [hesabınız yoksa ücretsiz olarak bir hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) .
* Aşağıdaki uzantılara sahip [Visual Studio Code](https://code.visualstudio.com/) :
    * [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
    * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1). 

> [!TIP]
> Azure IoT araçları uzantısını yüklerken Docker yüklemeniz istenebilir. İstemi yoksayabilirsiniz.

## <a name="set-up-azure-resources"></a>Azure kaynakları ayarlama

Bu öğreticide aşağıdaki Azure kaynakları gereklidir:

* IoT Hub
* Depolama hesabı
* Azure Media Services hesabı
* Azure 'da [IoT Edge Runtime](../../iot-edge/how-to-install-iot-edge-linux.md) yüklüyken Linux sanal makinesi

Bu hızlı başlangıç için, Azure aboneliğinizde gerekli kaynakları dağıtmak üzere [canlı video analizi kaynakları kurulum betiğini](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) kullanmanızı öneririz. Bunu yapmak için şu adımları uygulayın:

1. [Azure Cloud Shell](https://shell.azure.com)’i açın.
1. İlk kez Cloud Shell kullanıyorsanız, bir depolama hesabı ve bir Microsoft Azure dosya paylaşımının oluşturulması için bir abonelik seçmeniz istenir. Cloud Shell oturum bilgileriniz için bir depolama hesabı oluşturmak üzere **depolama oluştur** ' u seçin. Bu depolama hesabı, komut dosyasının Azure Media Services hesabınızla kullanılmak üzere oluşturulacağı hesaptan ayrıdır.
1. Cloud Shell penceresinin sol tarafındaki açılan menüde, ortamınız olarak **Bash** ' i seçin.

    ![Ortam Seçicisi](./media/quickstarts/env-selector.png)

1. Aşağıdaki komutu çalıştırın.

    ```
    bash -c "$(curl -sL https://aka.ms/lva-edge/setup-resources-for-samples)"
    ```

    Betik başarıyla tamamlanerdiğinde, aboneliğinizdeki tüm gerekli kaynakları görmeniz gerekir.

1. Betik tamamlandıktan sonra, klasör yapısını göstermek için süslü ayraçları seçin. *~/CloudDrive/LVA-Sample* dizininde birkaç dosya görürsünüz. Bu hızlı başlangıçta ilgilendiğiniz:

     * ***~/CloudDrive/LVA-Sample/Edge-Deployment/.exe*** -bu dosya Visual Studio Code bir uç cihaza modül dağıtmak için kullandığı özellikleri içerir.
     * ***~/CloudDrive/LVA-Sample/appsetting.json*** -Visual Studio Code, örnek kodu çalıştırmak için bu dosyayı kullanır.
     
Bir sonraki bölümde Visual Studio Code ' de geliştirme ortamınızı ayarlarken bu dosyalar gerekir. Bunları şimdilik yerel bir dosyaya kopyalamak isteyebilirsiniz.

 ![Uygulama ayarları](./media/quickstarts/clouddrive.png)

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

1. Depoyu bu konumdan kopyala: https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp .
1. Visual Studio Code, deponun indirildiği klasörü açın.
1. Visual Studio Code, *src/buluttan cihaza-Console-App* klasörüne gidin. Burada bir dosya oluşturun ve *appsettings.js*. Bu dosya, programı çalıştırmak için gereken ayarları içerir.
1. Bu hızlı başlangıçta oluşturduğunuz dosyadaki *~/CloudDrive/LVA-Sample/appsettings.js* içeriğini kopyalayın.

    Metin aşağıdaki çıktıya benzer şekilde görünmelidir.

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```
1. *Src/Edge* klasörüne gidin ve *. env*adlı bir dosya oluşturun.
1. */CloudDrive/LVA-Sample/Edge-Deployment/-env* dosyasının içeriğini kopyalayın. Metin aşağıdaki kod gibi görünmelidir.

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

1. Visual Studio Code, *src/Edge*bölümüne gidin. *. Env* dosyası ve birkaç dağıtım şablonu dosyası görürsünüz.

    Dağıtım şablonu, sınır cihazının bazı özellikler için kullanıldığı dağıtım bildirimini ifade eder. *. Env* dosyası bu değişkenlerin değerlerini içerir.
1. *Src/buluttan cihaza-Console-App* klasörüne gidin. Burada dosya ve diğer birkaç dosya *appsettings.js* görürsünüz:

    * ***C2D-Console-App. csproj*** -Visual Studio Code için proje dosyası.
    * ***operations.js*** , programın çalıştırmasını istediğiniz işlemlerin bir listesi.
    * ***Program.cs*** -örnek program kodu. Bu kod:
    
      * Uygulama ayarlarını yükler.
      * IoT Edge modülündeki canlı video analizi tarafından açığa çıkarılan doğrudan yöntemleri çağırır. [Doğrudan yöntemlerini](direct-methods.md)çağırarak canlı video akışlarını çözümlemek için modülünü kullanabilirsiniz.
      * Programın çıkışını **TERMINAL** penceresinde Incelemenize ve **Çıkış** penceresinde modül tarafından oluşturulan olayları incelemenize olanak sağlamak için duraklar.
      * Kaynakları temizlemek için doğrudan yöntemleri çağırır.   

## <a name="generate-and-deploy-the-deployment-manifest"></a>Dağıtım bildirimini oluşturma ve dağıtma

Dağıtım bildirimi, bir sınır cihazına hangi modüllerin dağıtıldığını tanımlar. Ayrıca, bu modüllerle ilgili yapılandırma ayarlarını tanımlar. 

Şablon dosyasından bildirim oluşturmak ve ardından Edge cihazına dağıtmak için bu adımları izleyin.

1. Visual Studio Code’u açın.
1. **Azure ıOT hub** bölmesinin yanındaki IoT Hub bağlantı dizesini ayarlamak için **diğer eylemler** simgesini seçin. Dizeyi *src/buluttan cihazdan-Console-App/appsettings.js* dosyasından kopyalayabilirsiniz. 

    ![IOT bağlantı dizesi ayarla](./media/quickstarts/set-iotconnection-string.png)

1. **Src/Edge/deployment.template.jsüzerinde** sağ tıklayın ve **IoT Edge dağıtım bildirimi oluştur**' u seçin.

    ![IoT Edge dağıtım bildirimini oluşturma](./media/quickstarts/generate-iot-edge-deployment-manifest.png)

    Bu eylem, *src/Edge/config* klasöründe *deployment.amd64.js* adlı bir bildirim dosyası oluşturmalı.
1. **Src/Edge/config/deployment.amd64.jsüzerinde**sağ tıklayın, **tek cihaz için dağıtım oluştur**' u seçin ve ardından Edge cihazınızın adını seçin.

    ![Tek bir cihaz için dağıtım oluşturma](./media/quickstarts/create-deployment-single-device.png)

1. IoT Hub bir cihaz seçmeniz istendiğinde, açılan menüden **LVA-Sample-Device** ' ı seçin.
1. 30 saniye sonra, pencerenin sol alt köşesinde Azure IoT Hub ' yi yenileyin. Edge cihazında artık aşağıdaki dağıtılan modüller gösterilmektedir:

    * IoT Edge (modül adı) üzerinde canlı video analizi `lvaEdge`
    * Gerçek zamanlı akış protokolü (RTSP) Simülatörü (modül adı `rtspsim` )

RTSP simülatör modülü, [canlı video analizi kaynakları kurulum betiğini](https://github.com/Azure/live-video-analytics/tree/master/edge/setup)çalıştırdığınızda Edge cihazınıza kopyalanmış bir video dosyası kullanarak canlı bir video akışının benzetimini yapar. 

Bu aşamada modüller dağıtılır ancak hiçbir medya grafiği etkin değildir.

## <a name="prepare-to-monitor-events"></a>Olayları izlemeye hazırlanma

IoT Edge modüldeki canlı video analizlerini kullanarak gelen canlı video akışındaki hareketleri algılayabilir ve olayları IoT Hub gönderebilirsiniz. Bu olayları görmek için şu adımları izleyin:

1. Visual Studio Code Gezgin bölmesini açın ve sol alt köşedeki Azure IoT Hub arayın.
1. **Cihazlar** düğümünü genişletin.
1. **LVA-örnek-cihaz öğesine** sağ tıklayın ve **Izlemeyi Başlat yerleşik olay uç noktası**' nı seçin.

    ![Yerleşik bir olay uç noktasını izlemeye başlama](./media/quickstarts/start-monitoring-iothub-events.png)

## <a name="run-the-sample-program"></a>Örnek programı çalıştırma

Örnek kodu çalıştırmak için aşağıdaki adımları izleyin:

1. Visual Studio Code ' de, *src/buluttan cihaza-Console-App/operations.json*' a gidin.
1. **Graphtopologyset** düğümünde aşağıdaki değeri görtığınızdan emin olun:

    `"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/motion-detection/topology.json"`
1. **Graphınstanceset** ve **Graphtopologydelete** düğümlerinde, değerinin `topologyName` `name` grafik topolojisinde özelliğin değeriyle eşleştiğinden emin olun:

    `"topologyName" : "MotionDetection"`
    
1. F5 tuşunu seçerek bir hata ayıklama oturumu başlatın. **TERMINAL** penceresinde bazı iletiler görüntülenir.
1. Dosyasındaki *operations.js* , ve çağrıları ile başlatılır `GraphTopologyList` `GraphInstanceList` . Önceki hızlı başlangıçlarını bitirdikten sonra kaynakları temizledikten sonra bu işlem boş listeleri döndürür ve ardından duraklatılır. Devam etmek için Enter tuşunu seçin.

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
     
     * `GraphInstanceActivate`Grafik örneğini ve videonun akışını başlatan öğesine yönelik bir çağrı
     * `GraphInstanceList`Grafik örneğinin çalışır durumda olduğunu gösteren ikinci bir çağrı
1. **TERMINAL** penceresindeki çıktı, tarihinde duraklatılır `Press Enter to continue` . Henüz ENTER ' ı seçmeyin. Doğrudan çağrdığınız yöntemler için JSON yanıtı yüklerini görmek üzere yukarı kaydırın.
1. Visual Studio Code **Çıkış** penceresine geçin. IoT Edge modülündeki canlı video analizinin IoT Hub 'ına gönderdiğini iletiler görürsünüz. Bu hızlı başlangıçta aşağıdaki bölümde bu iletiler ele alınmaktadır.
1. Medya grafiği çalışmaya devam eder ve sonuçları yazdırır. RTSP simülatörü kaynak videoyu döngüye sokmaya devam eder. Medya grafiğini durdurmak için, **TERMINAL** penceresine dönün ve ENTER ' u seçin. 

    Sonraki çağrı dizisi kaynakları temizler:
     * `GraphInstanceDeactivate`Grafik örneğini devre dışı bırakmak için bir çağrı.
     * `GraphInstanceDelete`Örneği silme çağrısı.
     * İçin bir çağrı `GraphTopologyDelete` , topolojiyi siler.
     * İçin nihai bir çağrı `GraphTopologyList` , listenin boş olduğunu gösterir.

## <a name="interpret-results"></a>Sonuçları yorumlama

Medya grafiğini çalıştırdığınızda, hareket algılayıcısı işlemci düğümündeki sonuçlar, IoT Hub havuz düğümünden IoT Hub 'ına geçer. Visual Studio Code **Çıkış** penceresinde gördüğünüz iletiler bir `body` bölümü ve bir `applicationProperties` bölümü içerir. Daha fazla bilgi için bkz. [IoT Hub Iletileri oluşturma ve okuma](../../iot-hub/iot-hub-devguide-messages-construct.md).

Aşağıdaki iletilerde, canlı video analizi modülü, uygulama özelliklerini ve gövdenin içeriğini tanımlar.

### <a name="mediasessionestablished-event"></a>Mediasessionkurulduğu olayı

Bir medya grafiği oluşturulduğunda, RTSP kaynak düğümü, rtspsim-live555 kapsayıcısında çalışan RTSP sunucusuna bağlanmaya çalışır. Bağlantı başarılı olursa, aşağıdaki olay yazdırılır.

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

Önceki çıktıda: 
* İleti bir tanılama olayıdır `MediaSessionEstablished` . RTSP kaynak düğümünün (konu) RTSP simülatörü ile bağlandığını ve (benzetimli) canlı akışını almaya başladığını gösterir.
* ' De, içinde `applicationProperties` `subject` iletinin oluşturulduğu grafik topolojisinde bulunan düğüme başvurur. Bu durumda, ileti RTSP kaynak düğümünden gelmektedir.
* `applicationProperties`' De, `eventType` Bu olayın bir tanılama olayı olduğunu gösterir.
* `eventTime`Değer, olayın gerçekleştiği zamanı gösterir.
* `body`Bölüm, tanılama olayı hakkındaki verileri içerir. Bu durumda, veriler [oturum açıklaması Protokolü (SDP)](https://en.wikipedia.org/wiki/Session_Description_Protocol) ayrıntılarını içerir.


### <a name="motiondetection-event"></a>MotionDetection olayı

Hareket algılandığında IoT Edge modüldeki canlı video analizi bir çıkarım olayı gönderir. , `type` `motion` Hareket algılama işlemcisinden kaynaklanan bir sonuç olduğunu belirtmek için olarak ayarlanır. `eventTime`Değer, hareketin ne zaman (UTC) meydana oluştuğunu söyler. 

Bu iletinin bir örneği aşağıda verilmiştir:

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

Bu örnekte: 

* `applicationProperties`' De, `subject` iletinin oluşturulduğu medya grafiğindeki düğüme başvurur. Bu durumda, ileti hareket algılama işlemcisi düğümünden gelmektedir.
* `applicationProperties`' De, `eventType` Bu olayın bir analiz olayı olduğunu gösterir.
* `eventTime`Değer, olayın gerçekleştiği zaman değeridir.
* `body`Değer, analiz olayı hakkında veri değeridir. Bu durumda, olay bir çıkarım olayıdır, bu nedenle gövde `timestamp` ve veri içerir `inferences` .
* Veri, olduğunu `inferences` gösterir `type` `motion` . Bu olay hakkında ek verilere sahiptir `motion` .
* `box`Bölüm, hareketli nesnenin çevresindeki bir sınırlayıcı kutunun koordinatlarını içerir. Değerler, videonun piksel cinsinden genişliği ve yüksekliğiyle normalleştirilir. Örneğin, Genişlik 1920, yükseklik ise 1080 ' dir.

    ```
    l - distance from left of image
    t - distance from top of image
    w - width of bounding box
    h - height of bounding box
    ```
    
## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıçlarını denemek istiyorsanız, oluşturduğunuz kaynakları saklamanız gerekir. Aksi takdirde, Azure portal kaynak gruplarınıza gidin, bu hızlı başlangıcı çalıştırdığınız kaynak grubunu seçin ve ardından tüm kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

Canlı video akışındaki bir nesneyi algılama gibi diğer hızlı başlangıçlarını çalıştırın.        
