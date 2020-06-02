---
title: IoT Edge Azure 'da canlı video analiziyle çalışmaya başlama
description: Bu hızlı başlangıçta IoT Edge 'da canlı video analiziyle çalışmaya başlama ve canlı video akışında hareket algılama gösterilmektedir.
ms.topic: quickstart
ms.date: 04/27/2020
ms.openlocfilehash: 307a81938be3e25b8a6a07bb3696ca3b7647c0aa
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84262017"
---
# <a name="quickstart-get-started---live-video-analytics-on-iot-edge"></a>Hızlı başlangıç: IoT Edge kullanmaya başlama-canlı video analizi

Bu hızlı başlangıç, IoT Edge üzerinde canlı video analizlerini kullanmaya başlamanıza yönelik adımlarda size yol gösterir. Bir Azure VM 'yi IoT Edge bir cihaz ve sanal bir canlı video akışı olarak kullanır. Kurulum adımlarını tamamladıktan sonra, bu akıştaki tüm hareketleri algılayan ve raporlayan bir medya grafiğiyle sanal bir canlı video akışı çalıştırabileceksiniz. Aşağıdaki diyagramda, bu medya grafiğinin grafik temsili gösterilmektedir.

![Hareket algılamayı temel alan canlı video analizi](./media/analyze-live-video/motion-detection.png)

## <a name="prerequisites"></a>Ön koşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Azure IoT araçları uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)ile geliştirme makinenizde [Visual Studio Code](https://code.visualstudio.com/) .
* Geliştirme makinenizin bağlı olduğu ağ, 5671 numaralı bağlantı noktası üzerinden AMQP protokolüne izin vermesi gerekir (Azure IoT araçları 'nın Azure IoT Hub ile iletişim kurabilmesi için).

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
    
Betik başarıyla tamamlanırsa, aboneliğinizde yukarıda bahsedilen tüm kaynakları görmeniz gerekir. Betik çıktısının bir parçası olarak, IoT Hub adını listelemek için kaynak tablosu oluşturulacaktır. **"Microsoft. Devices/IotHubs"** kaynak türünü arayın ve adı aklınızda bulun. Bu, bir sonraki adımda gerekli olacaktır. Komut dosyası, ~/CloudDrive/LVA-Sample/Directory içinde birkaç yapılandırma dosyası da oluşturur; Bu işlem hızlı başlangıçta daha sonra gerekecektir.

## <a name="deploy-modules-on-your-edge-device"></a>Sınır cihazınızda modüller dağıtma

Aşağıdaki komutu çalıştırın Cloud Shell

```
az iot edge set-modules --hub-name <iot-hub-name> --device-id lva-sample-device --content ~/clouddrive/lva-sample/edge-deployment/deployment.amd64.json
```

Yukarıdaki komut, Edge cihazına (Linux VM) aşağıdaki modülleri dağıtır:

* IoT Edge (modül adı "lvaEdge") üzerinde canlı video analizi
* RTSP simülatör (modül adı "rtspsim")

RTSP simülatör modülü, [canlı video analizi kaynakları kurulum betiğini](https://github.com/Azure/live-video-analytics/tree/master/edge/setup)çalıştırdığınızda Edge cihazınıza kopyalanmış olan bir video dosyası kullanarak canlı bir video akışının benzetimini yapar. Bu aşamada, modüller dağıtıldı, ancak hiçbir medya grafiği etkin değil.

## <a name="configure-azure-iot-tools-extension-in-visual-studio-code"></a>Visual Studio Code 'de Azure IoT araçları uzantısını yapılandırma

Visual Studio Code başlatın ve Azure IoT araçları uzantısını kullanarak Azure IoT Hub bağlanmak için aşağıdaki yönergeleri izleyin.

1. **Görünüm**Gezgini aracılığıyla Visual Studio Code gezgin sekmesine gidin  >  **Explorer** veya basitçe (Ctrl + Shift + E) tuşuna basın.
1. Gezgin sekmesinde, sol alt köşedeki "Azure IoT Hub" öğesine tıklayın.
1. Daha fazla seçenek simgesine tıklayarak bağlam menüsünü görüntüleyin ve "IoT Hub bağlantı dizesi ayarla" seçeneğini belirleyin.
1. Bir giriş kutusu açılır ve sonra IoT Hub bağlantı dizenizi girin. IoT Hub için bağlantı dizesini Cloud Shell ~/CloudDrive/LVA-Sample/appSettings.JSON konumundan alabilirsiniz.
1. Bağlantı başarılı olursa, sınır cihazlarının listesi gösterilir. "LVA-Sample-Device" adlı en az bir cihaz olmalıdır.
1. Artık IoT Edge cihazlarınızı yönetebilir ve bağlam menüsü aracılığıyla Azure IoT Hub etkileşim kurabilirsiniz.
1. "LVA-örnek-cihaz" altındaki modüller düğümünü genişleterek, Edge cihazında dağıtılan modülleri görüntüleyebilirsiniz.

    ![LVA-örnek-cihaz düğümü](./media/quickstarts/lva-sample-device-node.png)

## <a name="use-direct-methods"></a>Doğrudan yöntemler kullanma

Doğrudan yöntemleri çağırarak canlı video akışlarını çözümlemek için modülünü kullanabilirsiniz. Modülün sunduğu tüm doğrudan yöntemleri anlamak için [IoT Edge üzerindeki canlı video analizlerine yönelik doğrudan yöntemleri](direct-methods.md) okuyun. 

### <a name="invoke-graphtopologylist"></a>Graphtopologyılist komutunu çağır
Bu, modüldeki tüm [grafik topolojilerini](media-graph-concept.md#media-graph-topologies-and-instances) numaralandırır.

1. "LvaEdge" modülüne sağ tıklayıp bağlam menüsünden "modül doğrudan yöntemini çağır" seçeneğini belirleyin.
1. Visual Studio Code penceresinin üst ortasında bir düzenleme kutusu açılır penceresi görürsünüz. Düzenle kutusuna "Graphtopologyılist" yazın ve ENTER tuşuna basın.
1. Sonra, aşağıdaki JSON yükünü kopyalayıp düzenleme kutusuna yapıştırın ve ENTER tuşuna basın.

    ```
    {
        "@apiVersion" : "1.0"
    }
    ```

    Birkaç saniye içinde, aşağıdaki Yanıt ile Visual Studio Code açılır penceresinde çıkış penceresini görürsünüz

    ```
    [DirectMethod] Invoking Direct Method [GraphTopologyList] to [lva-sample-device/lvaEdge] ...
    [DirectMethod] Response from [lva-sample-device/lvaEdge]:
    {
      "status": 200,
      "payload": {
        "value": []
      }
    }
    ```
    
    Grafik topolojisi oluşturulmadığından yukarıdaki yanıt beklenmez.
    

### <a name="invoke-graphtopologyset"></a>Graphtopologyıset komutunu çağır

Graphtopologyılist ' i çağırmak için özetlenen adımlarla aynı adımları kullanarak, yük olarak aşağıdaki JSON 'u kullanarak bir [Graph topolojisi](media-graph-concept.md#media-graph-topologies-and-instances) ayarlamak Için Graphtopologyıset komutunu çağırabilirsiniz.

```
{
    "@apiVersion": "1.0",
    "name": "MotionDetection",
    "properties": {
        "description": "Analyzing live video to detect motion and emit events",
        "parameters": [
            {
                "name": "rtspUserName",
                "type": "String",
                "description": "rtsp source user name.",
                "default": "dummyUserName"
            },
            {
                "name": "rtspPassword",
                "type": "String",
                "description": "rtsp source password.",
                "default": "dummyPassword"
            },
            {
                "name": "rtspUrl",
                "type": "String",
                "description": "rtsp Url"
            }
        ],
        "sources": [
            {
                "@type": "#Microsoft.Media.MediaGraphRtspSource",
                "name": "rtspSource",
                "endpoint": {
                    "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
                    "url": "${rtspUrl}",
                    "credentials": {
                        "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
                        "username": "${rtspUserName}",
                        "password": "${rtspPassword}"
                    }
                }
            }
        ],
        "processors": [
            {
                "@type": "#Microsoft.Media.MediaGraphMotionDetectionProcessor",
                "name": "motionDetection",
                "sensitivity": "medium",
                "inputs": [
                    {
                        "nodeName": "rtspSource"
                    }
                ]
            }
        ],
        "sinks": [
            {
                "@type": "#Microsoft.Media.MediaGraphIoTHubMessageSink",
                "name": "hubSink",
                "hubOutputName": "inferenceOutput",
                "inputs": [
                    {
                        "nodeName": "motionDetection"
                    }
                ]
            }
        ]
    }
}

```


Yukarıdaki JSON yükü, üç parametre tanımlayan (ikisi de varsayılan değerlere sahip) bir grafik topolojisi oluşturulmasına neden olur. Topolojide bir kaynak (RTSP kaynağı) düğümü, bir işlemci (hareket algılama işlemcisi) düğümü ve bir havuz (IoT Hub havuzu) düğümü vardır.

Birkaç saniye içinde çıkış penceresinde aşağıdaki yanıtı görürsünüz:

```
[DirectMethod] Invoking Direct Method [GraphTopologySet] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 201,
  "payload": {
    "systemData": {
      "createdAt": "2020-05-19T07:41:34.507Z",
      "lastModifiedAt": "2020-05-19T07:41:34.507Z"
    },
    "name": "MotionDetection",
    "properties": {
      "description": "Analyzing live video to detect motion and emit events",
      "parameters": [
        {
          "name": "rtspUserName",
          "type": "String",
          "description": "rtsp source user name.",
          "default": "dummyUserName"
        },
        {
          "name": "rtspPassword",
          "type": "String",
          "description": "rtsp source password.",
          "default": "dummyPassword"
        },
        {
          "name": "rtspUrl",
          "type": "String",
          "description": "rtsp Url"
        }
      ],
      "sources": [
        {
          "@type": "#Microsoft.Media.MediaGraphRtspSource",
          "name": "rtspSource",
          "transport": "Tcp",
          "endpoint": {
            "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
            "url": "${rtspUrl}",
            "credentials": {
              "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
              "username": "${rtspUserName}"
            }
          }
        }
      ],
      "processors": [
        {
          "@type": "#Microsoft.Media.MediaGraphMotionDetectionProcessor",
          "sensitivity": "medium",
          "name": "motionDetection",
          "inputs": [
            {
              "nodeName": "rtspSource",
              "outputSelectors": []
            }
          ]
        }
      ],
      "sinks": [
        {
          "@type": "#Microsoft.Media.MediaGraphIoTHubMessageSink",
          "hubOutputName": "inferenceOutput",
          "name": "hubSink",
          "inputs": [
            {
              "nodeName": "motionDetection",
              "outputSelectors": []
            }
          ]
        }
      ]
    }
  }
}
```

Döndürülen durum 201 ' dir ve yeni bir topoloji oluşturulduğunu gösterir. Sonraki adımlar olarak aşağıdakileri deneyin:

* Graphtopologyıset komutunu yeniden çağırın ve döndürülen durum kodu 200 olduğunu unutmayın. Durum kodu 200, var olan bir topolojinin başarıyla güncelleştirildiğini gösterir.
* Graphtopologyıset komutunu yeniden çağırın, ancak açıklama dizesini değiştirin. Yanıt içindeki durum kodu 200, açıklama ise yeni değere güncelleştirilir.
* Önceki bölümde özetlenen Graphtopologyılist dosyasını çağırın ve artık döndürülen yükün "MotionDetection" topolojisini görebileceğinizi unutmayın.

### <a name="invoke-graphtopologyget"></a>Graphtopologyıget çağırma

Şimdi aşağıdaki yük ile Graphtopologyıget komutunu çağır

```

{
    "@apiVersion" : "1.0",
    "name" : "MotionDetection"
}
```

Birkaç saniye içinde çıkış penceresinde aşağıdaki yanıtı görmeniz gerekir:

```
[DirectMethod] Invoking Direct Method [GraphTopologyGet] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": {
    "systemData": {
      "createdAt": "2020-05-19T07:41:34.507Z",
      "lastModifiedAt": "2020-05-19T07:41:34.507Z"
    },
    "name": "MotionDetection",
    "properties": {
      "description": "Analyzing live video to detect motion and emit events",
      "parameters": [
        {
          "name": "rtspUserName",
          "type": "String",
          "description": "rtsp source user name.",
          "default": "dummyUserName"
        },
        {
          "name": "rtspPassword",
          "type": "String",
          "description": "rtsp source password.",
          "default": "dummyPassword"
        },
        {
          "name": "rtspUrl",
          "type": "String",
          "description": "rtsp Url"
        }
      ],
      "sources": [
        {
          "@type": "#Microsoft.Media.MediaGraphRtspSource",
          "name": "rtspSource",
          "transport": "Tcp",
          "endpoint": {
            "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
            "url": "${rtspUrl}",
            "credentials": {
              "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
              "username": "${rtspUserName}"
            }
          }
        }
      ],
      "processors": [
        {
          "@type": "#Microsoft.Media.MediaGraphMotionDetectionProcessor",
          "sensitivity": "medium",
          "name": "motionDetection",
          "inputs": [
            {
              "nodeName": "rtspSource",
              "outputSelectors": []
            }
          ]
        }
      ],
      "sinks": [
        {
          "@type": "#Microsoft.Media.MediaGraphIoTHubMessageSink",
          "hubOutputName": "inferenceOutput",
          "name": "hubSink",
          "inputs": [
            {
              "nodeName": "motionDetection",
              "outputSelectors": []
            }
          ]
        }
      ]
    }
  }
}
```

Yanıt yükünde aşağıdakileri göz önünde edin:

* Durum kodu, başarıyı belirten 200 ' dir.
* Yükün "oluşturma" ve "lastModified" zaman damgası vardır.

### <a name="invoke-graphinstanceset"></a>Graphınstanceset 'i çağır

Ardından, yukarıdaki grafik topolojisine başvuran bir grafik örneği oluşturun. [Burada](media-graph-concept.md#media-graph-topologies-and-instances)açıklandığı gibi, grafik örnekleri aynı grafik topolojisine sahip birçok kameradan canlı video akışlarını analiz etmenize olanak tanır.

Aşağıdaki yük ile doğrudan yöntem Graphınstanceset öğesini çağırın.

```
{
    "@apiVersion" : "1.0",
    "name" : "Sample-Graph-1",
    "properties" : {
        "topologyName" : "MotionDetection",
        "description" : "Sample graph description",
        "parameters" : [
            { "name" : "rtspUrl", "value" : "rtsp://rtspsim:554/media/camera-300s.mkv" }
        ]
    }
}
```

Şunlara dikkat edin:

* Yukarıdaki yük, örneğin oluşturulması gereken topoloji adını (MotionDetection) belirtir.
* Yük, Graph topolojisi yükünde varsayılan bir değere sahip olmayan "rtspUrl" için parametre değeri içeriyor.

Birkaç saniye içinde çıkış penceresinde aşağıdaki yanıtı görürsünüz:

```
[DirectMethod] Invoking Direct Method [GraphInstanceSet] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 201,
  "payload": {
    "name": "Sample-Graph-1",
    "properties": {
      "created": "2020-05-19T07:44:33.868Z",
      "lastModified": "2020-05-19T07:44:33.868Z",
      "state": "Inactive",
      "description": "Sample graph description",
      "topologyName": "MotionDetection",
      "parameters": [
        {
          "name": "rtspUrl",
          "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
        }
      ]
    }
  }
}
```

Yanıt yükünde aşağıdakileri göz önünde edin:

* Durum kodu, yeni bir örneğin oluşturulduğunu belirten 201 ' dir.
* Durum, grafik örneğinin oluşturulduğunu ancak etkinleştirilmediğini gösteren "etkin değil" olarak belirlenir. Daha fazla bilgi için bkz. [medya grafiği durumları](media-graph-concept.md).

Sonraki adımlar olarak aşağıdakileri deneyin:

* Aynı yük ile Graphınstanceset komutunu yeniden çağırın ve döndürülen durum kodunun artık 200 olduğunu unutmayın.
* Graphınstanceset komutunu tekrar çağırın, ancak farklı bir açıklama ile, yanıt yükünde güncelleştirilmiş açıklamanın, grafik örneğinin başarıyla güncelleştirildiğini belirten bir açıklama olduğunu unutmayın.
* Graphınstanceset 'i çağırın, ancak adı "Sample-Graph-2" olarak değiştirin ve yanıt yükünü gözlemleyin. Yeni bir grafik örneği oluşturulduğunu (yani, durum kodu 201 olduğunu) unutmayın.

### <a name="invoke-graphinstanceactivate"></a>Graphınstanceactivate çağır

Şimdi, etkin video akışını modül aracılığıyla Başlatan grafik örneğini etkinleştirin. Aşağıdaki yük ile doğrudan yöntem Graphınstanceactivate öğesini çağırın.

```
{
    "@apiVersion" : "1.0",
    "name" : "Sample-Graph-1"
}
```

Birkaç saniye içinde çıkış penceresinde aşağıdaki yanıtı görmeniz gerekir:

```
[DirectMethod] Invoking Direct Method [GraphInstanceActivate] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": null
}
```

Yanıt yükünde 200 durum kodu, grafik örneğinin başarıyla etkinleştirildiğini gösterir.

### <a name="invoke-graphinstanceget"></a>Graphınstanceget 'i çağır

Şimdi aşağıdaki yük ile Graphınstanceget doğrudan metodunu çağırın:

```
 {
     "@apiVersion" : "1.0",
     "name" : "Sample-Graph-1"
 }
 ```

Birkaç saniye içinde çıkış penceresinde aşağıdaki yanıtı görmeniz gerekir:

```
[DirectMethod] Invoking Direct Method [GraphInstanceGet] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": {
    "name": "Sample-Graph-1",
    "properties": {
      "created": "2020-05-19T07:44:33.868Z",
      "lastModified": "2020-05-19T07:44:33.868Z",
      "state": "Active",
      "description": "graph description",
      "topologyName": "MotionDetection",
      "parameters": [
        {
          "name": "rtspUrl",
          "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
        }
      ]
    }
  }
}
```

Yanıt yükünde aşağıdakileri göz önünde edin:

* Durum kodu, başarıyı belirten 200 ' dir.
* Durum, grafik örneğinin Şu anda "etkin" durumda olduğunu gösteren "etkin" olarak belirlenir.

## <a name="observe-results"></a>Sonuçları gözlemleyin

Yukarıda oluşturduğumuz ve etkinleştirildiğimiz grafik örneği, gelen canlı video akışındaki hareketi algılamak ve olayları IoT Hub havuz düğümüne göndermek için hareket algılama işlemcisi düğümünü kullanır. Bu olaylar daha sonra gözlemlenebilir IoT Edge hub 'ınıza aktarılmıştır. Bunu yapmak için şu adımları uygulayın.

1. Visual Studio Code Gezgin bölmesini açın ve sol alt köşedeki Azure IoT Hub arayın.
2. Cihazlar düğümünü genişletin.
3. LVA-örnek-cihazındaki sağ-Clink ve "yerleşik olay Izlemeyi Izlemeye başla" seçeneğini seçti.

![IoT Hub olaylarını izlemeye başlama](./media/quickstarts/start-monitoring-iothub-events.png)

Çıkış penceresinde aşağıdaki iletiler görüntülenir:

```
[IoTHubMonitor] [7:44:33 AM] Message received from [lva-sample-device/lvaEdge]:
{
    "body": {
    "timestamp": 143005362606360,
    "inferences": [
        {
        "type": "motion",
        "motion": {
            "box": {
            "l": 0.828452,
            "t": 0.455224,
            "w": 0.1,
            "h": 0.088889
            }
        }
        },
        {
        "type": "motion",
        "motion": {
            "box": {
            "l": 0.661088,
            "t": 0.597015,
            "w": 0.0625,
            "h": 0.051852
            }
        }
        }
    ]
    },
    "applicationProperties": {
    "topic": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/microsoft.media/mediaservices/{amsAccountName}",
    "subject": "/graphInstances/Sample-Graph-1/processors/motionDetection",
    "eventType": "Microsoft.Media.Graph.Analytics.Inference",
    "eventTime": "2020-05-19T07:45:34.404Z",
    "dataVersion": "1.0"
    }
}
```

Yukarıdaki iletide aşağıdakilere göz önünde

* İleti bir "Body" bölümü ve bir "applicationProperties" bölümü içerir. Bu bölümlerin neyi temsil ettiğini anlamak için [IoT Hub oluştur ve oku](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct)makalesini okuyun.
* applicationProperties 'teki "Subject" iletinin oluşturulduğu MediaGraph içindeki düğüme başvurur. Bu durumda, ileti hareket algılama işlemcisinin kaynağı olur.
* applicationProperties 'teki "eventType" bir analiz olayı olduğunu gösterir.
* "eventTime" olayın gerçekleştiği saati gösterir.
* "gövde" analiz olayı hakkındaki verileri içerir. Bu durumda, olay bir çıkarım olayıdır ve bu nedenle gövde "timestamp" ve "ında" verilerini içerir.
* "ikinci dereceden" bölümü "tür" ın "Motion" olduğunu ve "Motion" olayı hakkında ek verilere sahip olduğunu gösterir.

MediaGraph 'in bir süre çalışmasına izin verirseniz çıkış penceresinde aşağıdaki iletiyi de görürsünüz:

```
[IoTHubMonitor] [7:47:45 AM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1588948185746703 1 IN IP4 172.xx.xx.xx\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.04.12\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets={SPS}\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "dataVersion": "1.0",
    "topic": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<my-resource-group>/providers/microsoft.media/mediaservices/<ams-account-name>",
    "subject": "/graphInstances/Sample-Graph-1/sources/rtspSource",
    "eventType": "Microsoft.Media.Graph.Diagnostics.MediaSessionEstablished",
    "eventTime": "2020-05-19T07:47:45.747Z"
  }
}
```

Yukarıdaki iletide aşağıdakilere göz önünde

* applicationProperties 'teki "Subject" iletisi, iletinin medya grafiğindeki RTSP kaynak düğümünden oluşturulduğunu gösterir.
* applicationProperties 'teki "eventType", bunun bir tanılama olayı olduğunu gösterir.
* "gövde" Tanılama olayı hakkındaki verileri içerir. Bu durumda, olay Mediasessionkurulduğu ve bu nedenle gövdedir.

## <a name="invoke-additional-direct-methods-to-clean-up"></a>Temizlemek için ek doğrudan Yöntemler çağırma

Şimdi, grafik örneğini devre dışı bırakmak ve silmek için doğrudan yöntemleri çağırın (Bu sırada).

### <a name="invoke-graphinstancedeactivate"></a>Graphınstancedeactivate öğesini çağır

Aşağıdaki yük ile doğrudan yöntem Graphınstancedeactivate öğesini çağırın.

```
{
    "@apiVersion" : "1.0",
    "name" : "Sample-Graph-1"
}
```

Birkaç saniye içinde çıkış penceresinde aşağıdaki yanıtı görmeniz gerekir:

```
[DirectMethod] Invoking Direct Method [GraphInstanceDeactivate] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": null
}
```

200 durum kodu grafik örneğinin başarıyla devre dışı bırakıldığını gösterir.

Sonraki adımlarda aşağıdaki adımları deneyin.

* Önceki bölümlerde gösterildiği gibi Graphınstanceget komutunu çağırın ve "durum" değerini gözlemleyin.

### <a name="invoke-graphinstancedelete"></a>Graphınstancedelete 'i çağır

Aşağıdaki yük ile Graphınstancedelete doğrudan yöntemini çağır

```
{
    "@apiVersion" : "1.0",
    "name" : "Sample-Graph-1"
}
```

Birkaç saniye içinde çıkış penceresinde aşağıdaki yanıtı görmeniz gerekir:

```
[DirectMethod] Invoking Direct Method [GraphInstanceDelete] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": null
}
```

Yanıtta 200 durum kodu, grafik örneğinin başarıyla silindiğini gösterir.

### <a name="invoke-graphtopologydelete"></a>Graphtopologyıdelete komutunu çağır

Aşağıdaki yük ile doğrudan GraphTopologyDelete yöntemini çağırın:

```
{
    "@apiVersion" : "1.0",
    "name" : "MotionDetection"
}
```

Birkaç saniye içinde çıkış penceresinde aşağıdaki yanıtı görmeniz gerekir:

```
[DirectMethod] Invoking Direct Method [GraphTopologyDelete] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": null
}
```

200 durum kodu grafik topolojisinin başarıyla silindiğini gösterir.

Sonraki adımlar olarak aşağıdakileri deneyin.

* Graphtopologyılist ' i çağırın ve modülde hiç grafik topolojisi olmadığını gözlemleyin.
* Graphtopologyılist ile aynı yük ile Graphınstancelist öğesini çağırın ve bir grafik örneği numaralandırılmıyor olduğunu gözlemleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekecekseniz, bu hızlı başlangıçta oluşturulan kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

* IoT Edge 'da canlı video analizi kullanarak video kaydetmeyi öğrenin
* Tanılama iletileri hakkında daha fazla bilgi edinin.
