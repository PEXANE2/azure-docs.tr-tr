---
title: URL Önceden oluşturulmuş varlıklar - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makale, Dil Anlama (LUIS) url önceden oluşturulmuş varlık bilgilerini içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/04/2019
ms.author: diberry
ms.openlocfilehash: 49f145ce3e9022826abad5d274dd611bb2cc6530
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270359"
---
# <a name="url-prebuilt-entity-for-a-luis-app"></a>LUIS uygulaması için URL önceden oluşturulmuş varlık
URL varlığı etki alanı adları veya IP adresleri ile URL'leri ayıklar. Bu varlık zaten eğitilmiş olduğundan, uygulamaya URL içeren örnek sözcükler eklemeniz gerekmez. URL varlığı yalnızca `en-us` kültürde desteklenir.

## <a name="types-of-urls"></a>URL Türleri
Url, [Tanıyanlar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) GitHub deposundan yönetilir

## <a name="resolution-for-prebuilt-url-entity"></a>Önceden oluşturulmuş URL varlığı için çözünürlük

Aşağıdaki varlık nesneleri sorgu için döndürülür:

`https://www.luis.ai is a great cognitive services example of artificial intelligence`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametre ile `false`ayarlanır:

```json
"entities": {
    "url": [
        "https://www.luis.ai"
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/V3-verbose)

Aşağıdaki JSON `verbose` parametre ile `true`ayarlanır:

```json
"entities": {
    "url": [
        "https://www.luis.ai"
    ],
    "$instance": {
        "url": [
            {
                "type": "builtin.url",
                "text": "https://www.luis.ai",
                "startIndex": 0,
                "length": 17,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-response"></a>[V2 yanıtı](#tab/V2)

Aşağıdaki örnek, yapay zekanın büyük bir bilişsel hizmetler örneği https://www.luis.ai çözünürlüğünü gösterir

```json
"entities": [
    {
        "entity": "https://www.luis.ai",
        "type": "builtin.url",
        "startIndex": 0,
        "endIndex": 17
    }
]
```

* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Ordinal,](luis-reference-prebuilt-ordinal.md) [sayı](luis-reference-prebuilt-number.md)ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıkları hakkında bilgi edinin.
