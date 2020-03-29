---
title: Dil desteği - Çevirmen Metin API
titleSuffix: Azure Cognitive Services
description: Çevirmen Metin API'si, Nöral Makine Çevirisi (NMT) kullanarak metin çevirisi için aşağıdaki dilleri destekler.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/10/2020
ms.author: swmachan
ms.openlocfilehash: a4f9833e8dd14dc7c8ec5849cb809bf2089a5dae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77206132"
---
# <a name="language-and-region-support-for-the-translator-text-api"></a>Çevirmen Metin API'si için dil ve bölge desteği

Çevirmen Metin API metin çevirisi için aşağıdaki dilleri destekler. Nöral Makine Çevirisi (NMT) yüksek kaliteli Yapay Bilgisayar destekli makine çevirileri için yeni bir standarttır ve bir sinir sistemi kullanılabilir olduğunda Çevirmen Metin API V3 kullanarak varsayılan olarak kullanılabilir.

[Makine çevirisinin nasıl çalıştığı hakkında daha fazla bilgi edinin](https://www.microsoft.com/translator/mt.aspx)

## <a name="translation"></a>Çeviri

**V2 Çevirmen API**

> [!NOTE]
> V2 30 Nisan 2018 tarihinde amortismana kaldırıldı. Yalnızca V3'te kullanılabilen yeni işlevlerden yararlanmak için lütfen uygulamalarınızı V3'e geçirin.

* Sadece istatistiksel: Bu dil için sinir sistemi bulunmamaktadır.
* Nöral kullanılabilir: Bir sinir sistemi mevcuttur. Sinir sistemine `category=generalnn` erişmek için parametreyi kullanın.
* Nöral varsayılan: Nöral varsayılan çeviri sistemidir. Microsoft Translator `category=smt` Hub ile kullanılmak üzere istatistik sistemine erişmek için parametreyi kullanın.
* Sadece nöral çeviri mevcuttur.

**V3 Çevirmen API** V3 Translator API varsayılan olarak nöraldir ve istatistiksel sistemler yalnızca sinirsel sistem olmadığında kullanılabilir.

> [!NOTE]
> Şu anda, nöral dillerin bir alt kümesi Özel Çevirmen mevcuttur ve biz yavaş yavaş ek olanları ekliyoruz. [Şu anda Özel Çevirmen'de kullanılabilen dilleri görüntüleyin.](#customization)

|Dil|  Dil kodu|  V3 API|
|:-----|:-----:|:-----|
|Afrikaner| `af`|   Sinir|
|Arapça|    `ar`    |   Sinir|
|Bangla|    `bn`    |   Sinir|
|Boşnakça (Latin)|   `bs`    |   Sinir|
|Bulgarca| `bg`    |   Sinir|
|Kanton (Geleneksel)|   `yue`|  Istatistiksel|
|Katalanca|   `ca`    |   Istatistiksel|
|Basitleştirilmiş Çince|    `zh-Hans`|Sinir|
|Geleneksel Çince|   `zh-Hant`       |Sinir|
|Hırvatça|  `hr`    |Sinir|
|Çekçe| `cs`    |   Sinir|
|Danca|    `da`        |Sinir|
|Felemenkçe| `nl`|   Sinir|
|Türkçe|   `en`    |   Sinir|
|Estonya Dili|  `et`    |   Sinir|
|Fiji|    `fj`    |   Istatistiksel|
|Filipino|  `fil`   |   Istatistiksel|
|Fince|   `fi`    |   Sinir|
|Fransızca|    `fr`    |   Sinir|
|Almanca|    `de`    |   Sinir|
|Yunanca| `el`    |   Sinir|
|Haiti Creole|    `ht`        |Istatistiksel|
|İbranice |`he`   |Sinir
|Hintçe| `hi`    |   Sinir|
|Hmong Daw| `mww`   |   Istatistiksel|
|Macarca| `hu`    |   Sinir|
|İzlandaca| `is`    |   Sinir|
|Endonezce|    `id`    |   Istatistiksel|
|İrlandaca | `ga`| Sinir
|İtalyanca|   `it`    |   Sinir|
|Japonca|  `ja`    |   Sinir|
|Kannada dili|`kn`| Sinir
|Kiswahili| `sw`    |   Istatistiksel|
|Klingon|   `tlh`   |   Istatistiksel|
|Klingon (plqaD)|   `tlh-Qaak`  |   Istatistiksel|
|Korece |`ko`   |   Sinir|
|Letonca|   `lv`    |   Sinir|
|Litvanca|    `lt`    |   Sinir|
|Malagasy|  `mg`    |   Istatistiksel|
|Malayca| `ms`        |Istatistiksel|
|Malayalam dili| `ml` | Sinir
|Maltaca|   `mt`    |   Istatistiksel|
|Maori dili| `mi`  | Sinir|
|Norveççe| `nb`    |   Sinir|
|Farsça|   `fa`    |   Sinir|
|Lehçe|    `pl`    |   Sinir|
|Portekizce (Brezilya)|   `pt-br` |   Sinir|
|Portekizce (Portekiz)| `pt-pt` | Sinir
|Pencap dili|`pa`|Sinir
|Queretaro Otomi|   `otq`   |   Istatistiksel|
|Rumence|  `ro`    |   Sinir|
|Rusça|   `ru`    |   Sinir|
|Samoaca|    `sm`    |   Istatistiksel|
|Sırpça (Kiril)|    `sr-Cyrl`|  Istatistiksel|
|Sırpça (Latin)|   `sr-Latn`       |Istatistiksel|
|Slovakça|    `sk`    |   Sinir|
|Slovence| `sl`    |   Sinir|
|İspanyolca|   `es`    |   Sinir|
|İsveççe|   `sv`    |Sinir|
|Tahiti|  `ty`    |Istatistiksel|
|Tamil dili| `ta`    |   Sinir|
|Telugu dili|    `te`    |   Sinir|
|Tayca|  `th`    |   Sinir|
|Tongan|    `to`    |   Istatistiksel|
|Türkçe|   `tr`        |Sinir|
|Ukraynaca| `uk`    |   Sinir|
|Urduca|  `ur`    |   Istatistiksel|
|Vietnamca|    `vi`    |   Sinir|
|Galce| `cy`    |   Sinir|
|Yucatec Maya|  `yua`   |   Istatistiksel|

> [!NOTE]
> Dil `pt` kodu varsayılan `pt-br`olarak , Portekizce (Brazil) olarak tanımlanır.

## <a name="transliteration"></a>Çevirisi

Transliterate yöntemi aşağıdaki dilleri destekler. "To/From"da "<->", dilin listelenen komut dosyalarından herhangi birinden veya herhangi birinden çevrilebileceğini belirtir. "-->" dilin yalnızca bir komut dosyasından diğerine çevrilebileceğini gösterir.

| Dil    | Dil kodu | Betik | Hedef/Kaynak | Betik|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Arapça | `ar` | Arapça`Arab` | <--> | Latin`Latn` |
|Bangla  | `bn` | Bengali`Beng` | <--> | Latin`Latn` |
| Çince (Basitleştirilmiş) | `zh-Hans` | Basitleştirilmiş Çince`Hans`| <--> | Latin`Latn` |
| Çince (Basitleştirilmiş) | `zh-Hans` | Basitleştirilmiş Çince`Hans`| <--> | Geleneksel Çince`Hant`|
| seçenekleri yerine | `zh-Hant` | Geleneksel Çince`Hant`| <--> | Latin`Latn` |
| seçenekleri yerine | `zh-Hant` | Geleneksel Çince`Hant`| <--> | Basitleştirilmiş Çince`Hans` |
| Gucerat dili | `gu`  | Gucerat dili`Gujr` | --> | Latin`Latn` |
| İbranice | `he` | İbranice`Hebr` | <--> | Latin`Latn` |
| Hintçe | `hi` | Devanagari`Deva` | <--> | Latin`Latn` |
| Japonca | `ja` | Japonca`Jpan` | <--> | Latin`Latn` |
| Kannada dili | `kn` | Kannada dili`Knda` | --> | Latin`Latn` |
| Malayalam dili | `ml` | Malayalam dili`Mlym` | --> | Latin`Latn` |
| Marathi | `mr` | Devanagari`Deva` | --> | Latin`Latn` |
| Oriya | `or` | Oriya`Orya` | <--> | Latin`Latn` |
| Pencap dili | `pa` | Gurmukhi`Guru`  | <--> | Latin`Latn`  |
| Sırpça (Kiril) | `sr-Cyrl` | Kiril`Cyrl`  | --> | Latin`Latn` |
| Sırpça (Latin) | `sr-Latn` | Latin`Latn` | --> | Kiril`Cyrl`|
| Tamil dili | `ta` | Tamil dili`Taml` | --> | Latin`Latn` |
| Telugu dili | `te` | Telugu dili`Telu` | --> | Latin`Latn` |
| Tayca | `th` | Tay dili`Thai` | --> | Latin`Latn` |

## <a name="dictionary"></a>Sözlük

Sözlük, Arama ve Örnekler yöntemlerini kullanarak aşağıdaki dilleri İngilizce'ye veya İngilizce'den destekler.

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
| Haiti Creole      | `ht`          |
| İbranice      | `he`          |
| Hintçe      | `hi`          |
| Hmong Daw      | `mww`          |
| Macarca      | `hu`          |
| İzlandaca    | `is`  |
| Endonezce      | `id`          |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
| Kiswahili      | `sw`          |
| Klingon      | `tlh`          |
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

Çevirmen Metin API' si çeviri ve çeviri için kullanılabilen tüm dilleri algılar.


## <a name="access-the-translator-text-api-language-list-programmatically"></a>Çevirmen Metin API dil listesine programlı olarak erişin

Diller yöntemini kullanarak Çevirmen Metin API v3.0 için desteklenen dillerin listesini alabilirsiniz. Listeyi özelliğe, dil koduna ve İngilizce'deki dil adına veya desteklenen diğer dillere göre görüntüleyebilirsiniz. Yeni diller kullanıma sunuldukça bu liste Microsoft Translator hizmeti tarafından otomatik olarak güncelleştirilir.

[Dilleri çalışma başvuru belgelerini görüntüle](reference/v3-0-languages.md)

## <a name="customization"></a>Özelleştirme

[Custom Translator](https://aka.ms/CustomTranslator)kullanarak İngilizce'ye veya İngilizce'den özelleştirme için aşağıdaki diller kullanılabilir.

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
| Estonya Dili      | `et`          |
| Fince      | `fi`          |
| Fransızca      | `fr`          |
| Almanca      | `de`          |
| Yunanca      | `el`          |
| İbranice      | `he`          |
| Hintçe      | `hi`          |
| Macarca      | `hu`          |
| İzlandaca | `is` |
| Endonezce|   `id`    |
| İrlandaca | `ga`  |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
| Kiswahili|    `sw`    |
| Korece      | `ko`          |
| Letonca      | `lv`          |
| Litvanca      | `lt`          |
| Malagasy| `mg`    |
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
| Tayca      | `th`          |
| Türkçe      | `tr`          |
| Ukraynaca      | `uk`          |
| Vietnamca      | `vi`          |
| Galce | `cy` |

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Microsoft Translator web sitesindeki listeye erişin

Dillere hızlı bir bakış için, Microsoft Translator web sitesi Çevirmen Metin ve Konuşma API'leri tarafından desteklenen tüm dilleri gösterir. Bu liste, dil kodları gibi geliştiriciye özgü bilgileri içermez.

[Diller listesine bakın](https://www.microsoft.com/translator/languages.aspx)
