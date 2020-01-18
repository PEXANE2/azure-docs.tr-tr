---
title: Konuşma tanıma hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti, konuşmadan metne, metinden konuşmaya ve konuşma çevirisi için tek bir Azure aboneliğine sahip değildir. Konuşma SDK 'Sı, konuşma cihazları SDK 'Sı veya REST API 'Leri ile uygulamalarınıza, araçlara ve cihazlara konuşma ekleyin.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 11/05/2019
ms.author: erhopf
ms.openlocfilehash: df0ce5efefe86ed3b7e2e8ccf5a2bd9011e168e6
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76261441"
---
# <a name="what-is-the-speech-service"></a>Konuşma tanıma hizmeti nedir?

Konuşma hizmeti, konuşmadan metne, metinden konuşmaya ve konuşma çevirisi için tek bir Azure aboneliğine yönelik bir birleşme hizmetidir. Konuşma [SDK 'sı](speech-sdk-reference.md), [konuşma cihazları SDK](https://aka.ms/sdsdk-quickstart)'Sı veya [REST API 'leri](rest-apis.md)ile uygulamalarınızı, araçları ve cihazlarınızı kolayca yapabilirsiniz.

> [!IMPORTANT]
> Konuşma hizmeti Bing Konuşma API'si, Translator Konuşma Çevirisi ve Özel Konuşma Tanıma değiştirdi. Geçiş yönergeleri için bkz. _nasıl yapılır kılavuzlarından geçiş >_ .

Bu özellikler konuşma hizmetini yapar. Her bir özellik için genel kullanım örnekleri hakkında daha fazla bilgi edinmek veya API başvurusuna gitmek için bu tablodaki bağlantıları kullanın.

| Hizmet | Özellik | Açıklama | SDK | REST |
| ------- | ------- | ----------- | --- | ---- |
| [Konuşmayı metne dönüştürme](speech-to-text.md) | Konuşmayı metne dönüştürme | Konuşmadan metne dönüştürme, uygulamalarınızın, araçlarınızın veya cihazlarınızın tüketebileceği veya görüntüleyeceği gerçek zamanlı olarak metin halinde ses akışları. Kullanıcı amaçlarını [Language Understanding (lusıs)](https://docs.microsoft.com/azure/cognitive-services/luis/) ile birlikte kullanarak konuşma ve ses komutları üzerinde Kullanıcı hedefleri türetebilirsiniz. | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Toplu iş dökümü](batch-transcription.md) | Toplu iş dökümü, büyük hacimde verilerin zaman uyumsuz konuşmadan metne dökümünü sağlar. Bu, özelleştirme ve model yönetimi ile aynı uç noktayı kullanan REST tabanlı bir hizmettir. | Hayır | [Evet](https://westus.cris.ai/swagger/ui/index) |
| | [Çok cihazlı konuşma](multi-device-conversation.md) | Konuşma ve çeviri için kolay destek sayesinde konuşma veya metin tabanlı iletiler göndermek için bir konuşmadaki birden çok cihaza veya istemciye bağlanın| Evet | Hayır |
| | [Konuşma dökümü](conversation-transcription-service.md) | Gerçek zamanlı konuşma tanımayı, konuşmacı tanımlamayı ve okumayı mümkün hale getirme. Hoparlörleri, konuşmacıları ayırt etme imkanına sahip olmak için bir kişi içi toplantılar için mükemmeldir. | Evet | Hayır |
| | [Özel Konuşma Tanıma modelleri oluşturma](#customize-your-speech-experience) | Benzersiz bir ortamda tanıma ve döküm için konuşmaya metin kullanıyorsanız, çevresel gürültü veya sektöre özgü sözlük için özel akustik, dil ve telaffuz modeller oluşturup eğitebilirsiniz. | Hayır | [Evet](https://westus.cris.ai/swagger/ui/index) |
| [Metin Okuma](text-to-speech.md) | Metin okuma | Metin okuma [, konuşma birleştirme biçimlendirme dili (SSML)](speech-synthesis-markup.md)kullanarak giriş metnini insan benzeri sentezleştirilmiş konuşmaya dönüştürür. Standart sesler ve sinir sesler arasından seçim yapın (bkz. [dil desteği](language-support.md)). | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Özel sesler oluşturma](#customize-your-speech-experience) | Markanızı veya ürününüzün benzersiz özel ses yazı tiplerini oluşturun. | Hayır | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Konuşma Çevirisi](speech-translation.md) | Konuşma çevirisi | Konuşma çevirisi, uygulamalarınıza, araçlara ve cihazlara gerçek zamanlı, çok dilli konuşma çevirisi sağlar. Konuşmayı konuşmaya dönüştürme ve konuşmadan metne çevirme için bu hizmeti kullanın. | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | Hayır |
| [Ses yardımcıları](voice-assistants.md) | Ses yardımcıları | Konuşma hizmetini kullanan sesli yardımcılar, geliştiricilerin uygulamalar ve deneyimleri için doğal ve insan benzeri konuşma arabirimleri oluşturmalarına olanak sağlar. Ses Yardımcısı hizmeti, bir cihaz ile bir yardımcı uygulama arasında hızlı ve güvenilir bir etkileşim sağlar ve bu sayede, görev tamamlama için bir aygıt ve doğrudan hat konuşma kanalı veya tümleşik özel komutlar (Önizleme) hizmeti kullanılır. | [Evet](voice-assistants.md) | Hayır |

## <a name="news-and-updates"></a>Haberler ve güncelleştirmeler

Konuşma hizmeti yenilikleri hakkında bilgi edinin.

- Kasım 2019
  - `en-US-JessaNeural` sesinin [`newscast`](speech-synthesis-markup.md#adjust-speaking-styles) ve [`customerservice`](speech-synthesis-markup.md#adjust-speaking-styles) iki yeni konuşma stili eklendi.
- Eylül 2019
  - Yayınlanan konuşma SDK 1.7.0. Güncelleştirmelerin, geliştirmelerin ve bilinen sorunların tam listesi için bkz. [sürüm notları](releasenotes.md).
- Ağustos 2019
  - **Yeni öğretici**: [konuşma SDK 'sı ile sesinizi etkinleştirme, C# ](tutorial-voice-enable-your-bot-speech-sdk.md)
  - `en-US-JessaNeural` sesi için yeni bir konuşma stili [`chat`](speech-synthesis-markup.md#adjust-speaking-styles)eklendi.
- Haziran 2019
  - Yayınlanan konuşma SDK 1.6.0. Güncelleştirmelerin, geliştirmelerin ve bilinen sorunların tam listesi için bkz. [sürüm notları](releasenotes.md).
- 2019 Mayıs-belgeler artık [konuşma dökümü](conversation-transcription-service.md), [çağrı merkezi dökümü](call-center-transcription.md)ve [sesli yardımcılar](voice-assistants.md)için kullanılabilir.
- Mayıs 2019
  - Yayınlanan konuşma SDK 1.5.1. Güncelleştirmelerin, geliştirmelerin ve bilinen sorunların tam listesi için bkz. [sürüm notları](releasenotes.md).
  - Yayınlanan konuşma SDK 1.5.0. Güncelleştirmelerin, geliştirmelerin ve bilinen sorunların tam listesi için bkz. [sürüm notları](releasenotes.md).

## <a name="try-the-speech-service"></a>Konuşma hizmetini deneyin

Her biri 10 dakikadan kısa bir süre içinde kod çalıştırmak için tasarlanan en popüler programlama dillerinde hızlı başlangıçları sunuyoruz. Bu tablo her bir özellik için en popüler hızlı başlangıçlara sahiptir. Ek dilleri ve platformları araştırmak için sol taraftaki gezintiyi kullanın.

| Konuşmayı metne dönüştürme (SDK) | Metinden konuşmaya (SDK) | Çeviri (SDK) |
| -------------------- | -------------------- | ----------------- |
| [Ses dosyasından konuşmayı tanıma](quickstarts/speech-to-text-from-file.md) | [Konuşmayı bir ses dosyasına sentezleştirme](quickstarts/text-to-speech-audio-file.md) | [Konuşmayı metne çevir](quickstarts/translate-speech-to-text.md) |
| [Konuşmayı bir mikrofonla tanıma](quickstarts/speech-to-text-from-microphone.md) | [Konuşmayı konuşmacıyı sentezleştirme](quickstarts/text-to-speech.md) | [Konuşmayı birden çok hedef dile çevirme](quickstarts/translate-speech-to-text-multiple-languages.md) |
| [Blob depolamada depolanan konuşmayı tanıma](quickstarts/from-blob.md) | [Uzun biçimli ses için zaman uyumsuz sensıs](quickstarts/text-to-speech/async-synthesis-long-form-audio.md) | [Konuşmayı konuşmaya çevir](quickstarts/translate-speech-to-speech.md) |

> [!NOTE]
> Konuşmadan metne ve metinden konuşmaya Ayrıca REST uç noktalarına ve ilgili hızlı başlangıçlara sahiptir.

Konuşma hizmetini kullanma şansınız olduktan sonra, konuşma SDK 'sını ve LUO kullanarak konuşmayı nasıl anlayacağınızı öğreten Öğreticimizi deneyin.

- [Öğretici: konuşma SDK 'Sı ve LUSıS ile konuşma amaçlarını tanımaC#](how-to-recognize-intents-from-speech-csharp.md)
- [Öğretici: konuşma SDK 'Sı ile sesinizi etkinleştirme,C#](tutorial-voice-enable-your-bot-speech-sdk.md)
- [Öğretici: metin çevirmek, yaklaşımı çözümlemek ve çevrilmiş metni konuşmaya dönüştürmek için bir Flask uygulaması oluşturun, REST](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2fazure%2fcognitive-services%2fspeech-service%2ftoc.json&bc=%2fazure%2fcognitive-services%2fspeech-service%2fbreadcrumb%2ftoc.json&toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fspeech-service%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)

## <a name="get-sample-code"></a>Örnek kodu alma

Örnek kod, konuşma hizmeti için GitHub 'da kullanılabilir. Bu örnekler, bir dosya veya akıştan ses okuma, sürekli ve tek kararlı bir tanıma ve özel modellerle çalışma gibi yaygın senaryoları kapsar. SDK ve REST örneklerini görüntülemek için bu bağlantıları kullanın:

- [Konuşmadan metne, metinden konuşmaya ve konuşma çevirisi örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Toplu iş dökümü örnekleri (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
- [Metinden konuşmaya örnekleri (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)
- [Ses Yardımcısı örnekleri (SDK)](https://aka.ms/csspeech/samples)

## <a name="customize-your-speech-experience"></a>Konuşma deneyiminizi özelleştirin

Konuşma hizmeti yerleşik modellerle iyi çalışmaktadır, ancak ürününüzün veya ortamınızın deneyimini daha fazla özelleştirmek ve ayarlamak isteyebilirsiniz. Özelleştirme seçenekleri, akustik model ayarlamalarından markanızla ilgili benzersiz ses yazı tiplerine kadar değişir.

| Konuşma Hizmeti | Platform | Açıklama |
| -------------- | -------- | ----------- |
| Konuşmayı Metne Dönüştürme | [Özel konuşma tanıma](https://aka.ms/customspeech) | Konuşma tanıma modellerini gereksinimlerinize ve kullanılabilir verilerinize göre özelleştirin. Konuşma tarzı, sözlük ve arka plan gürültüsü gibi konuşma tanıma engellerini aşın. |
| Metin Okuma | [Özel ses](https://aka.ms/customvoice) | Kullanılabilir konuşma verilerinizle Metin Okuma uygulamalarınız için tanınabilir, kendine özgü bir ses oluşturun. Ses çıkışlarını bir dizi parametre kümesini ayarlayarak daha fazla hassas ayarlamalar yapabilirsiniz. |

## <a name="reference-docs"></a>Başvuru belgeleri

- [Konuşma SDK'sı](speech-sdk-reference.md)
- [Konuşma cihazları SDK 'Sı](speech-devices-sdk.md)
- [REST API: konuşmayı metne dönüştürme](rest-speech-to-text.md)
- [REST API: metinden konuşmaya](rest-text-to-speech.md)
- [REST API: toplu Iş dökümü ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](get-started.md)
