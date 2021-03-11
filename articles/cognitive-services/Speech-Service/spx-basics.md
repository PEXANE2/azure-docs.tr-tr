---
title: Konuşma CLı hızlı başlangıç-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Azure konuşma CLı ile çalışmaya başlayın. Konuşma gibi konuşma hizmetleriyle metin, metin okuma ve konuşma çevirisi ile kod yazmadan etkileşim kurabilirsiniz.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 01/13/2021
ms.author: trbye
ms.openlocfilehash: 53138a22c58e89ade4af234630e9429a19738a6a
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102556477"
---
# <a name="get-started-with-the-azure-speech-cli"></a>Azure konuşma CLı ile çalışmaya başlama

Bu makalede, konuşma ve metin yazma, metin okuma ve konuşma çevirisi gibi konuşma hizmetlerine kod yazmadan erişmek için bir komut satırı arabirimi olan konuşma CLı 'yı nasıl kullanacağınızı öğreneceksiniz. Konuşma CLı, üretime hazırlanıyor ve konuşma hizmetindeki basit iş akışlarını otomatikleştirmek için `.bat` veya kabuk betikleri kullanılarak kullanılabilir.

Bu makalede komut istemi, Terminal veya PowerShell ile ilgili bilgi sahibi olduğunuz varsayılır.

[!INCLUDE [](includes/spx-setup.md)]

## <a name="basic-usage"></a>Temel kullanım

Bu bölümde, genellikle ilk kez test ve deneme için yararlı olan birkaç temel SPX komutu gösterilmektedir. Aşağıdaki komutu çalıştırarak araca yerleşik yardımı görüntüleyerek başlayın.

```console
spx
```

Yardım konularını anahtar sözcüğe göre arayabilirsiniz. Örneğin, konuşma CLı kullanım örneklerinin bir listesini görmek için aşağıdaki komutu girin:

```console
spx help find --topics "examples"
```

Recognize komutuna yönelik seçenekleri görmek için aşağıdaki komutu girin:

```console
spx help recognize
```

Sağ sütunda listelenen ek yardım komutları. Alt komutlar hakkında ayrıntılı yardım almak için bu komutları girebilirsiniz.

## <a name="speech-to-text-speech-recognition"></a>Konuşmayı metne dönüştürme (konuşma tanıma)

Sisteminizin varsayılan mikrofonunu kullanarak konuşmayı metne (konuşma tanıma) dönüştürmek için konuşma CLı 'sını kullanalım. Komutu girdikten sonra, SPX geçerli etkin giriş cihazında ses dinlemeye başlar ve **ENTER** tuşuna bastığınızda durur. Kaydedilen konuşma daha sonra tanınır ve konsol çıkışında metne dönüştürülür.

>[!IMPORTANT]
> Docker kapsayıcısı kullanıyorsanız, `--microphone` çalışmaz.

Şu komutu çalıştırın:

```console
spx recognize --microphone
```

Konuşma CLı 'sı sayesinde konuşmayı bir ses dosyasından de tanıyabilirsiniz.

```console
spx recognize --file /path/to/file.wav
```

> [!TIP]
> Bir Docker kapsayıcısında konuşmayı bir ses dosyasından tanıyor ve ses dosyasının önceki adımda bağladığınız dizinde bulunduğundan emin olun.

Unutmayın, konuşma CLı 'nın tanıma seçenekleri hakkında daha fazla bilgi edinmek istiyorsanız şunu yazın:

```console
spx help recognize
```

## <a name="text-to-speech-speech-synthesis"></a>Metinden konuşmaya (konuşma senşi)

Aşağıdaki komutun çalıştırılması metin girişi olarak ele alınır ve birleştirilmiş konuşmayı geçerli etkin çıkış cihazına (örneğin, bilgisayarınızın hoparlörleri) çıktı olarak alır.

```console
spx synthesize --text "Testing synthesis using the Speech CLI" --speakers
```

Ayrıca, sentezlenmiş çıktıyı dosyasına kaydedebilirsiniz. Bu örnekte, dizinde komutun çalıştırıldığı adlı bir dosya oluşturacağız `my-sample.wav` .

```console
spx synthesize --text "Enjoy using the Speech CLI." --audio output my-sample.wav
```

Bu örnekler, Ingilizce 'de test olduğunuzu kabul ediyor. Ancak, çok sayıda dilde konuşma birleştirmemiz destekliyoruz. Bu komutla birlikte seslerin tam listesini çekerek veya [dil desteği sayfasını](./language-support.md)ziyaret edebilirsiniz.

```console
spx synthesize --voices
```

Bulduğunuz sesden birini nasıl kullanacağınızı aşağıda bulabilirsiniz.

```console
spx synthesize --text "Bienvenue chez moi." --voice fr-CA-Caroline --speakers
```

Unutmayın, konuşma CLı 'nın senlim seçenekleri hakkında daha fazla bilgi edinmek istiyorsanız şunu yazın:

```console
spx help synthesize
```

## <a name="speech-to-text-translation"></a>Konuşmayı metne dönüştürme

Konuşma CLı ile konuşma 'yı metin çevirisi de yapabilirsiniz. Varsayılan mikrofonunuzdan ses yakalamak ve çeviriyi metin olarak çıktısını almak için bu komutu çalıştırın. Komutuyla ve dilini sağlamanız gerektiğini aklınızda bulundurun `source` `target` `translate` .

```console
spx translate --microphone --source en-US --target ru-RU
```

Birden çok dile çevrilirken, dil kodlarını ile ayırın `;` .

```console
spx translate --microphone --source en-US --target ru-RU;fr-FR;es-ES
```

Çeviriniz çıkışını kaydetmek istiyorsanız `--output` bayrağını kullanın. Bu örnekte, bir dosyadan de okuyacaksınız.

```console
spx translate --file /some/file/path/input.wav --source en-US --target ru-RU --output file /some/file/path/russian_translation.txt
```

> [!NOTE]
> Tüm desteklenen dillerin bir listesi için ilgili yerel ayar kodlarıyla birlikte [dil ve yerel ayar makalesine](language-support.md) bakın.

Unutmayın, konuşma CLı 'nın çeviri seçenekleri hakkında daha fazla bilgi edinmek istiyorsanız şunu yazın:

```console
spx help translate
```

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma CLI’sini yapılandırma seçenekleri](./spx-data-store-configuration.md)
* [Konuşma CLı ile Batch işlemleri](./spx-batch-operations.md)
