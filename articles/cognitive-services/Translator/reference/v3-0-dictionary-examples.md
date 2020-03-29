---
title: Çevirmen Metin API Sözlük Örnekleri Yöntemi
titleSuffix: Azure Cognitive Services
description: Çevirmen Metin API Sözlük Örnekleri yöntemi, sözlükteki terimlerin bağlam içinde nasıl kullanıldığını gösteren örnekler sağlar.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 01/21/2020
ms.author: swmachan
ms.openlocfilehash: b3551a8df19e47178c7bacd9218cfa60b66d81f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76548076"
---
# <a name="translator-text-api-30-dictionary-examples"></a>Çevirmen Metin API 3.0: Sözlük Örnekleri

Sözlükteki terimlerin bağlam içinde nasıl kullanıldığını gösteren örnekler sağlar. Bu işlem [Sözlük araması](./v3-0-dictionary-lookup.md)ile birlikte kullanılır.

## <a name="request-url"></a>İstek URL'si

Bir `POST` istek gönderin:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen istek parametreleri şunlardır:

| Sorgu Parametresi | Açıklama |
| --------- | ----------- |
| api-sürümü <img width=200/> | **Gerekli parametre**.<br/>İstemci tarafından istenen API sürümü. Değer. `3.0` |
| Kaynak | **Gerekli parametre**.<br/>Giriş metninin dilini belirtir. Kaynak `dictionary` dil, kapsamda bulunan [desteklenen dillerden](./v3-0-languages.md) biri olmalıdır. |
| - | **Gerekli parametre**.<br/>Çıktı metninin dilini belirtir. Hedef `dictionary` dil, kapsamda bulunan [desteklenen dillerden](./v3-0-languages.md) biri olmalıdır.  | 

İstek üstbilgisi şunları içerir:

| Üst bilgiler  | Açıklama |
| ------ | ----------- |
| Kimlik doğrulama üstbilgisi(lar) <img width=200/>  | **Gerekli istek üstbilgi**.<br/><a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">Kimlik doğrulama için kullanılabilir seçeneklere</a>bakın. |
| İçerik Türü | **Gerekli istek üstbilgi**.<br/>Yükün içerik türünü belirtir. Olası değerler `application/json`şunlardır: . |
| İçerik Uzunluğu   | **Gerekli istek üstbilgi**.<br/>İstek gövdesinin uzunluğu. |
| X-ClientTraceId   | **İsteğe bağlı**.<br/>İsteği benzersiz olarak tanımlamak için istemci tarafından oluşturulan GUID. 'li bir sorgu parametresi `ClientTraceId`kullanarak sorgu dizesinde izleme kimliği eklerseniz, bu üstbilginin atlayabilirsiniz. |

## <a name="request-body"></a>İstek gövdesi

İsteğin gövdesi bir JSON dizisidir. Her dizi öğesi aşağıdaki özelliklere sahip bir JSON nesnesidir:

  * `Text`: Aramak için terimbelirten bir dize. Bu, önceki sözlük `normalizedText` [arama](./v3-0-dictionary-lookup.md) isteğinin geri çevirilerinden bir alanın değeri olmalıdır. Aynı zamanda `normalizedSource` alanın değeri olabilir.

  * `Translation`: [Sözlük arama](./v3-0-dictionary-lookup.md) işlemi yle daha önce döndürülen çevrilmiş metni belirten bir dize. Bu, [Sözlük arama](./v3-0-dictionary-lookup.md) `normalizedTarget` yanıtı `translations` listesindeki alandaki değer olmalıdır. Hizmet, belirli kaynak hedef sözcük çifti için örnekler döndürecektir.

Bir örnek:

```json
[
    {"Text":"fly", "Translation":"volar"}
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 10 öğeye sahip olabilir.
* Bir dizi öğesinin metin değeri boşluklar da dahil olmak üzere 100 karakteri geçemez.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt, giriş dizisindeki her dize için bir sonucu olan bir JSON dizisidir. Sonuç nesnesi aşağıdaki özellikleri içerir:

  * `normalizedSource`: Kaynak terimin normalleştirilmiş biçimini veren bir dize. Genellikle, bu istek gövdesinde eşleşen `Text` liste dizini alan değeri ile aynı olmalıdır.
    
  * `normalizedTarget`: Hedef terimin normalleştirilmiş biçimini veren bir dize. Genellikle, bu istek gövdesinde eşleşen `Translation` liste dizini alan değeri ile aynı olmalıdır.
  
  * `examples`: (Kaynak terim, hedef terim) çiftine örnek listesi. Listenin her öğesi aşağıdaki özelliklere sahip bir nesnedir:

    * `sourcePrefix`: Tam bir örnek _before_ oluşturmak `sourceTerm` için değeri önce concatenate dize. Olması gereken zaman zaten orada olduğundan, bir boşluk karakteri eklemeyin. Bu değer boş bir dize olabilir.

    * `sourceTerm`: Gerçek terime eşit bir dize yukarı baktı. Dize ile `sourcePrefix` eklenir `sourceSuffix` ve tam örnek oluşturmak için. Değeri, örneğin bir kullanıcı arabiriminde kalınlaşarak işaretlenebileceği şekilde ayrılır.

    * `sourceSuffix`: Tam bir örnek _after_ oluşturmak `sourceTerm` için değer sonra concatenate dize. Olması gereken zaman zaten orada olduğundan, bir boşluk karakteri eklemeyin. Bu değer boş bir dize olabilir.

    * `targetPrefix`: Hedefe benzer `sourcePrefix` ama bir dize.

    * `targetTerm`: Hedefe benzer `sourceTerm` ama bir dize.

    * `targetSuffix`: Hedefe benzer `sourceSuffix` ama bir dize.

    > [!NOTE]
    > Sözlükte örnek yoksa, yanıt 200 (Tamam) ancak `examples` liste boş bir listedir.

## <a name="examples"></a>Örnekler

Bu örnek, İngilizce terim `fly` ve İspanyolca çevirisi `volar`nden oluşan çift için nasıl arama örnekleri gösterilmektedir.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly', 'Translation':'volar'}]"
```

Yanıt gövdesi (netlik için kısaltılır):

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
