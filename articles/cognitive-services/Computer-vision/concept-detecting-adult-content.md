---
title: Yetişkin, müstehcen, kanlı içerik - Bilgisayar Lı
titleSuffix: Azure Cognitive Services
description: Bilgisayarlı Vizyon APi'yi kullanarak görüntülerdeki yetişkinlere uygun içeriğin algılanmasıyla ilgili kavramlar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: ee18916a59bb081d65494f46e7aba7c29c7177cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "71718524"
---
# <a name="detect-adult-content"></a>Yetişkinlere uygun içeriği algılama

Computer Vision, geliştiricilerin bu görüntülerin yazılımlarında görüntülenmesini kısıtlayabilmeleri için görüntülerdeki yetişkinlere uygun materyalleri algılayabilir. İçerik bayrakları, geliştiricilerin sonuçları kendi tercihlerine göre yorumlayabilmeleri için sıfır ile bir arasında bir puanla uygulanır.

> [!NOTE]
> Bu işlevselliğin çoğu [Azure İçerik Moderatörhizmeti](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview) tarafından sunulur. Metin denetleme ve insan incelemesi iş akışları gibi daha sıkı içerik Denetleme senaryolarına yönelik çözümler için bu alternatife bakın.

## <a name="content-flag-definitions"></a>İçerik bayrağı tanımları

"Yetişkin" sınıflandırması içinde birkaç farklı kategori vardır:

- **Yetişkin** imgeleri doğada açıkça cinsel olan ve genellikle çıplaklık ve cinsel eylemleri betimleyen görüntüler olarak tanımlanır.
- **Müstehcen** görüntüler doğada cinsel açıdan müstehcen olan ve genellikle **Yetişkin**olarak etiketlenen resimlere göre daha az müstehcen içerik içeren görüntüler olarak tanımlanır.
- **Kanlı** imgeler, gore tasvir olanlar olarak tanımlanır.

## <a name="use-the-api"></a>API’yi kullanma

Görüntü API'si [ile](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) yetişkinlere uygun içeriği algılayabilirsiniz. `Adult` **VisualFeatures** sorgu parametresine değerini eklediğinizde, API üç boolean&mdash;`isAdultContent` `isRacyContent`özelliğini `isGoryContent` &mdash;ve JSON yanıtını döndürür. Yöntem ayrıca, her&mdash;`adultScore` `racyScore`bir `goreScore` &mdash;kategori için sıfır ve bir arasındaki güven puanlarını temsil eden ilgili özellikleri de döndürür.

- [Hızlı başlatma: Görüntüyü analiz et (.NET SDK)](./quickstarts-sdk/csharp-analyze-sdk.md)
- [Quickstart: Görüntüyü çözümleme (REST API)](./quickstarts/csharp-analyze.md)
