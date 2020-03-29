---
title: Ses İçerik Oluşturma - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Ses İçerik Oluşturma, uygulamalarınız ve ürünleriniz için Microsoft'un metin-konuşma çıktısını özelleştirmenize ve ince ayar yapmanızı sağlayan çevrimiçi bir araçtır.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: dapine
ms.openlocfilehash: ab0d2b8d95b4cb5996dd93fa0bb24085c9de26d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78331545"
---
# <a name="improve-synthesis-with-audio-content-creation"></a>Ses İçeriği oluşturma ile sentezi geliştirin

[Ses İçerik Oluşturma,](https://aka.ms/audiocontentcreation) uygulamalarınız ve ürünleriniz için Microsoft'un metin-konuşma çıktısını özelleştirmenize ve ince ayar yapmanızı sağlayan çevrimiçi bir araçtır. Bu aracı, daha doğru doğal ifadeler için genel ve özel seslere ince ayar yapmak ve çıktınızı bulutta yönetmek için kullanabilirsiniz.

Ses İçerik Oluşturma aracı [Konuşma Sentezi Biçimlendirme Dili (SSML)](speech-synthesis-markup.md)dayanmaktadır. Özelleştirme ve ayarı basitleştirmek için, Ses İçerik Oluşturma, metinden konuşmaya çıktılarınızı gerçek zamanlı olarak görsel olarak incelemenize olanak tanır.

## <a name="how-does-it-work"></a>Nasıl çalışır?

Bu diyagram, özelleştirilmiş konuşma-metin çıktılarını ayarlamak ve dışa aktarmak için gereken adımları gösterir. Her adım hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın.

![](media/audio-content-creation/audio-content-creation-diagram.jpg)

1. İlk [adım, bir Azure hesabı oluşturmak, bir Konuşma kaynağı kaydetmek ve bir abonelik anahtarı almaktır.](#create-a-speech-resource) Bir abonelik anahtarınız olduktan sonra, konuşma hizmetini aramak ve [Ses İçerik Oluşturma'ya](https://aka.ms/audiocontentcreation)erişmek için kullanabilirsiniz.
2. Düz metin veya SSML kullanarak [bir ses tuning dosyası oluşturun.](#create-an-audio-tuning-file)
3. Ayarlamak istediğiniz sesi ve dili seçin. Ses İçerik Oluşturma, [Microsoft metinden konuşmaya tüm sesleri](language-support.md#text-to-speech)içerir. Standart, sinirsel veya kendi özel sesinizi kullanabilirsiniz.
   >[!NOTE]
   > Doğal görünen konuşmaya benzer yüksek tanımlı sesler oluşturmanıza olanak tanıyan Özel Nöral Sesler için kapılı erişim mevcuttur. Ek ayrıntılar için [Gating işlemine](https://aka.ms/ignite2019/speech/ethics)bakın.

4. Varsayılan sonucu gözden geçirin. Ardından telaffuzu, adım, hız, tonlama, ses stili ve daha fazlasını ayarlamak için ayarlama aracını kullanın. Seçeneklerin tam listesi için konuşma [sentezi biçimlendirme diline](speech-synthesis-markup.md)bakın.
5. [Ayarlı sesinizi kaydedin ve dışa aktarın.](#export-tuned-audio) Sistemdeki aparat parçasını kaydettiğinizde, çalışmaya devam edebilir ve çıktıüzerinde yineleyebilirsiniz. Çıktıdan memnun olduğunuzda, dışa aktarma özelliğiyle bir ses oluşturma görevi oluşturabilirsiniz. Dışa aktarma görevinin durumunu gözlemleyebilir ve çıktıyı uygulamalarınız ve ürünleriniz ile kullanmak üzere indirebilirsiniz.
6. Son adım, uygulamalarınızda ve ürünlerinizde özel ayarlı sesi kullanmaktır.

## <a name="create-a-speech-resource"></a>Konuşma kaynağı oluşturma

Konuşma kaynağı oluşturmak ve Konuşma Stüdyosu'na bağlamak için aşağıdaki adımları izleyin.

1. [Azure hesabına kaydolmak](get-started.md#new-resource) ve [bir Konuşma kaynağı oluşturmak](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-the-resource)için bu yönergeleri izleyin. Fiyatlandırma katmanınızın **S0**olarak ayarlandıktan emin olun. Nöral seslerden birini kullanıyorsanız, kaynağınızı desteklenen bir [bölgede](regions.md#standard-and-neural-voices)oluşturduğunuzdan emin olun.
2. Ses İçerik Oluşturma oturum [açın.](https://aka.ms/audiocontentcreation)
3. Varolan bir projeyi seçin veya **Yeni Oluştur'u**tıklatın.
4. Üst navigasyonda bulunan **Ayarlar** seçeneği ile aboneliğinizi istediğiniz zaman değiştirebilirsiniz.

## <a name="create-an-audio-tuning-file"></a>Ses tuning dosyası oluşturma

İçeriğinizi Ses İçerik Oluşturma aracına sokmanın iki yolu vardır.

**Seçenek 1:**

1. Ses İçerik [Oluşturma'da](https://aka.ms/audiocontentcreation)oturum imzaladıktan sonra, yeni bir ses tuning dosyası oluşturmak için **Ses Tuning'i** tıklatın.
2. Düzenleme penceresi göründüğünde, en fazla 10.000 karakter giriş yapabilirsiniz.
3. Kurtarmayı unutma.

**Seçenek 2:**

1. Ses İçerik [Oluşturma'da](https://aka.ms/audiocontentcreation)oturum imzaladıktan sonra, bir veya daha fazla metin dosyası almak için **Yükle'yi** tıklatın. Hem düz metin hem de SSML desteklenir.
2. Metin dosyalarınızı yüklerken, içeriğin bu gereksinimleri karşıladığından emin olun.

   | Özellik | Değer / Notlar |
   |----------|---------------|
   | Dosya biçimi | Düz metin (.txt)<br/> SSML metni (.txt)<br/> Zip dosyaları desteklenmiyor |
   | Kodlama biçimi | UTF-8 |
   | Dosya adı | Her dosyanın benzersiz bir adı olmalıdır. Yinelenenler desteklenmez. |
   | Metin uzunluğu | Metin dosyaları 10.000 karakteri geçmemelidir. |
   | SSML kısıtlamaları | Her SSML dosyası yalnızca tek bir SSML parçası içerebilir. |

### <a name="plain-text-example"></a>Düz metin örneği

```txt
Welcome to use Audio Content Creation to customize audio output for your products.
```

### <a name="ssml-text-example"></a>SSML metin örneği

```xml
<speak xmlns="http://www.w3.org/2001/10/synthesis" xmlns:mstts="http://www.w3.org/2001/mstts" version="1.0" xml:lang="en-US">
    <voice name="Microsoft Server Speech Text to Speech Voice (en-US, JessaNeural)">
    Welcome to use Audio Content Creation <break time="10ms" />to customize audio output for your products.
    </voice>
</speak>
```

## <a name="export-tuned-audio"></a>Ayarlı sesi dışa aktarma

Ses çıktınızı inceledikten ve ayarlama ve ayarlamanızdan memnun kaldıktan sonra sesi dışa aktarabilirsiniz.

1. Ses [İçeriği Oluşturma](https://aka.ms/audiocontentcreation) aracından, ses oluşturma görevi oluşturmak için **Dışa** Aktar'ı tıklatın.
2. Ayarlanmış sesin çıktı biçimini seçin. Desteklenen biçimlerin ve örnek oranların bir listesini aşağıda bulabilirsiniz.
3. Dışa **Aktarma görev** sekmesinde görevin durumunu görüntüleyebilirsiniz. Görev başarısız olursa, tam bir rapor için ayrıntılı bilgi sayfasına bakın.
4. Görev tamamlandığında, sesin **Ses Kitaplığı** sekmesinden indirilebilir.
5. **İndir'i**tıklatın. Artık uygulamalarınızda veya ürünlerinizde özel ayarlı sesinizi kullanmaya hazırsınız.

### <a name="supported-audio-formats"></a>Desteklenen ses biçimleri

| Biçimlendir | 16 kHz numune oranı | 24 kHz numune oranı |
|--------|--------------------|--------------------|
| Wav | riff-16khz-16bit-mono-pcm | riff-24khz-16bit-mono-pcm |
| Mp3 | ses-16khz-128kbitrate-mono-mp3 | ses-24khz-160kbitrate-mono-mp3 |

## <a name="see-also"></a>Ayrıca bkz.

* [Uzun Ses API'si](https://aka.ms/long-audio-api)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma Stüdyosu](https://speech.microsoft.com)
