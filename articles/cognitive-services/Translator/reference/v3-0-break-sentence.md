---
title: Çevirmen Metin API Kesme Cümlesi Yöntemi
titleSuffix: Azure Cognitive Services
description: Çevirmen Metin API BreakSentence yöntemi, cümle sınırlarının bir metin parçasındaki konumlandırmasını tanımlar.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 01/21/2020
ms.author: swmachan
ms.openlocfilehash: 4c314148b8e1495a8b5a12c42d4989d13cdd6a08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76548127"
---
# <a name="translator-text-api-30-breaksentence"></a>Çevirmen Metin API 3.0: Kesme Cümlesi

Cümle sınırlarının bir metin parçasındaki konumlandırmasını tanımlar.

## <a name="request-url"></a>İstek URL'si

Bir `POST` istek gönderin:

```HTTP
https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen istek parametreleri şunlardır:

| Sorgu Parametresi | Açıklama |
| -------| ----------- |
| api-sürümü <img width=200/>   | **Gerekli sorgu parametresi**.<br/>İstemci tarafından istenen API sürümü. Değer. `3.0` |
| language | **İsteğe bağlı sorgu parametresi**.<br/>Giriş metninin dilini tanımlayan dil etiketi. Kod belirtilmemişse, otomatik dil algılama uygulanır. |
| betiğini çalıştırın    | **İsteğe bağlı sorgu parametresi**.<br/>Giriş metni tarafından kullanılan komut dosyasını tanımlayan komut dosyası etiketi. Bir komut dosyası belirtilmemişse, dilin varsayılan komut dosyası varsayalım.  | 

İstek üstbilgisi şunları içerir:

| Üst bilgiler | Açıklama |
| ------- | ----------- |
| Kimlik doğrulama üstbilgisi(lar) <img width=200/>  | **Gerekli istek üstbilgi**.<br/><a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">Kimlik doğrulama için kullanılabilir seçeneklere</a>bakın. |
| İçerik Türü | **Gerekli istek üstbilgi**.<br/>Yükün içerik türünü belirtir. Olası değerler `application/json`şunlardır: . |
| İçerik Uzunluğu    | **Gerekli istek üstbilgi**.<br/>İstek gövdesinin uzunluğu.  | 
| X-ClientTraceId   | **İsteğe bağlı**.<br/>İsteği benzersiz olarak tanımlamak için istemci tarafından oluşturulan GUID. 'li bir sorgu parametresi kullanarak sorgu dizesinde izleme kimliği eklerseniz, `ClientTraceId`bu üstbilginin atlayabildiğinizi unutmayın.  | 

## <a name="request-body"></a>İstek gövdesi

İsteğin gövdesi bir JSON dizisidir. Her dizi öğesi, dize özelliğine `Text`sahip bir JSON nesnesidir. Cümle sınırları `Text` özelliğin değeri için hesaplanır. Tek bir metin parçası olan örnek bir istek gövdesi aşağıdaki gibi görünür:

```json
[
    { "Text": "How are you? I am fine. What did you do today?" }
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 100 öğeye sahip olabilir.
* Bir dizi öğesinin metin değeri boşluklar da dahil olmak üzere 10.000 karakteri geçemez.
* İsteğe dahil edilen metnin tamamı boşluklar da dahil olmak üzere 50.000 karakteri geçemez.
* Sorgu `language` parametresi belirtilirse, tüm dizi öğeleri aynı dilde olmalıdır. Aksi takdirde, dil otomatik algılama bağımsız olarak her dizi öğesine uygulanır.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt, giriş dizisindeki her dize için bir sonucu olan bir JSON dizisidir. Sonuç nesnesi aşağıdaki özellikleri içerir:

  * `sentLen`: Metin öğesindeki cümlelerin uzunluklarını temsil eden bir dizi toplamcı. Dizinin uzunluğu cümle sayısıdır ve değerler her cümlenin uzunluğudur. 

  * `detectedLanguage`: Algılanan dili aşağıdaki özelliklerle açıklayan bir nesne:

     * `language`: Algılanan dilin kodu.

     * `score`: Sonuca olan güveni gösteren bir float değeri. Skor sıfır ve bir arasında dır ve düşük bir puan düşük bir güven gösterir.
     
    Özelliğin `detectedLanguage` yalnızca dil otomatik algılaması istendiğinde sonuç nesnesinde bulunduğunu unutmayın.

Bir örnek JSON yanıtı:

```json
[
  {
    "sentLen": [ 13, 11, 22 ]
    "detectedLanguage": {
      "language": "en",
      "score": 401
    },
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

Aşağıdaki örnek, tek bir cümle için cümle sınırlarının nasıl elde edilebildiğini gösterir. Tümcenin dili servis tarafından otomatik olarak algılanır.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'How are you? I am fine. What did you do today?'}]"
```

