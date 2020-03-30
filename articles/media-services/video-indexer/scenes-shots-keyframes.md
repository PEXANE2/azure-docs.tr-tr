---
title: Video Indexer sahneleri, çekimler ve anahtar kareler
titleSuffix: Azure Media Services
description: Bu konu, Video Dizinleyici sahneleri, çekimler ve anahtar kareler hakkında genel bir bakış sağlar.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 07/05/2019
ms.author: juliako
ms.openlocfilehash: a833fd808049cfce95b182910e50e38d3c39f4e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245947"
---
# <a name="scenes-shots-and-keyframes"></a>Sahneler, çekimler, ana kareler

Video Indexer, videoları yapısal ve anlamsal özelliklere dayalı olarak zamansal birimlere bölmeyi destekler. Bu özellik, müşterilerin farklı parçalı durumları temel alınarak video içeriğine kolayca göz atmalarını, yönetmelerini ve bunları yönetmelerini sağlar. Örneğin, bu konuda açıklanan sahnelere, çekimlere ve anahtar karelere dayanır.   

![Sahneler, çekimler, ana kareler](./media/scenes-shots-keyframes/scenes-shots-keyframes.png)
 
## <a name="scene-detection"></a>Sahne algılama  
 
Video Indexer, görsel ipuçlarına göre videoda bir sahnenin ne zaman değişer olduğunu belirler. Bir sahne tek bir olayı betimler ve bir dizi ardışık çekimden oluşur, bunlar anlamsal olarak ilişkilidir. Sahne küçük resmi, altta yatan çekimin ilk anahtar çerçevesidir. Video dizinleyici, bir videoyu ardışık çekimlerarasında renk tutarlılığına dayalı sahnelere böler ve her sahnenin başlangıç ve bitiş saatini alır. Videoların anlamsal yönlerini ölçmeyi içerdiğinden sahne algılama zor bir görev olarak kabul edilir.

> [!NOTE]
> En az 3 sahne içeren videolar için geçerlidir.

## <a name="shot-detection"></a>Atış algılama

Video Indexer, bir çekimin görsel ipuçlarına göre ne zaman değiştiğini, bitişik çerçevelerin renk düzenindeki ani ve aşamalı geçişleri izleyerek belirler. Çekimin meta verileri, başlangıç ve bitiş saatinin yanı sıra çekimde yer alan anahtar karelerin listesini içerir. Çekimler aynı anda aynı kameradan çekilen ardışık karelerdir.

## <a name="keyframe-detection"></a>Anahtar kare algılama

Video Indexer, her çekimi en iyi temsil eden kare(ler)'i seçer. Anahtar kareler, videonun tamamından estetik özelliklere (örneğin kontrast ve kararlılık) göre seçilen temsili çerçevelerdir. Video Indexer, müşterilerin anahtar çerçeveyi yüksek çözünürlüklü bir görüntü olarak ayıklayabildiği metne dayalı olarak çekimin meta verilerinin bir parçası olarak anahtar kare lidlerin bir listesini alır.  

### <a name="extracting-keyframes"></a>Anahtar Kareleri Ayıklama

Videonuz için yüksek çözünürlüklü anahtar kareleri ayıklamak için önce videoyu yüklemeniz ve dizine dizine dizine yüklemeniz gerekir.

![Anahtar Kareler](./media/scenes-shots-keyframes/extracting-keyframes.png)

#### <a name="with-the-video-indexer-website"></a>Video Indexer web sitesi ile

Video Indexer web sitesini kullanarak anahtar kareleri ayıklamak için videonuzu yükleyin ve dizine ekleyin. Dizin oluşturma işi tamamlandıktan sonra **İndir** düğmesine tıklayın ve **Artefakt (ZIP) seçeneğini belirleyin.** Bu, yapılar klasörünü bilgisayarınıza indirir. 

![Anahtar Kareler](./media/scenes-shots-keyframes/extracting-keyframes2.png)
 
Zip'i açın ve klasörü açın. *_KeyframeThumbnail* klasöründe ve videonuzdan çıkarılan tüm anahtar kareleri bulacaksınız. 

#### <a name="with-the-video-indexer-api"></a>Video Dizinleyici API ile

Video Indexer API'sini kullanarak anahtar kareler almak için [Video Yükle'yi](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Upload-Video?) kullanarak videonuzu yükleyin ve dizine ekleyin. Dizin oluşturma işi tamamlandıktan sonra [Video Dizini Al'ı](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Video-Index?)arayın. Bu, Video Indexer'ın bir JSON dosyasında içeriğinizden çıkardığı tüm bilgileri size verecektir.  

Her çekimin meta verilerinin bir parçası olarak anahtar kare lidlerin bir listesini alırsınız. 

```json
"shots":[  
    {  
      "id":0,
      "keyFrames":[  
          {  
            "id":0,
            "instances":[  
                {  
                  "thumbnailId":"00000000-0000-0000-0000-000000000000",
                  "start":"0:00:00.209",
                  "end":"0:00:00.251",
                  "duration":"0:00:00.042"
                }
            ]
          },
          {  
            "id":1,
            "instances":[  
                {  
                  "thumbnailId":"00000000-0000-0000-0000-000000000000",
                  "start":"0:00:04.755",
                  "end":"0:00:04.797",
                  "duration":"0:00:00.042"
                }
            ]
          }
      ],
      "instances":[  
          {  
            "start":"0:00:00",
            "end":"0:00:06.34",
            "duration":"0:00:06.34"
          }
      ]
    },

]
```

Artık bu anahtar kare lisenin her birini [Küçük Resimleri Al](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Video-Thumbnail?) çağrısında çalıştırmanız gerekir. Bu, anahtar kare görüntülerin her birini bilgisayarınıza indirir. 

## <a name="editorial-shot-type-detection"></a>Editoryal atış türü algılama

Anahtar kareler çıkış JSON çekim ile ilişkilidir. 

JSON'Un editoryal türünü temsil ettiği kavrayışlarda tek bir çekimle ilişkili çekim türü. Videoları kliplere, fragmanlara düzenlerken veya sanatsal amaçlar için belirli bir anahtar çerçevesi stilini ararken bu çekim türü özelliklerini yararlı bulabilirsiniz. Farklı türler, her çekimin ilk anahtar çerçevesinin analizine göre belirlenir. Çekimler, ilk anahtar karelerinde görünen yüzlerin ölçeğine, boyutuna ve konumuna göre tanımlanır. 

Çekim boyutu ve ölçeği, kamera ile çerçevede görünen yüzler arasındaki mesafeye göre belirlenir. Bu özellikleri kullanarak, Video Dizinleyici aşağıdaki çekim türlerini algılar:

* Geniş: bütün bir kişinin vücudunu gösterir.
* Orta: bir kişinin üst vücut ve yüz gösterir.
* Yakın çekim: esas olarak bir kişinin yüzünü gösterir.
* Aşırı yakın çekim: ekranı dolduran bir kişinin yüzünü gösterir. 

Çekim türleri, çerçevenin ortasına göre konu karakterlerinin konumuna göre de belirlenebilir. Bu özellik, Video Dizinleyici'de aşağıdaki çekim türlerini tanımlar:

* Sol yüz: çerçevenin sol tarafında bir kişi belirir.
* Orta yüz: Çerçevenin orta bölgesinde bir kişi görünür.
* Sağ yüz: bir kişi çerçevenin sağ tarafında görünür.
* Açık hava: bir kişi açık havada görünür.
* Kapalı: bir kişi kapalı bir ortamda görünür.

Ek özellikler:

* İki çekim: orta boy iki kişinin yüzlerini gösterir.
* Birden fazla yüz: ikiden fazla kişi.


## <a name="next-steps"></a>Sonraki adımlar

[API tarafından üretilen Video İndeksleyici çıktısını inceleyin](video-indexer-output-json-v2.md#scenes)
