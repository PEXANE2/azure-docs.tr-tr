---
title: Uygulama eğitimi-LUSıS
titleSuffix: Azure Cognitive Services
description: Eğitim, doğal dil anlama geliştirmek için Language Understanding (LUIS) uygulama sürümü eğitiminde işlemidir. Ekleme, düzenleme, etiketleme veya varlıkları, amacı veya konuşma silme gibi bir Modeli'ne güncelleştirmelerinden sonra LUIS uygulamanızı eğitin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/15/2019
ms.author: diberry
ms.openlocfilehash: 1da8ab3015730c6b3e1962301a34b1ad43b1aad6
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79219868"
---
# <a name="train-your-active-version-of-the-luis-app"></a>LUSıS uygulamasının etkin sürümünü eğitme 

Eğitim, doğal dil anlama geliştirmek için Language Understanding (LUIS) uygulamanızı eğitiminde işlemidir. Ekleme, düzenleme, etiketleme veya varlıkları, amacı veya konuşma silme gibi bir Modeli'ne güncelleştirmelerinden sonra LUIS uygulamanızı eğitin. 

Bir uygulamayı eğitim ve [test etme](luis-concept-test.md) işlemi yinelemeli bir işlemdir. LUIS uygulamanızı eğitme sonra varlıkları ve hedefleri doğru olarak tanınır olmadığını görmek için örnek Konuşma ile test edin. Değilseniz, güncelleştirmeleri LUIS uygulaması, eğitin ve test için yeniden yapın. 

Eğitim LUIS Portalı'nda etkin sürüme uygulanır. 

## <a name="how-to-train-interactively"></a>Etkileşimli olarak eğitme

[Luu portalındaki](https://www.luis.ai)yinelemeli işlemi başlatmak için, Ilk olarak lusıs uygulamanızı en az bir kez eğmeniz gerekir. Eğitim önce en az bir utterance her hedefi olduğundan emin olun.

1. **Uygulamalarım** sayfasında adını seçerek uygulamanıza erişin. 

1. Uygulamanızda, üst panelde **eğit** ' i seçin. 

1. Eğitim tamamlandığında tarayıcının en üstünde bir bildirim görüntülenir.

## <a name="training-date-and-time"></a>Eğitim tarihi ve saati

Eğitim tarihi ve saati GMT + 2 ' dir. 

## <a name="train-with-all-data"></a>Tüm verilerle eğitim

Eğitim negatif örnekleme küçük bir yüzdesine kullanır. Küçük negatif örnekleme yerine tüm verileri kullanmak istiyorsanız, [API](#version-settings-api-use-of-usealltrainingdata)'yi kullanın.

### <a name="version-settings-api-use-of-usealltrainingdata"></a>Sürüm Ayarları API 'SI Usealltraıningdata kullanımı

Bu özelliği kapatmak için `UseAllTrainingData` true olarak ayarlanan [Sürüm Ayarları API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) 'sini kullanın. 

## <a name="unnecessary-training"></a>Gereksiz eğitim

Tek her değişiklikten sonra eğitme gerekmez. Eğitim, sonra bir grup değişiklikleri modele uygulanır ve test etmek veya yayımlamak için yapmanız gereken sonraki adım olan yapılmalıdır. Eğitim, test veya yayımlamak ihtiyacınız yoksa, gerekli değildir. 

## <a name="training-with-the-rest-apis"></a>REST API'leri ile eğitim

Lua portalındaki eğitim, **eğitme** düğmesine basmanın tek adımından oluşur. REST API'leri ile eğitim iki adımlı bir işlemdir. Birincisi, HTTP POST ile [eğitim isteğidir](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) . Ardından HTTP Get ile [eğitim durumunu](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) isteyin. 

Eğitim tamamlandığında öğrenmek için tüm modelleri başarıyla eğitilir kadar durum yoklaması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Etkileşimli test](luis-interactive-test.md)
* [Toplu işlem testi](luis-how-to-batch-test.md)
