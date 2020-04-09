---
title: Konteyner sınırlamaları - LUIS
titleSuffix: Azure Cognitive Services
description: Desteklenen LUIS kapsayıcı dilleri.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 7fe773b35c5aba31b2fea66bd2be7b2745eac3ee
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879250"
---
# <a name="language-understanding-luis-container-limitations"></a>Dil Anlama (LUIS) konteyner sınırlamaları

LUIS konteynerler birkaç önemli sınırlamalar vardır. Desteklenmeyen bağımlılıklardan desteklenen dil alt kümesine kadar bu makalede bu kısıtlamalar ayrıntılarıyla anlatılır.

## <a name="supported-dependencies-for-latest-container"></a>Kapsayıcı için `latest` desteklenen bağımlılıklar

[//build/ 2019'da](https://news.microsoft.com/build2019/)piyasaya sürülen en son LUIS konteyneri aşağıdakileri destekleyecektir:

* [Yeni önceden oluşturulmuş etki alanları:](luis-reference-prebuilt-domains.md)Bu kurumsal odaklı etki alanları varlıklar, örnek söyleyerek ve desenler içerir. Bu etki alanlarını kendi kullanımınız için genişletin.

## <a name="unsupported-dependencies-for-latest-container"></a>Kapsayıcı için `latest` desteklenmeyen bağımlılıklar

[Kapsayıcı için dışa aktarmak için,](luis-container-howto.md#export-packaged-app-from-luis)DESTEKLENMEYEn bağımlılıkları LUIS uygulamanızdan kaldırmanız gerekir. Kapsayıcı için dışa aktarmayı denediğinizde, LUIS portalı kaldırmanız gereken bu desteklenmeyen özellikleri bildirir.

Aşağıdaki bağımlılıklardan herhangi birini **içermiyorsa** BIR LUIS uygulamasını kullanabilirsiniz:

Desteklenmeyen uygulama yapılandırmaları|Ayrıntılar|
|--|--|
|Desteklenmeyen konteyner kültürleri| Felemenkçe (`nl-NL`)<br>Japonca`ja-JP`( )<br>Almanca sadece [1.0.2 belirteç](luis-language-support.md#custom-tokenizer-versions)ile desteklenir.|
|Tüm kültürler için desteklenmeyen varlıklar|Tüm kültürler için [KeyPhrase](luis-reference-prebuilt-keyphrase.md) önceden oluşturulmuş varlık|
|İngilizce (`en-US`) kültürü için desteklenmeyen varlıklar|[GeographyV2](luis-reference-prebuilt-geographyV2.md) önceden oluşturulmuş varlıklar|
|Konuşma astarı|Dışa bağımlılıklar kapsayıcıda desteklenmez.|
|Yaklaşım analizi|Dışa bağımlılıklar kapsayıcıda desteklenmez.|
|Bing yazım denetimi|Dışa bağımlılıklar kapsayıcıda desteklenmez.|

## <a name="languages-supported"></a>Desteklenen diller

LUIS kapsayıcıları, LUIS tarafından [desteklenen dillerin](luis-language-support.md#languages-supported) bir alt kümesini destekler. LUIS kapları aşağıdaki dillerdeki söyleyiyi anlayama yeteneğine sahiptir:

| Dil | Yerel Ayar | Önceden oluşturulmuş etki alanı | Önceden oluşturulmuş varlık | İfade listesi önerileri | **[Metin analizi](../text-analytics/language-support.md)<br>(Duygu ve<br>Anahtar Kelimeler)|
|--|--|:--:|:--:|:--:|:--:|
| Amerikan İngilizcesi | `en-US` | ✔️ | ✔️ | ✔️ | ✔️ |
| *[Çince](#chinese-support-notes) |`zh-CN` | ✔️ | ✔️ | ✔️ | ❌ |
| Fransızca (Fransa) |`fr-FR` | ✔️ | ✔️ | ✔️ | ✔️ |
| Fransızca (Kanada) |`fr-CA` | ❌ | ❌ | ❌ | ✔️ |
| Almanca |`de-DE` | ✔️ | ✔️ | ✔️ | ✔️ |
| Hintçe | `hi-IN`| ❌ | ❌ | ❌ | ❌ |
| İtalyanca |`it-IT` | ✔️ | ✔️ | ✔️ | ✔️ |
| Korece |`ko-KR` | ✔️ | ❌ | ❌ | *Yalnızca anahtar tümcecik* |
| Portekizce (Brezilya) |`pt-BR` | ✔️ | ✔️ | ✔️ | tüm alt kültürler |
| İspanyolca (İspanya) |`es-ES` | ✔️ | ✔️ |✔️|✔️|
| İspanyolca (Meksika)|`es-MX` | ❌ | ❌ |✔️|✔️|
| Türkçe | `tr-TR` |✔️| ❌ | ❌ | *Sadece duygusallık* |

[!INCLUDE [Chinese language support notes](includes/chinese-language-support-notes.md)]

[!INCLUDE [Text Analytics support notes](includes/text-analytics-support-notes.md)]