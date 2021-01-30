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
ms.date: 01/22/2021
ms.author: aahi
ms.openlocfilehash: 883c5a20612f4dab44c0d06776ee287b27174ab9
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99097312"
---
# <a name="supported-entity-categories-in-the-text-analytics-api-v3"></a>Metin Analizi API'si v3 içindeki desteklenen varlık kategorileri

[Adlandırılmış varlık tanıma](how-tos/text-analytics-how-to-entity-linking.md) (ner) tarafından döndürülebilecek varlık kategorilerini bulmak için bu makaleyi kullanın. NER, bir giriş belgesinden adlandırılmış varlıkları tanımlamak ve kategorilere ayırmak için bir tahmine dayalı model çalıştırır.

NER v 3.1 'nin bir önizlemesi de mevcuttur ve bu da kişisel ( `PII` ) ve sağlık () bilgilerini algılama özelliğini içerir `PHI` . Ayrıca **sistem durumu sekmesine tıklayarak** metin analizi sistem durumu için desteklenen kategorilerin bir listesini görüntüleyin. 

[Geçiş kılavuzunda](migration-guide.md?tabs=named-entity-recognition) sürüm 2,1 tarafından döndürülen türlerin listesini bulabilirsiniz

## <a name="entity-categories"></a>Varlık kategorileri

#### <a name="general"></a>[Genel](#tab/general)

[!INCLUDE [supported entity types - general](./includes/entity-types/general-entities.md)]

#### <a name="pii"></a>[TP](#tab/personal)

[!INCLUDE [supported entity types - personally identifying information](./includes/entity-types/personal-information-entities.md)]

#### <a name="health"></a>[Sağlık](#tab/health)

[!INCLUDE [biomedical entity types](./includes/entity-types/health-entities.md)]

**_

## <a name="next-steps"></a>Sonraki adımlar

_ [Metin analizi Içinde adlandırılmış varlık tanımayı kullanma](how-tos/text-analytics-how-to-entity-linking.md)
