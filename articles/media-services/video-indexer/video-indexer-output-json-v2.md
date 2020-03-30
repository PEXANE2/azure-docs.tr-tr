---
title: v2 API tarafından üretilen Video Dizinleyici çıktısını inceleyin - Azure
titleSuffix: Azure Media Services
description: Bu konu, v2 API tarafından üretilen Azure Medya Hizmetleri Video Dizinleyici çıktısını inceler.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 12/09/2019
ms.author: juliako
ms.openlocfilehash: 2fac5e07f9646c4fc0fac7b1be53b5a5ac1ea803
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245934"
---
# <a name="examine-the-video-indexer-output-produced-by-api"></a>API tarafından üretilen Video İndeksleyici çıktısını inceleyin

**Video Dizini Al** API'yi aradiğinizde ve yanıt durumu iyi olduğunda, yanıt içeriği olarak ayrıntılı bir JSON çıktısı alırsınız. JSON içeriği, belirtilen video istatistiklerinin ayrıntılarını içerir. Kavrayışlar şunlardır: transkriptler, OK'ler, yüzler, konular, bloklar, vb. Her içgörü türü, içgörünün videoda ne zaman göründüğünü gösteren zaman aralıkları örneklerini içerir. 

1. JSON dosyasını almak için [Video Dizini API'sini Al'ı](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Video-Index?) arayın
1. Belirli eserlerle de ilgileniyorsanız, [Video Artifact İndir URL API'sını](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Video-Artifact-Download-Url?) arayın

    API çağrısında, istenen yapı türünü (OCR, Yüzler, Anahtar çerçeveler vb.) belirtin

Ayrıca [Video Indexer](https://www.videoindexer.ai/) web sitesindeki videonun **Oynat** düğmesine basarak videonun özetlenen görüşlerini görsel olarak inceleyebilirsiniz. Daha fazla bilgi için [bkz.](video-indexer-view-edit.md)

![Insights](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

Bu makalede, **Video Dizini API'si Alın** tarafından döndürülen JSON içeriği incelen. 

> [!NOTE]
> Video Indexer'daki tüm erişim belirteçlerinin sona ermesi bir saattir.


## <a name="root-elements"></a>Kök elementleri

|Adı|Açıklama|
|---|---|
|accountId|Çalma listesinin VI hesap kimliği.|
|id|Çalma listesinin kimliği.|
|ad|Çalma listesinin adı.|
|açıklama|Çalma listesinin açıklaması.|
|userName|Çalma listesini oluşturan kullanıcının adı.|
|Oluşturulan|Çalma listesinin oluşturma zamanı.|
|privacyMode|Çalma listesinin gizlilik modu (Özel/Genel).|
|durum|Çalma listesi (yüklenen, işlenen, işlenen, başarısız, karantinaya alınmış).|
|isOwned|Çalma listesinin geçerli kullanıcı tarafından oluşturulup oluşturulmadığını gösterir.|
|ıseditable|Geçerli kullanıcının çalma listesini atmaya yetkili olup olmadığını gösterir.|
|isBase|Oynatma listesinin temel oynatma listesi (video) veya diğer videolardan (türetilmiş) yapılmış bir oynatma listesi olup olmadığını gösterir.|
|süreInSeconds|Çalma listesinin toplam süresi.|
|özetInsights|Bir [özetInsights](#summarizedinsights)içerir.
|videolar|Oynatma listesini oluşturan [videoların](#videos) listesi.<br/>Diğer videoların (türetilmiş) zaman aralıklarından oluşan bu oynatma listesi yalnızca dahil edilen zaman aralıklarından verileri içerir.|

```json
{
  "accountId": "bca61527-1221-bca6-1527-a90000002000",
  "id": "abc3454321",
  "name": "My first video",
  "description": "I am trying VI",
  "userName": "Some name",
  "created": "2018/2/2 18:00:00.000",
  "privacyMode": "Private",
  "state": "Processed",
  "isOwned": true,
  "isEditable": false,
  "isBase": false,
  "durationInSeconds": 120, 
  "summarizedInsights" : { . . . }
  "videos": [{ . . . }]
}
```

## <a name="summarizedinsights"></a>özetInsights

Bu bölümde, içgörülerin özeti gösterilmektedir.

|Öznitelik | Açıklama|
|---|---|
|ad|Videonun adı. Örneğin, Azure Monitör.|
|id|Videonun kimliği. Örneğin, 63c6d532ff.|
|privacyMode|Dökümünüz aşağıdaki modlardan birine sahip olabilir: **Özel**, **Genel**. **Herkese açık** - video hesabınızdaki herkes ve videoyla bağlantısı olan herkes tarafından görülebilir. **Özel** - video hesabınızdaki herkes tarafından görülebilir.|
|süre|Bir öngörünün oluştuğu zamanı açıklayan bir süre içerir. Süre saniye cinsinden.|
|küçük resimVideoId|Küçük resmin alındığı videonun kimliği.
|küçük resimId|Videonun küçük resmi. Gerçek küçük resmi almak için [Get-Thumbthumb'ı](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Video-Thumbnail) arayın ve küçük resmi VideoId ve küçük resmi aktarın.|
|Yüz|Sıfır veya daha fazla yüz içerebilir. Daha ayrıntılı bilgi için [yüzler](#faces)görün.|
|anahtar sözcükler|Sıfır veya daha fazla anahtar kelime içerebilir. Daha ayrıntılı bilgi için [anahtar kelimelere](#keywords)bakın.|
|Duyguları|Sıfır veya daha fazla duyarlılık içerebilir. Daha ayrıntılı bilgi için, [bkz.](#sentiments)|
|audioEffects| Sıfır veya daha fazla ses Efekti içerebilir. Daha ayrıntılı bilgi için [audioEffects'e](#audioEffects)bakın.|
|Etiket| Sıfır veya daha fazla etiket içerebilir. Ayrıntılı bilgi için [etiketlere](#labels)bakın.|
|Marka| Sıfır veya daha fazla marka içerebilir. Daha ayrıntılı bilgi için [markalara](#brands)bakın.|
|istatistikler | Daha ayrıntılı bilgi için [istatistiklere](#statistics)bakın.|
|Duyguları| Sıfır veya daha fazla duygu içerebilir. Daha ayrıntılı bilgi için, [duygulara](#emotions)bakın.|
|konuları|Sıfır veya daha fazla konu içerebilir. [Konular](#topics) içgörü.|

## <a name="videos"></a>videolar

|Adı|Açıklama|
|---|---|
|accountId|Videonun VI hesap kimliği.|
|id|Videonun kimliği.|
|ad|Videonun adı.
|durum|Videonun durumu (yüklenen, işlenen, işlenen, başarısız olan, karantinaya alınmış).|
|processingİlerleme|İşlem sırasında işlem ilerlemesi (örneğin, %20).|
|failureCode|İşleme başarısız olduysa hata kodu (örneğin, 'DesteklenmeyenFileType').|
|failureMessage|İşleme başarısız olursa hata iletisi.|
|externalId|Videonun harici kimliği (kullanıcı tarafından belirtilmişse).|
|externalUrl|Videonun harici url'si (kullanıcı tarafından belirtilmişse).|
|meta veriler|Videonun dış meta verileri (kullanıcı tarafından belirtilirse).|
|isAdult|Videonun el ile incelenip yetişkin videosu olarak tanımlanıp tanımlanmadığını gösterir.|
|Anlayış|Anlayışlar nesne. Daha fazla bilgi için [bkz.](#insights)|
|küçük resimId|Videonun küçük resmi. Gerçek küçük resmi almak için [Get-Thumbnail'i](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Video-Thumbnail) arayın ve video kimliğini ve küçük resmi kartı iletin.|
|publishedUrl|Videoyu yayınlamak için bir url.|
|yayınlandıUrlProxy|Videoyu (Apple aygıtları için) aktarmak için bir url.|
|görünümToken|Videoyu akışlamak için kısa ömürlü bir görünüm belirteci.|
|sourceLanguage|Videonun kaynak dili.|
|language|Videonun gerçek dili (çeviri).|
|indexingPreset|Videoyu dizine dizine koymak için kullanılan ön ayar.|
|streamingPreset|Videoyu yayınlamak için kullanılan ön ayar.|
|dilselModelId|Videoyu yazıya çıkarmak için kullanılan CRIS modeli.|
|istatistikler | Daha fazla bilgi için [istatistiklere](#statistics)bakın.|

```json
{
    "videos": [{
        "accountId": "2cbbed36-1972-4506-9bc7-55367912df2d",
        "id": "142a356aa6",
        "state": "Processed",
        "privacyMode": "Private",
        "processingProgress": "100%",
        "failureCode": "General",
        "failureMessage": "",
        "externalId": null,
        "externalUrl": null,
        "metadata": null,
        "insights": {. . . },
        "thumbnailId": "89d7192c-1dab-4377-9872-473eac723845",
        "publishedUrl": "https://videvmediaservices.streaming.mediaservices.windows.net:443/d88a652d-334b-4a66-a294-3826402100cd/Xamarine.ism/manifest",
        "publishedProxyUrl": null,
        "viewToken": "Bearer=<token>",
        "sourceLanguage": "En-US",
        "language": "En-US",
        "indexingPreset": "Default",
        "linguisticModelId": "00000000-0000-0000-0000-000000000000"
    }],
}
```
### <a name="insights"></a>Anlayış

Her kavrayış (örneğin, transkript satırları, yüzler, markalar, vb.), benzersiz öğelerin bir listesini içerir (örneğin, face1, face2, face3) ve her öğenin kendi meta verileri ve örneklerinin bir listesi vardır (ek isteğe bağlı meta verilerle birlikte zaman aralıkları).

Bir yüzün kimliği, adı, küçük resmi, diğer meta verileri ve geçici örneklerinin bir listesi olabilir (örneğin: 00:00:05 – 00:00:10, 00:01:00 - 00:02:30 ve 00:41:21 - 00:41:49.) Her zamansal örnek ek meta veri olabilir. Örneğin, yüzün dikdörtgen koordinatları (20,230,60,60).

|Sürüm|Kod sürümü|
|---|---|
|sourceLanguage|Videonun kaynak dili (bir ana dil varsayarak). [BCP-47](https://tools.ietf.org/html/bcp47) dizesi şeklinde.|
|language|Kavrayışlar dili (kaynak dilden çevrilmiştir). [BCP-47](https://tools.ietf.org/html/bcp47) dizesi şeklinde.|
|Transkript|[Transkript](#transcript) içgörüsü.|
|Ocr|[OCR](#ocr) içgörüsü.|
|anahtar sözcükler|[Anahtar kelimeler](#keywords) içgörü.|
|bloklar|Bir veya daha fazla [blok](#blocks) içerebilir|
|Yüz|[Yüzler](#faces) içgörü.|
|Etiket|[Etiketlerin](#labels) içgörüsü.|
|Çekim|[Çekim](#shots) içgörü.|
|Marka|[Markalar](#brands) içgörü.|
|audioEffects|[AudioEffects](#audioEffects) içgörü.|
|Duyguları|[Duygusallık](#sentiments) içgörüsü.|
|visualContentModeration|[visualContentModeration](#visualcontentmoderation) içgörü.|
|textualContentModeration|[TextualContentModeration](#textualcontentmoderation) içgörü.|
|Duyguları| [Duyguların](#emotions) içgörüsü.|
|konuları|[Konular](#topics) içgörü.|

Örnek:

```json
{
  "version": "0.9.0.0",
  "sourceLanguage": "en-US",
  "language": "es-ES",
  "transcript": ...,
  "ocr": ...,
  "keywords": ...,
  "faces": ...,
  "labels": ...,
  "shots": ...,
  "brands": ...,
  "audioEffects": ...,
  "sentiments": ...,
  "visualContentModeration": ...,
  "textualContentModeration": ...
}
```

#### <a name="blocks"></a>bloklar

Öznitelik | Açıklama
---|---
id|Bloğun kimliği.|
Örnek|Bu bloğun zaman aralıklarının listesi.|

#### <a name="transcript"></a>Transkript

|Adı|Açıklama|
|---|---|
|id|Hat kimliği.|
|metin|Transkriptin kendisi.|
|language|Transkript dili. Her satırın farklı bir dile sahip olabileceği transkripti desteklemek için tasarlanmıştır.|
|Örnek|Bu çizginin göründüğü zaman aralıklarının listesi. Örnek transkript ise, yalnızca 1 örneği olacaktır.|

Örnek:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

#### <a name="ocr"></a>Ocr

|Adı|Açıklama|
|---|---|
|id|OCR satır kimliği.|
|metin|OCR metni.|
|güvenilirlik|Tanıma güveni.|
|language|OCR dili.|
|Örnek|Bu OCR'nin göründüğü zaman aralıklarının listesi (aynı OCR birden çok kez görünebilir).|
|yükseklik|OCR dikdörtgeninin yüksekliği|
|üst|px'te en üst teki konum|
|left| px'te sol konum|
|genişlik|OCR dikdörtgeninin genişliği|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 675.971,
      "height": 35,
      "language": "en-US",
      "left": 31,
      "top": 97,
      "width": 400,      
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    }
  ],
```

#### <a name="keywords"></a>anahtar sözcükler

|Adı|Açıklama|
|---|---|
|id|Anahtar kelime kimliği.|
|metin|Anahtar kelime metni.|
|güvenilirlik|Anahtar kelimenin tanıma güveni.|
|language|Anahtar kelime dili (çevrildiğinde).|
|Örnek|Bu anahtar kelimenin göründüğü zaman aralıklarının listesi (bir anahtar kelime birden çok kez görünebilir).|

```json
{
    id: 0,
    text: "technology",
    confidence: 1,
    language: "en-US",
    instances: [{
            adjustedStart: "0:05:15.782",
            adjustedEnd: "0:05:16.249",
            start: "0:05:15.782",
            end: "0:05:16.249"
    },
    {
            adjustedStart: "0:04:54.761",
            adjustedEnd: "0:04:55.228",
            start: "0:04:54.761",
            end: "0:04:55.228"
    }]
}
```

#### <a name="faces"></a>Yüz

|Adı|Açıklama|
|---|---|
|id|Yüz kimliği.|
|ad|Yüzün adı. Bu 'Bilinmeyen #0, tanımlanmış bir ünlü veya bir müşteri eğitimli kişi olabilir.|
|güvenilirlik|Yüz tanımlama güveni.|
|açıklama|Ünlünün tarifi. |
|küçük resimId|O yüzün küçük resminin kimliği.|
|bilinen PersonId|Bilinen bir kişiise, dahili kimliği.|
|referenceId|Bing ünlüsüyse, Bing kimliği.|
|referenceType|Şu anda, sadece Bing.|
|başlık|Ünlü yse, başlığı (örneğin "Microsoft'un CEO'su").|
|ımageurl|Bir ünlü ise, onun görüntü url.|
|Örnek|Bunlar, yüzün verilen zaman aralığında ortaya çıktığı durumlardır. Her örnekte bir küçük resim de vardır. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

#### <a name="labels"></a>Etiket

|Adı|Açıklama|
|---|---|
|id|Etiket kimliği.|
|ad|Etiket adı (örneğin, 'Bilgisayar', 'TV').|
|language|Etiket adı dili (çevrildiğinde). BCP-47|
|Örnek|Bu etiketin göründüğü zaman aralıklarının listesi (etiket birden çok kez görünebilir). Her örneğin bir güven alanı vardır. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

#### <a name="scenes"></a>Sahne

|Adı|Açıklama|
|---|---|
|id|Olay yeri kimliği.|
|Örnek|Bu sahnenin zaman aralıklarının listesi (bir sahnenin yalnızca 1 örneği olabilir).|

```json
"scenes":[  
    {  
      "id":0,
      "instances":[  
          {  
            "start":"0:00:00",
            "end":"0:00:06.34",
            "duration":"0:00:06.34"
          }
      ]
    },
    {  
      "id":1,
      "instances":[  
          {  
            "start":"0:00:06.34",
            "end":"0:00:47.047",
            "duration":"0:00:40.707"
          }
      ]
    },

]
```

#### <a name="shots"></a>Çekim

|Adı|Açıklama|
|---|---|
|id|Çekim kimliği.|
|keyFrames|Çekimdeki anahtar Çerçevelerin listesi (her birinde bir kimlik ve örnek zaman aralıklarının listesi bulunur). Her keyFrame örneğinde, anahtar Çerçevesi'nin küçük resmi kimliğini tutan bir küçük resim kimliği bulunur.|
|Örnek|Bu çekimin zaman aralıklarının listesi (bir çekimde yalnızca 1 örnek olabilir).|

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

#### <a name="brands"></a>Marka

Metin transkript ve/veya Video OCR'ye konuşmada tespit edilen iş ve ürün marka adları. Bu, markaların görsel olarak tanınmasını veya logo algılamayı içermez.

|Adı|Açıklama|
|---|---|
|id|Marka kimliği.|
|ad|Markaların adı.|
|referenceId | Marka wikipedia url soneki. Örneğin, "Target_Corporation" sonektir. [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation)
|referenceUrl | Varsa markanın Wikipedia url'si. Örneğin, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|açıklama|Markaların açıklaması.|
|etiketler|Bu markayla ilişkili önceden tanımlanmış etiketlerin listesi.|
|güvenilirlik|Video Indexer marka dedektörünün güven değeri (0-1).|
|Örnek|Bu markanın zaman aralıklarının bir listesi. Her örnekte, bu markanın transkriptte mi yoksa OCR'de mi göründüğünü gösteren bir marka Türü vardır.|

```json
"brands": [
{
    "id": 0,
    "name": "MicrosoftExcel",
    "referenceId": "Microsoft_Excel",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft_Excel",
    "referenceType": "Wiki",
    "description": "Microsoft Excel is a sprea..",
    "tags": [],
    "confidence": 0.975,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 00: 31.3000000",
        "end": "00: 00: 39.0600000"
    }
    ]
},
{
    "id": 1,
    "name": "Microsoft",
    "referenceId": "Microsoft",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft",
    "description": "Microsoft Corporation is...",
    "tags": [
    "competitors",
    "technology"
    ],
    "confidence": 1.0,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 01: 44",
        "end": "00: 01: 45.3670000"
    },
    {
        "brandType": "Ocr",
        "start": "00: 01: 54",
        "end": "00: 02: 45.3670000"
    }
    ]
}
]
```

#### <a name="statistics"></a>istatistikler

|Adı|Açıklama|
|---|---|
|Yazışma Sayısı|Videodaki yazışma sayısı.|
|SözcüWordSayısı|Konuşmacı başına kelime sayısı.|
|HoparlörNumberOfFragments|Hoparlörün bir videoda sahip olduğu parça miktarı.|
|HoparlörLongestMonolog|Hoparlörün en uzun monologu. Hoparlör ün içinde sessizlik varsa, bu durum dahildir. Monologbaşında ve sonunda sessizlik kaldırılır.| 
|HoparlörTalktoListenRatio|Hesaplama, konuşmacının monologunda (arada sessizlik olmadan) harcanan süreye göre hesaplanır ve videonun toplam süresine bölünür. Zaman üçüncü ondalık sayıya yuvarlanır.|

#### <a name="audioeffects"></a><a id="audioEffects"/>audioEffects

|Adı|Açıklama|
|---|---|
|id|Ses efekti kimliği.|
|type|Ses efekti türü (örneğin, Alkışlama, Konuşma, Sessizlik).|
|Örnek|Bu ses efektinin göründüğü zaman aralıklarının listesi.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```

#### <a name="sentiments"></a>Duyguları

Duygular kendi sentimentType alanı (Pozitif / Nötr / Negatif) tarafından toplanır. Örneğin, 0-0.1, 0.1-0.2.

|Adı|Açıklama|
|---|---|
|id|Duygusal kimlik.|
|ortalamaPuan |Bu duyarlılık türünün tüm örneklerinin tüm puanlarının ortalaması - Pozitif/Nötr/Negatif|
|Örnek|Bu duygunun ortaya çıktığı zaman aralıklarının bir listesi.|
|sentimentType |Türü 'Pozitif', 'Nötr' veya 'Negatif' olabilir.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

#### <a name="visualcontentmoderation"></a>visualContentModeration

visualContentModeration bloğu, Video Indexer'ın yetişkin içeriğe sahip olabileceğini bulduğu zaman aralıklarını içerir. visualContentModeration boşsa, tanımlanan hiçbir yetişkin içeriği yoktur.

Yetişkinlere uygun veya müstehcen içerik içerdiği tespit edilebilen videolar yalnızca özel görünüm için kullanılabilir. Kullanıcılar, içeriğin insan tarafından incelenmesi için bir istek gönderme seçeneğine sahiptir, bu durumda IsAdult özniteliği insan incelemesinin sonucunu içerir.

|Adı|Açıklama|
|---|---|
|id|Görsel içerik moderasyon kimliği.|
|adultPuan|Yetişkin puanı (içerik moderatöründen).|
|racyScore|Müstehcen skor (içerik ılımlılığından).|
|Örnek|Bu görsel içerik ılımlılığı ortaya çıktı zaman aralıkları listesi.|

```json
"VisualContentModeration": [
{
    "id": 0,
    "adultScore": 0.00069,
    "racyScore": 0.91129,
    "instances": [
    {
        "start": "00:00:25.4840000",
        "end": "00:00:25.5260000"
    }
    ]
},
{
    "id": 1,
    "adultScore": 0.99231,
    "racyScore": 0.99912,
    "instances": [
    {
        "start": "00:00:35.5360000",
        "end": "00:00:35.5780000"
    }
    ]
}
] 
```

#### <a name="textualcontentmoderation"></a>textualContentModeration 

|Adı|Açıklama|
|---|---|
|id|Metinsel içerik moderasyon kimliği.|
|bannedWordsCount |Yasaklı kelimelerin sayısı.|
|bannedWordsRatio |Toplam sözcük sayısından oran.|

#### <a name="emotions"></a>Duyguları

Video Indexer konuşma ve ses ipuçlarına dayalı duyguları tanımlar. Tanımlanan duygu olabilir: sevinç, üzüntü, öfke, ya da korku.

|Adı|Açıklama|
|---|---|
|id|Duygu kimliği.|
|type|Konuşma ve ses ipuçlarına göre tanımlanan duygu anı. Duygu olabilir: sevinç, üzüntü, öfke, ya da korku.|
|Örnek|Bu duygunun ortaya çıktığı zaman aralıklarının bir listesi.|

```json
"emotions": [{
    "id": 0,
    "type": "Fear",
    "instances": [{
      "adjustedStart": "0:00:39.47",
      "adjustedEnd": "0:00:45.56",
      "start": "0:00:39.47",
      "end": "0:00:45.56"
    },
    {
      "adjustedStart": "0:07:19.57",
      "adjustedEnd": "0:07:23.25",
      "start": "0:07:19.57",
      "end": "0:07:23.25"
    }]
  },
  {
    "id": 1,
    "type": "Anger",
    "instances": [{
      "adjustedStart": "0:03:55.99",
      "adjustedEnd": "0:04:05.06",
      "start": "0:03:55.99",
      "end": "0:04:05.06"
    },
    {
      "adjustedStart": "0:04:56.5",
      "adjustedEnd": "0:05:04.35",
      "start": "0:04:56.5",
      "end": "0:05:04.35"
    }]
  },
  {
    "id": 2,
    "type": "Joy",
    "instances": [{
      "adjustedStart": "0:12:23.68",
      "adjustedEnd": "0:12:34.76",
      "start": "0:12:23.68",
      "end": "0:12:34.76"
    },
    {
      "adjustedStart": "0:12:46.73",
      "adjustedEnd": "0:12:52.8",
      "start": "0:12:46.73",
      "end": "0:12:52.8"
    },
    {
      "adjustedStart": "0:30:11.29",
      "adjustedEnd": "0:30:16.43",
      "start": "0:30:11.29",
      "end": "0:30:16.43"
    },
    {
      "adjustedStart": "0:41:37.23",
      "adjustedEnd": "0:41:39.85",
      "start": "0:41:37.23",
      "end": "0:41:39.85"
    }]
  },
  {
    "id": 3,
    "type": "Sad",
    "instances": [{
      "adjustedStart": "0:13:38.67",
      "adjustedEnd": "0:13:41.3",
      "start": "0:13:38.67",
      "end": "0:13:41.3"
    },
    {
      "adjustedStart": "0:28:08.88",
      "adjustedEnd": "0:28:18.16",
      "start": "0:28:08.88",
      "end": "0:28:18.16"
    }]
  }
],
```

#### <a name="topics"></a>konuları

Video Indexer transkriptlerden ana konuların çıkarımYapar. Mümkün olduğunda, 2. [IPTC](https://iptc.org/standards/media-topics/) 

|Adı|Açıklama|
|---|---|
|id|Konu kimliği.|
|ad|Konu adı, örneğin: "İlaç".|
|referenceId|Konular hiyerarşisini yansıtan ekmek kırıntıları. Örneğin: "Sağlık ve refah / Tıp ve sağlık / İlaç".|
|güvenilirlik|Aralığında güven puanı [0,1]. Daha yüksek daha emin.|
|language|Konubaşlığında kullanılan dil.|
|iptcName|Tespit edilirse IPTC medya kodu adı.|
|Örnek |Şu anda, Video Indexer zaman aralıklarına bir konu dizini yapmaz, bu nedenle tüm video aralık olarak kullanılır.|

```json
"topics": [{
    "id": 0,
    "name": "INTERNATIONAL RELATIONS",
    "referenceId": "POLITICS AND GOVERNMENT/FOREIGN POLICY/INTERNATIONAL RELATIONS",
    "referenceType": "VideoIndexer",
    "confidence": 1,
    "language": "en-US",
    "instances": [{
        "adjustedStart": "0:00:00",
        "adjustedEnd": "0:03:36.25",
        "start": "0:00:00",
        "end": "0:03:36.25"
    }]
}, {
    "id": 1,
    "name": "Politics and Government",
    "referenceType": "VideoIndexer",
    "iptcName": "Politics",
    "confidence": 0.9041,
    "language": "en-US",
    "instances": [{
        "adjustedStart": "0:00:00",
        "adjustedEnd": "0:03:36.25",
        "start": "0:00:00",
        "end": "0:03:36.25"
    }]
}]
. . .
```

## <a name="next-steps"></a>Sonraki adımlar

[Video Indexer Geliştirici Portalı](https://api-portal.videoindexer.ai)

Uygulamanıza widget'ları nasıl gömecekleriniz hakkında bilgi için, [uygulamalarınıza Video Dizinleyici widget'larını gömme](video-indexer-embed-widgets.md)katıştırma bilgisini görün. 

