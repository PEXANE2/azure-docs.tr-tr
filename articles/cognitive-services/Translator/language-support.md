---
title: Dil desteği-Translator Metin Çevirisi API'si
titleSuffix: Azure Cognitive Services
description: Translator Metin Çevirisi API'si, sinir makine çevirisi (NMT) kullanarak metin çevirisi için aşağıdaki dilleri destekler.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 12/02/2019
ms.author: swmachan
ms.openlocfilehash: 2ec8d389c99ad96e59bf49d4345855fa44d6c7aa
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76902007"
---
# <a name="language-and-region-support-for-the-translator-text-api"></a>Translator Metin Çevirisi API'si için dil ve bölge desteği

Translator Metin Çevirisi API'si metin çevirisi için aşağıdaki dilleri destekler. Sinir makine çevirisi (NMT), yüksek kaliteli AI destekli makine çevirileri için yeni standarttır ve bir sinir sistemi kullanılabilir olduğunda Translator Metin Çevirisi API'si v3 kullanılarak varsayılan olarak kullanılabilir.

[Makine çevirisinin nasıl çalıştığı hakkında daha fazla bilgi edinin](https://www.microsoft.com/translator/mt.aspx)

## <a name="translation"></a>Çeviri

**V2 Translator API 'SI**

> [!NOTE]
> V2, 30 Nisan 2018 tarihinde kullanımdan kaldırılmıştır. Yalnızca v3 'de bulunan yeni işlevsellikten yararlanmak için uygulamalarınızı v3 'e geçirin.

* Yalnızca istatistiksel: Bu dil için kullanılabilir sinir sistemi yok.
* Sinir kullanılabilir: bir sinir sistemi kullanılabilir. Sinir sistemine erişmek için `category=generalnn` parametresini kullanın.
* Sinir varsayılan: sinir, varsayılan çeviri sistemidir. Microsoft Translator hub ile kullanmak üzere istatistiksel sisteme erişmek için `category=smt` parametresini kullanın.
* Yalnızca sinir: yalnızca sinir çeviri kullanılabilir.

**V3 Translator API 'si** V3 Translator API 'SI varsayılan olarak sinir ve istatistiksel sistemler yalnızca sinir sistemi yoksa kullanılabilir.

> [!NOTE]
> Şu anda, sinir dillerinin bir alt kümesi özel çevirmende bulunabilir ve bunlar yavaş yavaş bir şekilde ekliyoruz. [Özel çevirmende mevcut olan dilleri görüntüleyin](#customization).

|Dil|  Dil kodu|  V3 APı 'SI|
|:-----|:-----:|:-----|
|Afrikaner dili| `af`|   Nöral|
|Arapça|    `ar`    |   Nöral|
|Bangla|    `bn`    |   Nöral|
|Boşnakça (Latin)|   `bs`    |   Nöral|
|Bulgarca| `bg`    |   Nöral|
|Kanton (Geleneksel)|   `yue`|  Ki|
|Katalanca|   `ca`    |   Ki|
|Basitleştirilmiş Çince|    `zh-Hans`|Nöral|
|Geleneksel Çince|   `zh-Hant`       |Nöral|
|Hırvatça|  `hr`    |Nöral|
|Çekçe| `cs`    |   Nöral|
|Danca|    `da`        |Nöral|
|Felemenkçe| `nl`|   Nöral|
|Türkçe|   `en`    |   Nöral|
|Estonca|  `et`    |   Nöral|
|Fiji Adaları dili|    `fj`    |   Ki|
|Filipin dili|  `fil`   |   Ki|
|Fince|   `fi`    |   Nöral|
|Fransızca|    `fr`    |   Nöral|
|Almanca|    `de`    |   Nöral|
|Yunanca| `el`    |   Nöral|
|Haiti Kreyolu|    `ht`        |Ki|
|İbranice |`he`   |Nöral
|Hintçe| `hi`    |   Nöral|
|Hmong Daw| `mww`   |   Ki|
|Macarca| `hu`    |   Nöral|
|İzlanda dili| `is`    |   Nöral|
|Endonezya dili|    `id`    |   Ki|
|İrlanda dili | `ga`| Nöral
|İtalyanca|   `it`    |   Nöral|
|Japonca|  `ja`    |   Nöral|
|Kannada dili|`kn`| Nöral
|Svahili dili| `sw`    |   Ki|
|Klingon|   `tlh`   |   Ki|
|Klingon (plqaD)|   `tlh-Qaak`  |   Ki|
|Korece |`ko`   |   Nöral|
|Letonca|   `lv`    |   Nöral|
|Litvanca|    `lt`    |   Nöral|
|Malgaşça|  `mg`    |   Ki|
|Malay dili| `ms`        |Ki|
|Malayalam dili| `ml` | Nöral
|Malta dili|   `mt`    |   Ki|
|Maori dili| `mi`  | Nöral|
|Norveççe| `nb`    |   Nöral|
|Farsça|   `fa`    |   Nöral|
|Lehçe|    `pl`    |   Nöral|
|Portekizce (Brezilya)|   `pt-br` |   Nöral|
|Portekizce (Portekiz)| `pt-pt` | Nöral
|Pencap dili|`pa`|Nöral
|Queretaro Otomi|   `otq`   |   Ki|
|Rumence|  `ro`    |   Nöral|
|Rusça|   `ru`    |   Nöral|
|Samoaca|    `sm`    |   Ki|
|Sırpça (Kiril)|    `sr-Cyrl`|  Ki|
|Sırpça (Latin)|   `sr-Latn`       |Ki|
|Slovakça|    `sk`    |   Nöral|
|Slovence| `sl`    |   Nöral|
|İspanyolca|   `es`    |   Nöral|
|İsveççe|   `sv`    |Nöral|
|Tahitian|  `ty`    |Ki|
|Tamil dili| `ta`    |   Nöral|
|Telugu dili|    `te`    |   Nöral|
|Tay Dili|  `th`    |   Nöral|
|Tonga Dili|    `to`    |   Ki|
|Türkçe|   `tr`        |Nöral|
|Ukrayna dili| `uk`    |   Nöral|
|Urduca|  `ur`    |   Ki|
|Vietnam dili|    `vi`    |   Nöral|
|Gaelce| `cy`    |   Nöral|
|Yucatec Maya|  `yua`   |   Ki|

> [!NOTE]
> Dil kodu `pt` varsayılan olarak `pt-br`, Portekizce (Brezilya) olur.

## <a name="transliteration"></a>Alfabe çevirisi

Alfabede bulunan yöntem aşağıdaki dilleri destekler. "To/from", "<-->", dilin listelenen betiklerden veya bunlardan herhangi birine dönüştürülmüş olduğunu gösterir. "-->", Dilin yalnızca bir betikten diğerine dönüştürülmüş olduğunu gösterir.

| Dil    | Dil kodu | Komut Dosyası | /Kimden | Komut Dosyası|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Arapça | `ar` | Arapça `Arab` | <--> | Latin `Latn` |
|Bangla  | `bn` | Bangla `Beng` | <--> | Latin `Latn` |
| ve | `zh-Hans` | Basitleştirilmiş Çince `Hans`| <--> | Latin `Latn` |
| ve | `zh-Hans` | Basitleştirilmiş Çince `Hans`| <--> | Geleneksel Çince `Hant`|
| seçenekleri yerine | `zh-Hant` | Geleneksel Çince `Hant`| <--> | Latin `Latn` |
| seçenekleri yerine | `zh-Hant` | Geleneksel Çince `Hant`| <--> | Basitleştirilmiş Çince `Hans` |
| Gucerat dili | `gu`  | Gucerat dili `Gujr` | --> | Latin `Latn` |
| İbranice | `he` | İbranice `Hebr` | <--> | Latin `Latn` |
| Hintçe | `hi` | Devanagari `Deva` | <--> | Latin `Latn` |
| Japonca | `ja` | Japonca `Jpan` | <--> | Latin `Latn` |
| Kannada dili | `kn` | Kannada dili `Knda` | --> | Latin `Latn` |
| Malayalam dili | `ml` | Malayalam dili `Mlym` | --> | Latin `Latn` |
| Marathi | `mr` | Devanagari `Deva` | --> | Latin `Latn` |
| Oriya | `or` | Odia `Orya` | <--> | Latin `Latn` |
| Pencap dili | `pa` | Gurmukhi `Guru`  | <--> | Latin `Latn`  |
| Sırpça (Kiril) | `sr-Cyrl` | Kiril `Cyrl`  | --> | Latin `Latn` |
| Sırpça (Latin) | `sr-Latn` | Latin `Latn` | --> | Kiril `Cyrl`|
| Tamil dili | `ta` | Tamil dili `Taml` | --> | Latin `Latn` |
| Telugu dili | `te` | Telugu dili `Telu` | --> | Latin `Latn` |
| Tay Dili | `th` | Tay dili `Thai` | <--> | Latin `Latn` |

## <a name="dictionary"></a>Sözlük

Sözlük, arama ve örnekler yöntemlerini kullanarak Ingilizce 'ye veya Ingilizce 'ye kadar aşağıdaki dilleri destekler.

| Dil    | Dil kodu |
|:----------- |:-------------:|
| Afrikaner dili      | `af`          |
| Arapça       | `ar`          |
| Bangla      | `bn`          |
| Boşnakça (Latin)      | `bs`          |
| Bulgarca      | `bg`          |
| Katalanca      | `ca`          |
| Basitleştirilmiş Çince      | `zh-Hans`          |
| Hırvatça      | `hr`          |
| Çekçe      | `cs`          |
| Danca      | `da`          |
| Felemenkçe      | `nl`          |
| Estonca      | `et`          |
| Fince      | `fi`          |
| Fransızca      | `fr`          |
| Almanca      | `de`          |
| Yunanca      | `el`          |
| Haiti Kreyolu      | `ht`          |
| İbranice      | `he`          |
| Hintçe      | `hi`          |
| Hmong Daw      | `mww`          |
| Macarca      | `hu`          |
| İzlanda dili    | `is`  |
| Endonezya dili      | `id`          |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
| Svahili dili      | `sw`          |
| Klingon      | `tlh`          |
| Korece      | `ko`          |
| Letonca      | `lv`          |
| Litvanca      | `lt`          |
| Malay dili      | `ms`          |
| Malta dili      | `mt`          |
| Norveççe      | `nb`          |
| Farsça      | `fa`          |
| Lehçe      | `pl`          |
| Portekizce (Brezilya)     | `pt-br`          |
| Rumence      | `ro`          |
| Rusça      | `ru`          |
| Sırpça (Latin)      | `sr-Latn`          |
| Slovakça     | `sk`          |
| Slovence      | `sl`          |
| İspanyolca      | `es`          |
| İsveççe      | `sv`          |
| Tamil dili      | `ta`          |
| Tay Dili      | `th`          |
| Türkçe      | `tr`          |
| Ukrayna dili      | `uk`          |
| Urduca      | `ur`          |
| Vietnam dili      | `vi`          |
| Gaelce      | `cy`          |

## <a name="detect"></a>Algılama

Translator Metin Çevirisi API'si, çeviri ve alfabede kullanılabilen tüm dilleri algılar.


## <a name="access-the-translator-text-api-language-list-programmatically"></a>Translator Metin Çevirisi API'si dil listesine programlı olarak erişin

Diller metodunu kullanarak Translator Metin Çevirisi API'si v 3.0 için desteklenen dillerin bir listesini alabilirsiniz. Liste özelliği, dil kodu ve dil adı ile Ingilizce veya desteklenen başka bir dilde görünüm görüntüleyebilirsiniz. Yeni diller kullanılabilir hale getirildiğinden bu liste Microsoft Translator hizmeti tarafından otomatik olarak güncelleştirilir.

[Dilleri görüntüle işlem başvurusu belgeleri](reference/v3-0-languages.md)

## <a name="customization"></a>Özelleştirme

Aşağıdaki diller, [özel çevirici](https://aka.ms/CustomTranslator)kullanılarak Ingilizce veya İngilizce arasında özelleştirme için kullanılabilir.

| Dil    | Dil kodu |
|:----------- |:-------------:|
| Arapça       | `ar`          |
| Bangla      | `bn`          |
| Boşnakça (Latin)      | `bs`          |
| Bulgarca      | `bg`          |
| Basitleştirilmiş Çince      | `zh-Hans`          |
|Geleneksel Çince|   `zh-Hant`   |
| Hırvatça      | `hr`          |
| Çekçe      | `cs`          |
| Danca      | `da`          |
| Felemenkçe      | `nl`          |
| Türkçe    | `en`     |
| Estonca      | `et`          |
| Fince      | `fi`          |
| Fransızca      | `fr`          |
| Almanca      | `de`          |
| Yunanca      | `el`          |
| İbranice      | `he`          |
| Hintçe      | `hi`          |
| Macarca      | `hu`          |
| İzlanda dili | `is` |
| Endonezya dili|   `id`    |
| İrlanda dili | `ga`  |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
| Svahili dili|    `sw`    |
| Korece      | `ko`          |
| Letonca      | `lv`          |
| Litvanca      | `lt`          |
| Malgaşça| `mg`    |
| Maori dili| `mi`  |
| Norveççe      | `nb`          |
| Farsça      | `fa`          |
| Lehçe      | `pl`          |
| Portekizce (Brezilya) | `pt-br` |
| Rumence      | `ro`          |
| Rusça      | `ru`          |
| Samoaca|   `sm`    |
| Sırpça (Latin)      | `sr-Latn`          |
| Slovakça     | `sk`          |
| Slovence      | `sl`          |
| İspanyolca      | `es`          |
| İsveççe      | `sv`          |
| Tay Dili      | `th`          |
| Türkçe      | `tr`          |
| Ukrayna dili      | `uk`          |
| Vietnam dili      | `vi`          |
| Gaelce | `cy` |

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Microsoft Translator Web sitesindeki listeye erişin

Microsoft Translator Web sitesinde, dillere hızlı bir bakış için Translator Metin Çevirisi ve konuşma API 'Leri tarafından desteklenen tüm diller gösterilmektedir. Bu liste, dil kodları gibi geliştiriciye özgü bilgiler içermez.

[Dillerin listesine bakın](https://www.microsoft.com/translator/languages.aspx)
