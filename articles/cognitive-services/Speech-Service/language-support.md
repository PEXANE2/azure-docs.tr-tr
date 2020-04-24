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
ms.openlocfilehash: 0b6fea381bd6b4aa8ad3e7061e6f632176c41033
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82113842"
---
# <a name="language-and-voice-support-for-the-speech-service"></a>Konuşma hizmeti için dil ve ses desteği

Dil desteği, konuşma hizmeti işlevselliğine göre farklılık gösterir. Aşağıdaki tablolar, [konuşmadan metne](#speech-to-text), [metinden konuşmaya](#text-to-speech)ve [konuşma çevirisi](#speech-translation) hizmet teklifleri için dil desteğini özetler.

## <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

Hem Microsoft konuşma SDK 'Sı hem de REST API aşağıdaki dilleri (yerel ayarlar) destekler. Doğruluğu artırmak için, bir dilin bir alt kümesi için, ses + ınsan etiketli yazılı döküm veya Ilgili metin: cümleler karşıya yüklenirken özelleştirme sunulur. Telaffuz özelleştirmesi Şu anda yalnızca ve `en-US` `de-DE`için kullanılabilir. [Özelleştirme hakkında](how-to-custom-speech.md)daha fazla bilgi edinin.

<!--
To get the AM and ML bits:
https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20models%3A/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.ai -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

| Yerel Ayar  | Dil                          | Destekleniyor | Özelleştirmeler                                    |
|---------|-----------------------------------|-----------|---------------------------------------------------|
| `ar-AE` | Arapça (UAE)                      | Yes       | Hayır                                                |
| `ar-BH` | Arapça (Bahreyn), modern standart | Yes       | Dil modeli                                    |
| `ar-EG` | Arapça (Mısır)                    | Yes       | Dil modeli                                    |
| `ar-KW` | Arapça (Kuveyt)                   | Yes       | Hayır                                                |
| `ar-QA` | Arapça (Qtor)                    | Yes       | Hayır                                                |
| `ar-SA` | Arapça (Suudi Arabistan)             | Yes       | Hayır                                                |
| `ar-SY` | Arapça (Suriye)                    | Yes       | Dil modeli                                    |
| `ca-ES` | Katalanca                           | Yes       | Dil modeli                                    |
| `da-DK` | Danca (Danimarka)                  | Yes       | Dil modeli                                    |
| `de-DE` | Almanca (Almanya)                  | Yes       | Akustik model<br>Dil modeli<br>İşi |
| `en-AU` | İngilizce (Avustralya)               | Yes       | Akustik model<br>Dil modeli                  |
| `en-CA` | İngilizce (Kanada)                  | Yes       | Akustik model<br>Dil modeli                  |
| `en-GB` | İngilizce (İngiltere)          | Yes       | Akustik model<br>Dil modeli<br>İşi |
| `en-IN` | İngilizce (Hindistan)                   | Yes       | Akustik model<br>Dil modeli                  |
| `en-NZ` | İngilizce (Yeni Zelanda)             | Yes       | Akustik model<br>Dil modeli                  |
| `en-US` | İngilizce (ABD)           | Yes       | Akustik model<br>Dil modeli<br>İşi |
| `es-ES` | İspanyolca (İspanya)                   | Yes       | Akustik model<br>Dil modeli                  |
| `es-MX` | İspanyolca (Meksika)                  | Yes       | Akustik model<br>Dil modeli                  |
| `fi-FI` | Fince (Finlandiya)                 | Yes       | Dil modeli                                    |
| `fr-CA` | Fransızca (Kanada)                   | Yes       | Akustik model<br>Dil modeli                  |
| `fr-FR` | Fransızca (Fransa)                   | Yes       | Akustik model<br>Dil modeli<br>İşi |
| `gu-IN` | Gucerat dili (Hindistan)                 | Yes       | Dil modeli                                    |
| `hi-IN` | Hintçe (Hindistan)                     | Yes       | Akustik model<br>Dil modeli                  |
| `it-IT` | İtalyanca (İtalya)                   | Yes       | Akustik model<br>Dil modeli<br>İşi |
| `ja-JP` | Japonca (Japonya)                  | Yes       | Dil modeli                                    |
| `ko-KR` | Korece (Kore)                    | Yes       | Dil modeli                                    |
| `mr-IN` | Marathi dili (Hindistan)                   | Yes       | Dil modeli                                    |
| `nb-NO` | Norveççe (Bokmål) (Norveç)       | Yes       | Dil modeli                                    |
| `nl-NL` | Felemenkçe (Hollanda)               | Yes       | Dil modeli                                    |
| `pl-PL` | Lehçe (Polonya)                   | Yes       | Dil modeli                                    |
| `pt-BR` | Portekizce (Brezilya)               | Yes       | Akustik model<br>Dil modeli<br>İşi |
| `pt-PT` | Portekizce (Portekiz)             | Yes       | Dil modeli                                    |
| `ru-RU` | Rusça (Rusya)                  | Yes       | Akustik model<br>Dil modeli                  |
| `sv-SE` | İsveççe (İsviçre)                  | Yes       | Dil modeli                                    |
| `ta-IN` | Tamil dili (Hindistan)                     | Yes       | Dil modeli                                    |
| `te-IN` | Telugu dili (Hindistan)                    | Yes       | Hayır                                                |
| `th-TH` | Tayca (Tayland)                   | Yes       | Hayır                                                |
| `tr-TR` | Türkçe (Türkiye)                  | Yes       | Hayır                                                |
| `zh-CN` | Çince (Mandarin, Basitleştirilmiş)    | Yes       | Akustik model<br>Dil modeli                  |
| `zh-HK` | Çince (Cantoneo, geleneksel)  | Yes       | Dil modeli                                    |
| `zh-TW` | Çince (Taiwanese Mandarin)      | Yes       | Dil modeli                                    |

## <a name="text-to-speech"></a>Metin okuma

Hem Microsoft konuşma SDK 'Sı hem de REST API 'Leri, her biri yerel ayar ile tanımlanan belirli bir dili ve diyalekt destekleyen bu sesleri destekler.

> [!IMPORTANT]
> Fiyatlandırma, standart, özel ve sinir sesleri için farklılık gösterir. Daha fazla bilgi için lütfen [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) sayfasını ziyaret edin.

### <a name="neural-voices"></a>Sinir sesleri

Sinir metin okuma, derin sinir ağlarla desteklenen yeni bir konuşma türü türüdür. Bir sinir sesi kullanırken, sentezlenmiş konuşma, insan kayıtlarından neredeyse ayırt edilemez.

Sinir sesleri, chatbots ve ses yardımcılarıyla daha doğal ve etkileyici bir şekilde etkileşim kurmak, e-kitaplar gibi dijital metinleri audiobooks 'a dönüştürmek ve oto içi gezinti sistemlerini geliştirmek için kullanılabilir. İnsan benzeri doğal Prosody ve sözcüklerin bir kısmını temizleyerek, sinir seslerde kullanıcılar AI sistemleriyle etkileşim kurarken büyük ölçüde dinleme performansını önemli ölçüde azaltır.

Bölgesel kullanılabilirlik hakkında daha fazla bilgi için bkz. [bölgeler](regions.md#standard-and-neural-voices).

| Yerel Ayar  | Dil            | Cinsiyet | Tam hizmet adı eşleme                                               | Kısa ses adı        |
|---------|---------------------|--------|-------------------------------------------------------------------------|-------------------------|
| `de-DE` | Almanca (Almanya)    | Kadın | "Microsoft Server konuşma Metin Okuma sesi (de-DE, KatjaNeural)"     | "de-KatjaNeural"     |
| `en-US` | İngilizce (ABD)        | Kadın | "Microsoft Server konuşma Metin Okuma sesi (en-US, AriaNeural)"      | "en-US-AriaNeural"      |
| `en-US` | İngilizce (ABD)        | Erkek   | "Microsoft Server konuşma Metin Okuma sesi (en-US, GuyNeural)"       | "en-US-GuyNeural"       |
| `it-IT` | İtalyanca (İtalya)     | Kadın | "Microsoft Server konuşma Metin Okuma sesi (It-IT, ElsaNeural)"      | "It-IT-ElsaNeural"      |
| `pt-BR` | Portekizce (Brezilya) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (PT-BR, FranciscaNeural)" | "PT-BR-FranciscaNeural" |
| `zh-CN` | Çince (Mandarin, Basitleştirilmiş)  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (zh-CN, XiaoxiaoNeural)"  | "zh-CN-XiaoxiaoNeural"  |

> [!IMPORTANT]
> `en-US-JessaNeural` Ses olarak `en-US-AriaNeural`değiştirildi. Daha önce "Jessa" kullandıysanız, "Aria" öğesine dönüştürün.

Sinir seslerini nasıl yapılandıracağınızı ve ayarlayabileceğinizi öğrenmek için bkz. [konuşma sen, biçimlendirme dili](speech-synthesis-markup.md#adjust-speaking-styles).

> [!TIP]
> Konuşma senssıs isteklerinizin tam hizmet adı eşlemesini veya kısa ses adını kullanabilirsiniz.

### <a name="standard-voices"></a>Standart sesler

75 ' den fazla standart ses, daha fazla 45 dilde ve yerel ayarlarda bulunabilir ve bu da metni sentezleştirilmiş konuşmaya dönüştürmenize olanak tanır. Bölgesel kullanılabilirlik hakkında daha fazla bilgi için bkz. [bölgeler](regions.md#standard-and-neural-voices).

| Yerel Ayar | Dil | Cinsiyet | Tam hizmet adı eşleme | Kısa ad |
|--|--|--|--|--|
| <sup>1</sup>`ar-EG` | Arapça (Mısır) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (ar-EG, Hoda)" | "ar-EG-Hoda" |
| `ar-SA` | Arapça (Suudi Arabistan) | Erkek | "Microsoft Server konuşma Metin Okuma sesi (ar-SA, Naayf)" | "ar-SA-Naayf" |
| `bg-BG` | Bulgarca | Erkek | "Microsoft Server konuşma Metin Okuma sesi (BG-BG, Ivan)" | "BG-BG-Ivan" |
| `ca-ES` | Katalanca (İspanya) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (CA-ES, HerenaRUS)" | "CA-ES-HerenaRUS" |
| `cs-CZ` | Çekçe | Erkek | "Microsoft Server konuşma Metin Okuma sesi (CS-CZ, Jakub)" | "CS-CZ-Jakub" |
| `da-DK` | Danca | Kadın | "Microsoft Server konuşma Metin Okuma sesi (da-DK, HelleRUS)" | "da-DK-HelleRUS" |
| `de-AT` | Almanca (Avusturya) | Erkek | "Microsoft Server konuşma Metin Okuma sesi (de-AT, Michael)" | "on-Michael" |
| `de-CH` | Almanca (İsviçre) | Erkek | "Microsoft Server konuşma Metin Okuma sesi (de CH, Karsten)" | "de CH-Karsten" |
| `de-DE` | Almanca (Almanya) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (de-DE, Hedda)" | "de-DE-Hedda" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (de-DE, HeddaRUS)" | "de, HeddaRUS" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (de-DE, Stefan, Apollo)" | "de-DE-Stefan-Apollo" |
| `el-GR` | Yunanca | Erkek | "Microsoft Server konuşma Metin Okuma sesi (el-GR, Stefanos)" | "el-GR-Stefanos" |
| `en-AU` | İngilizce (Avustralya) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (en-AU, Catherine)" | "en-AU-Catherine" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (en-AU, HayleyRUS)" | "en-AU-HayleyRUS" |
| `en-CA` | İngilizce (Kanada) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (en-CA, Linda)" | "en-CA-Linda" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (en-CA, Centherru)" | "en-CA-Centherrus" |
| `en-GB` | İngilizce (UK) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (en-GB, Çiğdem, Apollo)" | "en-GB-çiğdem-Apollo" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (en-GB, tehlikeli Elrus)" | "en-GB-HazelRUS" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (en-GB, George, Apollo)" | "en-GB-George-Apollo" |
| `en-IE` | İngilizce (İrlanda) | Erkek | "Microsoft Server konuşma Metin Okuma sesi (en-IE, kemal)" | "en-IE-kemal" |
| `en-IN` | İngilizce (Hindistan) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (en-ın, Heera, Apollo)" | "en-ın-heçi-Apollo" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (en-ın, PriyaRUS)" | "en-basit Yarus" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (en-ın, Ravi, Apollo)" | "en-ın-Ravi-Apollo" |
| `en-US` | İngilizce (ABD) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (en-US, ZiraRUS)" | "en-US-ZiraRUS" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (en-US, AriaRUS)" | "en-US-AriaRUS" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (en-US, BenjaminRUS)" | "en-US-BenjaminRUS" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (en-US, Guy24kRUS)" | "en-US-Guy24kRUS" |
| `es-ES` | İspanyolca (İspanya) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (ES-ES, Gamze, Apollo)" | "ES-ES-gamze-Apollo" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (ES-ES, HelenaRUS)" | "ES-ES-HelenaRUS" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (ES-ES, Pablo, Apollo)" | "ES-ES-Pablo-Apollo" |
| `es-MX` | İspanyolca (Meksika) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (es-MX, Tepdarus)" | "es-MX-Tepdarus" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (es-MX, Rayul, Apollo)" | "es-MX-OYUL-Apollo" |
| `fi-FI` | Fince | Kadın | "Microsoft Server konuşma Metin Okuma sesi (FI-FI, Heidrus)" | "fi-FI-Heidrus" |
| `fr-CA` | Fransızca (Kanada) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (fr-CA, Caroline)" | "fr-CA-Caroline" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (fr-CA, Harmonitorerus)" | "fr-CA-Harmonitorerus" |
| `fr-CH` | Fransızca (İsviçre) | Erkek | "Microsoft Server konuşma Metin Okuma sesi (fr-CH, Guldefme)" | "fr-CH-Guildefme" |
| `fr-FR` | Fransızca (Fransa) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (fr-FR, Julie, Apollo)" | "fr-FR-Julie-Apollo" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (fr-FR, HortenseRUS)" | "fr-FR-HortenseRUS" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (fr-FR, Paul, Apollo)" | "fr-FR-Paul-Apollo" |
| `he-IL` | İbranice (Israil) | Erkek | "Microsoft Server konuşma Metin Okuma sesi (he-Il, Asaf)" | "he-Il-asaf" |
| `hi-IN` | Hintçe (Hindistan) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (Hi-ın, Kalpana, Apollo)" | "HI-IN-Kalpana-Apollo" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (Hi-ın, Kalpana)" | "Hi Pana" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (Hi-IN, Hemant)" | "Hi-IN-Hemant" |
| `hr-HR` | Hırvatça | Erkek | "Microsoft Server konuşma Metin Okuma sesi (HR-HR, Matej)" | "HR-HR-Matej" |
| `hu-HU` | Macarca | Erkek | "Microsoft Server konuşma Metin Okuma sesi (HU-HU, Szacıvacs)" | "HU-HU-Szacıvacs" |
| `id-ID` | Endonezce | Erkek | "Microsoft Server konuşma Metin Okuma sesi (kimlik KIMLIĞI, Andika)" | "kimlik-KIMLIĞI-Andika" |
| `it-IT` | İtalyanca | Erkek | "Microsoft Server konuşma Metin Okuma sesi (It-IT, Cosımo, Apollo)" | "It-IT-Cosımo-Apollo" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (It-IT, LuciaRUS)" | "It-IT-LuciaRUS" |
| `ja-JP` | Japonca | Kadın | "Microsoft Server konuşma Metin Okuma sesi (ja-JP, Ayumi, Apollo)" | "ja-JP-Ayumi-Apollo" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (ja-JP, Ichiro, Apollo)" | "ja-JP-Ichiro-Apollo" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (ja-JP, HarukaRUS)" | "ja-JP-HarukaRUS" |
| `ko-KR` | Korece | Kadın | "Microsoft Server konuşma Metin Okuma sesi (ko-KR, Heamırus)" | "ko-KR-Heamırus" |
| `ms-MY` | Malayca | Erkek | "Microsoft Server konuşma Metin Okuma sesi (MS-MY, Rizwan)" | "MS-MY-Rizwan" |
| `nb-NO` | Norveççe | Kadın | "Microsoft Server konuşma Metin Okuma sesi (NB-NO, HuldaRUS)" | "NB-hayır-HuldaRUS" |
| `nl-NL` | Felemenkçe | Kadın | "Microsoft Server konuşma Metin Okuma sesi (nl-NL, HannaRUS)" | "nl-NL-HannaRUS" |
| `pl-PL` | Lehçe | Kadın | "Microsoft Server konuşma Metin Okuma sesi (PL-PL, PaulinaRUS)" | "pl-PL-PaulinaRUS" |
| `pt-BR` | Portekizce (Brezilya) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (PT-BR, HeloisaRUS)" | "PT-BR-HeloisaRUS" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (PT-BR, Daniel, Apollo)" | "PT-BR-Daniel-Apollo" |
| `pt-PT` | Portekizce (Portekiz) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (pt-PT, HeliaRUS)" | "PT-PT Helinarus" |
| `ro-RO` | Rumence | Erkek | "Microsoft Server konuşma Metin Okuma sesi (RO-RO, ANDREI)" | "RO-RO-Andrei" |
| `ru-RU` | Rusça | Kadın | "Microsoft Server konuşma Metin Okuma sesi (ru-RU, Irina, Apollo)" | "ru-RU-Irina-Apollo" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (ru-RU, Pabol, Apollo)" | "ru-RU-Palevel-Apollo" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (ru-RU, EkaterinaRUS)" | ru-RU-EkaterinaRUS |
| `sk-SK` | Slovakça | Erkek | "Microsoft Server konuşma Metin Okuma sesi (SK-SK, Filıp)" | "SK-SK-Filıp" |
| `sl-SI` | Slovence | Erkek | "Microsoft Server konuşma Metin Okuma sesi (SL-sı, Lado)" | "SL-SI-Lado" |
| `sv-SE` | İsveççe | Kadın | "Microsoft Server konuşma Metin Okuma sesi (ZF-o, HedvigRUS)" | "ZF-i-HedvigRUS" |
| `ta-IN` | Tamil dili (Hindistan) | Erkek | "Microsoft Server konuşma Metin Okuma sesi (ta-ın, Valluvar)" | "ta-Valluvar" |
| `te-IN` | Telugu dili (Hindistan) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (te-ın, Chitra)" | "te-Chitra" |
| `th-TH` | Tayca | Erkek | "Microsoft Server konuşma Metin Okuma sesi (TH, Pattara)" | "TH-Pattara" |
| `tr-TR` | Türkçe (Türkiye) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (tr-TR, SedaRUS)" | "tr-TR-SedaRUS" |
| `vi-VN` | Vietnamca | Erkek | "Microsoft Server konuşma Metin Okuma sesi (VN, a)" | "VI-VN-a" |
| `zh-CN` | Çince (Mandarin, Basitleştirilmiş) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (zh-CN, Huihuırus)" | "zh-CN-Huihuırus" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (zh-CN, Yaoyao, Apollo)" | "zh-CN-Yaoyao-Apollo" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (zh-CN, Kangkang, Apollo)" | "zh-CN-Kangkang-Apollo" |
| `zh-HK` | Çince (Cantoneo, geleneksel) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (zh-HK, Tracy, Apollo)" | "zh-HK-Tracy-Apollo" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (zh-HK, Tracyıru)" | "zh-HK-Tracyırus" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (zh-HK, Danny, Apollo)" | "zh-HK-Danny-Apollo" |
| `zh-TW` | Çince (Taiwanese Mandarin) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (zh-TW, Yadırıcı, Apollo)" | "zh-TW-Yating-Apollo" |
|  |  | Kadın | "Microsoft Server konuşma Metin Okuma sesi (zh-TW, Hanhanru)" | "zh-TW-HanHanRUS" |
|  |  | Erkek | "Microsoft Server konuşma Metin Okuma sesi (zh-TW, Zhiwei, Apollo)" | "zh-TW-Zhiwei-Apollo" |

**1** *ar-örneğin modern Standart Arapça (MSA) desteği.*

> [!IMPORTANT]
> `en-US-Jessa` Ses olarak `en-US-Aria`değiştirildi. Daha önce "Jessa" kullandıysanız, "Aria" öğesine dönüştürün.

> [!TIP]
> Konuşma senssıs isteklerinizin tam hizmet adı eşlemesini veya kısa ses adını kullanabilirsiniz.

### <a name="customization"></a>Özelleştirme

, `de-DE` `en-GB` `fr-FR` `it-IT` `pt-BR`,,, `zh-CN`,,, Ve için ses özelleştirmesi kullanılabilir. `en-IN` `en-US` `es-MX` Özel bir ses modeli eğitmeniz gereken eğitim verileriyle eşleşen doğru yerel ayarı seçin. Örneğin, kullandığınız kayıt verileri Ingiliz alfabesindeki bir vurgu ile konuşulursam, seçeneğini belirleyin `en-GB`.

> [!NOTE]
> Çince-Ingilizce bı-dil dışında, özel seste bı-dilli model eğitimini desteklemiyoruz. Ingilizce 'ye de konuşarak bir Çince ses alıştırması yapmak istiyorsanız "Çince-Ingilizce çift dilli" seçeneğini belirleyin. Tüm yerel ayarlarda sesli eğitim, `en-US` her türlü eğitim verisi ile başlayabileceğiniz ve `zh-CN` dışında bir 2000 + utterations veri kümesiyle başlar.

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
| Klingon                 | `tlh`         |
| Klingon (plqaD)         | `tlh-Qaak`    |
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

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmeti deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [Bkz. C 'de konuşmayı tanıma #](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
