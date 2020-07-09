---
title: Adlandırılmış varlık tanıma için desteklenen Kategoriler
titleSuffix: Azure Cognitive Services
description: Metin Analizi API'si desteklenen varlık kategorileri hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 07/07/2020
ms.author: aahi
ms.openlocfilehash: e36a69be19844a75562f87d3c195494e3ef148a9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108522"
---
# <a name="supported-entity-categories-in-the-text-analytics-api-v3"></a>Metin Analizi API'si v3 içindeki desteklenen varlık kategorileri

[Adlandırılmış varlık tanıma](how-tos/text-analytics-how-to-entity-linking.md) (ner) tarafından döndürülebilecek varlık kategorilerini bulmak için bu makaleyi kullanın. NER v 3.1 'nin bir önizlemesi de mevcuttur ve bu da kişisel ( `PII` ) ve sağlık () bilgilerini algılama özelliğini içerir `PHI` . Ayrıca **sistem durumu sekmesine tıklayarak** metin analizi sistem durumu için desteklenen kategorilerin bir listesini görüntüleyin.

## <a name="entity-categories"></a>Varlık kategorileri

#### <a name="general"></a>[Genel](#tab/general)

[!INCLUDE [supported entity types - general](./includes/entity-types/general-entities.md)]

#### <a name="personal"></a>[Kişisel](#tab/personal)

[!INCLUDE [supported entity types - personally identifying information](./includes/entity-types/personal-information-entities.md)]

#### <a name="health"></a>[Sağlık](#tab/health)

[!INCLUDE [biomedical entity types](./includes/entity-types/health-entities.md)]

***

## <a name="next-steps"></a>Sonraki adımlar

* [Metin Analizi içinde adlandırılmış varlık tanımayı kullanma](how-tos/text-analytics-how-to-entity-linking.md)
