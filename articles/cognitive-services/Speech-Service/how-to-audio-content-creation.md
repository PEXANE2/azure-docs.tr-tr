---
title: Ses Içeriği oluşturma-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Ses Içeriği oluşturma, Microsoft 'un Uygulamalarınız ve ürünleriniz için kendi metin okuma çıktısını özelleştirmenizi ve ayarlamanıza olanak tanıyan bir çevrimiçi araçtır.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: trbye
ms.openlocfilehash: 72fecbdc173a6174e54a28e48f983965f397ba6a
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86224596"
---
# <a name="improve-synthesis-with-the-audio-content-creation-tool"></a>Ses Içeriği oluşturma aracı ile sensıs 'yi geliştirme

[Ses Içeriği oluşturma](https://aka.ms/audiocontentcreation) , Microsoft 'un Uygulamalarınız ve ürünleriniz için kendi metin okuma çıktısını özelleştirmenizi ve ayarlamanıza olanak tanıyan bir çevrimiçi araçtır. Bu aracı, daha doğru doğal ifadeler için genel ve özel seslere ince ayar yapmak ve çıktunuzu bulutta yönetmek için kullanabilirsiniz.

Ses Içeriği oluşturma aracı, [konuşma birleştirme biçimlendirme dilini (SSML)](speech-synthesis-markup.md)temel alır. Görsel Içerik oluşturma, özelleştirmeyi ve ayarlamayı basitleştirmek için, metin okuma çıktılarınızı gerçek zamanlı olarak görsel olarak incelemenize olanak sağlar.

Ses Içeriği oluşturma için [video öğreticisine](https://www.youtube.com/watch?v=O1wIJ7mts_w) bakın.

## <a name="how-does-it-work"></a>Nasıl çalışır?

Bu diyagramda, metinden konuşmaya okuma çıktıları üzerinde ince ayar yapmak için gereken adımlar gösterilir. Her adım hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın.

![Metin okuma çıkışları üzerinde ince ayar yapmak için gereken adımların bir diyagramı.](media/audio-content-creation/audio-content-creation-diagram.jpg)

1. Başlamak için [Azure hesabınızı ve konuşma kaynağını ayarlayın](#set-up-your-azure-account-and-speech-resource) .
2. Düz metin veya SSML betikleri kullanarak [bir ses ayarlama dosyası oluşturun](#create-an-audio-tuning-file) .
3. Komut dosyası içeriğiniz için ses ve dili seçin. Ses Içeriği oluşturma, tüm [Microsoft metin okuma seslerini](language-support.md#text-to-speech)içerir. Standart, sinir veya kendi özel sesinizi kullanabilirsiniz.
   >[!NOTE]
   > Geçitli erişim, doğal bir konuşma konuşmayla benzer yüksek tanımlı sesler oluşturmanıza olanak sağlayan özel sinir sesleri için kullanılabilir. Daha fazla bilgi için bkz. [işlem işleme](https://aka.ms/ignite2019/speech/ethics).

4. Varsayılan sensıs çıkışını gözden geçirin. Ardından, telaffuz, kesme, sıklık, oran, intonation, ses stili ve daha fazlasını ayarlayarak çıktıyı geliştirebilirsiniz. Seçeneklerin tamamı listesi için bkz. [konuşma sen, biçimlendirme dili](speech-synthesis-markup.md). İşte ses Içeriği oluşturma ile konuşma çıkışının nasıl ince ayarlanacağını gösteren bir [video](https://youtu.be/mUvf2NbfuYU) . 
5. [Ayarlanmış sesinizi kaydedin ve dışarı aktarın](#export-tuned-audio). Ayarlama parçasını sisteme kaydettiğinizde, çalışmaya ve çıkışta yineleme yapmaya devam edebilirsiniz. Çıktıyı karşıladığınızda, dışa aktarma özelliği ile bir ses oluşturma görevi oluşturabilirsiniz. Dışarı aktarma görevinin durumunu gözlemleyebilirsiniz ve uygulama ve ürünlerinizle kullanım için çıktıyı indirebilirsiniz.

## <a name="set-up-your-azure-account-and-speech-resource"></a>Azure hesabınızı ve konuşma kaynağını ayarlama

1. Ses Içeriği oluşturma ile çalışmak için bir Azure hesabınızın olması gerekir. Microsoft hesabınızı kullanarak bir Azure hesabı oluşturabilirsiniz. [Bir Azure hesabı ayarlamak](get-started.md#new-resource)için bu yönergeleri izleyin. 
2. Azure hesabınızda [bir konuşma kaynağı oluşturun](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-the-resource) . Fiyatlandırma katmanınızın **S0**olarak ayarlandığından emin olun. Sinir sesinden birini kullanıyorsanız, kaynağı [desteklenen bir bölgede](regions.md#standard-and-neural-voices)oluşturduğunuzdan emin olun.
2. Azure hesabı ve konuşma kaynağını aldıktan sonra, konuşma hizmetlerini kullanabilir ve [Ses Içeriği oluşturma](https://aka.ms/audiocontentcreation)erişimi sağlayabilirsiniz.
3. Üzerinde çalışmanız gereken konuşma kaynağını seçin. Burada yeni bir konuşma kaynağı da oluşturabilirsiniz. 
4. Konuşma kaynağınızı, üst gezinti bölmesinde bulunan **Ayarlar** seçeneğiyle istediğiniz zaman değiştirebilirsiniz.

## <a name="create-an-audio-tuning-file"></a>Ses ayarlama dosyası oluşturma

İçeriğinizi ses Içeriği oluşturma aracına almanın iki yolu vardır.

**Seçenek 1:**

1. Yeni bir ses ayarlama dosyası oluşturmak için **yeni dosya** ' ya tıklayın.
2. İçeriğinizi, Düzen penceresine yazın veya yapıştırın. Her bir dosyanın karakterleri 20.000 ' e kadar olur. Betiğinizin 20.000 karakterden uzun olması durumunda, içeriğinizi otomatik olarak birden çok dosyaya bölmek için 2 seçeneğini kullanabilirsiniz. 
3. Kaydetmeyi unutmayın.

**Seçenek 2:**

1. Bir veya daha fazla metin dosyasını içeri aktarmak için **Yükle** ' ye tıklayın. Hem düz metin hem de SSML desteklenir.
2. Betik dosyanız 20.000 karakterden büyükse, lütfen dosyayı paragraflara, karaktere veya normal ifadelerle ayırın. 
3. Metin dosyalarınızı karşıya yüklediğinizde, dosyanın bu gereksinimleri karşıladığından emin olun.

   | Özellik | Değer/notlar |
   |----------|---------------|
   | Dosya biçimi | Düz metin (. txt)<br/> SSML metni (. txt)<br/> ZIP dosyaları desteklenmiyor |
   | Kodlama biçimi | UTF-8 |
   | Dosya adı | Her dosyanın benzersiz bir adı olmalıdır. Yinelemeler desteklenmez. |
   | Metin uzunluğu | Metin dosyalarının 20.000 karakteri aşmaması gerekir. |
   | SSML kısıtlamaları | Her SSML dosyası yalnızca tek bir SSML parçası içerebilir. |

### <a name="plain-text-example"></a>Düz metin örneği

```txt
Welcome to use Audio Content Creation to customize audio output for your products.
```

### <a name="ssml-text-example"></a>SSML metin örneği

```xml
<speak xmlns="http://www.w3.org/2001/10/synthesis" xmlns:mstts="http://www.w3.org/2001/mstts" version="1.0" xml:lang="en-US">
    <voice name="Microsoft Server Speech Text to Speech Voice (en-US, AriaNeural)">
    Welcome to use Audio Content Creation <break time="10ms" />to customize audio output for your products.
    </voice>
</speak>
```

## <a name="export-tuned-audio"></a>Ayarlanmış sesi dışa aktarma

Ses çıktlarınızı inceledikten ve ayarlama ve ayarlamasıyla memnun olduktan sonra, sesi dışa aktarabilirsiniz.

1. Ses oluşturma görevi oluşturmak için **dışarı aktar** ' a tıklayın. Uzun ses çıkışını ve tam ses çıkış deneyimini desteklediğinden, **Ses kitaplığına dışa aktarma** önerilir. Ayrıca, sesi yerel diskinize doğrudan indirebilirsiniz, ancak yalnızca ilk 10 dakika kullanılabilir. 
2. Ayarlanmış sesinizi için çıkış biçimini seçin. Desteklenen biçimlerin ve örnek hızların listesi aşağıda verilmiştir.
3. Görevi **dışarı aktar** sekmesinde görevin durumunu görüntüleyebilirsiniz. Görev başarısız olursa, tam bir rapor için ayrıntılı bilgi sayfasına bakın.
4. Görev tamamlandığında ses **kitaplığı** sekmesinde Sesinizdeki indirilebilir.
5. **İndir**'e tıklayın. Artık uygulama veya ürünleriniz için özel olarak ayarlanmış sesinizi kullanmaya hazırsınız.

### <a name="supported-audio-formats"></a>Desteklenen ses biçimleri

| Biçimlendir | 16 kHz örnek hızı | 24 kHz örnek hızı |
|--------|--------------------|--------------------|
| WAV | Riff-16khz-16bit-mono-PCM | Riff-24khz-16bit-mono-PCM |
| ça | Ses-16khz-128kbit hızı-mono-MP3 | Ses-24khz-160kbit hızı-mono-MP3 |

## <a name="see-also"></a>Ayrıca bkz.

* [Uzun ses API 'SI](https://aka.ms/long-audio-api)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Speech Studio](https://speech.microsoft.com)
