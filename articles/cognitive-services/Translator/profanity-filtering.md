---
title: Küfür filtrelemesi-Translator Metin Çevirisi API'si
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler Translator Metin Çevirisi API'si metninizi çevrilmiş bir küfür düzeyini öğrenmek için uygunsuz metin filtrelemeyi kullanın.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: e7e2097e0d3daf360f1fa0f30bf3fd2c62c07163
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "73836221"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>Translator Metin Çevirisi API'si ile küfür filtrelemesi ekleme

Normalde çevirmen hizmeti, çevirinin kaynağında bulunan küfür 'ı korur. Küfür derecesi ve küfürlü sözcüklerini yapan bağlam, kültürler arasında farklılık gösterir. Sonuç olarak, hedef dildeki küfür derecesi, daha fazla veya daha az olabilir.

Kaynak metinde küfür mevcut olsa bile, çeviri içinde küfür görmekten kaçınmak isterseniz, Translate () yönteminde bulunan küfür filtreleme seçeneğini kullanın. Bu seçenek, küfür silinip silinmeyeceğini, uygun etiketlerle işaretlenip işaretlenmediğini veya hiçbir eylemde bulunulmayacağını seçmenizi sağlar.

Translate () yöntemi "ProfanityAction" Yeni öğesini içeren "Options" parametresini alır. Kabul edilen ProfanityAction değeri "NoAction", "Işaretlendi" ve "Deleted" dir.

## <a name="accepted-values-of-profanityaction-and-examples"></a>ProfanityAction ve örneklerin kabul edilen değerleri
|ProfanityAction değeri | Eylem | Örnek: kaynak-Japonca | Örnek: hedef-Ingilizce|
| :---|:---|:---|:---|
| NoAction | Varsayılan. Seçeneği ayarlamaya benzer. Küfür kaynaktan hedefe geçiyor. | 彼は変態です. | Bu bir Jerk. |
| İm | Küfürlü kelimeler, XML etiketleri \<küfür>... \</küfür>. | 彼は変態です. | >Jerk \<\</küfür> bir küfür. |
| Silme | Küfürlü sözcükler, değişiklik yapılmadan çıktıdan kaldırılır. | 彼は. | Bir. |

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Çevirmen API çağrlarınız ile uygunsuz filtre uygulama](reference/v3-0-translate.md)
