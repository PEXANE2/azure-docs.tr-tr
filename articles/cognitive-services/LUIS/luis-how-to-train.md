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
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 259ea23c05f0c0a138ad54b6efd11aad2061cf7a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73500227"
---
# <a name="train-your-active-version-of-the-luis-app"></a>LUSıS uygulamasının etkin sürümünü eğitme 

Eğitim, doğal dil anlama sürecini geliştirmek üzere Language Understanding (LUSıS) uygulamanızı öğretirme işlemidir. Varlık, amaç veya sayaç ekleme, düzenlenme, etiketleme veya silme gibi modeldeki güncelleştirmelerden sonra LUSıS uygulamanızı eğitme. 

Bir uygulamayı eğitim ve [test etme](luis-concept-test.md) işlemi yinelemeli bir işlemdir. LUSıS uygulamanızı eğitdikten sonra, amaçları ve varlıkların doğru şekilde tanınıp tanınmadığını görmek için örnek bir şekilde test edersiniz. Aksi takdirde, LUSıS uygulaması, eğit ve test güncelleştirmelerini yeniden yapın. 

Eğitim, LUI portalındaki etkin sürüme uygulanır. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

## <a name="how-to-train-interactively"></a>Etkileşimli olarak eğitme

[Luu portalındaki](https://www.luis.ai)yinelemeli işlemi başlatmak için, Ilk olarak lusıs uygulamanızı en az bir kez eğmeniz gerekir. Her amacınmadan önce en az bir deterance olduğundan emin olun.

1. **Uygulamalarım** sayfasında adını seçerek uygulamanıza erişin. 

2. Uygulamanızda, üst panelde **eğit** ' i seçin. 

3. Eğitim tamamlandığında tarayıcının üst kısmında yeşil bir bildirim çubuğu görüntülenir.

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Uygulamanızda örnek SÖYLEYEK içermeyen bir veya daha fazla amaç varsa, uygulamanızı eğitemezsiniz. Tüm amaçlarınız için utterer ekleyin. Daha fazla bilgi için bkz. [örnek ekleme örnekleri](luis-how-to-add-example-utterances.md).

## <a name="training-date-and-time"></a>Eğitim tarihi ve saati

Eğitim tarihi ve saati GMT + 2 ' dir. 

## <a name="train-with-all-data"></a>Tüm verilerle eğitme

Eğitim, negatif örnekleme küçük bir yüzdesini kullanır. 

Küçük negatif örnekleme yerine tüm verileri kullanmak istiyorsanız, [API](#version-settings-api-use-of-usealltrainingdata)'yi kullanın.

<!--

 or the [LUIS portal setting](#luis-portal-setting-to-use-all-training-data)

### LUIS portal setting to use all training data

!!!IGNITE


-->

### <a name="version-settings-api-use-of-usealltrainingdata"></a>Sürüm Ayarları API 'SI Usealltraıningdata kullanımı

Bu özelliği kapatmak için `UseAllTrainingData` true olarak ayarlanan [Sürüm Ayarları API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) 'sini kullanın. 

## <a name="unnecessary-training"></a>Gereksiz eğitim

Her tek değişiklikten sonra eğmeniz gerekmez. Modele bir grup değişiklik uygulandıktan sonra eğitim yapılmalıdır ve yapmak istediğiniz sonraki adım test ya da yayımlamaktır. Test veya yayımlama gerekmiyorsa eğitim gerekli değildir. 

## <a name="training-with-the-rest-apis"></a>REST API 'lerle eğitim

Lua portalındaki eğitim, **eğitme** düğmesine basmanın tek adımından oluşur. REST API 'lerle eğitim, iki adımlı bir işlemdir. Birincisi, HTTP POST ile [eğitim isteğidir](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) . Ardından HTTP Get ile [eğitim durumunu](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) isteyin. 

Eğitimin tamamlandığını bilmeniz için, tüm modeller başarılı bir şekilde eğitilene kadar durumu yoklamalıdır. 

## <a name="next-steps"></a>Sonraki adımlar

* [LUSıS ile önerilen bir şekilde etiketleme](luis-how-to-review-endpoint-utterances.md) 
* [LUSıS uygulamanızın performansını artırmak için özellikleri kullanın](luis-how-to-add-features.md) 
