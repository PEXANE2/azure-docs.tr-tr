---
title: Dil desteği - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti, konuşma çevirisinin yanı sıra konuşma-metin ve metinden konuşmaya dönüştürme için çok sayıda dili destekler. Bu makalede, hizmet özelliğine göre dil desteğinin kapsamlı bir listesi sağlanmıştır.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: dapine
ms.custom: seodec18
ms.openlocfilehash: d01ab60790311649e424a98d5a08c6af0bca90f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336050"
---
# <a name="language-and-voice-support-for-the-speech-service"></a>Konuşma hizmeti için dil ve ses desteği

Dil desteği Konuşma hizmeti işlevine göre değişir. Aşağıdaki [tablolarda Konuşma-metin,](#speech-to-text) [Metinden konuşmaya](#text-to-speech)ve [Konuşma çeviri](#speech-translation) hizmeti teklifleri için dil desteği özetlenmiştir.

## <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

Hem Microsoft Speech SDK hem de REST API aşağıdaki dilleri (yerel olarak) destekler. Doğruluğu artırmak için, Ses + İnsan etiketli Transkriptler veya İlgili Metin: Cümleler yükleyerek dillerin bir alt kümesi için özelleştirme sunulur. Telaffuz özelleştirme şu anda yalnızca `en-US` ve `de-DE`. Özelleştirme hakkında daha fazla bilgi [için burada.](how-to-custom-speech.md)

<!--
To get the AM and ML bits:
https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20models%3A/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.ai -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

| Yerel Ayar  | Dil                          | Destekleniyor | Özelleştirmeler                                    |
|---------|-----------------------------------|-----------|---------------------------------------------------|
| `ar-AE` | Arapça (BAE)                      | Evet       | Hayır                                                |
| `ar-BH` | Arapça (Bahreyn), modern standart | Evet       | Dil modeli                                    |
| `ar-EG` | Arapça (Mısır)                    | Evet       | Dil modeli                                    |
| `ar-KW` | Arapça (Kuveyt)                   | Evet       | Hayır                                                |
| `ar-QA` | Arapça (Katar)                    | Evet       | Hayır                                                |
| `ar-SA` | Arapça (Suudi Arabistan)             | Evet       | Hayır                                                |
| `ar-SY` | Arapça (Suriye)                    | Evet       | Dil modeli                                    |
| `ca-ES` | Katalanca                           | Evet       | Dil modeli                                    |
| `da-DK` | Danca (Danimarka)                  | Evet       | Dil modeli                                    |
| `de-DE` | Almanca (Almanya)                  | Evet       | Akustik model<br>Dil modeli<br>Telaffuzu |
| `en-AU` | İngilizce (Avustralya)               | Evet       | Akustik model<br>Dil modeli                  |
| `en-CA` | İngilizce (Kanada)                  | Evet       | Akustik model<br>Dil modeli                  |
| `en-GB` | İngilizce (İngiltere)          | Evet       | Akustik model<br>Dil modeli<br>Telaffuzu |
| `en-IN` | İngilizce (Hindistan)                   | Evet       | Akustik model<br>Dil modeli                  |
| `en-NZ` | İngilizce (Yeni Zelanda)             | Evet       | Akustik model<br>Dil modeli                  |
| `en-US` | İngilizce (ABD)           | Evet       | Akustik model<br>Dil modeli<br>Telaffuzu |
| `es-ES` | İspanyolca (İspanya)                   | Evet       | Akustik model<br>Dil modeli                  |
| `es-MX` | İspanyolca (Meksika)                  | Evet       | Akustik model<br>Dil modeli                  |
| `fi-FI` | Fince (Finlandiya)                 | Evet       | Dil modeli                                    |
| `fr-CA` | Fransızca (Kanada)                   | Evet       | Akustik model<br>Dil modeli                  |
| `fr-FR` | Fransızca (Fransa)                   | Evet       | Akustik model<br>Dil modeli<br>Telaffuzu |
| `gu-IN` | Gujarati (Hint)                 | Evet       | Dil modeli                                    |
| `hi-IN` | Hintçe (Hindistan)                     | Evet       | Akustik model<br>Dil modeli                  |
| `it-IT` | İtalyanca (İtalya)                   | Evet       | Akustik model<br>Dil modeli<br>Telaffuzu |
| `ja-JP` | Japonca (Japonya)                  | Evet       | Dil modeli                                    |
| `ko-KR` | Korece (Kore)                    | Evet       | Dil modeli                                    |
| `mr-IN` | Marathi (Hindistan)                   | Evet       | Dil modeli                                    |
| `nb-NO` | Norveççe (Bokmål) (Norveç)       | Evet       | Dil modeli                                    |
| `nl-NL` | Felemenkçe (Hollanda)               | Evet       | Dil modeli                                    |
| `pl-PL` | Lehçe (Polonya)                   | Evet       | Dil modeli                                    |
| `pt-BR` | Portekizce (Brezilya)               | Evet       | Akustik model<br>Dil modeli<br>Telaffuzu |
| `pt-PT` | Portekizce (Portekiz)             | Evet       | Dil modeli                                    |
| `ru-RU` | Rusça (Rusya)                  | Evet       | Akustik model<br>Dil modeli                  |
| `sv-SE` | İsveççe (İsviçre)                  | Evet       | Dil modeli                                    |
| `ta-IN` | Tamil (Hindistan)                     | Evet       | Dil modeli                                    |
| `te-IN` | Telugu (Hindistan)                    | Evet       | Hayır                                                |
| `th-TH` | Tayca (Tayland)                   | Evet       | Hayır                                                |
| `tr-TR` | Türkçe (Türkiye)                  | Evet       | Hayır                                                |
| `zh-CN` | Çince (Mandarin, basitleştirilmiş)    | Evet       | Akustik model<br>Dil modeli                  |
| `zh-HK` | Çince (Kanton, Geleneksel)  | Evet       | Dil modeli                                    |
| `zh-TW` | Çince (Tayvan Mandarin)      | Evet       | Dil modeli                                    |

## <a name="text-to-speech"></a>Metin okuma

Hem Microsoft Speech SDK hem de REST API'leri, her biri yerel olarak tanımlanan belirli bir dili ve lehçeyi destekleyen bu sesleri destekler.

> [!IMPORTANT]
> Fiyatlandırma standart, özel ve sinirsel sesler için değişir. Daha fazla bilgi için lütfen [Fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) sayfasını ziyaret edin.

### <a name="neural-voices"></a>Sinirsel sesler

Nöral metinden konuşmaya derin sinir ağları tarafından desteklenen yeni bir konuşma sentezi türüdür. Sinirsel bir ses kullanırken, sentezlenmiş konuşma insan kayıtlarından neredeyse ayırt edilemez.

Sinirsel sesler chatbots ve ses asistanları ile etkileşimleri daha doğal ve ilgi çekici hale getirmek için kullanılabilir, sesli kitaplar içine e-kitap gibi dijital metinlerdönüştürmek ve araç içi navigasyon sistemleri geliştirmek. İnsana benzer doğal prosody ve kelimelerin net eklemleme ile, nöral sesler önemli ölçüde kullanıcıların AI sistemleri ile etkileşim dinleme yorgunluğunu azaltır.

Bölgesel kullanılabilirlik hakkında daha fazla bilgi için [bölgelere](regions.md#standard-and-neural-voices)bakın.

| Yerel Ayar  | Dil            | Cinsiyet | Tam hizmet adı eşleme                                               | Kısa ses adı        |
|---------|---------------------|--------|-------------------------------------------------------------------------|-------------------------|
| `de-DE` | Almanca (Almanya)    | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (de-DE, KatjaNeural)"     | "de-DE-KatjaNeural"     |
| `en-US` | İngilizce (ABD)        | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (tr-US, AriaNeural)"      | "en-ABD-AriaNeural"      |
| `en-US` | İngilizce (ABD)        | Erkek   | "Microsoft Server Konuşma Metni Konuşma Ses (tr-US, GuyNeural)"       | "en-ABD-GuyNeural"       |
| `it-IT` | İtalyanca (İtalya)     | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (it-IT, ElsaNeural)"      | "it-IT-ElsaNeural"      |
| `pt-BR` | Portekizce (Brezilya) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (pt-BR, FranciscaNeural)" | "pt-BR-FranciscaNeural" |
| `zh-CN` | Çince (Mandarin, basitleştirilmiş)  | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (zh-CN, XiaoxiaoNeural)"  | "zh-CN-XiaoxiaoNeural"  |

> [!IMPORTANT]
> Ses `en-US-JessaNeural` . `en-US-AriaNeural` Daha önce "Jessa" kullanıyorsanız, "Aria" olarak değiştirin.

Nöral sesleri nasıl yapılandırabileceğinizi ve ayarlayabildiğinizi öğrenmek için [Konuşma sentezi biçimlendirme diline](speech-synthesis-markup.md#adjust-speaking-styles)bakın.

> [!TIP]
> Konuşma sentezi isteklerinizde tam hizmet adı eşlemesini veya kısa ses adını kullanabilirsiniz.

### <a name="standard-voices"></a>Standart sesler

Metni sentezlenmiş konuşmaya dönüştürmenize olanak tanıyan 45'ten fazla dil ve yerel dilde 75'ten fazla standart ses mevcuttur. Bölgesel kullanılabilirlik hakkında daha fazla bilgi için [bölgelere](regions.md#standard-and-neural-voices)bakın.

| Yerel Ayar | Dil | Cinsiyet | Tam hizmet adı eşleme | Kısa ad |
|--|--|--|--|--|
| <sup>1</sup>`ar-EG` | Arapça (Mısır) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (ar-EG, Hoda)" | "ar-EG-Hoda" |
| `ar-SA` | Arapça (Suudi Arabistan) | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (ar-SA, Naayf)" | "ar-SA-Naayf" |
| `bg-BG` | Bulgarca | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (bg-BG, Ivan)" | "bg-BG-Ivan" |
| `ca-ES` | Katalanca (İspanya) | Kadın | "Microsoft Server Konuşma Metninden Konuşma Sesine (ca-ES, HerenaRUS)" | "ca-ES-HerenaRUS" |
| `cs-CZ` | Çekçe | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (cs-CZ, Jakub)" | "cs-CZ-Jakub" |
| `da-DK` | Danca | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (da-DK, HelleRUS)" | "da-DK-HelleRUS" |
| `de-AT` | Almanca (Avusturya) | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (de-AT, Michael)" | "de-AT-Michael" |
| `de-CH` | Almanca (İsviçre) | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (de-CH, Karsten)" | "de-CH-Karsten" |
| `de-DE` | Almanca (Almanya) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (de-DE, Hedda)" | "de-DE-Hedda" |
|  |  | Kadın | "Microsoft Server Konuşma Metni Nden Konuşma Sesine (de-DE, HeddaRUS)" | "de-DE-HeddaRUS" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (de-DE, Stefan, Apollo)" | "de-DE-Stefan-Apollo" |
| `el-GR` | Yunanca | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (el-GR, Stefanos)" | "el-GR-Stefanos" |
| `en-AU` | İngilizce (Avustralya) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (tr-AU, Catherine)" | "en-AU-Catherine" |
|  |  | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (tr-AU, HayleyRUS)" | "en-AU-HayleyRUS" |
| `en-CA` | İngilizce (Kanada) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (tr-CA, Linda)" | "en-CA-Linda" |
|  |  | Kadın | "Microsoft Server Konuşma Metninden Konuşma Sesine (tr-CA, HeatherRUS)" | "en-CA-HeatherRUS" |
| `en-GB` | İngilizce (İngiltere) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (tr-GB, Susan, Apollo)" | "en-GB-Susan-Apollo" |
|  |  | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (tr-GB, HazelRUS)" | "en-GB-HazelRUS" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (tr-GB, George, Apollo)" | "en-GB-George-Apollo" |
| `en-IE` | İngilizce (İrlanda) | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (tr-IE, Kemal)" | "en-IE-Sean" |
| `en-IN` | İngilizce (Hindistan) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (en-IN, Heera, Apollo)" | "en-IN-Heera-Apollo" |
|  |  | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (tr-IN, PriyaRUS)" | "en-IN-PriyaRUS" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (en-IN, Ravi, Apollo)" | "en-IN-Ravi-Apollo" |
| `en-US` | İngilizce (ABD) | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (tr-US, ZiraRUS)" | "en-ABD-ZiraRUS" |
|  |  | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (tr-US, AriaRUS)" | "en-ABD-AriaRUS" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Nden Konuşma Sesine (tr-US, BenjaminRUS)" | "en-ABD-BenjaminRUS" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (tr-US, Guy24kRUS)" | "en-ABD-Guy24kRUS" |
| `es-ES` | İspanyolca (İspanya) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (es-ES, Laura, Apollo)" | "es-ES-Laura-Apollo" |
|  |  | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (es-ES, HelenaRUS)" | "es-ES-HelenaRUS" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (es-ES, Pablo, Apollo)" | "es-ES-Pablo-Apollo" |
| `es-MX` | İspanyolca (Meksika) | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (es-MX, HildaRUS)" | "es-MX-HildaRUS" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (es-MX, Raul, Apollo)" | "es-MX-Raul-Apollo" |
| `fi-FI` | Fince | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (fi-FI, HeidiRUS)" | "fi-FI-HeidiRUS" |
| `fr-CA` | Fransızca (Kanada) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (fr-CA, Caroline)" | "fr-CA-Caroline" |
|  |  | Kadın | "Microsoft Server Konuşma Metninden Konuşma Sesine (fr-CA, HarmonieRUS)" | "fr-CA-HarmonieRUS" |
| `fr-CH` | Fransızca (İsviçre) | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (fr-CH, Guillaume)" | "fr-CH-Guillaume" |
| `fr-FR` | Fransızca (Fransa) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (fr-FR, Julie, Apollo)" | "fr-FR-Julie-Apollo" |
|  |  | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (fr-FR, HortenseRUS)" | "fr-FR-HortenseRUS" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (fr-FR, Paul, Apollo)" | "fr-FR-Paul-Apollo" |
| `he-IL` | İbranice (İsrail) | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (he-IL, Asaf)" | "he-IL-Asaf" |
| `hi-IN` | Hintçe (Hindistan) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (hi-IN, Kalpana, Apollo)" | "hi-IN-Kalpana-Apollo" |
|  |  | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (hi-IN, Kalpana)" | "hi-IN-Kalpana" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (hi-IN, Hemant)" | "hi-IN-Hemant" |
| `hr-HR` | Hırvatça | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (hr-HR, Matej)" | "hr-HR-Matej" |
| `hu-HU` | Macarca | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (hu-HU, Szabolcs)" | "hu-HU-Szabolcs" |
| `id-ID` | Endonezce | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (id-ID, Andika)" | "id-id-Andika" |
| `it-IT` | İtalyanca | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (it-IT, Cosimo, Apollo)" | "it-IT-Cosimo-Apollo" |
|  |  | Kadın | "Microsoft Server Konuşma Metni Nden Konuşma Sesine (it-IT, LuciaRUS)" | "it-IT-LuciaRUS" |
| `ja-JP` | Japonca | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (ja-JP, Ayumi, Apollo)" | "ja-JP-Ayumi-Apollo" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (ja-JP, Ichiro, Apollo)" | "ja-JP-Ichiro-Apollo" |
|  |  | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (ja-JP, HarukaRUS)" | "ja-JP-HarukaRUS" |
| `ko-KR` | Korece | Kadın | "Microsoft Server Konuşma Metni Nden Konuşma Sesine (ko-KR, HeamiRUS)" | "ko-KR-HeamiRUS" |
| `ms-MY` | Malayca | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (ms-MY, Rizwan)" | "ms-MY-Rizwan" |
| `nb-NO` | Norveççe | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (nb-NO, HuldaRUS)" | "nb-NO-HuldaRUS" |
| `nl-NL` | Felemenkçe | Kadın | "Microsoft Server Konuşma Metni Nden Konuşma Sesine (nl-NL, HannaRUS)" | "nl-NL-HannaRUS" |
| `pl-PL` | Lehçe | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (pl-PL, PaulinaRUS)" | "pl-PL-PaulinaRUS" |
| `pt-BR` | Portekizce (Brezilya) | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (pt-BR, HeloisaRUS)" | "pt-BR-HeloisaRUS" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (pt-BR, Daniel, Apollo)" | "pt-BR-Daniel-Apollo" |
| `pt-PT` | Portekizce (Portekiz) | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (pt-PT, HeliaRUS)" | "pt-PT-HeliaRUS" |
| `ro-RO` | Rumence | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (ro-RO, Andrei)" | "ro-RO-Andrei" |
| `ru-RU` | Rusça | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (ru-RU, Irina, Apollo)" | "ru-RU-Irina-Apollo" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (ru-RU, Pavel, Apollo)" | "ru-RU-Pavel-Apollo" |
|  |  | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (ru-RU, EkaterinaRUS)" | ru-RU-EkaterinaRUS |
| `sk-SK` | Slovakça | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (sk-SK, Filip)" | "sk-SK-Filip" |
| `sl-SI` | Slovence | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (sl-SI, Lado)" | "sl-SI-Lado" |
| `sv-SE` | İsveççe | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (sv-SE, HedvigRUS)" | "sv-SE-HedvigRUS" |
| `ta-IN` | Tamil (Hindistan) | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (ta-IN, Valluvar)" | "ta-IN-Valluvar" |
| `te-IN` | Telugu (Hindistan) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (te-IN, Chitra)" | "te-IN-Chitra" |
| `th-TH` | Tayca | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (th-TH, Pattara)" | "th-TH-Pattara" |
| `tr-TR` | Türkçe (Türkiye) | Kadın | "Microsoft Server Konuşma MetniNden Konuşma Sesine (tr-TR, SedaRUS)" | "tr-TR-SedaRUS" |
| `vi-VN` | Vietnamca | Erkek | "Microsoft Server Konuşma Metni -Konuşma Sesi (vi-VN, An)" | "vi-VN-An" |
| `zh-CN` | Çince (Mandarin, basitleştirilmiş) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (zh-CN, HuihuiRUS)" | "zh-CN-HuihuiRUS" |
|  |  | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (zh-CN, Yaoyao, Apollo)" | "zh-CN-Yaoyao-Apollo" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (zh-CN, Kangkang, Apollo)" | "zh-CN-Kangkang-Apollo" |
| `zh-HK` | Çince (Kanton, Geleneksel) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (zh-HK, Tracy, Apollo)" | "zh-HK-Tracy-Apollo" |
|  |  | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (zh-HK, TracyRUS)" | "zh-HK-TracyRUS" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (zh-HK, Danny, Apollo)" | "zh-HK-Danny-Apollo" |
| `zh-TW` | Çince (Tayvan Mandarin) | Kadın | "Microsoft Server Konuşma Metni Konuşma Ses (zh-TW, Yating, Apollo)" | "zh-TW-Yating-Apollo" |
|  |  | Kadın | "Microsoft Server Konuşma Metni Nden Konuşma Sesine (zh-TW, HanHanRUS)" | "zh-TW-HanHanRUS" |
|  |  | Erkek | "Microsoft Server Konuşma Metni Konuşma Ses (zh-TW, Zhiwei, Apollo)" | "zh-TW-Zhiwei-Apollo" |

**1** *ar-EG Modern Standart Arapça (MSA) destekler.*

> [!IMPORTANT]
> Ses `en-US-Jessa` . `en-US-Aria` Daha önce "Jessa" kullanıyorsanız, "Aria" olarak değiştirin.

> [!TIP]
> Konuşma sentezi isteklerinizde tam hizmet adı eşlemesini veya kısa ses adını kullanabilirsiniz.

### <a name="customization"></a>Özelleştirme

Ses özelleştirme `de-DE`için kullanılabilir `en-GB` `en-IN`, `en-US` `es-MX`, `fr-FR` `it-IT`, `pt-BR`, `zh-CN`, , , , ve . Özel bir ses modeli eğitmek için sahip olduğunuz eğitim verileriyle eşleşen doğru yerel alanı seçin. Örneğin, sahip olduğunuz kayıt verileri İngiliz aksanıyla İngilizce konuşuluyorsa, 'yi seçin. `en-GB`

> [!NOTE]
> Biz Özel Ses iki dilli model eğitimi desteklemeyiz, Çince-İngilizce iki dilli dışında. İngilizce de konuşabilen bir Çince ses eğitmek istiyorsanız "Çince-İngilizce iki dilli" seçin. Tüm yerel ayarlarda ses eğitimi, herhangi bir eğitim verisi boyutuyla `en-US` başlabildiğiniz yerler `zh-CN` dışında 2.000'den fazla kelimeden oluşan bir veri kümesiyle başlar.

## <a name="speech-translation"></a>Konuşma çevirisi

**Konuşma Çevirisi** API, konuşma-konuşma ve konuşma-metin çevirisi için farklı dilleri destekler. Kaynak dil her zaman Konuşma-metin dil tablosundan olmalıdır. Kullanılabilir hedef diller, çeviri hedefinin konuşma mı yoksa metin mi olduğuna bağlıdır. Gelen konuşmayı [60'tan](https://www.microsoft.com/translator/business/languages/)fazla dile çevirebilirsiniz. [Konuşma sentezi](language-support.md#text-languages)için bir dil alt kümesi mevcuttur.

### <a name="text-languages"></a>Metin dilleri

| Metin dili           | Dil kodu |
|:------------------------|:-------------:|
| Afrikaner               | `af`          |
| Arapça                  | `ar`          |
| Bangla                  | `bn`          |
| Boşnakça (Latin)         | `bs`          |
| Bulgarca               | `bg`          |
| Kanton (Geleneksel) | `yue`         |
| Katalanca                 | `ca`          |
| Basitleştirilmiş Çince      | `zh-Hans`     |
| Geleneksel Çince     | `zh-Hant`     |
| Hırvatça                | `hr`          |
| Çekçe                   | `cs`          |
| Danca                  | `da`          |
| Felemenkçe                   | `nl`          |
| Türkçe                 | `en`          |
| Estonya Dili                | `et`          |
| Fiji                  | `fj`          |
| Filipino                | `fil`         |
| Fince                 | `fi`          |
| Fransızca                  | `fr`          |
| Almanca                  | `de`          |
| Yunanca                   | `el`          |
| Haiti Creole          | `ht`          |
| İbranice                  | `he`          |
| Hintçe                   | `hi`          |
| Hmong Daw               | `mww`         |
| Macarca               | `hu`          |
| Endonezce              | `id`          |
| İrlandaca                   | `ga`          |
| İtalyanca                 | `it`          |
| Japonca                | `ja`          |
| Kannada dili                 | `kn`          |
| Kiswahili               | `sw`          |
| Klingon                 | `tlh`         |
| Klingon (plqaD)         | `tlh-Qaak`    |
| Korece                  | `ko`          |
| Letonca                 | `lv`          |
| Litvanca              | `lt`          |
| Malagasy                | `mg`          |
| Malayca                   | `ms`          |
| Malayalam dili               | `ml`          |
| Maltaca                 | `mt`          |
| Norveççe               | `nb`          |
| Farsça                 | `fa`          |
| Lehçe                  | `pl`          |
| Portekizce (Brezilya)     | `pt-br`       |
| Portekizce (Portekiz)   | `pt-pt`       |
| Pencap dili                 | `pa`          |
| Queretaro Otomi         | `otq`         |
| Rumence                | `ro`          |
| Rusça                 | `ru`          |
| Samoaca                  | `sm`          |
| Sırpça (Kiril)      | `sr-Cyrl`     |
| Sırpça (Latin)         | `sr-Latn`     |
| Slovakça                  | `sk`          |
| Slovence               | `sl`          |
| İspanyolca                 | `es`          |
| İsveççe                 | `sv`          |
| Tahiti                | `ty`          |
| Tamil dili                   | `ta`          |
| Telugu dili                  | `te`          |
| Tayca                    | `th`          |
| Tongan                  | `to`          |
| Türkçe                 | `tr`          |
| Ukraynaca               | `uk`          |
| Urduca                    | `ur`          |
| Vietnamca              | `vi`          |
| Galce                   | `cy`          |
| Yucatec Maya            | `yua`         |

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmeti deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C'deki konuşmayı nasıl tanıyacağınızı görme #](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
