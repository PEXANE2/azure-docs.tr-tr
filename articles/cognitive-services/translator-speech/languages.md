---
title: Desteklenen diller-Translator Konuşma Çevirisi API'si
titlesuffix: Azure Cognitive Services
description: Translator Konuşma Çevirisi API'si tarafından desteklenen dilleri görüntüleyin.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: conceptual
ms.date: 3/5/2018
ms.author: nitinme
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 2f5b48f2dbc2d109c03613676c6a119fd971603b
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70965435"
---
# <a name="languages-supported-by-the-translator-speech-api"></a>Translator Konuşma Çevirisi API'si tarafından desteklenen diller

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Aşağıdaki diller konuşma çevirisi için desteklenir. Konuşma çevirisi için her iki dil de destekleniyorsa, konuşmayı konuşmaya veya konuşmayı metne dönüştürme özelliği vardır. Hedef dil konuşma çevirisi için desteklenmiyorsa, yalnızca konuşmayı metin çevirisi kullanılabilir.

| Konuşma dili    |
|:----------- |
| Arapça (Modern standart)      |
| Brezilya Portekizcesi     |
| Çince (Mandarin)      |
| Türkçe      |
| Fransızca      |
| Almanca      |
| İtalyanca      |
| Japonca      |
| Rusça      |
| İspanyolca      |

Translator Konuşma Çevirisi API'si, konuşma için metin çevirisi için bir hedef dil olarak aşağıdaki dilleri destekler.

| SMS dili    | Dil kodu |
|:----------- |:-------------:|
| Afrikaner dili      | `af`          |
| Arapça       | `ar`          |
| Bangla      | `bn`          |
| Boşnakça (Latin)      | `bs`          |
| Bulgarca      | `bg`          |
| Kanton (Geleneksel)      | `yue`          |
| Katalanca      | `ca`          |
| Basitleştirilmiş Çince      | `zh-Hans`          |
| Geleneksel Çince      | `zh-Hant`          |
| Hırvatça      | `hr`          |
| Çekçe      | `cs`          |
| Danca      | `da`          |
| Felemenkçe      | `nl`          |
| Türkçe      | `en`          |
| Estonca      | `et`          |
| Fiji Adaları dili      | `fj`          |
| Filipin dili      | `fil`          |
| Fince      | `fi`          |
| Fransızca      | `fr`          |
| Almanca      | `de`          |
| Yunanca      | `el`          |
| Haiti Kreyolu      | `ht`          |
| İbranice      | `he`          |
| Hintçe      | `hi`          |
| Hmong Daw      | `mww`          |
| Macarca      | `hu`          |
|İzlanda dili|`is`          |
| Endonezya dili      | `id`          |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
| Svahili dili      | `sw`          |
| Klingon      | `tlh`          |
| Klingon (plqaD)      | `tlh-Qaak`          |
| Korece      | `ko`          |
| Letonca      | `lv`          |
| Litvanca      | `lt`          |
| Malgaşça      | `mg`          |
| Malay dili      | `ms`          |
| Malta dili      | `mt`          |
| Norveççe      | `nb`          |
| Farsça      | `fa`          |
| Lehçe      | `pl`          |
| Portekizce      | `pt`          |
| Queretaro Otomi      | `otq`          |
| Rumence      | `ro`          |
| Rusça      | `ru`          |
| Samoaca      | `sm`          |
| Sırpça (Kiril)      | `sr-Cyrl`          |
| Sırpça (Latin)      | `sr-Latn`          |
| Slovakça     | `sk`          |
| Slovence      | `sl`          |
| İspanyolca      | `es`          |
| İsveççe      | `sv`          |
| Tahitian      | `ty`          |
| Tamil dili      | `ta`          |
| Tay Dili      | `th`          |
| Tonga Dili      | `to`          |
| Türkçe      | `tr`          |
| Ukrayna dili      | `uk`          |
| Urduca      | `ur`          |
| Vietnam dili      | `vi`          |
| Gaelce      | `cy`          |
| Yucatec Maya      | `yua`          |

## <a name="access-the-list-programmatically"></a>Listeye program aracılığıyla erişin

Diller kaynağını kullanarak desteklenen dillerin listesine programlı bir şekilde erişebilirsiniz. Liste, dil adının yanı sıra Ingilizce veya desteklenen başka bir dili de sağlar. Yeni diller kullanılabilir hale geldiğinde bu liste Translator Konuşma Çevirisi Hizmeti tarafından otomatik olarak güncelleştirilir.

Diller kaynağı konuşma, metin ve metin okuma için desteklenen dillerin listesini döndürür. Diller kaynağı kimlik doğrulaması gerektirmez.

[Diller metodunu denemek için API başvurusunu ziyaret edin](languages-reference.md)

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Microsoft Translator Web sitesindeki listeye erişin

Microsoft Translator Web sitesinde, dillere hızlı bir bakış için Translator Metin Çevirisi ve konuşma API 'Leri tarafından desteklenen tüm diller gösterilmektedir. Bu liste, dil kodları gibi geliştiriciye özgü bilgiler içermez.

[Dillerin listesine bakın](https://www.microsoft.com/translator/languages.aspx)
