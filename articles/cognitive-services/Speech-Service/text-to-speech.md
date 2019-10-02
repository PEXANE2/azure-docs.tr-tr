---
title: Metin okuma-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmetindeki metinden konuşmaya özelliği, uygulamalarınızın, araçlarınızın veya cihazların metni doğal insan benzeri sentezleştirilmiş konuşmaya dönüştürmesini sağlayan bir özelliktir. Standart ve sinir sesler arasından seçim yapın ya da ürün veya marka için özel sesinizi benzersiz bir şekilde oluşturun. 75 + standart sesler 45 ' den fazla dilde ve yerel ayarlarda kullanılabilir ve 4 dilde ve yerel ayarlarda 5 sinir seste erişilebilir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: erhopf
ms.openlocfilehash: d12b952d298b41c8d06f0fcac141a45749de9051
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71799950"
---
# <a name="what-is-text-to-speech"></a>Metin okuma nedir?

Azure konuşma hizmetlerinden gelen metin okuma, uygulamalarınızın, araçların veya cihazların metni doğal insan benzeri sentezleştirilmiş konuşmaya dönüştürmesini sağlayan bir hizmettir. Standart ve sinir sesler arasından seçim yapın ya da ürün veya marka için özel sesinizi benzersiz bir şekilde oluşturun. 75 + standart sesler 45 ' den fazla dilde ve yerel ayarlarda kullanılabilir ve 4 dilde ve yerel ayarlarda 5 sinir seste erişilebilir. Tam liste için bkz. [desteklenen diller](language-support.md#text-to-speech).

Metin okuma teknolojisi, içerik oluşturucularının kullanıcılarıyla farklı yollarla etkileşime geçmesini sağlar. Metin okuma, kullanıcılara içerik sesiyle etkileşime geçme seçeneği sunarak erişilebilirliği iyileştirebilir. Kullanıcının görsel bir biçimde, öğrenimi bir eğitim veya geliştirme sırasında gezinme bilgileri gerektirmesi durumunda, metinden konuşmaya, mevcut bir deneyimi iyileştirebilir. Ayrıca metin okuma, sesli botlar ve sanal Yardımcılar için de değerli bir eklentidir.


XML tabanlı bir biçimlendirme dili olan konuşma sentezleme dili (SSML) özelliğinden yararlanarak, metin okuma hizmetini kullanan geliştiriciler giriş metninin sentezleştirilmiş konuşmaya nasıl dönüştürüleceğini belirtebilir. SSML ile, sıklık, telaffuz, konuşma oranı, hacim ve daha fazlasını ayarlayabilirsiniz. Daha fazla bilgi için bkz. [SSML](#speech-synthesis-markup-language-ssml).

### <a name="standard-voices"></a>Standart sesler

Standart sesler Istatistiksel parametrik sen, ve/veya birleştirme birleştirme tekniklerini kullanarak oluşturulur. Bu sesler, yüksek oranda anlaşılır ve sessiz doğal bir şekilde yapılır. Uygulamalarınızın çok sayıda dilde 45 konuşmasını, çok çeşitli ses seçenekleriyle kolayca sağlayabilirsiniz. Bu sesler, kısaltmalar, kısaltma genişletmeleri, tarih/saat yorumlamalar, polyphones ve daha fazlası için destek de dahil olmak üzere yüksek telaffuz doğruluk sağlar. Kullanıcılarınızın içeriğinizle birlikte etkileşime geçmesini sağlayarak uygulama ve hizmetlerinize yönelik erişilebilirliği geliştirmek için standart ses kullanın.

### <a name="neural-voices"></a>Sinir sesleri

Sinir sesleri, konuşulan dilde stres ve kullanım desenlerini eşleştirirken ve konuşma birimlerinin bir bilgisayar sesine çıkarılması halinde geleneksel metin okuma sistemlerinin sınırlarını aşmak için derin sinir ağlarını kullanır. Standart metin okuma, bağımsız modeller tarafından yönetilen ayrı bir dil analizine ve akustik tahmin adımlarına sahiptir ve bu da muffled Voice senile sonuçlanabilir. Sinir yetenekimiz, daha akıcı ve doğal bir ses elde eden daha hızlı bir tahmin ve ses senişçisi.

Sinir sesleri, chatbots ve sanal yardımcılar ile daha doğal ve ilgi çekici bir şekilde etkileşim kurmak, e-kitaplar gibi dijital metinleri audiobooks 'a dönüştürmek ve oto içi gezinti sistemlerini geliştirmek için kullanılabilir. İnsan benzeri doğal Prosody ve sözcüklerin bir kısmını temizleyerek, AI sistemleriyle etkileşime geçerek sinir seslileri büyük ölçüde dinlemeyi azaltır.

Sinir sesleri, nötr ve peynsuz gibi farklı stilleri destekler. Örneğin, Jessa (en-US) sesi, normal ve mutlu konuşmalar için en iyi duruma getirilmiş cheertam olarak konuşabilmektedir. Ses çıkışını, [konuşma birleştirme biçimlendirme dilini](speech-synthesis-markup.md)kullanarak ton, sıklık ve hız gibi ayarlayabilirsiniz. Kullanılabilir seslerin tam listesi için bkz. [desteklenen diller](language-support.md#text-to-speech).

Sinir seslerin avantajları hakkında daha fazla bilgi edinmek için bkz. [Microsoft 'un yeni sinir metin okuma hizmeti, makinelerin kişiler gibi konuşmasına yardımcı olur](https://azure.microsoft.com/blog/microsoft-s-new-neural-text-to-speech-service-helps-machines-speak-like-people/).

### <a name="custom-voices"></a>Özel sesler

Ses özelleştirmesi, markanız için tanınabilir, tek bir tür sesi oluşturmanızı sağlar. Özel ses yazı tipi oluşturmak için bir Studio kaydı yaparsınız ve ilgili betikleri eğitim verileri olarak karşıya yüklersiniz. Daha sonra hizmet, kaydınıza ayarlanmış benzersiz bir ses modeli oluşturur. Konuşmayı sentezleştirmek için bu özel ses yazı tipini kullanabilirsiniz. Daha fazla bilgi için bkz. [özel sesler](how-to-customize-voice-font.md).

## <a name="speech-synthesis-markup-language-ssml"></a>Konuşma Sentezi Biçimlendirme Dili (SSML)

Konuşma sentezi biçimlendirme dili (SSML), geliştiricilerin metin okuma hizmeti kullanılarak, giriş metninin birleştirilmiş konuşmaya nasıl dönüştürüldüğünü belirtmesini sağlayan XML tabanlı bir biçimlendirme dilidir. SSML, düz metin ile karşılaştırıldığında, geliştiricilerin sıklık, telaffuz, konuşma oranı, hacim ve metin okuma çıktısından daha fazlasını yapma olanağı sağlar. Bir dönemden sonra duraklatma gibi normal noktalama işaretleri veya bir tümce bir soru işaretiyle sona erdiğinde doğru innnation 'ın kullanılması otomatik olarak işlenir.

Metin okuma hizmetine gönderilen tüm metin girişlerinin SSML olarak yapılandırılması gerekir. Daha fazla bilgi için bkz. [konuşma sen, biçimlendirme dili](speech-synthesis-markup.md).

### <a name="pricing-note"></a>Fiyatlandırma notunun

Metin okuma hizmeti kullanılırken, noktalama, noktalama dahil olmak üzere her bir karakter için faturalandırılırsınız. SSML belgesinin kendisi faturalanabilir olsa da, metnin, alfabesine ve sıklık gibi bir şekilde konuşmaya dönüştürülmesini ayarlamak için kullanılan isteğe bağlı öğeler faturalanabilir karakter olarak sayılır. Faturalandırılabilir nelerin listesi aşağıda verilmiştir:

* İsteğin SSML gövdesinde metin okuma hizmetine geçilen metin
* @No__t-0 ve `<voice>` etiketleri dışında SSML biçimindeki istek gövdesinin metin alanı içindeki tüm biçimlendirmeler
* Harfler, noktalama, boşluk, sekme, biçimlendirme ve tüm beyaz boşluk karakterleri
* Unicode 'da tanımlanan her kod noktası

Ayrıntılı bilgi için bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

> [!IMPORTANT]
> Her Çince, Japonca ve Kore dili karakteri faturalandırma için iki karakter olarak sayılır.

## <a name="core-features"></a>Temel Özellikler

Bu tabloda, metinden konuşmaya için temel özellikler listelenmektedir:

| Kullanım örneği | SDK | REST |
|----------|-----|------|
| Metni konuşmaya dönüştürün. | Yes | Yes |
| Ses uyarlama için veri kümelerini karşıya yükleyin. | Hayır | Evet @ no__t-0 |
| Ses yazı tipi modelleri oluşturun ve yönetin. | Hayır | Evet @ no__t-0 |
| Ses yazı tipi dağıtımları oluşturun ve yönetin. | Hayır | Evet @ no__t-0 |
| Ses yazı tipi testleri oluşturun ve yönetin. | Hayır | Evet @ no__t-0 |
| Abonelikleri yönetin. | Hayır | Evet @ no__t-0 |

\* *Bu hizmetler Cris.ai uç noktası kullanılarak kullanılabilir. Bkz. [Swagger başvurusu](https://westus.cris.ai/swagger/ui/index). Bu özel sesli eğitim ve yönetim API 'Leri, istekleri 5 saniyede 25 ' e sınırlayan azaltma uygular, ancak konuşma birleştirme API 'sinin kendisi için saniyede 200 istek sağlayan azaltma uygular. Kısıtlama gerçekleştiğinde ileti üstbilgileri aracılığıyla size bildirilir.*

## <a name="get-started-with-text-to-speech"></a>Metin okuma ile çalışmaya başlama

Kodu, 10 dakikadan kısa bir süre içinde çalıştırmak için tasarlanan hızlı başlangıç sunuyoruz. Bu tablo, dile göre düzenlenen metin okuma hızlarından oluşan bir liste içerir.

### <a name="sdk-quickstarts"></a>SDK hızlı başlangıçlarını

| Hızlı Başlangıç (SDK) | Platform | API başvurusu |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-text-to-speech-dotnetcore.md) | Windows | [Ata](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](quickstart-text-to-speech-dotnet-windows.md) | Windows | [Ata](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](quickstart-text-to-speech-csharp-uwp.md) | Windows | [Ata](https://aka.ms/csspeech/csharpref) |
| [C#, Unity](quickstart-text-to-speech-csharp-unity.md) | Windows, Android | [Ata](https://aka.ms/csspeech/csharpref) |
| [C++](quickstart-text-to-speech-cpp-windows.md) | Windows | [Ata](https://aka.ms/csspeech/cppref) |
| [C++](quickstart-text-to-speech-cpp-linux.md) | Linux | [Ata](https://aka.ms/csspeech/cppref) |
| [Java](quickstart-text-to-speech-java-jre.md) | Windows, Linux, macOS | [Ata](https://aka.ms/csspeech/javaref) |
| [Java](quickstart-text-to-speech-java-android.md) | Android | [Ata](https://aka.ms/csspeech/javaref) |
| [Objective-C](quickstart-text-to-speech-objectivec-macos.md) | macOS | [Ata](https://aka.ms/csspeech/objectivecref) |
| [Objective-C](quickstart-text-to-speech-objectivec-ios.md) | iOS | [Ata](https://aka.ms/csspeech/objectivecref) |
| [Swift](quickstart-text-to-speech-swift-macos.md) | macOS | [Ata](https://aka.ms/csspeech/objectivecref) |
| [Swift](quickstart-text-to-speech-swift-ios.md) | iOS | [Ata](https://aka.ms/csspeech/objectivecref) |
| [Python](quickstart-text-to-speech-python.md) | Pencere, Linux, macOS | [Ata](https://aka.ms/csspeech/pythonref) |

### <a name="rest-quickstarts"></a>REST hızlı başlangıç

| Hızlı Başlangıç (REST) | Platform | API başvurusu |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-dotnet-text-to-speech.md) | Windows, macOS, Linux | [Ata](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Node.js](quickstart-nodejs-text-to-speech.md) | Pencere, macOS, Linux | [Ata](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Python](quickstart-python-text-to-speech.md) | Pencere, macOS, Linux | [Ata](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |

## <a name="sample-code"></a>Örnek kod

Metinden konuşmaya yönelik örnek kod GitHub ' da kullanılabilir. Bu örnekler, en popüler programlama dillerinde metinden konuşmaya dönüştürmeyi kapsar.

* [Metinden konuşmaya örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Metinden konuşmaya örnekleri (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Konuşma cihazları SDK 'Sı](speech-devices-sdk.md)
* [REST API: konuşmayı metne dönüştürme](rest-speech-to-text.md)
* [REST API: metinden konuşmaya](rest-text-to-speech.md)
* [REST API: toplu Iş dökümü ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

* [Ücretsiz bir konuşma Hizmetleri aboneliği alın](get-started.md)
* [Özel ses yazı tipleri oluşturma](how-to-customize-voice-font.md)
