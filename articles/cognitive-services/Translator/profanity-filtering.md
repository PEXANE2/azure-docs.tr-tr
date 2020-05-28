---
title: Küfür filtrelemesi-çevirmen
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler çeviricisinin metinlerinize çevrilmiş bir küfür düzeyini öğrenmek için uygunsuz metin filtrelemeyi kullanın.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: swmachan
ms.openlocfilehash: 7ebfe766e6362a3f62e70db8bf2dcae370aceee3
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996168"
---
# <a name="add-profanity-filtering-with-the-translator"></a>Çevirmenle bir küfür filtresi ekleme

Normalde çevirmen hizmeti, çevirinin kaynağında bulunan küfür 'ı korur. Küfür derecesi ve küfürlü sözcüklerini yapan bağlam, kültürler arasında farklılık gösterir. Sonuç olarak, hedef dildeki küfür derecesi, daha fazla veya daha az olabilir.

Kaynak metinde küfür mevcut olsa bile, çeviri içinde küfür görmekten kaçınmak isterseniz, Translate () yönteminde bulunan küfür filtreleme seçeneğini kullanın. Bu seçenek, küfür silinip silinmeyeceğini, uygun etiketlerle işaretlenip işaretlenmediğini veya hiçbir eylemde bulunulmayacağını seçmenizi sağlar.

Translate () yöntemi "ProfanityAction" Yeni öğesini içeren "Options" parametresini alır. Kabul edilen ProfanityAction değeri "NoAction", "Işaretlendi" ve "Deleted" dir.

## <a name="accepted-values-of-profanityaction-and-examples"></a>ProfanityAction ve örneklerin kabul edilen değerleri
|ProfanityAction değeri | Eylem | Örnek: kaynak-Japonca | Örnek: hedef-Ingilizce|
| :---|:---|:---|:---|
| NoAction | Varsayılan. Seçeneği ayarlamaya benzer. Küfür kaynaktan hedefe geçiyor. | 彼は変態です. | Bu bir Jerk. |
| İm | Küfürlü kelimeleri XML etiketleriyle çevrelenmiş \<profanity> ... \</profanity> . | 彼は変態です. | Bu bir \<profanity> Jerk \</profanity> . |
| Silindi | Küfürlü sözcükler, değişiklik yapılmadan çıktıdan kaldırılır. | 彼は. | Bir. |

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Çevirmen çağrlarınız ile uygunsuz filtre uygulama](reference/v3-0-translate.md)
