---
title: Metin Analizi v3 'de model sürümünü belirtin
titleSuffix: Azure Cognitive Services
description: Verilerinizde kullanılan Metin Analizi API'si modelini belirtmeyi öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 07/16/2020
ms.author: aahi
ms.openlocfilehash: e6763633ce27c537f1384e8e51020d44132c0377
ms.sourcegitcommit: 4feb198becb7a6ff9e6b42be9185e07539022f17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89469856"
---
# <a name="model-versioning-in-the-text-analytics-api"></a>Metin Analizi API'si model sürümü oluşturma

Metin Analizi API'si sürüm 3, verilerinizde kullanılan model sürümünü seçmenizi sağlar. `model-version`API isteklerinizin modelin sürümünü seçmek için isteğe bağlı parametresini kullanın. Örneğin: `<resource-url>/text/analytics/v3.0/sentiment?model-version=2020-04-01`. Bu parametre belirtilmemişse, API varsayılan olarak en son kararlı sürüme sahip olur. 

## <a name="available-versions"></a>Kullanılabilir sürümler

Her uç nokta tarafından hangi model sürümlerinin desteklendiğini bulmak için aşağıdaki tabloyu kullanın.


| Uç Noktası                        | Desteklenen Sürümler                                     | en son sürüm |
|---------------------------------|--------------------------------------------------------|----------------|
| `/sentiment`                    | `2019-10-01`, `2020-04-01`                             | `2020-04-01`   |
| `/languages`                    | `2019-10-01`, `2020-07-01`                             | `2020-07-01`   |
| `/entities/linking`             | `2019-10-01`, `2020-02-01`                             | `2020-02-01`   |
| `/entities/recognition/general` | `2019-10-01`, `2020-02-01`, `2020-04-01`,              | `2020-04-01`   |
| `/entities/recognition/pii`     | `2019-10-01`, `2020-02-01`, `2020-04-01`,`2020-07-01`  | `2020-07-01`   |
| `/keyphrases`                   | `2019-10-01`, `2020-07-01`                             | `2020-07-01`   |


Bu modellerin güncelleştirmeleriyle ilgili ayrıntıları, [Yenilikler bölümünde bulabilirsiniz](../whats-new.md).

## <a name="text-analytics-for-health"></a>Sistem durumu için Metin Analizi

[Sistem durumu kapsayıcısı için metin analizi](../how-tos/text-analytics-for-health.md) , yukarıdaki API uç noktalarından farklı model sürümü kullanır.  Kapsayıcı görüntüsü başına yalnızca bir model sürümünün kullanılabilir olduğunu lütfen unutmayın.

| Uç Noktası                        | Kapsayıcı görüntüsü etiketi                     | Model sürümü |
|---------------------------------|-----------------------------------------|---------------|
| `/entities/health`               | `1.1.013150001-amd64-preview` veya en son | `2020-07-24`  |
| `/domains/health`               | `1.1.012640001-amd64-preview`           | `2020-05-08`  |
| `/domains/health`               | `1.1.012420001-amd64-preview`           | `2020-05-08`  |
| `/domains/health`               | `1.1.012070001-amd64-preview`           | `2020-04-16`  |


## <a name="next-steps"></a>Sonraki adımlar

* [Metin Analizine genel bakış](../overview.md)
* [Yaklaşım analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Varlık tanıma](../how-tos/text-analytics-how-to-entity-linking.md)
