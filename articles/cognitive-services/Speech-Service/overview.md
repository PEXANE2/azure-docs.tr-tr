---
title: Konuşma tanıma hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti, konuşmadan metne, metinden konuşmaya ve konuşma çevirisi için tek bir Azure aboneliğine sahip değildir. Konuşma SDK 'Sı, konuşma cihazları SDK 'Sı veya REST API 'Leri ile uygulamalarınıza, araçlara ve cihazlara konuşma ekleyin.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 08/20/2020
ms.author: trbye
ms.openlocfilehash: 57577c840c93888b2b96971994724f085ad44ebe
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89569314"
---
# <a name="what-is-the-speech-service"></a>Konuşma tanıma hizmeti nedir?

Konuşma hizmeti, konuşmadan metne, metinden konuşmaya ve konuşma çevirisi için tek bir Azure aboneliğine yönelik bir birleşme hizmetidir. Konuşma [CLI](spx-overview.md), [konuşma SDK 'Sı](speech-sdk-reference.md), [konuşma cihazları SDK 'Sı](https://aka.ms/sdsdk-quickstart), [konuşma Studio](https://speech.microsoft.com/)veya [REST API 'leri](rest-apis.md)ile uygulamalarınızı, araçları ve cihazlarınızı kolayca yapabilirsiniz.

> [!IMPORTANT]
> Konuşma hizmeti Bing Konuşma API'si ve Translator Konuşma Çevirisi değiştirdi. Geçiş yönergeleri için bkz. _nasıl yapılır kılavuzlarından geçiş >_ .

Aşağıdaki özellikler konuşma hizmetinin bir parçasıdır. Her bir özellik için genel kullanım örnekleri hakkında daha fazla bilgi edinmek için bu tablodaki bağlantıları kullanın veya API başvurusuna gidin.

| Hizmet | Özellik | Açıklama | SDK | REST |
|---------|---------|-------------|-----|------|
| [Konuşmayı metne dönüştürme](speech-to-text.md) | Gerçek zamanlı konuşmayı metne dönüştürme | Konuşmadan metne dönüştürme veya ses akışlarını veya yerel dosyaları, uygulamalarınızın, araçlarınızın veya cihazlarınızın tüketebileceği veya görüntüleyeceği gerçek zamanlı metne dönüştürür. Kullanıcı amaçlarını [Language Understanding (lusıs)](https://docs.microsoft.com/azure/cognitive-services/luis/) ile birlikte kullanarak konuşma ve ses komutları üzerinde Kullanıcı hedefleri türetebilirsiniz. | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Toplu iş konuşmayı metne dönüştürme](batch-transcription.md) | Toplu iş konuşmayı metne dönüştürme, Azure Blob depolamada depolanan büyük hacimde konuşma ses verilerinin zaman uyumsuz konuşmadan metne dökümünü sağlar. Konuşma sesini metne dönüştürmenin yanı sıra, toplu konuşma tanıma özelliği de, yaklaşım ve yaklaşım analizi için de izin verir. | Hayır | [Evet](https://westus.cris.ai/swagger/ui/index) |
| | [Çok cihazlı konuşma](multi-device-conversation.md) | Konuşma ve çeviri için kolay destek sayesinde konuşma veya metin tabanlı iletiler göndermek için bir konuşmadaki birden çok cihaza veya istemciye bağlanın| Yes | Hayır |
| | [Konuşma dökümü](conversation-transcription-service.md) | Gerçek zamanlı konuşma tanımayı, konuşmacı tanımlamayı ve okumayı mümkün hale getirme. Hoparlörleri, konuşmacıları ayırt etme imkanına sahip olmak için bir kişi içi toplantılar için mükemmeldir. | Yes | Hayır |
| | [Özel Konuşma Tanıma modelleri oluşturma](#customize-your-speech-experience) | Benzersiz bir ortamda tanıma ve döküm için konuşmaya metin kullanıyorsanız, çevresel gürültü veya sektöre özgü sözlük için özel akustik, dil ve telaffuz modeller oluşturup eğitebilirsiniz. | Hayır | [Evet](https://westus.cris.ai/swagger/ui/index) |
| [Metin okuma](text-to-speech.md) | Metin okuma | Metin okuma [, konuşma birleştirme biçimlendirme dili (SSML)](speech-synthesis-markup.md)kullanarak giriş metnini insan benzeri sentezleştirilmiş konuşmaya dönüştürür. Standart sesler ve sinir sesler arasından seçim yapın (bkz. [dil desteği](language-support.md)). | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Özel sesler oluşturma](#customize-your-speech-experience) | Markanızı veya ürününüzün benzersiz özel ses yazı tiplerini oluşturun. | Hayır | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Konuşma Çevirisi](speech-translation.md) | Konuşma çevirisi | Konuşma çevirisi, uygulamalarınıza, araçlara ve cihazlara gerçek zamanlı, çok dilli konuşma çevirisi sağlar. Konuşmayı konuşmaya dönüştürme ve konuşmadan metne çevirme için bu hizmeti kullanın. | [Evet](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | Hayır |
| [Ses yardımcıları](voice-assistants.md) | Ses yardımcıları | Konuşma hizmetini kullanan sesli yardımcılar, geliştiricilerin uygulamalar ve deneyimleri için doğal ve insan benzeri konuşma arabirimleri oluşturmalarına olanak sağlar. Ses Yardımcısı hizmeti, bir cihaz ile bir yardımcı uygulama arasında hızlı ve güvenilir bir etkileşim sağlar ve bu sayede, görev tamamlama için bir aygıt ve doğrudan hat konuşma kanalı veya tümleşik özel komutlar (Önizleme) hizmeti kullanılır. | [Evet](voice-assistants.md) | Hayır |
| [Konuşmacı Tanıma](speaker-recognition-overview.md) | Konuşmacı doğrulama & kimliği | Konuşmacı Tanıma hizmeti, kendi benzersiz ses özelliklerine göre hoparlörleri doğrulayan ve tanımlayan algoritmalar sağlar. Konuşmacı Tanıma, "konuşuyor kim?" sorusunu yanıtlamak için kullanılır. | Yes | [Evet](https://docs.microsoft.com/rest/api/speakerrecognition/) |


[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

## <a name="try-the-speech-service-for-free"></a>Konuşma tanıma hizmetini ücretsiz olarak deneyin

Aşağıdaki adımlar için hem Microsoft hesabı hem de bir Azure hesabınızın olması gerekir. Bir Microsoft hesabı yoksa, [Microsoft hesabı portalında](https://account.microsoft.com/account)ücretsiz bir ücret karşılığında kaydolabilirsiniz. **Microsoft hesabıyla oturum açın** ' yi seçin ve ardından oturum açmak Istendiğinde **Microsoft hesabı oluştur**' u seçin. Yeni Microsoft hesabı oluşturup doğrulamak için adımları izleyin.

Microsoft hesabı aldıktan sonra [Azure kaydolma sayfasına](https://azure.microsoft.com/free/ai/)gidin, **ücretsiz Başlat**' ı seçin ve Microsoft hesabı kullanarak yeni bir Azure hesabı oluşturun.

> [!NOTE]
> Konuşma hizmetinde iki hizmet katmanı vardır: ücretsiz ve abonelik, farklı sınırlamalara ve avantajlara sahiptir. Ücretsiz bir Azure hesabı için kaydolduğunuzda, hizmet kredisi $200 ile birlikte gelir ve ücretli bir konuşma hizmeti aboneliğine uygulayabileceğiniz, 30 güne kadar geçerli olur.
>
> Ücretsiz, düşük hacimli konuşma hizmeti katmanını kullanıyorsanız, ücretsiz deneme veya hizmet kredinizin süresi dolduktan sonra bile bu ücretsiz aboneliği koruyabilirsiniz.
>
> Daha fazla bilgi için bkz. bilişsel [Hizmetler fiyatlandırması-konuşma hizmeti](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="create-the-azure-resource"></a>Azure kaynağı oluşturma

Azure hesabınıza bir konuşma hizmeti kaynağı (ücretsiz veya ücretli katman) eklemek için:

1. Microsoft hesabınızı kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.

1. Portalın sol üst kısmında **kaynak oluştur** ' u seçin. **Kaynak oluştur**' u görmüyorsanız, ekranın sol üst köşesindeki daraltılan menüyü seçerek her zaman bulabilirsiniz.

1. **Yeni** pencerede, arama kutusuna "konuşma" YAZıN ve ENTER tuşuna basın.

1. Arama sonuçlarında **konuşma**' yı seçin.

   ![Konuşma Arama sonuçları](media/index/speech-search.png)

1. **Oluştur**' u seçin, sonra:

   - Yeni kaynağınız için benzersiz bir ad verin. Ad, aynı hizmete bağlı birden çok abonelik arasında ayrım yapmanıza yardımcı olur.
   - Ücretlerin nasıl faturalandırıldığını belirlemek için yeni kaynağın ilişkilendirildiği Azure aboneliğini seçin.
   - Kaynağın kullanılacağı [bölgeyi](regions.md) seçin.
   - Ücretsiz (F0) ya da ücretli (S0) fiyatlandırma katmanını seçin. Her katmana yönelik fiyatlandırma ve kullanım kotaları hakkında tam bilgi için, **tam fiyatlandırma ayrıntılarını görüntüle**' yi seçin.
   - Bu konuşma aboneliği için yeni bir kaynak grubu oluşturun veya aboneliği var olan bir kaynak grubuna atayın. Kaynak grupları, çeşitli Azure aboneliklerinizi düzenli halde tutmanıza yardımcı olur.
   - **Oluştur**’u seçin. Bu işlem sizi dağıtıma genel bakış ve dağıtım ilerleme durumu iletilerini görüntüleyecek şekilde götürür.

> [!NOTE]
> Bir veya birden çok bölgede sınırsız sayıda standart katmanlı abonelik oluşturabilirsiniz. Ancak, yalnızca bir serbest katmanlı abonelik oluşturabilirsiniz. Ücretsiz katmanda 7 gün boyunca kullanılmamış olarak kalan model dağıtımları otomatik olarak kullanımdan kaldırılacaktır.

Yeni konuşma kaynağınızın dağıtılması birkaç dakika sürer. Dağıtım tamamlandıktan sonra **Kaynağa Git** ' i seçin ve sol gezinti bölmesinde konuşma hizmeti abonelik anahtarlarınızı göstermek için **anahtarlar** ' ı seçin. Her abonelik iki anahtara sahiptir; uygulamanızda herhangi bir anahtarı kullanabilirsiniz. Bir anahtarı kod düzenleyicinize veya başka bir konuma hızlıca kopyalamak/yapıştırmak için, her bir anahtarın yanındaki Kopyala düğmesini seçin, pencereler arasında Pano içeriğini istenen konuma yapıştırın.

> [!IMPORTANT]
> Bu abonelik anahtarları bilişsel hizmet API 'nize erişmek için kullanılır. Anahtarlarınızı paylaşmayın. Bunları güvenli bir şekilde depolayın (örneğin, Azure Key Vault kullanarak). Ayrıca, bu anahtarların düzenli olarak yeniden oluşturulması önerilir. API çağrısı yapmak için yalnızca bir anahtar gereklidir. İlk anahtarı yeniden oluştururken, hizmete devam eden erişim için ikinci anahtarı kullanabilirsiniz.

## <a name="complete-a-quickstart"></a>Hızlı başlangıç tamamlamayı

Her biri temel tasarım desenleri öğretmek ve kodu 10 dakikadan kısa bir süre içinde çalıştırmak için tasarlanan en popüler programlama dillerinde hızlı başlangıçlar sunuyoruz. Her bir özelliğin hızlı başlangıcı için aşağıdaki listeye bakın.

* [Konuşmayı metne hızlı başlangıç](get-started-speech-to-text.md)
* [Metin okuma için hızlı başlangıç](get-started-text-to-speech.md)
* [Konuşma çevirisine hızlı başlangıç](speech-translation-basics.md)
* [Amaç tanıma hızlı başlangıcı](quickstarts/intent-recognition.md)
* [Konuşmacı Tanıma hızlı başlangıç](speaker-recognition-basics.md)

Konuşma hizmetini kullanmaya başlama şansınız olduktan sonra, çeşitli senaryoları nasıl çözeceğinizi gösteren öğreticilerimizi deneyin.

- [Öğretici: konuşma SDK 'Sı ve LUU, C ile konuşma amaçlarını tanıma #](how-to-recognize-intents-from-speech-csharp.md)
- [Öğretici: konuşma SDK 'Sı ile sesinizi etkinleştirme, C #](tutorial-voice-enable-your-bot-speech-sdk.md)
- [Öğretici: metin çevirmek, yaklaşımı çözümlemek ve çevrilmiş metni konuşmaya dönüştürmek için bir Flask uygulaması oluşturun, REST](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2fazure%2fcognitive-services%2fspeech-service%2ftoc.json&bc=%2fazure%2fcognitive-services%2fspeech-service%2fbreadcrumb%2ftoc.json&toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fspeech-service%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)

## <a name="get-sample-code"></a>Örnek kodu alma

Örnek kod, konuşma hizmeti için GitHub 'da kullanılabilir. Bu örnekler, bir dosya veya akıştan ses okuma, sürekli ve tek kararlı bir tanıma ve özel modellerle çalışma gibi yaygın senaryoları kapsar. SDK ve REST örneklerini görüntülemek için bu bağlantıları kullanın:

- [Konuşmadan metne, metinden konuşmaya ve konuşma çevirisi örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Toplu iş dökümü örnekleri (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
- [Metinden konuşmaya örnekleri (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)
- [Ses Yardımcısı örnekleri (SDK)](https://aka.ms/csspeech/samples)

## <a name="customize-your-speech-experience"></a>Konuşma deneyiminizi özelleştirin

Konuşma hizmeti yerleşik modellerle iyi çalışmaktadır, ancak ürününüzün veya ortamınızın deneyimini daha fazla özelleştirmek ve ayarlamak isteyebilirsiniz. Özelleştirme seçenekleri, akustik model ayarlamalarından markanızla ilgili benzersiz ses yazı tiplerine kadar değişir.

Diğer ürünler, sağlık veya sigorta gibi belirli amaçlar için ayarlanan konuşma modellerini sunar, ancak herkese eşit olarak erişilebilir. Azure konuşma 'da özelleştirme, başka bir kullanıcı veya müşteri tarafından kullanılamayan *benzersiz rekabet avantajlarınızın* bir parçası haline gelir. Diğer bir deyişle, modelleriniz özel ve yalnızca kullanım için özel olarak ayarlanmıştır.

| Konuşma Hizmeti | Platform | Description |
| -------------- | -------- | ----------- |
| Konuşmayı Metne Dönüştürme | [Özel Konuşma Tanıma](https://aka.ms/customspeech) | Konuşma tanıma modellerini gereksinimlerinize ve kullanılabilir verilerinize göre özelleştirin. Konuşma stili, sözlük ve arka plan gürültüsü gibi konuşma tanıma engellerinin üstesinden gelme. |
| Metin Okuma | [Özel ses](https://aka.ms/customvoice) | Kullanılabilir konuşma verilerinizle Metin Okuma uygulamalarınız için tanınabilir, kendine özgü bir ses oluşturun. Ses çıkışlarını bir dizi parametre kümesini ayarlayarak daha fazla hassas ayarlamalar yapabilirsiniz. |

## <a name="reference-docs"></a>Başvuru belgeleri

- [Konuşma SDK'sı](speech-sdk-reference.md)
- [Konuşma Cihazları SDK’sı](speech-devices-sdk.md)
- [REST API: konuşmayı metne dönüştürme](rest-speech-to-text.md)
- [REST API: metinden konuşmaya](rest-text-to-speech.md)
- [REST API: toplu Iş dökümü ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşmaya metne Başlarken](speech-to-text-basics.md) 
>  [Metinden konuşmaya çalışmaya başlama](get-started-text-to-speech.md)
