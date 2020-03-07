---
title: Dil desteği-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti, konuşmadan metne ve metinden konuşmaya dönüştürmeye, konuşma çevirisi ile birlikte çok sayıda dili destekler. Bu makale, hizmet özelliğine göre dil desteğinin kapsamlı bir listesini sağlar.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: dapine
ms.custom: seodec18
ms.openlocfilehash: 13d12bfbf64af9e19b66e63c88c9d7e0534571f3
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78378997"
---
# <a name="language-and-region-support-for-the-speech-service"></a>Konuşma hizmeti için dil ve bölge desteği

Dil desteği, konuşma hizmeti işlevselliğine göre farklılık gösterir. Aşağıdaki tablolar, [konuşmadan metne](#speech-to-text), [metinden konuşmaya](#text-to-speech)ve [konuşma çevirisi](#speech-translation) hizmet teklifleri için dil desteğini özetler.

## <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

Hem Microsoft konuşma SDK 'Sı hem de REST API aşağıdaki dilleri (yerel ayarlar) destekler. Doğruluğu artırmak için, bir dilin bir alt kümesi için, ses + ınsan etiketli yazılı döküm veya Ilgili metin: cümleler karşıya yüklenirken özelleştirme sunulur. Telaffuz özelleştirmesi Şu anda yalnızca `en-US` ve `de-DE`için kullanılabilir. [Özelleştirme hakkında](how-to-custom-speech.md)daha fazla bilgi edinin.

<!--
To get the AM and ML bits:
https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20models%3A/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.ai -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

 Yerel Ayar | Dil | Desteklenen | Özelleştirmeleri
------|------------|-----------|-------------
`ar-AE` | Arapça (UAE) | Evet | Hayır
`ar-BH` | Arapça (Bahreyn), modern standart | Evet | Dil modeli
`ar-EG` | Arapça (Mısır) | Evet | Dil modeli
`ar-KW` | Arapça (Kuveyt) | Evet | Hayır
`ar-QA` | Arapça (Qtor) | Evet | Hayır
`ar-SA` | Arapça (Suudi Arabistan) | Evet | Hayır
`ca-ES` | Katalanca | Evet | Dil modeli
`da-DK` | Danca (Danimarka) | Evet | Dil modeli
`de-DE` | Almanca (Almanya) | Evet | Akustik model<br>Dil modeli<br>Söylenişi
`en-AU` | İngilizce (Avustralya) | Evet | Akustik model<br>Dil modeli
`en-CA` | İngilizce (Kanada) | Evet | Akustik model<br>Dil modeli
`en-GB` | İngilizce (Birleşik Krallık) | Evet | Akustik model<br>Dil modeli<br>Söylenişi
`en-IN` | English (India) | Evet | Akustik model<br>Dil modeli
`en-NZ` | İngilizce (Yeni Zelanda) | Evet | Akustik model<br>Dil modeli
`en-US` | Türkçe | Evet | Akustik model<br>Dil modeli<br>Söylenişi
`es-ES` | İspanyolca (İspanya) | Evet | Akustik model<br>Dil modeli
`es-MX` | İspanyolca (Meksika) | Evet | Akustik model<br>Dil modeli
`fi-FI` | Fince (Finlandiya) | Evet | Dil modeli
`fr-CA` | Fransızca (Kanada) | Evet | Akustik model<br>Dil modeli
`fr-FR` | Fransızca (Fransa) | Evet | Akustik model<br>Dil modeli<br>Söylenişi
`gu-IN` | Gucerat dili (Hindistan) | Evet | Dil modeli
`hi-IN` | Hintçe (Hindistan) | Evet | Akustik model<br>Dil modeli
`it-IT` | İtalyanca (İtalya) | Evet | Akustik model<br>Dil modeli<br>Söylenişi
`ja-JP` | Japonca (Japonya) | Evet | Dil modeli
`ko-KR` | Korece (Güney Kore) | Evet | Dil modeli
`mr-IN` | Marathi dili (Hindistan) | Evet | Dil modeli
`nb-NO` | Norveççe (Bokmal) (Norveç) | Evet | Dil modeli
`nl-NL` | Hollanda dili (Hollanda) | Evet | Dil modeli
`pl-PL` | Lehçe (Polonya) | Evet | Dil modeli
`pt-BR` | Portekizce (Brezilya) | Evet | Akustik model<br>Dil modeli<br>Söylenişi
`pt-PT` | Portekizce (Portekiz) | Evet | Dil modeli
`ru-RU` | Rusça (Rusya) | Evet | Akustik model<br>Dil modeli
`sv-SE` | İsveççe (İsveç) | Evet | Dil modeli
`ta-IN` | Tamilce (Hindistan) | Evet | Dil modeli
`te-IN` | Telugu dili (Hindistan) | Evet | Hayır
`th-TH` | Tayca (Tayland) | Evet | Hayır
`tr-TR` | Türkçe (Türkiye) | Evet | Hayır
`zh-CN` | Çince (Basitleştirilmiş Mandarin) | Evet | Akustik model<br>Dil modeli
`zh-HK` | Çince (Cantoneo, geleneksel) | Evet | Dil modeli
`zh-TW` | Çince (Tayvan Mandarin) | Evet | Dil modeli

## <a name="text-to-speech"></a>Metin okuma

Hem Microsoft konuşma SDK 'Sı hem de REST API 'Leri, her biri yerel ayar ile tanımlanan belirli bir dili ve diyalekt destekleyen bu sesleri destekler.

> [!IMPORTANT]
> Fiyatlandırma, standart, özel ve sinir sesleri için farklılık gösterir. Daha fazla bilgi için lütfen [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) sayfasını ziyaret edin.

### <a name="neural-voices"></a>Sinir sesleri

Sinir metin okuma, derin sinir ağlarla desteklenen yeni bir konuşma türü türüdür. Bir sinir sesi kullanırken, sentezlenmiş konuşma, insan kayıtlarından neredeyse ayırt edilemez.

Sinir sesleri, chatbots ve ses yardımcılarıyla daha doğal ve etkileyici bir şekilde etkileşim kurmak, e-kitaplar gibi dijital metinleri audiobooks 'a dönüştürmek ve oto içi gezinti sistemlerini geliştirmek için kullanılabilir. İnsan benzeri doğal Prosody ve sözcüklerin bir kısmını temizleyerek, sinir seslerde kullanıcılar AI sistemleriyle etkileşim kurarken büyük ölçüde dinleme performansını önemli ölçüde azaltır.

Bölgesel kullanılabilirlik hakkında daha fazla bilgi için bkz. [bölgeler](regions.md#standard-and-neural-voices).

Yerel Ayar | Dil | Cinsiyet | Tam hizmet adı eşleme | Kısa ses adı
--------|----------|--------|---------|------------
`de-DE` | Almanca (Almanya) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (de-DE, KatjaNeural)" | "de-KatjaNeural"
`en-US` | {1&gt;İngilizce (ABD)&lt;1} | Kadın | "Microsoft Server konuşma Metin Okuma sesi (en-US, JessaNeural)" | "en-US-JessaNeural"
`en-US` | {1&gt;İngilizce (ABD)&lt;1} | Erkek | "Microsoft Server konuşma Metin Okuma sesi (en-US, GuyNeural)" | "en-US-GuyNeural"
`it-IT` | İtalyanca (İtalya) | Kadın |"Microsoft Server konuşma Metin Okuma sesi (It-IT, ElsaNeural)" | "It-IT-ElsaNeural"
`pt-BR` | Portekizce (Brezilya) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (PT-BR, FranciscaNeural)" | "PT-BR-FranciscaNeural"
`zh-CN` | Çince (ana kara) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (zh-CN, XiaoxiaoNeural)" | "zh-CN-XiaoxiaoNeural"

Sinir seslerini nasıl yapılandıracağınızı ve ayarlayabileceğinizi öğrenmek için bkz. [konuşma sen, biçimlendirme dili](speech-synthesis-markup.md#adjust-speaking-styles).

> [!NOTE]
> Konuşma senssıs isteklerinizin tam hizmet adı eşlemesini veya kısa ses adını kullanabilirsiniz.

### <a name="standard-voices"></a>Standart sesler

75 ' den fazla standart ses, daha fazla 45 dilde ve yerel ayarlarda bulunabilir ve bu da metni sentezleştirilmiş konuşmaya dönüştürmenize olanak tanır. Bölgesel kullanılabilirlik hakkında daha fazla bilgi için bkz. [bölgeler](regions.md#standard-and-neural-voices).

Yerel Ayar | Dil | Cinsiyet | Tam hizmet adı eşleme | Kısa ad
-------|----------|---------|----------|----------
<sup>1</sup>`ar-EG` | Arapça (Mısır) | Kadın | "Microsoft sunucu konuşma Sesli konuşmayı metne (ar-Örneğin, Hoda)" | "ar-EG-Hoda"
`ar-SA` | Arapça (Suudi Arabistan) | Erkek | "Microsoft Server Konuşma metin konuşma ses (ar-SA, Naayf)" | "ar-SA-Naayf"
`bg-BG` | Bulgarca | Erkek | "Microsoft Server Konuşma metin okuma ses (bg-BG, çalışan Ivan)" | "BG-BG-Ivan"
`ca-ES` | Katalanca (İspanya) | Kadın | "Microsoft Server Konuşma metin okuma ses (ca-ES, HerenaRUS)" | "CA-ES-HerenaRUS"
`cs-CZ` | Çekçe | Erkek | "Microsoft sunucu konuşma Sesli konuşmayı metne (cs-CZ, Jakub)" | "CS-CZ-Jakub"
`da-DK` | Danca | Kadın | "Microsoft Server Konuşma metin konuşma ses (v-DK, HelleRUS)" | "da-DK-HelleRUS"
`de-AT` | Almanca (Avusturya) | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-AT, Michael)" | "on-Michael"
`de-CH` | Almanca (İsviçre) | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-CH, Karsten)" | "de CH-Karsten"
`de-DE` | Almanca (Almanya) | Kadın | "Microsoft Server Konuşma metin konuşma ses (de-DE, Hedda)" | "de-DE-Hedda"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (de-DE, HeddaRUS)" | "de, HeddaRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-DE, Stefan, Apollo)" | "de-DE-Stefan-Apollo"
`el-GR` | Yunanca | Erkek | "Microsoft Server Konuşma metin konuşma ses (el-GR, Stefanos)" | "el-GR-Stefanos"
`en-AU` | İngilizce (Avustralya) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-AU, Catherine)" | "en-AU-Catherine"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-AU, HayleyRUS)" | "en-AU-HayleyRUS"
`en-CA` | İngilizce (Kanada) | Kadın | "Microsoft Server Konuşma metin konuşma ses (tr-CA, Gamze)" | "en-CA-Linda"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (tr-CA, HeatherRUS)" | "en-CA-Centherrus"
`en-GB` | İngilizce (Birleşik Krallık) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-GB, Susan, Apollo)" | "en-GB-çiğdem-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-GB, HazelRUS)" | "en-GB-HazelRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-GB, George, Apollo)" | "en-GB-George-Apollo"
`en-IE` | İngilizce (İrlanda) | Erkek | "Microsoft Server Konuşma metin konuşma ses (tr-IE, Sean)" | "en-IE-kemal"
`en-IN` | English (India) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-IN Heera, Apollo)" | "en-ın-heçi-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-IN, PriyaRUS)" | "en-basit Yarus"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-IN Ravi, Apollo)" | "en-ın-Ravi-Apollo"
`en-US` | {1&gt;İngilizce (ABD)&lt;1} | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, ZiraRUS)" | "en-US-ZiraRUS"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, JessaRUS)" | "en-US-JessaRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-US, BenjaminRUS)" | "en-US-BenjaminRUS"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, Jessa24kRUS)" | "en-US-Jessa24kRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-US, Guy24kRUS)" | "en-US-Guy24kRUS"
`es-ES` | İspanyolca (İspanya) |Kadın | "Microsoft Server Konuşma metin okuma ses (es-ES, Gamze, Apollo)" | "ES-ES-gamze-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (es-ES, HelenaRUS)" | "ES-ES-HelenaRUS"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (es-ES, Pablo, Apollo)" | "ES-ES-Pablo-Apollo"
`es-MX` | İspanyolca (Meksika) | Kadın | "Microsoft Server Konuşma metin okuma ses (es-MX, HildaRUS)" | "es-MX-Tepdarus"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (es-MX, Raul Apollo)" | "es-MX-OYUL-Apollo"
`fi-FI` | Fince | Kadın | "Microsoft Server Konuşma metin konuşma ses (fi-FI, HeidiRUS)" | "fi-FI-Heidrus"
`fr-CA` | Fransızca (Kanada) |Kadın | "Microsoft Server Konuşma metin okuma ses (fr-CA, Caroline)" | "fr-CA-Caroline"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-CA, HarmonieRUS)" | "fr-CA-Harmonitorerus"
`fr-CH` | Fransızca (İsviçre)| Erkek | "Microsoft Server Konuşma metin okuma ses (fr-CH, Guillaume)" | "fr-CH-Guildefme"
`fr-FR` | Fransızca (Fransa)| Kadın | "Microsoft Server Konuşma metin okuma ses (fr-FR, Julie, Apollo)" | "fr-FR-Julie-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-FR, HortenseRUS)" | "fr-FR-HortenseRUS"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (fr-FR, Paul, Apollo)" | "fr-FR-Paul-Apollo"
`he-IL` | İbranice (İsrail) | Erkek| "Microsoft Server Konuşma metin konuşma ses (he-IL, Asaf)" | "he-Il-asaf"
`hi-IN` | Hintçe (Hindistan) | Kadın | "Microsoft Server Konuşma metin konuşma ses (Merhaba açma, Kalpana, Apollo)" | "HI-IN-Kalpana-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (Merhaba-IN, Kalpana)" | "Hi Pana"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (Merhaba-IN, Hemant)" | "Hi-IN-Hemant"
`hr-HR` | Hırvatça | Erkek | "Microsoft Server Konuşma metin okuma ses (hr-HR, Matej)" | "HR-HR-Matej"
`hu-HU` | Macarca | Erkek | "Microsoft Server Konuşma metin konuşma ses (hu-HU, Szabolcs)" | "HU-HU-Szacıvacs"
`id-ID` | Endonezce| Erkek | "Microsoft Server Konuşma metin konuşma ses (-ID, Andika)" | "kimlik-KIMLIĞI-Andika"
`it-IT` | İtalyanca | Erkek | "Microsoft Server Konuşma metin konuşma ses (Cosimo, it-IT, Apollo)" | "It-IT-Cosımo-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (it-IT, LuciaRUS)" | "It-IT-LuciaRUS"
`ja-JP` | Japonca | Kadın | "Microsoft Server Konuşma metin okuma ses (ja-JP, Ayumi, Apollo)" | "ja-JP-Ayumi-Apollo"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (ja-JP, Ichiro, Apollo)" | "ja-JP-Ichiro-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (ja-JP, HarukaRUS)" | "ja-JP-HarukaRUS"
`ko-KR` | Korece | Kadın | "Microsoft Server Konuşma metin konuşma ses (ko-KR, HeamiRUS)" | "ko-KR-Heamırus"
`ms-MY` | Malayca | Erkek | "Microsoft Server Konuşma metin okuma ses (ms MY, Rizwan)" | "MS-MY-Rizwan"
`nb-NO` | Norveççe | Kadın | "Microsoft Server Konuşma metin okuma ses (nb-yok, HuldaRUS)" | "NB-hayır-HuldaRUS"
`nl-NL` | Felemenkçe | Kadın | "Microsoft Server Konuşma metin okuma ses (nl-NL, HannaRUS)" | "nl-NL-HannaRUS"
`pl-PL` | Lehçe | Kadın | "Microsoft Server Konuşma metin okuma ses (pl-PL, PaulinaRUS)" | "pl-PL-PaulinaRUS"
`pt-BR` | Portekizce (Brezilya) | Kadın | "Microsoft Server Konuşma metin okuma ses (pt-BR, HeloisaRUS)" | "PT-BR-HeloisaRUS"
| | | Erkek |"Microsoft Server Konuşma metin okuma ses (pt-BR, Daniel, Apollo)" | "PT-BR-Daniel-Apollo"
`pt-PT` | Portekizce (Portekiz) | Kadın | "Microsoft Server Konuşma metin okuma ses (pt-PT, HeliaRUS)" | "PT-PT Helinarus"
`ro-RO` | Rumence | Erkek | "Microsoft Server Konuşma metin konuşma ses (ro-RO, Andrei)" | "RO-RO-Andrei"
`ru-RU` |Rusça| Kadın | "Microsoft Server Konuşma metin okuma ses (ru-RU, Irina, Apollo)" | "ru-RU-Irina-Apollo"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (ru-RU, Pavel, Apollo)" | "ru-RU-Palevel-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (ru-RU, EkaterinaRUS)" | ru-RU-EkaterinaRUS
`sk-SK` | Slovakça | Erkek | "Microsoft Server Konuşma metin okuma ses (sk-SK, Filip)" | "SK-SK-Filıp"
`sl-SI` | Slovence | Erkek | "Microsoft Server Konuşma metin okuma ses (sl-sı, Lado)" | "SL-SI-Lado"
`sv-SE` | İsveççe | Kadın | "Microsoft Server Konuşma metin konuşma ses (sv-SE, HedvigRUS)" | "ZF-i-HedvigRUS"
`ta-IN` | Tamilce (Hindistan) | Erkek | "Microsoft Server Konuşma metin konuşma ses (veri-ın, Valluvar)" | "ta-Valluvar"
`te-IN` | Telugu dili (Hindistan) | Kadın | "Microsoft Server Konuşma metin konuşma ses (metin-IN, Chitra)" | "te-Chitra"
`th-TH` | Tay Dili | Erkek | "Microsoft Server Konuşma metin okuma ses (th-TH, Pattara)" | "TH-Pattara"
`tr-TR` | Türkçe (Türkiye) | Kadın | "Microsoft Server Konuşma metin okuma ses (tr-TR, SedaRUS)" | "tr-TR-SedaRUS"
`vi-VN` | Vietnamca | Erkek | "Microsoft Server Konuşma metin okuma ses (vi-VN bir)" | "VI-VN-a"
`zh-CN` | Çince (ana kara) | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-CN, HuihuiRUS)" | "zh-CN-Huihuırus"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-CN, Yaoyao, Apollo)" | "zh-CN-Yaoyao-Apollo"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-CN, Kangkang, Apollo)" | "zh-CN-Kangkang-Apollo"
`zh-HK` | Çince (Hong Kong ÖİB) | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-HK Tracy, Apollo)" | "zh-HK-Tracy-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-HK TracyRUS)" | "zh-HK-Tracyırus"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-HK Danny, Apollo)" | "zh-HK-Danny-Apollo"
`zh-TW` | Çince (Tayvan) | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-TW Yating, Apollo)" | "zh-TW-Yating-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-TW, HanHanRUS)" | "zh-TW-HanHanRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-TW Zhiwei, Apollo)" | "zh-TW-Zhiwei-Apollo"

**1** *ar-örneğin modern Standart Arapça (MSA) desteği.*

> [!NOTE]
> Konuşma senssıs isteklerinizin tam hizmet adı eşlemesini veya kısa ses adını kullanabilirsiniz.

### <a name="customization"></a>Özelleştirme

Ses özelleştirmesi `de-DE`, `en-GB`, `en-IN`, `en-US`, `es-MX`, `fr-FR`, `it-IT`, `pt-BR`ve `zh-CN`için kullanılabilir. Özel bir ses modeli eğitmeniz gereken eğitim verileriyle eşleşen doğru yerel ayarı seçin. Örneğin, kullandığınız kayıt verileri Ingiliz alfabesindeki bir vurgu ile konuşulursam `en-GB`' yi seçin.

> [!NOTE]
> Çince-Ingilizce bı-dil dışında, özel seste bı-dilli model eğitimini desteklemiyoruz. Ingilizce 'ye de konuşarak bir Çince ses alıştırması yapmak istiyorsanız "Çince-Ingilizce çift dilli" seçeneğini belirleyin. Tüm yerel ayarlarda sesli eğitim, `en-US` ve herhangi bir eğitim verisi boyutuyla başlayabileceğiniz `zh-CN` dışında bir 2000 + utterde veri kümesiyle başlar.

## <a name="speech-translation"></a>Konuşma çevirisi

**Konuşma çevirisi** API 'si, konuşmaya konuşmaya ve konuşmadan metne dönüştürme için farklı dilleri destekler. Kaynak dilin her zaman konuşma-metin dili tablosundan olması gerekir. Hedef diller Çeviri hedef konuşma veya metin olmasına göre değişir. Gelen konuşmayı 60 ' den fazla [dile](https://www.microsoft.com/translator/business/languages/)çevirebilirsiniz. [Konuşma birleştirme](language-support.md#text-languages)için bir dil alt kümesi mevcuttur.

### <a name="text-languages"></a>Metin dilleri

| SMS dili    | Dil kodu |
|:----------- |:-------------:|
| Afrikaner dili      | `af`          |
| Arapça       | `ar`          |
| Bangla      | `bn`          |
| Boşnakça (Latin)      | `bs`          |
| Bulgarca      | `bg`          |
| Kanton (Geleneksel)      | `yue`          |
| Katalanca      | `ca`          |
| Basitleştirilmiş Çince      | `zh-Hans`          |
| Geleneksel Çince      | `zh-Hant`          |
| Hırvatça      | `hr`          |
| Çekçe      | `cs`          |
| Danca      | `da`          |
| Felemenkçe      | `nl`          |
| İngilizce      | `en`          |
| Estonca      | `et`          |
| Fiji Adaları dili      | `fj`          |
| Filipin dili      | `fil`          |
| Fince      | `fi`          |
| Fransızca      | `fr`          |
| Almanca      | `de`          |
| Yunanca      | `el`          |
| Haiti Kreyolu      | `ht`          |
| İbranice      | `he`          |
| Hintçe      | `hi`          |
| Hmong Daw      | `mww`          |
| Macarca      | `hu`          |
| Endonezce      | `id`          |
| İrlanda dili      | `ga`          |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
| Kannada dili      | `kn`          |
| Svahili dili      | `sw`          |
| Klingon      | `tlh`          |
| Klingon (plqaD)      | `tlh-Qaak`          |
| Korece      | `ko`          |
| Letonca      | `lv`          |
| Litvanca      | `lt`          |
| Malgaşça      | `mg`          |
| Malayca      | `ms`          |
| Malayalam dili      | `ml`          |
| Malta dili      | `mt`          |
| Norveççe      | `nb`          |
| Farsça      | `fa`          |
| Lehçe      | `pl`          |
| Portekizce (Brezilya)      | `pt-br`          |
| Portekizce (Portekiz)      | `pt-pt`          |
| Pencap dili      | `pa`          |
| Queretaro Otomi      | `otq`          |
| Rumence      | `ro`          |
| Rusça      | `ru`          |
| Samoaca      | `sm`          |
| Sırpça (Kiril)      | `sr-Cyrl`          |
| Sırpça (Latin)      | `sr-Latn`          |
| Slovakça     | `sk`          |
| Slovence      | `sl`          |
| İspanyolca      | `es`          |
| İsveççe      | `sv`          |
| Tahitian      | `ty`          |
| Tamil dili      | `ta`          |
| Telugu dili      | `te`          |
| Tay Dili      | `th`          |
| Tonga Dili      | `to`          |
| Türkçe      | `tr`          |
| Ukrayna dili      | `uk`          |
| Urduca      | `ur`          |
| Vietnamca      | `vi`          |
| Gaelce      | `cy`          |
| Yucatec Maya      | `yua`          |


## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmeti deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [Bkz. konuşmayı tanımaC#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
