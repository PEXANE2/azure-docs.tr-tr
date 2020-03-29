---
title: LUIS uygulamanızı test edin
titleSuffix: Azure Cognitive Services
description: Test, LUIS'e örnek söz verme ve LUIS tarafından tanınan niyet ve varlıkların yanıtını alma işlemidir.
author: diberry
manager: nitinme
services: cognitive-services
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: diberry
ms.openlocfilehash: 25b360f90a0920aad2ea5e68cda31a68be5d37a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73486678"
---
# <a name="testing-example-utterances-in-luis"></a>LUIS'te örnek söyleyişlerin test edilmesi

Test, LUIS'e örnek söz verme ve LUIS tarafından tanınan niyet ve varlıkların yanıtını alma işlemidir. 

LUIS'i etkileşimli olarak test edebilir veya bir erte bir söz verebilir veya bir söz verebilirsiniz. Sınama sırasında, geçerli etkin modelin tahmin yanıtını yayımlanmış modelin tahmin yanıtıyla karşılaştırabilirsiniz. 

<a name="A-test-score"></a>
<a name="Score-all-intents"></a>
<a name="E-(exponent)-notation"></a>

## <a name="what-is-a-score-in-testing"></a>Testte puan nedir?
Tahmin puanları hakkında daha fazla bilgi edinmek için [Tahmin puanı](luis-concept-prediction-score.md) kavramlarına bakın.

## <a name="interactive-testing"></a>Etkileşimli test
İnteraktif testler LUIS portalının **test** panelinden yapılır. Niyetlerin ve varlıkların nasıl tanımlanıp puanlandığını görmek için bir söz girebilirsiniz. LUIS, test panelinde bir sözde beklediğiniz gibi niyetleri ve varlıkları tahmin etmiyorsa, yeni bir söz olarak **Niyet** sayfasına kopyalayın. Sonra varlıklar için bu söyleyiş parçaları etiket ve LUIS tren. 

## <a name="batch-testing"></a>Toplu işe testi
Aynı anda birden fazla sözcük testi yapıyorsunuzsa [toplu iş testlerine](luis-concept-batch-test.md) bakın.

## <a name="endpoint-testing"></a>Bitiş noktası testi
[Uygulamanızın](luis-glossary.md#endpoint) en fazla iki sürümüyle bitiş noktasını kullanarak test edebilirsiniz. Uygulamanızın ana veya canlı sürümünüüretim **bitiş** noktası olarak **ayarlarken, evreleme** bitiş noktasına ikinci bir sürüm ekleyin. Bu yaklaşım size bir söyleyiş üç sürümü verir: [LUIS](luis-reference-regions.md) web sitesinin Test bölmesinde geçerli model ve iki farklı uç noktalarında iki sürüm. 

Tüm uç nokta testi kullanım kotanıza doğru sayılır. 

## <a name="do-not-log-tests"></a>Testleri günlüğe kaydetme
Bir bitiş noktasına karşı sınayırsanız ve sözcük günlüğe kaydolmasını istemiyorsanız, sorgu dizesi yapılandırmasını `logging=false` kullanmayı unutmayın.

## <a name="where-to-find-utterances"></a>Nerede söyleyiş bulmak için
LUIS, **uygulamalar** listesi sayfasından LUIS portalında indirilebilen tüm günlükteki tüm günlükleri ve [LUIS'in API'lerini yazarken](https://go.microsoft.com/fwlink/?linkid=2092087)saklar. 

LUIS'in emin olmadığı tüm söyleyişler, [LUIS](luis-reference-regions.md) web sitesinin **[Gözden Geçirin bitiş noktası söyleyiş](luis-how-to-review-endpoint-utterances.md)** sayfasında listelenmiştir. 

## <a name="remember-to-train"></a>Antrenman yapmayı unutma.
Modelde değişiklik yaptıktan sonra LUIS'i [eğitmeyi](luis-how-to-train.md) unutmayın. LUIS uygulamasındaki değişiklikler, uygulama eğitilene kadar testte görülmez. 

## <a name="best-practices"></a>En iyi uygulamalar
[En iyi uygulamaları](luis-concept-best-practices.md)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

* Söyleyişlerinizi [test](luis-interactive-test.md) etme hakkında daha fazla bilgi edinin.
