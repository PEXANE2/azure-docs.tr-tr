---
title: Yetişkin ve kcy içeriği-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Görüntü İşleme API 'sini kullanarak görüntülerde yetişkinlere ve kçsi içerikleri algılamayla ilgili kavramlar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: ca5f35ab47822d74de556671c38886942d23d9ff
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946274"
---
# <a name="detect-adult-and-racy-content"></a>Yetişkin ve kcy içeriğini algılama

Görüntü İşleme, geliştiricilerin yazılımında bu görüntülerin görüntülenmesini kısıtlayabilmeleri için resimlerde yetişkinlere yönelik malzemeleri algılayabilir. İçerik bayrakları, geliştiricilerin sonuçları kendi tercihlerine göre yorumlayabilmesi için sıfır ile diğeri arasında bir puan ile uygulanır. 

> [!NOTE]
> Bu özellik [Azure Content moderator](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview) hizmeti tarafından da sunulur. Metin denetleme ve insan incelemesi iş akışları gibi daha ayrıntılı içerik denetleme senaryolarına yönelik çözümler için bu alternatif bölümüne bakın.

## <a name="content-flag-definitions"></a>İçerik bayrağı tanımları

**Yetişkinlere yönelik** görüntüler, pornografik olarak tanımlananlar ve genellikle çıplaklık ve cinsel işlemler belirleyen bir şekilde tanımlanır. 

Kışkırtıcı görüntüler, açık cinsel içerikli görüntüler olarak tanımlanır ve genellikle **yetişkin**olarak etiketlenen görüntülerden daha az cinsel içerikli içerik içerir. 

## <a name="identify-adult-and-racy-content"></a>Yetişkin ve kcy içeriğini tanımla

[Analiz](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API 'si.

Görüntüyü çözümle yöntemi, iki Boole özelliği, `isAdultContent` ve `isRacyContent`sırasıyla yetişkin ve kcy içeriğini göstermek için yönteminin JSON yanıtında. Yöntemi, Ayrıca, Yetişkin ve kcy `racyScore`içeriğini belirlemek için güven puanlarını temsil eden iki özellik `adultScore` de döndürür.

## <a name="next-steps"></a>Sonraki adımlar

[Etki alanına özgü içeriği algılama](concept-detecting-domain-content.md) ve [yüzeyleri algılama](concept-detecting-faces.md)hakkında kavramlar öğrenin.
