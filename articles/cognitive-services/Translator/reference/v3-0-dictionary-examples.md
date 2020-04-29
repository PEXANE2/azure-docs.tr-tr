---
title: Translator Metin Çevirisi API'si sözlük örnekleri yöntemi
titleSuffix: Azure Cognitive Services
description: Translator Metin Çevirisi API'si Dictionary örnekleri yöntemi, Sözlükteki terimlerin bağlamda nasıl kullanıldığını gösteren örnekler sağlar.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 01/21/2020
ms.author: swmachan
ms.openlocfilehash: b3551a8df19e47178c7bacd9218cfa60b66d81f9
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76548076"
---
# <a name="translator-text-api-30-dictionary-examples"></a>Translator Metin Çevirisi API'si 3,0: Sözlük örnekleri

Sözlükteki terimlerin bağlamda nasıl kullanıldığını gösteren örnekler sağlar. Bu işlem [Sözlük aramayla](./v3-0-dictionary-lookup.md)birlikte kullanılır.

## <a name="request-url"></a>İstek URL'si

Şu kişiye `POST` bir istek gönder:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesine geçirilen istek parametreleri şunlardır:

| Sorgu parametresi | Açıklama |
| --------- | ----------- |
| api-sürümü <img width=200/> | **Gerekli parametre**.<br/>İstemci tarafından istenen API 'nin sürümü. Değer olmalıdır `3.0`. |
| Kaynak | **Gerekli parametre**.<br/>Giriş metninin dilini belirtir. Kaynak dili, `dictionary` kapsamda bulunan [desteklenen dillerden](./v3-0-languages.md) biri olmalıdır. |
| - | **Gerekli parametre**.<br/>Çıkış metninin dilini belirtir. Hedef dil, `dictionary` kapsamda bulunan [desteklenen dillerden](./v3-0-languages.md) biri olmalıdır.  | 

İstek üstbilgileri şunları içerir:

| Üst bilgiler  | Açıklama |
| ------ | ----------- |
| Kimlik doğrulama üst bilgisi <img width=200/>  | **Gerekli istek üst bilgisi**.<br/><a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">Kimlik doğrulaması için kullanılabilen seçeneklere</a>bakın. |
| İçerik Türü | **Gerekli istek üst bilgisi**.<br/>Yükün içerik türünü belirtir. Olası değerler şunlardır: `application/json`. |
| İçerik Uzunluğu   | **Gerekli istek üst bilgisi**.<br/>İstek gövdesinin uzunluğu. |
| X-Clienttraceıd   | **Isteğe bağlı**.<br/>İsteği benzersiz şekilde tanımlamak için istemci tarafından oluşturulan bir GUID. Adlı `ClientTraceId`sorgu parametresini kullanarak izleme kimliğini sorgu dizesine eklerseniz, bu üstbilgiyi atlayabilirsiniz. |

## <a name="request-body"></a>İstek gövdesi

İsteğin gövdesi bir JSON dizisidir. Her dizi öğesi, aşağıdaki özelliklere sahip bir JSON nesnesidir:

  * `Text`: Arama terimini belirten bir dize. Bu, önceki bir [Sözlük Arama](./v3-0-dictionary-lookup.md) isteğinin `normalizedText` arka çevirilerine ait bir alanın değeri olmalıdır. Ayrıca, `normalizedSource` alanın değeri de olabilir.

  * `Translation`: Daha önce [Sözlük Arama](./v3-0-dictionary-lookup.md) işlemi tarafından döndürülen çevrilmiş metni belirten bir dize. Bu, `normalizedTarget` [Sözlük Arama](./v3-0-dictionary-lookup.md) yanıtı `translations` listesindeki alanın değeri olmalıdır. Hizmet, belirli kaynak hedefi sözcük çiftinin örneklerini döndürür.

Örnek:

```json
[
    {"Text":"fly", "Translation":"volar"}
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 10 öğe içerebilir.
* Bir dizi öğesinin metin değeri boşluk dahil 100 karakteri aşamaz.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt, Giriş dizisindeki her bir dize için bir sonuç içeren bir JSON dizisidir. Bir sonuç nesnesi aşağıdaki özellikleri içerir:

  * `normalizedSource`: Kaynak terimin normalleştirilmiş biçimini sağlayan bir dize. Genellikle, bu, isteğin gövdesinde eşleşen liste dizininde bulunan `Text` alanın değeriyle aynı olmalıdır.
    
  * `normalizedTarget`: Hedef terimin normalleştirilmiş biçimini sağlayan bir dize. Genellikle, bu, isteğin gövdesinde eşleşen liste dizininde bulunan `Translation` alanın değeriyle aynı olmalıdır.
  
  * `examples`: (Kaynak terim, hedef terim) çifti için örneklerin listesi. Listenin her öğesi, aşağıdaki özelliklere sahip bir nesnedir:

    * `sourcePrefix`: Tamamen bir örnek oluşturmak _before_ `sourceTerm` için değerinden önce birleştirilecek dize. Bir boşluk karakteri eklemeyin, çünkü zaten olması gerekir. Bu değer boş bir dize olabilir.

    * `sourceTerm`: Aranan gerçek terime eşit bir dize. Dize, ve ' nin `sourcePrefix` tamamını `sourceSuffix` oluşturmak için ve birlikte eklenir. Değeri, bir kullanıcı arabiriminde işaretlenebilir, örneğin, bu değer kalın hale gelir.

    * `sourceSuffix`: Tamamen bir örnek oluşturmak _after_ `sourceTerm` için değerinden sonra birleştirilecek dize. Bir boşluk karakteri eklemeyin, çünkü zaten olması gerekir. Bu değer boş bir dize olabilir.

    * `targetPrefix`: Şuna benzer `sourcePrefix` ancak hedef için bir dize.

    * `targetTerm`: Şuna benzer `sourceTerm` ancak hedef için bir dize.

    * `targetSuffix`: Şuna benzer `sourceSuffix` ancak hedef için bir dize.

    > [!NOTE]
    > Sözlükte örnek yoksa, yanıt 200 ' dir (Tamam), ancak `examples` liste boş bir liste olur.

## <a name="examples"></a>Örnekler

Bu örnek, Ingilizce `fly` ve onun İspanyolca çevirilerinden `volar`oluşan çift için örneklerin nasıl arama yapılacağını gösterir.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly', 'Translation':'volar'}]"
```

Yanıt gövdesi (açıklık için kısaltılmış):

```
[
    {
        "normalizedSource":"fly",
        "normalizedTarget":"volar",
        "examples":[
            {
                "sourcePrefix":"They need machines to ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Necesitan máquinas para ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },      
            {
                "sourcePrefix":"That should really ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Eso realmente debe ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```
