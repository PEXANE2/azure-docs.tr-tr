---
title: Dil desteği-çevirici
titleSuffix: Azure Cognitive Services
description: Bilişsel hizmetler çevirmeni, sinir makine çevirisi (NMT) kullanarak metin çevirisi için aşağıdaki dilleri destekler.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/26/2020
ms.author: swmachan
ms.openlocfilehash: 6339b86166aff008bf17b5096d42629daf6e3401
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84434294"
---
# <a name="language-and-region-support-for-translator"></a>Çevirmen için dil ve bölge desteği

Çevirmen metin çevirisi için aşağıdaki dilleri destekler. Sinir makine çevirisi (NMT), yüksek kaliteli AI destekli makine çevirileri için yeni standarttır ve bir sinir sistemi kullanılabilir olduğunda varsayılan olarak v3 of Translator kullanılarak kullanılabilir.

[Makine çevirisinin nasıl çalıştığı hakkında daha fazla bilgi edinin](https://www.microsoft.com/translator/mt.aspx)

## <a name="translation"></a>Çeviri

**V2 çevirmeni**

> [!NOTE]
> V2, 30 Nisan 2018 tarihinde kullanımdan kaldırılmıştır. Yalnızca v3 'de bulunan yeni işlevsellikten yararlanmak için uygulamalarınızı v3 'e geçirin.

* Yalnızca istatistiksel: Bu dil için kullanılabilir sinir sistemi yok.
* Sinir kullanılabilir: bir sinir sistemi kullanılabilir. `category=generalnn`Sinir sistemine erişmek için parametresini kullanın.
* Sinir varsayılan: sinir, varsayılan çeviri sistemidir. `category=smt`Microsoft Translator hub 'ı ile kullanmak üzere istatistiksel sisteme erişmek için parametresini kullanın.
* Yalnızca sinir: yalnızca sinir çeviri kullanılabilir.

**V3 çevirmeni** V3 Translator varsayılan olarak sinir ve istatistiksel sistemler yalnızca sinir sistemi yoksa kullanılabilir.

> [!NOTE]
> Şu anda, sinir dillerinin bir alt kümesi özel çevirmende bulunabilir ve bunlar yavaş yavaş bir şekilde ekliyoruz. [Özel çevirmende mevcut olan dilleri görüntüleyin](#customization).

|Dil|    Dil kodu|    V3 APı 'SI|
|:-----|:-----:|:-----|
|Afrikaner|    `af`|    Sinir|
|Arapça|    `ar`    |    Sinir|
|Bangla|    `bn`    |    Sinir|
|Boşnakça (Latin)|    `bs`    |    Sinir|
|Bulgarca|    `bg`    |    Sinir|
|Cantonetıcı (Geleneksel)|    `yue`|    Ki|
|Katalanca|    `ca`    |    Ki|
|Basitleştirilmiş Çince|    `zh-Hans`|Sinir|
|Geleneksel Çince|    `zh-Hant`        |Sinir|
|Hırvatça|    `hr`    |Sinir|
|Çekçe|    `cs`    |    Sinir|
|Danca|    `da`        |Sinir|
|Felemenkçe|    `nl`|    Sinir|
|İngilizce|    `en`    |    Sinir|
|Estonya Dili|    `et`    |    Sinir|
|Fiji Adaları dili|    `fj`    |    Ki|
|Filipino|    `fil`    |    Ki|
|Fince|    `fi`    |    Sinir|
|Fransızca|    `fr`    |    Sinir|
|Almanca|    `de`    |    Sinir|
|Yunanca|    `el`    |    Sinir|
|Gucerat dili|    `gu`    |    Sinir|
|Haian Creole|    `ht`        |Ki|
|İbranice    |`he`    |Sinir
|Hintçe|    `hi`    |    Sinir|
|Hmong DAW|    `mww`    |    Ki|
|Macarca|    `hu`    |    Sinir|
|İzlandaca|    `is`    |    Sinir|
|Endonezce|    `id`    |    Ki|
|İrlandaca | `ga`| Sinir
|İtalyanca|    `it`    |    Sinir|
|Japonca|    `ja`    |    Sinir|
|Kannada dili|`kn`| Sinir|
|Kazakça |`kk`| Sinir|
|Svahili dili|    `sw`    |    Ki|
|Klingon|    `tlh-Latn`    |    Ki|
|Klingon (plqaD)|    `tlh-Piqd`    |    Ki|
|Korece    |`ko`    |    Sinir|
|Letonca|    `lv`    |    Sinir|
|Litvanca|    `lt`    |    Sinir|
|Malgaşça|    `mg`    |    Ki|
|Malayca|    `ms`        |Ki|
|Malayalam dili| `ml` | Sinir
|Maltaca|    `mt`    |    Ki|
|Maori dili| `mi`  | Sinir|
|Marathi| `mr`  | Sinir|
|Norveççe|    `nb`    |    Sinir|
|Farsça|    `fa`    |    Sinir|
|Lehçe|    `pl`    |    Sinir|
|Portekizce (Brezilya)|    `pt-br`    |    Sinir|
|Portekizce (Portekiz)| `pt-pt` | Sinir
|Pencap dili|`pa`|Sinir
|Queretaro Otomi|    `otq`    |    Ki|
|Rumence|    `ro`    |    Sinir|
|Rusça|    `ru`    |    Sinir|
|Samoan|    `sm`    |    Ki|
|Sırpça (Kiril)|    `sr-Cyrl`|    Ki|
|Sırpça (Latin)|    `sr-Latn`        |Ki|
|Slovakça|    `sk`    |    Sinir|
|Slovence|    `sl`    |    Sinir|
|İspanyolca|    `es`    |    Sinir|
|İsveççe|    `sv`    |Sinir|
|Tahiti dili|    `ty`    |Ki|
|Tamil dili|    `ta`    |    Sinir|
|Telugu dili|    `te`    |    Sinir|
|Tayca|    `th`    |    Sinir|
|Tonga dili|    `to`    |    Ki|
|Türkçe|    `tr`        |Sinir|
|Ukraynaca|    `uk`    |    Sinir|
|Urduca|    `ur`    |    Ki|
|Vietnamca|    `vi`    |    Sinir|
|Galce|    `cy`    |    Sinir|
|Yucatec Maya|    `yua`    |    Ki|

> [!NOTE]
> Dil kodu `pt` Varsayılan olarak `pt-br` , Portekizce (Brezilya) olur.

## <a name="transliteration"></a>Alfabeye çevirme

Alfabede bulunan yöntem aşağıdaki dilleri destekler. "To/from", "<-->", dilin listelenen betiklerden veya bunlardan herhangi birine dönüştürülmüş olduğunu gösterir. "-->", dilin yalnızca bir betikten diğerine dönüştürülmüş olduğunu gösterir.

| Dil    | Dil kodu | Komut Dosyası | Hedef/Kaynak | Komut Dosyası|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Arapça | `ar` | Arapça`Arab` | <--> | Tin`Latn` |
|Bangla  | `bn` | Bengali`Beng` | <--> | Tin`Latn` |
| Basitleştirilmiş Çince | `zh-Hans` | Basitleştirilmiş Çince`Hans`| <--> | Tin`Latn` |
| Basitleştirilmiş Çince | `zh-Hans` | Basitleştirilmiş Çince`Hans`| <--> | Geleneksel Çince`Hant`|
| Geleneksel Çince | `zh-Hant` | Geleneksel Çince`Hant`| <--> | Tin`Latn` |
| Geleneksel Çince | `zh-Hant` | Geleneksel Çince`Hant`| <--> | Basitleştirilmiş Çince`Hans` |
| Gucerat dili | `gu`  | Gucerat dili`Gujr` | <--> | Tin`Latn` |
| İbranice | `he` | İbranice`Hebr` | <--> | Tin`Latn` |
| Hintçe | `hi` | Devanagari`Deva` | <--> | Tin`Latn` |
| Japonca | `ja` | Japonca`Jpan` | <--> | Tin`Latn` |
| Kannada dili | `kn` | Kannada dili`Knda` | <--> | Tin`Latn` |
| Malayalam dili | `ml` | Malayalam dili`Mlym` | <--> | Tin`Latn` |
| Marathi | `mr` | Devanagari`Deva` | <--> | Tin`Latn` |
| Oriya | `or` | Oriya`Orya` | <--> | Tin`Latn` |
| Pencap dili | `pa` | Gurmukhi`Guru`  | <--> | Tin`Latn`  |
| Sırpça (Kiril) | `sr-Cyrl` | V`Cyrl`  | --> | Tin`Latn` |
| Sırpça (Latin) | `sr-Latn` | Tin`Latn` | --> | V`Cyrl`|
| Tamil dili | `ta` | Tamil dili`Taml` | <--> | Tin`Latn` |
| Telugu dili | `te` | Telugu dili`Telu` | <--> | Tin`Latn` |
| Tayca | `th` | Tay dili`Thai` | <--> | Tin`Latn` |

## <a name="dictionary"></a>Sözlük

Sözlük, arama ve örnekler yöntemlerini kullanarak Ingilizce 'ye veya Ingilizce 'ye kadar aşağıdaki dilleri destekler.

| Dil    | Dil kodu |
|:----------- |:-------------:|
| Afrikaner      | `af`          |
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
| Estonya Dili      | `et`          |
| Fince      | `fi`          |
| Fransızca      | `fr`          |
| Almanca      | `de`          |
| Yunanca      | `el`          |
| Haian Creole      | `ht`          |
| İbranice      | `he`          |
| Hintçe      | `hi`          |
| Hmong DAW      | `mww`          |
| Macarca      | `hu`          |
| İzlandaca    | `is`  |
| Endonezce      | `id`          |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
| Svahili dili      | `sw`          |
| Klingon      | `tlh-Latn`          |
| Korece      | `ko`          |
| Letonca      | `lv`          |
| Litvanca      | `lt`          |
| Malayca      | `ms`          |
| Maltaca      | `mt`          |
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
| Tayca      | `th`          |
| Türkçe      | `tr`          |
| Ukraynaca      | `uk`          |
| Urduca      | `ur`          |
| Vietnamca      | `vi`          |
| Galce      | `cy`          |

## <a name="detect"></a>Algılama

Çevirmen, çeviri ve alfabede kullanılabilen tüm dilleri algılar.


## <a name="access-the-translator-language-list-programmatically"></a>Çeviri dili listesine programlı olarak erişin

Diller metodunu kullanarak Translator v 3.0 için desteklenen dillerin bir listesini alabilirsiniz. Liste özelliği, dil kodu ve dil adı ile Ingilizce veya desteklenen başka bir dilde görünüm görüntüleyebilirsiniz. Yeni diller kullanılabilir hale getirildiğinden bu liste Microsoft Translator hizmeti tarafından otomatik olarak güncelleştirilir.

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
|Geleneksel Çince|    `zh-Hant`    |
| Hırvatça      | `hr`          |
| Çekçe      | `cs`          |
| Danca      | `da`          |
| Felemenkçe      | `nl`          |
| İngilizce    | `en`     |
| Estonya Dili      | `et`          |
| Fince      | `fi`          |
| Fransızca      | `fr`          |
| Almanca      | `de`          |
| Yunanca      | `el`          |
| İbranice      | `he`          |
| Hintçe      | `hi`          |
| Macarca      | `hu`          |
| İzlandaca | `is` |
| Endonezce|    `id`    |
| İrlandaca | `ga`    |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
| Svahili dili|    `sw`    |
| Korece      | `ko`          |
| Letonca      | `lv`          |
| Litvanca      | `lt`          |
| Malgaşça|    `mg`    |
| Maori dili| `mi`  |
| Norveççe      | `nb`          |
| Farsça      | `fa`          |
| Lehçe      | `pl`          |
| Portekizce (Brezilya) | `pt-br` |
| Rumence      | `ro`          |
| Rusça      | `ru`          |
| Samoan|    `sm`    |
| Sırpça (Latin)      | `sr-Latn`          |
| Slovakça     | `sk`          |
| Slovence      | `sl`          |
| İspanyolca      | `es`          |
| İsveççe      | `sv`          |
| Tayca      | `th`          |
| Türkçe      | `tr`          |
| Ukraynaca      | `uk`          |
| Vietnamca      | `vi`          |
| Galce | `cy` |

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Microsoft Translator Web sitesindeki listeye erişin

Dillere hızlı bir bakış için, Microsoft Translator Web sitesi, çevirmen ve konuşma API 'Leri tarafından desteklenen tüm dilleri gösterir. Bu liste, dil kodları gibi geliştiriciye özgü bilgiler içermez.

[Dillerin listesine bakın](https://www.microsoft.com/translator/languages.aspx)
