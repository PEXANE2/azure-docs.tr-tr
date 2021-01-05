---
title: Dil desteği-Metin Analizi API'si
titleSuffix: Azure Cognitive Services
description: Metin Analizi API'si tarafından desteklenen doğal dillerin bir listesi. Bu makalede, her bir işlem için hangi dillerin desteklendiği açıklanmaktadır.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 12/17/2020
ms.author: aahi
ms.openlocfilehash: 180de56e3c158802460d2ff995041e8572d4dcd7
ms.sourcegitcommit: 5ef018fdadd854c8a3c360743245c44d306e470d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/01/2021
ms.locfileid: "97844959"
---
# <a name="text-analytics-api-v3-language-support"></a>Metin Analizi API'si v3 dil desteği 

#### <a name="sentiment-analysis"></a>[Yaklaşım Analizi](#tab/sentiment-analysis)

| Dil              | Dil kodu | v2 desteği | v3 desteği | V3 model sürümü başlatılıyor: |              Notlar |
|:----------------------|:-------------:|:----------:|:----------:|:--------------------------:|-------------------:|
| Basitleştirilmiş Çince    |   `zh-hans`   |     ✓      |     ✓      |         2019-10-01         | `zh` Ayrıca kabul edildi |
| Geleneksel Çince   |   `zh-hant`   |            |     ✓      |         2019-10-01         |                    |
| Danca               |     `da`      |     ✓      |            |                            |                    |
| Felemenkçe                 |     `nl`      |     ✓      |            |                            |                    |
| İngilizce               |     `en`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Fince               |     `fi`      |     ✓      |            |                            |                    |
| Fransızca                |     `fr`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Almanca                |     `de`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Yunanca                 |     `el`      |     ✓      |            |                            |                    |
| Hintçe                 |     `hi`      |            |      ✓     |          2020-04-01        |                    |
| İtalyanca               |     `it`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Japonca              |     `ja`      |     ✓      |     ✓      |         2019-10-01         |                    |
| Korece                |     `ko`      |            |     ✓      |         2019-10-01         |                    |
| Norveççe (Bokmål)   |     `no`      |     ✓      |     ✓      |         2020-07-01         |                    |
| Lehçe                |     `pl`      |     ✓      |            |                            |                    |
| Portekizce (Brezilya)   |    `pt-BR`    |            |     ✓      |         2020-04-01         |                    |
| Portekizce (Portekiz) |    `pt-PT`    |     ✓      |     ✓      |         2019-10-01         | `pt` Ayrıca kabul edildi |
| Rusça               |     `ru`      |     ✓      |            |                            |                    |
| İspanyolca               |     `es`      |     ✓      |     ✓      |         2019-10-01         |                    |
| İsveççe               |     `sv`      |     ✓      |            |                            |                    |
| Türkçe               |     `tr`      |     ✓      |     ✓       |         2020-07-01        |                    |

### <a name="opinion-mining-v31-preview-only"></a>Görüşme madenciliği (v 3.1-yalnızca Önizleme)

| Dil              | Dil kodu | V3 modeli sürümünden itibaren: |              Notlar |
|:----------------------|:-------------:|:------------------------------------:|-------------------:|
| İngilizce               |     `en`      |              2020-04-01              |                    |


#### <a name="named-entity-recognition-ner"></a>[Adlandırılmış varlık tanıma (NER)](#tab/named-entity-recognition)

> [!NOTE]
> * NER v3 Şu anda yalnızca Ingilizce ve Ispanyolca dilleri desteklemektedir. NER v3 'i farklı bir dille çağırırsanız, dil sürüm 2,1 ' de desteklendiğinden, API v 2.1 sonuçları döndürür.
> * v 2.1 Yalnızca Ingilizce, Çince-Basitleştirilmiş, Fransızca, Almanca ve Ispanyolca dilleri için kullanılabilir varlıkların tam kümesini döndürür.  Desteklenen diğer diller için "kişi", "konum" ve "kuruluş" varlıkları döndürülür.

| Dil               | Dil kodu | v 2.1 desteği | v3 desteği | V3 modeli sürümünden itibaren: |       Notlar        |
|:-----------------------|:-------------:|:----------:|:----------:|:-------------------------------:|:------------------:|
| Arapça                |     `ar`      |     ✓      |            |                                 |                    |
| Basitleştirilmiş Çince     |   `zh-hans`   |     ✓      |            |                                 | `zh` Ayrıca kabul edildi |
| Geleneksel Çince   |   `zh-hant`   |     ✓      |            |                                 |                    |
| Çekçe                 |     `cs`      |     ✓      |            |                                 |                    |
| Danca                |     `da`      |     ✓      |            |                                 |                    |
| Felemenkçe                 |     `nl`      |     ✓      |            |                                 |                    |
| İngilizce                |     `en`      |     ✓      |     ✓      |           2019-10-01            |                    |
| Fince               |     `fi`      |     ✓      |            |                                 |                    |
| Fransızca                 |     `fr`      |     ✓      |            |                                 |                    |
| Almanca                 |     `de`      |     ✓      |            |                                 |                    |
| İbranice                |     `he`      |     ✓      |            |                                 |                    |
| Macarca             |     `hu`      |     ✓      |            |                                 |                    |
| İtalyanca               |     `it`      |     ✓      |            |                                 |                    |
| Japonca              |     `ja`      |     ✓      |            |                                 |                    |
| Korece                |     `ko`      |     ✓      |            |                                 |                    |
| Norveççe (Bokmål)   |     `no`      |     ✓      |            |                                 | `nb` Ayrıca kabul edildi |
| Lehçe                |     `pl`      |     ✓      |            |                                 |                    |
| Portekizce (Brezilya)   |    `pt-BR`    |     ✓      |            |                                 |                    |
| Portekizce (Portekiz) |    `pt-PT`    |     ✓      |            |                                 | `pt` Ayrıca kabul edildi |
| Rusça              |     `ru`      |     ✓      |            |                                 |                    |
| İspanyolca               |     `es`      |     ✓      |     ✓       |              2020-04-01                   |                    |
| İsveççe               |     `sv`      |     ✓      |            |                                 |                    |
| Türkçe               |     `tr`      |     ✓      |            |                                 |                    |

#### <a name="key-phrase-extraction"></a>[Anahtar tümceciği ayıklama](#tab/key-phrase-extraction)

> [!NOTE]
> 2020-07-01 ' den önceki Anahtar İfade Ayıklama model sürümlerinin 64 karakter sınırı vardır. Bu sınır sonraki model sürümlerinde yok.

| Dil              | Dil kodu | v2 desteği | v3 desteği | V3 model sürümü ile başlayarak kullanılabilir: |       Notlar        |
|:----------------------|:-------------:|:----------:|:----------:|:-----------------------------------------:|:------------------:|
| Felemenkçe                 |     `nl`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| İngilizce               |     `en`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Fince               |     `fi`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Fransızca                |     `fr`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Almanca                |     `de`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| İtalyanca               |     `it`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Japonca              |     `ja`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Korece                |     `ko`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Norveççe (Bokmål)   |     `no`      |     ✓      |     ✓      |                2019-10-01                 | `nb` Ayrıca kabul edildi |
| Lehçe                |     `pl`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| Portekizce (Brezilya)   |    `pt-BR`    |     ✓      |     ✓      |                2019-10-01                 |                    |
| Portekizce (Portekiz) |    `pt-PT`    |     ✓      |     ✓      |                2019-10-01                 | `pt` Ayrıca kabul edildi |
| Rusça               |     `ru`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| İspanyolca               |     `es`      |     ✓      |     ✓      |                2019-10-01                 |                    |
| İsveççe               |     `sv`      |     ✓      |     ✓      |                2019-10-01                 |                    |

#### <a name="entity-linking"></a>[Varlık bağlama](#tab/entity-linking)

| Dil | Dil kodu | v2 desteği | v3 desteği | V3 model sürümü ile başlayarak kullanılabilir: | Notlar |
|:---------|:-------------:|:----------:|:----------:|:-----------------------------------------:|:-----:|
| İngilizce  |     `en`      |     ✓      |     ✓      |                2019-10-01                 |       |
| İspanyolca  |     `es`      |     ✓      |     ✓      |                2019-10-01                 |       |

#### <a name="language-detection"></a>[Dil Algılama](#tab/language-detection)

Metin Analizi API'si, çok çeşitli diller, çeşitler, diapacts ve bazı bölgesel/kültürel dillerini algılayabilir ve algılanan dilleri ad ve kodla döndürebilir. Metin Analizi Dil Algılama dil kodu parametreleri [ISO-639-1](https://www.iso.org/iso-639-language-codes.html) tanımlayıcılarıyla uyumlu olan [bcp-47](https://tools.ietf.org/html/bcp47) standardına uygundur. 

Daha az sıklıkta kullanılan bir dilde ifade ettiğiniz bir içeriğiniz varsa, bir kodu döndürüp döndürdüğünü görmek için Dil Algılama deneyebilirsiniz. Tespit edilemez dillerin yanıtı `unknown` .

| Dil | Dil Kodu | v3 desteği | V3 model sürümü ile başlayarak kullanılabilir: |
|:-|:-:|:-:|:-:|
| Afrikaner | `af` | ✓ |  |
| Arnavutça | `sq` | ✓ |  |
| Arapça | `ar` | ✓ |  |
| Ermenice | `hy` | ✓ |  |
| Baskça | `eu` | ✓ |  |
| Beyaz Rusça | `be` | ✓ |  |
| Bengali | `bn` | ✓ |  |
| Boşnakça | `bs` | ✓ | 2020-09-01 |
| Bulgarca | `bg` | ✓ |  |
| Birman dili | `my` | ✓ |  |
| Katalanca, Alcian | `ca` | ✓ |  |
| Orta Khmer Dili | `km` | ✓ |  |
| Çince | `zh` | ✓ |  |
| Basitleştirilmiş Çince | `zh_chs` | ✓ |  |
| Geleneksel Çince | `zh_cht` | ✓ |  |
| Hırvatça | `hr` | ✓ |  |
| Çekçe | `cs` | ✓ |  |
| Danca | `da` | ✓ |  |
| Dili | `prs` | ✓ | 2020-09-01 |
| Divehi, Dhivehi, Maldivian | `dv` | ✓ |  |
| Felemenkçe, Flemish | `nl` | ✓ |  |
| İngilizce | `en` | ✓ |  |
| Esperanto | `eo` | ✓ |  |
| Estonya Dili | `et` | ✓ |  |
| Fiji Adaları dili | `fj` | ✓ | 2020-09-01 |
| Fince | `fi` | ✓ |  |
| Fransızca | `fr` | ✓ |  |
| Galiçya Dili | `gl` | ✓ |  |
| Gürcüce | `ka` | ✓ |  |
| Almanca | `de` | ✓ |  |
| Yunanca | `el` | ✓ |  |
| Gucerat dili | `gu` | ✓ |  |
| Haitia, Haian Creole | `ht` | ✓ |  |
| İbranice | `he` | ✓ |  |
| Hintçe | `hi` | ✓ |  |
| Hmong DAW | `mww` | ✓ | 2020-09-01 |
| Macarca | `hu` | ✓ |  |
| İzlandaca | `is` | ✓ |  |
| Endonezce | `id` | ✓ |  |
| Dili | `iu` | ✓ |  |
| İrlandaca | `ga` | ✓ |  |
| İtalyanca | `it` | ✓ |  |
| Japonca | `ja` | ✓ |  |
| Kannada dili | `kn` | ✓ |  |
| Kazakça | `kk` | ✓ | 2020-09-01 |
| Korece | `ko` | ✓ |  |
| Kürtçe | `ku` | ✓ |  |
| Laos | `lo` | ✓ |  |
| Tin | `la` | ✓ |  |
| Letonca | `lv` | ✓ |  |
| Litvanca | `lt` | ✓ |  |
| CA | `mk` | ✓ |  |
| Malgaşça | `mg` | ✓ | 2020-09-01 |
| Malayca | `ms` | ✓ |  |
| Malayalam dili | `ml` | ✓ |  |
| Maltaca | `mt` | ✓ |  |
| Maori dili | `mi` | ✓ | 2020-09-01 |
| Marathi | `mr` | ✓ | 2020-09-01 |
| Norveççe | `no` | ✓ |  |
| Norveççe Nynorsk | `nn` | ✓ |  |
| Oriya | `or` | ✓ |  |
| Pashto, Pushto | `ps` | ✓ |  |
| Farsça | `fa` | ✓ |  |
| Lehçe | `pl` | ✓ |  |
| Portekizce | `pt` | ✓ |  |
| Pencap dili, Panjabi | `pa` | ✓ |  |
| Queretaro Otomi | `otq` | ✓ | 2020-09-01 |
| Rumence, Moldavian, Moldovan | `ro` | ✓ |  |
| Rusça | `ru` | ✓ |  |
| Samoan | `sm` | ✓ | 2020-09-01 |
| Sırpça | `sr` | ✓ |  |
| Sinhali dili, Sinhalet | `si` | ✓ |  |
| Slovakça | `sk` | ✓ |  |
| Slovence | `sl` | ✓ |  |
| Dili | `so` | ✓ |  |
| İspanyolca, CAStilian | `es` | ✓ |  |
| Svahili dili | `sw` | ✓ |  |
| İsveççe | `sv` | ✓ |  |
| Tagalog | `tl` | ✓ |  |
| Tahiti dili | `ty` | ✓ | 2020-09-01 |
| Tamil dili | `ta` | ✓ |  |
| Telugu dili | `te` | ✓ |  |
| Tayca | `th` | ✓ |  |
| Tonga dili | `to` | ✓ | 2020-09-01 |
| Türkçe | `tr` | ✓ |  |
| Ukraynaca | `uk` | ✓ |  |
| Urduca | `ur` | ✓ |  |
| Özbekçe | `uz` | ✓ |  |
| Vietnamca | `vi` | ✓ |  |
| Galce | `cy` | ✓ |  |
| Yidiş | `yi` | ✓ |  |
| Yucatec Maya | `yua` | ✓ |  |

---

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizi API'si nedir?](overview.md)   
