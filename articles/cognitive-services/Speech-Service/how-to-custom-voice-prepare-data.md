---
title: Özel Ses - Konuşma hizmeti için veri hazırlama
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti ile markanız için özel bir ses oluşturun. Stüdyo kayıtları ve ilişkili komut dosyaları sağlarsanız, hizmet kaydedilen sese ayarlanmış benzersiz bir ses modeli oluşturur. Ürünlerinizde, araçlarınızda ve uygulamalarınızda konuşma sentezlemek için bu sesi kullanın.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: 5427e9f996fb77d455aa8064fc7cb1c65e1fcf7e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74805986"
---
# <a name="prepare-data-to-create-a-custom-voice"></a>Özel bir ses oluşturmak için veri hazırlama

Uygulamanız için özel bir metinden konuşmaya ses oluşturmaya hazır olduğunuzda, ilk adım ses modelini eğitmeye başlamak için ses kayıtları ve ilişkili komut dosyaları toplamaktır. Konuşma hizmeti, kayıtlardaki sesle eşleşecek şekilde ayarlanmış benzersiz bir ses oluşturmak için bu verileri kullanır. Sesi eğittikten sonra, uygulamalarınızda konuşma sentezlemeye başlayabilirsiniz.

Kavram kanıtı oluşturmak için az miktarda veriyle başlayabilirsiniz. Ancak, ne kadar çok veri sağlarsanız, özel sesiniz o kadar doğal olur. Kendi metinden konuşmaya ses modelinizi eğitebilmeniz için önce ses kayıtlarına ve ilgili metin transkripsiyonlarına ihtiyacınız vardır. Bu sayfada, veri türlerini, bunların nasıl kullanıldığını ve her birinin nasıl yönetilenini inceleriz.

## <a name="data-types"></a>Veri türleri

Sesli eğitim veri seti ses kayıtlarını ve ilişkili transkripsiyonları içeren bir metin dosyasını içerir. Her ses dosyası tek bir sözcük (tek bir cümle veya iletişim sistemi için tek bir dönüş) içermeli ve 15 saniyeden kısa olmalıdır.

Bazı durumlarda, doğru veri kümesi hazır olmayabilir ve transkriptli veya transkriptsiz, kısa veya uzun olan kullanılabilir ses dosyalarıyla özel ses eğitimini test etmek isteyeceksiniz. Toplu [Transkripsiyon API'sini](batch-transcription.md)kullanarak sesinizi dile segmente vermenize ve transkript hazırlamanıza yardımcı olacak araçlar (beta) sağlıyoruz.

Bu tablo, veri türlerini ve her birinin özel bir metinden konuşmaya ses modeli oluşturmak için nasıl kullanıldığını listeler.

| Veri türü | Açıklama | Kullanılması gereken durumlar | Ek hizmet gereklidir | Bir modeli eğitmek için miktar | Yerel(ler) |
| --------- | ----------- | ----------- | --------------------------- | ----------------------------- | --------- |
| **Bireysel söyleyiş + eşleşen transkript** | Ses dosyalarının (.wav) tek tek söylenmeleri olarak toplanması (.zip). Her ses dosyası 15 saniye veya daha kısa uzunlukta olmalı ve biçimlendirilmiş bir transkript (.txt) ile eşleştirilmiş olmalıdır. | Eşleşen transkriptler ile profesyonel kayıtlar | Eğitime hazır. | En-US ve zh-CN için zor bir gereklilik yoktur. Diğer yerel durumlar için 2.000'den fazla farklı söyleyiş. | [Tüm Özel Ses yerel leri](language-support.md#customization) |
| **Uzun ses + transkript (beta)** | Tüm konuşulan sözcükleri içeren bir transkript (.txt) ile eşleştirilmiş uzun, segmentsiz ses dosyalarından (20 saniyeden uzun) oluşan bir koleksiyon (.zip). | Ses dosyalarınız ve eşleşen transkriptleriniz var, ancak bunlar dile rızık olarak bölümlenmez. | Segmentasyon (toplu transkripsiyon kullanarak).<br>Gerektiğinde ses biçimi dönüşümü. | Zor bir gereksinim yok  | [Tüm Özel Ses yerel leri](language-support.md#customization) |
| **Yalnızca ses (beta)** | Transkriptsiz ses dosyalarının toplanması (.zip). | Transkript olmadan sadece ses dosyaları nız mevcuttur. | Segmentasyon + transkript oluşturma (toplu transkripsiyon kullanarak).<br>Gerektiğinde ses biçimi dönüşümü.| Zor bir gereksinim yok | [Tüm Özel Ses yerel leri](language-support.md#customization) |

Dosyalar bir veri kümesine göre türüne göre gruplandırılmalı ve zip dosyası olarak yüklenmelidir. Her veri kümesi yalnızca tek bir veri türü içerebilir.

> [!NOTE]
> Abonelik başına alınmasına izin verilen maksimum veri kümesi sayısı, ücretsiz abonelik (F0) kullanıcıları için 10.zip dosyası ve standart abonelik (S0) kullanıcıları için 500'dür.

## <a name="individual-utterances--matching-transcript"></a>Bireysel söyleyiş + eşleşen transkript

Tek tek konuşmaların ve eşleşen transkriptin kayıtlarını iki şekilde hazırlayabilirsiniz. Ya bir komut dosyası yazın ve bir ses yeteneği tarafından okundu veya kamuya açık ses kullanın ve metin için transkripsiyonu. İkincisini yaparsanız, "um" ve diğer dolgu sesleri, kekemeler, mırıldanan sözcükler veya yanlış telaffuzlar gibi ses dosyalarından disfluencies'i edin.

İyi bir ses yazı tipi oluşturmak için, kayıtları yüksek kaliteli mikrofona sahip sessiz bir odada oluşturun. Tutarlı hacim, konuşma hızı, konuşma perdesi ve konuşmanın ifade edici tavırları esastır.

> [!TIP]
> Üretim için bir ses oluşturmak için, profesyonel bir kayıt stüdyosu ve ses yeteneği kullanmanızı öneririz. Daha fazla bilgi için, [özel bir ses için ses örneklerinin nasıl kaydedildiğini](record-custom-voice-samples.md)görün.

### <a name="audio-files"></a>Ses dosyaları

Her ses dosyası 15 saniyeden kısa bir süre içinde tek bir sözcük (tek bir cümle veya iletişim sisteminin tek bir dönüşü) içermelidir. Tüm dosyalar aynı konuşma dilinde olmalıdır. Çince-İngilizce çift dilli ler hariç, çok dilli özel metinden konuşmaya sesler desteklenmez. Her ses dosyası, dosya adı uzantısı .wav ile benzersiz bir sayısal dosya adı olmalıdır.

Ses hazırlarken aşağıdaki yönergeleri izleyin.

| Özellik | Değer |
| -------- | ----- |
| Dosya biçimi | RIFF (.wav), bir .zip dosyasında gruplandırılır |
| Örnekleme oranı | En az 16.000 Hz |
| Örnek format | PCM, 16-bit |
| Dosya adı | Sayısal, .wav uzantılı. Yinelenen dosya adlarının girmesine izin verilmiyor. |
| Ses uzunluğu | 15 saniyeden kısa |
| Arşiv biçimi | .zip |
| Maksimum arşiv boyutu | 2048 MB |

> [!NOTE]
> .wav 16.000 Hz'den düşük örnekleme oranına sahip dosyalar reddedilir. .zip dosyası farklı örnek lem oranlarına sahip .wav dosyaları içeriyorsa, yalnızca 16.000 Hz'e eşit veya daha yüksek dosyalar alınır. Portal şu anda 200 MB'a kadar zip arşivi ithal ediyor. Ancak, birden çok arşiv yüklenebilir.

### <a name="transcripts"></a>Transkript

Transkripsiyon dosyası düz bir metin dosyasıdır. Transkripsiyonlarınızı hazırlamak için bu yönergeleri kullanın.

| Özellik | Değer |
| -------- | ----- |
| Dosya biçimi | Düz metin (.txt) |
| Kodlama biçimi | ANSI/ASCII, UTF-8, UTF-8-BOM, UTF-16-LE veya UTF-16-BE. zh-CN için ANSI/ASCII ve UTF-8 kodlamaları desteklenmez. |
| Satır başına konuşma sayısı | **Bir** - Transkripsiyon dosyasının her satırı ses dosyalarından birinin adını içermelidir ve ardından buna karşılık gelen transkripsiyon olmalıdır. Dosya adı ve transkripsiyon sekme (\t) ile ayrılmalıdır. |
| En büyük dosya boyutu | 2048 MB |

Aşağıda transkriptlerin bir .txt dosyasında söyleyerek nasıl düzenlendiğine bir örnek verilmiştir:

```
0000000001[tab] This is the waistline, and it's falling.
0000000002[tab] We have trouble scoring.
0000000003[tab] It was Janet Maslin.
```
Transkriptlerin ilgili sesin %100 doğru transkripsiyonu olması önemlidir. Transkriptteki hatalar eğitim sırasında kalite kaybına neden olacaktır.

> [!TIP]
> Üretim metinden konuşmaya sesler inşa ederken, hem fonetik kapsama alanını hem de verimliliği hesaba katan sözcükleri (veya komut dosyaları yazın) seçin. İstediğinsonucu almakta sorun mu yaşıyorsun? Bize danıştAk hakkında daha fazla bilgi edinmek için Özel Ses ekibi [yle iletişime geçin.](mailto:speechsupport@microsoft.com)

## <a name="long-audio--transcript-beta"></a>Uzun ses + transkript (beta)

Bazı durumlarda, kullanılabilir kesimi bölümlenmiş ses olmayabilir. Uzun ses dosyalarını segmente ve transkripsiyonoluşturmanıza yardımcı olmak için özel ses portalı aracılığıyla bir hizmet (beta) salıyoruz. Bu hizmetin konuşmadan metne abonelik kullanımınız için ücretlendireceğini unutmayın.

> [!NOTE]
> Uzun ses segmentasyon hizmeti, yalnızca standart abonelik (S0) kullanıcılarını destekleyen konuşmadan metne toplu transkripsiyon özelliğinden yararlanır. Segmentasyonun işlenmesi sırasında, ses dosyalarınız ve transkriptleriniz, verilerinizin doğruluğunun iyileştirilebilmeleri için tanıma modelini hassaslaştırmak için Özel Konuşma hizmetine de gönderilir. Bu işlem sırasında hiçbir veri tutulmaz. Segmentasyon yapıldıktan sonra, yalnızca sözcük bölümleri ve bunların haritalama transkriptleri indirme ve eğitim için depolanacaktır.

### <a name="audio-files"></a>Ses dosyaları

Ses kesimi için hazırlanırken aşağıdaki yönergeleri izleyin.

| Özellik | Değer |
| -------- | ----- |
| Dosya biçimi | PCM'de en az 16 khz-16-bit veya .mp3'te en az 256 KBps bit hızına sahip, bir .zip dosyasında gruplanmış örnekleme hızına sahip RIFF (.wav) |
| Dosya adı | ASCII ve Unicode karakterleri desteklendi. Yinelenen adlar giremez. |
| Ses uzunluğu | 20 saniyeden uzun |
| Arşiv biçimi | .zip |
| Maksimum arşiv boyutu | 2048 MB |

Tüm ses dosyaları bir zip dosyasında gruplandırılmalıdır. .wav dosyaları ve .mp3 dosyalarını tek bir ses zip'ine koymak ta sorun değil. Örneğin, 'kingstory.wav', 45 saniye uzunluğunda ve 'queenstory.mp3' adlı başka bir ses, 200 saniye uzunluğunda adlı bir ses dosyası içeren bir zip dosyası yükleyebilirsiniz. Tüm .mp3 dosyaları işlendikten sonra .wav biçimine dönüştürülür.

### <a name="transcripts"></a>Transkript

Transkriptler bu tabloda listelenen özelliklere göre hazırlanmalıdır. Her ses dosyası bir transkript ile eşlenmelidir.

| Özellik | Değer |
| -------- | ----- |
| Dosya biçimi | Düz metin (.txt), .zip olarak gruplandırılır |
| Dosya adı | Eşleşen ses dosyasıyla aynı adı kullanma |
| Kodlama biçimi | Yalnızca UTF-8-BOM |
| Satır başına konuşma sayısı | Sınır yok |
| En büyük dosya boyutu | 2048 MB |

Bu veri türündeki tüm transkript dosyaları bir zip dosyasında gruplandırılmalıdır. Örneğin, 'kingstory.wav', 45 saniye uzunluğunda ve 'queenstory.mp3' adında bir ses dosyası içeren bir zip dosyası yüklemiş siniz, 200 saniye uzunluğunda. Biri 'kingstory.txt', diğeri 'queenstory.txt' olmak üzere iki transkript içeren başka bir zip dosyası yüklemeniz gerekir. Her düz metin dosyası içinde, eşleşen ses için tam doğru transkripsiyon sağlayacaktır.

Veri setiniz başarıyla yüklendikten sonra, sağlanan transkripte göre ses dosyasını dile getirerek bölmenize yardımcı olacağız. Veri kümesini indirerek segmente ayrılmış kalıpları ve eşleşen transkriptleri kontrol edebilirsiniz. Benzersiz iT'ler otomatik olarak segmente edilmiş tümleme lere atanır. Verdiğiniz transkriptlerin %100 doğru olduğundan emin olmak önemlidir. Transkriptteki hatalar ses segmentasyonu sırasında doğruluğu azaltabilir ve daha sonra gelen eğitim aşamasında kalite kaybına neden olabilir.

## <a name="audio-only-beta"></a>Yalnızca ses (beta)

Ses kayıtlarınız için transkripsiyonlarınız yoksa, verilerinizi yüklemek için **yalnızca Ses** seçeneğini kullanın. Sistemimiz ses dosyalarınızı segmente ve yazıya aktarmanıza yardımcı olabilir. Bu hizmetin konuşmadan metne abonelik kullanımınız için geçerli olacağını unutmayın.

Ses hazırlarken aşağıdaki yönergeleri izleyin.

> [!NOTE]
> Uzun ses segmentasyon hizmeti, yalnızca standart abonelik (S0) kullanıcılarını destekleyen konuşmadan metne toplu transkripsiyon özelliğinden yararlanır.

| Özellik | Değer |
| -------- | ----- |
| Dosya biçimi | PCM'de en az 16 khz-16-bit veya .mp3'te en az 256 KBps bit hızına sahip, bir .zip dosyasında gruplanmış örnekleme hızına sahip RIFF (.wav) |
| Dosya adı | ASCII ve Unicode karakterleri desteklendi. Yinelenen ada izin verilmiyor. |
| Ses uzunluğu | 20 saniyeden uzun |
| Arşiv biçimi | .zip |
| Maksimum arşiv boyutu | 2048 MB |

Tüm ses dosyaları bir zip dosyasında gruplandırılmalıdır. Veri setiniz başarıyla yüklendikten sonra, ses dosyasını konuşma toplu transkripsiyon hizmetimize dayalı olarak dile getirerek segmentize etmenize yardımcı olacağız. Benzersiz iT'ler otomatik olarak segmente edilmiş tümleme lere atanır. Eşleşen transkriptler konuşma tanıma yoluyla oluşturulacaktır. Tüm .mp3 dosyaları işlendikten sonra .wav biçimine dönüştürülür. Veri kümesini indirerek segmente ayrılmış kalıpları ve eşleşen transkriptleri kontrol edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel Ses Oluşturma](how-to-custom-voice-create-voice.md)
- [Kılavuz: Ses örneklerinizi kaydetme](record-custom-voice-samples.md)
