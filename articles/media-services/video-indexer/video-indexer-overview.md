---
title: Video Indexer nedir?
titleSuffix: Azure Media Services
description: Bu makalede, Azure Medya Hizmetleri Video Dizinleyici hizmetine genel bir bakış sunulmaktadır.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 04/12/2020
ms.author: juliako
ms.openlocfilehash: 9bea9e375e00d6fc0f6c33aa1bcc766773db2059
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81269950"
---
# <a name="what-is-video-indexer"></a>Video Indexer nedir?

Video Indexer (VI), Azure Medya Hizmetleri AI çözümüdür ve Azure Bilişsel Hizmetler markasının bir parçasıdır. Video Indexer birden fazla kanal (ses, vokal, görsel) dayalı makine öğrenme modelleri kullanarak derin anlayışlar (veri analizi veya kodlama becerileri gerek kalmadan) ayıklamak için yeteneği sağlar. Modelleri daha da özelleştirebilir ve eğitebilirsiniz. Bu hizmet, derin arama sağlar, işletme maliyetlerini azaltır, yeni para kazanma fırsatları sağlar ve büyük video arşivlerinde (düşük giriş engelleri yle) yeni kullanıcı deneyimleri oluşturur.

Video Indexer ile öngörüler ayıklamaya başlamak için bir hesap oluşturmanız ve video yüklemeniz gerekir. Video Indexer'a videolarınızı yüklediğinizde, farklı AI modelleri çalıştırarak hem görselliği hem de sesi analiz eder. Video Indexer videonuzu analiz ettikçe, AI modelleri tarafından çıkarılan öngörüler.

Aşağıdaki diyagram, Video Indexer'ın arka uçta nasıl çalıştığına ilişkin teknik bir açıklama değil, bir çizimdir.

![Azure Medya Hizmetleri Video Dizinleyici akış diyagramı](./media/video-indexer-overview/model-chart.png)


## <a name="compliance-privacy-and-security"></a>Uyumluluk, Gizlilik ve Güvenlik

Önemli bir hatırlatma olarak, Video Dizinleyici'yi kullanımınızda geçerli tüm yasalara uymanız gerekir ve Video Dizinleyici'yi veya herhangi bir Azure hizmetini başkalarının haklarını ihlal eden veya başkaları için zararlı olabilecek bir şekilde kullanamazsınız.

Video Dizinleyici'ye herhangi bir video/resim yüklemeden önce, video/görüntüyü kullanmadan önce, yasaların gerektirdiği durumlarda, video/görüntüdeki bireylerin (varsa) verilerinin Video Dizinleyici ve Azure'da kullanımı, işlenmesi ve depolanması için gerekli tüm izinler de dahil olmak üzere, videoyu/görüntüyü kullanmak için gereken tüm haklara sahip olmalısınız. Bazı yargı bölgeleri, biyometrik veriler gibi belirli veri kategorilerinin toplanması, çevrimiçi işlenmesi ve depolanması için özel yasal gereklilikler uygulayabilir. Özel yasal gerekliliklere tabi tüm verilerin işlenmesi ve saklanması için Video Indexer ve Azure'u kullanmadan önce, sizin için geçerli olabilecek bu tür yasal gerekliliklere uygunluğu sağlamalısınız.

Video Dizinleyici'de uyumluluk, gizlilik ve güvenlik hakkında bilgi edinmek için lütfen Microsoft [Güven Merkezi'ni](https://www.microsoft.com/TrustCenter/CloudServices/Azure/default.aspx)ziyaret edin. Microsoft'un gizlilik yükümlülükleri, verilerinizin nasıl silindigini içeren veri işleme ve saklama uygulamaları için lütfen Microsoft' un [Gizlilik Bildirimini,](https://privacy.microsoft.com/PrivacyStatement) [Çevrimiçi Hizmetler Koşulları'nı](https://www.microsoft.com/licensing/product-licensing/products?rtc=1) ("OST") ve [Veri İşleme Eki 'ni](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) ("DPA") inceleyin. Video Indexer'ı kullanarak, OST, DPA ve Gizlilik Bildirimi'ne bağlı olmayı kabul edeyim.

## <a name="what-can-i-do-with-video-indexer"></a>Video Indexer ile ne yapabilirim?

Video Indexer'ın öngörüleri birçok senaryoya uygulanabilir:

* *Derin arama*: Video kitaplığındaki arama deneyimini geliştirmek için videodan çıkarılan bilgileri kullanın. Örneğin, konuşulan sözcükleri ve yüzleri dizine ekleyerek, bir kişinin belirli sözcükleri konuştuğu veya iki kişinin birlikte görüldüğü bir videoda anları bulma nın arama deneyimini etkinleştirebilirsiniz. Videolardan elde edilen bu tür öngörülere dayalı arama, haber ajansları, eğitim enstitüleri, yayıncılar, eğlence içerik sahipleri, kurumsal LOB uygulamaları ve genel olarak kullanıcıların karşı araması gereken bir video kitaplığı olan tüm sektör için geçerlidir.
* *İçerik oluşturma*: Video Indexer'ın içeriğinizden çıkardığı kavrayışlara dayalı olarak fragmanlar oluşturun, makaraları, sosyal medya içeriğini veya haber kliplerini vurgulayın. Anahtar kareler, sahne işaretçileri ve kişiler ve etiket görünümleri için zaman damgaları oluşturma işlemini çok daha düzgün ve kolay hale getirir ve oluşturduğunuz içerik için videonun gereken bölümlerine ulaşabilmenizi sağlar.
* *Erişilebilirlik*: İçeriğinizi engelli kişiler için kullanılabilir hale getirmek veya içeriğinizin farklı diller kullanarak farklı bölgelere dağıtılmasını istiyorsanız, video dizinleyici tarafından sağlanan transkripsiyon ve çeviriyi birden çok dilde kullanabilirsiniz.
* *Para kazanma*: Video Indexer videoların değerini artırmaya yardımcı olabilir. Örneğin, reklam gelirine (haber medyası, sosyal medya vb.) güvenen sektörler, çıkarılan istatistikleri reklam sunucusuna ek sinyaller olarak kullanarak alakalı reklamlar sunabilir.
* *İçerik Moderasyonu*: Kullanıcılarınızın uygunsuz içerikten korunmasını sağlamak ve yayınladığınız içeriğin kuruluşunuzun değerleriyle eşleştiğini doğrulamak için metinsel ve görsel içerik moderasyon modellerini kullanın. Belirli videoları otomatik olarak engelleyebilir veya kullanıcılarınızı içerik hakkında uyarabilirsiniz.
* *Öneriler*: Video istatistikleri, kullanıcılara alakalı video anlarını vurgulayarak kullanıcı etkileşimini geliştirmek için kullanılabilir. Her videoyu ek meta verilerle etiketleyerek, kullanıcılara en alakalı videoları önerebilir ve videonun gereksinimlerini karşılayacak bölümlerini vurgulayabilirsiniz.

## <a name="features"></a>Özellikler

Aşağıdaki liste, Video Indexer video ve ses modellerini kullanarak videolarınızdan alabileceğiniz öngörüleri gösterir:

### <a name="video-insights"></a>Video istatistikleri

* **Yüz algılama**: Videoda görünen yüzleri algılar ve gruplandırır.
* **Ünlü kimliği**: Video Indexer, dünya liderleri, aktörler, aktrisler, sporcular, araştırmacılar, iş dünyası ve teknoloji liderleri gibi 1 milyondan fazla ünlüyü otomatik olarak tanımlar. Bu ünlüler hakkında veriler de çeşitli web sitelerinde bulunabilir (IMDB, Vikipedi, vb).
* **Hesap tabanlı yüz tanıma**: Video Indexer belirli bir hesap için bir model eğitir. Daha sonra videodaki yüzleri eğitimli modele göre tanır. Daha fazla bilgi için, [Video Indexer web sitesinden bir Kişi modelini özelleştir](customize-person-model-with-website.md) ve [Video Dizinleyici API ile Bir Kişi modelini özelleştirin.](customize-person-model-with-api.md)
* **Yüzler için küçük resim ekstraksiyonu** ("en iyi yüz"): Her yüz grubunda yakalanan en iyi yüzü otomatik olarak tanımlar (kalite, boyut ve ön konuma göre) ve bir görüntü varlığı olarak ayıklar.
* **Görsel metin tanıma** (OCR): Videoda görsel olarak görüntülenen metni ayıklar.
* **Görsel içerik moderasyonu**: Yetişkinlere yönelik ve/veya müstehcen görselleri algılar.
* **Etiket belirleme**: Görüntülenen görsel nesneleri ve eylemleri belirler.
* **Sahne segmentasyonu**: Görsel ipuçlarına göre videodaki bir sahnenin ne zaman değiştiğini belirler. Bir sahne tek bir olayı betimler ve bir dizi ardışık çekimden oluşur, bunlar anlamsal olarak ilişkilidir.
* **Çekim algılama**: Görsel ipuçlarına göre videoda bir çekimin ne zaman değiştiğini belirler. Çekim, aynı sinema kamerasından alınan bir dizi karedir. Daha fazla bilgi için [Sahneler, çekimler ve anahtar kareler](scenes-shots-keyframes.md)hakkında bilgi alabiliyorum.
* **Siyah kare algılama**: Videoda yer alan siyah kareleri belirler.
* **Ana kare ayıklama**: Videodaki kararlı ana kareleri algılar.
* **Rolling credits**: TV şovları ve filmlerin sonunda ki haddeleme jeneriğinin başlangıcını ve sonunu tanımlar.
* **Animasyonlu karakter algılama** (önizleme): Algılama, gruplandırma ve Bilişsel Hizmetler [özel vizyonu](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/)ile entegrasyon yoluyla animasyonlu içerik karakterlerin tanınması. Daha fazla bilgi için [Animasyonkarakter algılama'ya](animated-characters-recognition.md)bakın.
* **Editoryal çekim türü algılama**: Kendi türüne göre etiketleme çekimleri (geniş çekim, orta çekim, yakın çekim, aşırı yakın çekim, iki atış, birden fazla kişi, açık ve kapalı vb. gibi). Daha fazla bilgi [için, Editoryal çekim türü algılama](scenes-shots-keyframes.md#editorial-shot-type-detection)bakın.

### <a name="audio-insights"></a>Ses öngörüleri

* **Ses transkripsiyonu**: Konuşmayı 12 dilde metne dönüştürür ve uzantılara izin verir. Desteklenen diller Arasında İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Mandarin Chines, Japonca, Arapça, Rusça, Brezilya Portekizcesi, Hintçe ve Korece bulunmaktadır.
* **Otomatik dil algılama**: En baskın olarak konuşulan dili otomatik olarak belirler. Desteklenen diller Arasında İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Mandarin Chines, Japonca, Rusça ve Brezilya Portekizcesi bulunmaktadır. Dil güvenle tanımlanamazsa, Video Indexer konuşulan dilin İngilizce olduğunu varsayar. Daha fazla bilgi için [Dil tanımlama modeline](language-identification-model.md)bakın.
* **Çok dilli konuşma tanımlama ve transkripsiyon** (önizleme): Sesten farklı segmentlerde konuşulan dili otomatik olarak tanımlar. Medya dosyasının her bir bölümünü yazıya geçirilmeye gönderir ve transkripsiyonla tek bir leştirilmiş transkripsiyonla birleştirir. Daha fazla bilgi için, [çoklu dil içeriğini otomatik olarak tanımlayın ve yazıya döktü.](multi-language-identification-transcription.md)
* **Açıklamalı altyazı**: Üç biçimde açıklamalı altyazı oluşturur: VTT, TTML, SRT.
* **İki kanal işleme**: Otomatik ayrı transkript algılar ve tek zaman çizelgesi ile birleştirir.
* **Gürültü azaltma**: Telefon ses veya gürültülü kayıtları temizler (Skype filtrelerine dayalı).
* **Transkript özelleştirme** (CRIS): Sektöre özel transkriptler oluşturmak için metin modellerine özel konuşma trenler. Daha fazla bilgi [için, Video Indexer web sitesinden bir Dil modelini özelleştir](customize-language-model-with-website.md) ve [Video Dizinleyici API'leriyle bir Dil modelini özelleştirin.](customize-language-model-with-api.md)
* **Hoparlör numaralandırma**: Hangi konuşmacının hangi kelimeleri ve ne zaman konuştuğunu haritalar ve anlar.
* **Konuşmacı istatistikleri**: Konuşmacıların konuşma oranları için istatistikler sağlar.
* **Metinsel içerik moderasyonu**: Ses transkriptlerindeki müstehcen metinleri algılar.
* **Ses efektleri**: El çırpma, konuşma ve sessizlik gibi ses efektlerini tanımlar.
* **Duygu algılama**: Konuşmaları (söylenenlere) ve ses tonalitesine (nasıl söylenir) dayalı duyguları tanımlar. Duygu sevinç, üzüntü, öfke, ya da korku olabilir.
* **Çeviri**: Ses transkriptinin 54 farklı dile çevirisini oluşturur.

### <a name="audio-and-video-insights-multi-channels"></a>Ses ve video öngörüleri (çok kanallı)

Bir kanal tarafından dizine eksinre, bu modeller için kısmi sonuç kullanılabilir olacaktır.

* **Anahtar kelimeler çıkarma**: Konuşma ve görsel metinden anahtar kelimeleri ayıklar.
* **Adlandırılmış varlıklar çıkarma**: Doğal dil işleme (NLP) aracılığıyla konuşmaları ve görsel metinlerden markaları, konumları ve kişileri ayıklar.
* **Konu çıkarımı**: Transkriptlerdeki ana konuların çıkarımını yapar. 2. düzey IPTC taksonomi dahildir.
* **Yapıtlar**: Modellerin her biri için "daha üst düzeyde ayrıntıya sahip" zengin bir yapıt kümesini ayıklar.
* **Yaklaşım analizi**: Konuşmalardaki ve görsel metinlerdeki olumlu, olumsuz ve nötr yaklaşımları belirler.

## <a name="how-can-i-get-started-with-video-indexer"></a>Video Indexer ile nasıl başlarım?

Video Dizinleyici özelliklerine üç şekilde erişebilirsiniz:

* Video Indexer portalı: Ürünü değerlendirmenize, hesabı yönetmenize ve modelleri özelleştirmenize olanak tanıyan kullanımı kolay bir çözümdür.

    Portal hakkında daha fazla bilgi için Bkz. [Video Indexer web sitesi ile başlayın.](video-indexer-get-started.md)  

* API entegrasyonu: Video Indexer'ın tüm yetenekleri, çözümü uygulamalarınız ve altyapınıza entegre etmenizi sağlayan BIR REST API'si aracılığıyla kullanılabilir.

    Geliştirici olarak başlamak için [Video Indexer REST API'yi kullanın'a](video-indexer-use-apis.md)bakın.

* Katıştırılabilir widget: Video Indexer istatistiklerini, oynatıcıyı ve düzenleyici deneyimlerini uygulamanıza gömmenizi sağlar.

    Daha fazla bilgi için [başvurunuzdaki Embed görsel widget'ları'na](video-indexer-embed-widgets.md)bakın.

Web sitesini kullanıyorsanız, öngörüler meta veri olarak eklenir ve portalda görünür. API'ler kullanıyorsanız, öngörüler JSON dosyası olarak kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Video Indexer hizmetini kullanmaya başlamaya hazırsınız. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Video Indexer web sitesi ile başlayın.](video-indexer-get-started.md)
- [Video Indexer REST API ile işleme içeriği.](video-indexer-use-apis.md)
- [Uygulamanıza görsel widget'lar gömün.](video-indexer-embed-widgets.md)
