---
title: Video Indexer nedir?
titleSuffix: Azure Media Services
description: Bu makale, Azure Media Services Video Indexer hizmetine genel bir bakış sunar.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 02/02/2020
ms.author: juliako
ms.openlocfilehash: efd8386f464bfdf2ac27d3be07e6572dc27952e1
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78163599"
---
# <a name="what-is-video-indexer"></a>Video Indexer nedir?

Video Indexer (VI), Azure Media Services AI çözümüdür ve Azure bilişsel hizmetler markasının bir parçasıdır. Video Indexer, birden fazla kanala (ses, Vocal, görsel) göre makine öğrenimi modellerini kullanarak derin içgörüler (veri çözümleme veya kodlama becerileri gerektirmez) ayıklayabilme olanağı sağlar. Modelleri daha fazla özelleştirebilir ve eğitebilirsiniz. Hizmet derin arama imkanı sunar, işlem maliyetlerini azaltır, yeni para kullanımı fırsatlarını azaltır ve büyük video arşivleri üzerinde yeni kullanıcı deneyimleri oluşturur (düşük girişli engelleri olan).

Video Indexer öngörüleri çıkarmaya başlamak için bir hesap oluşturmanız ve videoları yüklemeniz gerekir. Videolarınızı Video Indexer yüklediğinizde, farklı AI modellerini çalıştırarak hem görselleri hem de sesi analiz edersiniz. Video Indexer, video, AI modelleri tarafından ayıklanan Öngörüler ile analiz edilir.

Aşağıdaki diyagram, Video Indexer arka uçta nasıl çalıştığına ilişkin teknik bir açıklama değildir.

![Azure Media Services Video Indexer akış diyagramı](./media/video-indexer-overview/model-chart.png)


## <a name="compliance-privacy-and-security"></a>Uyumluluk, Gizlilik ve Güvenlik

Önemli bir anımsatıcı olarak, Video Indexer kullanmanız için geçerli olan yasaların tümüne uymanız gerekir ve Video Indexer veya herhangi bir Azure hizmetini diğerlerinin haklarını ihlal eden veya başkalarına zararlı olabilecek bir biçimde kullanamazsınız.

Video Indexer herhangi bir videoyu/görüntüyü karşıya yüklemeden önce, video/görüntüdeki tüm gerekli haklara (varsa), verilerin Video Indexer ve Azure 'da kullanım, işleme ve depolama için gerekli olduğu durumlar dahil olmak üzere videoyu/görüntüsünü kullanmak için uygun haklara sahip olmanız gerekir. Bazı daireler, koleksiyon için özel yasal gereksinimler (örneğin, biyometrik veriler gibi belirli veri kategorilerinin çevrimiçi olarak işlenmesi ve depolanması) uygulayabilir. Video Indexer ve Azure 'u özel yasal gereksinimlere yönelik herhangi bir veri konusunun işleme ve depolama alanı için kullanmadan önce, sizin için geçerli olabilecek her türlü meşru gereksinimlerle uyumluluğa sahip olduğunuzdan emin olmanız gerekir.

Video Indexer uyumluluk, gizlilik ve güvenlik hakkında bilgi edinmek için lütfen Microsoft [Güven Merkezi](https://www.microsoft.com/TrustCenter/CloudServices/Azure/default.aspx)' ni ziyaret edin. Microsoft 'un gizlilik yükümlülüklerinde, verilerinizi silme dahil olmak üzere veri işleme ve bekletme uygulamaları için lütfen Microsoft 'un [Gizlilik bildirimi](https://privacy.microsoft.com/PrivacyStatement), [çevrimiçi hizmet koşulları](https://www.microsoft.com/licensing/product-licensing/products?rtc=1) ("OST") ve [veri işleme eki](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) ("DPA") gözden geçirin. Video Indexer kullanarak, OST, DPA ve gizlilik bildirimiyle bağlanmayı kabul etmiş olursunuz.

## <a name="what-can-i-do-with-video-indexer"></a>Video Indexer ile ne yapabilirim?

Video Indexer içgörüler aşağıdakiler arasında birçok senaryoya uygulanabilir:

* *Derin arama*: video kitaplığı genelinde arama deneyimini geliştirmek için videodan ayıklanan öngörüleri kullanın. Örneğin, konuşulan sözcüklerin ve yüzlerin dizininin oluşturulması, bir kişinin belirli kelimeleri veya iki kişinin birlikte görüldüğü bir videoda dakika bulmayla ilgili arama deneyimini etkinleştirebilir. Videolardan bu tür Öngörüler temelinde arama, haber kurumları, eğitim kurum, yayımcılar, eğlence içerik sahipleri, kurumsal LOB uygulamaları ve genel olarak, kullanıcıların aramaları gereken bir video kitaplığı olan herhangi bir sektör için geçerlidir.
* *İçerik oluşturma*: İçeriğinizden çıkaran öngörülere göre, daha fazla bilgi video Indexer edinin, Re, sosyal medya içeriğini veya haberleri vurgulayın. Kişiler ve etiket görünümleri için ana kareler, sahneler işaretçileri ve zaman damgaları oluşturma işlemini çok daha yumuşak ve daha kolay hale getirir ve oluşturmakta olduğunuz içerik için ihtiyacınız olan videonun bölümlerine almanızı sağlar.
* *Erişilebilirlik*: içeriğinizi Engelli kişiler için kullanılabilir hale getirmek isteyip istemediğinizi veya içeriğinizi farklı diller kullanılarak farklı bölgelere dağıtmak istiyorsanız, video Indexer tarafından sağlanan döküm ve çeviriyi birden çok dilde kullanabilirsiniz.
* Paraya *getirme: video Indexer*videoların değerini artırmaya yardımcı olabilir. Örneğin, ad gelirini (haber medyası, sosyal medya vb.) kullanan sektörler, ayıklanan öngörüleri ad sunucusuna ek sinyaller olarak kullanarak, ilgili reklamları sunabilir.
* *İçerik denetleme*: kullanıcılarınızın uygunsuz içeriklerden güvenli kalmasını sağlamak ve yayımladığınız içeriğin kuruluşunuzun değerleriyle eşleştiğinden emin olmak için metin ve görsel içerik denetleme modellerini kullanın. Belirli videoları otomatik olarak bloke edebilir veya içeriğinizi içerik hakkında uyarır.
* *Öneriler*: video öngörüleri, kullanıcılara ilgili video dakikanızı ayırarak kullanıcı etkileşimini geliştirmek için kullanılabilir. Her videoyu ek meta verilerle etiketleyerek, kullanıcılara en uygun Videoları ve bunların ihtiyaçlarını karşılayacak şekilde videonun parçalarını vurgulamanız önerilir.

## <a name="features"></a>Özellikler

Aşağıdaki listede video ve ses modellerini Video Indexer kullanarak videolarınızdan alabileceğiniz Öngörüler gösterilmektedir:

### <a name="video-insights"></a>Video öngörüleri

* **Yüz algılama**: Videoda görünen yüzleri algılar ve gruplandırır.
* **Ünlüklik kimliği**: video Indexer dünya genelindeki dünya liderleri, aktörler, kadın, atlet, araştırmacılar, iş ve teknik liderler gibi 1.000.000 ünlükler üzerinden otomatik olarak tanımlar. Bu ünlüler hakkındaki veriler ayrıca çeşitli web sitelerinde (ıMDB, Vikipler, vb.) bulunabilir.
* **Hesap tabanlı yüz tanıma**: Video Indexer belirli bir hesap için bir model eğitir. Daha sonra eğitilen modele göre videodaki yüzeyleri tanır. Daha fazla bilgi için, [video Indexer Web sitesinden kişi modelini özelleştirme](customize-person-model-with-website.md) ve [video Indexer API 'Si ile bir kişi modelini özelleştirme](customize-person-model-with-api.md)bölümüne bakın.
* **Yüzler Için küçük resim ayıklama** ("en iyi yüz"): her bir yüzey grubundaki en iyi yakalanan yüzeyi (kalite, boyut ve en yüksek konuma göre) otomatik olarak tanımlar ve bunu bir görüntü varlığı olarak ayıklar.
* **Görsel metin tanıma** (OCR): videoda görsel olarak görüntülenen metni ayıklar.
* **Görsel içerik moderasyonu**: Yetişkinlere yönelik ve/veya müstehcen görselleri algılar.
* **Etiket belirleme**: Görüntülenen görsel nesneleri ve eylemleri belirler.
* **Sahne segmentleri**: görsel ipuçları temelinde videoda bir sahnenin ne zaman değişiklik olduğunu belirler. Bir sahne, tek bir olay gösterir ve bu, anlam ile ilgili olan bir dizi ardışık görünüm tarafından oluşur.
* **Görüntü algılama**: görsel ipuçları temelinde videoda bir çekneye ne zaman değişiklik olduğunu belirler. Bir görüntü, aynı hareket resmi kamerasından alınan bir çerçeve dizisidir. Daha fazla bilgi için bkz. [sahneler, görüntüleri ve ana kareler](scenes-shots-keyframes.md).
* **Siyah kare algılama**: Videoda yer alan siyah kareleri belirler.
* **Ana kare ayıklama**: Videodaki kararlı ana kareleri algılar.
* **Jenerik**KREDISI: TV programları ve filminin sonunda, hareketli kredilerin başlangıcını ve bitişini tanımlar.
* **Animasyonlu karakter algılama** (Önizleme): bilişsel [Hizmetler özel vizyonu](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/)ile tümleştirme aracılığıyla animasyonlu içerikte karakterlerin algılanması, gruplandırılması ve tanınması. Daha fazla bilgi için bkz. [animasyonlu karakter algılama](animated-characters-recognition.md).
* **Düzenleme görüntüsü türü algılama**: görüntülerini türlerine göre etiketleme (geniş görüntü, orta görüntü, yakın, büyük bir kapanış, iki görüntü, birden fazla kişi, dış mekve inkapıda vb.). Daha fazla bilgi için bkz. [düzenleme görüntüsü türü algılama](scenes-shots-keyframes.md#editorial-shot-type-detection).

### <a name="audio-insights"></a>Audio Insights

* **Otomatik dil algılama**: En baskın olarak konuşulan dili otomatik olarak belirler. Desteklenen diller arasında Ingilizce, Ispanyolca, Fransızca, Almanca, Italyanca, Çince (Basitleştirilmiş), Japonca, Rusça ve Portekizce (Brezilya). Dil güvenle tanımlanamıyorsa, Video Indexer konuşulan dilin Ingilizce olduğunu varsayar. Daha fazla bilgi için bkz. [dil tanımlama modeli](language-identification-model.md).
* **Çok dilli konuşma tanımlama ve** döküm (Önizleme): ses 'tan farklı kesimlerde konuşulan dili otomatik olarak tanımlar. Bu, oluşturulacak medya dosyasının her bir segmentini gönderir ve ardından geri alma işlemi ile Birleşik bir dökümü yeniden birleştirir. Daha fazla bilgi için bkz. [Çoklu dil Içeriğini otomatik olarak tanımla ve yeniden tanımla](multi-language-identification-transcription.md).
* **Ses dökümü**: konuşmayı 12 dilde metne dönüştürür ve uzantılara izin verir. Desteklenen diller arasında Ingilizce, Ispanyolca, Fransızca, Almanca, Italyanca, Çince (Basitleştirilmiş), Japonca, Arapça, Rusça, Brezilya Portekizcesi, Hintçe ve Korece bulunur.
* **Açıklamalı altyazı**: Üç biçimde açıklamalı altyazı oluşturur: VTT, TTML, SRT.
* **İki kanal işleme**: otomatik olarak ayrı dökümü algılar ve tek bir zaman çizelgesine birleştirir.
* **Gürültü azaltma**: telefon sesini veya gürültülü kayıtları (Skype filtreleri temelinde) temizler.
* **Transcript özelleştirmesi** (CRA): sektöre özgü döküm oluşturmak için özel konuşmayı metin modellerine kadar kullanın. Daha fazla bilgi için bkz. [video Indexer Web sitesinden dil modelini özelleştirme](customize-language-model-with-website.md) ve [video Indexer API 'leriyle bir dil modelini özelleştirme](customize-language-model-with-api.md).
* **Konuşmacı numaralandırması**: hangi konuşmacının hangi sözcüklere ve ne zaman bağlı olduğunu eşleştirir ve anlamıştır.
* **Konuşmacı İstatistikleri: konuşmacının**konuşma oranları için istatistikler sağlar.
* **Metinsel içerik moderasyonu**: Ses transkriptlerindeki müstehcen metinleri algılar.
* **Ses efektleri**: parmaklar, konuşma ve sessizlik gibi ses efektlerini tanımlar.
* **Duygu algılama**: konuşmayı (söylemekte olan) ve ses tonısına (nasıl söylenir) göre tanımlar. Duygu tanıma, Joy, sadyeti, Anger veya korku olabilir.
* **Çeviri**: Ses transkriptinin 54 farklı dile çevirisini oluşturur.

### <a name="audio-and-video-insights-multi-channels"></a>Ses ve video öngörüleri (çoklu kanallar)

Bir kanal tarafından Dizin oluşturulduğunda, bu modellerin kısmi sonucu kullanılabilir olacaktır.

* **Anahtar sözcük ayıklama**: konuşma ve görsel metinden anahtar sözcükleri ayıklar.
* **Adlandırılmış varlıkların ayıklanması**: marka, konum ve kişileri, doğal dil işleme (NLP) aracılığıyla konuşma ve görsel metinden ayıklar.
* **Konu çıkarımı**: Transkriptlerdeki ana konuların çıkarımını yapar. 2\. düzey ıPTC taksonomi dahil edilir.
* **Yapıtlar**: Modellerin her biri için "daha üst düzeyde ayrıntıya sahip" zengin bir yapıt kümesini ayıklar.
* **Yaklaşım analizi**: Konuşmalardaki ve görsel metinlerdeki olumlu, olumsuz ve nötr yaklaşımları belirler.

## <a name="how-can-i-get-started-with-video-indexer"></a>Video Indexer nasıl başlamalıyım?

Video Indexer özelliklerine üç yolla erişebilirsiniz:

* Video Indexer Portal: ürünü değerlendirmenize, hesabı yönetmenize ve modelleri özelleştirmenize olanak tanıyan kullanımı kolay bir çözümdür.

    Portal hakkında daha fazla bilgi için bkz. [video Indexer Web sitesi ile çalışmaya başlama](video-indexer-get-started.md).  

* API Tümleştirmesi: Tüm Video Indexer özellikleri, çözümü Uygulamalarınız ve altyapınızla tümleştirmenize olanak tanıyan bir REST API üzerinden kullanılabilir.

    Geliştirici olarak başlamak için bkz. [Use Video Indexer REST API](video-indexer-use-apis.md).

* Katıştırılabilir pencere öğesi: Video Indexer öngörüleri, oynatıcı ve düzenleyici deneyimlerini uygulamanıza eklemenizi sağlar.

    Daha fazla bilgi için bkz. [uygulamanıza görsel pencere öğeleri ekleme](video-indexer-embed-widgets.md).

Web sitesini kullanıyorsanız, Öngörüler meta veriler olarak eklenir ve portalda görünür. API 'Leri kullanıyorsanız, Öngörüler bir JSON dosyası olarak kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Video Indexer hizmetini kullanmaya başlamaya hazırsınız. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Video Indexer Web sitesini](video-indexer-get-started.md)kullanmaya başlayın.
- [Video Indexer REST API ile Içerik işleyin](video-indexer-use-apis.md).
- [Uygulamanıza görsel pencere öğeleri ekleyin](video-indexer-embed-widgets.md).
