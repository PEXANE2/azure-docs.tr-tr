---
title: Küfür filtrelemesi-Translator Metin Çevirisi API'si
titleSuffix: Azure Cognitive Services
description: Translator Metin Çevirisi API'si, küfür filtrelemeyi kullanın.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 114dea098db5c824a1235ba1635f547383bf6743
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595183"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>Translator Metin Çevirisi API'si ile küfür filtrelemesi ekleme

Normalde çevirmen hizmeti, çevirinin kaynağında bulunan küfür 'ı korur. Küfür derecesi ve küfürlü sözcüklerini yapan bağlam, kültürler arasında farklılık gösterir. Sonuç olarak, hedef dildeki küfür derecesi, daha fazla veya daha az olabilir.

Kaynak metinde küfür mevcut olsa bile, çeviri içinde küfür görmekten kaçınmak isterseniz, Translate () yönteminde bulunan küfür filtreleme seçeneğini kullanın. Bu seçenek, küfür silinip silinmeyeceğini, uygun etiketlerle işaretlenip işaretlenmediğini veya hiçbir eylemde bulunulmayacağını seçmenizi sağlar.

Translate () yöntemi "ProfanityAction" Yeni öğesini içeren "Options" parametresini alır. Kabul edilen ProfanityAction değeri "NoAction", "Işaretlendi" ve "Deleted" dir.

## <a name="accepted-values-of-profanityaction-and-examples"></a>ProfanityAction ve örneklerin kabul edilen değerleri
|ProfanityAction değeri | Action | Örnek: Kaynak-Japonca | Örnek: Hedef-Ingilizce|
| :---|:---|:---|:---|
| NoAction | Varsayılan. Seçeneği ayarlamaya benzer. Küfür kaynaktan hedefe geçiyor. | 彼は変態です. | Bu bir Jerk. |
| İm | Küfürlü kelimeler, XML etiketleri \<küfür >... \</küfür >. | 彼は変態です. | > Jerk \<\</küfür > bir küfür. |
| Silinmiş | Küfürlü sözcükler, değişiklik yapılmadan çıktıdan kaldırılır. | 彼は. | Bir. |

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Çevirmen API çağrlarınız ile uygunsuz filtre uygulama](reference/v3-0-translate.md)
