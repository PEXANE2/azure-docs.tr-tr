---
title: Video Indexer ile çok dilli içeriği otomatik olarak tanımlayın ve yazıya döktü
titleSuffix: Azure Media Services
description: Bu konu, Video Indexer ile çok dilli içeriğin otomatik olarak nasıl tanımlanabildiğini ve aktarılabildiğini gösterir.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 09/01/2019
ms.author: juliako
ms.openlocfilehash: f0dede42891069bb5d01ddc33f3797c20c5493d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72968734"
---
# <a name="automatically-identify-and-transcribe-multi-language-content-preview"></a>Çok dilli içeriği otomatik olarak tanımlar ve yazıya çevirir (önizleme)

Video Indexer, çok dilli içerikte otomatik dil tanımlama ve transkripsiyon destekler. Bu işlem, sesten farklı segmentlerde konuşulan dili otomatik olarak tanımlamayı, medya dosyasının her bir bölümünün yazıya dönüştürülmesini ve transkripsiyonun tek bir birleşik transkripsiyonla birleştirilmesini içerir. 

## <a name="choosing-multilingual-identification-on-indexing-with-portal"></a>Portal ile indeksleme de çok dilli tanımlama seçimi

Videonuzu yüklerken ve dizine ekrirken **çok dilli algılamayı** seçebilirsiniz. Alternatif olarak, videonuzu yeniden dizine alırken **çok dilli algılamayı** seçebilirsiniz. Aşağıdaki adımlar nasıl yeniden dizine ekin ekinle yapılacağını açıklar:

1. [Video Indexer](https://vi.microsoft.com/) web sitesine gidip oturum açın.
1. **Kitaplık** sayfasına gidin ve yeniden dizine almak istediğiniz videonun adının üzerinde gezinin. 
1. Sağ alt köşede, Yeniden **dizin video** düğmesini tıklatın. 
1. Yeniden **dizine video** iletişim kutusunda, Video kaynak **dili** açılır kutusundan **çok dilli algılamayı** seçin.

    * Bir video çok dilli olarak dizine alındığında, içgörü sayfası bu seçeneği içerir ve kullanıcının hangi kesimin hangi dilde "Konuşulan dil" olarak transkripsiyonedildiğini görüntülemesini sağlayan ek bir içgörü türü görüntülenir.
    * Tüm dillere çeviri çok dilli transkript ten tam olarak elde edilebilir.
    * Diğer tüm kavrayışlar algılanan ana dilde görünür – bu seste en çok görünen dildir.
    * Oyuncu üzerinde kapalı altyazı çok dilli olarak da mevcuttur.

![Portal deneyimi](./media/multi-language-identification-transcription/portal-experience.png)

## <a name="choosing-multilingual-identification-on-indexing-with-api"></a>API ile dizin oluşturmada çok dilli tanımlama seçimi

API'yi kullanarak bir videoyu dizine ekin `multi-language detection` lerken `sourceLanguage` veya [yeniden dizine alırken,](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-Index-Video?) parametredeki seçeneği seçin.

### <a name="model-output"></a>Model çıkışı

Model, videoda algılanan tüm dilleri tek bir listede alır

```json
"sourceLanguage": null,
"sourceLanguages": [
    "es-ES",
    "en-US"
],
```

Ayrıca, transkripsiyon bölümündeki her örnek, transkripsiyon yapılan dili

```json
{
  "id": 136,
  "text": "I remember well when my youth Minister took me to hear Doctor King I was a teenager.",
  "confidence": 0.9343,
  "speakerId": 1,
  "language": "en-US",
  "instances": [
    {
       "adjustedStart": "0:21:10.42",
       "adjustedEnd": "0:21:17.48",
       "start": "0:21:10.42",
       "end": "0:21:17.48"
    }
  ]
},
```

## <a name="guidelines-and-limitations"></a>Yönergeler ve sınırlamalar

* Desteklenen diller kümesi: İngilizce, Fransızca, Almanca, İspanyolca.
* Desteklenen en fazla üç dilde çok dilli içerik desteği.
* Ses yukarıda desteklenen liste dışında diller içeriyorsa, sonuç beklenmeyen.
* Her dil için algılamak için en az segment uzunluğu – 15 saniye.
* Dil algılama ofset ortalama 3 saniyedir.
* Konuşmanın sürekli olması beklenmektedir. Diller arasında sık sık yapılan değişiklikler modellerin performansını etkileyebilir.
* Ana dili İngilizce olmayanların konuşması model performansını etkileyebilir (örneğin, konuşmacılar ana dillerini kullanıp başka bir dile geçtiklerinde).
* Model makul ses akustiği ile spontan bir konuşma konuşma tanımak için tasarlanmıştır (değil sesli komutları, şarkı, vb).
* Proje oluşturma ve düzenleme şu anda çok dilli videolar için kullanılamıyor.
* Çok dilli algılama kullanırken özel dil modelleri kullanılamaz.
* Anahtar kelimeler eklemek desteklenmez.
* Kapalı resim yazısı dosyaları dışa aktarırken dil göstergesi görünmez.
* Güncelleştirme transkript API birden çok dil dosyasını desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

[Video Indexer’a genel bakış](video-indexer-overview.md)
