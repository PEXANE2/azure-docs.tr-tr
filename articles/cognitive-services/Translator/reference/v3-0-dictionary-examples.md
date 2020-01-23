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
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76548076"
---
# <a name="translator-text-api-30-dictionary-examples"></a>Translator Metin Çevirisi API'si 3,0: Sözlük örnekleri

Sözlükteki terimlerin bağlamda nasıl kullanıldığını gösteren örnekler sağlar. Bu işlem [Sözlük aramayla](./v3-0-dictionary-lookup.md)birlikte kullanılır.

## <a name="request-url"></a>İstek URL'si

`POST` isteği gönder:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesine geçirilen istek parametreleri şunlardır:

| Sorgu parametresi | Açıklama |
| --------- | ----------- |
| api-version <img width=200/> | **Gerekli parametre**.<br/>İstemci tarafından istenen API 'nin sürümü. Değer `3.0`olmalıdır. |
| başlangıç | **Gerekli parametre**.<br/>Giriş metninin dilini belirtir. Kaynak dili, `dictionary` kapsamında yer alan [desteklenen dillerden](./v3-0-languages.md) biri olmalıdır. |
| - | **Gerekli parametre**.<br/>Çıkış metninin dilini belirtir. Hedef dil `dictionary` kapsamında bulunan [desteklenen dillerden](./v3-0-languages.md) biri olmalıdır.  | 

İstek üstbilgileri şunları içerir:

| Üst bilgiler  | Açıklama |
| ------ | ----------- |
| Kimlik doğrulama üst bilgisi <img width=200/>  | **Gerekli istek üst bilgisi**.<br/><a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">Kimlik doğrulaması için kullanılabilen seçeneklere</a>bakın. |
| Content-Type | **Gerekli istek üst bilgisi**.<br/>Yükün içerik türünü belirtir. Olası değerler şunlardır: `application/json`. |
| İçerik Uzunluğu   | **Gerekli istek üst bilgisi**.<br/>İstek gövdesinin uzunluğu. |
| X-Clienttraceıd   | **İsteğe bağlı**.<br/>İsteği benzersiz şekilde tanımlamak için istemci tarafından oluşturulan bir GUID. `ClientTraceId`adlı bir sorgu parametresi kullanarak, sorgu dizesinde izleme KIMLIĞINI eklerseniz bu üstbilgiyi atlayabilirsiniz. |

## <a name="request-body"></a>İstek gövdesi

İsteğin gövdesi bir JSON dizisidir. Her dizi öğesi, aşağıdaki özelliklere sahip bir JSON nesnesidir:

  * `Text`: arama terimini belirten bir dize. Bu, önceki bir [Sözlük Arama](./v3-0-dictionary-lookup.md) isteğinin arka çevirilerine ait bir `normalizedText` alanının değeri olmalıdır. Ayrıca, `normalizedSource` alanının değeri de olabilir.

  * `Translation`: daha önce [Sözlük Arama](./v3-0-dictionary-lookup.md) işlemi tarafından döndürülen çevrilmiş metni belirten bir dize. Bu değer, [Sözlük Arama](./v3-0-dictionary-lookup.md) yanıtının `translations` listesindeki `normalizedTarget` alanından değeri olmalıdır. Hizmet, belirli kaynak hedefi sözcük çiftinin örneklerini döndürür.

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

  * `normalizedSource`: kaynak terimin normalleştirilmiş biçimini sağlayan bir dize. Genellikle, bu, isteğin gövdesindeki eşleşen liste dizininde bulunan `Text` alanı değeriyle aynı olmalıdır.
    
  * `normalizedTarget`: hedef terimin normalleştirilmiş biçimini sağlayan bir dize. Genellikle, bu, isteğin gövdesindeki eşleşen liste dizininde bulunan `Translation` alanı değeriyle aynı olmalıdır.
  
  * `examples`: (kaynak terim, hedef terim) çifti için örneklerin listesi. Listenin her öğesi, aşağıdaki özelliklere sahip bir nesnedir:

    * `sourcePrefix`: bir örnek oluşturmak için `sourceTerm` değerinden _önce_ birleştirilecek dize. Bir boşluk karakteri eklemeyin, çünkü zaten olması gerekir. Bu değer boş bir dize olabilir.

    * `sourceTerm`: aranan gerçek terime eşit bir dize. Dize, `sourcePrefix` ve `sourceSuffix` tüm örnekleri oluşturacak şekilde eklenir. Değeri, bir kullanıcı arabiriminde işaretlenebilir, örneğin, bu değer kalın hale gelir.

    * `sourceSuffix`: `sourceTerm` değerinden _sonra_ birleştirilecek dize, tüm bir örnek oluşturur. Bir boşluk karakteri eklemeyin, çünkü zaten olması gerekir. Bu değer boş bir dize olabilir.

    * `targetPrefix`: `sourcePrefix`, ancak hedefe benzer bir dize.

    * `targetTerm`: `sourceTerm`, ancak hedefe benzer bir dize.

    * `targetSuffix`: `sourceSuffix`, ancak hedefe benzer bir dize.

    > [!NOTE]
    > Sözlükte örnek yoksa, yanıt 200 ' dir (Tamam), ancak `examples` listesi boş bir liste.

## <a name="examples"></a>Örnekler

Bu örnek, Ingilizce terim `fly` ve onun Ispanyolca çeviri `volar`oluşan çift için örneklerin nasıl arama yapılacağını gösterir.

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
