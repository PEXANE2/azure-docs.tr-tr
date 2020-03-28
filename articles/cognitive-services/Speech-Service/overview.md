---
title: Konuşma tanıma hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti, konuşma-metin, metin-konuşma ve konuşma çevirisinin tek bir Azure aboneliğinde birleştirilmesidir. Speech SDK, Speech Devices SDK veya REST API'leri ile uygulamalarınız, araçlarınız ve cihazlarınız için konuşma ekleyin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 03/12/2020
ms.author: dapine
ms.openlocfilehash: 353e66c2d4ebb288244866b12a32e9acb7c765e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371565"
---
# <a name="what-is-the-speech-service"></a>Konuşma tanıma hizmeti nedir?

Konuşma hizmeti, konuşma-metin, metin-konuşma ve konuşma çevirisinin tek bir Azure aboneliğinde birleştirilmesidir. [Konuşma SDK, Konuşma](speech-sdk-reference.md) [Cihazları SDK](https://aka.ms/sdsdk-quickstart)veya [REST API'leri](rest-apis.md)ile uygulamalarınızı, araçlarınızı ve cihazlarınızı etkinleştirmek kolaydır.

> [!IMPORTANT]
> Konuşma hizmeti Bing Konuşma API'sinin ve Çevirmen Konuşma'nın yerini aldı. Geçiş yönergeleri için _Geçiş > Nasıl Dır kılavuzlarına_ bakın.

Bu özellikler Konuşma hizmetini oluşturan. Her özellik için yaygın kullanım örnekleri hakkında daha fazla bilgi edinmek veya API başvurusuna göz atmak için bu tablodaki bağlantıları kullanın.

| Hizmet | Özellik | Açıklama | SDK | REST |
|---------|---------|-------------|-----|------|
| [Konuşmadan Metne](speech-to-text.md) | Gerçek zamanlı Konuşma-to-metin | Konuşmadan metne, ses akışlarını veya yerel dosyaları uygulamalarınızın, araçlarınızın veya aygıtlarınızın tüketebileceği veya görüntülebildiği gerçek zamanlı olarak metne çevirir veya çevirir. Kullanıcı niyetlerini yazılı konuşmalardan elde etmek ve sesli komutlarla hareket etmek için [Dil Bilgisi (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/) ile metinden metne kullanın. | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Toplu Konuşmadan Metne](batch-transcription.md) | Toplu Konuşmadan metne, Azure Blob Depolama'da depolanan büyük hacimli konuşma ses verilerinin eş zamanlı konuşmadan metne transkripsiyonu sağlar. Konuşma sesini metne dönüştürmenin yanı sıra, Toplu Konuşma-metin de ishal ve duygu analizi sağlar. | Hayır | [Evet](https://westus.cris.ai/swagger/ui/index) |
| | [Çok cihazlı Konuşma](multi-device-conversation.md) | Bir konuşmada birden çok aygıtı veya istemciyi, transkripsiyon ve çeviri için kolay destekle konuşma veya metin tabanlı iletiler göndermek için bağlama| Evet | Hayır |
| | [Konuşma Transkripsiyonu](conversation-transcription-service.md) | Gerçek zamanlı konuşma tanıma, hoparlör tanımlama ve ishal sağlar. Hoparlörleri ayırt etme yeteneğiyle bizzat toplantılar yapmak için idealdir. | Evet | Hayır |
| | [Özel Konuşma Modelleri Oluşturma](#customize-your-speech-experience) | Benzersiz bir ortamda tanıma ve transkripsiyon için metinden metne sözcük kullanıyorsanız, ortam gürültüsünü veya sektöre özgü kelime dağarcığını gidermek için özel akustik, dil ve telaffuz modelleri oluşturabilir ve eğitebilirsiniz. | Hayır | [Evet](https://westus.cris.ai/swagger/ui/index) |
| [Metinden Konuşmaya](text-to-speech.md) | Metin okuma | Metinden konuşmaya giriş metnini [Konuşma Sentezi Biçimlendirme Dili (SSML)](speech-synthesis-markup.md)kullanarak insana benzer sentezlenmiş konuşmaya dönüştürür. Standart sesler ve sinirsel sesler arasından seçim yapın [(Bkz. Dil desteği).](language-support.md) | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Özel Sesler Oluşturma](#customize-your-speech-experience) | Markanıza veya ürününize özel özel sesli yazı tipleri oluşturun. | Hayır | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Konuşma Çevirisi](speech-translation.md) | Konuşma çevirisi | Konuşma çevirisi, uygulamalarınıza, araçlarınıza ve cihazlarınıza konuşmanın gerçek zamanlı, çok dilli çevrilmesini sağlar. Konuşma-konuşma ve konuşma-metin çeviri için bu hizmeti kullanın. | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | Hayır |
| [Ses yardımcıları](voice-assistants.md) | Ses yardımcıları | Konuşma hizmetini kullanan ses asistanları, geliştiricileri uygulamaları ve deneyimleri için doğal, insana benzer konuşma arabirimleri oluşturma konusunda güçlendirir. Sesli asistan hizmeti, görevin tamamlanması için Bot Framework'ün Doğrudan Satır Konuşması kanalını veya tümleşik Özel Komutlar (Önizleme) hizmetini kullanan bir aygıt la yardımcı uygulama arasında hızlı ve güvenilir etkileşim sağlar. | [Evet](voice-assistants.md) | Hayır |


[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

## <a name="try-the-speech-service"></a>Konuşma hizmetini deneyin

Her biri 10 dakikadan kısa bir sürede kod çalıştırmanız için tasarlanmış en popüler programlama dillerinde hızlı başlangıçlar sunuyoruz. Bu tablo, her özellik için en popüler hızlı başlangıçları içerir. Ek dilleri ve platformları keşfetmek için sol daki gezinmeyi kullanın.

| Konuşma-metin (SDK) | Metin-Konuşma (SDK) | Çeviri (SDK) |
|----------------------|----------------------|-------------------|
| [Ses dosyasından alınan konuşmayı tanıma](quickstarts/speech-to-text-from-file.md) | [Konuşmayı ses dosyasına sentezleme](quickstarts/text-to-speech-audio-file.md) | [Konuşmayı metne çevir](quickstarts/translate-speech-to-text.md) |
| [Mikrofonla yapılan konuşmayı tanıma](quickstarts/speech-to-text-from-microphone.md) | [Konuşmayı hoparlöre sentezleme](quickstarts/text-to-speech.md) | [Konuşmayı birden fazla hedef dile çevirme](quickstarts/translate-speech-to-text-multiple-languages.md) |
| [Blob depolama alanında depolanan konuşmayı tanıma](quickstarts/from-blob.md) | [Uzun ses için zaman uyumsuz sentez](quickstarts/text-to-speech/async-synthesis-long-form-audio.md) | [Konuşmayı konuşmaya çevirme](quickstarts/translate-speech-to-speech.md) |

> [!NOTE]
> Konuşma-metin ve metin-to-speech da REST uç noktaları ve ilişkili quickstarts var.

Konuşma hizmetini kullanma fırsatınız olduktan sonra, Konuşma SDK ve LUIS'i kullanarak konuşma nın amacını nasıl tanıyabileceğinizi öğreten öğreticimizi deneyin.

- [Öğretici: Konuşma SDK ve LUIS, C ile konuşma niyetleri tanıyın #](how-to-recognize-intents-from-speech-csharp.md)
- [Öğretici: Ses Konuşma SDK, C ile bot etkinleştirin #](tutorial-voice-enable-your-bot-speech-sdk.md)
- [Öğretici: Metni çevirmek, duyarlılığı analiz etmek ve çevrilmiş metni konuşmaya sentezlemek için bir Flask uygulaması oluşturun, REST](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2fazure%2fcognitive-services%2fspeech-service%2ftoc.json&bc=%2fazure%2fcognitive-services%2fspeech-service%2fbreadcrumb%2ftoc.json&toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fspeech-service%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)

## <a name="get-sample-code"></a>Örnek kodu alma

Konuşma hizmeti için GitHub'da örnek kod mevcuttur. Bu örnekler, bir dosya veya akıştan ses okuma, sürekli ve tek çekim tanıma ve özel modeller ile çalışma gibi yaygın senaryoları kapsar. SDK ve REST örneklerini görüntülemek için bu bağlantıları kullanın:

- [Konuşma-metin, metin-konuşma ve konuşma çeviri örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Toplu transkripsiyon örnekleri (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
- [Metinden konuşmaya örnekleri (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)
- [Ses asistanı örnekleri (SDK)](https://aka.ms/csspeech/samples)

## <a name="customize-your-speech-experience"></a>Konuşma deneyiminizi özelleştirin

Konuşma hizmeti yerleşik modellerde iyi çalışır, ancak, ürün veya ortamınız için deneyimi daha da özelleştirmek ve ayarlamak isteyebilirsiniz. Özelleştirme seçenekleri, akustik model atondan markanız için benzersiz ses yazı tiplerine kadar çeşitlilik sağlar.

| Konuşma Hizmeti | Platform | Açıklama |
| -------------- | -------- | ----------- |
| Konuşmayı Metne Dönüştürme | [Özel Konuşma](https://aka.ms/customspeech) | Konuşma tanıma modellerini gereksinimlerinize ve kullanılabilir verilere göre özelleştirin. Konuşma stili, kelime dağarcığı ve arka plan gürültüsü gibi konuşma tanıma engellerini aşmak. |
| Metin Okuma | [Özel Ses](https://aka.ms/customvoice) | Kullanılabilir konuşma verilerinizle Metin Okuma uygulamalarınız için tanınabilir, kendine özgü bir ses oluşturun. Bir dizi ses parametrelerini ayarlayarak ses çıktılarında daha fazla ince ayar yapabilirsiniz. |

## <a name="reference-docs"></a>Referans dokümanları

- [Konuşma SDK'sı](speech-sdk-reference.md)
- [Konuşma Cihazları SDK’sı](speech-devices-sdk.md)
- [REST API: Konuşma-metin](rest-speech-to-text.md)
- [REST API: Metinden konuşmaya](rest-text-to-speech.md)
- [REST API: Toplu transkripsiyon ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma hizmeti abonelik anahtarını ücretsiz alın](get-started.md)
