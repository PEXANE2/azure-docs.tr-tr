---
title: Basit varlık türü
titleSuffix: Language Understanding - Azure Cognitive Services
description: Basit bir varlık, tek bir kavramı açıklayan ve makine tarafından öğrenilen bağlamdan öğrenilmiş genel bir varlıktır. Basit varlıklar genellikle şirket adları, ürün adları veya diğer ad kategorileri gibi adlar olduğundan, kullanılan adların sinyalini artırmak için basit bir varlık kullanırken bir tümcecik listesi ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: 3f03b33f685bb5c7c9ba8f2267b8556c5dadade4
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68480225"
---
# <a name="simple-entity"></a>Basit varlık 

Basit bir varlık, tek bir kavramı açıklayan ve makine tarafından öğrenilen bağlamdan öğrenilmiş genel bir varlıktır. Basit varlıklar genellikle şirket adları, ürün adları veya diğer ad kategorileri gibi adlar olduğundan, kullanılan adların sinyalini artırmak için basit bir varlık kullanırken bir [tümcecik listesi](luis-concept-feature.md) ekleyin. 

**Varlık, şu durumlarda iyi bir uyum:**

* Veriler sürekli olarak biçimlendirilmemiştir ancak aynı şeyi gösterir. 

![varlığın](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>Örnek JSON

`Bob Jones wants 3 meatball pho`

Önceki utterance içinde `Bob Jones` basit etiketlenmiş `Customer` varlık.

Uç noktadan döndürülen veriler, varlık adı, utterance bulunan metni, bulunan metin ve puan konumunu içerir:

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

|Veri nesnesi|Varlık adı|Değer|
|--|--|--|
|Varlığın|`Customer`|`bob jones`|

## <a name="next-steps"></a>Sonraki adımlar

Bu [öğreticide](luis-quickstart-primary-and-secondary-data.md), **basit varlığı**kullanarak bir noktadan itibaren iş işi adının makineye öğrenilen verilerini ayıklayın. Ayıklama doğruluğunu artırmak için, basit varlığa özel terimlerin [tümcecik listesini](luis-concept-feature.md) ekleyin.