---
title: Canlı iş transkripsiyonu
titleSuffix: Azure Media Services
description: Azure Medya Hizmetleri hakkında canlı transkripsiyon hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/19/2019
ms.author: juliako
ms.openlocfilehash: b364b6e70e3b5723c483bc3435f0c3a152c03aa9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79499864"
---
# <a name="live-transcription-preview"></a>Canlı transkripsiyon (önizleme)

Azure Media Service farklı protokollerde video, ses ve metin sunar. Canlı yayınınızı MPEG-DASH veya HLS/CMAF kullanarak yayımladığınızda, ardından video ve ses ile birlikte, hizmetimiz imsc1.1 uyumlu TTML'de transkripsiyonu yapılan metni sunar. Teslimat MPEG-4 Bölüm 30 (ISO/IEC 14496-30) parçaları halinde paketlenir. HLS/TS üzerinden teslimat kullanıyorsanız, metin parçalanmış VTT olarak teslim edilir.

Bu makalede, Azure Medya Hizmetleri v3 ile Canlı Etkinlik akışı yaparken canlı transkripsiyon etkinleştirin. Devam etmeden önce, Medya Hizmetleri v3 REST API'lerinin kullanımına aşina olduğunuzdan emin olun (ayrıntılar için [bu öğreticiye](stream-files-tutorial-with-rest.md) bakın). Ayrıca [canlı akış](live-streaming-overview.md) kavramı aşina olmalıdır. [Medya Hizmetleri öğreticisiyle Stream'i canlı](stream-live-tutorial-with-api.md) olarak tamamlaman önerilir.

> [!NOTE]
> Şu anda, canlı transkripsiyon yalnızca Batı ABD 2 bölgesinde önizleme özelliği olarak kullanılabilir. İngilizce konuşulan kelimelerin metne transkripsiyonu destekler. Bu özelliğin API başvurusu aşağıda yer almaktadır— becasuse önizlemede, ayrıntılar REST belgelerimizle birlikte kullanılamaz.

## <a name="creating-the-live-event"></a>Canlı Etkinliği Oluşturma

Canlı Etkinlik oluşturmak için PUT işlemini örneğin 2019-05-01 önizleme sürümüne gönderirsiniz:

```
PUT https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/liveEvents/:liveEventName?api-version=2019-05-01-preview&autoStart=true 
```

İşlem aşağıdaki gövdeye sahiptir (bir geçiş Live Event'in rtmp ile oluşturulduğu en iyi protokol olarak). Bir transkripsiyon özelliğinin eklenmesine dikkat edin. Dil için izin verilen tek değer en-US'dur.

```
{ 
  "properties": { 
    "description": "Demonstrate how to enable live transcriptions", 
    "input": { 
      "streamingProtocol": "RTMP", 
      "accessControl": { 
        "ip": { 
          "allow": [ 
            { 
              "name": "Allow All", 
              "address": "0.0.0.0", 
              "subnetPrefixLength": 0 
            } 
          ] 
        } 
      } 
    }, 
    "preview": { 
      "accessControl": { 
        "ip": { 
          "allow": [ 
            { 
              "name": "Allow All", 
              "address": "0.0.0.0", 
              "subnetPrefixLength": 0 
            } 
          ] 
        } 
      } 
    }, 
    "encoding": { 
      "encodingType": "None" 
    }, 
    "transcriptions": [ 
      { 
        "language": "en-US" 
      } 
    ], 
    "vanityUrl": false, 
    "streamOptions": [ 
      "Default" 
    ] 
  }, 
  "location": "West US 2" 
} 
```

Canlı Etkinliğin durumunu "Çalışan" durumuna girene kadar yokleyin, bu da artık bir katkı RTMP akışı gönderebileceğinizi gösterir. Artık önizleme akışını denetlemek ve Canlı çıktılar oluşturma gibi bu öğreticideki yle aynı adımları izleyebilirsiniz.

## <a name="transcription-delivery-and-playback"></a>Transkripsiyon teslimi ve oynatma

Hizmetimizin farklı protokollerde video, ses ve metin sunmak için dinamik ambalajı nasıl kullandığına ilişkin [Dinamik ambalaja genel bakış](dynamic-packaging-overview.md#to-prepare-your-source-files-for-delivery) makalesini inceleyin. Canlı yayınınızı MPEG-DASH veya HLS/CMAF kullanarak yayımladığınızda, ardından video ve ses ile birlikte, hizmetimiz imsc1.1 uyumlu TTML'de transkripsiyonu yapılan metni sunar. Bu teslimat MPEG-4 Bölüm 30 (ISO/IEC 14496-30) parçaları na paketlenir. HLS/TS üzerinden teslimat kullanıyorsanız, metin parçalanmış VTT olarak teslim edilir. Akışı oynamak için [Azure Media Player](use-azure-media-player.md) gibi bir web oynatıcık kullanabilirsiniz.  

> [!NOTE]
> Azure Media Player kullanıyorsanız, sürüm 2.3.3 veya sonraki sürümlerini kullanın.

## <a name="known-issues"></a>Bilinen sorunlar

Önizleme için, canlı transkripsiyon ile bilinen sorunlar şunlardır:

* Özellik yalnızca West US 2'de kullanılabilir.
* Uygulamaların [Medya Hizmetleri v3 OpenAPI Belirtiminde](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/preview/2019-05-01-preview/streamingservice.json)açıklanan önizleme API'lerini kullanması gerekir.
* Desteklenen tek dil İngilizcedir (en-us).
* İçerik koruması yla yalnızca AES zarf şifrelemesi desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Medya Hizmetlerine genel bakış](media-services-overview.md)
