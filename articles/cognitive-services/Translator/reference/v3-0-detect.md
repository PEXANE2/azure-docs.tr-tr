---
title: Çevirmen Metin API Algılama Yöntemi
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler Çevirmeni Metin API Algılama yöntemiyle bir metnin dilini tanımlayın.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/01/2019
ms.author: swmachan
ms.openlocfilehash: 370f3b14c12fc05f181d6497b7069bbf1cf3c9cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73837291"
---
# <a name="translator-text-api-30-detect"></a>Çevirmen Metin API 3.0: Algılama

Bir metnin dilini tanımlar.

## <a name="request-url"></a>İstek URL'si

Bir `POST` istek gönderin:

```HTTP
https://api.cognitive.microsofttranslator.com/detect?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen istek parametreleri şunlardır:

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>api-sürümü</td>
    <td>*Gerekli parametre*.<br/>İstemci tarafından istenen API sürümü. Değer. `3.0`</td>
  </tr>
</table> 

İstek üstbilgisi şunları içerir:

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>Kimlik doğrulama üstbilgisi(lar)</td>
    <td><em>Gerekli istek üstbilgi</em>.<br/><a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">Kimlik doğrulama için kullanılabilir seçeneklere</a>bakın.</td>
  </tr>
  <tr>
    <td>İçerik Türü</td>
    <td>*Gerekli istek üstbilgi*.<br/>Yükün içerik türünü belirtir. Olası değerler `application/json`şunlardır: .</td>
  </tr>
  <tr>
    <td>İçerik Uzunluğu</td>
    <td>*Gerekli istek üstbilgi*.<br/>İstek gövdesinin uzunluğu.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*İsteğe bağlı*.<br/>İsteği benzersiz olarak tanımlamak için istemci tarafından oluşturulan GUID. 'li bir sorgu parametresi kullanarak sorgu dizesinde izleme kimliği eklerseniz, `ClientTraceId`bu üstbilginin atlayabildiğinizi unutmayın.</td>
  </tr>
</table> 

## <a name="request-body"></a>İstek gövdesi

İsteğin gövdesi bir JSON dizisidir. Her dizi öğesi, dize özelliğine `Text`sahip bir JSON nesnesidir. Dil algılama `Text` özelliğinin değerine uygulanır. Örnek istek gövdesi şuna benzer:

```json
[
    { "Text": "Ich würde wirklich gern Ihr Auto um den Block fahren ein paar Mal." }
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 100 öğeye sahip olabilir.
* Bir dizi öğesinin metin değeri boşluklar da dahil olmak üzere 10.000 karakteri geçemez.
* İsteğe dahil edilen metnin tamamı boşluklar da dahil olmak üzere 50.000 karakteri geçemez.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt, giriş dizisindeki her dize için bir sonucu olan bir JSON dizisidir. Sonuç nesnesi aşağıdaki özellikleri içerir:

  * `language`: Algılanan dilin kodu.

  * `score`: Sonuca olan güveni gösteren bir float değeri. Skor sıfır ve bir arasında dır ve düşük bir puan düşük bir güven gösterir.

  * `isTranslationSupported`: Algılanan dil metin çevirisi için desteklenen dillerden biri yse doğru olan boolean değeri.

  * `isTransliterationSupported`: Algılanan dil çeviri için desteklenen dillerden biri yse doğru olan boolean değeridir.
  
  * `alternatives`: Diğer olası dillerin bir dizi. Dizinin her öğesi yukarıda listelenen aynı özelliklere sahip `language` `score`başka `isTranslationSupported` `isTransliterationSupported`bir nesnedir: , , ve .

Bir örnek JSON yanıtı:

```json
[
  {
    "language": "de",
    "score": 0.92,
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "alternatives": [
      {
        "language": "pt",
        "score": 0.23,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      },
      {
        "language": "sk",
        "score": 0.23,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      }
    ]
  }
]
```

## <a name="response-headers"></a>Yanıt üst bilgileri

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>X-RequestId</td>
    <td>İsteği tanımlamak için hizmet tarafından oluşturulan değer. Sorun giderme amacıyla kullanılır.</td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Yanıt durum kodları

Aşağıda, bir isteğin döndürdettiği olası HTTP durum kodları vereme olasılığı ve 

<table width="100%">
  <th width="20%">Durum Kodu</th>
  <th>Açıklama</th>
  <tr>
    <td>200</td>
    <td>Başarılı.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Sorgu parametrelerinden biri eksik veya geçersiz. Yeniden denemeden önce istek parametrelerini düzeltin.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>İstek doğrulanamadı. Kimlik bilgilerinin belirtilmiş ve geçerli olup olmadığını denetleyin.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>İstek yetkili değil. Ayrıntılar hata iletisini denetleyin. Bu genellikle deneme aboneliği ile sağlanan tüm ücretsiz çevirilerin kullanıldığını gösterir.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>İstemci istek sınırlarını aştığı için sunucu isteği reddetti.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Beklenmeyen bir hata oluştu. Hata devam ederse, bunu şu şekilde bildirin: hatanın tarih ve saati, `X-RequestId`yanıt üstbilgisinden identifier `X-ClientTraceId`ve istek üstbilgisinden istemci tanımlayıcısı.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Sunucu geçici olarak kullanılamıyor. İsteği yeniden deneyin. Hata devam ederse, bunu şu şekilde bildirin: hatanın tarih ve saati, `X-RequestId`yanıt üstbilgisinden identifier `X-ClientTraceId`ve istek üstbilgisinden istemci tanımlayıcısı.</td>
  </tr>
</table> 

Bir hata oluşursa, istek de bir JSON hata yanıtı döndürecek. Hata kodu, hatayı daha fazla kategorilere ayırmak için 3 basamaklı HTTP durum kodunu ve ardından 3 basamaklı bir sayıyı birleştiren 6 basamaklı bir sayıdır. Yaygın hata kodları [v3 Translator Text API başvuru sayfasında](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors)bulunabilir. 

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, metin çevirisi için desteklenen dillerin nasıl alınabildiğini gösterir.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/detect?api-version=3.0" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'What language is this text written in?'}]"
```
