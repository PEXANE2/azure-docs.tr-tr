---
title: Uygulama eğitimi-LUSıS
titleSuffix: Azure Cognitive Services
description: Eğitim, doğal dil anlama 'yı geliştirmek üzere Language Understanding (LUSıS) uygulama sürümünüzü öğretirecek bir işlemdir. Varlık, amaç veya sayaç ekleme, düzenlenme, etiketleme veya silme gibi modeldeki güncelleştirmelerden sonra LUSıS uygulamanızı eğitme.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79219868"
---
# <a name="train-your-active-version-of-the-luis-app"></a>LUSıS uygulamasının etkin sürümünü eğitme 

Eğitim, doğal dil anlama sürecini geliştirmek üzere Language Understanding (LUSıS) uygulamanızı öğretirme işlemidir. Varlık, amaç veya sayaç ekleme, düzenlenme, etiketleme veya silme gibi modeldeki güncelleştirmelerden sonra LUSıS uygulamanızı eğitme. 

Bir uygulamayı eğitim ve [test etme](luis-concept-test.md) işlemi yinelemeli bir işlemdir. LUSıS uygulamanızı eğitdikten sonra, amaçları ve varlıkların doğru şekilde tanınıp tanınmadığını görmek için örnek bir şekilde test edersiniz. Aksi takdirde, LUSıS uygulaması, eğit ve test güncelleştirmelerini yeniden yapın. 

Eğitim, LUI portalındaki etkin sürüme uygulanır. 

## <a name="how-to-train-interactively"></a>Etkileşimli olarak eğitme

[Luu portalındaki](https://www.luis.ai)yinelemeli işlemi başlatmak için, Ilk olarak lusıs uygulamanızı en az bir kez eğmeniz gerekir. Her amacınmadan önce en az bir deterance olduğundan emin olun.

1. **Uygulamalarım** sayfasında adını seçerek uygulamanıza erişin. 

1. Uygulamanızda, üst panelde **eğit** ' i seçin. 

1. Eğitim tamamlandığında tarayıcının en üstünde bir bildirim görüntülenir.

## <a name="training-date-and-time"></a>Eğitim tarihi ve saati

Eğitim tarihi ve saati GMT + 2 ' dir. 

## <a name="train-with-all-data"></a>Tüm verilerle eğitme

Eğitim, negatif örnekleme küçük bir yüzdesini kullanır. Küçük negatif örnekleme yerine tüm verileri kullanmak istiyorsanız, [API](#version-settings-api-use-of-usealltrainingdata)'yi kullanın.

### <a name="version-settings-api-use-of-usealltrainingdata"></a>Sürüm Ayarları API 'SI Usealltraıningdata kullanımı

Bu özelliği kapatmak için [Sürüm Ayarları API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) 'sini true olarak `UseAllTrainingData` ayarla ' yı kullanın. 

## <a name="unnecessary-training"></a>Gereksiz eğitim

Her tek değişiklikten sonra eğmeniz gerekmez. Modele bir grup değişiklik uygulandıktan sonra eğitim yapılmalıdır ve yapmak istediğiniz sonraki adım test ya da yayımlamaktır. Test veya yayımlama gerekmiyorsa eğitim gerekli değildir. 

## <a name="training-with-the-rest-apis"></a>REST API 'lerle eğitim

Lua portalındaki eğitim, **eğitme** düğmesine basmanın tek adımından oluşur. REST API 'lerle eğitim, iki adımlı bir işlemdir. Birincisi, HTTP POST ile [eğitim isteğidir](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) . Ardından HTTP Get ile [eğitim durumunu](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) isteyin. 

Eğitimin tamamlandığını bilmeniz için, tüm modeller başarılı bir şekilde eğitilene kadar durumu yoklamalıdır. 

## <a name="next-steps"></a>Sonraki adımlar

* [Etkileşimli test](luis-interactive-test.md)
* [Toplu işe testi](luis-how-to-batch-test.md)
