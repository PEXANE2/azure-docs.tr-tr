---
title: Çevirmen Metin API Çeviri Yöntemi
titleSuffix: Azure Cognitive Services
description: Çevirmen Metin API Çeviri yöntemiyle metni bir komut dosyasından bir komut dosyasından başka bir komut dosyasına dönüştürün.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/01/2019
ms.author: swmachan
ms.openlocfilehash: e6bb1541b2b668796b352bebc68d59b4ade143e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73837273"
---
# <a name="translator-text-api-30-transliterate"></a>Çevirmen Metin API 3.0: Çeviri

Bir dildeki metni bir komut dosyasından başka bir komut dosyasına dönüştürür.

## <a name="request-url"></a>İstek URL'si

Bir `POST` istek gönderin:

```HTTP
https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0
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
  <tr>
    <td>language</td>
    <td>*Gerekli parametre*.<br/>Bir komut dosyasından diğerine dönüştürmek için metnin dilini belirtir. Olası diller, desteklenen `transliteration` [diller](./v3-0-languages.md)için hizmet sorgulayarak elde edilen kapsamda listelenir.</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*Gerekli parametre*.<br/>Giriş metni tarafından kullanılan komut dosyasını belirtir. Seçili dil için `transliteration` kullanılabilir giriş komut dosyalarını bulmak için kapsamı kullanarak [desteklenen dillere](./v3-0-languages.md) bakın.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td>*Gerekli parametre*.<br/>Çıktı komut dosyasını belirtir. Seçilen giriş dili ve `transliteration` giriş komut dosyası birleşimi için kullanılabilir çıktı komut dosyalarını bulmak için kapsamı kullanarak [desteklenen dillere](./v3-0-languages.md) bakın.</td>
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

İsteğin gövdesi bir JSON dizisidir. Her dizi öğesi, dönüştürmek için dize `Text`temsil eden bir dize özelliği ile bir JSON nesnesidir.

```json
[
    {"Text":"こんにちは"},
    {"Text":"さようなら"}
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 10 öğeye sahip olabilir.
* Bir dizi öğesinin metin değeri boşluklar da dahil olmak üzere 1.000 karakteri geçemez.
* İsteğe dahil edilen metnin tamamı boşluklar da dahil olmak üzere 5.000 karakteri geçemez.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt, giriş dizisindeki her öğe için bir sonucu olan bir JSON dizisidir. Sonuç nesnesi aşağıdaki özellikleri içerir:

  * `text`: Giriş dizesinin çıkış komut dosyasına dönüştürülmesi sonucu oluşan dize.
  
  * `script`: Çıktıda kullanılan komut dosyasını belirten bir dize.

Bir örnek JSON yanıtı:

```json
[
    {"text":"konnnichiha","script":"Latn"},
    {"text":"sayounara","script":"Latn"}
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

Aşağıdaki örnek, iki Japonca dizesinin Romanlaştırılmış Japonca'ya nasıl dönüştürüldüğünü gösterir.

Bu örnekte istek için JSON yükü:

```json
[{"text":"こんにちは","script":"jpan"},{"text":"さようなら","script":"jpan"}]
```

Unicode karakterlerini desteklemeyen bir komut satırı penceresinde cURL kullanıyorsanız, aşağıdaki JSON yükünü alın ve `request.txt`adlı bir dosyaya kaydedin. Dosyayı kodlama ile `UTF-8` kaydettiğinden emin olun.

```
curl -X POST "https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0&language=ja&fromScript=Jpan&toScript=Latn" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d @request.txt
```
