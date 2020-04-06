---
title: Konuşma SDK Hakkında - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma yazılım geliştirme kiti (SDK), Konuşma hizmeti özelliklerinin çoğunu ortaya çıkararak konuşma özellikli uygulamaların geliştirilmesini kolaylaştırır.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: dapine
ms.openlocfilehash: 440944033e4b6a8fb29cf9ace6ad784b91316526
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668758"
---
# <a name="about-the-speech-sdk"></a>Konuşma SDK'sı hakkında

Konuşma yazılım geliştirme kiti (SDK), konuşma özellikli uygulamalar geliştirmenizi sağlamak için Konuşma hizmeti yeteneklerinin çoğunu ortaya çıkarır. Konuşma SDK birçok programlama dilleri ve tüm platformlarda mevcuttur.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

## <a name="scenario-capabilities"></a>Senaryo yetenekleri

Konuşma SDK Konuşma hizmetibirçok özellikleri ortaya çıkarır, ancak hepsi değil. Konuşma SDK yetenekleri genellikle senaryolar ile ilişkilidir. Speech SDK, yerel aygıtlar, dosyalar, Azure blob depolama alanı ve hatta giriş ve çıkış akışlarını kullanarak hem gerçek zamanlı hem de gerçek zamanlı olmayan senaryolar için idealdir. Konuşma SDK ile bir senaryo elde edilemediğinde, REST API alternatifi arayın.

### <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

[Konuşma-metin](speech-to-text.md) *(konuşma tanıma*olarak da bilinir) ses akışlarını uygulamalarınızın, araçlarınızın veya aygıtlarınızın tüketebileceği veya görüntüleyebileceği metne çevirir. Kullanıcı niyetlerini yazılı konuşmalardan elde etmek ve sesli komutlarla hareket etmek için [Dil Bilgisi (LUIS)](../luis/index.yml) ile metinden metne kullanın. Konuşma girişini tek bir çağrıyla farklı bir dile çevirmek için [Konuşma Çevirisi'ni](speech-translation.md) kullanın. Daha fazla bilgi için [metinden metne temelleri'ne](speech-to-text-basics.md)bakın.

### <a name="text-to-speech"></a>Metin okuma

[Metinden konuşmaya](text-to-speech.md) *(konuşma sentezi*olarak da bilinir) metni insan benzeri sentezlenmiş konuşmaya dönüştürür. Giriş metni ya dize literals veya [Konuşma Sentezi Biçimlendirme Dili (SSML)](speech-synthesis-markup.md)kullanarak. Standart veya sinirsel sesler hakkında daha fazla bilgi için [Metinden konuşmaya dil ve ses desteğine](language-support.md#text-to-speech)bakın.

### <a name="voice-assistants"></a>Ses yardımcıları

Konuşma SDK'sını kullanan sesli asistanlar, geliştiricilerin uygulamaları ve deneyimleri için doğal, insana benzer konuşma arabirimleri oluşturmalarına olanak tanır. Ses asistanı hizmeti, aygıt ve asistan arasında hızlı ve güvenilir etkileşim sağlar. Uygulama, görevin tamamlanması için Bot Framework'ün Doğrudan Satır Konuşması kanalını veya tümleşik Özel Komutlar (Önizleme) hizmetini kullanır. Ayrıca, benzersiz bir ses deneyimi oluşturmak için [Özel Ses Portalı](https://aka.ms/customvoice) kullanılarak sesli asistanlar oluşturulabilir.

#### <a name="keyword-spotting"></a>Anahtar kelime tespit

[Anahtar kelime tespit](speech-devices-sdk-create-kws.md) kavramı Konuşma SDK desteklenir. Anahtar kelime tespit, konuşmada bir anahtar kelimeyi tanımlama eylemidir ve ardından anahtar kelimenin işitilmesi üzerine bir eylem dir. Örneğin, "Hey Cortana" Cortana yardımcısını etkinleştirir.

### <a name="meeting-scenarios"></a>Toplantı senaryoları

Konuşma SDK, ister tek bir cihazdan ister çok cihazlı konuşmadan olsun, toplantı senaryolarını yazıya geçirmek için idealdir.

#### <a name="conversation-transcription"></a>Konuşma Transkripsiyonu

[Konuşma Transkripsiyonu](conversation-transcription.md) gerçek zamanlı (ve eşzamanlı) konuşma tanıma, hoparlör tanımlama ve her konuşmacıya cümle atıfta *(ayrıca ishal*olarak da bilinir) sağlar. Hoparlörleri ayırt etme yeteneğiyle bizzat toplantılar yapmak için idealdir.

#### <a name="multi-device-conversation"></a>Çok cihazlı Konuşma

[Çoklu aygıtlı Konuşma](multi-device-conversation.md)ile, bir konuşmada birden çok aygıtı veya istemciyi bağlayarak konuşma tabanlı veya metin tabanlı iletiler gönderin ve transkripsiyon ve çeviri için kolay destek sağlayın.

### <a name="custom--agent-scenarios"></a>Özel / aracı senaryoları

Konuşma SDK, telefon verilerinin oluşturulduğu çağrı merkezi senaryolarını çevirmek için kullanılabilir.

#### <a name="call-center-transcription"></a>Çağrı merkezi Transkripsiyonu

[Çağrı Merkezi Transkripsiyonu,](call-center-transcription.md) Etkileşimli Sesli Yanıt (IVR) gibi çeşitli sistemlerden gelebilecek büyük hacimli telefon verilerini aktarmak için konuşma-metin için yaygın bir senaryodur. Konuşma hizmetinin en son konuşma tanıma modelleri, verilerin anlaşılmasının zor olduğu durumlarda bile bu telefon verilerini aktarmada başarılı olur.

### <a name="codec-compressed-audio-input"></a>Codec sıkıştırılmış ses girişi

Konuşma SDK programlama dillerinin bir çoğu codec sıkıştırılmış ses giriş akışlarını destekler. Daha fazla bilgi için <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-use-codec-compressed-audio-input-streams" target="_blank">sıkıştırılmış ses <span class="docon docon-navigate-external x-hidden-focus"> </span>giriş biçimleri kullanın. </a>

## <a name="rest-api"></a>REST API

Konuşma SDK Konuşma Hizmeti'nin birçok özellik özelliğini kapsarken, bazı senaryolar için REST API'sini kullanmak isteyebilirsiniz.

### <a name="batch-transcription"></a>Toplu iş transkripsiyonu

[Toplu transkripsiyon,](batch-transcription.md) büyük hacimli verilerin eş zamanlı olarak konuşmadan metne transkripsiyonuna olanak tanır. Toplu transkripsiyon sadece REST API mümkündür. Konuşma sesini metne dönüştürmenin yanı sıra, toplu konuşmadan metne de ishal ve duygu analizi sağlar.

## <a name="customization"></a>Özelleştirme

Konuşma Hizmeti, konuşmadan metne, metinden konuşmaya ve konuşma çevirisinde varsayılan modelleri ile harika işlevsellik sunar. Bazen benzersiz kullanım kılıfınızla daha iyi çalışmak için temel performansı artırmak isteyebilirsiniz. Konuşma Hizmeti, kolaylaştıran ve kendi verilerinize dayalı özel modellerle rekabet avantajı oluşturmanıza olanak tanıyan çeşitli kodsuz özelleştirme araçlarına sahiptir. Bu modeller yalnızca sizin ve kuruluşunuz için kullanılabilir olacaktır.

### <a name="custom-speech-to-text"></a>Özel Konuşma-metin

Benzersiz bir ortamda tanıma ve transkripsiyon için konuşma-metin kullanırken, ortam gürültüsünü veya sektöre özgü kelime dağarcığını gidermek için özel akustik, dil ve telaffuz modelleri oluşturabilir ve eğitebilirsiniz. No-code Özel Konuşma modelleri oluşturma ve yönetimi [Özel Konuşma Portalı](https://aka.ms/customspeech)üzerinden kullanılabilir. Özel Konuşma modeli yayımlandıktan sonra Konuşma SDK tarafından tüketilebilir.

### <a name="custom-text-to-speech"></a>Özel Metinden konuşmaya

Özel metinden konuşmaya, özel ses olarak da bilinir, markanız için tanınabilir, türünün tek bir sesi oluşturmanıza olanak tanıyan çevrimiçi araçlar kümesidir. No-code Özel Ses modelleri oluşturma ve yönetimi [Özel Ses Portalı](https://aka.ms/customvoice)üzerinden kullanılabilir. Özel Ses modeli yayınlandıktan sonra Konuşma SDK tarafından tüketilebilir.

## <a name="get-the-speech-sdk"></a>Konuşma SDK alın

# <a name="windows"></a>[Windows](#tab/windows)

[!INCLUDE [Get the Speech SDK](includes/get-speech-sdk-windows.md)]

# <a name="linux"></a>[Linux](#tab/linux)

[!INCLUDE [Get the Speech SDK](includes/get-speech-sdk-linux.md)]

# <a name="ios"></a>[iOS](#tab/ios)

[!INCLUDE [Get the Speech SDK](includes/get-speech-sdk-ios.md)]

# <a name="macos"></a>[Macos](#tab/macos)

[!INCLUDE [Get the Speech SDK](includes/get-speech-sdk-macos.md)]

# <a name="android"></a>[Android](#tab/android)

[!INCLUDE [Get the Speech SDK](includes/get-speech-sdk-android.md)]

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

[!INCLUDE [Get the Node.js Speech SDK](includes/get-speech-sdk-nodejs.md)]

# <a name="browser"></a>[Tarayıcı](#tab/browser)

[!INCLUDE [Get the Browser Speech SDK](includes/get-speech-sdk-browser.md)]

---

[!INCLUDE [License notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

[!INCLUDE [Sample source code](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C'deki konuşmayı nasıl tanıyacağınızı görme #](quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
