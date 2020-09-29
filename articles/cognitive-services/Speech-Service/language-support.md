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
ms.custom: seodec18
ms.openlocfilehash: e0d6694d44fdac9958acdf70be067348fadde57f
ms.sourcegitcommit: a0c4499034c405ebc576e5e9ebd65084176e51e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91461249"
---
# <a name="language-and-voice-support-for-the-speech-service"></a>Konuşma hizmeti için dil ve ses desteği

Dil desteği, konuşma hizmeti işlevselliğine göre farklılık gösterir. Aşağıdaki tablolar, [konuşmadan metne](#speech-to-text), [metinden konuşmaya](#text-to-speech)ve [konuşma çevirisi](#speech-translation) hizmet teklifleri için dil desteğini özetler.

## <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

Hem Microsoft konuşma SDK 'Sı hem de REST API aşağıdaki dilleri (yerel ayarlar) destekler. 

Doğruluğu artırmak için, bir dilin bir alt kümesi için, **Ses + insan etiketli yazılı** döküm veya **Ilgili metin: cümleler**karşıya yüklenirken özelleştirme sunulur. Özelleştirme hakkında daha fazla bilgi için bkz. [özel konuşma tanıma kullanmaya başlama](how-to-custom-speech.md).

<!--
To get the AM and ML bits:
https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20models%3A/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.ai -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

| Yerel Ayar  | Dil                          | Özelleştirmeler                                    |
|---------|-----------------------------------|---------------------------------------------------|
| `ar-AE` | Arapça (Birleşik Arap Emirlikleri)     | Dil modeli                                    |
| `ar-BH` | Arapça (Bahreyn), modern standart | Dil modeli                                    |
| `ar-EG` | Arapça (Mısır)                    | Dil modeli                                    |
| `ar-IQ` | Arapça (Irak)                     | Dil modeli                                    |
| `ar-JO` | Arapça (Ürdün)                   | Dil modeli                                    |
| `ar-KW` | Arapça (Kuveyt)                   | Dil modeli                                    |
| `ar-LB` | Arapça (Lübnan)                  | Dil modeli                                    |
| `ar-OM` | Arapça (Umman)                     | Dil modeli                                    |
| `ar-QA` | Arapça (Qtor)                    | Dil modeli                                    |
| `ar-SA` | Arapça (Suudi Arabistan)             | Dil modeli                                    |
| `ar-SY` | Arapça (Suriye)                    | Dil modeli                                    |
| `bg-BG` | Bulgarca (Bulgaristan)              | Dil modeli                                    |
| `ca-ES` | Katalanca (İspanya)                   | Dil modeli                                    |
| `cs-CZ` | Çekçe (Çek Cumhuriyeti)            | Dil modeli                                    | 
| `da-DK` | Danca (Danimarka)                  | Dil modeli                                    |
| `de-DE` | Almanca (Almanya)                  | Akustik model<br>Dil modeli<br>İşi |
| `el-GR` | Yunanca (Yunanistan)                    | Dil modeli                                    |
| `en-AU` | İngilizce (Avustralya)               | Akustik model<br>Dil modeli                  |
| `en-CA` | İngilizce (Kanada)                  | Akustik model<br>Dil modeli                  |
| `en-GB` | İngilizce (İngiltere)          | Akustik model<br>Dil modeli<br>İşi |
| `en-HK` | İngilizce (Hong Kong)               | Dil modeli                                    | 
| `en-IE` | İngilizce (İrlanda)                 | Dil modeli                                    | 
| `en-IN` | İngilizce (Hindistan)                   | Akustik model<br>Dil modeli                  |
| `en-NZ` | İngilizce (Yeni Zelanda)             | Akustik model<br>Dil modeli                  |
| `en-PH` | İngilizce (Filipinler)             | Dil modeli                                    | 
| `en-SG` | İngilizce (Singapur)               | Dil modeli                                    | 
| `en-US` | İngilizce (ABD)           | Akustik model<br>Dil modeli<br>İşi |
| `en-ZA` | İngilizce (Güney Afrika)            | Dil modeli                                    | 
| `es-AR` | İspanyolca (Arjantin)               | Dil modeli                                    | 
| `es-BO` | İspanyolca (Bolivya)                 | Dil modeli                                    | 
| `es-CL` | İspanyolca (Şili)                   | Dil modeli                                    | 
| `es-CO` | İspanyolca (Kolombiya)                | Dil modeli                                    | 
| `es-CR` | İspanyolca (Kosta Rika)              | Dil modeli                                    | 
| `es-CU` | İspanyolca (Küa)                    | Dil modeli                                    | 
| `es-DO` | İspanyolca (Dominik Cumhuriyeti)      | Dil modeli                                    | 
| `es-EC` | İspanyolca (Ekvador)                 | Dil modeli                                    | 
| `es-ES` | İspanyolca (İspanya)                   | Akustik model<br>Dil modeli                  |
| `es-GT` | İspanyolca (Guatemala)               | Dil modeli                                    | 
| `es-HN` | İspanyolca (Honduras)                | Dil modeli                                    | 
| `es-MX` | İspanyolca (Meksika)                  | Akustik model<br>Dil modeli                  |
| `es-NI` | İspanyolca (Nikaragua)               | Dil modeli                                    | 
| `es-PA` | İspanyolca (Panama)                  | Dil modeli                                    | 
| `es-PE` | İspanyolca (Peru)                    | Dil modeli                                    | 
| `es-PR` | İspanyolca (Porto Riko)             | Dil modeli                                    | 
| `es-PY` | İspanyolca (Paraguay)                | Dil modeli                                    | 
| `es-SV` | İspanyolca (El Salvador)             | Dil modeli                                    | 
| `es-US` | İspanyolca (ABD)                     | Dil modeli                                    | 
| `es-UY` | İspanyolca (Uruguay)                 | Dil modeli                                    | 
| `es-VE` | İspanyolca (Venezuela)               | Dil modeli                                    |
| `et-EE` | Estonya dili (Estonya)                 | Dil modeli                                    | 
| `fi-FI` | Fince (Finlandiya)                 | Dil modeli                                    |
| `fr-CA` | Fransızca (Kanada)                   | Akustik model<br>Dil modeli                  |
| `fr-FR` | Fransızca (Fransa)                   | Akustik model<br>Dil modeli<br>İşi |
| `ga-IE` | İrlanda dili (Irlanda)                    | Dil modeli                                    |
| `gu-IN` | Gucerat dili (Hindistan)                 | Dil modeli                                    |
| `hi-IN` | Hintçe (Hindistan)                     | Akustik model<br>Dil modeli                  |
| `hr-HR` | Hırvatça (Hırvatistan)                | Dil modeli                                    |
| `hu-HU` | Macarca (Macaristan)               | Dil modeli                                    | 
| `it-IT` | İtalyanca (İtalya)                   | Akustik model<br>Dil modeli<br>İşi |
| `ja-JP` | Japonca (Japonya)                  | Dil modeli                                    |
| `ko-KR` | Korece (Kore)                    | Dil modeli                                    |
| `lt-LT` | Litvanca (Litvanya)            | Dil modeli                                    |
| `lv-LV` | Letonca (Letonya)                  | Dil modeli                                    |
| `mr-IN` | Marathi dili (Hindistan)                   | Dil modeli                                    |
| `mt-MT` | Malta dili (Malta)                    | Dil modeli                                    |
| `nb-NO` | Norveççe (Bokmål) (Norveç)       | Dil modeli                                    |
| `nl-NL` | Felemenkçe (Hollanda)               | Dil modeli                                    |
| `pl-PL` | Lehçe (Polonya)                   | Dil modeli                                    |
| `pt-BR` | Portekizce (Brezilya)               | Akustik model<br>Dil modeli<br>İşi |
| `pt-PT` | Portekizce (Portekiz)             | Dil modeli                                    |
| `ro-RO` | Rumence (Romanya)                | Dil modeli                                    |
| `ru-RU` | Rusça (Rusya)                  | Akustik model<br>Dil modeli                  |
| `sk-SK` | Slovakça (Slovakya)                 | Dil modeli                                    |
| `sl-SI` | Slovence (Slovenya)              | Dil modeli                                    |
| `sv-SE` | İsveççe (İsviçre)                  | Dil modeli                                    |
| `ta-IN` | Tamil dili (Hindistan)                     | Dil modeli                                    |
| `te-IN` | Telugu dili (Hindistan)                    | Dil modeli                                    |
| `th-TH` | Tayca (Tayland)                   | Dil modeli                                    |
| `tr-TR` | Türkçe (Türkiye)                  | Dil modeli                                    |
| `zh-CN` | Çince (Mandarin, Basitleştirilmiş)    | Akustik model<br>Dil modeli                  |
| `zh-HK` | Çince (Cantoneo, geleneksel)  | Dil modeli                                    |
| `zh-TW` | Çince (Taiwanese Mandarin)      | Dil modeli                                    |

## <a name="text-to-speech"></a>Metin okuma

Hem Microsoft konuşma SDK 'Sı hem de REST API 'Leri, her biri yerel ayar ile tanımlanan belirli bir dili ve diyalekt destekleyen bu sesleri destekler. Ayrıca, [sesler/liste API 'si](rest-text-to-speech.md#get-a-list-of-voices)aracılığıyla her belirli bölge/uç nokta için desteklenen dillerin ve seslerin tam listesini de alabilirsiniz. 

> [!IMPORTANT]
> Fiyatlandırma, standart, özel ve sinir sesleri için farklılık gösterir. Daha fazla bilgi için lütfen [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) sayfasını ziyaret edin.

### <a name="neural-voices"></a>Sinir sesleri

Sinir metin okuma, derin sinir ağlarla desteklenen yeni bir konuşma türü türüdür. Bir sinir sesi kullanırken, sentezlenmiş konuşma, insan kayıtlarından neredeyse ayırt edilemez.

Sinir sesleri, chatbots ve ses yardımcılarıyla daha doğal ve etkileyici bir şekilde etkileşim kurmak, e-kitaplar gibi dijital metinleri audiobooks 'a dönüştürmek ve oto içi gezinti sistemlerini geliştirmek için kullanılabilir. İnsan benzeri doğal Prosody ve sözcüklerin bir kısmını temizleyerek, sinir seslerde kullanıcılar AI sistemleriyle etkileşim kurarken büyük ölçüde dinleme performansını önemli ölçüde azaltır.

Bölgesel kullanılabilirlik hakkında daha fazla bilgi için bkz. [bölgeler](regions.md#standard-and-neural-voices).

|Yerel Ayar  | Dil            | Cinsiyet | Ses adı | Stil desteği |
|--|--|--|--|--|
| `ar-EG` | Arapça (Mısır) | Kadın | `ar-EG-SalmaNeural` | Genel |
| `ar-SA` | Arapça (Suudi Arabistan) | Kadın | `ar-SA-ZariyahNeural` | Genel |
| `bg-BG` <sup>Yeni</sup> | Bulgarca (Bulgary) | Kadın | `bg-BG-KalinaNeural` | Genel |
| `ca-ES` | Katalanca (Ispanya) | Kadın | `ca-ES-AlbaNeural` | Genel |
| `cs-CZ` <sup>Yeni</sup> | Çekçe (Çek)  | Kadın | `cs-CZ-VlastaNeural` | Genel |
| `da-DK` | Danca (Danimarka) | Kadın | `da-DK-ChristelNeural` | Genel |
| `de-AT` <sup>Yeni</sup> | Almanca (Avusturya) | Kadın | `de-AT-IngridNeural` | Genel |
| `de-CH` <sup>Yeni</sup> | Almanca (İsviçre) | Kadın | `de-CH-LeniNeural` | Genel |
| `de-DE` | Almanca (Almanya) | Kadın | `de-DE-KatjaNeural` | Genel |
| `de-DE` <sup>Yeni</sup> | Almanca (Almanya) | Erkek | `de-DE-ConradNeural` | Genel |
| `el-GR` <sup>Yeni</sup> | Yunanca (Yunanistan) | Kadın | `el-GR-AthinaNeural` | Genel |
| `en-AU` | İngilizce (Avustralya) | Kadın | `en-AU-NatashaNeural` | Genel |
| `en-AU` <sup>Yeni</sup> | Avustralya (Avustralya) | Erkek | `en-AU-WilliamNeural` | Genel |
| `en-CA` | İngilizce (Kanada) | Kadın | `en-CA-ClaraNeural` | Genel |
| `en-GB` | İngilizce (Birleşik Krallık) | Kadın | `en-GB-LibbyNeural` | Genel |
| `en-GB` | İngilizce (Birleşik Krallık) | Kadın | `en-GB-MiaNeural` | Genel |
| `en-GB` <sup>Yeni</sup> | İngilizce (İngiltere) | Erkek | `En-GB-RyanNeural` | Genel |
| `en-IE` <sup>Yeni</sup> | İrlanda Ingilizce (Irlanda) | Kadın | `en-IE-EmilyNeural` | Genel |
| `en-IN` | İngilizce (Hindistan) | Kadın | `en-IN-NeerjaNeural` | Genel |
| `en-US` | İngilizce (Birleşik Devletler) | Kadın | `en-US-AriaNeural` | Genel, birden çok ses stili mevcuttur |
| `en-US` | İngilizce (Birleşik Devletler) | Erkek | `en-US-GuyNeural` | Genel |
| `en-US` <sup>Yeni</sup> | İngilizce (Birleşik Devletler) | Kadın | `en-US-JennyNeural` | Genel, birden çok ses stili mevcuttur |
| `es-ES` | İspanyolca (Ispanya) | Kadın | `es-ES-ElviraNeural` | Genel |
| `es-ES` <sup>Yeni</sup> | İspanyolca (Ispanya) | Erkek | `es-ES-AlvaroNeural` | Genel |
| `es-MX` | İspanyolca (Meksika) | Kadın | `es-MX-DaliaNeural` | Genel |
| `es-MX` <sup>Yeni</sup> | İspanyolca (Meksika) | Erkek | `es-MX-JorgeNeural` | Genel |
| `fi-FI` | Fince (Finlandiya) | Kadın | `fi-FI-NooraNeural` | Genel |
| `fr-CA` | Fransızca (Kanada) | Kadın | `fr-CA-SylvieNeural` | Genel |
| `fr-CA` <sup>Yeni</sup> | Fransızca (Kanada) | Erkek | `fr-CA-JeanNeural` | Genel |
| `fr-CH` <sup>Yeni</sup> | Fransızca (İsviçre) | Kadın | `fr-CH-ArianeNeural` | Genel |
| `fr-FR` | Fransızca (Fransa) | Kadın | `fr-FR-DeniseNeural` | Genel |
| `fr-FR` <sup>Yeni</sup> | Fransızca (Fransa) | Erkek | `fr-FR-HenriNeural` | Genel |
| `he-IL` <sup>Yeni</sup> | İbranice (ısareal) | Kadın | `he-IL-HilaNeural` | Genel |
| `hi-IN` | Hintçe (Hindistan) | Kadın | `hi-IN-SwaraNeural` | Genel |
| `hr-HR` <sup>Yeni</sup> | Hırvatça (Hırvatistan) | Kadın | `hr-HR-GabrijelaNeural` | Genel |
| `hu-HU` <sup>Yeni</sup> | Macarca (Macaristan) | Kadın | `hu-HU-NoemiNeural` | Genel |
| `id-ID` <sup>Yeni</sup> | Bahasa Endonezya dili (Endonezya) | Erkek | `id-ID-ArdiNeural` | Genel |
| `it-IT` | İtalyanca (Italya) | Kadın | `it-IT-ElsaNeural` | Genel |
| `it-IT` <sup>Yeni</sup> | İtalyanca (İtalya) | Kadın | `it-IT-IsabellaNeural` | Genel |
| `it-IT` <sup>Yeni</sup> | İtalyanca (İtalya) | Erkek | `it-IT-DiegoNeural` | Genel |
| `ja-JP` | Japonca (Japonya) | Kadın | `ja-JP-NanamiNeural` | Genel |
| `ja-JP` <sup>Yeni</sup> | Japonca (Japonya) | Erkek | `ja-JP-KeitaNeural` | Genel |
| `ko-KR` | Kore dili (Kore) | Kadın | `ko-KR-SunHiNeural` | Genel |
| `ko-KR` <sup>Yeni</sup> | Korece (Kore) | Erkek | `ko-KR-InJoonNeural` | Genel |
| `ms-MY` <sup>Yeni</sup> | Malayca (Malezya) | Kadın | `ms-MY-YasminNeural` | Genel |
| `nb-NO` | Norveççe, Bokmål (Norveç) | Kadın | `nb-NO-IselinNeural` | Genel |
| `nl-NL` | Hollanda dili (Hollanda) | Kadın | `nl-NL-ColetteNeural` | Genel |
| `pl-PL` | Lehçe (Polonya) | Kadın | `pl-PL-ZofiaNeural` | Genel |
| `pt-BR` | Portekizce (Brezilya) | Kadın | `pt-BR-FranciscaNeural` | Genel, birden çok ses stili mevcuttur |
| `pt-BR` <sup>Yeni</sup> | Brezilya Portekizcesi Portekizce (Brezilya) | Erkek | `pt-BR-AntonioNeural` | Genel |
| `pt-PT` | Portekizce (Portekiz) | Kadın | `pt-PT-FernandaNeural` | Genel |
| `ro-RO` <sup>Yeni</sup> | Rumence (Romanya) | Kadın | `ro-RO-AlinaNeural` | Genel |
| `ru-RU` | Rusça (Rusya) | Kadın | `ru-RU-DariyaNeural` | Genel |
| `sk-SK` <sup>Yeni</sup> | Slovakça (Slovakya) | Kadın | `sk-SK-ViktoriaNeural` | Genel |
| `sl-SI` <sup>Yeni</sup> | Slovence (Slovenya) | Kadın | `sl-SI-PetraNeural` | Genel |
| `sv-SE` | İsveççe (Isveç) | Kadın | `sv-SE-HilleviNeural` | Genel |
| `ta-IN` <sup>Yeni</sup> | Tamil dili (Hindistan) | Kadın | `ta-IN-PallaviNeural` | Genel |
| `te-IN` <sup>Yeni</sup> | Telugu dili (Hindistan) | Kadın | `te-IN-ShrutiNeural` | Genel |
| `th-TH` | Tay dili (Tayland) | Kadın | `th-TH-AcharaNeural` | Genel |
| `th-TH` <sup>Yeni</sup> | Tayca (Tayland) | Kadın | `th-TH-PremwadeeNeural` | Genel |
| `tr-TR` | Türkçe (Türkiye) | Kadın | `tr-TR-EmelNeural` | Genel |
| `vi-VN` <sup>Yeni</sup> | Vietnamca (Vietnam) | Kadın | `vi-VN-HoaiMyNeural` | Genel |
| `zh-CN` | Mandarin (Basitleştirilmiş Çince, Çin) | Kadın | `zh-CN-XiaoxiaoNeural` | Genel, birden çok ses stili mevcuttur |
| `zh-CN` | Mandarin (Basitleştirilmiş Çince, Çin) | Kadın | `zh-CN-XiaoyouNeural` | Çocuk sesi, öykü anlatımı için iyileştirilmiş |
| `zh-CN` | Mandarin (Basitleştirilmiş Çince, Çin) | Erkek | `zh-CN-YunyangNeural` | Haber okuma için iyileştirilmiş, birden fazla ses stili var |
| `zh-CN` | Mandarin (Basitleştirilmiş Çince, Çin) | Erkek | `zh-CN-YunyeNeural` | Öykü anlatımı için iyileştirildi |
| `zh-HK` | Cantonetıcı (Geleneksel Çince, Hong Kong) | Kadın | `zh-HK-HiuGaaiNeural` | Genel |
| `zh-TW` | Mandarin (Geleneksel Çince, Tayvan) | Kadın | `zh-TW-HsiaoYuNeural` | Genel |


> [!IMPORTANT]
> `en-US-JessaNeural`Ses olarak değiştirildi `en-US-AriaNeural` . Daha önce "Jessa" kullandıysanız, "Aria" öğesine dönüştürün.

Sinir seslerini nasıl yapılandıracağınızı ve ayarlayabileceğinizi öğrenmek için bkz. [konuşma sen, biçimlendirme dili](speech-synthesis-markup.md#adjust-speaking-styles).

> [!TIP]
> Konuşma sensimi isteklerinizin "Microsoft Server konuşma Metin Okuma sesi (en-US, AriaNeural)" gibi tam hizmet adı eşlemesini kullanmaya devam edebilirsiniz.

### <a name="standard-voices"></a>Standart sesler

75 ' den fazla standart ses, daha fazla 45 dilde ve yerel ayarlarda bulunabilir ve bu da metni sentezleştirilmiş konuşmaya dönüştürmenize olanak tanır. Bölgesel kullanılabilirlik hakkında daha fazla bilgi için bkz. [bölgeler](regions.md#standard-and-neural-voices).

| Yerel Ayar | Dil | Cinsiyet | Ses adı |
|--|--|--|--|
| `ar-EG` | Arapça (Arapça) | Kadın | `ar-EG-Hoda` |
| `ar-SA` | Arapça (Suudi Arabistan) | Erkek | `ar-SA-Naayf` |
| `bg-BG` | Bulgarca (Bulgaristan) | Erkek | `bg-BG-Ivan` |
| `ca-ES` | Katalanca (İspanya) | Kadın | `ca-ES-HerenaRUS` |
| `cs-CZ` | Çekçe (Çek Cumhuriyeti) | Erkek | `cs-CZ-Jakub` |
| `da-DK` | Danca (Danimarka) | Kadın | `da-DK-HelleRUS` |
| `de-AT` | Almanca (Avusturya) | Erkek | `de-AT-Michael` |
| `de-CH` | Almanca (İsviçre) | Erkek | `de-CH-Karsten` |
| `de-DE` | Almanca (Almanya) | Kadın | `de-DE-HeddaRUS` |
| `de-DE` | Almanca (Almanya) | Erkek | `de-DE-Stefan` |
| `el-GR` | Yunanca (Yunanistan) | Erkek | `el-GR-Stefanos` |
| `en-AU` | İngilizce (Avustralya) | Kadın | `en-AU-Catherine` |
| `en-AU` | İngilizce (Avustralya) | Kadın | `en-AU-HayleyRUS` |
| `en-CA` | İngilizce (Kanada) | Kadın | `en-CA-HeatherRUS` |
| `en-CA` | İngilizce (Kanada) | Kadın | `en-CA-Linda` |
| `en-GB` | İngilizce (İngiltere) | Erkek | `en-GB-George` |
| `en-GB` | İngilizce (İngiltere) | Kadın | `en-GB-HazelRUS` |
| `en-GB` | İngilizce (İngiltere) | Kadın | `en-GB-Susan` |
| `en-IE` | İngilizce (İrlanda) | Erkek | `en-IE-Sean` |
| `en-IN` | İngilizce (Hindistan) | Kadın | `en-IN-Heera` |
| `en-IN` | İngilizce (Hindistan) | Kadın | `en-IN-PriyaRUS` |
| `en-IN` | İngilizce (Hindistan) | Erkek | `en-IN-Ravi` |
| `en-US` | İngilizce (ABD) | Erkek | `en-US-BenjaminRUS` |
| `en-US` | İngilizce (ABD) | Erkek | `en-US-GuyRUS` |
| `en-US` | İngilizce (ABD) | Kadın | `en-US-JessaRUS` |
| `en-US` | İngilizce (ABD) | Kadın | `en-US-ZiraRUS` |
| `es-ES` | İspanyolca (İspanya) | Kadın | `es-ES-HelenaRUS` |
| `es-ES` | İspanyolca (İspanya) | Kadın | `es-ES-Laura` |
| `es-ES` | İspanyolca (İspanya) | Erkek | `es-ES-Pablo` |
| `es-MX` | İspanyolca (Meksika) | Kadın | `es-MX-HildaRUS` |
| `es-MX` | İspanyolca (Meksika) | Erkek | `es-MX-Raul` |
| `fi-FI` | Fince (Finlandiya) | Kadın | `fi-FI-HeidiRUS` |
| `fr-CA` | Fransızca (Kanada) | Kadın | `fr-CA-Caroline` |
| `fr-CA` | Fransızca (Kanada) | Kadın | `fr-CA-HarmonieRUS` |
| `fr-CH` | Fransızca (İsviçre) | Erkek | `fr-CH-Guillaume` |
| `fr-FR` | Fransızca (Fransa) | Kadın | `fr-FR-HortenseRUS` |
| `fr-FR` | Fransızca (Fransa) | Kadın | `fr-FR-Julie` |
| `fr-FR` | Fransızca (Fransa) | Erkek | `fr-FR-Paul` |
| `he-IL` | İbranice (Israil) | Erkek | `he-IL-Asaf` |
| `hi-IN` | Hintçe (Hindistan) | Erkek | `hi-IN-Hemant` |
| `hi-IN` | Hintçe (Hindistan) | Kadın | `hi-IN-Kalpana` |
| `hr-HR` | Hırvatça (Hırvatistan) | Erkek | `hr-HR-Matej` |
| `hu-HU` | Macarca (Macaristan) | Erkek | `hu-HU-Szabolcs` |
| `id-ID` | Endonezce (Endonezya) | Erkek | `id-ID-Andika` |
| `it-IT` | İtalyanca (İtalya) | Erkek | `it-IT-Cosimo` |
| `it-IT` | İtalyanca (İtalya) | Kadın | `it-IT-LuciaRUS` |
| `ja-JP` | Japonca (Japonya) | Kadın | `ja-JP-Ayumi` |
| `ja-JP` | Japonca (Japonya) | Kadın | `ja-JP-HarukaRUS` |
| `ja-JP` | Japonca (Japonya) | Erkek | `ja-JP-Ichiro` |
| `ko-KR` | Korece (Kore) | Kadın | `ko-KR-HeamiRUS` |
| `ms-MY` | Malayca (Malezya) | Erkek | `ms-MY-Rizwan` |
| `nb-NO` | Norveççe, Bokmål (Norveç) | Kadın | `nb-NO-HuldaRUS` |
| `nl-NL` | Felemenkçe (Hollanda) | Kadın | `nl-NL-HannaRUS` |
| `pl-PL` | Lehçe (Polonya) | Kadın | `pl-PL-PaulinaRUS` |
| `pt-BR` | Portekizce (Brezilya) | Erkek | `pt-BR-Daniel` |
| `pt-BR` | Portekizce (Brezilya) | Kadın | `pt-BR-HeloisaRUS` |
| `pt-PT` | Portekizce (Portekiz) | Kadın | `pt-PT-HeliaRUS` |
| `ro-RO` | Rumence (Romanya) | Erkek | `ro-RO-Andrei` |
| `ru-RU` | Rusça (Rusya) | Kadın | `ru-RU-EkaterinaRUS` |
| `ru-RU` | Rusça (Rusya) | Kadın | `ru-RU-Irina` |
| `ru-RU` | Rusça (Rusya) | Erkek | `ru-RU-Pavel` |
| `sk-SK` | Slovakça (Slovakya) | Erkek | `sk-SK-Filip` |
| `sl-SI` | Slovence (Slovenya) | Erkek | `sl-SI-Lado` |
| `sv-SE` | İsveççe (İsviçre) | Kadın | `sv-SE-HedvigRUS` |
| `ta-IN` | Tamil dili (Hindistan) | Erkek | `ta-IN-Valluvar` |
| `te-IN` | Telugu dili (Hindistan) | Kadın | `te-IN-Chitra` |
| `th-TH` | Tayca (Tayland) | Erkek | `th-TH-Pattara` |
| `tr-TR` | Türkçe (Türkiye) | Kadın | `tr-TR-SedaRUS` |
| `vi-VN` | Vietnamca (Vietnam) | Erkek | `vi-VN-An` |
| `zh-CN` | Mandarin (Basitleştirilmiş Çince, Çin) | Kadın | `zh-CN-HuihuiRUS` |
| `zh-CN` | Mandarin (Basitleştirilmiş Çince, Çin) | Erkek | `zh-CN-Kangkang` |
| `zh-CN` | Mandarin (Basitleştirilmiş Çince, Çin) | Kadın | `zh-CN-Yaoyao` |
| `zh-HK` | Cantonetıcı (Geleneksel Çince, Hong Kong) | Erkek | `zh-HK-Danny` |
| `zh-HK` | Cantonetıcı (Geleneksel Çince, Hong Kong) | Kadın | `zh-HK-TracyRUS` |
| `zh-TW` | Mandarin (Geleneksel Çince, Tayvan) | Kadın | `zh-TW-HanHanRUS` |
| `zh-TW` | Mandarin (Geleneksel Çince, Tayvan) | Kadın | `zh-TW-Yating` |
| `zh-TW` | Mandarin (Geleneksel Çince, Tayvan) | Erkek | `zh-TW-Zhiwei` |

> [!IMPORTANT]
> `en-US-Jessa`Ses olarak değiştirildi `en-US-Aria` . Daha önce "Jessa" kullandıysanız, "Aria" öğesine dönüştürün.

> [!TIP]
> Konuşma birleştirme isteklerinizin "Microsoft Server konuşma Metin Okuma sesi (en-US, AriaRUS)" gibi tam hizmet adı eşlemesini kullanmaya devam edebilirsiniz.

### <a name="customization"></a>Özelleştirme

,,,,, `de-DE` `en-GB` `en-IN` `en-US` `es-MX` `fr-FR` , `it-IT` , `pt-BR` Ve `zh-CN` için ses özelleştirmesi kullanılabilir. Özel bir ses modeli eğitmeniz gereken eğitim verileriyle eşleşen doğru yerel ayarı seçin. Örneğin, kullandığınız kayıt verileri Ingiliz alfabesindeki bir vurgu ile konuşulursam, seçeneğini belirleyin `en-GB` .

> [!NOTE]
> Çince-Ingilizce bı-dil dışında, özel seste bı-dilli model eğitimini desteklemiyoruz. Ingilizce 'ye de konuşarak bir Çince ses alıştırması yapmak istiyorsanız "Çince-Ingilizce çift dilli" seçeneğini belirleyin. Tüm yerel ayarlarda sesli eğitim, `en-US` `zh-CN` her türlü eğitim verisi ile başlayabileceğiniz ve dışında bir 2000 + utterations veri kümesiyle başlar.

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

| Yerel Ayar | Dil | Metne bağımlı doğrulama | Metnin bağımsız doğrulaması | Metnin bağımsız tanımlayıcısı |
|----|----|----|----|----|
| tr-TR | İngilizce (ABD) | evet | evet | evet |
|zh-CN    |Çince (Mandarin, Basitleştirilmiş)|    yok|    evet|    evet|
|de-DE    |Almanca (Almanya)    |yok    |evet    |evet|
|en-GB    |İngilizce (UK)    |yok    |evet    |evet|
|fr-FR    |Fransızca (Fransa)    |yok    |evet    |evet|
|En-AU    |İngilizce (Avustralya)    |yok    |evet    |evet|
|en-CA    |İngilizce (Kanada)    |yok|    evet|    evet|
|fr-CA    |Fransızca (Kanada)    |yok    |evet|    evet|
|it-IT    |İtalyanca|    yok    |evet|    evet|
|es-ES|    İspanyolca (İspanya)    |yok    |evet|    evet|
|es-MX    |İspanyolca (Meksika)    |yok|    evet|    evet|
|ja-JP|    Japonca    |yok    |evet    |evet|
|pt-BR|    Portekizce (Brezilya)|    yok|    evet|    evet|

## <a name="next-steps"></a>Sonraki adımlar

* [Ücretsiz bir Azure hesabı oluşturma](https://azure.microsoft.com/free/cognitive-services/)
* [Bkz. C 'de konuşmayı tanıma #](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
