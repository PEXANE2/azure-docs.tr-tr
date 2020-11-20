---
title: Dil desteği-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti, konuşmadan metne ve metinden konuşmaya dönüştürmeye, konuşma çevirisi ile birlikte çok sayıda dili destekler. Bu makale, hizmet özelliğine göre dil desteğinin kapsamlı bir listesini sağlar.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: trbye
ms.custom: references_regions
ms.openlocfilehash: a430333f57b3734e5977db5c104c8f6d37bfd3c4
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94949097"
---
# <a name="language-and-voice-support-for-the-speech-service"></a>Konuşma hizmeti için dil ve ses desteği

Dil desteği, konuşma hizmeti işlevselliğine göre farklılık gösterir. Aşağıdaki tablolar, [konuşmadan metne](#speech-to-text), [metinden konuşmaya](#text-to-speech)ve [konuşma çevirisi](#speech-translation) hizmet teklifleri için dil desteğini özetler.

## <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

Hem Microsoft konuşma SDK 'Sı hem de REST API aşağıdaki dilleri (yerel ayarlar) destekler. 

Doğruluğu artırmak için, bir dilin bir alt kümesi için, **Ses + insan etiketli yazılı** döküm veya **Ilgili metin: cümleler** karşıya yüklenirken özelleştirme sunulur. Özelleştirme hakkında daha fazla bilgi için bkz. [özel konuşma tanıma kullanmaya başlama](how-to-custom-speech.md).

<!--
To get the AM and ML bits:
https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.ai -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

| Dil                          | Yerel ayar (BCP-47) | Özelleştirmeler                                   |
|-----------------------------------|--------|--------------------------------------------------|
|Arapça (Bahreyn), modern standart  |`ar-BH` | Dil modeli                                   |
|Arapça (Mısır)                     |`ar-EG` | Dil modeli                                   |
|Arapça (Irak)                      |`ar-IQ` | Dil modeli                                   |
|Arapça (Ürdün)                    |`ar-JO` | Dil modeli                                   |
|Arapça (Kuveyt)                    |`ar-KW` | Dil modeli                                   |
|Arapça (Lübnan)                   |`ar-LB` | Dil modeli                                   |
|Arapça (Umman)                      |`ar-OM` | Dil modeli                                   |
|Arapça (Qtor)                     |`ar-QA` | Dil modeli                                   |
|Arapça (Suudi Arabistan)              |`ar-SA` | Dil modeli                                   |
|Arapça (Suriye)                     |`ar-SY` | Dil modeli                                   |
|Arapça (Birleşik Arap Emirlikleri)      |`ar-AE` | Dil modeli                                   |
|Bulgarca (Bulgaristan)               |`bg-BG` | Dil modeli                                   |
|Katalanca (İspanya)                    |`ca-ES` | Dil modeli                                   |
|Çince (Cantoneo, geleneksel)   |`zh-HK` | Dil modeli                                   |
|Çince (Mandarin, Basitleştirilmiş)     |`zh-CN` | Akustik model<br>Dil modeli                 |
|Çince (Taiwanese Mandarin)       |`zh-TW` | Dil modeli                                   |
|Hırvatça (Hırvatistan)                 |`hr-HR` | Dil modeli                                   |
|Çekçe (Çek Cumhuriyeti)             |`cs-CZ` | Dil modeli                                   |
|Danca (Danimarka)                   |`da-DK` | Dil modeli                                   |
|Felemenkçe (Hollanda)                |`nl-NL` | Dil modeli                                   |
|İngilizce (Avustralya)                |`en-AU` | Akustik model<br>Dil modeli                 |
|İngilizce (Kanada)                   |`en-CA` | Akustik model<br>Dil modeli                 |
|İngilizce (Hong Kong)                |`en-HK` | Dil modeli                                   |
|İngilizce (Hindistan)                    |`en-IN` | Akustik model<br>Dil modeli                 |
|İngilizce (İrlanda)                  |`en-IE` | Dil modeli                                   |
|İngilizce (Yeni Zelanda)              |`en-NZ` | Akustik model<br>Dil modeli                 |
|İngilizce (Filipinler)              |`en-PH` | Dil modeli                                   |
|İngilizce (Singapur)                |`en-SG` | Dil modeli                                   |
|İngilizce (Güney Afrika)             |`en-ZA` | Dil modeli                                   |
|İngilizce (İngiltere)           |`en-GB` | Akustik model<br>Dil modeli<br>İşi|
|İngilizce (ABD)            |`en-US` | Akustik model<br>Dil modeli<br>İşi|
|Estonya dili (Estonya)                  |`et-EE` | Dil modeli                                   |
|Fince (Finlandiya)                  |`fi-FI` | Dil modeli                                   |
|Fransızca (Kanada)                    |`fr-CA` | Akustik model<br>Dil modeli                 |
|Fransızca (Fransa)                    |`fr-FR` | Akustik model<br>Dil modeli<br>İşi|
|Almanca (Almanya)                   |`de-DE` | Akustik model<br>Dil modeli<br>İşi|
|Yunanca (Yunanistan)                     |`el-GR` | Dil modeli                                   |
|Gucerat dili (Hindistan)                  |`gu-IN` | Dil modeli                                   |
|Hintçe (Hindistan)                      |`hi-IN` | Akustik model<br>Dil modeli                 |
|Macarca (Macaristan)                |`hu-HU` | Dil modeli                                   |
|İrlanda dili (Irlanda)                     |`ga-IE` | Dil modeli                                   |
|İtalyanca (İtalya)                    |`it-IT` | Akustik model<br>Dil modeli<br>İşi|
|Japonca (Japonya)                   |`ja-JP` | Dil modeli                                   |
|Korece (Kore)                     |`ko-KR` | Dil modeli                                   |
|Letonca (Letonya)                   |`lv-LV` | Dil modeli                                   |
|Litvanca (Litvanya)             |`lt-LT` | Dil modeli                                   |
|Malta dili (Malta)                     |`mt-MT` | Dil modeli                                   |
|Marathi dili (Hindistan)                    |`mr-IN` | Dil modeli                                   |
|Norveççe (Bokmål, Norveç)         |`nb-NO` | Dil modeli                                   |
|Lehçe (Polonya)                    |`pl-PL` | Dil modeli                                   |
|Portekizce (Brezilya)                |`pt-BR` | Akustik model<br>Dil modeli<br>İşi|
|Portekizce (Portekiz)              |`pt-PT` | Dil modeli                                   |
|Rumence (Romanya)                 |`ro-RO` | Dil modeli                                   |
|Rusça (Rusya)                   |`ru-RU` | Akustik model<br>Dil modeli                 |
|Slovakça (Slovakya)                  |`sk-SK` | Dil modeli                                   |
|Slovence (Slovenya)               |`sl-SI` | Dil modeli                                   |
|İspanyolca (Arjantin)                |`es-AR` | Dil modeli                                   |
|İspanyolca (Bolivya)                  |`es-BO` | Dil modeli                                   |
|İspanyolca (Şili)                    |`es-CL` | Dil modeli                                   |
|İspanyolca (Kolombiya)                 |`es-CO` | Dil modeli                                   |
|İspanyolca (Kosta Rika)               |`es-CR` | Dil modeli                                   |
|İspanyolca (Küa)                     |`es-CU` | Dil modeli                                   |
|İspanyolca (Dominik Cumhuriyeti)       |`es-DO` | Dil modeli                                   |
|İspanyolca (Ekvador)                  |`es-EC` | Dil modeli                                   |
|İspanyolca (El Salvador)              |`es-SV` | Dil modeli                                   |
|İspanyolca (Guatemala)                |`es-GT` | Dil modeli                                   |
|İspanyolca (Honduras)                 |`es-HN` | Dil modeli                                   |
|İspanyolca (Meksika)                   |`es-MX` | Akustik model<br>Dil modeli                 |
|İspanyolca (Nikaragua)                |`es-NI` | Dil modeli                                   |
|İspanyolca (Panama)                   |`es-PA` | Dil modeli                                   |
|İspanyolca (Paraguay)                 |`es-PY` | Dil modeli                                   |
|İspanyolca (Peru)                     |`es-PE` | Dil modeli                                   |
|İspanyolca (Porto Riko)              |`es-PR` | Dil modeli                                   |
|İspanyolca (İspanya)                    |`es-ES` | Akustik model<br>Dil modeli                 |
|İspanyolca (Uruguay)                  |`es-UY` | Dil modeli                                   |
|İspanyolca (ABD)                      |`es-US` | Dil modeli                                   |
|İspanyolca (Venezuela)                |`es-VE` | Dil modeli                                   |
|İsveççe (İsviçre)                   |`sv-SE` | Dil modeli                                   |
|Tamil dili (Hindistan)                      |`ta-IN` | Dil modeli                                   |
|Telugu dili (Hindistan)                     |`te-IN` | Dil modeli                                   |
|Tayca (Tayland)                    |`th-TH` | Dil modeli                                   |
|Türkçe (Türkiye)                   |`tr-TR` | Dil modeli                                   |

## <a name="text-to-speech"></a>Metin okuma

Hem Microsoft konuşma SDK 'Sı hem de REST API 'Leri, her biri yerel ayar ile tanımlanan belirli bir dili ve diyalekt destekleyen bu sesleri destekler. Ayrıca, [sesler/liste API 'si](rest-text-to-speech.md#get-a-list-of-voices)aracılığıyla her belirli bölge/uç nokta için desteklenen dillerin ve seslerin tam listesini de alabilirsiniz. 

> [!IMPORTANT]
> Fiyatlandırma, standart, özel ve sinir sesleri için farklılık gösterir. Daha fazla bilgi için lütfen [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) sayfasını ziyaret edin.

### <a name="neural-voices"></a>Sinir sesleri

Sinir metin okuma, derin sinir ağlarla desteklenen yeni bir konuşma türü türüdür. Bir sinir sesi kullanırken, sentezlenmiş konuşma, insan kayıtlarından neredeyse ayırt edilemez.

Sinir sesleri, chatbots ve ses yardımcılarıyla daha doğal ve etkileyici bir şekilde etkileşim kurmak, e-kitaplar gibi dijital metinleri audiobooks 'a dönüştürmek ve oto içi gezinti sistemlerini geliştirmek için kullanılabilir. İnsan benzeri doğal Prosody ve sözcüklerin bir kısmını temizleyerek, sinir seslerde kullanıcılar AI sistemleriyle etkileşim kurarken büyük ölçüde dinleme performansını önemli ölçüde azaltır.

| Dil | Yerel Ayar | Cinsiyet | Ses adı | Stil desteği |
|---|---|---|---|---|
| Arapça (Mısır) | `ar-EG` | Kadın | `ar-EG-SalmaNeural` | Genel |
| Arapça (Suudi Arabistan) | `ar-SA` | Kadın | `ar-SA-ZariyahNeural` | Genel |
| Bulgarca (Bulgary) | `bg-BG` | Kadın | `bg-BG-KalinaNeural` <sup>Yeni</sup> | Genel |
| Katalanca (İspanya) | `ca-ES` | Kadın | `ca-ES-AlbaNeural` | Genel |
| Çince (Cantoneo, geleneksel) | `zh-HK` | Kadın | `zh-HK-HiuGaaiNeural` | Genel |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-XiaoxiaoNeural` | Genel, [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) birden çok ses stili mevcuttur |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-XiaoyouNeural` | Çocuk sesi, öykü anlatımı için iyileştirilmiş |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Erkek   | `zh-CN-YunyangNeural` | Haberleri okuma için iyileştirilmiş,<br /> [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) birden çok ses stili kullanılabilir |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Erkek   | `zh-CN-YunyeNeural` | Öykü anlatımı için iyileştirildi |
| Çince (Taiwanese Mandarin) | `zh-TW` | Kadın | `zh-TW-HsiaoYuNeural` | Genel |
| Hırvatça (Hırvatistan) | `hr-HR` | Kadın | `hr-HR-GabrijelaNeural` <sup>Yeni</sup> | Genel |
| Çekçe (Çek) | `cs-CZ` | Kadın | `cs-CZ-VlastaNeural` <sup>Yeni</sup>    | Genel |
| Danca (Danimarka) | `da-DK` | Kadın | `da-DK-ChristelNeural` | Genel |
| Felemenkçe (Hollanda) | `nl-NL` | Kadın | `nl-NL-ColetteNeural` | Genel |
| İngilizce (Avustralya) | `en-AU` | Kadın | `en-AU-NatashaNeural` | Genel |
| İngilizce (Avustralya) | `en-AU` | Erkek   | `en-AU-WilliamNeural` <sup>Yeni</sup>  | Genel |
| İngilizce (Kanada) | `en-CA` | Kadın | `en-CA-ClaraNeural` | Genel |
| İngilizce (Hindistan) | `en-IN` | Kadın | `en-IN-NeerjaNeural` | Genel |
| İngilizce (İrlanda) | `en-IE` | Kadın | `en-IE-EmilyNeural` <sup>Yeni</sup> | Genel |
| İngilizce (İngiltere) | `en-GB` | Kadın | `en-GB-LibbyNeural` | Genel |
| İngilizce (İngiltere) | `en-GB` | Kadın | `en-GB-MiaNeural` | Genel |
| İngilizce (İngiltere) | `en-GB` | Erkek | `en-GB-RyanNeural` <sup>Yeni</sup> | Genel |
| İngilizce (ABD) | `en-US` | Kadın | `en-US-AriaNeural` | Genel, [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) birden çok ses stili mevcuttur |
| İngilizce (ABD) | `en-US` | Erkek | `en-US-GuyNeural` | Genel |
| İngilizce (ABD) | `en-US` | Kadın | `en-US-JennyNeural` <sup>Yeni</sup> | Genel, [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) birden çok ses stili mevcuttur |
| Fince (Finlandiya) | `fi-FI` | Kadın | `fi-FI-NooraNeural` | Genel |
| Fransızca (Kanada) | `fr-CA` | Kadın | `fr-CA-SylvieNeural` | Genel |
| Fransızca (Kanada) | `fr-CA` | Erkek | `fr-CA-JeanNeural` <sup>Yeni</sup> | Genel |
| Fransızca (Fransa) | `fr-FR` | Kadın | `fr-FR-DeniseNeural` | Genel |
| Fransızca (Fransa) | `fr-FR` | Erkek | `fr-FR-HenriNeural` <sup>Yeni</sup> | Genel |
| Fransızca (İsviçre) | `fr-CH` | Kadın | `fr-CH-ArianeNeural` <sup>Yeni</sup> | Genel |
| Almanca (Avusturya) | `de-AT` | Kadın | `de-AT-IngridNeural` <sup>Yeni</sup> | Genel |
| Almanca (Almanya) | `de-DE` | Kadın | `de-DE-KatjaNeural` | Genel |
| Almanca (Almanya) | `de-DE` | Erkek | `de-DE-ConradNeural` <sup>Yeni</sup> | Genel |
| Almanca (İsviçre) | `de-CH` | Kadın | `de-CH-LeniNeural` <sup>Yeni</sup> | Genel |
| Yunanca (Yunanistan) | `el-GR` | Kadın | `el-GR-AthinaNeural` <sup>Yeni</sup> | Genel |
| İbranice (Israil) | `he-IL` | Kadın | `he-IL-HilaNeural` <sup>Yeni</sup> | Genel |
| Hintçe (Hindistan) | `hi-IN` | Kadın | `hi-IN-SwaraNeural` | Genel |
| Macarca (Macaristan) | `hu-HU` | Kadın | `hu-HU-NoemiNeural` <sup>Yeni</sup> | Genel |
| Endonezce (Endonezya) | `id-ID` | Erkek | `id-ID-ArdiNeural` <sup>Yeni</sup> | Genel |
| İtalyanca (İtalya) | `it-IT` | Kadın | `it-IT-ElsaNeural` | Genel |
| İtalyanca (İtalya) | `it-IT` | Kadın | `it-IT-IsabellaNeural` <sup>Yeni</sup> | Genel |
| İtalyanca (İtalya) | `it-IT` | Erkek | `it-IT-DiegoNeural` <sup>Yeni</sup> | Genel |
| Japonca (Japonya) | `ja-JP` | Kadın | `ja-JP-NanamiNeural` | Genel |
| Japonca (Japonya) | `ja-JP` | Erkek | `ja-JP-KeitaNeural` <sup>Yeni</sup> | Genel |
| Korece (Kore) | `ko-KR` | Kadın | `ko-KR-SunHiNeural` | Genel |
| Korece (Kore) | `ko-KR` | Erkek | `ko-KR-InJoonNeural` <sup>Yeni</sup> | Genel |
| Malayca (Malezya) | `ms-MY` | Kadın | `ms-MY-YasminNeural` <sup>Yeni</sup> | Genel |
| Norveççe (Bokmål, Norveç) | `nb-NO` | Kadın | `nb-NO-IselinNeural` | Genel |
| Lehçe (Polonya) | `pl-PL` | Kadın | `pl-PL-ZofiaNeural` | Genel |
| Portekizce (Brezilya) | `pt-BR` | Kadın | `pt-BR-FranciscaNeural` | Genel, [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) birden çok ses stili mevcuttur |
| Portekizce (Brezilya) | `pt-BR` | Erkek | `pt-BR-AntonioNeural` <sup>Yeni</sup> | Genel |
| Portekizce (Portekiz) | `pt-PT` | Kadın | `pt-PT-FernandaNeural` | Genel |
| Rumence (Romanya) | `ro-RO` | Kadın | `ro-RO-AlinaNeural` <sup>Yeni</sup> | Genel |
| Rusça (Rusya) | `ru-RU` | Kadın | `ru-RU-DariyaNeural` | Genel |
| Slovakça (Slovakya) | `sk-SK` | Kadın | `sk-SK-ViktoriaNeural` <sup>Yeni</sup> | Genel |
| Slovence (Slovenya) | `sl-SI` | Kadın | `sl-SI-PetraNeural` <sup>Yeni</sup> | Genel |
| İspanyolca (Meksika) | `es-MX` | Kadın | `es-MX-DaliaNeural` | Genel |
| İspanyolca (Meksika) | `es-MX` | Erkek | `es-MX-JorgeNeural` <sup>Yeni</sup> | Genel |
| İspanyolca (İspanya) | `es-ES` | Kadın | `es-ES-ElviraNeural` | Genel |
| İspanyolca (İspanya) | `es-ES` | Erkek | `es-ES-AlvaroNeural` <sup>Yeni</sup> | Genel |
| İsveççe (İsviçre) | `sv-SE` | Kadın | `sv-SE-HilleviNeural` | Genel |
| Tamil dili (Hindistan) | `ta-IN` | Kadın | `ta-IN-PallaviNeural` <sup>Yeni</sup> | Genel |
| Telugu dili (Hindistan) | `te-IN` | Kadın | `te-IN-ShrutiNeural` <sup>Yeni</sup> | Genel |
| Tayca (Tayland) | `th-TH` | Kadın | `th-TH-AcharaNeural` | Genel |
| Tayca (Tayland) | `th-TH` | Kadın | `th-TH-PremwadeeNeural` <sup>Yeni</sup> | Genel |
| Türkçe (Türkiye) | `tr-TR` | Kadın | `tr-TR-EmelNeural` | Genel |
| Vietnamca (Vietnam) | `vi-VN` <sup>Yeni</sup> | Kadın | `vi-VN-HoaiMyNeural` | Genel|

#### <a name="neural-voices-in-preview"></a>Önizlemede sinir sesler

Aşağıdaki sinir sesleri genel önizlemede. 

| Dil                         | Yerel Ayar  | Cinsiyet | Ses adı                             | Stil desteği |
|----------------------------------|---------|--------|----------------------------------------|---------------|
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Erkek   | `zh-CN-YunxiNeural` <sup>Yeni</sup> | [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) genel, birden çok stil kullanılabilir |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-XiaohanNeural` <sup>Yeni</sup> | [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) genel, birden çok stil kullanılabilir |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-XiaoxuanNeural` <sup>Yeni</sup> | [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) genel, birden çok rol yürütme ve stil kullanılabilir |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-XiaomoNeural` <sup>Yeni</sup> | [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) genel, birden çok rol yürütme ve stil kullanılabilir |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-XiaoruiNeural` <sup>Yeni</sup> | [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) üst düzey ses, birden çok stil kullanılabilir |
| Estonca (Estonya) | `et-EE` | Kadın | `et-EE-AnuNeural` <sup>Yeni</sup> | Genel |
| Gaelce (Irlanda) | `ga-IE` | Kadın | `ga-IE-OrlaNeural` <sup>Yeni</sup> | Genel |
| Litvanca (Litvanya) | `lt-LT` | Kadın | `lt-LT-OnaNeural` <sup>Yeni</sup> | Genel |
| Letonca (Letonya) | `lv-LV` | Kadın | `lv-LV-EveritaNeural` <sup>Yeni</sup> | Genel |
| Malta dili (Malta) | `mt-MT` | Kadın | `mt-MT-GraceNeural` <sup>Yeni</sup> | Genel |

> [!IMPORTANT]
> Genel önizlemede bulunan sesler yalnızca 3 hizmet bölgesinde kullanılabilir: Doğu ABD, Batı Avrupa ve Güneydoğu Asya.

Bölgesel kullanılabilirlik hakkında daha fazla bilgi için bkz. [bölgeler](regions.md#standard-and-neural-voices).

Konuşma stilleri gibi sinir seslerini nasıl yapılandıracağınızı ve ayarlayabileceğinizi öğrenmek için bkz. [konuşma SensMe biçimlendirme dili](speech-synthesis-markup.md#adjust-speaking-styles).

> [!IMPORTANT]
> `en-US-JessaNeural`Ses olarak değiştirildi `en-US-AriaNeural` . Daha önce "Jessa" kullandıysanız, "Aria" öğesine dönüştürün.

> [!TIP]
> Konuşma sensimi isteklerinizin "Microsoft Server konuşma Metin Okuma sesi (en-US, AriaNeural)" gibi tam hizmet adı eşlemesini kullanmaya devam edebilirsiniz.

### <a name="standard-voices"></a>Standart sesler

75 ' den fazla standart ses, daha fazla 45 dilde ve yerel ayarlarda bulunabilir ve bu da metni sentezleştirilmiş konuşmaya dönüştürmenize olanak tanır. Bölgesel kullanılabilirlik hakkında daha fazla bilgi için bkz. [bölgeler](regions.md#standard-and-neural-voices).

| Dil | Yerel ayar (BCP-47) | Cinsiyet | Ses adı |
|--|--|--|--|
| Arapça (Arapça) | `ar-EG` | Kadın | `ar-EG-Hoda`|
| Arapça (Suudi Arabistan) | `ar-SA` | Erkek | `ar-SA-Naayf`|
| Bulgarca (Bulgaristan) | `bg-BG` | Erkek | `bg-BG-Ivan`|
| Katalanca (İspanya) | `ca-ES` | Kadın | `ca-ES-HerenaRUS`|
| Çince (Cantoneo, geleneksel) | `zh-HK` | Erkek | `zh-HK-Danny`|
| Çince (Cantoneo, geleneksel) | `zh-HK` | Kadın | `zh-HK-TracyRUS`|
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-HuihuiRUS`|
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Erkek | `zh-CN-Kangkang`|
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-Yaoyao`|
| Çince (Taiwanese Mandarin) |  `zh-TW` | Kadın | `zh-TW-HanHanRUS`|
| Çince (Taiwanese Mandarin) |  `zh-TW` | Kadın | `zh-TW-Yating`|
| Çince (Taiwanese Mandarin) |  `zh-TW` | Erkek | `zh-TW-Zhiwei`|
| Hırvatça (Hırvatistan) | `hr-HR` | Erkek | `hr-HR-Matej`|
| Çekçe (Çek Cumhuriyeti) | `cs-CZ` | Erkek | `cs-CZ-Jakub`|
| Danca (Danimarka) | `da-DK` | Kadın | `da-DK-HelleRUS`|
| Felemenkçe (Hollanda) | `nl-NL` | Kadın | `nl-NL-HannaRUS`|
| İngilizce (Avustralya) | `en-AU` | Kadın | `en-AU-Catherine`|
| İngilizce (Avustralya) | `en-AU` | Kadın | `en-AU-HayleyRUS`|
| İngilizce (Kanada) | `en-CA` | Kadın | `en-CA-HeatherRUS`|
| İngilizce (Kanada) | `en-CA` | Kadın | `en-CA-Linda`|
| İngilizce (Hindistan) | `en-IN` | Kadın | `en-IN-Heera`|
| İngilizce (Hindistan) | `en-IN` | Kadın | `en-IN-PriyaRUS`|
| İngilizce (Hindistan) | `en-IN` | Erkek | `en-IN-Ravi`|
| İngilizce (İrlanda) | `en-IE` | Erkek | `en-IE-Sean`|
| İngilizce (İngiltere) | `en-GB` | Erkek | `en-GB-George`|
| İngilizce (İngiltere) | `en-GB` | Kadın | `en-GB-HazelRUS`|
| İngilizce (İngiltere) | `en-GB` | Kadın | `en-GB-Susan`|
| İngilizce (ABD) | `en-US` | Erkek | `en-US-BenjaminRUS`|
| İngilizce (ABD) | `en-US` | Erkek | `en-US-GuyRUS`|
| İngilizce (ABD) | `en-US` | Kadın | `en-US-JessaRUS`|
| İngilizce (ABD) | `en-US` | Kadın | `en-US-ZiraRUS`|
| Fince (Finlandiya) | `fi-FI` | Kadın | `fi-FI-HeidiRUS`|
| Fransızca (Kanada) | `fr-CA` | Kadın | `fr-CA-Caroline`|
| Fransızca (Kanada) | `fr-CA` | Kadın | `fr-CA-HarmonieRUS`|
| Fransızca (Fransa) | `fr-FR` | Kadın | `fr-FR-HortenseRUS`|
| Fransızca (Fransa) | `fr-FR` | Kadın | `fr-FR-Julie`|
| Fransızca (Fransa) | `fr-FR` | Erkek | `fr-FR-Paul`|
| Fransızca (İsviçre) | `fr-CH` | Erkek | `fr-CH-Guillaume`|
| Almanca (Avusturya) | `de-AT` | Erkek | `de-AT-Michael`|
| Almanca (Almanya) | `de-DE` | Kadın | `de-DE-HeddaRUS`|
| Almanca (Almanya) | `de-DE` | Erkek | `de-DE-Stefan`|
| Almanca (İsviçre) | `de-CH` | Erkek | `de-CH-Karsten`|
| Yunanca (Yunanistan) | `el-GR` | Erkek | `el-GR-Stefanos`|
| İbranice (Israil) | `he-IL` | Erkek | `he-IL-Asaf`|
| Hintçe (Hindistan) | `hi-IN` | Erkek | `hi-IN-Hemant`|
| Hintçe (Hindistan) | `hi-IN` | Kadın | `hi-IN-Kalpana`|
| Macarca (Macaristan) | `hu-HU` | Erkek | `hu-HU-Szabolcs`|
| Endonezce (Endonezya) | `id-ID` | Erkek | `id-ID-Andika`|
| İtalyanca (İtalya) | `it-IT` | Erkek | `it-IT-Cosimo`|
| İtalyanca (İtalya) | `it-IT` | Kadın | `it-IT-LuciaRUS`|
| Japonca (Japonya) | `ja-JP` | Kadın | `ja-JP-Ayumi`|
| Japonca (Japonya) | `ja-JP` | Kadın | `ja-JP-HarukaRUS`|
| Japonca (Japonya) | `ja-JP` | Erkek | `ja-JP-Ichiro`|
| Korece (Kore) | `ko-KR` | Kadın | `ko-KR-HeamiRUS`|
| Malayca (Malezya) | `ms-MY` | Erkek | `ms-MY-Rizwan`|
| Norveççe (Bokmål, Norveç) | `nb-NO` | Kadın | `nb-NO-HuldaRUS`|
| Lehçe (Polonya) | `pl-PL` | Kadın | `pl-PL-PaulinaRUS`|
| Portekizce (Brezilya) | `pt-BR` | Erkek | `pt-BR-Daniel`|
| Portekizce (Brezilya) | `pt-BR` | Kadın | `pt-BR-HeloisaRUS`|
| Portekizce (Portekiz) | `pt-PT` | Kadın | `pt-PT-HeliaRUS`|
| Rumence (Romanya) | `ro-RO` | Erkek | `ro-RO-Andrei`|
| Rusça (Rusya) | `ru-RU` | Kadın | `ru-RU-EkaterinaRUS`|
| Rusça (Rusya) | `ru-RU` | Kadın | `ru-RU-Irina`|
| Rusça (Rusya) | `ru-RU` | Erkek | `ru-RU-Pavel`|
| Slovakça (Slovakya) | `sk-SK` | Erkek | `sk-SK-Filip`|
| Slovence (Slovenya) | `sl-SI` | Erkek | `sl-SI-Lado`|
| İspanyolca (Meksika) | `es-MX` | Kadın | `es-MX-HildaRUS`|
| İspanyolca (Meksika) | `es-MX` | Erkek | `es-MX-Raul`|
| İspanyolca (İspanya) | `es-ES` | Kadın | `es-ES-HelenaRUS`|
| İspanyolca (İspanya) | `es-ES` | Kadın | `es-ES-Laura`|
| İspanyolca (İspanya) | `es-ES` | Erkek | `es-ES-Pablo`|
| İsveççe (İsviçre) | `sv-SE` | Kadın | `sv-SE-HedvigRUS`|
| Tamil dili (Hindistan) | `ta-IN` | Erkek | `ta-IN-Valluvar`|
| Telugu dili (Hindistan) | `te-IN` | Kadın | `te-IN-Chitra`|
| Tayca (Tayland) | `th-TH` | Erkek | `th-TH-Pattara`|
| Türkçe (Türkiye) | `tr-TR` | Kadın | `tr-TR-SedaRUS`|
| Vietnamca (Vietnam) | `vi-VN` | Erkek | `vi-VN-An` |

> [!IMPORTANT]
> `en-US-Jessa`Ses olarak değiştirildi `en-US-Aria` . Daha önce "Jessa" kullandıysanız, "Aria" öğesine dönüştürün.

> [!TIP]
> Konuşma birleştirme isteklerinizin "Microsoft Server konuşma Metin Okuma sesi (en-US, AriaRUS)" gibi tam hizmet adı eşlemesini kullanmaya devam edebilirsiniz.

### <a name="customization"></a>Özelleştirme

,,,,, `de-DE` `en-GB` `en-IN` `en-US` `es-MX` `fr-FR` , `it-IT` , `pt-BR` Ve `zh-CN` için ses özelleştirmesi kullanılabilir. Özel bir ses modeli eğitmeniz gereken eğitim verileriyle eşleşen doğru yerel ayarı seçin. Örneğin, kullandığınız kayıt verileri Ingiliz alfabesindeki bir vurgu ile konuşulursam, seçeneğini belirleyin `en-GB` .

> [!NOTE]
> Chinese-English bı-dilli dışında, özel seste bı-dilli model eğitimini desteklemiyoruz. Ingilizce 'ye de konuşarak bir Çince ses alıştırması yapmak istiyorsanız "Çince-Ingilizce çift dilli" seçeneğini belirleyin. Tüm yerel ayarlarda sesli eğitim, `en-US` `zh-CN` her türlü eğitim verisi ile başlayabileceğiniz ve dışında bir 2000 + utterations veri kümesiyle başlar.

## <a name="speech-translation"></a>Konuşma çevirisi

**Konuşma çevirisi** API 'si, konuşmaya konuşmaya ve konuşmadan metne dönüştürme için farklı dilleri destekler. Kaynak dilin her zaman konuşma-metin dili tablosundan olması gerekir. Kullanılabilir hedef diller, çeviri hedefinin konuşma veya metin olmasına bağlıdır. Gelen konuşmayı 60 ' den fazla [dile](https://www.microsoft.com/translator/business/languages/)çevirebilirsiniz. [Konuşma birleştirme](language-support.md#text-languages)için bir dil alt kümesi mevcuttur.

### <a name="text-languages"></a>Metin dilleri

| Metin dili           | Dil kodu |
|:------------------------|:-------------:|
| Afrikaner               | `af`          |
| Arapça                  | `ar`          |
| Bangla                  | `bn`          |
| Boşnakça (Latin)         | `bs`          |
| Bulgarca               | `bg`          |
| Cantonetıcı (Geleneksel) | `yue`         |
| Katalanca                 | `ca`          |
| Basitleştirilmiş Çince      | `zh-Hans`     |
| Geleneksel Çince     | `zh-Hant`     |
| Hırvatça                | `hr`          |
| Çekçe                   | `cs`          |
| Danca                  | `da`          |
| Felemenkçe                   | `nl`          |
| İngilizce                 | `en`          |
| Estonya Dili                | `et`          |
| Fiji Adaları dili                  | `fj`          |
| Filipino                | `fil`         |
| Fince                 | `fi`          |
| Fransızca                  | `fr`          |
| Almanca                  | `de`          |
| Yunanca                   | `el`          |
| Gucerat dili                | `gu`          |
| Haian Creole          | `ht`          |
| İbranice                  | `he`          |
| Hintçe                   | `hi`          |
| Hmong DAW               | `mww`         |
| Macarca               | `hu`          |
| Endonezce              | `id`          |
| İrlandaca                   | `ga`          |
| İtalyanca                 | `it`          |
| Japonca                | `ja`          |
| Kannada dili                 | `kn`          |
| Svahili dili               | `sw`          |
| Klingon                 | `tlh-Latn`    |
| Klingon (plqaD)         | `tlh-Piqd`    |
| Korece                  | `ko`          |
| Letonca                 | `lv`          |
| Litvanca              | `lt`          |
| Malgaşça                | `mg`          |
| Malayca                   | `ms`          |
| Malayalam dili               | `ml`          |
| Maltaca                 | `mt`          |
| Maori dili                   | `mi`          |
| Marathi                 | `mr`          |
| Norveççe               | `nb`          |
| Farsça                 | `fa`          |
| Lehçe                  | `pl`          |
| Portekizce (Brezilya)     | `pt-br`       |
| Portekizce (Portekiz)   | `pt-pt`       |
| Pencap dili                 | `pa`          |
| Queretaro Otomi         | `otq`         |
| Rumence                | `ro`          |
| Rusça                 | `ru`          |
| Samoan                  | `sm`          |
| Sırpça (Kiril)      | `sr-Cyrl`     |
| Sırpça (Latin)         | `sr-Latn`     |
| Slovakça                  | `sk`          |
| Slovence               | `sl`          |
| İspanyolca                 | `es`          |
| İsveççe                 | `sv`          |
| Tahiti dili                | `ty`          |
| Tamil dili                   | `ta`          |
| Telugu dili                  | `te`          |
| Tayca                    | `th`          |
| Tonga dili                  | `to`          |
| Türkçe                 | `tr`          |
| Ukraynaca               | `uk`          |
| Urduca                    | `ur`          |
| Vietnamca              | `vi`          |
| Galce                   | `cy`          |
| Yucatec Maya            | `yua`         |

## <a name="speaker-recognition"></a>Konuşmacı Tanıma

Çeşitli konuşmacı tanıma API 'Leri için desteklenen diller için aşağıdaki tabloya bakın. Konuşmacı Tanıma hakkında daha fazla bilgi için bkz. [genel bakış](speaker-recognition-overview.md) .

| Dil | Yerel ayar (BCP-47) | Metne bağımlı doğrulama | Metnin bağımsız doğrulaması | Metnin bağımsız tanımlayıcısı |
|----|----|----|----|----|
|İngilizce (ABD)  |  en-US  |  evet  |  evet  |  evet |
|Çince (Mandarin, Basitleştirilmiş) | zh-CN     |     yok |     evet |     evet|
|İngilizce (Avustralya)     | En-AU     | yok     | evet     | evet|
|İngilizce (Kanada)     | en-CA     | yok |     evet |     evet|
|İngilizce (UK)     | en-GB     | yok     | evet     | evet|
|Fransızca (Kanada)     | fr-CA     | yok     | evet |     evet|
|Fransızca (Fransa)     | fr-FR     | yok     | evet     | evet|
|Almanca (Almanya)     | de-DE     | yok     | evet     | evet|
|İtalyanca | it-IT     |     yok     | evet |     evet|
|Japonca     | ja-JP | yok     | evet     | evet|
|Portekizce (Brezilya) | pt-BR |     yok |     evet |     evet|
|İspanyolca (Meksika)     | es-MX     | yok |     evet |     evet|
|İspanyolca (İspanya)     | es-ES | yok     | evet |     evet|

## <a name="next-steps"></a>Sonraki adımlar

* [Ücretsiz bir Azure hesabı oluşturma](https://azure.microsoft.com/free/cognitive-services/)
* [Bkz. C 'de konuşmayı tanıma #](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
