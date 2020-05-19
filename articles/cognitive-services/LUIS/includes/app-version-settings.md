---
title: include dosyası
ms.topic: include
ms.custom: include file
ms.date: 5/04/2020
ms.openlocfilehash: 819dfa3127eb91a2f08687a76ea0586439b650a4
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83591004"
---
|Düzey|UI ayarı|API ayarı|Bilgi|
|--|--|--|--|
|Uygulama|Uç noktaları genel yap|`Public`|Tahmin anahtarı varsa ve uygulama KIMLIĞINIZI biliyorsanız, herkes herkese açık uygulamanıza erişebilir. |
|Sürüm|Belirleyici olmayan eğitim kullanma|`UseAllTrainingData`|Eğitim, negatif örnekleme küçük bir yüzdesini kullanır. Küçük negatif örnekleme yerine tüm verileri kullanmak istiyorsanız, olarak ayarlayın `true` . |
|Sürüm|Aksanları Normalleştir|`NormalizeDiacritics`|Aksanların normalleştirilmesi, karakterleri normal karakterlerle birlikte gelen aksan işaretleri ile değiştirir.|
|Sürüm|Noktalama işaretlerini Normalleştir|`NormalizePunctuation`|Noktalama işareti, modelleriniz eğitilen ve uç nokta sorgularınız tahmin etmeden önce, noktalama işaretlerinden kaldırılacak şekilde görünür.|
|Sürüm|Sözcük formlarını Normalleştir|`NormalizeWordForm`|Kök dışındaki sözcük biçimlerini yoksayın.|

Normalleştirme [kavramlarını](../luis-concept-utterance.md#utterance-normalization-for-diacritics-and-punctuation) ve bu ayarları güncelleştirmek için [uygulama](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58aeface39e2bb03dcd5909e) ve [Sürüm](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) API 'lerinin nasıl kullanılacağını öğrenin veya LUIS portalının **yönetme** bölümünü, **uygulama ayarları** sayfasını kullanın.