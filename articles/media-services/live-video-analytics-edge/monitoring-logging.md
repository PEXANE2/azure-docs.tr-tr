---
title: İzleme ve günlüğe kaydetme-Azure
description: Bu makalede, IoT Edge izleme ve günlüğe kaydetme hakkında canlı video analizine genel bakış sunulmaktadır.
ms.topic: reference
ms.date: 04/27/2020
ms.openlocfilehash: 807b0623159e0b50285b89da2835e9dd6cb037aa
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84261215"
---
# <a name="monitoring-and-logging"></a>İzleme ve günlüğe kaydetme

Bu makalede, uzaktan izleme için IoT Edge modülündeki canlı video analizinden nasıl olay alabileceğinizi öğreneceksiniz. 

Ayrıca, modülün oluşturduğu günlükleri nasıl denetleyebileceğinizi de öğreneceksiniz.

## <a name="taxonomy-of-events"></a>Olay sınıflandırması

IoT Edge canlı video analizi, olayları veya telemetri verilerini aşağıdaki sınıflandırmaya göre yayar.

![IoT Edge telemetri şemasında canlı video analizi](./media/telemetry-schema/taxonomy.png)

* İşletimsel: bir kullanıcı tarafından alınan eylemlerin bir parçası olarak veya bir [medya grafiğinin](media-graph-concept.md)yürütülmesi sırasında oluşturulan olaylar.
   
   * Birim: düşük (bir dakika veya daha düşük bir hız) olması bekleniyor.
   * Örnekler:

      Kayıt başlatıldı (aşağıda), kayıt durduruldu
      
      ```
      {
        "body": {
          "outputType": "assetName",
          "outputLocation": "sampleAssetFromEVR-LVAEdge-20200512T233309Z"
        },
        "applicationProperties": {
          "topic": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<my-resource-group>/providers/microsoft.media/mediaservices/<ams-account-name>",
          "subject": "/graphInstances/Sample-Graph-2/sinks/assetSink",
          "eventType": "Microsoft.Media.Graph.Operational.RecordingStarted",
          "eventTime": "2020-05-12T23:33:10.392Z",
          "dataVersion": "1.0"
        }
      }
      ```
* Tanılama: sorunları ve/veya performansla ilgili sorunları tanılamaya yardımcı olan olaylar.

   * Birim: yüksek (birkaç kez bir dakika) olabilir.
   * Örnekler:
   
      RTSP [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol) bilgileri (aşağıda) veya gelen video akışındaki boşluklar.

      ```
      {
        "body": {
          "sdp": "SDP:\nv=0\r\no=- 1589326384077235 1 IN IP4 XXX.XX.XX.XXX\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/lots_015.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.04.12\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-73.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/lots_015.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=640028;sprop-parameter-sets=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\r\na=control:track1\r\n"
        },
        "applicationProperties": {
          "topic": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<my-resource-group>/providers/microsoft.media/mediaservices/<ams-account-name>",
          "subject": "/graphInstances/Sample-Graph-2/sources/rtspSource",
          "eventType": "Microsoft.Media.Graph.Diagnostics.MediaSessionEstablished",
          "eventTime": "2020-05-12T23:33:04.077Z",
          "dataVersion": "1.0"
        }
      }
      ```
* Analytics: video analizinin bir parçası olarak oluşturulan olaylar.

   * Birim: yüksek (bir dakika veya daha fazla zaman) olabilir.
   * Örnekler:
      
      Hareket algılandı (aşağıda), çıkarım sonucu.
   ```      
   {
     "body": {
       "timestamp": 143039375044290,
       "inferences": [
         {
           "type": "motion",
           "motion": {
             "box": {
               "l": 0.48954,
               "t": 0.140741,
               "w": 0.075,
               "h": 0.058824
             }
           }
         }
       ]
     },
     "applicationProperties": {
       "topic": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<my-resource-group>/providers/microsoft.media/mediaservices/<ams-account-name>",
       "subject": "/graphInstances/Sample-Graph-2/processors/md",
       "eventType": "Microsoft.Media.Graph.Analytics.Inference",
       "eventTime": "2020-05-12T23:33:09.381Z",
       "dataVersion": "1.0"
     }
   }
   ```
Modül tarafından yayılan olaylar [IoT Edge hub 'ına](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime#iot-edge-hub)gönderilir ve oradan başka hedeflere yönlendirilebilir. 

## <a name="controlling-events"></a>Olayları denetleme

IoT Edge modülündeki canlı video analizi tarafından yayımlanan işletimsel ve Tanılama olaylarını denetlemek için, aşağıdaki modül ikizi özelliklerini [Modül IKIZI JSON şeması](module-twin-configuration-schema.md)'nda belgelendiği şekilde kullanabilirsiniz.

`diagnosticsEventsOutputName`– modülden tanılama olayları almak için bu özellik için dahil etme ve sağlama (any) değeri. Bunu atlayın veya modülün Tanılama olaylarını yayımlamasını durdurmak için boş bırakın.
   
`operationalEventsOutputName`– modülden işlem olaylarını almak için bu özellik için dahil etme ve sağlama (any) değeri. Modülün işlemsel olayları yayımlamasını durdurmak için bunu atlayın veya boş bırakın.
   
Analiz olayları, hareket algılama işlemcisi veya HTTP uzantısı işlemcisi gibi düğümler tarafından oluşturulur ve bunları IoT Edge hub 'ına göndermek için IoT Hub havuzu kullanılır. 

[Yukarıdaki tüm olayların](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) ikizi $edgeHub modülünün istenen özelliği aracılığıyla yönlendirilmesini denetleyebilirsiniz (dağıtım bildiriminde):

```
 "$edgeHub": {
   "properties.desired": {
     "schemaVersion": "1.0",
     "routes": {
       "moduleToHub": "FROM /messages/modules/lvaEdge/outputs/* INTO $upstream"
     },
     "storeAndForwardConfiguration": {
       "timeToLiveSecs": 7200
     }
   }
 }
```

Yukarıdaki, lvaEdge, IoT Edge Module üzerindeki canlı video analizinin adıdır ve yönlendirme kuralı, [yolları bildir](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes)bölümünde tanımlanan şemayı izler.

> [!NOTE]
> Analiz olaylarının IoT Edge hub 'ına ulaşmasını sağlamak için, herhangi bir hareket algılama işlemcisi düğümü ve/veya herhangi bir HTTP uzantısı işlemci düğümünün bir IoT Hub havuz düğümü aşağı akış olması gerekir.

## <a name="event-schema"></a>Olay şeması

Olaylar Edge cihazında olur ve kenarda veya bulutta tüketilebilir. IoT Edge üzerinde canlı video analizi tarafından oluşturulan olaylar, Azure IoT Hub tarafından oluşturulan ve sistem özellikleri, uygulama özellikleri ve bir gövdedeki [akış mesajlaşma düzenine](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct) uygundur.

### <a name="summary"></a>Özet

IoT Hub üzerinden gözlemlendiği her olayın, aşağıda açıklandığı gibi ortak özellikler kümesi olacaktır.

|Özellik   |Özellik Türü| Veri Türü   |Açıklama|
|---|---|---|---|
|ileti kimliği |sistemin |guid|  Benzersiz olay KIMLIĞI.|
|konu başlığı| applicationProperty |string|    Media Services hesabının yolunu Azure Resource Manager.|
|Konu|   applicationProperty |string|    Olayı yayan varlığın alt yolu.|
|eventTime| applicationProperty|    string| Olayın oluşturulduğu zaman.|
|Türü| applicationProperty |string|    Olay türü tanımlayıcısı (aşağıya bakın).|
|body|body  |nesne|    Belirli olay verileri.|
|dataVersion    |applicationProperty|   string  |{Birincil}. Bazı|

### <a name="properties"></a>Özellikler

#### <a name="message-id"></a>ileti kimliği

Olay genel benzersiz tanıtıcısı (GUID)

#### <a name="topic"></a>konu başlığı

Grafikle ilişkili Azure Medya hizmeti hesabını temsil eder.

`/subscriptions/{subId}/resourceGroups/{rgName}/providers/Microsoft.Media/mediaServices/{accountName}`

#### <a name="subject"></a>Konu

Olayı yayan varlık:

`/graphInstances/{graphInstanceName}`<br/>
`/graphInstances/{graphInstanceName}/sources/{sourceName}`<br/>
`/graphInstances/{graphInstanceName}/processors/{processorName}`<br/>
`/graphInstances/{graphInstanceName}/sinks/{sinkName}`

Subject özelliği, genel olayların oluşturma modülüyle eşleştirilmesini sağlar. Örneğin, geçersiz RTSP Kullanıcı adı veya parola durumunda oluşturulan olay `Microsoft.Media.Graph.Diagnostics.ProtocolError` `/graphInstances/myGraph/sources/myRtspSource` düğümde olur.

#### <a name="event-types"></a>Olay türleri

Olay türleri bir ad alanına aşağıdaki şemaya göre atanır:

`Microsoft.Media.Graph.{EventClass}.{EventType}`

#### <a name="event-classes"></a>Olay sınıfları

|Sınıf Adı|Açıklama|
|---|---|
|Analiz  |İçerik analizinin bir parçası olarak oluşturulan olaylar.|
|Tanılama    |Sorunların ve performansın tanılanmasına yardımcı olan olaylar.|
|İşlemdeki    |Kaynak işleminin bir parçası olarak oluşturulan olaylar.|

Olay türleri her bir olay sınıfına özeldir.

Örnekler:

* Microsoft. Media. Graph. Analytics. çıkarımı
* Microsoft. Media. Graph. Diagnostics. AuthorizationError
* Microsoft. Media. Graph. Operational. Graphınstancestarted

### <a name="event-time"></a>Olay saati

Olay saati, ıSO8601 dizesinde ve olayın gerçekleştiği zaman ile açıklanır.

## <a name="logging"></a>Günlüğe kaydetme

Diğer IoT Edge modülleriyle benzer şekilde, uç cihazdaki [kapsayıcı günlüklerini de inceleyebilirsiniz](https://docs.microsoft.com/azure/iot-edge/troubleshoot#check-container-logs-for-issues) . Günlüklere yazılan bilgiler [aşağıdaki Module ikizi](module-twin-configuration-schema.md) özellikleri tarafından denetlenebilir:

* logLevel

   * İzin verilen değerler verbose, bilgi, uyarı, hata, yok.
   * Varsayılan değer bilgi: günlüklerde hata, uyarı ve bilgi yer alacak. iletilerine.
   * Değeri uyarı olarak ayarlarsanız, Günlükler hata ve uyarı iletileri içerir
   * Değeri hata olarak ayarlarsanız, Günlükler yalnızca hata iletileri içerir.
   * Değeri None olarak ayarlarsanız hiçbir günlük oluşturulmaz (bu önerilmez).
   * Yalnızca bir sorunu tanılamak için günlükleri Azure desteğiyle paylaşmanız gerekiyorsa verbose ' i kullanmanız gerekir.
* Günlüğe kaydetme kategorileri

   * Aşağıdakilerden biri veya daha fazlası için virgülle ayrılmış bir liste: Application, Events, MediaPipeline.
   * Varsayılan: uygulama, olaylar.
   * Uygulama – modül başlangıç iletileri, ortam hataları ve doğrudan Yöntem çağrıları gibi modülden yüksek düzey bilgiler.
   * Olaylar: Bunlar, bu makalede daha önce açıklanan olaylardır.
   * MediaPipeline: Bunlar, bir RTSP özellikli kamerayla bağlantı kurmaya yönelik zorluklar gibi sorunları giderirken öngörü sunan bazı düşük düzeyli günlüklerdir.
   
### <a name="generating-debug-logs"></a>Hata ayıklama günlükleri oluşturuluyor

Belirli durumlarda, Azure desteği 'nin bir sorunu çözümlemesine yardımcı olması için yukarıda açıklananlardan daha ayrıntılı Günlükler oluşturmanız gerekebilir. Bunu yapmanın iki adımı vardır.

İlk olarak, createOptions aracılığıyla [Modül depolamayı cihaz depolamasına bağlarsınız](https://docs.microsoft.com/azure/iot-edge/how-to-access-host-storage-from-module#link-module-storage-to-device-storage) . Hızlı başlayan bir [dağıtım bildirimi şablonunu](https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp/blob/master/src/edge/deployment.template.json) incelerseniz şunları görürsünüz:

```
"createOptions": {
   …
   "Binds": [
     "/var/local/mediaservices/:/var/lib/azuremediaservices/"
   ]
 }
```

Yukarıdaki sınır modülünün günlükleri (cihaz) depolama yoluna "/var/Local/mediaservices/" yazmasını sağlar. Aşağıdaki istenen özelliği modüle eklerseniz:

`"debugLogsDirectory": "/var/lib/azuremediaservices/debuglogs/",`

Ardından, modül hata ayıklama günlüklerini bir ikili biçimde (cihaz) depolama yolu/var/Local/mediaservices/debuglogs/dizinine yazacak ve bu da Azure desteğiyle paylaşabilirsiniz.

## <a name="faq"></a>SSS

[SSS](faq.md#monitoring-and-metrics)

## <a name="next-steps"></a>Sonraki adımlar

[Sürekli video kaydı](continuous-video-recording-tutorial.md)
