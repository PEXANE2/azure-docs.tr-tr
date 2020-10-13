---
title: URL önceden oluşturulmuş varlıklar-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makale, Language Understanding (LUSıS) içindeki URL önceden oluşturulmuş varlık bilgilerini içerir.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/04/2019
ms.openlocfilehash: bc33fd1b90306a016c419f227fb9e73e83e8ea83
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91535262"
---
# <a name="url-prebuilt-entity-for-a-luis-app"></a>Bir LUSıS uygulaması için URL önceden oluşturulmuş varlık
URL varlığı, etki alanı adlarıyla veya IP adresleriyle URL 'Leri ayıklar. Bu varlık zaten eğitiltiğinden, uygulamaya URL 'Leri içeren örnek bir parametre eklemeniz gerekmez. URL varlığı yalnızca kültür ' de desteklenir `en-us` .

## <a name="types-of-urls"></a>URL türleri
URL, [Tanıyıcılar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) GitHub deposundan yönetiliyor

## <a name="resolution-for-prebuilt-url-entity"></a>Önceden oluşturulmuş URL varlığı için çözüm

Sorgu için aşağıdaki varlık nesneleri döndürülür:

`https://www.luis.ai is a great cognitive services example of artificial intelligence`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametresi olarak ayarlanmıştır `false` :

```json
"entities": {
    "url": [
        "https://www.luis.ai"
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 ayrıntılı yanıt](#tab/V3-verbose)

Aşağıdaki JSON `verbose` parametresi olarak ayarlanmıştır `true` :

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

Aşağıdaki örnek, https://www.luis.ai yapay zeka 'nın harika bir bilişsel hizmetler örneği olduğunu göstermektedir.

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

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Sıra](luis-reference-prebuilt-ordinal.md), [sayı](luis-reference-prebuilt-number.md)ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıkları hakkında bilgi edinin.
