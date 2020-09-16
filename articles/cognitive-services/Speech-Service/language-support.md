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
ms.openlocfilehash: de6cd4ee2c9800757399b7e32b59d903b817c657
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90604402"
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
| `ar-AE` | Arapça (UAE)                      | No                                                |
| `ar-BH` | Arapça (Bahreyn), modern standart | Dil modeli                                    |
| `ar-EG` | Arapça (Mısır)                    | Dil modeli                                    |
| `ar-IL` | Arapça (Israil)                   | No                                                |
| `ar-IQ` | Arapça (Irak)                     | No                                                |
| `ar-JO` | Arapça (Ürdün)                   | No                                                |
| `ar-KW` | Arapça (Kuveyt)                   | No                                                |
| `ar-LB` | Arapça (Lübnan)                  | No                                                |
| `ar-OM` | Arapça (Umman)                     | No                                                |
| `ar-PS` | Arapça (Filistin Yönetimi)                | No                                                |
| `ar-QA` | Arapça (Qtor)                    | No                                                |
| `ar-SA` | Arapça (Suudi Arabistan)             | No                                                |
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
| `th-TH` | Tayca (Tayland)                   | No                                                |
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
| `ar-EG` | Arapça (Mısır)                  | Kadın | `ar-EG-SalmaNeural`      | Genel |
| `ar-SA` | Arapça (Suudi Arabistan)           | Kadın | `ar-SA-ZariyahNeural`    | Genel |
| `ca-ES` | Katalanca (İspanya)                 | Kadın | `ca-ES-AlbaNeural`       | Genel |
| `da-DK` | Danca (Danimarka)                | Kadın | `da-DK-ChristelNeural`   | Genel |
| `de-DE` | Almanca (Almanya)                | Kadın | `de-DE-KatjaNeural`      | Genel |
| `en-AU` | İngilizce (Avustralya)             | Kadın | `en-AU-NatashaNeural`    | Genel |
| `en-CA` | İngilizce (Kanada)                | Kadın | `en-CA-ClaraNeural`      | Genel |
| `en-GB` | İngilizce (UK)                    | Kadın | `en-GB-LibbyNeural`      | Genel |
|         |                                 | Kadın | `en-GB-MiaNeural`        | Genel |
| `en-IN` | İngilizce (Hindistan)                 | Kadın | `en-IN-NeerjaNeural`     | Genel |
| `en-US` | İngilizce (ABD)                    | Kadın | `en-US-AriaNeural`       | Genel, birden çok ses stili mevcuttur |
|         |                                 | Erkek   | `en-US-GuyNeural`        | Genel |
| `es-ES` | İspanyolca (İspanya)                 | Kadın | `es-ES-ElviraNeural`     | Genel |
| `es-MX` | İspanyolca (Meksika)                | Kadın | `es-MX-DaliaNeural`      | Genel |
| `fi-FI` | Fince (Finlandiya)               | Kadın | `fi-FI-NooraNeural`      | Genel |
| `fr-CA` | Fransızca (Kanada)                 | Kadın | `fr-CA-SylvieNeural`     | Genel |
| `fr-FR` | Fransızca (Fransa)                 | Kadın | `fr-FR-DeniseNeural`     | Genel |
| `hi-IN` | Hintçe (Hindistan)                   | Kadın | `hi-IN-SwaraNeural`      | Genel |
| `it-IT` | İtalyanca (İtalya)                 | Kadın | `it-IT-ElsaNeural`       | Genel |
| `ja-JP` | Japonca                        | Kadın | `ja-JP-NanamiNeural`     | Genel |
| `ko-KR` | Korece                          | Kadın | `ko-KR-SunHiNeural`      | Genel |
| `nb-NO` | Norveççe                       | Kadın | `nb-NO-IselinNeural`     | Genel |
| `nl-NL` | Felemenkçe (Netherland)              | Kadın | `nl-NL-ColetteNeural`    | Genel |
| `pl-PL` | Lehçe (Polonya)                 | Kadın | `pl-PL-ZofiaNeural`      | Genel |
| `pt-BR` | Portekizce (Brezilya)             | Kadın | `pt-BR-FranciscaNeural`  | Genel, birden çok ses stili mevcuttur |
| `tr-TR` | Türkçe                         | Kadın | `tr-TR-EmelNeural`       | Genel |
| `pt-PT` | Portekizce (Portekiz)           | Kadın | `pt-PT-FernandaNeural`   | Genel |
| `ru-RU` | Rusça (Rusya)                | Kadın | `ru-RU-DariyaNeural`     | Genel |
| `sv-SE` | İsveççe (İsviçre)                | Kadın | `sv-SE-HilleviNeural`    | Genel |
| `th-TH` | Tayca (Tayland)                 | Kadın | `th-TH-AcharaNeural`     | Genel |
| `zh-CN` | Çince (Mandarin, Basitleştirilmiş)  | Kadın | `zh-CN-XiaoxiaoNeural`   | Genel, birden çok ses stili mevcuttur |
|         |                                 | Kadın | `zh-CN-XiaoyouNeural`    | Çocuk sesi, öykü anlatımı için iyileştirilmiş |
|         |                                 | Erkek   | `zh-CN-YunyangNeural`    | Haber okuma için iyileştirilmiş, birden fazla ses stili var |
|         |                                 | Erkek   | `zh-CN-YunyeNeural`      | Öykü anlatımı için iyileştirildi |
| `zh-HK` | Çince (Cantoneo, geleneksel)   | Kadın | `zh-HK-HiuGaaiNeural`| Genel |
| `zh-TW` | Çince (Taiwanese Mandarin)   | Kadın | `zh-TW-HsiaoYuNeural`    | Genel |

> [!IMPORTANT]
> `en-US-JessaNeural`Ses olarak değiştirildi `en-US-AriaNeural` . Daha önce "Jessa" kullandıysanız, "Aria" öğesine dönüştürün.

Sinir seslerini nasıl yapılandıracağınızı ve ayarlayabileceğinizi öğrenmek için bkz. [konuşma sen, biçimlendirme dili](speech-synthesis-markup.md#adjust-speaking-styles).

> [!TIP]
> Konuşma sensimi isteklerinizin "Microsoft Server konuşma Metin Okuma sesi (en-US, AriaNeural)" gibi tam hizmet adı eşlemesini kullanmaya devam edebilirsiniz.

### <a name="standard-voices"></a>Standart sesler

75 ' den fazla standart ses, daha fazla 45 dilde ve yerel ayarlarda bulunabilir ve bu da metni sentezleştirilmiş konuşmaya dönüştürmenize olanak tanır. Bölgesel kullanılabilirlik hakkında daha fazla bilgi için bkz. [bölgeler](regions.md#standard-and-neural-voices).

| Yerel Ayar | Dil | Cinsiyet | Ses adı |
|--|--|--|--|
| <sup>1</sup>`ar-EG` | Arapça (Mısır) | Kadın | "ar-EG-Hoda" |
| `ar-SA` | Arapça (Suudi Arabistan) | Erkek | "ar-SA-Naayf" |
| `bg-BG` | Bulgarca | Erkek |  "BG-BG-Ivan" |
| `ca-ES` | Katalanca | Kadın |  "CA-ES-HerenaRUS" |
| `cs-CZ` | Çekçe | Erkek | "CS-CZ-Jakub" |
| `da-DK` | Danca | Kadın |  "da-DK-HelleRUS" |
| `de-AT` | Almanca (Avusturya) | Erkek | "on-Michael" |
| `de-CH` | Almanca (İsviçre) | Erkek |  "de CH-Karsten" |
| `de-DE` | Almanca (Almanya) | Kadın |  "de-DE-Hedda" |
|  |  | Kadın | "de, HeddaRUS" |
|  |  | Erkek |  "de-DE-Stefan-Apollo" |
| `el-GR` | Yunanca | Erkek | "el-GR-Stefanos" |
| `en-AU` | İngilizce (Avustralya) | Kadın |  "en-AU-Catherine" |
|  |  | Kadın |  "en-AU-HayleyRUS" |
| `en-CA` | İngilizce (Kanada) | Kadın |  "en-CA-Linda" |
|  |  | Kadın |  "en-CA-Centherrus" |
| `en-GB` | İngilizce (UK) | Kadın |  "en-GB-çiğdem-Apollo" |
|  |  | Kadın |  "en-GB-HazelRUS" |
|  |  | Erkek |  "en-GB-George-Apollo" |
| `en-IE` | İngilizce (İrlanda) | Erkek | "en-IE-kemal" |
| `en-IN` | İngilizce (Hindistan) | Kadın | "en-ın-heçi-Apollo" |
|  |  | Kadın |  "en-basit Yarus" |
|  |  | Erkek |  "en-ın-Ravi-Apollo" |
| `en-US` | İngilizce (ABD) | Kadın |  "en-US-ZiraRUS" |
|  |  | Kadın | "en-US-AriaRUS" |
|  |  | Erkek | "en-US-BenjaminRUS" |
|  |  | Erkek |  "en-US-Guy24kRUS" |
| `es-ES` | İspanyolca (İspanya) | Kadın |  "ES-ES-gamze-Apollo" |
|  |  | Kadın | "ES-ES-HelenaRUS" |
|  |  | Erkek | "ES-ES-Pablo-Apollo" |
| `es-MX` | İspanyolca (Meksika) | Kadın |  "es-MX-Tepdarus" |
|  |  | Erkek | "es-MX-OYUL-Apollo" |
| `fi-FI` | Fince | Kadın | "fi-FI-Heidrus" |
| `fr-CA` | Fransızca (Kanada) | Kadın | "fr-CA-Caroline" |
|  |  | Kadın | "fr-CA-Harmonitorerus" |
| `fr-CH` | Fransızca (İsviçre) | Erkek | "fr-CH-Guildefme" |
| `fr-FR` | Fransızca (Fransa) | Kadın |  "fr-FR-Julie-Apollo" |
|  |  | Kadın |"fr-FR-HortenseRUS" |
|  |  | Erkek |  "fr-FR-Paul-Apollo" |
| `he-IL` | İbranice (Israil) | Erkek |  "he-Il-asaf" |
| `hi-IN` | Hintçe (Hindistan) | Kadın | "HI-IN-Kalpana-Apollo" |
|  |  | Kadın |  "Hi Pana" |
|  |  | Erkek |  "Hi-IN-Hemant" |
| `hr-HR` | Hırvatça | Erkek | "HR-HR-Matej" |
| `hu-HU` | Macarca | Erkek |  "HU-HU-Szacıvacs" |
| `id-ID` | Endonezce | Erkek | "kimlik-KIMLIĞI-Andika" |
| `it-IT` | İtalyanca | Erkek |  "It-IT-Cosımo-Apollo" |
|  |  | Kadın |  "It-IT-LuciaRUS" |
| `ja-JP` | Japonca | Kadın |  "ja-JP-Ayumi-Apollo" |
|  |  | Erkek | "ja-JP-Ichiro-Apollo" |
|  |  | Kadın |  "ja-JP-HarukaRUS" |
| `ko-KR` | Korece | Kadın | "ko-KR-Heamırus" |
| `ms-MY` | Malayca | Erkek |  "MS-MY-Rizwan" |
| `nb-NO` | Norveççe | Kadın |  "NB-hayır-HuldaRUS" |
| `nl-NL` | Felemenkçe | Kadın |  "nl-NL-HannaRUS" |
| `pl-PL` | Lehçe | Kadın |  "pl-PL-PaulinaRUS" |
| `pt-BR` | Portekizce (Brezilya) | Kadın | "PT-BR-HeloisaRUS" |
|  |  | Erkek |  "PT-BR-Daniel-Apollo" |
| `pt-PT` | Portekizce (Portekiz) | Kadın | "PT-PT Helinarus" |
| `ro-RO` | Rumence | Erkek | "RO-RO-Andrei" |
| `ru-RU` | Rusça | Kadın |  "ru-RU-Irina-Apollo" |
|  |  | Erkek | "ru-RU-Palevel-Apollo" |
|  |  | Kadın |  ru-RU-EkaterinaRUS |
| `sk-SK` | Slovakça | Erkek | "SK-SK-Filıp" |
| `sl-SI` | Slovence | Erkek |  "SL-SI-Lado" |
| `sv-SE` | İsveççe | Kadın | "ZF-i-HedvigRUS" |
| `ta-IN` | Tamil dili (Hindistan) | Erkek |  "ta-Valluvar" |
| `te-IN` | Telugu dili (Hindistan) | Kadın |  "te-Chitra" |
| `th-TH` | Tayca | Erkek |  "TH-Pattara" |
| `tr-TR` | Türkçe (Türkiye) | Kadın | "tr-TR-SedaRUS" |
| `vi-VN` | Vietnamca | Erkek |  "VI-VN-a" |
| `zh-CN` | Çince (Mandarin, Basitleştirilmiş) | Kadın |  "zh-CN-Huihuırus" |
|  |  | Kadın | "zh-CN-Yaoyao-Apollo" |
|  |  | Erkek | "zh-CN-Kangkang-Apollo" |
| `zh-HK` | Çince (Cantoneo, geleneksel) | Kadın |  "zh-HK-Tracy-Apollo" |
|  |  | Kadın | "zh-HK-Tracyırus" |
|  |  | Erkek |  "zh-HK-Danny-Apollo" |
| `zh-TW` | Çince (Taiwanese Mandarin) | Kadın |  "zh-TW-Yating-Apollo" |
|  |  | Kadın | "zh-TW-HanHanRUS" |
|  |  | Erkek |  "zh-TW-Zhiwei-Apollo" |

**1** *ar-örneğin modern Standart Arapça (MSA) desteği.*

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
