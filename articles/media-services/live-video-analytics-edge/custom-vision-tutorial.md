---
title: IoT Edge ve Azure Özel Görüntü İşleme canlı video analiziyle canlı videoyu çözümleyin
description: Azure Özel Görüntü İşleme kullanarak bir oyunsuna algılayan ve canlı video akışından oyungeleks 'leri saptamak için modeli bir kenara dağıtmak üzere Azure IoT Edge Azure Live video Analytics 'in AI genişletilebilirliği özelliğini kullanan kapsayıcılı bir model oluşturma hakkında bilgi edinin.
ms.topic: tutorial
ms.date: 09/08/2020
ms.openlocfilehash: 52678d66bd4a91c9308a3cc48fbf784e89a5cfe8
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92171516"
---
# <a name="tutorial-analyze-live-video-with-live-video-analytics-on-iot-edge-and-azure-custom-vision"></a>Öğretici: IoT Edge ve Azure Özel Görüntü İşleme canlı video analizi ile canlı videoyu çözümleyin

Bu öğreticide, Azure Özel Görüntü İşleme kullanarak bir oyunsuna algılayan ve canlı video akışından oyungeleks 'i saptamak için modeli bir kenara dağıtmak üzere Azure IoT Edge Azure Live video Analytics 'in [AI genişletilebilirliği özelliğini](analyze-live-video-concept.md#analyzing-video-using-a-custom-vision-model) kullanarak Azure [Custom Vision](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/) 'yi nasıl kullanacağınızı öğreneceksiniz.

Birkaç görüntüyü karşıya yükleyerek ve etiketleyerek bir bilgisayar vizyonu modeli oluşturmak ve eğitme için Özel Görüntü İşleme gücünü nasıl bir araya getirebileceğiniz gösterilmektedir. Veri bilimi, makine öğrenimi veya AI hakkında bilgi sahibi olmanız gerekmez. Ayrıca, canlı video analizlerinin yeteneklerini, özel bir modeli kenarda bir kapsayıcı olarak kolayca dağıtmak ve sanal bir canlı video akışını analiz etmek için de öğreneceksiniz.

Bu öğretici bir Azure sanal makinesini (VM) IoT Edge bir cihaz olarak kullanır ve C# dilinde yazılan örnek kodları temel alır. Bu öğreticideki bilgiler, [hareket ve yayma olaylarını Algıla](detect-motion-emit-events-quickstart.md) hızlı başlangıç bölümünde oluşturulur.

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * İlgili kaynakları ayarlayın.
> * Oyuncak paketleri algılamak ve kenarda dağıtmak için bulutta bir Özel Görüntü İşleme modeli oluşturun.
> * Bir Özel Görüntü İşleme modeline HTTP Uzantısı ile bir medya grafiği oluşturun ve dağıtın.
> * Örnek kodu çalıştırın.
> * Sonuçları inceleyin ve yorumlayın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="suggested-pre-reading"></a>Önerilen önceden okuma

Başlamadan önce aşağıdaki makaleleri okuyun:

* [IoT Edge genel bakış üzerinde canlı video analizi](overview.md)
* [Azure Özel Görüntü İşleme genel bakış](../../cognitive-services/custom-vision-service/overview.md)
* [IoT Edge terminolojisinde canlı video analizi](terminology.md)
* [Medya grafiği kavramları](media-graph-concept.md)
* [Video kaydı olmadan Canlı Video Analizi](analyze-live-video-concept.md)
* [Kendi modelinizle canlı video analizi çalıştırma](use-your-model-quickstart.md)
* [Öğretici: IoT Edge modülünü geliştirme](../../iot-edge/tutorial-develop-for-linux.md)
* [Dağıtımı düzenleme. * .template.js](https://github.com/microsoft/vscode-azure-iot-edge/wiki/How-to-edit-deployment.*.template.json)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticinin önkoşulları şunlardır:

* [Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) ve [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) uzantıları ile geliştirme makinenizde [Visual Studio Code](https://code.visualstudio.com/) .

    > [!TIP]
    > Docker 'ı yüklemek isteyip istemediğiniz sorulabilir. Bu istemi yoksayın.
* Geliştirme makinenizde [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/thank-you/sdk-3.1.201-windows-x64-installer) .
* Sahip olduğunuzdan emin olun:
    
    * [Azure kaynakları ayarlama](detect-motion-emit-events-quickstart.md#set-up-azure-resources)
    * [Geliştirme ortamınızı kurma](detect-motion-emit-events-quickstart.md#set-up-your-development-environment)

## <a name="review-the-sample-video"></a>Örnek videoyu gözden geçirin

Bu öğreticide, canlı bir akışın benzetimini yapmak için bir [oyuncar bir video](https://lvamedia.blob.core.windows.net/public/t2.mkv) dosyası kullanılmaktadır. [VLC medya oynatıcı](https://www.videolan.org/vlc/)gibi bir uygulama aracılığıyla videoyu inceleyebilirsiniz. **CTRL + N**' ı seçin ve ardından kayıttan yürütmeyi başlatmak üzere [oyuncar arabasının videosunu](https://lvamedia.blob.core.windows.net/public/t2.mkv) bir bağlantı yapıştırın. Videoyu izlerken videoda bir oyunçın 36 saniyelik işaretçisi ile ilgili olduğunu unutmayın. Özel model bu oyuncak kamyonu algılamak için eğitildi. Bu öğreticide, bu tür oyunları algılamak ve ilişkili çıkarım olaylarını IoT Edge hub 'ına yayımlamak için IoT Edge üzerinde canlı video analizi kullanacaksınız.

## <a name="overview"></a>Genel Bakış

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/custom-vision-tutorial/topology-custom-vision.svg" alt-text="Özel Görüntü İşleme genel bakış gösteren diyagram.":::

Bu diyagramda, sinyallerin Bu öğreticide nasıl akagösterdiği gösterilmektedir. [Edge modülü](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) , Real-Time Akış Protokolü (RTSP) sunucusunu BARıNDıRAN bir IP kamerasına benzetir. Bir [RTSP kaynak](media-graph-concept.md#rtsp-source) düğümü, bu sunucudan video akışını çeker ve [çerçeve hızı filtre işlemcisi](media-graph-concept.md#frame-rate-filter-processor) düğümüne video çerçeveleri gönderir. Bu işlemci, [http uzantısı işlemci](media-graph-concept.md#http-extension-processor) düğümüne ulaşan video akışının kare oranını sınırlandırır.

HTTP uzantısı düğümü bir ara sunucu rolünü yürütür. Video çerçevelerini belirtilen görüntü türüne dönüştürür. Ardından, görüntüyü REST üzerinden bir HTTP uç noktasının arkasında bulunan bir AI modeli çalıştıran başka bir Edge modülüne geçirir. Bu örnekte, bu Edge modülü Özel Görüntü İşleme kullanılarak oluşturulan oyunker algılayıcı modelidir. HTTP uzantısı işlemci düğümü, algılama sonuçlarını toplar ve olayları [Azure IoT Hub havuzu](media-graph-concept.md#iot-hub-message-sink) düğümüne yayımlar. Düğüm daha sonra bu olayları [IoT Edge hub 'ına](../../iot-edge/iot-edge-glossary.md#iot-edge-hub)gönderir.

## <a name="build-and-deploy-a-custom-vision-toy-detection-model"></a>Özel Görüntü İşleme oyunsıya algılama modeli oluşturun ve dağıtın

Ad Özel Görüntü İşleme önerdiğinde, bulutu kullanarak kendi özel nesne algılayıcısının veya sınıflandırıcınızı oluşturabilirsiniz. Bulutta veya bir kenara kapsayıcılar aracılığıyla dağıtılabilecek Özel Görüntü İşleme modeller oluşturmak için basit, kullanımı kolay ve sezgisel bir arabirim sağlar.

Bir oyunker algılayıcısı oluşturmak için [hızlı başlangıç: özel görüntü işleme Web sitesiyle bir nesne algılayıcısı oluşturma](../../cognitive-services/custom-vision-service/get-started-build-detector.md)' daki adımları izleyin.

Ek notlar:
 
* Bu öğretici için, hızlı başlangıç makalesi [önkoşulları bölümünde](../../cognitive-services/custom-vision-service/get-started-build-detector.md#prerequisites)belirtilen örnek görüntüleri kullanmayın. Bunun yerine, bir oyuncak algılayıcı Özel Görüntü İşleme modeli oluşturmak için belirli bir görüntü kümesi kullandık. Hızlı başlangıçta [eğitim görüntülerinizi seçmeniz](../../cognitive-services/custom-vision-service/get-started-build-detector.md#choose-training-images) istendiğinde [Bu görüntüleri](https://lvamedia.blob.core.windows.net/public/ToyCarTrainingImages.zip) kullanın.
* Hızlı Başlangıç ' ın etiketleme resmi bölümünde, resimde görülen oyunu kamyonu "teslim kamyonu" etiketiyle etiketdiğinizden emin olun.

İşiniz bittiğinde, **performans** sekmesindeki **dışarı aktar** düğmesini kullanarak modeli bir Docker kapsayıcısına dışarı aktarabilirsiniz. Kapsayıcı platformu türü olarak Linux ' u seçtiğinizden emin olun. Bu, kapsayıcının çalışacağı platformdur. Kapsayıcıyı yüklediğiniz makine Windows veya Linux olabilir. Aşağıdaki yönergeler, bir Windows makinesine indirilen kapsayıcı dosyasını temel alır.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/custom-vision-tutorial/docker-file.png" alt-text="Özel Görüntü İşleme genel bakış gösteren diyagram."   13 hours ago        Up 25 seconds       127.0.0.1:80->80/tcp   practical_cohen
        ```
      1. `curl -X POST http://127.0.0.1:80/image -F imageData=@<path to any image file that has the toy delivery truck in it>`
            
            Bu komut, kapsayıcıyı yerel makinede sınar. Görüntü üzerinde eğitidiğimiz görüntüde aynı teslimat kamyonu varsa, çıktı aşağıdaki örneğe benzer bir şekilde olmalıdır. Teslim kamyonu% 90,12 olasılık ile algıladığını önerir.
    
            ```
            {"created":"2020-03-20T07:10:47.827673","id":"","iteration":"","predictions":[{"boundingBox":{"height":0.66167289,"left":-0.03923762,"top":0.12781593,"width":0.70003178},"probability":0.90128148,"tagId":0,"tagName":"delivery truck"},{"boundingBox":{"height":0.63733053,"left":0.25220079,"top":0.0876643,"width":0.53331227},"probability":0.59745145,"tagId":0,"tagName":"delivery truck"}],"project":""}
            ```

<!--## Create and deploy a media graph with http extension to custom vision model-->

## <a name="examine-the-sample-files"></a>Örnek dosyaları inceleyin

1. Visual Studio Code, src/Edge öğesine gidin. Oluşturduğunuz. env dosyasını, birkaç dağıtım şablonu dosyası ile birlikte görürsünüz.

    Dağıtım şablonu, kenar cihazının bazı yer tutucu değerleriyle birlikte dağıtım bildirimini ifade eder. . Env dosyası bu değişkenlerin değerlerini içerir.
1. Ardından src/buluttan cihaza-Console-App klasörüne gidin. Burada, oluşturduğunuz dosyada, diğer birkaç dosyayla birlikte appsettings.jsgörürsünüz:

    * C2D-Console-App. csproj: Visual Studio Code için proje dosyasıdır.
    * operations.js: Bu dosya, programın çalıştırmasını istediğiniz farklı işlemleri listeler.
    * Program.cs: Bu örnek program kodu:

        * Uygulama ayarlarını yükler.
        * Topoloji oluşturmak, grafiği örneklemek ve grafiği etkinleştirmek için IoT Edge modülünün doğrudan yöntemlerinde canlı video analizlerini çağırır.
        * , **TERMINAL** penceresinde grafik çıkışını ve **Çıkış** penceresinde IoT Hub 'ına gönderilen olayları incelemenizi sağlar.
        * Grafik örneğini devre dışı bırakır, grafik örneğini siler ve grafik topolojisini siler.
        
## <a name="generate-and-deploy-the-deployment-manifest"></a>Dağıtım bildirimini oluşturma ve dağıtma

1. Visual Studio Code ' de, src/buluttan cihaza-Console-App/operations.json ' a gidin.

1. Altında `GraphTopologySet` , aşağıdakilerin doğru olduğundan emin olun:<br/>`"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/httpExtension/topology.json"`
1. Altında `GraphInstanceSet` , aşağıdakileri doğrulayın:
    1. `"topologyName" : "InferencingWithHttpExtension"`
    1. Parametreleri dizisinin en üstüne aşağıdakileri ekleyin: `{"name": "inferencingUrl","value": "http://cv:80/image"},`
    1. `rtspUrl`Parametre değerini olarak değiştirin `"rtsp://rtspsim:554/media/t2.mkv"` .
1. Altında `GraphTopologyDelete` , emin olun `"name": "InferencingWithHttpExtension"` .
1. Dosyasında src/Edge/deployment.customvision.template.jsöğesine sağ tıklayın ve **IoT Edge dağıtım bildirimi oluştur**' u seçin.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-vision-tutorial/deployment-template-json.png" alt-text="Özel Görüntü İşleme genel bakış gösteren diyagram.":::
  
    Bu eylem, üzerinde deployment.customvision.amd64.jsadlı src/Edge/config klasöründe bir bildirim dosyası oluşturmalı.
1. Dosyasında src/Edge/deployment.customvision.template.jsaçın ve `registryCredentials` JSON bloğunu bulun. Bu blokta, Azure Container kayıt defterinizin adresini Kullanıcı adı ve parolasıyla birlikte bulacaksınız.
1. Komut satırında aşağıdaki adımları izleyerek yerel Özel Görüntü İşleme kapsayıcısını Azure Container Registry örneğinize gönderin:

    1. Aşağıdaki komutu yürüterek kayıt defterinde oturum açın:
    
        `docker login <address>`
    
        Kimlik doğrulaması sorulduğunda Kullanıcı adını ve parolayı girin.
        
        > [!NOTE]
        > Parola, komut satırında görünmez.
    1. Şu komutu kullanarak görüntünüzü etiketleyin: <br/>`docker tag cvtruck   <address>/cvtruck`.
    1. Şu komutu kullanarak görüntünüzü gönderin: <br/>`docker push <address>/cvtruck`.

        Başarılı olursa, `Pushed` görüntü IÇIN SHA ile birlikte komut satırında görmeniz gerekir.
    1. Ayrıca, Azure portal Azure Container Registry örneğinizi denetleyerek de doğrulayabilirsiniz. Burada, deponun adını etiketiyle birlikte görürsünüz.
1. Sol alt köşedeki **Azure ıOT hub** bölmesinin yanındaki **daha fazla eylem** simgesini seçerek IoT Hub bağlantı dizesini ayarlayın. Dizeyi dosyadaki appsettings.jskopyalayabilirsiniz. ( [Select IoT Hub komutu](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Select-IoT-Hub)aracılığıyla Visual Studio Code içinde yapılandırılmış doğru IoT Hub 'ına sahip olduğunuzdan emin olmak için başka bir önerilen yaklaşım aşağıda verilmiştir.)

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-vision-tutorial/connection-string.png" alt-text="Özel Görüntü İşleme genel bakış gösteren diyagram.":::
1. Ardından, kaynak/kenar/yapılandırma/deployment.customvision.amd64.jsüzerinde sağ tıklayın ve **tek cihaz Için dağıtım oluştur**' u seçin.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-vision-tutorial/deployment-amd64-json.png" alt-text="Özel Görüntü İşleme genel bakış gösteren diyagram.":::
1. Bundan sonra IoT Hub bir cihaz seçmeniz istenir. Aşağı açılan listeden **LVA-örnek-cihaz** ' ı seçin.
1. Yaklaşık 30 saniye içinde, sol alt bölümdeki Azure IoT Hub 'ını yenileyin. Aşağıdaki modüllerin dağıtıldığı uç cihazına sahip olmanız gerekir:

    * Adlı IoT Edge modülünde canlı video analizi `lvaEdge` .
    * Adlı bir modül `rtspsim` , canlı video akışı kaynağı görevi gören BIR RTSP sunucusuna benzetir.
    * Adında adı geçen bir modül, `cv` görüntülere özel görüntü işleme uygulanan ve birden çok etiket türü döndüren özel görüntü işleme oyunker algılama modelidir. (Modelimiz yalnızca bir etiket, teslim kamyonu üzerinde eğitildi.)

## <a name="prepare-for-monitoring-events"></a>İzleme olaylarını hazırlama

Canlı video analizi cihazına sağ tıklayın ve **Izlemeyi Başlat yerleşik olay uç noktası**' nı seçin. Visual Studio Code **Çıkış** penceresinde IoT Hub olaylarını izlemek için bu adıma ihtiyacınız vardır.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/custom-vision-tutorial/start-monitoring.png" alt-text="Özel Görüntü İşleme genel bakış gösteren diyagram.":::

## <a name="run-the-sample-program"></a>Örnek programı çalıştırma

Bu öğreticinin grafik topolojisini bir tarayıcıda açarsanız, değerinin `inferencingUrl` olarak ayarlandığını görürsünüz `http://cv:80/image` . Bu ayar, canlı videoda, varsa, çıkarım sunucusunun, varsa oyunpaketleri algılandıktan sonra sonuçlar döndürmeyeceği anlamına gelir.

1. Visual Studio Code, **Uzantılar** sekmesini açın (veya **CTRL + SHIFT + X**' i seçin) ve Azure IoT Hub aratın.
1. Sağ tıklayıp **uzantı ayarları**' nı seçin.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/run-program/extensions-tab.png" alt-text="Özel Görüntü İşleme genel bakış gösteren diyagram.":::
1. **Ayrıntılı Iletiyi göster**' i arayın ve etkinleştirin.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/run-program/show-verbose-message.png" alt-text="Özel Görüntü İşleme genel bakış gösteren diyagram."
              }
            ]
          }
        }
   ```
    
   * `GraphInstanceActivate`Grafik örneğini ve videonun akışını başlatan öğesine yönelik bir çağrı.
   * `GraphInstanceList`Grafik örneğinin çalışır durumda olduğunu gösteren ikinci bir çağrı.
    
1. **TERMINAL** penceresindeki çıktı, bir **ENTER tuşuna basarak komut istemine devam edebilir** . Henüz **ENTER** ' ı seçmeyin. Doğrudan çağrdığınız yöntemler için JSON yanıtı yüklerini görmek üzere yukarı kaydırın.
1. Visual Studio Code **Çıkış** penceresine geçin. IoT Edge modülündeki canlı video analizinin IoT Hub 'ına gönderdiğini iletiler görürsünüz. Bu öğreticinin aşağıdaki bölümünde bu iletiler ele alınmaktadır.
1. Medya grafiği çalışmaya devam eder ve sonuçları yazdırır. RTSP simülatörü kaynak videoyu döngüye sokmaya devam eder. Medya grafiğini durdurmak için, **TERMINAL** penceresine dönün ve **ENTER**' u seçin.
Sonraki çağrı dizisi kaynakları temizler:
    
   * `GraphInstanceDeactivate`Grafik örneğini devre dışı bırakmak için bir çağrı.
   * `GraphInstanceDelete`Örneği silme çağrısı.
   * İçin bir çağrı `GraphTopologyDelete` , topolojiyi siler.
   * İçin nihai bir çağrı `GraphTopologyList` , listenin boş olduğunu gösterir.
    
## <a name="interpret-the-results"></a>Sonuçları yorumlama

Medya grafiğini çalıştırdığınızda, HTTP uzantısı işlemci düğümünün sonuçları IoT Hub 'ına IoT Hub havuz düğümünden geçer. **Çıktı** penceresinde gördüğünüz iletiler bir gövde bölümü ve bir `applicationProperties` bölümü içerir. Daha fazla bilgi için bkz. [IoT Hub Iletileri oluşturma ve okuma](../../iot-hub/iot-hub-devguide-messages-construct.md).

Aşağıdaki iletilerde, canlı video analizi modülü, uygulama özelliklerini ve gövdenin içeriğini tanımlar.

### <a name="mediasessionestablished-event"></a>Mediasessionkurulduğu olayı

Bir medya grafiği oluşturulduğunda, RTSP kaynak düğümü, rtspsim-live555 kapsayıcısında çalışan RTSP sunucusuna bağlanmaya çalışır. Bağlantı başarılı olursa, aşağıdaki olay yazdırılır. Olay türü `Microsoft.Media.MediaGraph.Diagnostics.MediaSessionEstablished` .

```
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1599412849822466 1 IN IP4 172.18.0.3\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/t2.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.04.12\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-78.357\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/t2.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=42C01F;sprop-parameter-sets=Z0LAH9kAUAW6EAAAAwAQAAADAwDxgySA,aMuBcsg=\r\na=control:track1\r\nm=audio 0 RTP/AVP 97\r\nc=IN IP4 0.0.0.0\r\nb=AS:96\r\na=rtpmap:97 MPEG4-GENERIC/44100/2\r\na=fmtp:97 streamtype=5;profile-level-id=1;mode=AAC-hbr;sizelength=13;indexlength=3;indexdeltalength=3;config=121056E500\r\na=control:track2\r\n"
  },
  "applicationProperties": {
    "topic": "/subscriptions/35c2594a-23da-4fce-b59c-f6fb9513eeeb/resourceGroups/lvaavi_0827/providers/microsoft.media/mediaservices/lvasampleulf2rv2x5msy2",
    "subject": "/graphInstances/ GRAPHINSTANCENAMEHERE /sources/rtspSource",
    "eventType": "Microsoft.Media.Graph.Diagnostics.MediaSessionEstablished",
    "eventTime": "2020-09-06T17:20:49.823Z",
    "dataVersion": "1.0"
  }
```

Bu iletide, bu ayrıntılara dikkat edin:

* İleti bir tanılama olayıdır. `MediaSessionEstablished` RTSP kaynak düğümünün (konu) RTSP simülatörü ile bağlandığını ve sanal bir canlı akışı almaya başladığını gösterir.
* `applicationProperties`' De, `subject` iletinin medya grafiğindeki RTSP kaynak düğümünden oluşturulduğunu gösterir.
* `applicationProperties`' De, olay türü bu olayın bir tanılama olayı olduğunu gösterir.
* Olay saati, olayın gerçekleştiği saati gösterir.
* Gövde, tanılama olayı hakkındaki verileri içerir. Bu durumda, veriler [oturum açıklaması Protokolü (SDP)](https://en.wikipedia.org/wiki/Session_Description_Protocol) ayrıntılarını içerir.

### <a name="inference-event"></a>Çıkarım olayı

HTTP uzantısı işlemci düğümü Özel Görüntü İşleme kapsayıcısından çıkarım sonuçları alır ve sonuçları, çıkarım olayları olarak IoT Hub havuz düğümü aracılığıyla yayar.

```
{
  "body": {
    "created": "2020-05-18T01:08:34.483Z",
    "id": "",
    "iteration": "",
    "predictions": [
      {
        "boundingBox": {
          "height": 0.06219418,
          "left": 0.14977954,
          "top": 0.65847444,
          "width": 0.01879117
        },
        "probability": 0.69806677,
        "tagId": 0,
        "tagName": "delivery truck"
      },
      {
        "boundingBox": {
          "height": 0.1148454,
          "left": 0.77430711,
          "top": 0.54488315,
          "width": 0.11315252
        },
        "probability": 0.29394844,
        "tagId": 0,
        "tagName": "delivery truck"
      },
      {
        "boundingBox": {
          "height": 0.03187029,
          "left": 0.62644471,
          "top": 0.59640052,
          "width": 0.01582896
        },
        "probability": 0.14435098,
        "tagId": 0,
        "tagName": "delivery truck"
      },
      {
        "boundingBox": {
          "height": 0.11421723,
          "left": 0.72737214,
          "top": 0.55907888,
          "width": 0.12627538
        },
        "probability": 0.1141128,
        "tagId": 0,
        "tagName": "delivery truck"
      },
      {
        "boundingBox": {
          "height": 0.04717135,
          "left": 0.66516746,
          "top": 0.34617995,
          "width": 0.03802613
        },
        "probability": 0.10461298,
        "tagId": 0,
        "tagName": "delivery truck"
      },
      {
        "boundingBox": {
          "height": 0.20650843,
          "left": 0.56349564,
          "top": 0.51482571,
          "width": 0.35045707
        },
        "probability": 0.10056595,
        "tagId": 0,
        "tagName": "delivery truck"
      }
    ],
    "project": ""
  },
  "applicationProperties": {
    "topic": "/subscriptions/35c2594a-23da-4fce-b59c-f6fb9513eeeb/resourceGroups/lsravi/providers/microsoft.media/mediaservices/lvasamplerc3he6a7uhire",
    "subject": "/graphInstances/GRAPHINSTANCENAMEHERE/processors/httpExtension",
    "eventType": "Microsoft.Media.Graph.Analytics.Inference",
    "eventTime": "2020-05-18T01:08:34.038Z"
  }
}
```

Önceki iletilerde aşağıdaki bilgileri aklınızda edin:

* İçindeki konu, `applicationProperties` iletinin oluşturulduğu MediaGraph içindeki düğüme başvurur. Bu durumda, ileti HTTP uzantısı işlemcisinden kaynaklanır.
* İçindeki olay türü, `applicationProperties` bunun bir analiz çıkarım olayı olduğunu gösterir.
* Olay saati, olayın gerçekleştiği saati gösterir.
* Gövde, analiz olayı hakkındaki verileri içerir. Bu durumda, olay bir çıkarım olayıdır, bu nedenle gövde tahmin olarak adlandırılan bir Inse dizisi içerir.
* Tahmine dayalı bölüm, bir oyuncak teslimatı kamyonu (etiket "teslim kamyonu") çerçevede bulunduğu tahminlerden oluşan bir liste içerir. Geri çekmeniz sırasında, "teslim kamyonu" oyuncak için özel eğitilen modelinize verdiğiniz özel bir etikettir. Model, giriş videosunda oyuncak kamyonu farklı olasılık güvenilirliği puanlarına göre belirler ve tanımlar.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer öğreticileri veya hızlı başlangıçlarını denemek istiyorsanız, oluşturduğunuz kaynaklara açık tutun. Aksi takdirde Azure portal gidin, Kaynak gruplarınızı inceleyin, bu öğreticiyi çalıştırdığınız kaynak grubunu seçin ve tüm kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

Gelişmiş kullanıcılar için ek güçlükleri gözden geçirin:

* RTSP simülatörü kullanmak yerine RTSP desteği olan bir [IP kamerası](https://en.wikipedia.org/wiki/IP_camera) kullanın. [ONVIF uyumlu](https://www.onvif.org/conformant-products/) ürünler sayfasında RTSP 'YI destekleyen IP kameralarını arayabilirsiniz. Profiller G, S veya T ile uyumlu olan cihazları arayın.
* Azure Linux VM yerine AMD64 veya x64 Linux cihazı kullanın. Bu cihaz, IP kamerası ile aynı ağda olmalıdır. [Linux üzerinde Azure IoT Edge çalışma zamanını Install](../../iot-edge/how-to-install-iot-edge-linux.md)bölümündeki yönergeleri izleyebilirsiniz.

Ardından, [ilk IoT Edge modülünüzü bir sanal Linux cihazına dağıtma](../../iot-edge/quickstart-linux.md)konusundaki yönergeleri Izleyerek cihazı Azure IoT Hub kaydettirin.
