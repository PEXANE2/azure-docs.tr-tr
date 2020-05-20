---
title: include dosyası
ms.topic: include
ms.custom: include file
ms.date: 5/17/2020
ms.openlocfilehash: 79a45825be66b44d869ef741917436de87a1f5d0
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83653165"
---
Normalleştirme [kavramlarını](../luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) ve bu ayarları güncelleştirmek için [Sürüm](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) API 'lerinin nasıl kullanılacağını ve LUIS portalının **Yönetim** bölümü, **Ayarlar** sayfasını kullanmayı öğrenin.


|UI ayarı|API ayarı|Bilgi|
|--|--|--|
|Belirleyici olmayan eğitim kullanma|`UseAllTrainingData`|Eğitim, negatif örnekleme küçük bir yüzdesini kullanır. Küçük negatif örnekleme yerine tüm verileri kullanmak istiyorsanız, olarak ayarlayın `true` . |
|Aksanları Normalleştir|`NormalizeDiacritics`|Aksanların normalleştirilmesi, karakterleri normal karakterlerle birlikte gelen aksan işaretleri ile değiştirir. Bu ayar yalnızca aksanları destekleyen [dillerde](../luis-reference-application-settings.md#diacritics-normalization) kullanılabilir.|
|Noktalama işaretlerini Normalleştir|`NormalizePunctuation`|Noktalama işareti, modelleriniz eğitilen ve uç nokta sorgularınız tahmin etmeden önce, noktalama işaretlerinden kaldırılacak şekilde görünür.|
|Sözcük formlarını Normalleştir|`NormalizeWordForm`|Kök dışındaki sözcük biçimlerini yoksayın.|
