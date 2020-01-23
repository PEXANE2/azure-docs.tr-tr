---
title: Translator Metin Çevirisi API'si Breakcümle yöntemi
titleSuffix: Azure Cognitive Services
description: Translator Metin Çevirisi API'si Breakcümlesini metodu, tümce sınırlarının bir metin parçasına konumlandırılmasını belirler.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 01/21/2020
ms.author: swmachan
ms.openlocfilehash: 4c314148b8e1495a8b5a12c42d4989d13cdd6a08
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76548127"
---
# <a name="translator-text-api-30-breaksentence"></a>Translator Metin Çevirisi API'si 3,0: Breakcümlesi

Tümce sınırlarının metin parçasına konumlandırılmasını belirler.

## <a name="request-url"></a>İstek URL'si

`POST` isteği gönder:

```HTTP
https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesine geçirilen istek parametreleri şunlardır:

| Sorgu parametresi | Açıklama |
| -------| ----------- |
| api-version <img width=200/>   | **Gerekli sorgu parametresi**.<br/>İstemci tarafından istenen API 'nin sürümü. Değer `3.0`olmalıdır. |
| language | **Isteğe bağlı sorgu parametresi**.<br/>Giriş metninin dilini tanımlayan dil etiketi. Bir kod belirtilmemişse, otomatik dil algılama uygulanır. |
| betiğini çalıştırın    | **Isteğe bağlı sorgu parametresi**.<br/>Giriş metni tarafından kullanılan betiği tanımlayan betik etiketi. Bir komut dosyası belirtilmemişse, dilin varsayılan betiği kabul edilecektir.  | 

İstek üstbilgileri şunları içerir:

| Üst bilgiler | Açıklama |
| ------- | ----------- |
| Kimlik doğrulama üst bilgisi <img width=200/>  | **Gerekli istek üst bilgisi**.<br/><a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">Kimlik doğrulaması için kullanılabilen seçeneklere</a>bakın. |
| Content-Type | **Gerekli istek üst bilgisi**.<br/>Yükün içerik türünü belirtir. Olası değerler şunlardır: `application/json`. |
| İçerik Uzunluğu    | **Gerekli istek üst bilgisi**.<br/>İstek gövdesinin uzunluğu.  | 
| X-Clienttraceıd   | **İsteğe bağlı**.<br/>İsteği benzersiz şekilde tanımlamak için istemci tarafından oluşturulan bir GUID. `ClientTraceId`adlı bir sorgu parametresi kullanarak, sorgu dizesinde izleme KIMLIĞINI eklerseniz bu üstbilgiyi atlayabilirsiniz.  | 

## <a name="request-body"></a>İstek gövdesi

İsteğin gövdesi bir JSON dizisidir. Her dizi öğesi, `Text`adlı dize özelliğine sahip bir JSON nesnesidir. Tümce sınırları `Text` özelliğinin değeri için hesaplanır. Bir metin parçasına sahip örnek bir istek gövdesi şöyle görünür:

```json
[
    { "Text": "How are you? I am fine. What did you do today?" }
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 100 öğe içerebilir.
* Bir dizi öğesinin metin değeri boşluk dahil 10.000 karakteri aşamaz.
* İstekte bulunan metnin tamamı boşluk dahil 50.000 karakteri aşamaz.
* `language` sorgu parametresi belirtilirse, tüm dizi öğeleri aynı dilde olmalıdır. Aksi halde, dil otomatik algılama her bir dizi öğesine bağımsız olarak uygulanır.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt, Giriş dizisindeki her bir dize için bir sonuç içeren bir JSON dizisidir. Bir sonuç nesnesi aşağıdaki özellikleri içerir:

  * `sentLen`: metin öğesindeki Tümcelerin uzunluklarını temsil eden bir tamsayılar dizisi. Dizinin uzunluğu, Tümcelerin sayısıdır ve değerler her tümcenin uzunluktadır. 

  * `detectedLanguage`: aşağıdaki özellikler aracılığıyla algılanan dili açıklayan bir nesne:

     * `language`: algılanan dilin kodu.

     * `score`: sonucun güvenilirliği belirten bir float değeri. Puan sıfır ile bir ve düşük puan arasında düşük bir güvenilirlik olduğunu gösterir.
     
    `detectedLanguage` özelliğinin yalnızca dil otomatik algılaması istendiğinde sonuç nesnesinde bulunduğunu unutmayın.

Örnek bir JSON yanıtı:

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
    <td>Beklenmeyen bir hata oluştu. Hata devam ederse, bununla raporla: hatanın tarih ve saati, yanıt üst bilgisi `X-RequestId`istek tanımlayıcısı ve istek üst bilgisi `X-ClientTraceId`istemci tanımlayıcısı.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Sunucu geçici olarak kullanılamıyor. İsteği yeniden deneyin. Hata devam ederse, bununla raporla: hatanın tarih ve saati, yanıt üst bilgisi `X-RequestId`istek tanımlayıcısı ve istek üst bilgisi `X-ClientTraceId`istemci tanımlayıcısı.</td>
  </tr>
</table> 

Bir hata oluşursa, istek bir JSON hata yanıtı da döndürür. Hata kodu 3 basamaklı HTTP durum kodunu birleştiren 6 basamaklı bir sayıdır ve ardından hatayı daha fazla kategorilere ayırarak 3 basamaklı bir sayıdır. Ortak hata kodları, [v3 Translator metin çevirisi API'si başvuru sayfasında](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors)bulunabilir. 

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, tek bir cümle için tümce sınırlarının nasıl alınacağını gösterir. Tümce dili, hizmet tarafından otomatik olarak algılanır.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'How are you? I am fine. What did you do today?'}]"
```

