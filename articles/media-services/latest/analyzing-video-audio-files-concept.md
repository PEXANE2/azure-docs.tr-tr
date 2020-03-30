---
title: Video ve ses dosyalarını analiz edin
titleSuffix: Azure Media Services
description: Azure Medya Hizmetlerinde AudioAnalyzerPreset ve VideoAnalyzerPreset'i kullanarak ses ve video içeriğini nasıl analiz edebilirsiniz öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269893"
---
# <a name="analyze-video-and-audio-files-with-azure-media-services"></a>Azure Medya Hizmetleri ile video ve ses dosyalarını analiz edin

Azure Media Services v3, Video Indexer ile video ve ses dosyalarınızdan öngörüler elde etmenizi sağlar. Bu makalede, bu öngörüleri ayıklamak için kullanılan Medya Hizmetleri v3 çözümleyici sayar açıklanır. Daha ayrıntılı içgörüler istiyorsanız doğrudan Video Indexer’ı kullanın. Video Indexer ve Medya Hizmetleri çözümleyicisi hazır ayarlarını ne zaman kullanacağımı anlamak için [karşılaştırma belgesine](../video-indexer/compare-video-indexer-with-media-services-presets.md)göz atın.

Medya Hizmetleri v3 hazır ayarlarını kullanarak içeriğinizi analiz etmek için, bir **Dönüşüm** oluşturur ve bu hazır ayarlardan birini kullanan bir **İş** gönderirsiniz: [VideoAnalyzerPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#videoanalyzerpreset) veya **AudioAnalyzerPreset**. **VideoAnalyzerPreset'in**nasıl kullanılacağını gösteren bir öğretici için Azure [Medya Hizmetleri ile videoları analiz](analyze-videos-tutorial-with-api.md)etme'ye bakın.

> [!NOTE]
> Video veya Ses Çözümleyicisi önayarlarını kullanırken, hesabınızı 10 S3 Medya Ayrılmış Birimine sahip olacak şekilde ayarlamak için Azure portalı kullanın. Daha fazla bilgi için bkz. [Medya işlemeyi ölçeklendirme](media-reserved-units-cli-how-to.md).

## <a name="compliance-privacy-and-security"></a>Uyumluluk, Gizlilik ve Güvenlik

Önemli bir hatırlatma olarak, Video Dizinleyici'yi kullanımınızda geçerli tüm yasalara uymanız gerekir ve Video Dizinleyici'yi veya başka bir Azure hizmetini başkalarının haklarını ihlal eden veya başkalarına zarar verebilecek şekilde kullanamazsınız. Herhangi bir biyometrik veri de dahil olmak üzere herhangi bir videoyu işleme ve depolama için Video Dizinleyici hizmetine yüklemeden önce, videodaki bireyden (ler) tüm uygun izinler de dahil olmak üzere tüm uygun haklara sahip olmalısınız. Video Dizinleyici'de uyumluluk, gizlilik ve güvenlik hakkında bilgi edinmek için, Microsoft [Bilişsel Hizmetler Koşulları.](https://azure.microsoft.com/support/legal/cognitive-services-compliance-and-privacy/) Microsoft'un gizlilik yükümlülükleri ve verilerinizin işlenmesi için lütfen Microsoft'un [Gizlilik Bildirimini,](https://privacy.microsoft.com/PrivacyStatement) [Çevrimiçi Hizmet Koşullarını](https://www.microsoft.com/licensing/product-licensing/products) ("OST") ve Veri [İşleme Eki'ni](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) ("DPA") gözden geçirin. Veri saklama, silme/imha gibi ek gizlilik bilgileri OST'de ve [burada](../video-indexer/faq.md)mevcuttur. Video Indexer'ı kullanarak Bilişsel Hizmetler Koşulları, OST, DPA ve Gizlilik Bildirimi'ne bağlı olmayı kabul edersiniz.

## <a name="built-in-presets"></a>Dahili hazır ayarlar

Medya Hizmetleri şu anda aşağıdaki yerleşik çözümleyici ön ayarlarını destekler:  

|**Önceden ayarlanmış ad**|**Senaryo**|**Şey**|
|---|---|---|
|[AudioAnalyzerPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#audioanalyzerpreset)|Sesi analiz etme|Önceden ayarlanmış, konuşma transkripsiyonu da dahil olmak üzere önceden tanımlanmış bir AI tabanlı analiz işlemleri kümesi uygular. Şu anda, önceden ayarlanmış tek bir dilde konuşma içeren tek bir ses parçası ile içerik işleme destekler. 'Dil etiketi bölgesi'nin BCP-47 biçimini kullanarak girişteki ses yükü için dili belirtebilirsiniz. Desteklenen diller İngilizce ('en-US' ve 'en-GB'), İspanyolca ('es-ES' ve 'es-MX'), Fransızca ('fr-FR'), İtalyanca ('it-IT'), Japonca ('ja-JP'), Portekizce ('pt-BR'), Çince ('zh-CN'), Almanca ('de-DE'), Arapça ('ar-EG' ve 'ar-SY'ru'ru), Rusça (--RU'), Rusça Hintçe ('hi-IN'), ve Korece ('ko-KR').<br/><br/> Dil belirtilmemişse veya null olarak ayarlanmamışsa, otomatik dil algılama, algılanan ilk dili seçer ve dosya süresince seçili dille devam eder. Otomatik dil algılama özelliği şu anda İngilizce, Çince, Fransızca, Almanca, İtalyanca, Japonca, İspanyolca, Rusça ve Portekizce'yi destekler. İlk dil algılandıktan sonra diller arasında dinamik olarak geçişi desteklemez. Otomatik dil algılama özelliği, açıkça ayırt edilebilir konuşma ile ses kayıtları ile en iyi çalışır. Otomatik dil algılama dili bulmak için başarısız olursa, transkripsiyon İngilizce geri düşüyor.|
|[VideoAnalyzerPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#videoanalyzerpreset)|Ses ve video analizi|Hem ses hem de videodan öngörüler (zengin meta veriler) ayıklar ve JSON biçimdosyası çıktıları. Bir video dosyasını işlerken yalnızca ses öngörüleri ayıklamak isteyip istemediğinizbelirtebilirsiniz. Daha fazla bilgi için [videoyu analiz edin.](analyze-videos-tutorial-with-api.md)|
|[FaceDetectorPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#facedetectorpreset)|Videoda bulunan yüzleri algılama|Mevcut tüm yüzleri algılamak için bir videoyu analiz ederken kullanılacak ayarları açıklar.|

### <a name="audioanalyzerpreset"></a>AudioAnalyzerPreset

Önceden ayar, bir ses veya video dosyasından birden çok ses öngörüsü ayıklamanızı sağlar. Çıktı, ses dökümü için bir JSON dosyası (tüm öngörülerle birlikte) ve VTT dosyasını içerir. Bu ön ayar, giriş dosyasının dilini [BCP47](https://tools.ietf.org/html/bcp47) dizesi biçiminde belirten bir özelliği kabul eder. Ses istatistikleri şunları içerir:

* **Ses transkripsiyonu**: Zaman damgalı konuşulan kelimelerin transkripti. Birden çok dil desteklenir.
* **Hoparlör indeksleme**: Konuşmacıların ve karşılık gelen konuşulan sözcüklerin haritalaması.
* **Konuşma duyarlılığı analizi**: Ses transkripsiyonuna yapılan duygu analizinin çıktısı.
* **Anahtar Kelimeler**: Ses transkripsiyonundan çıkarılan anahtar kelimeler.

### <a name="videoanalyzerpreset"></a>VideoAnalyzerPreset

Önceden ayar, bir video dosyasından birden çok ses ve video öngörüsü ayıklamanızı sağlar. Çıktı, bir JSON dosyası (tüm öngörülerle birlikte), video transkripti için bir VTT dosyası ve küçük resimlerin bir koleksiyonunu içerir. Bu önceden ayarlanmış, bir [BCP47](https://tools.ietf.org/html/bcp47) dizesini (videonun dilini temsil eden) bir özellik olarak da kabul eder. Video istatistikleri, yukarıda belirtilen tüm ses istatistiklerini ve aşağıdaki ek öğeleri içerir:

* **Yüz izleme**: Videoda yüzlerin bulunduğu süre. Her yüzün bir yüz kimliği ve ilgili küçük resimler koleksiyonu vardır.
* **Görsel metin**: Optik karakter tanıma yoluyla algılanan metin. Metin zaman damgalı ve aynı zamanda anahtar kelimeleri ayıklamak için kullanılır (ses transkript ek olarak).
* **Anahtar Kareler**: Videodan çıkarılan anahtar kareler koleksiyonu.
* **Görsel içerik moderasyonu**: Videoların doğada yetişkin veya müstehcen olarak işaretlenen kısmı.
* **Ek Açıklama**: Önceden tanımlanmış bir nesne modeline dayalı olarak videoların ek açıklama sonucu

## <a name="insightsjson-elements"></a>insights.json elemanları

Çıktı, video veya seste bulunan tüm bilgileri içeren bir JSON dosyası (insights.json) içerir. JSON aşağıdaki öğeleri içerebilir:

### <a name="transcript"></a>Transkript

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

### <a name="ocr"></a>Ocr

|Adı|Açıklama|
|---|---|
|id|OCR satır kimliği.|
|metin|OCR metni.|
|güvenilirlik|Tanıma güveni.|
|language|OCR dili.|
|Örnek|Bu OCR'nin göründüğü zaman aralıklarının listesi (aynı OCR birden çok kez görünebilir).|

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

### <a name="faces"></a>Yüz

|Adı|Açıklama|
|---|---|
|id|Yüz kimliği.|
|ad|Yüz adı. 'Bilinmeyen #0', tanımlanmış bir ünlü veya müşteri eğitimli bir kişi olabilir.|
|güvenilirlik|Yüz tanımlama güveni.|
|açıklama|Ünlünün tarifi. |
|küçük resimId|O yüzün küçük resminin kimliği.|
|bilinen PersonId|Dahili kimlik (bilinen bir kişiise).|
|referenceId|Bing Kimliği (Bing ünlüsüyse).|
|referenceType|Şu anda sadece Bing.|
|başlık|Başlık (ünlü yse,örneğin, "Microsoft'un CEO'su").|
|ımageurl|Bir ünlüyse, resim URL'si.|
|Örnek|Verilen zaman aralığında yüzün göründüğü durumlar. Her örnekte bir küçük resim de vardır. |

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

### <a name="shots"></a>Çekim

|Adı|Açıklama|
|---|---|
|id|Çekim kimliği.|
|keyFrames|Çekimdeki anahtar karelerin listesi (her birinde bir kimlik ve örnek zaman aralıklarının listesi bulunur). Anahtar kare örnekleri, keyFrame'in küçük resmi kimliğiyle birlikte küçük resimli bir alana sahiptir.|
|Örnek|Bu çekim (çekim sadece 1 örnek var) zaman aralıkları listesi.|

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

### <a name="statistics"></a>istatistikler

|Adı|Açıklama|
|---|---|
|Yazışma Sayısı|Videodaki yazışma sayısı.|
|Sözcük Sayısı|Konuşmacı başına kelime sayısı.|
|HoparlörNumberOfFragments|Hoparlörün bir videoda sahip olduğu parça miktarı.|
|HoparlörLongestMonolog|Hoparlörün en uzun monologu. Hoparlörün monolog içinde sessizlik varsa, bu dahil. Monologbaşında ve sonunda sessizlik kaldırılır.|
|HoparlörTalktoListenRatio|Hesaplama, konuşmacının monologunda (arada sessizlik olmadan) harcanan süreye göre hesaplanır ve videonun toplam süresine bölünür. Zaman üçüncü ondalık sayıya yuvarlanır.|


### <a name="sentiments"></a>Duyguları

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

### <a name="labels"></a>Etiket

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

### <a name="keywords"></a>anahtar sözcükler

|Adı|Açıklama|
|---|---|
|id|Anahtar kelime kimliği.|
|metin|Anahtar kelime metni.|
|güvenilirlik|Anahtar kelimenin tanıma güveni.|
|language|Anahtar kelime dili (çevrildiğinde).|
|Örnek|Bu anahtar kelimenin göründüğü zaman aralıklarının listesi (bir anahtar kelime birden çok kez görünebilir).|

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

#### <a name="visualcontentmoderation"></a>visualContentModeration

visualContentModeration bloğu, Video Indexer'ın yetişkin içeriğe sahip olabileceğini bulduğu zaman aralıklarını içerir. visualContentModeration boşsa, tanımlanmış hiçbir yetişkin içeriği yoktur.

Yetişkinlere uygun veya müstehcen içerik içerdiği tespit edilebilen videolar yalnızca özel görünüm için kullanılabilir. Kullanıcılar içeriğin insan tarafından incelenmesi için bir istek `IsAdult` gönderebilir, bu durumda öznitelik insan incelemesinin sonucunu içerir.

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
## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Azure Media Services ile videoları analiz etme](analyze-videos-tutorial-with-api.md)
