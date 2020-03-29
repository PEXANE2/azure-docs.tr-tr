---
title: BLEU skoru nedir? - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: BLEU, makine çevirisi ile aynı kaynak cümlenin insan tarafından oluşturulan referans çevirileri arasındaki farkların bir ölçüsüdür.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: swmachan
ms.openlocfilehash: 85c4ee27a828a05c64ca6cbf84bff438535328be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73647367"
---
# <a name="what-is-a-bleu-score"></a>BLEU skoru nedir?

[BLEU (İki Dilli Değerlendirme Yedeği)](https://en.wikipedia.org/wiki/BLEU) otomatik çeviri ile aynı kaynak cümlenin bir veya daha fazla insan tarafından oluşturulan referans çevirileri arasındaki farkların bir ölçüsüdür.

## <a name="scoring-process"></a>Puanlama işlemi

BLEU algoritması, otomatik çevirinin ardışık tümcelerini başvuru çevirisinde bulduğu ardışık tümceciklerle karşılaştırır ve ağırlıklı bir şekilde eşleşme sayısını sayar. Bu maçlar pozisyon bağımsızdır. Daha yüksek bir eşleşme derecesi, başvuru çevirisiyle daha yüksek bir benzerlik derecesi ve daha yüksek puan gösterir. Anlaşılabilirlik ve dilbilgisi doğruluğu dikkate alınmaz.

## <a name="how-bleu-works"></a>BLEU nasıl çalışır?

BLEU'nun gücü, her cümle için tam insani yargıyı tasarlamaya çalışmak yerine, bir test korpusu üzerinden bireysel cümle yargısı hatalarını ortalamaya alarak insan yargısıyla iyi ilişkili olmasıdır.

BLEU puanları daha kapsamlı bir tartışma [burada](https://youtu.be/-UqDljMymMg).

BLEU sonuçları, etki alanınızın genişliğine, test verilerinin eğitim ve ayarlama verileriyle tutarlılığına ve eğitmek için ne kadar veriye sahip olduğunuza bağlıdır. Modelleriniz dar bir etki alanında eğitildiyse ve eğitim verileriniz test verilerinizle tutarlıysa, yüksek bir BLEU puanı bekleyebilirsiniz.

>[!NOTE]
>BLEU sonuçları aynı Test kümesi, aynı dil çifti ve aynı MT altyapısıyla karşılaştırıldığında, BLEU puanları arasındaki karşılaştırma yalnızca haklı dır. Farklı bir test kümesinden bleu puanı farklı olması kaçınılmazdır.
