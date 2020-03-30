---
title: Medya Hizmetleri etkinlikleri için Azure Olay Izgaraşe şemaları
description: Azure Etkinlik Ağıtı ile Medya Hizmetleri etkinlikleri için sağlanan özellikleri açıklar
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: reference
ms.date: 02/25/2020
ms.author: juliako
ms.openlocfilehash: d4a206bbddedfe9f23a943df27c6ac4b5fe17e8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251355"
---
# <a name="azure-event-grid-schemas-for-media-services-events"></a>Medya Hizmetleri etkinlikleri için Azure Olay Izgaraşe şemaları

Bu makalede, Medya Hizmetleri olayları için şema ve özellikleri sağlar.

Örnek komut dosyaları ve öğreticilerin listesi için [Medya Hizmetleri etkinlik kaynağına](../../event-grid/event-sources.md#azure-subscriptions)bakın.

## <a name="job-related-event-types"></a>İşle ilgili etkinlik türleri

Medya Hizmetleri, aşağıda açıklanan **İş** ile ilgili olay türlerini yayır. **İşle** ilgili olaylar için iki kategori vardır: "İş Durumu Değişikliklerini İzleme" ve "İş Çıktısı Durumu Değişikliklerini İzleme". 

JobStateChange etkinliğine abone olarak tüm etkinlikler için kaydolabilirsiniz. Veya yalnızca belirli etkinlikler için abone olabilirsiniz (örneğin, JobErrored, JobFinished ve JobCanceled gibi son durumlar).   

### <a name="monitoring-job-state-changes"></a>İş durumu değişikliklerini izleme

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Media.JobStateChange| Tüm İş Durumu değişiklikleri için bir etkinlik alın. |
| Microsoft.Media.JobScheduled| İş zamanlanan duruma geçtiğinde bir olay alın. |
| Microsoft.Media.JobProcessing| İş işleme durumuna geçtiğinde bir olay alın. |
| Microsoft.Media.JobCanceling| İş durumu iptal etmek için geçiş yaptığında bir olay alın. |
| Microsoft.Media.JobFinished| İş tamamlanan duruma geçtiğinde bir olay alın. Bu, İş çıktılarını içeren son durumdur.|
| Microsoft.Media.JobCanceled| İş iptal edilen duruma geçtiğinde bir etkinlik alın. Bu, İş çıktılarını içeren son durumdur.|
| Microsoft.Media.JobErrored| İş hata durumuna geçtiğinde bir olay alın. Bu, İş çıktılarını içeren son durumdur.|

Aşağıdaki [Şema örneklerine](#event-schema-examples) bakın.

### <a name="monitoring-job-output-state-changes"></a>İş çıktısı durumu değişikliklerinin izlenmesi

Bir iş birden çok iş çıktısı içerebilir (dönüşümü birden çok iş çıktısı olacak şekilde yapılandırırsanız.) Tek tek iş çıktısının ayrıntılarını izlemek istiyorsanız, iş çıktısı değişikliği olayını dinleyin.

Her **İş** **JobOutput**daha yüksek bir düzeyde olacak, böylece iş çıktısı olayları ilgili bir iş içinde ateş olsun. 

Tüm iş çıkışı `JobCanceled`için `JobError` toplanan sonuçları çıktı , , , hata iletileri . `JobFinished` Oysa, her görev sona ererken iş çıktısı olayları ateşler. Örneğin, bir kodlama çıktınız varsa ve ardından bir Video Analytics çıktısı varsa, son JobFinished olayı toplu verilerle birlikte ateşlemeden önce iki olayın iş çıktısı olayı olarak ateşlenmesini sağlarsınız.

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Media.JobOutputStateChange| Tüm İş çıktısı Durumu değişiklikleri için bir olay alın. |
| Microsoft.Media.JobOutputScheduled| İş çıktısı zamanlanan duruma geçtiğinde bir olay alın. |
| Microsoft.Media.JobOutputProcessing| İş çıktısı işleme durumuna geçtiğinde bir olay alın. |
| Microsoft.Media.JobOutputCanceling| İş çıktısı iptal durumuna geçtiğinde bir olay alın.|
| Microsoft.Media.JobOutputFinished| İş çıktısı tamamlanan duruma geçtiğinde bir olay alın.|
| Microsoft.Media.JobOutputCanceled| İş çıktısı iptal durumuna geçtiğinde bir olay alın.|
| Microsoft.Media.JobOutputHatası| İş çıktısı hata durumuna geçtiğinde bir olay alın.|

Aşağıdaki [Şema örneklerine](#event-schema-examples) bakın.

### <a name="monitoring-job-output-progress"></a>İş çıktısı ilerlemesini izleme

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Media.JobOutputProgress| Bu olay, %0'dan %100'e kadar iş işleme ilerlemesini yansıtır. Hizmet, ilerleme değerinde %5 veya daha fazla artış varsa veya son olaydan (sinyal) bu yana 30 saniyeden fazla bir süre geçtiyse, bir olay göndermeye çalışır. İlerleme değerinin %0'dan başlayıp %100'e ulaşması garanti edilmez ve zaman içinde sabit bir oranda artacağı garanti edilmez. Bu olay, işlemin tamamlandığını belirlemek için kullanılmamalıdır – bunun yerine durum değişikliği olaylarını kullanmalısınız.|

Aşağıdaki [Şema örneklerine](#event-schema-examples) bakın.

## <a name="live-event-types"></a>Canlı etkinlik türleri

Medya Hizmetleri ayrıca aşağıda açıklanan **Live** etkinlik türlerini de yayır. **Canlı** etkinlikler için iki kategori vardır: akış düzeyinde etkinlikler ve izleme düzeyi olaylar. 

### <a name="stream-level-events"></a>Akış düzeyindeki olaylar

Akış düzeyindeki olaylar akış veya bağlantı başına yükseltilir. Her olayın `StreamId` bağlantıyı veya akışı tanımlayan bir parametresi vardır. Her akış veya bağlantı, farklı türde bir veya daha fazla parçaya sahiptir. Örneğin, kodlayıcıdan bir bağlantının bir ses parçası ve dört video parçası olabilir. Akış olay türleri şunlardır:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Media.LiveEventConnection Reddedildi | Kodlayıcının bağlantı denemesi reddedildi. |
| Microsoft.Media.LiveEventEncoderConnected | Encoder canlı olay ile bağlantı kurar. |
| Microsoft.Media.LiveEventEncoderBağlantısı kesildi | Kodlayıcı bağlantıyı keser. |

Aşağıdaki [Şema örneklerine](#event-schema-examples) bakın.

### <a name="track-level-events"></a>İzleme düzeyindeki olaylar

Parça düzeyindeki olaylar parça başına yükseltilir. 

> [!NOTE]
> Tüm parça düzeyindeki olaylar, canlı kodlayıcı bağlandıktan sonra yükseltilir.

Parça düzeyinde olay türleri şunlardır:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Media.LiveEventIncomingDataChunkDropped | Çok geç olduğu veya çakışan bir zaman damgası olduğundan medya sunucusu veri yığınını düşürür (yeni veri yığınının zaman damgası önceki veri yığınının bitiş saatinden daha azdır). |
| Microsoft.Media.LiveEventIncomingStreamReceived | Medya sunucusu akış veya bağlantıdaki her parça için ilk veri öbeklerini alır. |
| Microsoft.Media.LiveEventIncomingStreamsOutOfSync | Medya sunucusu ses ve video akışlarının eşitlenmemiş olduğunu algılar. Kullanıcı deneyimi etkilenmeyebileceğinden uyarı olarak kullanın. |
| Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync | Medya sunucusu, harici kodlayıcıdan gelen iki video akışından herhangi birinin eşitolmadığını algılar. Kullanıcı deneyimi etkilenmeyebileceğinden uyarı olarak kullanın. |
| Microsoft.Media.LiveEventingestHeartbeat | Canlı etkinlik çalışırken her parça için her 20 saniyede bir yayınlanır. Yutma sağlık özeti sağlar.<br/><br/>Kodlayıcı başlangıçta bağlandıktan sonra, kodlayıcı nın hala bağlı olup olmadığını her 20 saniyede bir yayan sinyal olayı devam ediyor. |
| Microsoft.Media.LiveEventTrackDiscontinuityAlgılandı | Medya sunucusu gelen parçadaki süreksizlikalgılar. |

Aşağıdaki [Şema örneklerine](#event-schema-examples) bakın.

## <a name="event-schema-examples"></a>Olay şeması örnekleri

### <a name="jobstatechange"></a>JobStateChange

Aşağıdaki örnek, **JobStateChange** olayının şemalarını gösterir: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
    "eventType": "Microsoft.Media.JobStateChange",
    "eventTime": "2018-04-20T21:26:13.8978772",
    "id": "b9d38923-9210-4c2b-958f-0054467d4dd7",
    "data": {
      "previousState": "Processing",
      "state": "Finished"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| previousState | string | Olaydan önceki işin durumu. |
| durum | string | Bu olayda işin yeni durumu bildirilir. Örneğin, "Zamanlanan: İş başlamaya hazır" veya "Tamamlandı: İş bitti" .|

İş durumu değerlerden biri olabilir: *Sıralı*, *Zamanlanmış*, *İşleme*, *Bitmiş*, *Hata*, *İptal ,* *İptal*

> [!NOTE]
> *Sıraya sadece* **önceki Devlet** mülkiyetinde mevcut olacak, ancak **devlet** mülkiyetinde bulunmaz.

### <a name="jobscheduled-jobprocessing-jobcanceling"></a>İş Zamanlanan, İşİşlemleme, İşİptal Etme

Son olmayan her İş durumu değişikliği (JobScheduled, JobProcessing, JobCanceling gibi) için şema aşağıdakilere benzer:

```json
[{
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
  "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
  "eventType": "Microsoft.Media.JobProcessing",
  "eventTime": "2018-10-12T16:12:18.0839935",
  "id": "a0a6efc8-f647-4fc2-be73-861fa25ba2db",
  "data": {
    "previousState": "Scheduled",
    "state": "Processing",
    "correlationData": {
      "testKey1": "testValue1",
      "testKey2": "testValue2"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

### <a name="jobfinished-jobcanceled-joberrored"></a>İş Bitti, İşİptal edildi, İş HataLı

Her son İş durumu değişikliği (JobFinished, JobCanceled, JobErrored gibi), örnek şema aşağıdakilere benzer:

```json
[{
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
  "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
  "eventType": "Microsoft.Media.JobFinished",
  "eventTime": "2018-10-12T16:25:56.4115495",
  "id": "9e07e83a-dd6e-466b-a62f-27521b216f2a",
  "data": {
    "outputs": [
      {
        "@odata.type": "#Microsoft.Media.JobOutputAsset",
        "assetName": "output-7640689F",
        "error": null,
        "label": "VideoAnalyzerPreset_0",
        "progress": 100,
        "state": "Finished"
      }
    ],
    "previousState": "Processing",
    "state": "Finished",
    "correlationData": {
      "testKey1": "testValue1",
      "testKey2": "testValue2"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Çıkış | Dizi | İş çıktılarını alır.|

### <a name="joboutputstatechange"></a>JobOutputStateChange

Aşağıdaki örnek, **JobOutputStateChange** olayının şemalarını gösterir:

```json
[{
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
  "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
  "eventType": "Microsoft.Media.JobOutputStateChange",
  "eventTime": "2018-10-12T16:25:56.0242854",
  "id": "dde85f46-b459-4775-b5c7-befe8e32cf90",
  "data": {
    "previousState": "Processing",
    "output": {
      "@odata.type": "#Microsoft.Media.JobOutputAsset",
      "assetName": "output-7640689F",
      "error": null,
      "label": "VideoAnalyzerPreset_0",
      "progress": 100,
      "state": "Finished"
    },
    "jobCorrelationData": {
      "testKey1": "testValue1",
      "testKey2": "testValue2"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

### <a name="joboutputscheduled-joboutputprocessing-joboutputfinished-joboutputcanceling-joboutputcanceled-joboutputerrored"></a>JobOutputScheduled, JobOutputProcessing, JobOutputFinished, JobOutputCanceling, JobOutputCanceled, JobOutputErrored

Her JobOutput durum değişikliği için, örnek şema aşağıdakilere benzer:

```json
[{
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
  "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
  "eventType": "Microsoft.Media.JobOutputProcessing",
  "eventTime": "2018-10-12T16:12:18.0061141",
  "id": "f1fd5338-1b6c-4e31-83c9-cd7c88d2aedb",
  "data": {
    "previousState": "Scheduled",
    "output": {
      "@odata.type": "#Microsoft.Media.JobOutputAsset",
      "assetName": "output-7640689F",
      "error": null,
      "label": "VideoAnalyzerPreset_0",
      "progress": 0,
      "state": "Processing"
    },
    "jobCorrelationData": {
      "testKey1": "testValue1",
      "testKey2": "testValue2"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```
### <a name="joboutputprogress"></a>İş Çıktıları İlerleme

Örnek şema aşağıdakilere benzer:

 ```json
[{
  "topic": "/subscriptions/<subscription-id>/resourceGroups/belohGroup/providers/Microsoft.Media/mediaservices/<account-name>",
  "subject": "transforms/VideoAnalyzerTransform/jobs/job-5AB6DE32",
  "eventType": "Microsoft.Media.JobOutputProgress",
  "eventTime": "2018-12-10T18:20:12.1514867",
  "id": "00000000-0000-0000-0000-000000000000",
  "data": {
    "jobCorrelationData": {
      "TestKey1": "TestValue1",
      "testKey2": "testValue2"
    },
    "label": "VideoAnalyzerPreset_0",
    "progress": 86
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

### <a name="liveeventconnectionrejected"></a>LiveEventConnectionReddedildi

Aşağıdaki örnek, **LiveEventConnectionRejected** olayının şema gösterir: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/MyLiveEvent1",
    "eventType": "Microsoft.Media.LiveEventConnectionRejected",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "b303db59-d5c1-47eb-927a-3650875fded1",
    "data": { 
      "streamId":"Mystream1",
      "ingestUrl": "http://abc.ingest.isml",
      "encoderIp": "118.238.251.xxx",
      "encoderPort": 52859,
      "resultCode": "MPE_INGEST_CODEC_NOT_SUPPORTED"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| streamId | string | Akışın veya bağlantının tanımlayıcısı. Kodlayıcı veya müşteri, bu kimliği yutulan URL'ye eklemekle yükümlüdür. |  
| yutmaUrl | string | Canlı etkinlik tarafından sağlanan URL'yi yutma. |  
| encoderIp | string | Kodlayıcının IP'si. |
| encoderPort | string | Bu derenin geldiği yerden kodlayıcının limanı. |
| Resultcode | string | Bağlantının reddedilmesinin nedeni. Sonuç kodları aşağıdaki tabloda listelenir. |

Hata sonuç kodlarını [canlı Olay hata kodlarında](live-event-error-codes.md)bulabilirsiniz.

### <a name="liveeventencoderconnected"></a>LiveEventEncoderBağlı

Aşağıdaki örnek, **LiveEventEncoderConnected** olayının şemalarını gösterir: 

```json
[
  { 
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventEncoderConnected",
    "eventTime": "2018-08-07T23:08:09.1710643",
    "id": "<id>",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml",
      "streamId": "15864-stream0",
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| streamId | string | Akışın veya bağlantının tanımlayıcısı. Kodlayıcı veya müşteri, bu kimliği en yüksek URL'de sağlamaktan sorumludur. |
| yutmaUrl | string | Canlı etkinlik tarafından sağlanan URL'yi yutma. |
| encoderIp | string | Kodlayıcının IP'si. |
| encoderPort | string | Bu derenin geldiği yerden kodlayıcının limanı. |

### <a name="liveeventencoderdisconnected"></a>LiveEventEncoderBağlantısı kesildi

Aşağıdaki örnek, **LiveEventEncoderDisconnected** olayının şema gösterir: 

```json
[
  { 
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventEncoderDisconnected",
    "eventTime": "2018-08-07T23:08:09.1710872",
    "id": "<id>",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml",
      "streamId": "15864-stream0",
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485",
      "resultCode": "S_OK"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| streamId | string | Akışın veya bağlantının tanımlayıcısı. Kodlayıcı veya müşteri, bu kimliği yutulan URL'ye eklemekle yükümlüdür. |  
| yutmaUrl | string | Canlı etkinlik tarafından sağlanan URL'yi yutma. |  
| encoderIp | string | Kodlayıcının IP'si. |
| encoderPort | string | Bu derenin geldiği yerden kodlayıcının limanı. |
| Resultcode | string | Kodlayıcının bağlantısını kesme nedeni. Bu zarif bir kopukluk veya bir hata dan olabilir. Sonuç kodları aşağıdaki tabloda listelenir. |

Hata sonuç kodlarını [canlı Olay hata kodlarında](live-event-error-codes.md)bulabilirsiniz.

Zarif kesme sonuç kodları şunlardır:

| Sonuç kodu | Açıklama |
| ----------- | ----------- |
| S_OK | Kodlayıcı başarıyla kesildi. |
| MPE_CLIENT_TERMINATED_SESSION | Kodlayıcı nın bağlantısı kesildi (RTMP). |
| MPE_CLIENT_DISCONNECTED | Kodlayıcı nın bağlantısı kesildi (FMP4). |
| MPI_REST_API_CHANNEL_RESET | Kanal sıfırlama komutu alınır. |
| MPI_REST_API_CHANNEL_STOP | Kanal durdurma komutu alındı. |
| MPI_REST_API_CHANNEL_STOP | Kanal bakımdan geçiyor. |
| MPI_STREAM_HIT_EOF | EOF akışı kodlayıcı tarafından gönderilir. |

### <a name="liveeventincomingdatachunkdropped"></a>LiveEventIncomingDataChunkDropped

Aşağıdaki örnek, **LiveEventIncomingDataChunkDropped** olayının şemalarını gösterir: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/MyLiveEvent1",
    "eventType": "Microsoft.Media.LiveEventIncomingDataChunkDropped",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "03da9c10-fde7-48e1-80d8-49936f2c3e7d",
    "data": { 
      "trackType": "Video",
      "trackName": "Video",
      "bitrate": 300000,
      "timestamp": 36656620000,
      "timescale": 10000000,
      "resultCode": "FragmentDrop_OverlapTimestamp"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| trackType | string | Parçanın türü (Ses / Video). |
| trackName | string | Pistin adı. |
| Bitrate | integer | Parçanın Bitrate'si. |
| timestamp | string | Veri yığınının zaman damgası düştü. |
| Zaman ölçeği | string | Zaman damgasının zaman ölçeği. |
| Resultcode | string | Veri yığınının düşme nedeni. **FragmentDrop_OverlapTimestamp** ya da **FragmentDrop_NonIncreasingTimestamp.** |

### <a name="liveeventincomingstreamreceived"></a>LiveEventIncomingStreamReceived

Aşağıdaki örnek, **LiveEventIncomingStreamReceived** olayının şemalarını gösterir: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIncomingStreamReceived",
    "eventTime": "2018-08-07T23:08:10.5069288Z",
    "id": "7f939a08-320c-47e7-8250-43dcfc04ab4d",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml/Streams(15864-stream0)15864-stream0",
      "trackType": "video",
      "trackName": "video",
      "bitrate": 2962000,
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485",
      "timestamp": "15336831655032322",
      "duration": "20000000",
      "timescale": "10000000"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| trackType | string | Parçanın türü (Ses / Video). |
| trackName | string | Parçanın adı (kodlayıcı tarafından sağlanan veya RTMP durumunda sunucu *TrackType_Bitrate* biçiminde üretir). |
| Bitrate | integer | Parçanın Bitrate'si. |
| yutmaUrl | string | Canlı etkinlik tarafından sağlanan URL'yi yutma. |
| encoderIp | string  | Kodlayıcının IP'si. |
| encoderPort | string | Bu derenin geldiği yerden kodlayıcının limanı. |
| timestamp | string | Alınan veri yığınının ilk zaman damgası. |
| Zaman ölçeği | string | Zaman damgasının temsil edildiği zaman ölçeği. |

### <a name="liveeventincomingstreamsoutofsync"></a>LiveEventIncomingStreamsOutOfSync

Aşağıdaki örnek, **LiveEventIncomingStreamsOutOfSync** olayının şeasını gösterir: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIncomingStreamsOutOfSync",
    "eventTime": "2018-08-10T02:26:20.6269183Z",
    "id": "b9d38923-9210-4c2b-958f-0054467d4dd7",
    "data": {
      "minLastTimestamp": "319996",
      "typeOfStreamWithMinLastTimestamp": "Audio",
      "maxLastTimestamp": "366000",
      "typeOfStreamWithMaxLastTimestamp": "Video",
      "timescaleOfMinLastTimestamp": "10000000", 
      "timescaleOfMaxLastTimestamp": "10000000"       
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| minLastTimestamp | string | Tüm parçalar (ses veya video) arasında en az son zaman damgası. |
| typeOfTrackWithMinLastTimestamp | string | En az son zaman damgası olan parçanın (ses veya video) türü. |
| maxLastTimestamp | string | Tüm parçalar (ses veya video) arasında tüm zaman damgaları maksimum. |
| typeOfTrackWithMaxLastTimestamp | string | En yüksek son zaman damgası olan parçanın (ses veya video) türü. |
| zaman ölçeğiOfMinLastTimestamp| string | "MinLastTimestamp"ın temsil edildiği zaman ölçeğini alır.|
| zaman ölçeğiOfMaxLastTimestamp| string | "MaxLastTimestamp"ın temsil edildiği zaman ölçeğini alır.|

### <a name="liveeventincomingvideostreamsoutofsync"></a>LiveEventIncomingVideoStreamsOutOfSync

Aşağıdaki örnek, **LiveEventIncomingVideoStreamsOutOfSync** olayının şemalarını gösterir: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/LiveEvent1",
    "eventType": "Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "6dd4d862-d442-40a0-b9f3-fc14bcf6d750",
    "data": {
      "firstTimestamp": "2162058216",
      "firstDuration": "2000",
      "secondTimestamp": "2162057216",
      "secondDuration": "2000",
      "timescale": "10000000"      
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| ilkTimestamp | string | Zaman damgası, tip videonun parça/kalite düzeylerinden biri için alınmıştır. |
| ilkSüre | string | İlk zaman damgası ile veri yığınının süresi. |
| secondTimestamp | string  | Zaman damgası türü videonun başka bir parça/kalite düzeyi için alınmıştır. |
| secondDuration | string | İkinci zaman damgası ile veri yığınının süresi. |
| Zaman ölçeği | string | Zaman damgalarının ve sürenin zaman ölçeği.|

### <a name="liveeventingestheartbeat"></a>LiveEventingestHeartbeat

Aşağıdaki örnek, **LiveEventIngestHeartbeat** olayının şemalarını gösterir: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIngestHeartbeat",
    "eventTime": "2018-08-07T23:17:57.4610506",
    "id": "7f450938-491f-41e1-b06f-c6cd3965d786",
    "data": {
      "trackType": "audio",
      "trackName": "audio",
      "bitrate": 160000,
      "incomingBitrate": 155903,
      "lastTimestamp": "15336837535253637",
      "timescale": "10000000",
      "overlapCount": 0,
      "discontinuityCount": 0,
      "nonincreasingCount": 0,
      "unexpectedBitrate": false,
      "state": "Running",
      "healthy": true
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| trackType | string | Parçanın türü (Ses / Video). |
| trackName | string | Parçanın adı (kodlayıcı tarafından sağlanan veya RTMP durumunda sunucu *TrackType_Bitrate* biçiminde üretir). |
| Bitrate | integer | Parçanın Bitrate'si. |
| gelenBitrate | integer | Kodlayıcıdan gelen veri yığınlarına göre hesaplanan bit hızı. |
| lastTimestamp | string | Son 20 saniye içinde bir parça için alınan son zaman damgası. |
| Zaman ölçeği | string | Zaman damgalarının ifade edildiği zaman ölçeği. |
| çakışmaSayısı | integer | Veri parçalarının sayısı son 20 saniyede zaman damgaları ile örtüştü. |
| süreksizlikSayısı | integer | Son 20 saniyede gözlenen süreksizlik sayısı. |
| nonIncreasingCount | integer | Son 20 saniyede zaman damgalı veri parçalarının sayısı alındı. |
| beklenmeyenBitrate | bool | Beklenen ve gerçek bit hızları son 20 saniye de izin verilenden daha fazla sınıra göre değişirse. Eğer ve sadece eğer, gelenBitrate >= 2* bitrate VEYA gelenBitrate <= bitrate/2 VEYA GelenBitrate = 0. |
| durum | string | Canlı olayın durumu. |
| Sağlıklı | bool | Sayımları ve bayrakları temel alınarak yutulmanın sağlıklı olup olmadığını gösterir. ÖrtüşmeCount = 0 && süreksizlikCount = 0 && nonIncreasingCount = 0 && unexpectedBitrate = false ise sağlıklı doğrudur. |

### <a name="liveeventtrackdiscontinuitydetected"></a>LiveEventTrackSüretazlik Algılandı

Aşağıdaki örnek, **LiveEventTrackDiscontinuityAlgılanan** olayın şemagösterir: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventTrackDiscontinuityDetected",
    "eventTime": "2018-08-07T23:18:06.1270405Z",
    "id": "5f4c510d-5be7-4bef-baf0-64b828be9c9b",
    "data": {
      "trackName": "video",
      "previousTimestamp": "15336837615032322",
      "trackType": "video",
      "bitrate": 2962000,
      "newTimestamp": "15336837619774273",
      "discontinuityGap": "575284",
      "timescale": "10000000"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| trackType | string | Parçanın türü (Ses / Video). |
| trackName | string | Parçanın adı (kodlayıcı tarafından sağlanan veya RTMP durumunda sunucu *TrackType_Bitrate* biçiminde üretir). |
| Bitrate | integer | Parçanın Bitrate'si. |
| öncekiZaman Damgası | string | Önceki parçanın zaman damgası. |
| newTimestamp | string | Geçerli parçanın zaman damgası. |
| süreksizlikGap | string | İki zaman damgası arasındaki boşluk. |
| Zaman ölçeği | string | Hem zaman damgası hem de süreksizlik boşluğunun temsil edildiği zaman ölçeği. |

### <a name="common-event-properties"></a>Ortak olay özellikleri

Bir olay aşağıdaki üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | EventGrid konusu. Bu özellik, Medya Hizmetleri hesabının kaynak kimliğine sahiptir. |
| Konu | string | Medya Hizmetleri hesabı nın altındaki Medya Hizmetleri kanalının kaynak yolu. Konuyu ve konuyu daraltma, size iş için kaynak kimliğini verir. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. Örneğin, "Microsoft.Media.JobStateChange". |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| id | string | Etkinlik için benzersiz tanımlayıcı. |
| veri | object | Medya Hizmetleri olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

## <a name="next-steps"></a>Sonraki adımlar

[İş durumu değişikliği etkinlikleri için kaydolun](job-state-events-cli-how-to.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Medya Hizmeti olaylarını içeren EventGrid .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.EventGrid/)
- [Medya Hizmetleri etkinliklerinin tanımları](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/eventgrid/data-plane/Microsoft.Media/stable/2018-01-01/MediaServices.json)
- [Canlı Olay hata kodları](live-event-error-codes.md)
