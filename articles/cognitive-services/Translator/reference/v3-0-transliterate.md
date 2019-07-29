---
title: Translator Metin Çevirisi API'si alfabede yöntemi
titleSuffix: Azure Cognitive Services
description: Translator Metin Çevirisi API'si Alfabbir yöntemini kullanın.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/01/2019
ms.author: swmachan
ms.openlocfilehash: 0be56bbd421bcd12e3c494a671db2f322e07c575
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68594933"
---
# <a name="translator-text-api-30-transliterate"></a>Translator Metin Çevirisi API'si 3,0: Transliterate

Bir dildeki metni bir betikten başka bir betiğe dönüştürür.

## <a name="request-url"></a>İstek URL'si

Şu kişiye `POST` bir istek gönder:

```HTTP
https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesine geçirilen istek parametreleri şunlardır:

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>API sürümü</td>
    <td>*Gerekli parametre*.<br/>İstemci tarafından istenen API 'nin sürümü. Değer olmalıdır `3.0`.</td>
  </tr>
  <tr>
    <td>dil</td>
    <td>*Gerekli parametre*.<br/>Bir betikten diğerine dönüştürülecek metnin dilini belirtir. Olası diller, hizmet tarafından `transliteration` [desteklenen diller](./v3-0-languages.md)için sorgulanarak elde edilen kapsamda listelenmiştir.</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*Gerekli parametre*.<br/>Giriş metni tarafından kullanılan betiği belirtir. Seçili dil için kullanılabilen giriş betiklerini `transliteration` bulmak için kapsamı kullanarak [desteklenen dilleri](./v3-0-languages.md) arayın.</td>
  </tr>
  <tr>
    <td>Toscrıpt</td>
    <td>*Gerekli parametre*.<br/>Çıkış betiğini belirtir. Seçilen giriş dili ve giriş betiği `transliteration` birleşimi için kullanılabilir çıkış betikleri bulmak için, kapsamı kullanarak [desteklenen dilleri](./v3-0-languages.md) arayın.</td>
  </tr>
</table> 

İstek üstbilgileri şunları içerir:

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>Kimlik doğrulama üst bilgisi</td>
    <td><em>Gerekli istek üst bilgisi</em>.<br/><a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">Kimlik doğrulaması için kullanılabilen seçeneklere</a>bakın.</td>
  </tr>
  <tr>
    <td>Content-Type</td>
    <td>*Gerekli istek üst bilgisi*.<br/>Yükün içerik türünü belirtir. Olası değerler şunlardır: `application/json`.</td>
  </tr>
  <tr>
    <td>İçerik uzunluğu</td>
    <td>*Gerekli istek üst bilgisi*.<br/>İstek gövdesinin uzunluğu.</td>
  </tr>
  <tr>
    <td>X-Clienttraceıd</td>
    <td>*İsteğe bağlı*.<br/>İsteği benzersiz şekilde tanımlamak için istemci tarafından oluşturulan bir GUID. Adlı `ClientTraceId`sorgu PARAMETRESINI kullanarak izleme kimliğini sorgu dizesine eklerseniz bu üstbilgiyi atlayabilirsiniz.</td>
  </tr>
</table> 

## <a name="request-body"></a>İstek gövdesi

İsteğin gövdesi bir JSON dizisidir. Her dizi öğesi, dönüştürülecek dizeyi temsil eden adlı `Text`dize özelliği olan bir JSON nesnesidir.

```json
[
    {"Text":"こんにちは"},
    {"Text":"さようなら"}
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 10 öğe içerebilir.
* Bir dizi öğesinin metin değeri boşluk dahil 1.000 karakteri aşamaz.
* İstekte bulunan metnin tamamı boşluk dahil 5.000 karakteri aşamaz.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt, Giriş dizisindeki her öğe için bir sonuç içeren bir JSON dizisidir. Bir sonuç nesnesi aşağıdaki özellikleri içerir:

  * `text`: Giriş dizesini çıkış betiğine dönüştürmenin sonucu olan bir dize.
  
  * `script`: Çıktıda kullanılan betiği belirten bir dize.

Örnek bir JSON yanıtı:

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

Bir isteğin döndürdüğü olası HTTP durum kodları aşağıda verilmiştir. 

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
    <td>İsteğin kimliği doğrulanamadı. Kimlik bilgilerinin belirtildiğinden ve geçerli olduğundan emin olun.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>İstek yetkili değil. Ayrıntılar hata iletisini denetleyin. Bu genellikle bir deneme aboneliği ile birlikte sunulan tüm ücretsiz çevirilerin kullanıldığını gösterir.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>İstemci istek sınırlarını aştığından, sunucu isteği reddetti.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Beklenmeyen bir hata oluştu. Hata devam ederse, bununla raporla: hatanın tarih ve saati, yanıt başlığından `X-RequestId`istek tanımlayıcısı ve istek üst `X-ClientTraceId`bilgisinden istemci tanımlayıcısı.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Sunucu geçici olarak kullanılamıyor. İsteği yeniden deneyin. Hata devam ederse, bununla raporla: hatanın tarih ve saati, yanıt başlığından `X-RequestId`istek tanımlayıcısı ve istek üst `X-ClientTraceId`bilgisinden istemci tanımlayıcısı.</td>
  </tr>
</table> 

Bir hata oluşursa, istek bir JSON hata yanıtı da döndürür. Hata kodu 3 basamaklı HTTP durum kodunu birleştiren 6 basamaklı bir sayıdır ve ardından hatayı daha fazla kategorilere ayırarak 3 basamaklı bir sayıdır. Ortak hata kodları, [v3 Translator metin çevirisi API'si başvuru sayfasında](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors)bulunabilir. 

## <a name="examples"></a>Örnekler

Aşağıdaki örnekte, iki Japonca dizenin roman Japonca 'ya nasıl dönüştürüleceği gösterilmektedir.

# <a name="curltabcurl"></a>[kıvr](#tab/curl)

Bu örnekteki isteğin JSON yükü:

```
[{"text":"こんにちは","script":"jpan"},{"text":"さようなら","script":"jpan"}]
```

Unicode karakterlerini desteklemeyen bir komut satırı penceresinde kıvrımlı kullanıyorsanız, aşağıdaki JSON yükünü alıp adlı `request.txt`bir dosyaya kaydedin. Dosyayı `UTF-8` kodlamalı olarak kaydettiğinizden emin olun.

```
curl -X POST "https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0&language=ja&fromScript=Jpan&toScript=Latn" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d @request.txt
```

---
