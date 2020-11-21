---
title: Yetişkin, korcy, Gori içeriği-görüntü işleme
titleSuffix: Azure Cognitive Services
description: Görüntü İşleme API 'sini kullanarak görüntülerde yetişkinlere yönelik içeriği algılamayla ilgili kavramlar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 5cc8a4508ceeda245fbc10a81e16f3ecf05284c7
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95013626"
---
# <a name="detect-adult-content"></a>Yetişkin içeriğini algıla

Görüntü İşleme, geliştiricilerin bu görüntülerin yazılımda görüntülenmesini kısıtlayabilmeleri için resimlerde yetişkinlere yönelik malzemeleri algılayabilir. İçerik bayrakları, geliştiricilerin sonuçları kendi tercihlerine göre yorumlayabilmesi için sıfır ile diğeri arasında bir puan ile uygulanır.

> [!NOTE]
> Bu işlevlerin çoğu [Azure Content moderator](../content-moderator/overview.md) hizmeti tarafından sunulur. Metin denetleme ve insan incelemesi iş akışları gibi daha ayrıntılı içerik denetleme senaryolarına yönelik çözümler için bu alternatif bölümüne bakın.

## <a name="content-flag-definitions"></a>İçerik bayrağı tanımları

"Yetişkinlere yönelik" sınıflandırma içinde birkaç farklı kategoride:

- **Yetişkinlere yönelik** görüntüler, doğası gereği açıkça cinsel ve genellikle çıplaklık ve cinsel işlemler belirleyen bir şekilde tanımlanır.
- Kışkırtıcı görüntüler, açık cinsel **içerikli görüntüler olarak** tanımlanır ve genellikle **yetişkin** olarak etiketlenen görüntülerden daha az cinsel içerikli içerik içerir.
- **Gory** görüntüleri, gore ' i belirleyen şekilde tanımlanır.

## <a name="use-the-api"></a>API’yi kullanma

[Görüntü analizi](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b) API 'siyle yetişkinlere yönelik içeriği algılayabilirsiniz. Değerini `Adult` **visualfeatures** sorgu parametresine eklediğinizde, API üç Boole özelliği, &mdash; `isAdultContent` `isRacyContent` ve `isGoryContent` &mdash; JSON yanıtında döndürür. Yöntemi ayrıca karşılık gelen özellikleri döndürür &mdash; `adultScore` `racyScore` ve ilgili `goreScore` &mdash; her kategori için sıfır ile bir arasındaki güven puanlarını temsil eder.

- [Hızlı başlangıç: bir görüntüyü çözümleme (.NET SDK)](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
- [Hızlı başlangıç: bir görüntüyü çözümleme (REST API)](./quickstarts/csharp-analyze.md)