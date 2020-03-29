---
title: Tren uygulaması - LUIS
titleSuffix: Azure Cognitive Services
description: Eğitim, dil anlayışınızı geliştirmek için Dil Anlayışı (LUIS) uygulama sürümünü öğretme sürecidir. Varlıkları, niyetleri veya söyleyiyi ekleme, düzenleme, etiketleme veya silme gibi modelgüncellemelerinden sonra LUIS uygulamanızı eğitin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219868"
---
# <a name="train-your-active-version-of-the-luis-app"></a>LUIS uygulamasının aktif sürümünü eğitin 

Eğitim, dil anlayışınızı geliştirmek için Dil Anlayışı (LUIS) uygulamanızı öğretme sürecidir. Varlıkları, niyetleri veya söyleyiyi ekleme, düzenleme, etiketleme veya silme gibi modelgüncellemelerinden sonra LUIS uygulamanızı eğitin. 

Bir uygulamayı eğitmek ve [test etmek](luis-concept-test.md) yinelemeli bir süreçtir. LUIS uygulamanızı eğittinden sonra, niyetve varlıkların doğru tanıyıp tanınmadınsını görmek için bu uygulamayı örnek sözlerle test elersiniz. Değilse, LUIS uygulamasında güncellemeler yapın, eğitin ve tekrar test edin. 

Eğitim LUIS portalındaki aktif sürüme uygulanır. 

## <a name="how-to-train-interactively"></a>Etkileşimli eğitim nasıl

[LUIS portalında](https://www.luis.ai)yinelemeli işlemi başlatmak için öncelikle LUIS uygulamanızı en az bir kez eğitmeniz gerekir. Eğitimden önce her niyetin en az bir söz aldığından emin olun.

1. **Uygulamalarım** sayfasında adını seçerek uygulamanıza erişin. 

1. Uygulamanızda üst panelde **Tren'i** seçin. 

1. Eğitim tamamlandığında, tarayıcının üst kısmında bir bildirim görüntülenir.

## <a name="training-date-and-time"></a>Eğitim tarihi ve saati

Eğitim tarihi ve saati GMT + 2'dir. 

## <a name="train-with-all-data"></a>Tüm verilerle eğitim

Eğitim, negatif örneklemenin küçük bir yüzdesini kullanır. Küçük negatif örnekleme yerine tüm verileri kullanmak istiyorsanız, [API'yi](#version-settings-api-use-of-usealltrainingdata)kullanın.

### <a name="version-settings-api-use-of-usealltrainingdata"></a>UseAllTrainingData sürüm ayarları API kullanımı

Bu özelliği kapatmak için `UseAllTrainingData` true setli [Sürüm ayarları API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) kullanın. 

## <a name="unnecessary-training"></a>Gereksiz eğitim

Her değişiklikten sonra antrenman yapmanıza gerek yok. Eğitim, modele bir dizi değişiklik uygulandıktan sonra yapılmalıdır ve yapmak istediğiniz bir sonraki adım test etmek veya yayımlamaktır. Test etmeniz veya yayınlamanız gerekmiyorsa, eğitim etüt gerekmez. 

## <a name="training-with-the-rest-apis"></a>REST API'leri ile eğitim

LUIS portalında eğitim Tren **düğmesine** basarak tek bir adımdır. REST API'leri ile eğitim iki aşamalı bir süreçtir. Bunlardan ilki HTTP POST ile [eğitim talep](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) etmektir. Ardından HTTP Get ile [eğitim durumunu](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) isteyin. 

Eğitimin ne zaman tamamlanacağını bilmek için, tüm modeller başarılı bir şekilde eğitilene kadar durumu yoklamanız gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Etkileşimli test](luis-interactive-test.md)
* [Toplu işe testi](luis-how-to-batch-test.md)
