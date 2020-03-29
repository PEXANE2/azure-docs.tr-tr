---
title: Küfür filtreleme - Çevirmen Metin API
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler ÇevirmenI Metin API'sinde metninizde çevrilen küfür düzeyini belirlemek için küfür filtresi kullanın.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: e7e2097e0d3daf360f1fa0f30bf3fd2c62c07163
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73836221"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>Çevirmen Metin API ile küfür filtresi ekleme

Normalde Çevirmen hizmeti, çeviride kaynakta bulunan küfürleri korur. Küfür derecesi ve kelimeleri küfür yapan bağlam kültürler arasında farklılık gösterir. Sonuç olarak, hedef dildeki küfür derecesi yükseltilebilir veya azaltılabilir.

Çeviride küfür görmekten kaçınmak istiyorsanız, kaynak metinde küfür olsa bile, Çeviri() yönteminde bulunan küfür filtreleme seçeneğini kullanın. Bu seçenek, küfür silinmiş görmek isteyip istemediğinizi, uygun etiketlerle işaretlenmiş veya hiçbir işlem gerçekleştirmemek istiyorsunuz seçmenize olanak tanır.

Translate() yöntemi, "KüfürEylem" yeni öğesini içeren "seçenekler" parametresini alır. KüfürEylem kabul edilen değerler "NoAction", "İşaretli" ve "Silindi."

## <a name="accepted-values-of-profanityaction-and-examples"></a>KüfürEylem ve örneklerin kabul edilen değerleri
|KüfürEylem değeri | Eylem | Örnek: Kaynak - Japonca | Örnek: Target - Türkçe|
| :---|:---|:---|:---|
| Eylem Yok | Varsayılan. Seçeneği ayarlamamak la aynı. Küfür kaynaktan hedefe geçer. | Bir çok şey yok. | O bir. |
| Işaretlenmiş | Küfür kelimeler XML etiketleri \<küfür> ile çevrilidir ... \</küfür>. | Bir çok şey yok. | O bir \<küfür>\<pislik / küfür>. |
| Silme | Saygısız sözcükler değiştirilmeden çıktıdan çıkarılır. | 2007'de, 2006 | O bir. |

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Translator API çağrınızla küfür filtresi uygulayın](reference/v3-0-translate.md)
