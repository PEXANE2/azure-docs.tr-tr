---
title: SPX temelleri-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Bir kod ve en düşük kurulum olmadan konuşma SDK 'Sı ile çalışmak için SPX komut satırı aracını nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/04/2020
ms.author: trbye
ms.openlocfilehash: 68947895891b4875ef4c57355f1236afdb8c2c7d
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83204824"
---
# <a name="learn-the-basics-of-spx"></a>SPX hakkında temel bilgileri öğrenin

Bu makalede, konuşma hizmetini kod yazmadan kullanmak için bir komut satırı aracı olan SPX 'in temel kullanım düzenlerini öğrenirsiniz. Kullanım durumlarınızın yeterince karşılanıp karşılanmadığını görmek için, geliştirme ortamları oluşturmadan veya herhangi bir kod yazmaya gerek kalmadan, konuşma hizmetinin ana özelliklerini hızlıca test edebilirsiniz. Ayrıca, SPX üretime hazırlanın ve konuşma hizmetindeki basit iş akışlarını otomatikleştirebilmek için `.bat` veya kabuk betikleri kullanılarak kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Tek önkoşul, bir Azure konuşma aboneliğudur. Henüz bir abonelik yoksa, yeni abonelik oluşturma [kılavuzuna](get-started.md#new-resource) bakın.

## <a name="download-and-install"></a>İndirme ve yükleme

SPX, Windows ve Linux üzerinde kullanılabilir. [Zip arşivini](https://aka.ms/speech/spx-zips.zip)indirerek başlatın, sonra ayıklayın. SPX, .NET Core veya .NET Framework çalışma zamanı gerektirir ve aşağıdaki sürümler platform tarafından desteklenir:

* Windows: [.NET Framework 4,7](https://dotnet.microsoft.com/download/dotnet-framework/net471), [.NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0)
* Linux: [.NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0)

Bir çalışma zamanı yükledikten sonra, indirdikten sonra ayıkladığınız kök dizine gidin `spx-zips` ve ihtiyacınız olan alt dizini ayıklayın ( `spx-net471` Örneğin,). Bir komut isteminde dizini bu konum olarak değiştirip `spx` uygulamayı başlatmak için komutunu çalıştırın.

## <a name="create-subscription-config"></a>Abonelik yapılandırması oluştur

SPX kullanmaya başlamak için önce konuşma aboneliği anahtarınızı ve bölge bilgilerinizi girmeniz gerekir. Bölge tanımlarınızı bulmak için [bölge desteği](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın. Abonelik anahtarınızı ve bölge tanımlayıcıyı (örn. `eastus`, `westus` ), aşağıdaki komutları çalıştırın.

```shell
spx config @key --set YOUR-SUBSCRIPTION-KEY
spx config @region --set YOUR-REGION-ID
```

Abonelik kimlik doğrulaması artık gelecekteki SPX istekleri için depolandı. Bu depolanmış değerlerden birini kaldırmanız gerekirse, veya ' i çalıştırın `spx config @region --clear` `spx config @key --clear` .

## <a name="basic-usage"></a>Temel kullanım

Bu bölümde, genellikle ilk kez test ve deneme için yararlı olan birkaç temel SPX komutu gösterilmektedir. Aşağıdaki komutu çalıştırarak, varsayılan mikrofonunuzu kullanarak bazı konuşma tanıma işlemlerini gerçekleştirerek başlayın.

```shell
spx recognize --microphone
```

Komutu girdikten sonra, SPX geçerli etkin giriş cihazında sesi dinlemeye başlayacaktır ve ' ı bastıktan sonra durur `ENTER` . Kaydedilen konuşma daha sonra tanınır ve konsol çıkışında metne dönüştürülür. Ayrıca, metin okuma sen, SPX kullanmaya da kolay bir işlemdir. 

Aşağıdaki komutun çalıştırılması girilen metni girdi olarak alır ve birleştirilmiş konuşmayı geçerli etkin çıkış cihazına çıktı olarak alır.

```shell
spx synthesize --text "Testing synthesis using SPX" --speakers
```

Konuşma tanıma ve birleştirme özelliğine ek olarak, aynı zamanda SPX ile konuşma çevirisi de yapabilirsiniz. Yukarıdaki konuşma tanıma komutuna benzer şekilde, varsayılan mikrofonunuzdan ses yakalamak ve hedef dilde metne çeviri gerçekleştirmek için aşağıdaki komutu çalıştırın.

```shell
spx translate --microphone --source en-US --target ru-RU --output file C:\some\file\path\russian_translation.txt
```

Bu komutta, hem kaynak **(çevrilecek**dil) hem de hedef (çevrilecek **dil) dillerini**belirtirsiniz. `--microphone`Bağımsız değişkeninin kullanılması, geçerli etkin giriş cihazında ses dinlemek ve ' a bastıktan sonra duracaktır `ENTER` . Çıktı, bir metin dosyasına yazılan hedef dile bir metin dönüştürmesidir.

> [!NOTE]
> Tüm desteklenen dillerin bir listesi için ilgili yerel ayar kodlarıyla birlikte [dil ve yerel ayar makalesine](language-support.md) bakın.

## <a name="batch-operations"></a>Toplu işlemler

Önceki bölümdeki komutlar, konuşma hizmetinin nasıl çalıştığını hızlı bir şekilde görmek için harika bir yöntemdir. Ancak, kullanım durumlarının karşılanıp karşılanamayacağını değerlendirmek için büyük olasılıkla zaten sahip olduğunuz bir giriş aralığına karşı toplu işlemler gerçekleştirmeniz gerekir, bu da hizmetin çeşitli senaryoları nasıl işlediğini görmenizi sağlar. Bu bölümde nasıl yapılacağı gösterilmektedir:

* Toplu konuşma tanımayı bir ses dosyaları dizininde Çalıştır
* Bir dosya üzerinden yineleme yapın `.tsv` ve toplu iş metin okuma senşü çalıştırın

## <a name="batch-speech-recognition"></a>Toplu konuşma tanıma

Bir ses dosyası dizininiz varsa, toplu konuşma tanımayı hızlı bir şekilde çalıştırmak için SPX 'in kullanımı kolaydır. Aşağıdaki komutu çalıştırarak dizininizle birlikte `--files` komutunu çalıştırın. Bu örnekte, `\*.wav` dizinde bulunan tüm dosyaları tanımak için dizine eklenir `.wav` . Ayrıca, `--threads` tanımayı 10 paralel iş parçacığında çalıştırmak için bağımsız değişkenini belirtin.

> [!NOTE]
> `--threads`Bağımsız değişken, komutlar için sonraki bölümde de kullanılabilir `spx synthesize` ve kullanılabilir Iş parçacıkları CPU 'ya ve geçerli yük yüzdesine bağlıdır.

```shell
spx recognize --files C:\your_wav_file_dir\*.wav --output file C:\output_dir\speech_output.tsv --threads 10
```

Tanınan konuşma çıktısı `speech_output.tsv` `--output file` bağımsız değişkenini kullanarak yazılır. Aşağıda çıkış dosyası yapısına bir örnek verilmiştir.

    audio.input.id    recognizer.session.started.sessionid    recognizer.recognized.result.text
    sample_1    07baa2f8d9fd4fbcb9faea451ce05475    A sample wave file.
    sample_2    8f9b378f6d0b42f99522f1173492f013    Sample text synthesized.

## <a name="batch-text-to-speech-synthesis"></a>Batch metin okuma senşü

Batch metin okuma ' yı çalıştırmanın en kolay yolu, yeni bir `.tsv` (sekmeyle ayrılmış değer) dosyası oluşturmak ve `--foreach` SPX 'de komuttan faydalanır. Aşağıdaki dosyayı göz önünde bulundurun `text_synthesis.tsv` :

    audio.output    text
    C:\batch_wav_output\wav_1.wav    Sample text to synthesize.
    C:\batch_wav_output\wav_2.wav    Using SPX to run batch-synthesis.
    C:\batch_wav_output\wav_3.wav    Some more text to test capabilities.

 Ardından, öğesini işaret etmek için bir komut çalıştırırsınız `text_synthesis.tsv` , her bir alanda sen, `text` ve sonuç olarak karşılık gelen `audio.output` yola bir dosya olarak yazar `.wav` . 

```shell
spx synthesize --foreach in @C:\your\path\to\text_synthesis.tsv
```

Bu komut, `spx synthesize --text Sample text to synthesize --audio output C:\batch_wav_output\wav_1.wav` dosyadaki **her kayıt için** çalıştırmanın eşdeğeridir `.tsv` . Dikkat edilmesi gereken birkaç şey:

* Sütun başlıkları, `audio.output` ve `text` sırasıyla komut satırı bağımsız değişkenlerine karşılık gelir `--audio output` `--text` . Gibi çok parçalı komut satırı bağımsız değişkenleri `--audio output` , boşluk olmadan dosyada, önde gelen tireler olmadan ve dizeleri ayıran noktalarla biçimlendirilmelidir. Örneğin, `audio.output` . Diğer mevcut komut satırı bağımsız değişkenleri, bu model kullanılarak ek sütunlar olarak dosyaya eklenebilir.
* Dosya bu şekilde biçimlendirildiğinde, başka bir bağımsız değişken geçirilmesi gerekmez `--foreach` .
* İçindeki her değeri bir sekmeyle ayırtığınızdan emin olun `.tsv` . **tab**

Ancak, `.tsv` Aşağıdaki örnekte olduğu gibi bir dosyanız varsa, komut satırı bağımsız değişkenleriyle **eşleşmeyen** sütun başlıkları vardır:

    wav_path    str_text
    C:\batch_wav_output\wav_1.wav    Sample text to synthesize.
    C:\batch_wav_output\wav_2.wav    Using SPX to run batch-synthesis.
    C:\batch_wav_output\wav_3.wav    Some more text to test capabilities.

Bu alan adlarını, çağrıda aşağıdaki sözdizimini kullanarak doğru bağımsız değişkenlere geçersiz kılabilirsiniz `--foreach` . Bu, yukarıdaki çağrıdır.

```shell
spx synthesize --foreach audio.output;text in @C:\your\path\to\text_synthesis.tsv
```

## <a name="next-steps"></a>Sonraki adımlar

* SDK ['yı kullanarak konuşma tanımayı](./quickstarts/speech-to-text-from-microphone.md) veya [konuşma senşliğini](./quickstarts/text-to-speech.md) hızlı başlangıçlara tamamlayabilirsiniz.
