---
title: Metinden konuşmaya - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmetindeki metinden konuşmaya özelliği, uygulamalarınızın, araçlarınızın veya aygıtlarınızın metni doğal insan benzeri sentezlenmiş konuşmaya dönüştürmesini sağlar. Önceden ayarlanmış sesleri seçin veya kendi özel sesinizi oluşturun.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: trbye
ms.openlocfilehash: 1a8b458eb004b44d0045f36b18d88e11e019c4d2
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399494"
---
# <a name="what-is-text-to-speech"></a>Metin okuma nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Konuşma hizmetinden metin konuşma, uygulamalarınızın, araçlarınızın veya aygıtlarınızın metni insan ayarı gibi sentezlenmiş konuşmaya dönüştürmesini sağlar. Standart ve sinirsel sesler arasından seçim yapın veya ürününize veya markanıza özgü özel bir ses oluşturun. 75+ standart ses 45'ten fazla dilde ve yerel olarak mevcuttur ve belirli sayıda dilde ve yerel olarak 5 sinirsel ses mevcuttur. Desteklenen seslerin, dillerin ve yerel birimlerin tam listesi için [desteklenen dillere](language-support.md#text-to-speech)bakın.

> [!NOTE]
> Bing Konuşması 15 Ekim 2019 tarihinde kullanımdan kaldırıldı. Uygulamalarınız, araçlarınız veya ürünleriniz Bing Konuşma API'leri veya Özel Konuşma kullanıyorsa, Konuşma hizmetine geçiş inizi yardımcı olacak kılavuzlar oluşturduk.
> - [Bing Konuşmasından Konuşma hizmetine geçiş](how-to-migrate-from-bing-speech.md)

## <a name="core-features"></a>Temel özellikler

* Konuşma sentezi - Standart, sinirsel veya özel sesleri kullanarak metinden konuşmaya dönüştürmek için [Konuşma SDK'sını](quickstarts/text-to-speech-audio-file.md) veya [REST API'yi](rest-text-to-speech.md) kullanın.

* Uzun sesin eşzamanlı sentezi - [Uzun Ses API'sini](long-audio-api.md) kullanarak metin-konuşma dosyalarını 10 dakikadan uzun bir süre (örneğin sesli kitaplar veya dersler) eşzamanlı olarak sentezleyin. Konuşma SDK veya konuşma-to-metin REST API kullanılarak gerçekleştirilen sentez aksine, yanıtlar gerçek zamanlı olarak döndürülür değil. Beklenti, isteklerin eşit bir şekilde gönderilmesi, yanıtların yoklanmış olması ve sentezlenen sesin hizmetten kullanılabilir hale geldiğinde indirilmesidir. Sadece özel sinirsel sesler desteklenir.

* Standart sesler - İstatistiksel Parametrik Sentez ve/veya Concatenation Sentez teknikleri kullanılarak oluşturulmuştur. Bu sesler son derece anlaşılır ve ses doğaldır. Çok çeşitli ses seçenekleriyle uygulamalarınızın 45'ten fazla dilde kolayca konuşmasını sağlayabilirsiniz. Bu sesler, kısaltmalar, kısaltma genişletmeleri, tarih/saat yorumları, politelefonlar ve daha fazlası için destek de dahil olmak üzere yüksek telaffuz doğruluğu sağlar. Standart seslerin tam listesi için [desteklenen dillere](language-support.md#text-to-speech)bakın.

* Nöral sesler - Derin sinir ağları konuşma dilinde stres ve tonlama açısından geleneksel konuşma sentezinin sınırlarını aşmak için kullanılır. Prosody tahmin ve ses sentezi aynı anda gerçekleştirilir, bu da daha akıcı ve doğal sondaj çıkışları ile sonuçlanır. Sinirsel sesler chatbots ve ses asistanları ile etkileşimleri daha doğal ve ilgi çekici hale getirmek için kullanılabilir, sesli kitaplar içine e-kitap gibi dijital metinler dönüştürmek, ve araç içi navigasyon sistemleri geliştirmek. İnsana benzer doğal prosody ve kelimelerin net eklemleme ile, sinirsel sesler önemli ölçüde AI sistemleri ile etkileşim zaman dinleme yorgunluğunu azaltır. Nöral seslerin tam listesi için [desteklenen dillere](language-support.md#text-to-speech)bakın.

* Konuşma Sentezi Biçimlendirme Dili (SSML) - Konuşmadan metne çıktılarını özelleştirmek için kullanılan XML tabanlı biçimlendirme dili. SSML ile, adım süresini ayarlayabilir, duraklatma ekleyebilir, telaffuzu artırabilir, konuşma hızını hızlandırabilir veya yavaşlatabilir, ses düzeyini artırabilir veya azaltabilir ve birden çok sesi tek bir belgeye atfedebilirsiniz. Bkz. [SSML](speech-synthesis-markup.md).

## <a name="get-started"></a>başlarken

Metin-konuşma hizmeti [Konuşma SDK](speech-sdk.md)üzerinden kullanılabilir. Çeşitli dillerde ve platformlarda hızlı başlangıçlar olarak kullanılabilen birkaç yaygın senaryo vardır:

* [Konuşmayı ses dosyasına sentezleme](quickstarts/text-to-speech-audio-file.md)
* [Konuşmayı hoparlöre sentezleme](quickstarts/text-to-speech.md)
* [Uzun biçimli sesi eş zamanlı olarak sentezle](quickstarts/text-to-speech/async-synthesis-long-form-audio.md)

İstersen, metin-konuşma hizmeti [REST](rest-text-to-speech.md)üzerinden erişilebilir.

## <a name="sample-code"></a>Örnek kod

Metinden konuşmaya örnek kod GitHub'da mevcuttur. Bu örnekler, en popüler programlama dillerinde metinden konuşmaya dönüştürmeyi kapsar.

- [Metinden konuşmaya örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Metinden konuşmaya örnekleri (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="customization"></a>Özelleştirme

Standart ve sinirsel seslere ek olarak, ürününize veya markanıza özgü özel sesler oluşturabilir ve ince ayar yapabilirsiniz. Başlamak için gereken tek şey bir avuç ses dosyası ve ilgili transkripsiyonlar. Daha fazla bilgi için bkz: [Özel Ses ile başlayın](how-to-custom-voice.md)

## <a name="pricing-note"></a>Fiyatlandırma notu

Metinden konuşmaya hizmeti kullanırken, noktalama işaretleri de dahil olmak üzere konuşmaya dönüştürülen her karakter için faturalandırılırsınız. SSML belgesinin kendisi faturalandırılabilir olmasa da, metnin telefon ve perde gibi konuşmaya nasıl dönüştürüldüğünü ayarlamak için kullanılan isteğe bağlı öğeler faturalandırılabilir karakterler olarak sayılır. Aşağıda faturalandırılabilir olanların bir listesi verebilme

- İsteğin SSML gövdesindeki metinden konuşmaya hizmetine geçirilen metin
- İstek gövdesinin metin alanındaki tüm biçimlendirme, SSML `<speak>` biçiminde, `<voice>`
- Harfler, noktalama işaretleri, boşluklar, sekmeler, biçimlendirme ve tüm boşluk karakterleri
- Unicode'da tanımlanan her kod noktası

Ayrıntılı bilgi için [Fiyatlandırma'ya](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)bakın.

> [!IMPORTANT]
> Her Çince, Japonca ve Korece karakter faturalandırma için iki karakter olarak sayılır.

## <a name="reference-docs"></a>Referans dokümanları

- [Konuşma SDK'sı](speech-sdk.md)
- [REST API: Metinden konuşmaya](rest-text-to-speech.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Ücretsiz Konuşma hizmeti aboneliği alın](get-started.md)
- [Konuşma SDK alın](speech-sdk.md)
