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
ms.date: 01/07/2021
ms.author: trbye
ms.custom: references_regions
ms.openlocfilehash: 68a384197409115343a81aea23b0f7d42166d716
ms.sourcegitcommit: 8f0803d3336d8c47654e119f1edd747180fe67aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97976818"
---
# <a name="language-and-voice-support-for-the-speech-service"></a>Konuşma hizmeti için dil ve ses desteği

Dil desteği, konuşma hizmeti işlevselliğine göre farklılık gösterir. Aşağıdaki tablolar, [konuşmadan metne](#speech-to-text), [metinden konuşmaya](#text-to-speech)ve [konuşma çevirisi](#speech-translation) hizmet teklifleri için dil desteğini özetler.

## <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

Hem Microsoft konuşma SDK 'Sı hem de REST API aşağıdaki dilleri (yerel ayarlar) destekler. 

Doğruluğu artırmak için, bir dilin bir alt kümesi için, **Ses + insan etiketli yazılı** döküm veya **Ilgili metin: cümleler** karşıya yüklenirken özelleştirme sunulur. Özelleştirme hakkında daha fazla bilgi için bkz. [özel konuşma tanıma kullanmaya başlama](./custom-speech-overview.md).

<!--
To get the AM and ML bits:
https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.ai -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

| Dil                           | Yerel ayar (BCP-47) | Özelleştirmeler                                   |
|------------------------------------|--------|--------------------------------------------------|
| Arapça (Bahreyn), modern standart  | `ar-BH` | Dil modeli                                   |
| Arapça (Mısır)                     | `ar-EG` | Dil modeli                                   |
| Arapça (Irak)                      | `ar-IQ` | Dil modeli                                   |
| Arapça (Israil)                    | `ar-IL` | Dil modeli                                   |
| Arapça (Ürdün)                    | `ar-JO` | Dil modeli                                   |
| Arapça (Kuveyt)                    | `ar-KW` | Dil modeli                                   |
| Arapça (Lübnan)                   | `ar-LB` | Dil modeli                                   |
| Arapça (Umman)                      | `ar-OM` | Dil modeli                                   |
| Arapça (Qtor)                     | `ar-QA` | Dil modeli                                   |
| Arapça (Suudi Arabistan)              | `ar-SA` | Dil modeli                                   |
| Arapça (Filistin Yönetimi devleti)        | `ar-PS` | Dil modeli                                   |
| Arapça (Suriye)                     | `ar-SY` | Dil modeli                                   |
| Arapça (Birleşik Arap Emirlikleri)      | `ar-AE` | Dil modeli                                   |
| Bulgarca (Bulgaristan)               | `bg-BG` | Dil modeli                                   |
| Katalanca (İspanya)                    | `ca-ES` | Dil modeli                                   |
| Çince (Cantoneo, geleneksel)   | `zh-HK` | Akustik model<br>Dil modeli                 |
| Çince (Mandarin, Basitleştirilmiş)     | `zh-CN` | Akustik model<br>Dil modeli                 |
| Çince (Taiwanese Mandarin)       | `zh-TW` | Akustik model<br>Dil modeli                 |
| Hırvatça (Hırvatistan)                 | `hr-HR` | Dil modeli                                   |
| Çekçe (Çek Cumhuriyeti)             | `cs-CZ` | Dil modeli                                   |
| Danca (Danimarka)                   | `da-DK` | Dil modeli                                   |
| Felemenkçe (Hollanda)                | `nl-NL` | Dil modeli                                   |
| İngilizce (Avustralya)                | `en-AU` | Akustik model<br>Dil modeli                 |
| İngilizce (Kanada)                   | `en-CA` | Akustik model<br>Dil modeli                 |
| İngilizce (Hong Kong)                | `en-HK` | Dil modeli                                   |
| İngilizce (Hindistan)                    | `en-IN` | Akustik model<br>Dil modeli                 |
| İngilizce (İrlanda)                  | `en-IE` | Dil modeli                                   |
| İngilizce (Yeni Zelanda)              | `en-NZ` | Akustik model<br>Dil modeli                 |
| İngilizce (Nijerya)                  | `en-NG` | Dil modeli                                   |
| İngilizce (Filipinler)              | `en-PH` | Dil modeli                                   |
| İngilizce (Singapur)                | `en-SG` | Dil modeli                                   |
| İngilizce (Güney Afrika)             | `en-ZA` | Dil modeli                                   |
| İngilizce (İngiltere)           | `en-GB` | Akustik model<br>Dil modeli<br>İşi|
| İngilizce (ABD)            | `en-US` | Akustik model<br>Dil modeli<br>İşi|
| Estonya dili (Estonya)                  | `et-EE` | Dil modeli                                   |
| Fince (Finlandiya)                  | `fi-FI` | Dil modeli                                   |
| Fransızca (Kanada)                    | `fr-CA` | Akustik model<br>Dil modeli                 |
| Fransızca (Fransa)                    | `fr-FR` | Akustik model<br>Dil modeli<br>İşi|
| Almanca (Almanya)                   | `de-DE` | Akustik model<br>Dil modeli<br>İşi|
| Yunanca (Yunanistan)                     | `el-GR` | Dil modeli                                   |
| Gucerat dili (Hindistan)                  | `gu-IN` | Dil modeli                                   |
| Hintçe (Hindistan)                      | `hi-IN` | Akustik model<br>Dil modeli                 |
| Macarca (Macaristan)                | `hu-HU` | Dil modeli                                   |
| İrlanda dili (Irlanda)                     | `ga-IE` | Dil modeli                                   |
| İtalyanca (İtalya)                    | `it-IT` | Akustik model<br>Dil modeli<br>İşi|
| Japonca (Japonya)                   | `ja-JP` | Akustik model<br>Dil modeli                 |
| Korece (Kore)                     | `ko-KR` | Akustik model<br>Dil modeli                 |
| Letonca (Letonya)                   | `lv-LV` | Dil modeli                                   |
| Litvanca (Litvanya)             | `lt-LT` | Dil modeli                                   |
| Malta dili (Malta)                     | `mt-MT` | Dil modeli                                   |
| Marathi dili (Hindistan)                    | `mr-IN` | Dil modeli                                   |
| Norveççe (Bokmål, Norveç)         | `nb-NO` | Dil modeli                                   |
| Lehçe (Polonya)                    | `pl-PL` | Dil modeli                                   |
| Portekizce (Brezilya)                | `pt-BR` | Akustik model<br>Dil modeli<br>İşi|
| Portekizce (Portekiz)              | `pt-PT` | Dil modeli                                   |
| Rumence (Romanya)                 | `ro-RO` | Dil modeli                                   |
| Rusça (Rusya)                   | `ru-RU` | Akustik model<br>Dil modeli                 |
| Slovakça (Slovakya)                  | `sk-SK` | Dil modeli                                   |
| Slovence (Slovenya)               | `sl-SI` | Dil modeli                                   |
| İspanyolca (Arjantin)                | `es-AR` | Dil modeli                                   |
| İspanyolca (Bolivya)                  | `es-BO` | Dil modeli                                   |
| İspanyolca (Şili)                    | `es-CL` | Dil modeli                                   |
| İspanyolca (Kolombiya)                 | `es-CO` | Dil modeli                                   |
| İspanyolca (Kosta Rika)               | `es-CR` | Dil modeli                                   |
| İspanyolca (Küa)                     | `es-CU` | Dil modeli                                   |
| İspanyolca (Dominik Cumhuriyeti)       | `es-DO` | Dil modeli                                   |
| İspanyolca (Ekvador)                  | `es-EC` | Dil modeli                                   |
| İspanyolca (El Salvador)              | `es-SV` | Dil modeli                                   |
| İspanyolca (Ekvator Ginesi)        | `es-GQ` | Dil modeli                                   |
| İspanyolca (Guatemala)                | `es-GT` | Dil modeli                                   |
| İspanyolca (Honduras)                 | `es-HN` | Dil modeli                                   |
| İspanyolca (Meksika)                   | `es-MX` | Akustik model<br>Dil modeli                 |
| İspanyolca (Nikaragua)                | `es-NI` | Dil modeli                                   |
| İspanyolca (Panama)                   | `es-PA` | Dil modeli                                   |
| İspanyolca (Paraguay)                 | `es-PY` | Dil modeli                                   |
| İspanyolca (Peru)                     | `es-PE` | Dil modeli                                   |
| İspanyolca (Porto Riko)              | `es-PR` | Dil modeli                                   |
| İspanyolca (İspanya)                    | `es-ES` | Akustik model<br>Dil modeli                 |
| İspanyolca (Uruguay)                  | `es-UY` | Dil modeli                                   |
| İspanyolca (ABD)                      | `es-US` | Dil modeli                                   |
| İspanyolca (Venezuela)                | `es-VE` | Dil modeli                                   |
| İsveççe (İsviçre)                   | `sv-SE` | Dil modeli                                   |
| Tamil dili (Hindistan)                      | `ta-IN` | Dil modeli                                   |
| Telugu dili (Hindistan)                     | `te-IN` | Dil modeli                                   |
| Tayca (Tayland)                    | `th-TH` | Dil modeli                                   |
| Türkçe (Türkiye)                   | `tr-TR` | Dil modeli                                   |

## <a name="text-to-speech"></a>Metin okuma

Hem Microsoft konuşma SDK 'Sı hem de REST API 'Leri, her biri yerel ayar ile tanımlanan belirli bir dili ve diyalekt destekleyen bu sesleri destekler. Ayrıca, [sesler/liste API 'si](rest-text-to-speech.md#get-a-list-of-voices)aracılığıyla her belirli bölge/uç nokta için desteklenen dillerin ve seslerin tam listesini de alabilirsiniz. 

> [!IMPORTANT]
> Fiyatlandırma, standart, özel ve sinir sesleri için farklılık gösterir. Daha fazla bilgi için lütfen [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) sayfasını ziyaret edin.

### <a name="neural-voices"></a>Sinir sesleri

Sinir metin okuma, derin sinir ağlarla desteklenen yeni bir konuşma türü türüdür. Bir sinir sesi kullanırken, sentezlenmiş konuşma, insan kayıtlarından neredeyse ayırt edilemez.

Sinir sesleri, chatbots ve ses yardımcılarıyla daha doğal ve etkileyici bir şekilde etkileşim kurmak, e-kitaplar gibi dijital metinleri audiobooks 'a dönüştürmek ve oto içi gezinti sistemlerini geliştirmek için kullanılabilir. İnsan benzeri doğal Prosody ve sözcüklerin bir kısmını temizleyerek, sinir seslerde kullanıcılar AI sistemleriyle etkileşim kurarken büyük ölçüde dinleme performansını önemli ölçüde azaltır.

> [!NOTE]
> Sinir sesleri, 24 kHz örnek bir hız kullanan örneklerden oluşturulur.
> Tüm sesler, birleştirme sırasında diğer örnek hızlara eşit veya daha fazla örnek alabilir.


| Dil | Yerel Ayar | Cinsiyet | Ses adı | Stil desteği |
|---|---|---|---|---|
| Arapça (Mısır) | `ar-EG` | Kadın | `ar-EG-SalmaNeural` | Genel |
| Arapça (Mısır) | `ar-EG` | Erkek | `ar-EG-ShakirNeural` <sup>Yeni</sup> | Genel |
| Arapça (Suudi Arabistan) | `ar-SA` | Kadın | `ar-SA-ZariyahNeural` | Genel |
| Arapça (Suudi Arabistan) | `ar-SA` | Erkek | `ar-SA-HamedNeural` <sup>Yeni</sup> | Genel |
| Bulgarca (Bulgaristan) | `bg-BG` | Kadın | `bg-BG-KalinaNeural` | Genel |
| Bulgarca (Bulgaristan) | `bg-BG` | Erkek | `bg-BG-BorislavNeural` <sup>Yeni</sup> | Genel |
| Katalanca (İspanya) | `ca-ES` | Kadın | `ca-ES-AlbaNeural` | Genel |
| Katalanca (İspanya) | `ca-ES` | Kadın | `ca-ES-JoanaNeural` <sup>Yeni</sup> | Genel |
| Katalanca (İspanya) | `ca-ES` | Erkek | `ca-ES-EnricNeural` <sup>Yeni</sup> | Genel |
| Çince (Cantoneo, geleneksel) | `zh-HK` | Kadın | `zh-HK-HiuGaaiNeural` | Genel |
| Çince (Cantoneo, geleneksel) | `zh-HK` | Kadın | `zh-HK-HiuMaanNeural` <sup>Yeni</sup> | Genel |
| Çince (Cantoneo, geleneksel) | `zh-HK` | Erkek | `zh-HK-WanLungNeural` <sup>Yeni</sup> | Genel |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-XiaoxiaoNeural` | Genel, [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) birden çok ses stili mevcuttur  |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-XiaoyouNeural` | Çocuk sesi, öykü anlatımı için iyileştirilmiş |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Erkek | `zh-CN-YunyangNeural` | Haberleri okuma için iyileştirilmiş,<br /> [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) birden çok ses stili kullanılabilir |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Erkek | `zh-CN-YunyeNeural` | Öykü anlatımı için iyileştirildi  |
| Çince (Taiwanese Mandarin) | `zh-TW` | Kadın | `zh-TW-HsiaoChenNeural` <sup>Yeni</sup> | Genel |
| Çince (Taiwanese Mandarin) | `zh-TW` | Kadın | `zh-TW-HsiaoYuNeural` | Genel |
| Çince (Taiwanese Mandarin) | `zh-TW` | Erkek | `zh-TW-YunJheNeural` <sup>Yeni</sup> | Genel |
| Hırvatça (Hırvatistan) | `hr-HR` | Kadın | `hr-HR-GabrijelaNeural` | Genel |
| Hırvatça (Hırvatistan) | `hr-HR` | Erkek | `hr-HR-SreckoNeural` <sup>Yeni</sup> | Genel |
| Çekçe (Çek) | `cs-CZ` | Kadın | `cs-CZ-VlastaNeural` | Genel |
| Çekçe (Çek) | `cs-CZ` | Erkek | `cs-CZ-AntoninNeural` <sup>Yeni</sup> | Genel |
| Danca (Danimarka) | `da-DK` | Kadın | `da-DK-ChristelNeural` | Genel |
| Danca (Danimarka) | `da-DK` | Erkek | `da-DK-JeppeNeural` <sup>Yeni</sup> | Genel |
| Felemenkçe (Hollanda) | `nl-NL` | Kadın | `nl-NL-ColetteNeural` | Genel |
| Felemenkçe (Hollanda) | `nl-NL` | Kadın | `nl-NL-FennaNeural` <sup>Yeni</sup> | Genel |
| Felemenkçe (Hollanda) | `nl-NL` | Erkek | `nl-NL-MaartenNeural` <sup>Yeni</sup> | Genel |
| İngilizce (Avustralya) | `en-AU` | Kadın | `en-AU-NatashaNeural` | Genel |
| İngilizce (Avustralya) | `en-AU` | Erkek | `en-AU-WilliamNeural` | Genel |
| İngilizce (Kanada) | `en-CA` | Kadın | `en-CA-ClaraNeural` | Genel |
| İngilizce (Kanada) | `en-CA` | Erkek | `en-CA-LiamNeural` <sup>Yeni</sup> | Genel |
| İngilizce (Hindistan) | `en-IN` | Kadın | `en-IN-NeerjaNeural` | Genel |
| İngilizce (Hindistan) | `en-IN` | Erkek | `en-IN-PrabhatNeural` <sup>Yeni</sup> | Genel |
| İngilizce (İrlanda) | `en-IE` | Kadın | `en-IE-EmilyNeural` | Genel |
| İngilizce (İrlanda) | `en-IE` | Erkek | `en-IE-ConnorNeural` <sup>Yeni</sup> | Genel |
| İngilizce (İngiltere) | `en-GB` | Kadın | `en-GB-LibbyNeural` | Genel |
| İngilizce (İngiltere) | `en-GB` | Kadın | `en-GB-MiaNeural` | Genel |
| İngilizce (İngiltere) | `en-GB` | Erkek | `en-GB-RyanNeural` | Genel |
| İngilizce (ABD) | `en-US` | Kadın | `en-US-AriaNeural` | Genel, [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) birden çok ses stili mevcuttur  |
| İngilizce (ABD) | `en-US` | Kadın | `en-US-JennyNeural` | Genel |
| İngilizce (ABD) | `en-US` | Erkek | `en-US-GuyNeural` | Genel |
| Fince (Finlandiya) | `fi-FI` | Kadın | `fi-FI-NooraNeural` | Genel |
| Fince (Finlandiya) | `fi-FI` | Kadın | `fi-FI-SelmaNeural` <sup>Yeni</sup> | Genel |
| Fince (Finlandiya) | `fi-FI` | Erkek | `fi-FI-HarriNeural` <sup>Yeni</sup> | Genel |
| Fransızca (Kanada) | `fr-CA` | Kadın | `fr-CA-SylvieNeural` | Genel |
| Fransızca (Kanada) | `fr-CA` | Erkek | `fr-CA-JeanNeural` | Genel |
| Fransızca (Fransa) | `fr-FR` | Kadın | `fr-FR-DeniseNeural` | Genel |
| Fransızca (Fransa) | `fr-FR` | Erkek | `fr-FR-HenriNeural` | Genel |
| Fransızca (İsviçre) | `fr-CH` | Kadın | `fr-CH-ArianeNeural` | Genel |
| Fransızca (İsviçre) | `fr-CH` | Erkek | `fr-CH-FabriceNeural` <sup>Yeni</sup> | Genel |
| Almanca (Avusturya) | `de-AT` | Kadın | `de-AT-IngridNeural` | Genel |
| Almanca (Avusturya) | `de-AT` | Erkek | `de-AT-JonasNeural` <sup>Yeni</sup> | Genel |
| Almanca (Almanya) | `de-DE` | Kadın | `de-DE-KatjaNeural` | Genel |
| Almanca (Almanya) | `de-DE` | Erkek | `de-DE-ConradNeural` | Genel |
| Almanca (İsviçre) | `de-CH` | Kadın | `de-CH-LeniNeural` | Genel |
| Almanca (İsviçre) | `de-CH` | Erkek | `de-CH-JanNeural` <sup>Yeni</sup> | Genel |
| Yunanca (Yunanistan) | `el-GR` | Kadın | `el-GR-AthinaNeural` | Genel |
| Yunanca (Yunanistan) | `el-GR` | Erkek | `el-GR-NestorasNeural` <sup>Yeni</sup> | Genel |
| İbranice (Israil) | `he-IL` | Kadın | `he-IL-HilaNeural` | Genel |
| İbranice (Israil) | `he-IL` | Erkek | `he-IL-AvriNeural` <sup>Yeni</sup> | Genel |
| Hintçe (Hindistan) | `hi-IN` | Kadın | `hi-IN-SwaraNeural` | Genel |
| Hintçe (Hindistan) | `hi-IN` | Erkek | `hi-IN-MadhurNeural` <sup>Yeni</sup> | Genel |
| Macarca (Macaristan) | `hu-HU` | Kadın | `hu-HU-NoemiNeural` | Genel |
| Macarca (Macaristan) | `hu-HU` | Erkek | `hu-HU-TamasNeural` <sup>Yeni</sup> | Genel |
| Endonezce (Endonezya) | `id-ID` | Kadın | `id-ID-GadisNeural` <sup>Yeni</sup> | Genel |
| Endonezce (Endonezya) | `id-ID` | Erkek | `id-ID-ArdiNeural` | Genel |
| İtalyanca (İtalya) | `it-IT` | Kadın | `it-IT-ElsaNeural` | Genel |
| İtalyanca (İtalya) | `it-IT` | Kadın | `it-IT-IsabellaNeural` | Genel |
| İtalyanca (İtalya) | `it-IT` | Erkek | `it-IT-DiegoNeural` | Genel |
| Japonca (Japonya) | `ja-JP` | Kadın | `ja-JP-NanamiNeural` | Genel |
| Japonca (Japonya) | `ja-JP` | Erkek | `ja-JP-KeitaNeural` | Genel |
| Korece (Kore) | `ko-KR` | Kadın | `ko-KR-SunHiNeural` | Genel |
| Korece (Kore) | `ko-KR` | Erkek | `ko-KR-InJoonNeural` | Genel |
| Malayca (Malezya) | `ms-MY` | Kadın | `ms-MY-YasminNeural` | Genel |
| Malayca (Malezya) | `ms-MY` | Erkek | `ms-MY-OsmanNeural` <sup>Yeni</sup> | Genel |
| Norveççe (Bokmål, Norveç) | `nb-NO` | Kadın | `nb-NO-IselinNeural` | Genel |
| Norveççe (Bokmål, Norveç) | `nb-NO` | Kadın | `nb-NO-PernilleNeural` <sup>Yeni</sup> | Genel |
| Norveççe (Bokmål, Norveç) | `nb-NO` | Erkek | `nb-NO-FinnNeural` <sup>Yeni</sup> | Genel |
| Lehçe (Polonya) | `pl-PL` | Kadın | `pl-PL-AgnieszkaNeural` <sup>Yeni</sup> | Genel |
| Lehçe (Polonya) | `pl-PL` | Kadın | `pl-PL-ZofiaNeural` | Genel |
| Lehçe (Polonya) | `pl-PL` | Erkek | `pl-PL-MarekNeural` <sup>Yeni</sup> | Genel |
| Portekizce (Brezilya) | `pt-BR` | Kadın | `pt-BR-FranciscaNeural` | Genel, [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) birden çok ses stili mevcuttur  |
| Portekizce (Brezilya) | `pt-BR` | Erkek | `pt-BR-AntonioNeural` | Genel |
| Portekizce (Portekiz) | `pt-PT` | Kadın | `pt-PT-FernandaNeural` | Genel |
| Portekizce (Portekiz) | `pt-PT` | Kadın | `pt-PT-RaquelNeural` <sup>Yeni</sup> | Genel |
| Portekizce (Portekiz) | `pt-PT` | Erkek | `pt-PT-DuarteNeural` <sup>Yeni</sup> | Genel |
| Rumence (Romanya) | `ro-RO` | Kadın | `ro-RO-AlinaNeural` | Genel |
| Rumence (Romanya) | `ro-RO` | Erkek | `ro-RO-EmilNeural` <sup>Yeni</sup> | Genel |
| Rusça (Rusya) | `ru-RU` | Kadın | `ru-RU-DariyaNeural` | Genel |
| Rusça (Rusya) | `ru-RU` | Kadın | `ru-RU-SvetlanaNeural` <sup>Yeni</sup> | Genel |
| Rusça (Rusya) | `ru-RU` | Erkek | `ru-RU-DmitryNeural` <sup>Yeni</sup> | Genel |
| Slovakça (Slovakya) | `sk-SK` | Kadın | `sk-SK-ViktoriaNeural` | Genel |
| Slovakça (Slovakya) | `sk-SK` | Erkek | `sk-SK-LukasNeural` <sup>Yeni</sup> | Genel |
| Slovence (Slovenya) | `sl-SI` | Kadın | `sl-SI-PetraNeural` | Genel |
| Slovence (Slovenya) | `sl-SI` | Erkek | `sl-SI-RokNeural` <sup>Yeni</sup> | Genel |
| İspanyolca (Meksika) | `es-MX` | Kadın | `es-MX-DaliaNeural` | Genel |
| İspanyolca (Meksika) | `es-MX` | Erkek | `es-MX-JorgeNeural` | Genel |
| İspanyolca (İspanya) | `es-ES` | Kadın | `es-ES-ElviraNeural` | Genel |
| İspanyolca (İspanya) | `es-ES` | Erkek | `es-ES-AlvaroNeural` | Genel |
| İsveççe (İsviçre) | `sv-SE` | Kadın | `sv-SE-HilleviNeural` | Genel |
| İsveççe (İsviçre) | `sv-SE` | Kadın | `sv-SE-SofieNeural` <sup>Yeni</sup> | Genel |
| İsveççe (İsviçre) | `sv-SE` | Erkek | `sv-SE-MattiasNeural` <sup>Yeni</sup> | Genel |
| Tamil dili (Hindistan) | `ta-IN` | Kadın | `ta-IN-PallaviNeural` | Genel |
| Tamil dili (Hindistan) | `ta-IN` | Erkek | `ta-IN-ValluvarNeural` <sup>Yeni</sup> | Genel |
| Telugu dili (Hindistan) | `te-IN` | Kadın | `te-IN-ShrutiNeural` | Genel |
| Telugu dili (Hindistan) | `te-IN` | Erkek | `te-IN-MohanNeural` <sup>Yeni</sup> | Genel |
| Tayca (Tayland) | `th-TH` | Kadın | `th-TH-AcharaNeural` | Genel |
| Tayca (Tayland) | `th-TH` | Kadın | `th-TH-PremwadeeNeural` | Genel |
| Tayca (Tayland) | `th-TH` | Erkek | `th-TH-NiwatNeural` <sup>Yeni</sup> | Genel |
| Türkçe (Türkiye) | `tr-TR` | Kadın | `tr-TR-EmelNeural` | Genel |
| Türkçe (Türkiye) | `tr-TR` | Erkek | `tr-TR-AhmetNeural` <sup>Yeni</sup> | Genel |
| Vietnamca (Vietnam) | `vi-VN` | Kadın | `vi-VN-HoaiMyNeural` | Genel |
| Vietnamca (Vietnam) | `vi-VN` | Erkek | `vi-VN-NamMinhNeural` <sup>Yeni</sup> | Genel |

#### <a name="neural-voices-in-preview"></a>Önizlemede sinir sesler

Aşağıdaki sinir sesleri genel önizlemede. 

| Dil                         | Yerel Ayar  | Cinsiyet | Ses adı                             | Stil desteği |
|----------------------------------|---------|--------|----------------------------------------|---------------|
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-XiaohanNeural` | [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) genel, birden çok stil kullanılabilir |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-XiaomoNeural` | [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) genel, birden çok rol yürütme ve stil kullanılabilir |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-XiaoruiNeural` | [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) üst düzey ses, birden çok stil kullanılabilir |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Kadın | `zh-CN-XiaoxuanNeural` | [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) genel, birden çok rol yürütme ve stil kullanılabilir |
| Çince (Mandarin, Basitleştirilmiş) | `zh-CN` | Erkek   | `zh-CN-YunxiNeural` | [SSML kullanarak](speech-synthesis-markup.md#adjust-speaking-styles) genel, birden çok stil kullanılabilir |
| Estonca (Estonya) | `et-EE` | Kadın | `et-EE-AnuNeural` | Genel |
| Estonca (Estonya) | `et-EE` | Erkek | `et-EE-KertNeural` <sup>Yeni</sup> | Genel |
| İrlanda dili (Irlanda) | `ga-IE` | Kadın | `ga-IE-OrlaNeural` | Genel |
| İrlanda dili (Irlanda) | `ga-IE` | Erkek | `ga-IE-ColmNeural` <sup>Yeni</sup> | Genel |
| Letonca (Letonya) | `lv-LV` | Kadın | `lv-LV-EveritaNeural` | Genel |
| Letonca (Letonya) | `lv-LV` | Erkek | `lv-LV-NilsNeural` <sup>Yeni</sup> | Genel |
| Litvanca (Litvanya) | `lt-LT` | Kadın | `lt-LT-OnaNeural` | Genel |
| Litvanca (Litvanya) | `lt-LT` | Erkek | `lt-LT-LeonasNeural` <sup>Yeni</sup> | Genel |
| Malta dili (Malta) | `mt-MT` | Kadın | `mt-MT-GraceNeural` | Genel |
| Malta dili (Malta) | `mt-MT` | Erkek | `mt-MT-JosephNeural` <sup>Yeni</sup> | Genel |

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

> [!NOTE]
> İki özel durum ile, 16 kHz örnek oranı kullanan örneklerden standart sesler oluşturulur.
> **En-US-AriaRUS** ve **en-US-Guyrus** sesleriniz, 24 kHz örnek bir hız kullanan örneklerden de oluşturulmuştur.
> Tüm sesler, birleştirme sırasında diğer örnek hızlara eşit veya daha fazla örnek alabilir.

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
| İngilizce (ABD) | `en-US` | Kadın | `en-US-AriaRUS`|
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
* [Bkz. C 'de konuşmayı tanıma #](./get-started-speech-to-text.md?pivots=programming-language-chsarp)
