---
title: Çevirmen Alfabardın yöntemi
titleSuffix: Azure Cognitive Services
description: Bir dildeki metni, çevirmen alfabede bulunan yöntemi ile bir betikten başka bir betiğe dönüştürür.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/01/2019
ms.author: swmachan
ms.openlocfilehash: 8b811c57eb163931c39a311418ac9f1513e9393a
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592262"
---
# <a name="translator-30-transliterate"></a>Çevirici 3,0: alfabede

Bir dildeki metni bir betikten başka bir betiğe dönüştürür.

## <a name="request-url"></a>İstek URL’si

Şu kişiye bir `POST` istek gönder:

```HTTP
https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesine geçirilen istek parametreleri şunlardır:

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>api-sürümü</td>
    <td>*Gerekli parametre*.<br/>İstemci tarafından istenen API 'nin sürümü. Değer olmalıdır `3.0` .</td>
  </tr>
  <tr>
    <td>language</td>
    <td>*Gerekli parametre*.<br/>Bir betikten diğerine dönüştürülecek metnin dilini belirtir. Olası diller, `transliteration` hizmet tarafından [desteklenen diller](./v3-0-languages.md)için sorgulanarak elde edilen kapsamda listelenmiştir.</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*Gerekli parametre*.<br/>Giriş metni tarafından kullanılan betiği belirtir. [supported languages](./v3-0-languages.md) `transliteration` Seçili dil için kullanılabilen giriş betiklerini bulmak için kapsamı kullanarak desteklenen dilleri arayın.</td>
  </tr>
  <tr>
    <td>Toscrıpt</td>
    <td>*Gerekli parametre*.<br/>Çıkış betiğini belirtir. [supported languages](./v3-0-languages.md) `transliteration` Seçilen giriş dili ve giriş betiği birleşimi için kullanılabilir çıkış betikleri bulmak için, kapsamı kullanarak desteklenen dilleri arayın.</td>
  </tr>
</table> 

İstek üstbilgileri şunları içerir:

<table width="100%">
  <th width="20%">Üst Bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>Kimlik doğrulama üst bilgisi</td>
    <td><em>Gerekli istek üst bilgisi</em>.<br/><a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">Kimlik doğrulaması için kullanılabilen seçeneklere</a>bakın.</td>
  </tr>
  <tr>
    <td>İçerik Türü</td>
    <td>*Gerekli istek üst bilgisi*.<br/>Yükün içerik türünü belirtir. Olası değerler şunlardır: `application/json` .</td>
  </tr>
  <tr>
    <td>İçerik Uzunluğu</td>
    <td>*Gerekli istek üst bilgisi*.<br/>İstek gövdesinin uzunluğu.</td>
  </tr>
  <tr>
    <td>X-Clienttraceıd</td>
    <td>*Isteğe bağlı*.<br/>İsteği benzersiz şekilde tanımlamak için istemci tarafından oluşturulan bir GUID. Adlı sorgu parametresini kullanarak izleme KIMLIĞINI sorgu dizesine eklerseniz bu üstbilgiyi atlayabilirsiniz `ClientTraceId` .</td>
  </tr>
</table> 

## <a name="request-body"></a>İstek gövdesi

İsteğin gövdesi bir JSON dizisidir. Her dizi öğesi `Text` , dönüştürülecek dizeyi temsil eden adlı dize özelliği olan BIR JSON nesnesidir.

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
  
  * `script`: Çıkışta kullanılan betiği belirten bir dize.

Örnek bir JSON yanıtı:

```json
[
    {"text":"konnnichiha","script":"Latn"},
    {"text":"sayounara","script":"Latn"}
]
```

## <a name="response-headers"></a>Yanıt üst bilgileri

<table width="100%">
  <th width="20%">Üst Bilgiler</th>
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
    <td>Beklenmeyen bir hata oluştu. Hata devam ederse, bununla raporla: hatanın tarih ve saati, yanıt başlığından istek tanımlayıcısı `X-RequestId` ve istek üst bilgisinden istemci tanımlayıcısı `X-ClientTraceId` .</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Sunucu geçici olarak kullanılamıyor. İsteği yeniden deneyin. Hata devam ederse, bununla raporla: hatanın tarih ve saati, yanıt başlığından istek tanımlayıcısı `X-RequestId` ve istek üst bilgisinden istemci tanımlayıcısı `X-ClientTraceId` .</td>
  </tr>
</table> 

Bir hata oluşursa, istek bir JSON hata yanıtı da döndürür. Hata kodu 3 basamaklı HTTP durum kodunu birleştiren 6 basamaklı bir sayıdır ve ardından hatayı daha fazla kategorilere ayırarak 3 basamaklı bir sayıdır. Ortak hata kodları, [v3 Translator başvurusu sayfasında](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors)bulunabilir. 

## <a name="examples"></a>Örnekler

Aşağıdaki örnekte, iki Japonca dizenin roman Japonca 'ya nasıl dönüştürüleceği gösterilmektedir.

Bu örnekteki isteğin JSON yükü:

```json
[{"text":"こんにちは","script":"jpan"},{"text":"さようなら","script":"jpan"}]
```

Unicode karakterlerini desteklemeyen bir komut satırı penceresinde kıvrımlı kullanıyorsanız, aşağıdaki JSON yükünü alıp adlı bir dosyaya kaydedin `request.txt` . Dosyayı kodlamalı olarak kaydettiğinizden emin olun `UTF-8` .

```
curl -X POST "https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0&language=ja&fromScript=Jpan&toScript=Latn" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d @request.txt
```
