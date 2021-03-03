---
title: Özel bir sesli konuşma hizmeti oluşturma
titleSuffix: Azure Cognitive Services
description: Verilerinizi karşıya yüklemeye hazırsanız, özel sesli portala gidin. Özel bir ses projesi oluşturun veya seçin. Projenin, doğru dil/yerel ayar ve cinsiyet özelliklerini, ses eğitiminizi için kullanmayı düşündüğünüz verilerle paylaşmalıdır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: 541448f08e4ce9961d34063dcc225bf89d969a73
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101703380"
---
# <a name="create-a-custom-voice"></a>Özel bir ses oluşturun

[Özel ses için verileri hazırlama](how-to-custom-voice-prepare-data.md)bölümünde, özel bir sesi ve farklı biçim gereksinimlerini eğitebilmeniz için kullanabileceğiniz farklı veri türlerini açıklıyoruz. Verilerinizi hazırladıktan sonra, bunları [özel ses portalına](https://aka.ms/custom-voice-portal)veya özel ses eğitimi API 'sine yüklemeye başlayabilirsiniz. Burada, Portal üzerinden özel bir sesli eğitim adımlarını açıklanmaktadır.

> [!NOTE]
> Bu sayfada özel [sesle çalışmaya](how-to-custom-voice.md) başlayın ve özel [Ses için verileri hazırlayın](how-to-custom-voice-prepare-data.md)ve özel bir ses projesi oluşturmuş olursunuz.

Özel ses için desteklenen dilleri denetleyin: [Özelleştirme dili](language-support.md#customization).

## <a name="upload-your-datasets"></a>Veri kümelerinizi karşıya yükleyin

Verilerinizi karşıya yüklemeye hazırsanız, [özel sesli portala](https://aka.ms/custom-voice-portal)gidin. Özel bir ses projesi oluşturun veya seçin. Projenin, doğru dil/yerel ayar ve cinsiyet özelliklerini, ses eğitiminizi için kullanmayı düşündüğünüz verilerle paylaşmalıdır. Örneğin, `en-GB` BIR UK vurgusu Ile İngilizce 'de ses kayıtlarının gerçekleştirilip yapıllamadığını seçin.

**Veri** sekmesine gidin ve **verileri karşıya yükle**' ye tıklayın. Sihirbazda, hazırladığınız verilerle eşleşen doğru veri türünü seçin.

Karşıya yüklediğiniz her veri kümesinin seçtiğiniz veri türü için gereksinimleri karşılaması gerekir. Karşıya yüklenmeden önce verilerinizi doğru şekilde biçimlendirmek önemlidir. Bu, verilerin özel ses hizmeti tarafından doğru şekilde işlenmesini sağlar. [Özel ses için veri hazırlama](how-to-custom-voice-prepare-data.md) sayfasına gidin ve verilerinizin rightly biçimlendirildiğinden emin olun.

> [!NOTE]
> Ücretsiz abonelik (F0) kullanıcıları iki veri kümesini eşzamanlı olarak karşıya yükleyebilir. Standart abonelik (S0) kullanıcıları beş veri kümesini eşzamanlı olarak karşıya yükleyebilir. Sınıra ulaştıysanız, veri kümelerinden en az biri içeri aktarmayı bitirene kadar bekleyin. Sonra yeniden deneyin.

> [!NOTE]
> Abonelik başına içeri aktarılmaya izin verilen en fazla veri kümesi sayısı 10. ücretsiz abonelik (F0) kullanıcıları ve Standart abonelik (S0) kullanıcıları için 500. zip dosyalarıdır.

Veri kümeleri, karşıya yükle düğmesine ulaştıktan sonra otomatik olarak onaylanır. Veri doğrulama, dosya biçimlerini, boyutunu ve örnekleme oranını doğrulamak için ses dosyalarındaki denetim dizilerini içerir. Varsa hataları düzeltin ve yeniden gönder. Veri alma isteği başarıyla başlatıldığında, yeni karşıya yüklediğiniz veri kümesine karşılık gelen veri tablosunda bir giriş görmeniz gerekir.

Aşağıdaki tabloda, içeri aktarılan veri kümeleri için işleme durumları gösterilmektedir:

| Durum | Anlamı |
| ----- | ------- |
| İşleniyor | Veri kümeniz alındı ve işleniyor. |
| Başarılı | Veri kümeniz doğrulandıktan sonra artık bir ses modeli oluşturmak için kullanılabilir. |
| Başarısız | Veri kümeniz, çok sayıda nedenden dolayı işleme sırasında başarısız oldu, örneğin dosya hataları, veri sorunları veya ağ sorunları. |

Doğrulama işlemi tamamlandıktan sonra, **utterslar** sütunundaki veri kümelerinizin her biri için eşleşen Masların toplam sayısını görebilirsiniz. Seçtiğiniz veri türü uzun ses segmentlemesini gerektiriyorsa, bu sütun yalnızca kendi betiklerinizi temel alarak veya konuşma dökümü hizmeti aracılığıyla sizin için segmentettiğimiz göz asyonları yansıtır. Başarılı bir şekilde içeri aktarılan ve bunların eşleme dökümlerinin ayrıntı sonuçlarını görüntülemek için doğrulanan veri kümesini daha fazla indirebilirsiniz. İpucu: uzun ses segmentlenmesi, veri işlemeyi tamamlayacak bir saatten uzun sürebilir.

Veri Ayrıntıları görünümünde, veri kümelerinizin her biri için telaffuz ve gürültü düzeyini daha fazla kontrol edebilirsiniz. Söyleniş puanı 0 ile 100 arasındadır. 70 altındaki bir puan, normalde bir konuşma hatası veya betik uyuşmazlığını gösterir. Ağır bir vurgu, telaffuz puanınızı azaltabilir ve üretilen dijital sesi etkileyebilir.

Daha yüksek sinyalden gürültü oranı (SNR), Sesinizdeki küçük paraziti gösterir. Profesyonel Studios adresinde kayıt yaparak genellikle 50 + SNR 'ye ulaşabilirsiniz. 20 ' nin altında bir SNR ile ses, üretilen sesinize açık gürültü oluşmasına neden olabilir.

Düşük telaffuz puanlarla veya kötü bir sinyal gürültüsü oranlarına sahip tüm sinyalleri yeniden kaydetmeyi düşünün. Yeniden kaydedemeyebilirsiniz, bu ilgili söz konusu söz konusu söz konusu söz konusu söz konusu söz konusu söz konusu söz konusu

> [!NOTE]
> Özel sinir Voice kullanıyorsanız, ses tatatçinizi **Ses tatatmi** sekmesine kaydetmeniz gerekir. Kayıt betiğinizi hazırlarken, bir TTS ses modeli oluşturmak ve yapay konuşma oluşturmak için ses verilerini kullanmaya yönelik ses tatatsyon alındı bildirimini edinmek üzere aşağıdaki cümleyi eklediğinizden emin olun. "I [soyadınız ve soyadınız], sesimin yapay bir sürümünü oluşturmak ve kullanmak için sesimin kayıtlarının [Şirket adı olarak durum] tarafından kullanılacağını biliyor."
Bu cümle, eğitim veri kümelerinizde bulunan kayıtların onay yapan kişi tarafından gerçekleştirilip yapılkullanılmadığını doğrulamak için kullanılacaktır. [Verilerinizin nasıl işleneceği hakkında daha fazla bilgi edinmek ve ses tatatçinizi doğrulamanın burada nasıl yapılacağını](/legal/cognitive-services/speech-service/custom-neural-voice/data-privacy-security-custom-neural-voice?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext)öğrenin. 

## <a name="build-your-custom-voice-model"></a>Özel ses modelinizi oluşturma

Veri kümeniz doğrulandıktan sonra, özel ses modelinizi oluşturmak için kullanabilirsiniz.

1.  **Metin okuma > özel ses > [proje adı] > modeli**' ne gidin.

2.  **Modeli eğitme**' ye tıklayın.

3.  Ardından, bu modeli tanımlamanızı sağlayacak bir **ad** ve **Açıklama** girin.

    Bir adı dikkatle seçin. Buraya girdiğiniz ad, SSML girişinin bir parçası olarak konuşma sensimi için isteğiniz için kullandığınız adı kullanacaktır. Yalnızca harfler, rakamlar ve-, \_ , ve (', ') gibi birkaç noktalama karakteri kullanılabilir. Farklı ses modelleri için farklı adlar kullanın.

    **Description** alanının yaygın bir kullanımı, modeli oluşturmak için kullanılan veri kümelerinin adlarını kaydetmek olur.

4.  **Eğitim verilerini seçin** sayfasında, eğitim için kullanmak istediğiniz bir veya birden çok veri kümesi seçin. Göndermeden önce vurdıklarla sayısını denetleyin. "Uyarlamalı" eğitim yöntemini kullanarak, en-US ve zh-CN ses modelleriyle ilgili herhangi bir sayıda metinle başlayabilirsiniz. Diğer yerel ayarlarda, "Istatistiksel parametrik" ve "Concatenative" eğitim yöntemleri gibi standart bir katman kullanarak bir sesi eğitebilmeniz için 2.000 ' den fazla yer seçmeniz ve özel bir sinir sesini eğitme 300 ' den fazla yer seçmeniz gerekir. 

    > [!NOTE]
    > Yinelenen ses adları eğitiminden kaldırılacak. Seçtiğiniz veri kümelerinin birden çok. zip dosyası arasında aynı ses adlarını içermediğinden emin olun.

    > [!TIP]
    > Kalite sonuçları için aynı konuşmacının veri kümelerini kullanmak gereklidir. Farklı eğitim yöntemleri farklı eğitim verileri boyutu gerektirir. "Istatistiksel parametrik" yöntemiyle bir modeli eğitebilmeniz için en az 2.000 farklı uttersliği gereklidir. "Concatenative" yönteminde, 6.000 Bu, "sinir" için en düşük veri boyutu gereksinim300 idir.

5. Sonraki adımda **eğitim yöntemini** seçin. 

    > [!NOTE]
    > Bir sinir sesi eğmek isterseniz, özel bir ses modeli eğmek için kendi konuşma verilerini kullanma hakkında daha fazla ses onay dosyası içeren bir ses tatatsyon profili belirtmeniz gerekir. Özel sinir Voice sınırlı erişimle kullanılabilir. [Sorumlu AI gereksinimlerini](/legal/cognitive-services/speech-service/custom-neural-voice/limited-access-custom-neural-voice?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext) anladığınızdan emin olun ve [erişimi buraya uygulayın](https://aka.ms/customneural). 
    
    Bu sayfada, komut dosyanızı test için karşıya yüklemeyi de seçebilirsiniz. Sınama betiği, 1 MB 'tan küçük bir txt dosyası olmalıdır. Desteklenen kodlama biçimi ANSI/ASCII, UTF-8, UTF-8-BOM, UTF-16-LE veya UTF-16-of içerir. Söylenişi 'in her paragrafında ayrı bir ses elde edilir. Tüm cümleleri tek bir ses halinde birleştirmek istiyorsanız, bunları tek bir paragrafta yapın. 

6. Ses modelinizi oluşturmaya başlamak için **eğitme** ' ye tıklayın.

Eğitim tablosu, bu yeni oluşturulan modele karşılık gelen yeni bir giriş görüntüler. Tabloda Ayrıca durum görüntülenir: Işlem, başarılı, başarısız.

Gösterilen durum, burada gösterildiği gibi, veri kümenizi bir ses modeline dönüştürme sürecini yansıtır.

| Durum | Anlamı |
| ----- | ------- |
| İşleniyor | Ses modeliniz oluşturuluyor. |
| Başarılı | Ses modeliniz oluşturuldu ve dağıtılabilir. |
| Başarısız | Ses modeliniz birçok nedenden dolayı eğitiminde başarısız oldu, örneğin görülmeyen veri sorunları veya ağ sorunları. |

Eğitim süresi, işlenen ses verilerinin hacmine ve seçtiğiniz eğitim yöntemine bağlı olarak farklılık gösterir. 30 dakika ila 40 saat arasında değişebilir. Model eğitiminizi başarılı olduktan sonra test edebilirsiniz. 

> [!NOTE]
> Ücretsiz abonelik (F0) kullanıcıları aynı anda bir ses yazı tipi eğitebilir. Standart abonelik (S0) kullanıcıları aynı anda üç ses de eğitebilir. Sınıra ulaştıysanız, en az bir ses yazı tipinden eğitim bitene kadar bekleyip yeniden deneyin.

> [!NOTE]
> Özel sinir seslerine yönelik eğitim ücretsiz değildir. [Fiyatları](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) burada kontrol edin. 

> [!NOTE]
> Abonelik başına eğitime izin verilen maksimum ses modeli sayısı, ücretsiz abonelik (F0) kullanıcıları için 10 modeldir ve Standart abonelik (S0) kullanıcıları için 100.

Sinir Voice eğitim özelliğini kullanıyorsanız, gerçek zamanlı akış senaryoları için en iyi duruma getirilmiş bir modeli eğitme veya zaman uyumsuz [Long-Audio birleştirmede](long-audio-api.md)en iyi duruma GETIRILMIŞ bir HD sinir modeli seçebilirsiniz.  

## <a name="test-your-voice-model"></a>Ses modelinizi test etme

Her eğitim, modeli test etmenize yardımcı olmak için otomatik olarak 100 örnek ses dosyası oluşturacaktır. Ses modeliniz başarıyla derlendikten sonra, kullanım için dağıtım yapmadan önce test edebilirsiniz.

1.  **Metin okuma > özel ses > [proje adı] > modeli**' ne gidin.

2.  Test etmek istediğiniz modelin adına tıklayın.

3.  Model ayrıntısı sayfasında, örnek ses dosyalarını **Test** sekmesi altında bulabilirsiniz. 

Sesin kalitesi, eğitim verilerinin boyutu, kayıt kalitesi, döküm dosyasının doğruluğu, eğitim verilerinde kaydedilen sesin amaçlanan kullanım durumu için tasarlanan sesin kişiliğine ne kadar iyi eşleştiğini ve daha fazlasını içeren bir dizi etkene bağlıdır. [Teknolojimizin özellikleri ve limitleri hakkında daha fazla bilgi edinmek ve model kalitesini geliştirmek için en iyi uygulama hakkında daha fazla bilgi için buraya bakın](/legal/cognitive-services/speech-service/custom-neural-voice/characteristics-and-limitations-custom-neural-voice?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext). 

## <a name="create-and-use-a-custom-voice-endpoint"></a>Özel bir ses uç noktası oluşturma ve kullanma

Ses modelinizi başarıyla oluşturup test ettikten sonra, özel bir metin okuma uç noktasına dağıtırsınız. Sonra bu uç noktayı, REST API aracılığıyla metinden konuşmaya istekleri yaparken her zamanki uç noktanın yerine kullanırsınız. Özel uç noktanız yalnızca yazı tipini dağıtmak için kullandığınız abonelik tarafından çağrılabilir.

Yeni bir özel ses uç noktası oluşturmak için, **metinden konuşmaya > özel sesli > uç** noktasına gidin. **Uç nokta Ekle** ' yi seçin ve özel uç noktanız Için bir **ad** ve **Açıklama** girin. Ardından, bu uç nokta ile ilişkilendirmek istediğiniz özel ses modelini seçin.

**Ekle** düğmesine tıkladıktan sonra, uç nokta tablosunda, yeni uç noktanız için bir giriş görürsünüz. Yeni bir uç noktanın örneklendirilecek birkaç dakika sürebilir. Dağıtımın durumu **başarılı** olduğunda, uç nokta kullanıma hazırdır.

Her zaman kullanmıyorsanız, uç noktanızı **askıya** alabilir ve **sürdürebilirsiniz** . Bir uç nokta askıya alındıktan sonra yeniden etkinleştirildiğinde, uygulamalarınızda kodunuzun değiştirilmesini istemediğiniz için uç nokta URL 'SI aynı kalır. 

Uç noktasını yeni bir modele de güncelleştirebilirsiniz. Modeli değiştirmek için, yeni modelin güncelleştirmek istediğiniz adla aynı olduğundan emin olun. 

> [!NOTE]
> Ücretsiz abonelik (F0) kullanıcılarının yalnızca bir modeli dağıtılmış olabilir. Standart abonelik (S0) kullanıcıları, her biri kendi özel sesiyle en fazla 50 uç nokta oluşturabilir.

> [!NOTE]
> Özel sesinizi kullanmak için, ses modeli adını belirtmeniz, özel URI 'yi doğrudan bir HTTP isteğinde kullanmanız ve aynı aboneliği kullanarak TTS hizmetinin kimlik doğrulamasından geçmesi gerekir.

Uç noktanız dağıtıldıktan sonra, uç nokta adı bir bağlantı olarak görünür. Uç nokta anahtarı, uç nokta URL 'SI ve örnek kod gibi uç noktanıza özgü bilgileri göstermek için bağlantıya tıklayın.

Uç noktanın çevrimiçi testi, özel ses portalı aracılığıyla da kullanılabilir. Uç noktanızı test etmek için **uç nokta ayrıntısı** sayfasından **uç noktayı denetle** ' yi seçin. Uç nokta testi sayfası görüntülenir. Söylenen metni (metin kutusunda düz metin veya [SSML biçiminde](speech-synthesis-markup.md) ) girin. Özel ses yazı tipinde konuşulan metni dinlemek için **Yürüt**' ü seçin. Bu test özelliği, özel konuşma sensıs kullanımınıza göre ücretlendirilir.

Özel uç nokta, metinden konuşmaya istekleri için kullanılan standart uç nokta ile aynıdır. Daha fazla bilgi için bkz. [REST API](rest-text-to-speech.md) .

## <a name="next-steps"></a>Sonraki adımlar

* [Kılavuz: ses örneklerinizi kaydetme](record-custom-voice-samples.md)
* [Metinden konuşmaya API başvurusu](rest-text-to-speech.md)
* [Uzun ses API 'SI](long-audio-api.md)