---
title: Dil desteği-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti, konuşmadan metne ve metinden konuşmaya dönüştürmeye, konuşma çevirisi ile birlikte çok sayıda dili destekler. Bu makale, hizmet özelliğine göre dil desteğinin kapsamlı bir listesini sağlar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: ac3cb1d637eae1b4ee0a7db59efe631c7eb1ac6f
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815265"
---
# <a name="language-and-region-support-for-the-speech-services"></a>Konuşma Hizmetleri için dil ve bölge desteği

Farklı diller farklı konuşma Hizmetleri işlevleri için desteklenir. Aşağıdaki tablolarda, dil desteği özetler.

## <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

Hem Microsoft konuşma tanıma SDK 'Sı hem de REST API aşağıdaki dilleri (yerel ayarlar) destekler. Farklı düzeyde özelleştirme, her dil için kullanılabilir.

  Kod | Dil | [Akustik uyarlama](how-to-customize-acoustic-models.md) | [Dil uyarlama](how-to-customize-language-model.md) | [Söyleniş uyarlama](how-to-customize-pronunciation.md)
 ------|----------|---------------------|---------------------|-------------------------
 ar EG | Arapça (Mısır) modern standart | Hayır | Evet | Hayır
 CA-ES | Katalanca | Hayır | Hayır | Hayır
 v-DK | Danca (Danimarka) | Hayır | Hayır | Hayır
 de-DE | Almanca (Almanya) | Evet | Evet | Evet
 tr-AU | İngilizce (Avustralya) | Hayır | Evet | Hayır
 CA tr | İngilizce (Kanada) | Hayır | Evet | Hayır
 en-GB | İngilizce (Birleşik Krallık) | Hayır | Evet | Hayır
 tr-giriş | English (India) | Evet | Evet | Hayır
 tr NZ | İngilizce (Yeni Zelanda) | Hayır | Evet | Hayır 
 en-US | İngilizce (ABD) | Evet | Evet | Evet
 es-ES | İspanyolca (İspanya) | Evet | Evet | Hayır
 es-MX | İspanyolca (Meksika) | Hayır | Evet | Hayır
 FI-FI | Fince (Finlandiya) | Hayır | Hayır | Hayır
 fr-CA | Fransızca (Kanada) | Hayır | Evet | Hayır
 fr-FR | Fransızca (Fransa) | Evet | Evet | Hayır
 yüksek giriş | Hintçe (Hindistan) | Hayır | Evet | Hayır
 İt-IT | İtalyanca (İtalya) | Evet | Evet | Hayır
 ja-JP | Japonca (Japonya) | Hayır | Evet | Hayır
 ko-KR | Korece (Güney Kore) | Hayır | Evet | Hayır
 NB-yok | Norveççe (Bokmal) (Norveç) | Hayır | Hayır | Hayır
 NL-NL | Hollanda dili (Hollanda) | Hayır | Evet | Hayır
 pl-PL | Lehçe (Polonya) | Hayır | Hayır | Hayır
 pt-BR | Portekizce (Brezilya) | Evet | Evet | Hayır
 pt-PT | Portekizce (Portekiz) | Hayır | Evet | Hayır
 ru-RU | Rusça (Rusya) | Evet | Evet | Hayır
 sv-SE | İsveççe (İsveç) | Hayır | Hayır | Hayır
 zh-CN | Çince (Basitleştirilmiş Mandarin) | Evet | Evet | Hayır
 zh-HK | Çince (Cantoneo, geleneksel) | Hayır | Evet | Hayır
 zh-TW | Çince (Tayvan Mandarin) | Hayır | Evet | Hayır
 TH TH | Tayca (Tayland) | Hayır | Hayır | Hayır


## <a name="text-to-speech"></a>Metin okuma

Metinden konuşmaya REST API, her biri yerel ayar ile tanımlanan belirli bir dili ve diyalekt destekleyen bu sesleri destekler.

> [!IMPORTANT]
> Fiyatlandırma, standart, özel ve sinir sesleri için farklılık gösterir. Daha fazla bilgi için lütfen [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) sayfasını ziyaret edin.

### <a name="neural-voices"></a>Sinir sesleri

Sinir metin okuma, derin sinir ağlarla desteklenen yeni bir konuşma türü türüdür. Bir sinir sesi kullanırken, sentezlenmiş konuşma, insan kayıtlarından neredeyse ayırt edilemez.

Sinir sesleri, chatbots ve sanal yardımcılar ile daha doğal ve ilgi çekici bir şekilde etkileşim kurmak, e-kitaplar gibi dijital metinleri audiobooks 'a dönüştürmek ve oto içi gezinti sistemlerini geliştirmek için kullanılabilir. İnsan benzeri doğal Prosody ve sözcüklerin bir kısmını temizleyerek, sinir seslerde kullanıcılar AI sistemleriyle etkileşim kurarken büyük ölçüde dinleme performansını önemli ölçüde azaltır.

Sinir seslerine ve bölgesel kullanılabilirliğe ait tam bir liste için bkz. [bölgeler](regions.md#standard-and-neural-voices).

Yerel Ayar | Dil | Cinsiyet | Tam hizmet adı eşleme | Kısa ses adı
--------|----------|--------|---------|------------
de-DE | Almanca (Almanya) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (de-DE, KatjaNeural)" | "de-KatjaNeural"
en-US | English (US) | Erkek | "Microsoft Server konuşma Metin Okuma sesi (en-US, GuyNeural)" | "en-US-GuyNeural"
en-US | English (US) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (en-US, JessaNeural)" | "en-US-JessaNeural"
İt-IT | İtalyanca (İtalya) | Kadın |"Microsoft Server konuşma Metin Okuma sesi (It-IT, ElsaNeural)" | "It-IT-ElsaNeural"
zh-CN | Çince (ana kara) | Kadın | "Microsoft Server konuşma Metin Okuma sesi (zh-CN, XiaoxiaoNeural)" | "zh-CN-XiaoxiaoNeural"

> [!NOTE]
> Konuşma senssıs isteklerinizin tam hizmet adı eşlemesini veya kısa ses adını kullanabilirsiniz.

### <a name="standard-voices"></a>Standart sesler

75 ' den fazla standart ses, daha fazla 45 dilde ve yerel ayarlarda bulunabilir ve bu da metni sentezleştirilmiş konuşmaya dönüştürmenize olanak tanır. Bölgesel kullanılabilirlik hakkında daha fazla bilgi için bkz. [bölgeler](regions.md#standard-and-neural-voices).

Yerel Ayar | Dil | Cinsiyet | Tam hizmet adı eşleme | Kısa ses adı
-------|----------|---------|----------|----------
ar EG\* | Arapça (Mısır) | Kadın | "Microsoft sunucu konuşma Sesli konuşmayı metne (ar-Örneğin, Hoda)" | "ar-EG-Hoda"
ar-SA | Arapça (Suudi Arabistan) | Erkek | "Microsoft Server Konuşma metin konuşma ses (ar-SA, Naayf)" | "ar-SA-Naayf"
BG-BG | Bulgarca | Erkek | "Microsoft Server Konuşma metin okuma ses (bg-BG, çalışan Ivan)" | "BG-BG-Ivan"
CA-ES | Katalanca (İspanya) | Kadın | "Microsoft Server Konuşma metin okuma ses (ca-ES, HerenaRUS)" | "CA-ES-HerenaRUS"
cs-CZ | Çekçe | Erkek | "Microsoft sunucu konuşma Sesli konuşmayı metne (cs-CZ, Jakub)" | "CS-CZ-Jakub"
v-DK | Danca | Kadın | "Microsoft Server Konuşma metin konuşma ses (v-DK, HelleRUS)" | "da-DK-HelleRUS"
de-AT | Almanca (Avusturya) | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-AT, Michael)" | "on-Michael"
de-CH | Almanca (İsviçre) | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-CH, Karsten)" | "de CH-Karsten"
de-DE | Almanca (Almanya) | Kadın | "Microsoft Server Konuşma metin konuşma ses (de-DE, Hedda)" | "de-DE-Hedda"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (de-DE, HeddaRUS)" | "de, HeddaRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-DE, Stefan, Apollo)" | "de-DE-Stefan-Apollo"
el-GR | Yunanca | Erkek | "Microsoft Server Konuşma metin konuşma ses (el-GR, Stefanos)" | "el-GR-Stefanos"
tr-AU | İngilizce (Avustralya) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-AU, Catherine)" | "en-AU-Catherine"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-AU, HayleyRUS)" | "en-AU-HayleyRUS"
CA tr | İngilizce (Kanada) | Kadın | "Microsoft Server Konuşma metin konuşma ses (tr-CA, Gamze)" | "en-CA-Linda"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (tr-CA, HeatherRUS)" | "en-CA-Centherrus"
en-GB | English (UK) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-GB, Susan, Apollo)" | "en-GB-çiğdem-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-GB, HazelRUS)" | "en-GB-HazelRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-GB, George, Apollo)" | "en-GB-George-Apollo"
IE tr | İngilizce (İrlanda) | Erkek | "Microsoft Server Konuşma metin konuşma ses (tr-IE, Sean)" | "en-IE-kemal"
tr-giriş | English (India) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-IN Heera, Apollo)" | "en-ın-heçi-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-IN, PriyaRUS)" | "en-basit Yarus"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-IN Ravi, Apollo)" | "en-ın-Ravi-Apollo"
en-US | English (US) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, ZiraRUS)" | "en-US-ZiraRUS"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, JessaRUS)" | "en-US-JessaRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-US, BenjaminRUS)" | "en-US-BenjaminRUS"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, Jessa24kRUS)" | "en-US-Jessa24kRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-US, Guy24kRUS)" | "en-US-Guy24kRUS"
es-ES | İspanyolca (İspanya) |Kadın | "Microsoft Server Konuşma metin okuma ses (es-ES, Gamze, Apollo)" | "ES-ES-gamze-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (es-ES, HelenaRUS)" | "ES-ES-HelenaRUS"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (es-ES, Pablo, Apollo)" | "ES-ES-Pablo-Apollo"
es-MX | İspanyolca (Meksika) | Kadın | "Microsoft Server Konuşma metin okuma ses (es-MX, HildaRUS)" | "es-MX-Tepdarus"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (es-MX, Raul Apollo)" | "es-MX-OYUL-Apollo"
FI-FI | Fince | Kadın | "Microsoft Server Konuşma metin konuşma ses (fi-FI, HeidiRUS)" | "fi-FI-Heidrus"
fr-CA | Fransızca (Kanada) |Kadın | "Microsoft Server Konuşma metin okuma ses (fr-CA, Caroline)" | "fr-CA-Caroline"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-CA, HarmonieRUS)" | "fr-CA-Harmonitorerus"
FR-CH | Fransızca (İsviçre)| Erkek | "Microsoft Server Konuşma metin okuma ses (fr-CH, Guillaume)" | "fr-CH-Guildefme"
fr-FR | Fransızca (Fransa)| Kadın | "Microsoft Server Konuşma metin okuma ses (fr-FR, Julie, Apollo)" | "fr-FR-Julie-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-FR, HortenseRUS)" | "fr-FR-HortenseRUS"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (fr-FR, Paul, Apollo)" | "fr-FR-Paul-Apollo"
He IL| İbranice (İsrail) | Erkek| "Microsoft Server Konuşma metin konuşma ses (he-IL, Asaf)" | "he-Il-asaf"
yüksek giriş | Hintçe (Hindistan) | Kadın | "Microsoft Server Konuşma metin konuşma ses (Merhaba açma, Kalpana, Apollo)" | "HI-IN-Kalpana-Apollo"
| | |Kadın | "Microsoft Server Konuşma metin konuşma ses (Merhaba-IN, Kalpana)" | "Hi Pana"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (Merhaba-IN, Hemant)" | "Hi-IN-Hemant"
hr-HR | Hırvatça | Erkek | "Microsoft Server Konuşma metin okuma ses (hr-HR, Matej)" | "HR-HR-Matej"
hu-HU | Macarca | Erkek | "Microsoft Server Konuşma metin konuşma ses (hu-HU, Szabolcs)" | "HU-HU-Szacıvacs"
ID | Endonezya dili| Erkek | "Microsoft Server Konuşma metin konuşma ses (-ID, Andika)" | "kimlik-KIMLIĞI-Andika"
İt-IT | İtalyanca | Erkek | "Microsoft Server Konuşma metin konuşma ses (Cosimo, it-IT, Apollo)" | "It-IT-Cosımo-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (it-IT, LuciaRUS)" | "It-IT-LuciaRUS"
ja-JP | Japonca | Kadın | "Microsoft Server Konuşma metin okuma ses (ja-JP, Ayumi, Apollo)" | "ja-JP-Ayumi-Apollo"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (ja-JP, Ichiro, Apollo)" | "ja-JP-Ichiro-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (ja-JP, HarukaRUS)" | "ja-JP-HarukaRUS"
ko-KR | Korece | Kadın | "Microsoft Server Konuşma metin konuşma ses (ko-KR, HeamiRUS)" | "ko-KR-Heamırus"
ms-MY | Malay dili | Erkek | "Microsoft Server Konuşma metin okuma ses (ms MY, Rizwan)" | "MS-MY-Rizwan"
NB-yok | Norveççe | Kadın | "Microsoft Server Konuşma metin okuma ses (nb-yok, HuldaRUS)" | "NB-hayır-HuldaRUS"
NL-NL | Felemenkçe | Kadın | "Microsoft Server Konuşma metin okuma ses (nl-NL, HannaRUS)" | "nl-NL-HannaRUS"
pl-PL | Lehçe | Kadın | "Microsoft Server Konuşma metin okuma ses (pl-PL, PaulinaRUS)" | "pl-PL-PaulinaRUS"
pt-BR | Portekizce (Brezilya) | Kadın | "Microsoft Server Konuşma metin okuma ses (pt-BR, HeloisaRUS)" | "PT-BR-HeloisaRUS"
| | | Erkek |"Microsoft Server Konuşma metin okuma ses (pt-BR, Daniel, Apollo)" | "PT-BR-Daniel-Apollo"
pt-PT | Portekizce (Portekiz) | Kadın | "Microsoft Server Konuşma metin okuma ses (pt-PT, HeliaRUS)" | "PT-PT Helinarus"
Ro-RO | Rumence | Erkek | "Microsoft Server Konuşma metin konuşma ses (ro-RO, Andrei)" | "RO-RO-Andrei"
ru-RU |Rusça| Kadın | "Microsoft Server Konuşma metin okuma ses (ru-RU, Irina, Apollo)" | "ru-RU-Irina-Apollo"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (ru-RU, Pavel, Apollo)" | "ru-RU-Palevel-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (ru-RU, EkaterinaRUS)" | ru-RU-EkaterinaRUS
SK-SK | Slovakça | Erkek | "Microsoft Server Konuşma metin okuma ses (sk-SK, Filip)" | "SK-SK-Filıp"
SL SI | Slovence | Erkek | "Microsoft Server Konuşma metin okuma ses (sl-sı, Lado)" | "SL-SI-Lado"
sv-SE | İsveççe | Kadın | "Microsoft Server Konuşma metin konuşma ses (sv-SE, HedvigRUS)" | "ZF-i-HedvigRUS"
Veri-ın | Tamilce (Hindistan) | Erkek | "Microsoft Server Konuşma metin konuşma ses (veri-ın, Valluvar)" | "ta-Valluvar"
metin-giriş | Telugu dili (Hindistan) | Kadın | "Microsoft Server Konuşma metin konuşma ses (metin-IN, Chitra)" | "te-Chitra"
TH TH | Tay Dili | Erkek | "Microsoft Server Konuşma metin okuma ses (th-TH, Pattara)" | "TH-Pattara"
tr-TR | Türkçe | Kadın | "Microsoft Server Konuşma metin okuma ses (tr-TR, SedaRUS)" | "tr-TR-SedaRUS"
VI VN | Vietnam dili | Erkek | "Microsoft Server Konuşma metin okuma ses (vi-VN bir)" | "VI-VN-a"
zh-CN | Çince (ana kara) | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-CN, HuihuiRUS)" | "zh-CN-Huihuırus"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-CN, Yaoyao, Apollo)" | "zh-CN-Yaoyao-Apollo"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-CN, Kangkang, Apollo)" | "zh-CN-Kangkang-Apollo"
zh-HK | Çince (Hong Kong) | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-HK Tracy, Apollo)" | "zh-HK-Tracy-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-HK TracyRUS)" | "zh-HK-Tracyırus"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-HK Danny, Apollo)" | "zh-HK-Danny-Apollo"
zh-TW | Çince (Tayvan) | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-TW Yating, Apollo)" | "zh-TW-Yating-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-TW, HanHanRUS)" | "zh-TW-HanHanRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-TW Zhiwei, Apollo)" | "zh-TW-Zhiwei-Apollo"

\* *ar-ÖRN Modern standart Arapça (MSA) destekler.*

> [!NOTE]
> Konuşma senssıs isteklerinizin tam hizmet adı eşlemesini veya kısa ses adını kullanabilirsiniz.

### <a name="customization"></a>Özelleştirme

Ses özelleştirmesi, de-de, en-GB, en-ın, en-US, es-MX, fr-FR, It-IT, PT-BR ve zh-CN için kullanılabilir. Özel bir ses modeli eğitmeniz gereken eğitim verileriyle eşleşen doğru yerel ayarı seçin. Örneğin, kullandığınız kayıt verileri Ingiliz alfabesindeki bir vurgu ile konuşululursam, en-GB ' y i seçin.  

> [!NOTE]
> Çince-Ingilizce bı-dil dışında, özel seste bı-dilli model eğitimini desteklemiyoruz. Ingilizce 'yi de okuyabileceği bir Çince ses alıştırması yapmak istiyorsanız ' Çince-Ingilizce çift dilli ' seçeneğini belirleyin. Tüm yerel ayarlarda bulunan sesli eğitim, her türlü eğitim verisi ile başlayabileceğiniz en-US ve zh-CN dışında bir 2000 ' in veri kümesiyle başlar.

## <a name="speech-translation"></a>Konuşma çevirisi

**Konuşma çevirisi** API, konuşma tanıma ve konuşma tanıma ve konuşma metin çevirisi için farklı dilleri destekler. Kaynak dilin her zaman konuşma-metin dili tablosundan olması gerekir. Hedef diller Çeviri hedef konuşma veya metin olmasına göre değişir. Gelen konuşmaya Çevir birden fazla [60 diller](https://www.microsoft.com/translator/business/languages/). Bu dillerin bir alt kümesi için kullanılabilir [konuşma sentezi](language-support.md#text-languages).

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
| Türkçe      | `en`          |
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
| Endonezya dili      | `id`          |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
| Svahili dili      | `sw`          |
| Klingon      | `tlh`          |
| Klingon (plqaD)      | `tlh-Qaak`          |
| Korece      | `ko`          |
| Letonca      | `lv`          |
| Litvanca      | `lt`          |
| Malgaşça      | `mg`          |
| Malay dili      | `ms`          |
| Malta dili      | `mt`          |
| Norveççe      | `nb`          |
| Farsça      | `fa`          |
| Lehçe      | `pl`          |
| Portekizce      | `pt`          |
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
| Tay Dili      | `th`          |
| Tonga Dili      | `to`          |
| Türkçe      | `tr`          |
| Ukrayna dili      | `uk`          |
| Urduca      | `ur`          |
| Vietnam dili      | `vi`          |
| Gaelce      | `cy`          |
| Yucatec Maya      | `yua`          |


## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma Tanıma Hizmetleri deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
