---
title: Konuşma CLı toplu işlemleri-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma CLı ile, toplu konuşmayı metin (konuşma tanıma), okuma için toplu metin (konuşma senişte) yapmayı öğrenin.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 01/13/2021
ms.author: erhopf
ms.openlocfilehash: 60cacafc89f2335b87885e4ea11dcf8992d68c3f
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2021
ms.locfileid: "98540069"
---
# <a name="speech-cli-batch-operations"></a>Konuşma CLı toplu işlemleri

Azure konuşma Hizmetleri 'ni kullanırken ortak görevler Batch operasyonlardır. Bu makalede, konuşma CLı ile metin (konuşma tanıma), okuma için toplu metin (konuşma senu) için toplu konuşmayı nasıl gerçekleştireceğinizi öğreneceksiniz. Yapmayı öğreneceklerinizin listesi aşağıdadır:

* Toplu konuşma tanımayı bir ses dosyaları dizininde Çalıştır
* Bir dosya üzerinden yineleme yaparak toplu konuşma senşişini Çalıştır `.tsv`

## <a name="batch-speech-to-text-speech-recognition"></a>Toplu konuşmayı metne dönüştürme (konuşma tanıma)

Konuşma hizmeti genellikle ses dosyalarından konuşmayı tanımak için kullanılır. Bu örnekte, her bir dosya için tanınma çıkışını yakalamak üzere konuşmayı CLı kullanarak bir dizin üzerinde nasıl yineleyeceğinizi öğreneceksiniz `.wav` . `--files`Bayrak, ses dosyalarının depolandığı dizini işaret etmek için kullanılır ve joker karakter, `*.wav` konuşma CLI 'nın uzantılı her dosya üzerinde tanıma çalıştırmasını söylemek için kullanılır `.wav` . Her bir tanıma dosyası için çıkış, içinde sekmeyle ayrılmış bir değer olarak yazılır `speech_output.tsv` .

> [!NOTE]
> `--threads`Bağımsız değişken, komutlar için sonraki bölümde de kullanılabilir `spx synthesize` ve kullanılabilir Iş parçacıkları CPU 'ya ve geçerli yük yüzdesine göre değişir.

```console
spx recognize --files C:\your_wav_file_dir\*.wav --output file C:\output_dir\speech_output.tsv --threads 10
```

Aşağıda çıkış dosyası yapısına bir örnek verilmiştir.

```output
audio.input.id  recognizer.session.started.sessionid    recognizer.recognized.result.text
sample_1    07baa2f8d9fd4fbcb9faea451ce05475    A sample wave file.
sample_2    8f9b378f6d0b42f99522f1173492f013    Sample text synthesized.
```

## <a name="batch-text-to-speech-speech-synthesis"></a>Toplu metin okuma (konuşma birleştirme)

Batch metin okuma 'yı çalıştırmanın en kolay yolu, yeni bir `.tsv` (sekmeyle ayrılmış değer) dosyası oluşturmak ve `--foreach` konuşma CLI 'de komutunu kullanmaktır. En `.tsv` sevdiğiniz metin düzenleyiciyi kullanarak bir dosya oluşturabilirsiniz, bu örnek için şunu arayalım `text_synthesis.tsv` :

>[!IMPORTANT]
> Bu metin dosyasının içeriğini kopyalarken, dosyanızın dosya konumu ve metin arasında boşluk olmayan bir **sekmeye** sahip olduğundan emin olun. Bazen, içeriği bu örnekteki kopyalarken, sekmeler boşluklara dönüştürülür ve `spx` çalıştırıldığında komutun başarısız olmasına neden olur.

```Input
audio.output    text
C:\batch_wav_output\wav_1.wav   Sample text to synthesize.
C:\batch_wav_output\wav_2.wav   Using the Speech CLI to run batch-synthesis.
C:\batch_wav_output\wav_3.wav   Some more text to test capabilities.
```

Ardından, öğesini işaret etmek için bir komut çalıştırırsınız `text_synthesis.tsv` , her bir alanda sen, `text` ve sonuç olarak karşılık gelen `audio.output` yola bir dosya olarak yazar `.wav` .

```console
spx synthesize --foreach in @C:\your\path\to\text_synthesis.tsv
```

Bu komut, `spx synthesize --text "Sample text to synthesize" --audio output C:\batch_wav_output\wav_1.wav` dosyadaki **her kayıt için** çalıştırmanın eşdeğeridir `.tsv` .

Dikkat edilmesi gereken birkaç şey:

* Sütun başlıkları, `audio.output` ve `text` sırasıyla komut satırı bağımsız değişkenlerine karşılık gelir `--audio output` `--text` . Gibi çok parçalı komut satırı bağımsız değişkenleri `--audio output` , boşluk olmadan dosyada, önde gelen çizgiler olmadan ve dizeleri ayıran noktalarla biçimlendirilmelidir. Örneğin, `audio.output` . Diğer mevcut komut satırı bağımsız değişkenleri, bu model kullanılarak ek sütunlar olarak dosyaya eklenebilir.
* Dosya bu şekilde biçimlendirildiğinde, başka bir bağımsız değişken geçirilmesi gerekmez `--foreach` .
* İçindeki her değeri bir sekmeyle ayırtığınızdan emin olun `.tsv` . 

Ancak, `.tsv` Aşağıdaki örnekte olduğu gibi bir dosyanız varsa, komut satırı bağımsız değişkenleriyle **eşleşmeyen** sütun başlıkları vardır:

```Input
wav_path    str_text
C:\batch_wav_output\wav_1.wav   Sample text to synthesize.
C:\batch_wav_output\wav_2.wav   Using the Speech CLI to run batch-synthesis.
C:\batch_wav_output\wav_3.wav   Some more text to test capabilities.
```

Bu alan adlarını, çağrıda aşağıdaki sözdizimini kullanarak doğru bağımsız değişkenlere geçersiz kılabilirsiniz `--foreach` . Bu, yukarıdaki çağrıdır.

```console
spx synthesize --foreach audio.output;text in @C:\your\path\to\text_synthesis.tsv
```

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma CLI’sine genel bakış](./spx-overview.md)
* [Konuşma CLı hızlı başlangıç](./spx-basics.md)
