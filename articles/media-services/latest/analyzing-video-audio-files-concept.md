---
title: Video ve ses dosyalarını çözümleme
titleSuffix: Azure Media Services
description: Azure Media Services ' de Audioanalizzerönayar ve Videoanalizzerönayar kullanarak ses ve video içeriğini çözümlemeyi öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/30/2020
ms.author: juliako
ms.openlocfilehash: 1d28fc37b98493322b9e201ac899b7911dd1d705
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76988354"
---
# <a name="analyze-video-and-audio-files-with-azure-media-services"></a>Video ve ses dosyalarını Azure Media Services Çözümleme

Azure Media Services v3, video ve ses dosyalarından Video Indexer ile öngörüleri ayıklamanıza olanak tanır. Bu makalede, bu öngörüleri ayıklamak için kullanılan Media Services v3 Analyzer önayarları açıklanmaktadır. Daha ayrıntılı içgörüler istiyorsanız doğrudan Video Indexer’ı kullanın. Video Indexer ve Media Services çözümleyici önayarlarının ne zaman kullanılacağını anlamak için [karşılaştırma belgesine](../video-indexer/compare-video-indexer-with-media-services-presets.md)göz atın.

Media Services v3 ön ayarlarını kullanarak içeriğinizi analiz etmek için bir **dönüşüm** oluşturur ve şu önayarlardan birini kullanan bir **Iş** gönderebilirsiniz: [Videoanalizzerönayar](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#videoanalyzerpreset) veya **audioanalizzerönayar**. **Videoanalizzerönayar**'in nasıl kullanılacağını gösteren bir öğretici için bkz. [Azure Media Services videoları analiz etme](analyze-videos-tutorial-with-api.md).

> [!NOTE]
> Video veya Ses Çözümleyicisi önayarlarını kullanırken, hesabınızı 10 S3 Medya Ayrılmış Birimine sahip olacak şekilde ayarlamak için Azure portalı kullanın. Daha fazla bilgi için bkz. [Medya işlemeyi ölçeklendirme](media-reserved-units-cli-how-to.md).

## <a name="compliance-privacy-and-security"></a>Uyumluluk, gizlilik ve güvenlik

Önemli bir anımsatıcı olarak, Video Indexer ortamınızda geçerli olan tüm yasalara uymak zorundasınız ve Video Indexer ya da başka bir Azure hizmetini diğerlerinin haklarını ihlal eden veya başkalarına zararlı olabilecek bir biçimde kullanamazsınız. Biyometrik veriler de dahil olmak üzere herhangi bir videoyu, işleme ve depolama için Video Indexer hizmetine yüklemeden önce, videodaki her türden uygun tüm haklara sahip olmanız gerekir. Microsoft bilişsel [Hizmetler koşulları](https://azure.microsoft.com/support/legal/cognitive-services-compliance-and-privacy/)video Indexer, uyumluluk, gizlilik ve güvenlik hakkında bilgi edinmek için. Microsoft 'un gizlilik yükümlülüklerini ve verilerinizi işleme için lütfen Microsoft 'un [Gizlilik bildirimini](https://privacy.microsoft.com/PrivacyStatement), [çevrimiçi hizmetler koşullarını](https://www.microsoft.com/licensing/product-licensing/products) ("OST") ve [veri Işleme eki](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) 'ni ("DPA") gözden geçirin. Veri saklama, silme/yok etme dahil olmak üzere ek gizlilik bilgileri, OST ve [burada](../video-indexer/faq.md)bulunabilir. Video Indexer kullanarak bilişsel hizmetler koşulları, OST, DPA ve gizlilik bildirimiyle bağlanmayı kabul etmiş olursunuz.

## <a name="built-in-presets"></a>Yerleşik hazır

Media Services Şu anda aşağıdaki yerleşik çözümleyici önayarlarını desteklemektedir:  

|**Önceden tanımlı ayar adı**|**Senaryo**|**Ayrıntılar**|
|---|---|---|
|[Audioanaliz Zerönayar](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#audioanalyzerpreset)|Ses çözümleme|Önayar, konuşma dökümü dahil olmak üzere, önceden tanımlanmış bir AI tabanlı analiz işlemleri kümesi uygular. Şu anda önayar, tek bir dilde konuşmayı içeren tek bir ses izlemeli içeriği işlemeyi destekler. Girişte ses yükünün dilini ' Language Tag-Region ' BCP-47 biçimini kullanarak belirtebilirsiniz. Desteklenen diller Ingilizce (' en-US ' ve ' en-GB '), Ispanyolca (' es-ES ' ve ' es-MX '), Fransızca (' fr-FR '), Italyanca (' It-IT '), Japonca (' ja-JP '), Portekizce (' PT-BR '), Çince (' zh-CN '), Almanca (' de-DE '), Arapça (' ar-EG ' ve ' ar-SY '), Rusça (' ru-RU '), Hintçe (' Hi-IN ') ve Korece (' ko-KR ').<br/><br/> Dil belirtilmezse veya null olarak ayarlanırsa, otomatik dil algılama algılanan ilk dili seçer ve dosya süresince seçili dile devam eder. Otomatik dil algılama özelliği şu anda Ingilizce, Çince, Fransızca, Almanca, Italyanca, Japonca, Ispanyolca, Rusça ve Portekizce desteklemektedir. İlk dil algılandıktan sonra diller arasında dinamik geçiş yapmayı desteklemez. Otomatik dil algılama özelliği, açık bir konuşma sayesinde ses kayıtları ile en iyi şekilde işe yarar. Otomatik dil algılama dili bulamazsa, geri bulma işlemi Ingilizce 'ye döner.|
|[Videoanaliz Zerönayar](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#videoanalyzerpreset)|Ses ve video çözümleme|Hem ses hem de videodan Öngörüler (zengin meta veriler) ayıklar ve bir JSON biçim dosyası verir. Video dosyasını işlerken yalnızca ses öngörülerini ayıklamak isteyip istemediğinizi belirtebilirsiniz. Daha fazla bilgi için bkz. [video çözümleme](analyze-videos-tutorial-with-api.md).|
|[Facedetectorönayar](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#facedetectorpreset)|Videoda bulunan yüzler algılanıyor|Bir video çözümlenirken, mevcut tüm yüzleri tespit etmek için kullanılacak ayarları açıklar.|

### <a name="audioanalyzerpreset"></a>Audioanaliz Zerönayar

Önceden ayarlanmış, bir ses veya video dosyasından birden çok ses öngörülerini ayıklamanızı sağlar. Çıktı, ses dökümü için bir JSON dosyası (tüm Öngörüler ile) ve VTT dosyasını içerir. Bu önayar, bir [BCP47](https://tools.ietf.org/html/bcp47) dizesi biçimindeki giriş dosyasının dilini belirten bir özelliği kabul eder. Ses öngörüleri şunları içerir:

* **Ses dökümü**: zaman damgalarına sahip konuşulan sözcüklerin dökümünü alma. Birden çok dil desteklenir.
* **Konuşmacı dizin oluşturma**: konuşmacıları ve ilgili konuşulan kelimeleri eşleme.
* **Konuşma yaklaşımı Analizi**: ses dökümü üzerinde gerçekleştirilen yaklaşım analizinin çıktısı.
* **Anahtar sözcükler**: ses dökümden ayıklanan anahtar sözcükler.

### <a name="videoanalyzerpreset"></a>Videoanaliz Zerönayar

Önceden ayarlanmış, bir video dosyasından birden çok ses ve video öngörüleri ayıklamanızı sağlar. Çıktı, bir JSON dosyası (tüm içgörüler ile), video dökümü için bir VTT dosyası ve küçük resim koleksiyonu içerir. Bu önayar Ayrıca özellik olarak bir [BCP47](https://tools.ietf.org/html/bcp47) dizesi (videonun dilini temsil eder) kabul eder. Video öngörüleri yukarıda belirtilen tüm ses öngörülerini ve aşağıdaki ek öğeleri içerir:

* **Yüz izleme**: videoda yüzlerin bulunduğu zaman. Her yüz bir yüz KIMLIĞI ve karşılık gelen bir küçük resim koleksiyonuna sahiptir.
* **Görsel metin**: optik karakter tanıma aracılığıyla algılanan metin. Metin zaman damgalı olur ve anahtar sözcükleri ayıklamak için de kullanılır (ses dökümü dosyasına ek olarak).
* **Ana kareler**: videodan çıkarılan bir ana kare koleksiyonu.
* **Görsel içerik**denetimi: videoların, yetişkin veya Dicy olarak işaretlenen kısmı.
* **Ek açıklama**: önceden tanımlanmış bir nesne modeline göre videolara açıklama ekleme sonucu

## <a name="insightsjson-elements"></a>Insights. JSON öğeleri

Çıktı, video veya ses üzerinde bulunan tüm öngörülere sahip bir JSON dosyası (Insights. JSON) içerir. JSON aşağıdaki öğeleri içerebilir:

### <a name="transcript"></a>döküm

|Ad|Açıklama|
|---|---|
|id|Satır KIMLIĞI.|
|metin|Dökümü.|
|language|Döküm dili. Her satırın farklı bir dile sahip olduğu yazılı betiği desteklemeye yöneliktir.|
|örnekler|Bu satırın göründüğü zaman aralıklarının listesi. Örnek TRANSCRIPT ise, yalnızca 1 örneğe sahip olur.|

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

### <a name="ocr"></a>OCR

|Ad|Açıklama|
|---|---|
|id|OCR satır KIMLIĞI.|
|metin|OCR metni.|
|likli|Tanıma güvenilirliği.|
|language|OCR dili.|
|örnekler|Bu OCR 'nin göründüğü zaman aralıklarının listesi (aynı OCR birden çok kez görünebilir).|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 0.91,
      "language": "en-US",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    },
    {
      "id": 1,
      "text": "NOTICIAS EN VIVO",
      "confidence": 0.9,
      "language": "es-ES",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:28"
        },
        {
          "start": "00:00:32",
          "end": "00:00:38"
        }
      ]
    }
  ],
```

### <a name="faces"></a>leri

|Ad|Açıklama|
|---|---|
|id|Yüz KIMLIĞI.|
|ad|Yüz adı. ' Bilinmeyen #0 ', tanımlanan bir ünlüde veya müşterinin eğitilen bir kişi olabilir.|
|likli|Yüz tanıma kimlik güveni.|
|açıklama|Ünlüğün açıklaması. |
|thumbnailId|Bu yüzün küçük resminin KIMLIĞI.|
|Knownpersonıd|İç KIMLIK (bilinen bir kişiyse).|
|Referenceıd|Bing KIMLIĞI (bir Bing ünlüğise).|
|referenceType|Şu anda yalnızca Bing.|
|başlık|Başlık (örneğin, "Microsoft 'un CEO 'SU").|
|ImageUrl|Bir ünlüğlık ise resim URL 'SI.|
|örnekler|Yüzün verilen zaman aralığında göründüğü örnekler. Her örneğin bir thumbnailsId de vardır. |

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

### <a name="shots"></a>görüntüleri

|Ad|Açıklama|
|---|---|
|id|Görüntü KIMLIĞI.|
|Gezen|Görüntü içindeki anahtar çerçevelerinin listesi (her birinin bir KIMLIĞI ve bir örnek zaman aralığı listesi vardır). Anahtar çerçeve örneklerinin, ana karenin küçük resim KIMLIĞI olan bir thumbnailId alanı vardır.|
|örnekler|Bu görüntüsündeki zaman aralıklarının bir listesi (görüntüleri yalnızca 1 örneğe sahiptir).|

```json
"Shots": [
    {
      "id": 0,
      "keyFrames": [
        {
          "id": 0,
          "instances": [
            {
                "thumbnailId": "00000000-0000-0000-0000-000000000000",
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
            "thumbnailId": "00000000-0000-0000-0000-000000000000",  
          "start": "00: 00: 00.2000000",
          "end": "00: 00: 05.0330000"
        }
      ]
    },
    {
      "id": 1,
      "keyFrames": [
        {
          "id": 1,
          "instances": [
            {
                "thumbnailId": "00000000-0000-0000-0000-000000000000",      
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
      "thumbnailId": "00000000-0000-0000-0000-000000000000",
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```

### <a name="statistics"></a>girecek

|Ad|Açıklama|
|---|---|
|Yazışma sayısı|Videodaki yazışma sayısı.|
|WordCount|Konuşmacı başına sözcük sayısı.|
|Hoparlörkernumberoffragments|Konuşmacının videoda bulunduğu parçaların miktarı.|
|SpeakerLongestMonolog|Hoparlörün en uzun monolog. Konuşmacı, monolog içinde susraysa dahil edilmiştir. Monolog 'in başındaki ve sonundaki sessizlik kaldırılır.|
|Hoparlörkertalktolistenratio|Hesaplama, konuşmacının monolog harcanan zamanına (arasında sessizlik olmadan), videonun toplam süresine göre bölünür. Saat, üçüncü ondalık noktaya yuvarlanır.|


### <a name="sentiments"></a>yaklaşımları

Sentiments, sentimentType alanı tarafından toplanır (pozitif/nötr/negatif). Örneğin, 0-0.1, 0,1-0.2.

|Ad|Açıklama|
|---|---|
|id|Yaklaşım KIMLIĞI.|
|averageScore |Bu yaklaşım türünün tüm örneklerinin ortalaması-pozitif/nötr/negatif|
|örnekler|Bu yaklaşım görüntülenen zaman aralıklarının bir listesi.|
|sentimentType |Tür ' pozitif ', ' Nötr ' veya ' negative ' olabilir.|

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

### <a name="labels"></a>etiketleri

|Ad|Açıklama|
|---|---|
|id|Etiket KIMLIĞI.|
|ad|Etiket adı (örneğin, ' bilgisayar ', ' TV ').|
|language|Etiket adı dili (çevrildiğinde). BCP-47|
|örnekler|Bu etiketin göründüğü zaman aralıklarının listesi (bir etiket birden çok kez görünebilir). Her örneğin bir güvenirlik alanı vardır. |

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

### <a name="keywords"></a>anahtar sözcükler

|Ad|Açıklama|
|---|---|
|id|Anahtar sözcük KIMLIĞI.|
|metin|Anahtar sözcük metni.|
|likli|Anahtar sözcüğünün tanıma güvenilirliği.|
|language|Anahtar sözcük dili (çevrildiğinde).|
|örnekler|Bu anahtar sözcüğünün göründüğü zaman aralıklarının listesi (bir anahtar sözcük birden çok kez görünebilir).|

```json
"keywords": [
{
    "id": 0,
    "text": "office",
    "confidence": 1.6666666666666667,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    },
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    }
    ]
},
{
    "id": 1,
    "text": "icons",
    "confidence": 1.4,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    },
    {
        "start": "00:00:13.9900000",
        "end": "00:00:15.6100000"
    }
    ]
}
] 
```

#### <a name="visualcontentmoderation"></a>Visualcontentdenetlemesi

Visualcontentmoderblock blok, büyük olasılıkla yetişkinlere yönelik içeriğe sahip Video Indexer bulunan zaman aralıklarını içerir. Visualcontentdenetlemesi boşsa, tanımlı yetişkin içerik yoktur.

Yetişkin veya kcy içeriği içeren videolar yalnızca özel görünüm için kullanılabilir olabilir. Kullanıcılar içeriğin insan incelemesi için bir istek gönderebilir, bu durumda `IsAdult` özniteliği insan incelemesi sonucunu içerecektir.

|Ad|Açıklama|
|---|---|
|id|Görsel içerik denetleme KIMLIĞI.|
|adultScore|Yetişkin puanı (İçerik Yöneticisi 'nden).|
|Oycyscore|Yağanın puanı (içerik denetleme).|
|örnekler|Bu görsel içerik denetimi 'nin göründüğü zaman aralıklarının listesi.|

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
## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Azure Media Services videoları analiz etme](analyze-videos-tutorial-with-api.md)
