---
title: Küfür filtrelemesi-çevirmen
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler çeviricisinin metinlerinize çevrilmiş bir küfür düzeyini öğrenmek için uygunsuz metin filtrelemeyi kullanın.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: lajanuar
ms.openlocfilehash: 9f389d669e69dbfa6ec6d4d0b4716d2367443f17
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "98896808"
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
