---
title: Özel Ses Oluşturma - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Verilerinizi yüklemeye hazır olduğunuzda, Özel Ses portalına gidin. Özel Ses projesi oluşturun veya seçin. Proje, ses eğitiminiz için kullanmayı niyetettiğiniz veriler olarak doğru dili/yerel durumu ve cinsiyet özelliklerini paylaşmalıdır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: bbe1d651a7d2d2cac1b1aa78b815b2797ad185c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76717319"
---
# <a name="create-a-custom-voice"></a>Özel Ses Oluşturma

[Özel Ses için veri hazırlama'da,](how-to-custom-voice-prepare-data.md)özel bir ses ve farklı biçim gereksinimlerini eğitmek için kullanabileceğiniz farklı veri türlerini tanımladık. Verilerinizi hazırladıktan sonra, bunları Özel Ses [portalına](https://aka.ms/custom-voice-portal)veya Özel Ses eğitim API'si aracılığıyla yüklemeye başlayabilirsiniz. Burada portal üzerinden özel bir ses eğitim adımları açıklar.

> [!NOTE]
> Bu sayfa, Özel [Ses ile başlayın](how-to-custom-voice.md) ve [Özel Ses için veri hazırlayın](how-to-custom-voice-prepare-data.md)ve bir Özel Ses projesi oluşturduk okudum varsayar.

Özel ses için desteklenen dilleri kontrol [edin: özelleştirme için dil.](language-support.md#customization)

## <a name="upload-your-datasets"></a>Veri kümelerinizi yükleyin

Verilerinizi yüklemeye hazır olduğunuzda, Özel [Ses portalına](https://aka.ms/custom-voice-portal)gidin. Özel Ses projesi oluşturun veya seçin. Proje, ses eğitiminiz için kullanmayı niyetettiğiniz veriler olarak doğru dili/yerel durumu ve cinsiyet özelliklerini paylaşmalıdır. Örneğin, sahip `en-GB` olduğunuz ses kayıtlarının İngiltere aksanıyla İngilizce olarak yapIlip yapılmadığını seçin.

**Veri** sekmesine gidin ve **Verileri Yükle'yi**tıklatın. Sihirbazda, hazırladıklarına uyan doğru veri türünü seçin.

Yüklediğiniz her veri kümesi, seçtiğiniz veri türü için gereksinimleri karşılamalıdır. Verilerinizi yüklenmeden önce doğru biçimlendirmek önemlidir. Bu, verilerin Özel Ses hizmeti tarafından doğru bir şekilde işlenmesini sağlar. Özel [Ses için veri hazırlamaya](how-to-custom-voice-prepare-data.md) gidin ve verilerinizin doğru biçimlendirilmiş olduğundan emin olun.

> [!NOTE]
> Ücretsiz abonelik (F0) kullanıcıları aynı anda iki veri kümesi yükleyebilirsiniz. Standart abonelik (S0) kullanıcıları aynı anda beş veri kümesi yükleyebilir. Sınıra ulaşırsanız, veri kümelerinizden en az birinin içe aktarma yı bitirmesini bekleyin. O zaman tekrar dene.

> [!NOTE]
> Abonelik başına alınmasına izin verilen maksimum veri kümesi sayısı, ücretsiz abonelik (F0) kullanıcıları için 10.zip dosyası ve standart abonelik (S0) kullanıcıları için 500'dür.

Yükleme düğmesine bastığınızda veri kümeleri otomatik olarak doğrulanır. Veri doğrulama, dosya biçimlerini, boyutlarını ve örnekleme hızlarını doğrulamak için ses dosyalarında bir dizi denetim içerir. Varsa hataları düzeltin ve yeniden gönderin. Veri alma isteği başarıyla başlatıldığında, veri tablosunda yeni yüklediğiniz veri kümesine karşılık gelen bir giriş görmeniz gerekir.

Aşağıdaki tablo, içe aktarılan veri kümeleri için işleme durumlarını gösterir:

| Durum | Anlamı |
| ----- | ------- |
| İşleniyor | Veri kümeniz alındı ve işleniyor. |
| Başarılı oldu | Veri kümeniz doğrulandı ve artık bir ses modeli oluşturmak için kullanılabilir. |
| Başarısız | Dosya hataları, veri sorunları veya ağ sorunları gibi birçok nedenden dolayı veri kümeniz işleme sırasında başarısız oldu. |

Doğrulama tamamlandıktan sonra, **Tümlemeler** sütununda her bir veri kümeniz için eşleşen tüm sözcük sayısını görebilirsiniz. Seçtiğiniz veri türü uzun ses segmentasyonu gerektiriyorsa, bu sütun yalnızca transkriptlerinize veya konuşma transkripsiyon hizmetinize dayanarak sizin için bölümlediğimiz söylemleri yansıtır. Başarıyla aktarılan konuşmaların ayrıntı sonuçlarını ve bunların eşleme transkriptlerini görüntülemek için doğrulanmış veri kümesini daha da indirebilirsiniz. İpucu: uzun ses segmentasyonunun veri işlemeyi tamamlaması bir saatten fazla sürebilir.

en-US ve zh-CN veri kümeleri için, her kayıt için telaffuz puanlarını ve gürültü düzeyini kontrol etmek için bir rapor daha indirebilirsiniz. Telaffuz puanı 0 ile 100 arasında değişir. 70'in altındaki bir puan normalde bir konuşma hatası veya komut dosyası uyuşmazlığı gösterir. Ağır bir vurgu telaffuz puanınızı azaltabilir ve oluşturulan dijital sesi etkileyebilir.

Daha yüksek bir sinyal-gürültü oranı (SNR) sesinizde daha düşük gürültü yü gösterir. Genellikle profesyonel stüdyolarda kayıt yaparak 50+ SNR'a ulaşabilirsiniz. 20'nin altında Bir SNR ile ses oluşturulan ses bariz gürültü neden olabilir.

Düşük telaffuz puanları veya düşük sinyal-gürültü oranları ile tüm söyleyiyi yeniden kaydetmeyi düşünün. Yeniden kaydedemezseniz, bu sözcükleri veri kümenizden çıkarabilirsiniz.

## <a name="build-your-custom-voice-model"></a>Özel ses modelinizi oluşturun

Veri setiniz doğrulandıktan sonra, özel ses modelinizi oluşturmak için kullanabilirsiniz.

1.  Metinden Konuşmaya > **Özel Ses > Eğitimine**gidin.

2.  **Tren modeline**tıklayın.

3.  Ardından, bu modeli tanımlamanıza yardımcı olacak bir **Ad** ve **Açıklama** girin.

    Bir ad dikkatle seçin. Buraya girdiğiniz ad, SSML girişinin bir parçası olarak konuşma sentezi isteğinizdeki sesi belirtmek için kullandığınız ad olacaktır. Yalnızca harflere, sayılara ve -, \_, , ve (', ') gibi birkaç noktalama işaretine izin verilir. Farklı ses modelleri için farklı adlar kullanın.

    **Açıklama** alanının yaygın kullanımı, modeli oluşturmak için kullanılan veri kümelerinin adlarını kaydetmektir.

4.  Eğitim **veri sini seç** sayfasından, eğitim için kullanmak istediğiniz bir veya birden çok veri kümesini seçin. Göndermeden önce söylentin sayısını kontrol edin. En-US ve zh-CN ses modelleri için herhangi bir sayıda söyleyiş ile başlayabilirsiniz. Diğer yerel yerler için, bir sesi eğitebilmek için 2.000'den fazla sözcük seçmeniz gerekir.

    > [!NOTE]
    > Yinelenen ses adları eğitimden kaldırılır. Seçtiğiniz veri kümelerinin birden çok .zip dosyasında aynı ses adlarını içermediğinden emin olun.

    > [!TIP]
    > Kaliteli sonuçlar için aynı hoparlörden gelen veri kümelerini kullanmak gereklidir. Eğitim için gönderdiğiniz veri kümeleri toplam 6.000'den az farklı söz içeriyorsa, ses modelinizi İstatistiksel Parametrik Sentez tekniği ile eğiteceksiniz. Eğitim verilerinizin toplam 6.000 farklı kelime sayısını aşması durumunda, Concatenation Synthesis tekniği ile bir eğitim sürecini başlatmış olursunuz. Normalde concatenation teknolojisi daha doğal ve daha yüksek sadakat ses sonuçları neden olabilir. Kamuya açık [sinirsel seslere](language-support.md#neural-voices)eşdeğer bir dijital ses üretebilen en son Nöral TTS teknolojisine sahip bir model eğitmek istiyorsanız [Özel Ses ekibiyle iletişime geçin.](https://go.microsoft.com/fwlink/?linkid=2108737)

5.  Ses modelinizi oluşturmaya başlamak için **Tren'i** tıklatın.

Eğitim tablosu, bu yeni oluşturulan modele karşılık gelen yeni bir giriş görüntüler. Tablo da durumu görüntüler: İşleme, Başarılı, Başarısız.

Gösterilen durum, burada gösterildiği gibi veri kümenizi bir ses modeline dönüştürme işlemini yansıtır.

| Durum | Anlamı |
| ----- | ------- |
| İşleniyor | Ses modelin oluşturuluyor. |
| Başarılı oldu | Ses modeliniz oluşturuldu ve dağıtılabilir. |
| Başarısız | Ses modeliniz, örneğin görünmeyen veri sorunları veya ağ sorunları gibi birçok nedenden dolayı eğitimde başarısız oldu. |

Eğitim süresi işlenen ses verilerinin hacmine bağlı olarak değişir. Tipik zamanlar, yüzlerce söz için yaklaşık 30 dakika ile 20.000 kelime için 40 saat arasında değişir. Model eğitiminiz başarılı olduktan sonra, bunu test etmeye başlayabilirsiniz.

> [!NOTE]
> Ücretsiz abonelik (F0) kullanıcıları aynı anda bir sesli yazı tipi eğitebilirsiniz. Standart abonelik (S0) kullanıcıları aynı anda üç sesi eğitebilir. Sınıra ulaşırsanız, ses yazı tiplerinizden en az birinin eğitimini bitirmesini bekleyin ve sonra yeniden deneyin.

> [!NOTE]
> Abonelik başına eğitilmesine izin verilen maksimum ses modeli sayısı ücretsiz abonelik (F0) kullanıcıları için 10 model ve standart abonelik (S0) kullanıcıları için 100 modeldir.

Eğer nöral ses eğitim yeteneği kullanıyorsanız, gerçek zamanlı akış senaryoları için optimize edilmiş bir model veya eşzamanlı [uzun ses sentezi](long-audio-api.md)için optimize edilmiş bir HD nöral modeli eğitmek için seçebilirsiniz.  

## <a name="test-your-voice-model"></a>Ses modelinizi test edin

Ses yazı tipiniz başarıyla oluşturulmadan sonra, kullanım için dağıtmadan önce test edebilirsiniz.

1.  Metinden Konuşmaya > **Özel Ses > Testine**gidin.

2.  **Test Ekle'yi**tıklatın.

3.  Test etmek istediğiniz bir veya birden çok modeli seçin.

4.  Sesin(ler) konuşmasını istediğiniz metni sağlayın. Aynı anda birden çok modeli test etmeyi seçtiyseniz, aynı metin farklı modeller için sınama için kullanılır.

    > [!NOTE]
    > Metninizin dili, ses yazı tipinizin diliyle aynı olmalıdır. Sadece başarılı eğitimli modeller test edilebilir. Bu adımda yalnızca düz metin desteklenir.

5.  **Oluştur'u**tıklatın.

Sınav isteğinizi gönderdikten sonra, test sayfasına geri döneceksiniz. Tablo şimdi yeni isteğinize ve durum sütununa karşılık gelen bir giriş içerir. Konuşmayı sentezlemek birkaç dakika sürebilir. Durum sütununda **Başarılı**olduğunu söylediğinde, sesi oynatabilir veya metin girişini (.txt dosyası) ve ses çıktısını (.wav dosyası) indirebilir ve kalite için ikincisini daha fazla değerlendirebilirsiniz.

Test sonuçlarını, test için seçtiğiniz her modelin ayrıntı sayfasında da bulabilirsiniz. **Eğitim** sekmesine gidin ve model ayrıntı sayfasını girmek için model adını tıklatın.

## <a name="create-and-use-a-custom-voice-endpoint"></a>Özel bir ses bitiş noktası oluşturma ve kullanma

Ses modelinizi başarıyla oluşturduktan ve test ettikten sonra, bu modeli özel bir Metin-Konuşma bitiş noktasına dağımış olursunuz. Daha sonra, REST API'de Metinden Konuşmaya isteklerini yaparken bu bitiş noktasını normal bitiş noktasının yerine kullanırsınız. Özel bitiş noktanız yalnızca yazı tipini dağıtmak için kullandığınız abonelik tarafından çağrılabilir.

Yeni bir özel ses bitiş noktası oluşturmak için **Metinden Konuşmaya > Özel Ses > Dağıtım'a**gidin. **Bitiş Noktası Ekle'yi** seçin ve özel bitiş noktanız için bir **Ad** ve **Açıklama** girin. Ardından, bu bitiş noktasıyla ilişkilendirmek istediğiniz özel ses modelini seçin.

**Ekle** düğmesini tıklattıktan sonra, bitiş noktası tablosunda, yeni bitiş noktanız için bir giriş görürsünüz. Yeni bir bitiş noktasının anlık olarak anını vermek birkaç dakika sürebilir. Dağıtım durumu **Başarılı**olduğunda, bitiş noktası kullanıma hazırdır.

> [!NOTE]
> Ücretsiz abonelik (F0) kullanıcıları yalnızca bir model dağıtılabilir. Standart abonelik (S0) kullanıcıları, her biri kendi özel sesiyle 50 uç nokta oluşturabilir.

> [!NOTE]
> Özel sesinizi kullanmak için ses modeli adını belirtmeniz, özel URI'yi doğrudan bir HTTP isteğinde kullanmanız ve TTS hizmetinin kimlik doğrulamasına geçmek için aynı aboneliği kullanmanız gerekir.

Bitiş noktanız dağıtıldıktan sonra, bitiş noktası adı bağlantı olarak görünür. Uç nokta tuşu, uç nokta URL'si ve örnek kod gibi uç noktanıza özgü bilgileri görüntülemek için bağlantıyı tıklatın.

Bitiş noktasının çevrimiçi testi ne özel ses portalı aracılığıyla da kullanılabilir. Bitiş noktanızı test etmek **için, Bitiş Noktası ayrıntı** sayfasından bitiş noktasını **kontrol** et'i seçin. Bitiş noktası test sayfası görüntülenir. Konuşulacak metni girin (metin kutusuna düz metin veya [SSML biçiminde.](speech-synthesis-markup.md) Özel ses yazı tipinizde konuşulan metni duymak için **Oynat'ı**seçin. Bu test özelliği, özel konuşma sentezi kullanımınıza karşı ücretlendirilir.

Özel bitiş noktası, metinden konuşmaya istekleri için kullanılan standart uç noktayla işlevsel olarak aynıdır. Daha fazla bilgi için [REST API'ye](rest-text-to-speech.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Kılavuz: Ses örneklerinizi kaydetme](record-custom-voice-samples.md)
* [Metinden Konuşmaya API başvurusu](rest-text-to-speech.md)
* [Uzun Ses API'si](long-audio-api.md)
