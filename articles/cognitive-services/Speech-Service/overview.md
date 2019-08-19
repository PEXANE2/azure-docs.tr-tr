---
title: Konuşma Tanıma Hizmetleri nelerdir?
titleSuffix: Azure Cognitive Services
description: Konuşma Hizmetleri, konuşmadan metne, metinden konuşmaya ve konuşma çevirisi için tek bir Azure aboneliğine sahip değildir. Konuşma SDK 'Sı, konuşma cihazları SDK 'sı veya REST API 'Leri ile uygulamalarınızı, araçlarınızı ve cihazlarınızı kolayca ekleyebilirsiniz. Mevcut bir sohbet bot 'a konuşma işlevselliği ekleyin, bir çeviri uygulamasında metin konuşmaya dönüştürün veya çok büyük miktarlarda çağrı merkezi verisi koyun.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 4ea6f04dbfd18a6e561a255988ba0465b9e017be
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967623"
---
# <a name="what-are-the-speech-services"></a>Konuşma Tanıma Hizmetleri nelerdir?

Konuşma Hizmetleri, konuşmadan metne, metinden konuşmaya ve konuşma çevirisi için tek bir Azure aboneliğine yönelik bir birleşme özelliği. Konuşma [SDK 'sı](speech-sdk-reference.md), [konuşma cihazları SDK](https://aka.ms/sdsdk-quickstart)'Sı veya [REST API 'leri](rest-apis.md)ile uygulamalarınızı, araçları ve cihazlarınızı kolayca yapabilirsiniz.

> [!IMPORTANT]
> Konuşma Hizmetleri Bing Konuşma API'si, Translator Konuşma Çevirisi ve Özel Konuşma Tanıma değiştirdi. Geçiş yönergeleri için bkz. *nasıl yapılır kılavuzlarından geçiş >* .

Bu özellikler Azure konuşma hizmetlerini yapar. Her bir özellik için genel kullanım örnekleri hakkında daha fazla bilgi edinmek veya API başvurusuna gitmek için bu tablodaki bağlantıları kullanın.

| Hizmet | Özellik | Açıklama | SDK | REST |
|---------|---------|-------------|-----|------|
| [Konuşmayı metne dönüştürme](speech-to-text.md) | Konuşmayı Metne Dönüştürme | Konuşmadan metne dönüştürme, uygulamalarınızın, araçlarınızın veya cihazlarınızın tüketebileceği veya görüntüleyeceği gerçek zamanlı olarak metin halinde ses akışları. Kullanıcı amaçlarını [Language Understanding (lusıs)](https://docs.microsoft.com/azure/cognitive-services/luis/) ile birlikte kullanarak konuşma ve ses komutları üzerinde Kullanıcı hedefleri türetebilirsiniz. | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Toplu iş dökümü](batch-transcription.md) | Toplu iş dökümü, büyük hacimde verilerin zaman uyumsuz konuşmadan metne dökümünü sağlar. Bu, özelleştirme ve model yönetimi ile aynı uç noktayı kullanan REST tabanlı bir hizmettir. | Hayır | [Evet](https://westus.cris.ai/swagger/ui/index) |
| | [Konuşma dökümü](conversation-transcription-service.md) | Gerçek zamanlı konuşma tanımayı, konuşmacı tanımlamayı ve okumayı mümkün hale getirme. Hoparlörleri, konuşmacıları ayırt etme imkanına sahip olmak için bir kişi içi toplantılar için mükemmeldir. | Evet | Hayır |
| | [Özel Konuşma Tanıma modelleri oluşturma](#customize-your-speech-experience) | Benzersiz bir ortamda tanıma ve döküm için konuşmaya metin kullanıyorsanız, çevresel gürültü veya sektöre özgü sözlük için özel akustik, dil ve telaffuz modeller oluşturup eğitebilirsiniz. | Hayır | [Evet](https://westus.cris.ai/swagger/ui/index) |
| [Metin okuma](text-to-speech.md) | Metin okuma | Metin okuma [, konuşma birleştirme biçimlendirme dili (SSML)](text-to-speech.md#speech-synthesis-markup-language-ssml)kullanarak giriş metnini insan benzeri sentezleştirilmiş konuşmaya dönüştürür. Standart sesler ve sinir sesler arasından seçim yapın (bkz. [dil desteği](language-support.md)). | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Özel sesler oluşturma](#customize-your-speech-experience) | Markanızı veya ürününüzün benzersiz özel ses yazı tiplerini oluşturun. | Hayır | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Konuşma Çevirisi](speech-translation.md) | Konuşma çevirisi | Konuşma çevirisi, uygulamalarınıza, araçlara ve cihazlara gerçek zamanlı, çok dilli konuşma çevirisi sağlar. Konuşmayı konuşmaya dönüştürme ve konuşmadan metne çevirme için bu hizmeti kullanın. | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | Hayır |
| [Ses-ilk sanal yardımcılar](voice-first-virtual-assistants.md) | Ses-ilk sanal yardımcılar | Azure konuşma Hizmetleri 'ni kullanan özel sanal yardımcılar, geliştiricilerin uygulamaları ve deneyimleri için doğal ve insan benzeri konuşma arabirimleri oluşturmalarına olanak sağlar. Bot çerçevesinin doğrudan hat konuşma kanalı, daha düşük gecikme süresi ve yüksek güvenilirliğe sahip ses girişi, ses çıkış etkileşimini sağlayan uyumlu bir bot 'a Eşgüdümlü, düzenlenmiş bir giriş noktası sağlayarak bu özellikleri geliştirir. | [Evet](voice-first-virtual-assistants.md) | Hayır |

## <a name="news-and-updates"></a>Haberler ve güncelleştirmeler

Azure konuşma Hizmetleri yenilikleri hakkında bilgi edinin.

* Ağustos 2019
  * **Yeni öğretici**: [Konuşmayı konuşma SDK 'Sı ile etkinleştirin,C#](tutorial-voice-enable-your-bot-speech-sdk.md)
* Haziran 2019
  * Yayınlanan konuşma SDK 1.6.0. Güncelleştirmelerin, geliştirmelerin ve bilinen sorunların tam listesi için bkz. [sürüm notları](releasenotes.md).
* 2019 Mayıs-belgeler artık [konuşma dökümü](conversation-transcription-service.md), [çağrı merkezi dökümü](call-center-transcription.md)ve [sesli ilk sanal yardımcılar](voice-first-virtual-assistants.md)için kullanılabilir.
* Mayıs 2019
  * Yayınlanan konuşma SDK 1.5.1. Güncelleştirmelerin, geliştirmelerin ve bilinen sorunların tam listesi için bkz. [sürüm notları](releasenotes.md).
  * Yayınlanan konuşma SDK 1.5.0. Güncelleştirmelerin, geliştirmelerin ve bilinen sorunların tam listesi için bkz. [sürüm notları](releasenotes.md).

## <a name="try-speech-services"></a>Konuşma hizmetlerini deneyin

Her biri 10 dakikadan kısa bir süre içinde kod çalıştırmak için tasarlanan en popüler programlama dillerinde hızlı başlangıçları sunuyoruz. Bu tablo her bir özellik için en popüler hızlı başlangıçlara sahiptir. Ek dilleri ve platformları araştırmak için sol taraftaki gezintiyi kullanın.

| Konuşmayı metne dönüştürme (SDK) | Metinden konuşmaya (SDK) | Çeviri (SDK) |
|----------------------|----------------------|-------------------|
| [C#, .NET Core (Windows)](quickstart-csharp-dotnet-windows.md) | [C#, .NET Framework (Windows)](quickstart-text-to-speech-dotnet-windows.md) | [Java (Windows, Linux)](quickstart-translate-speech-java-jre.md) |
| [JavaScript (tarayıcı)](quickstart-js-browser.md) | [C++Pencerelerin](quickstart-text-to-speech-cpp-windows.md) | [C#, .NET Core (Windows)](quickstart-translate-speech-dotnetcore-windows.md) |
| [Python (Windows, Linux, macOS)](quickstart-python.md) | [C++'Un](quickstart-text-to-speech-cpp-linux.md) | [C#, .NET Framework (Windows)](quickstart-translate-speech-dotnetframework-windows.md) |
| [Java (Windows, Linux)](quickstart-java-jre.md) | | [C++Pencerelerin](quickstart-translate-speech-cpp-windows.md) |

> [!NOTE]
> Konuşmadan metne ve metinden konuşmaya Ayrıca REST uç noktalarına ve ilgili hızlı başlangıçlara sahiptir.

Konuşma hizmetlerini kullanma şansınız olduktan sonra, konuşma SDK 'sını ve LUO kullanarak konuşmayı nasıl anlayacağınızı öğreten Öğreticimizi deneyin.

* [Öğretici: Konuşma SDK 'Sı ve LUSıS ile konuşmadan amaçları tanımaC#](how-to-recognize-intents-from-speech-csharp.md)
* [Öğretici: Konuşmayı konuşma SDK 'Sı ile etkinleştirin,C#](tutorial-voice-enable-your-bot-speech-sdk.md)
* [Öğretici: Metin çevirmek, yaklaşımı çözümlemek ve çevrilmiş metni konuşmaya, REST 'e eklemek için bir Flask uygulaması oluşturun](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2fazure%2fcognitive-services%2fspeech-service%2ftoc.json&bc=%2fazure%2fcognitive-services%2fspeech-service%2fbreadcrumb%2ftoc.json&toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fspeech-service%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)

## <a name="get-sample-code"></a>Örnek kodu alma

Örnek kod, Azure konuşma hizmetlerinin her biri için GitHub 'da kullanılabilir. Bu örnekler, bir dosya veya akıştan ses okuma, sürekli ve tek kararlı bir tanıma ve özel modellerle çalışma gibi yaygın senaryoları kapsar. SDK ve REST örneklerini görüntülemek için bu bağlantıları kullanın:

* [Konuşmadan metne, metinden konuşmaya ve konuşma çevirisi örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Toplu iş dökümü örnekleri (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
* [Metinden konuşmaya örnekleri (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)
* [Ses-ilk Sanal Yardımcısı örnekleri (SDK)](https://aka.ms/csspeech/samples)

## <a name="customize-your-speech-experience"></a>Konuşma deneyiminizi özelleştirin

Azure konuşma Hizmetleri yerleşik modellerle iyi çalışmaktadır, ancak ürün veya ortamınızın deneyimini daha fazla özelleştirmek ve ayarlamak isteyebilirsiniz. Özelleştirme seçenekleri, akustik model ayarlamalarından markanızla ilgili benzersiz ses yazı tiplerine kadar değişir. Özel bir model oluşturduktan sonra Azure konuşma hizmetlerinden herhangi biriyle kullanabilirsiniz.

| Konuşma Hizmeti | Model | Açıklama |
|----------------|-------|-------------|
| Konuşmayı Metne Dönüştürme | [Akustik model](how-to-customize-acoustic-models.md) | Her biri belirli kayıt koşullarına sahip bir otomobil veya bir fabrika katında gibi belirli ortamlarda kullanılan uygulamalar, araçlar veya cihazlar için özel bir akustik model oluşturun. Bu örneklere, vurgulu konuşma, belirli arka plan gürültüler veya kayıt için belirli bir mikrofon kullanımı dahildir. |
| | [Dil modeli](how-to-customize-language-model.md) | Alana özgü sözlük ve dilbilgisinde tıp terminolojisi ya da It jarggibi bir dökümü geliştirmek için özel bir dil modeli oluşturun. |
| | [Söyleniş modeli](how-to-customize-pronunciation.md) | Özel bir telaffuz modeliyle, bir sözcük veya terimin fonetik biçimini tanımlayabilir ve görüntüleyebilirsiniz. Ürün adları veya kısaltmalar gibi özelleştirilmiş koşullarını işlemek için kullanışlıdır. Başlamak için ihtiyacınız olan telaffuz dosya--basit .txt dosyası. |
| Metin Okuma | [Ses tipi](how-to-customize-voice-font.md) | Özel ses yazı tipleri, markanız için tanınabilir, tek bir tür sesi oluşturmanızı sağlar. Başlamak için yalnızca az miktarda veri alır. Sağladığınız daha fazla veri, ses yazı tipi daha doğal ve insan gibi bir şekilde gönderilir. |

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Konuşma cihazları SDK 'Sı](speech-devices-sdk.md)
* [REST API: Konuşmayı metne dönüştürme](rest-speech-to-text.md)
* [REST API: Metin okuma](rest-text-to-speech.md)
* [REST API: Toplu iş dökümü ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ücretsiz bir konuşma Hizmetleri abonelik anahtarı alın](get-started.md)
