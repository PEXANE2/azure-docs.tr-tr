---
title: IoT Edge ve Azure Özel Görüntü İşleme canlı video analiziyle canlı videoyu çözümleyin
description: Özel Görüntü İşleme kullanarak, bir oyunsuna ve canlı video analizine ait canlı IoT Edge video analizine ait AI genişletilebilirliği özelliğini kullanarak, canlı video akışından oyungeleks 'i tespit etmek için modeli bir kenara dağıtabilirsiniz.
ms.topic: tutorial
ms.date: 09/08/2020
ms.openlocfilehash: e77521765156a13f0675602ffd0b39f78d8957bb
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92016816"
---
# <a name="tutorial-analyze-live-video-with-live-video-analytics-on-iot-edge-and-azure-custom-vision"></a>Öğretici: IoT Edge ve Azure Özel Görüntü İşleme canlı video analizi ile canlı videoyu çözümleyin

Bu öğreticide, bir oyunsuna algılayıp, canlı video analizinden gelen oyunpaketleri algılamak için modeli bir kenara dağıtmak üzere IoT Edge ' de canlı video analizlerinin [AI genişletilebilirliği özelliğini](analyze-live-video-concept.md#analyzing-video-using-a-custom-vision-model) kullanarak, bir oyunsuna algılayan bir model oluşturmak için [özel görüntü işleme](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/) kullanmayı öğreneceksiniz.

Özel Görüntü İşleme 'ın gücünü nasıl bir araya getirecağınızı göstereceğiz. Bu, herkesin, bir veri bilimi, ML veya AI bilgisi olmadan, canlı video analizlerinin yanı sıra, özel bir modeli kenarda bir kapsayıcı olarak kolayca dağıtmaları ve sanal bir canlı video akışını analiz etmesine olanak tanır. Bu öğretici, bir Azure VM 'yi IoT Edge bir cihaz olarak kullanır, C# dilinde yazılan örnek koda dayalıdır ve [hareket ve yayma olaylarını Algıla](detect-motion-emit-events-quickstart.md) hızlı başlangıç bölümünde oluşturulur.

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * İlgili kaynakları ayarlayın.
> * Bulutta bir Özel Görüntü İşleme model oluşturun ve oyuncak paketleri bir kenara dağıtın
> * Özel Vision modeline http Uzantısı ile bir medya grafiği oluşturun ve dağıtın
> * Örnek kodu çalıştırın.
> * Sonuçları inceleyin ve yorumlayın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="suggested-pre-reading"></a>Önerilen önceden okuma  

Başlamadan önce aşağıdaki makaleleri okumanız önerilir: 

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
    
    * [Azure kaynaklarını ayarlama](detect-motion-emit-events-quickstart.md#set-up-azure-resources)
    * [Geliştirme ortamınızı kurma](detect-motion-emit-events-quickstart.md#set-up-your-development-environment)

## <a name="review-the-sample-video"></a>Örnek videoyu gözden geçirin

Bu öğreticide, canlı bir akışın benzetimini yapmak için bir [oyuncar bir video](https://lvamedia.blob.core.windows.net/public/t2.mkv) dosyası kullanılmaktadır. [VLC medya oynatıcı](https://www.videolan.org/vlc/)gibi bir uygulama aracılığıyla videoyu inceleyebilirsiniz. CTRL + N ' ı seçin ve ardından kayıttan yürütmeyi başlatmak üzere [oyuncar arabasının videosunu](https://lvamedia.blob.core.windows.net/public/t2.mkv) bir bağlantı yapıştırın. Videoyu izlerken videoda bir oyunsuna 36 saniyelik işaret olduğunu unutmayın. Özel model bu oyuncak kamyonu algılamak için eğitildi. Bu öğreticide, bu tür oyunları algılamak ve ilişkili çıkarım olaylarını IoT Edge hub 'a yayımlamak için IoT Edge üzerinde canlı video analizi kullanacaksınız.

## <a name="overview"></a>Genel Bakış

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/custom-vision-tutorial/topology-custom-vision.svg" alt-text="Özel Görüntü İşleme genel bakış":::

Bu diyagramda, sinyallerin Bu öğreticide nasıl akagösterdiği gösterilmektedir. [Edge modülü](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) , Real-Time Akış Protokolü (RTSP) sunucusunu BARıNDıRAN bir IP kamerasına benzetir. Bir [RTSP kaynak](media-graph-concept.md#rtsp-source) düğümü, bu sunucudan video akışını çeker ve [çerçeve hızı filtre işlemcisi](media-graph-concept.md#frame-rate-filter-processor) düğümüne video çerçeveleri gönderir. Bu işlemci, [http uzantısı işlemci](media-graph-concept.md#http-extension-processor) düğümüne ulaşan video akışının kare oranını sınırlandırır.
HTTP uzantısı düğümü bir ara sunucu rolünü yürütür. Video çerçevelerini belirtilen görüntü türüne dönüştürür. Ardından, görüntüyü REST üzerinden bir HTTP uç noktasının arkasında bulunan bir AI modeli çalıştıran başka bir Edge modülüne geçirir. Bu örnekte, bu Edge modülü Özel Görüntü İşleme kullanılarak oluşturulan oyunker algılayıcı modelidir. HTTP uzantısı işlemci düğümü, algılama sonuçlarını toplar ve olayları [IoT Hub havuz](media-graph-concept.md#iot-hub-message-sink) düğümüne yayımlar. Düğüm daha sonra bu olayları [IoT Edge hub 'ına](../../iot-edge/iot-edge-glossary.md#iot-edge-hub)gönderir.

## <a name="build-and-deploy-a-custom-vision-toy-detection-model"></a>Özel Görüntü İşleme oyunnı algılama modeli oluşturun ve dağıtın 

Ad Özel Görüntü İşleme önerdiğinde, bulutta kendi özel nesne algılayıcısının veya sınıflandırıcınızı oluşturmak için onu kullanabilirsiniz. Bulutta veya bir kenara kapsayıcılar aracılığıyla dağıtılabilecek özel Vision modelleri oluşturmak için basit, kullanımı kolay ve sezgisel bir arabirim sağlar. 

Bir oyunker algılayıcısı oluşturmak için, bu özel vizyonun Web portalı [hızlı başlangıç makalesi](../../cognitive-services/custom-vision-service/get-started-build-detector.md) aracılığıyla bir nesne algılayıcısının derlemesini izlemenizi öneririz.

Ek notlar:
 
* Bu öğretici için, hızlı başlangıç makalesinin [Önkoşul bölümünde](../../cognitive-services/custom-vision-service/get-started-build-detector.md#prerequisites)sunulan örnek görüntüleri kullanmayın. Bunun yerine, bir oyunalgılayıcısı özel bakış modeli oluşturmak için belirli bir görüntü kümesi yararlanılabilir, hızlı başlangıçta [eğitim görüntülerinizi seçmeniz](../../cognitive-services/custom-vision-service/get-started-build-detector.md#choose-training-images) istendiğinde [Bu görüntüleri](https://lvamedia.blob.core.windows.net/public/ToyCarTrainingImages.zip) kullanmanızı öneririz.
* Hızlı başlangıç aracının etiketleme resmi bölümünde, resimde görülen oyunu kamyonu "teslim kamyonu" etiketiyle etiketleyerek lütfen emin olun.

İşiniz bittiğinde, model memnuniyet uyarınca hazırsanız, performans sekmesindeki dışarı aktar düğmesini kullanarak bir Docker kapsayıcısına dışarı aktarabilirsiniz. Lütfen kapsayıcı platformu türü olarak Linux 'u seçtiğinizden emin olun. Bu, kapsayıcının çalışacağı platformdur. Kapsayıcıyı yüklediğiniz makine Windows veya Linux olabilir. Aşağıdaki yönergeler, bir Windows makinesine indirilen kapsayıcı dosyasını temel alır.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/custom-vision-tutorial/docker-file.png" alt-text="Özel Görüntü İşleme genel bakış"   13 hours ago        Up 25 seconds       127.0.0.1:80->80/tcp   practical_cohen
        ```
      1. `curl -X POST http://127.0.0.1:80/image -F imageData=@<path to any image file that has the toy delivery truck in it>`
            
            Bu komut, kapsayıcıyı yerel makinede sınar ve görüntüyü üzerinde eğitidiğimiz görüntüde aynı teslimat kamyonu varsa çıktının aşağıdakine benzer olması gerekir. Bu durumda, teslimat kamyonu% 90,12 olasılığa göre algılandı.
    
            ```
            {"created":"2020-03-20T07:10:47.827673","id":"","iteration":"","predictions":[{"boundingBox":{"height":0.66167289,"left":-0.03923762,"top":0.12781593,"width":0.70003178},"probability":0.90128148,"tagId":0,"tagName":"delivery truck"},{"boundingBox":{"height":0.63733053,"left":0.25220079,"top":0.0876643,"width":0.53331227},"probability":0.59745145,"tagId":0,"tagName":"delivery truck"}],"project":""}
            ```

<!--## Create and deploy a media graph with http extension to custom vision model-->

## <a name="examine-the-sample-files"></a>Örnek dosyaları inceleyin

1. VSCode 'da "src/Edge" e gidin. Oluşturduğunuz. env dosyasını, birkaç dağıtım şablonu dosyası ile birlikte görürsünüz.

    Dağıtım şablonu, kenar cihazının bazı yer tutucu değerleriyle birlikte dağıtım bildirimini ifade eder. . Env dosyası bu değişkenlerin değerlerini içerir.
1. Sonra, "src/buluttan-cihaza-Console-App" klasörüne gidin. Burada, oluşturduğunuz dosyada, diğer birkaç dosyayla birlikte appsettings.jsgörürsünüz:

    * C2D-Console-App. csproj-bu, VSCode için proje dosyasıdır.
    * operations.json-bu dosya, programın çalıştırmasını istediğiniz farklı işlemleri listeler.
    * Program.cs-Bu örnek program kodudur ve şunları yapar:

        * Uygulama ayarlarını yükler.
        * Topoloji oluşturmak için IoT Edge modülün doğrudan yöntemlerinde canlı video analizlerini çağırın, grafiğin örneğini oluşturun ve grafiği etkinleştirin.
        * , TERMINAL penceresinde grafik çıkışını ve çıkış penceresinde IoT Hub 'a gönderilen olayları incelemenizi sağlar.
        * Grafik örneğini devre dışı bırakın, grafik örneğini silin ve grafik topolojisini silin.
        
## <a name="generate-and-deploy-the-deployment-manifest"></a>Dağıtım bildirimini oluşturma ve dağıtma

1. VSCode 'da "src/Cloud-cihaza-Console-App/operations.json" bölümüne gidin

1. GraphTopologySet altında aşağıdakilerin doğru olduğundan emin olun:<br/>`"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/httpExtension/topology.json"`
1. Graphınstanceset altında şunları doğrulayın: 
    1. "topologyName": "ınıngencingwithhttpextension"
    1. Aşağıdaki parametreleri dizi parametrelerinin üstüne ekleyin- `{"name": "inferencingUrl","value": "http://cv:80/image"},`
    1. RtspUrl parametre değerini – "rtsp://rtspsim:554/media/t2.mkv" olarak değiştirin    
1. GraphTopologyDelete altında "Name": "ınıngencingwithhttpextension" olduğundan emin olun
1. "Src/Edge/deployment.customvision.template.json" dosyasına sağ tıklayın ve **IoT Edge dağıtım bildirimi oluştur**' a tıklayın.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-vision-tutorial/deployment-template-json.png" alt-text="Özel Görüntü İşleme genel bakış" simgesine tıklayarak ıothub bağlantı dizesini ayarlayın. Dizeyi dosyadaki appsettings.jskopyalayabilirsiniz. ( [IoT Hub 'ı Seç komutu](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Select-IoT-Hub)aracılığıyla vscode içinde yapılandırılmış uygun IoT Hub sahip olduğunuzdan emin olmak için başka bir önerilen yaklaşım aşağıda verilmiştir).

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-vision-tutorial/connection-string.png" alt-text="Özel Görüntü İşleme genel bakış" öğesine sağ tıklayın ve **tek cihaz Için dağıtım oluştur**' a tıklayın. 

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-vision-tutorial/deployment-amd64-json.png" alt-text="Özel Görüntü İşleme genel bakış" olarak adlandırılan canlı video analizi.
    * Adlı bir modül `rtspsim` , canlı video akışı kaynağı olarak davranan BIR RTSP sunucusunun benzetimini yapar.
    * Adında gösterildiği gibi bir modül, `cv` görüntülere özel vizyon uygulayan ve birden çok etiket türü döndüren özel görüntü işleme oyunker algılama modelidir. (Modeliniz yalnızca bir etikette eğitildi: "teslim kamyonu").

## <a name="prepare-for-monitoring-events"></a>İzleme olaylarını hazırlama

Canlı video analizi cihazına sağ tıklayın ve **Izlemeyi Başlat yerleşik olay uç noktası**' nı seçin. Visual Studio Code çıkış penceresinde IoT Hub olaylarını izlemek için bu adıma ihtiyacınız vardır.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/custom-vision-tutorial/start-monitoring.png" alt-text="Özel Görüntü İşleme genel bakış":::

## <a name="run-the-sample-program"></a>Örnek programı çalıştırma

Bu öğreticinin grafik topolojisini bir tarayıcıda açarsanız, ınıngencingurl değerinin olarak ayarlandığını görürsünüz. Bu, http://cv:80/image çıkarım sunucusunun canlı videoda, varsa oyunpaketleri algılandıktan sonra sonuçlar döndürmesi anlamına gelir.

1. Visual Studio Code, **Uzantılar** sekmesini açın (veya CTRL + SHIFT + X tuşlarına basın) ve Azure IoT Hub aratın.
1. Sağ tıklayıp **uzantı ayarları**' nı seçin.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/run-program/extensions-tab.png" alt-text="Özel Görüntü İşleme genel bakış" i arayın ve etkinleştirin.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/run-program/show-verbose-message.png" alt-text="Özel Görüntü İşleme genel bakış"
              }
            ]
          }
        }
   ```
    
   * Grafik örneğini ve videonun akışını başlatan Graphınstanceactivate öğesine yönelik bir çağrı.
   * Grafik örneğinin çalışır durumda olduğunu gösteren Graphınstancelist öğesine yapılan ikinci bir çağrı.
    
1. TERMINAL penceresindeki çıktı, bir ENTER tuşuna basarak komut istemine devam edebilir. Henüz ENTER ' ı seçmeyin. Doğrudan çağrdığınız yöntemler için JSON yanıtı yüklerini görmek üzere yukarı kaydırın.
1. Visual Studio Code çıkış penceresine geçin. IoT Edge modülündeki canlı video analizinin IoT Hub 'ına gönderdiğini iletiler görürsünüz. Bu öğreticinin aşağıdaki bölümünde bu iletiler ele alınmaktadır.
1. Medya grafiği çalışmaya devam eder ve sonuçları yazdırır. RTSP simülatörü kaynak videoyu döngüye sokmaya devam eder. Medya grafiğini durdurmak için, TERMINAL penceresine dönün ve ENTER ' u seçin.
Sonraki çağrı dizisi kaynakları temizler:
    
   * Graphınstancedeactivate öğesine yapılan bir çağrı, grafik örneğini devre dışı bırakır.
   * Graphınstancedelete çağrısı örneği siler.
   * Graphtopologyıdelete çağrısı topolojiyi siler.
   * Graphtopologyılist öğesine yapılan son çağrı, listenin boş olduğunu gösterir.
    
## <a name="interpret-the-results"></a>Sonuçları yorumlama

Medya grafiğini çalıştırdığınızda, HTTP uzantısı işlemci düğümünün sonuçları IoT Hub 'ına IoT Hub havuz düğümünden geçer. ÇıKTı penceresinde gördüğünüz iletiler bir Body bölümü ve bir applicationProperties bölümü içerir. Daha fazla bilgi için bkz. [IoT Hub Iletileri oluşturma ve okuma](../../iot-hub/iot-hub-devguide-messages-construct.md).

Aşağıdaki iletilerde, canlı video analizi modülü, uygulama özelliklerini ve gövdenin içeriğini tanımlar.

### <a name="mediasessionestablished-event"></a>Mediasessionkurulduğu olayı

Bir medya grafiği oluşturulduğunda, RTSP kaynak düğümü, rtspsim-live555 kapsayıcısında çalışan RTSP sunucusuna bağlanmaya çalışır. Bağlantı başarılı olursa, aşağıdaki olay yazdırılır. Olay türü Microsoft. Media. MediaGraph. Diagnostics. Mediasessionsetup ' tur.

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

* İleti bir tanılama olayıdır. Mediasessionkuruldu, RTSP kaynak düğümünün (konu) RTSP simülatörü ile bağlandığını ve (benzetimli) canlı akışını almaya başladığını gösterir.
* ApplicationProperties içinde, ilgili iletinin medya grafiğindeki RTSP kaynak düğümünden oluşturulduğunu gösterir.
* ApplicationProperties içinde, eventType bu olayın bir tanılama olayı olduğunu gösterir.
* EventTime, olayın gerçekleştiği saati gösterir.
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

Yukarıdaki iletilerde aşağıdakileri göz önünde edin:

* ApplicationProperties 'teki konu, iletinin oluşturulduğu MediaGraph içindeki düğüme başvurur. Bu durumda, ileti http uzantısı işlemcisinin kaynağı olur.
* ApplicationProperties 'teki eventType, bunun bir analiz çıkarım olayı olduğunu gösterir.
* EventTime, olayın gerçekleştiği saati gösterir.
* "gövde" analiz olayı hakkındaki verileri içerir. Bu durumda, olay bir çıkarım olayıdır ve bu nedenle gövde "tahmine dayalı" olarak adlandırılan bir dizi dereceden oluşur.
* "Öngörüler" bölümünde, oyuntavanın teslim kamyonu (Tag = teslim kamyonu) çerçevede bulunduğu tahminler listesi bulunur. Hatırlayacağınız gibi, teslim kamyonu, oyuncak için özel eğitilen modelinize verdiğiniz özel bir etikettir ve model, giriş videosunda farklı olasılık güvenilirliği puanlarına sahip oyunnu kamyonu tanımlar.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer öğreticileri veya hızlı başlangıçlarını denemek istiyorsanız oluşturulan kaynaklarda tutmanız gerekir. Aksi takdirde Azure portal gidin, Kaynak gruplarınızı inceleyin, bu öğreticiyi çalıştırdığınız kaynak grubunu seçin ve tüm kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

Gelişmiş kullanıcılar için ek güçlükleri gözden geçirin:

* RTSP simülatörü kullanmak yerine RTSP desteği olan bir [IP kamerası](https://en.wikipedia.org/wiki/IP_camera) kullanın. [ONVIF uyumlu](https://www.onvif.org/conformant-products/) ürünler sayfasında RTSP 'YI destekleyen IP kameralarını arayabilirsiniz. Profiller G, S veya T ile uyumlu olan cihazları arayın.
* Azure Linux VM yerine AMD64 veya x64 Linux cihazı kullanın. Bu cihaz, IP kamerası ile aynı ağda olmalıdır. [Linux üzerinde Azure IoT Edge çalışma zamanını Install](../../iot-edge/how-to-install-iot-edge-linux.md)bölümündeki yönergeleri izleyebilirsiniz. 

Ardından, [ilk IoT Edge modülünüzü bir sanal Linux cihazına dağıtma](../../iot-edge/quickstart-linux.md)konusundaki yönergeleri Izleyerek cihazı Azure IoT Hub kaydettirin.