---
title: Azure Media Services Video Indexer nedir?
titleSuffix: Azure Media Services
description: Bu konu, Azure Media Services Video Indexer hizmetine genel bir bakış sunar.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 09/17/2019
ms.author: juliako
ms.openlocfilehash: a30fa858ea7befa2363e0ac2742e16d46037d0e0
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71123493"
---
# <a name="what-is-video-indexer"></a>Video Indexer nedir?

Azure Media Services Video Indexer, Azure bilişsel hizmetler üzerinde oluşturulmuş bir AI çözümüdür. Video Indexer, birden fazla kanala (ses, Vocal, görsel) göre makine öğrenimi modellerini kullanarak derin içgörüler (veri çözümleme veya kodlama becerileri gerektirmez) ayıklayabilme olanağı sağlar. Modelleri daha fazla özelleştirebilir ve eğitebilirsiniz. Hizmet derin arama imkanı sunar, işlem maliyetlerini azaltır, Yeni Kullanıcı, büyük video arşivlerinde (düşük giriş engelleri ile) yeni kullanıcı deneyimleri sunar. 

Video Indexer öngörüleri çıkarmaya başlamak için bir hesap oluşturmanız ve videoları yüklemeniz gerekir. Videolarınızı Video Indexer yüklediğinizde, farklı AI modellerini çalıştırarak hem görselleri hem de sesi analiz edersiniz. Video Indexer, modellerinizi analiz eden, modeller tarafından ayıklanan Öngörüler.

![Akış diyagramı](./media/video-indexer-overview/model-chart.png)

## <a name="what-can-i-do-with-video-indexer"></a>Video Indexer ile ne yapabilirim?

Video Indexer içgörüler aşağıdakiler arasında birçok senaryoya uygulanabilir:

* *Derin arama* -video kitaplığı genelinde arama deneyimini geliştirmek için videodan ayıklanan öngörüleri kullanın. Örneğin, konuşulan sözcüklerin ve yüzlerin dizininin oluşturulması, bir kişinin belirli kelimeleri veya iki kişinin birlikte görüldüğü bir videoda dakika bulmayla ilgili arama deneyimini etkinleştirebilir. Videolardan elde edilen bu içgörülerle yapılan aramalar haber ajansları, eğitim kurumları, yayıncılar, eğlence içeriği sahipleri, kurumsal iş kolu uygulamaları ve kullanıcıların arama yapması gereken bir video kitaplığına sahip olan tüm sektörleri hedeflemektedir.
* *İçerik oluşturma*: İçeriğinizden çıkaran öngörülere göre, daha fazla bilgi video Indexer edinin, Re, sosyal medya içeriğini veya haberleri vurgulayın. Kişiler ve etiket görünümleri için ana kareler, sahneler işaretçileri ve zaman damgaları oluşturma işlemini çok daha yumuşak ve daha kolay hale getirir ve oluşturmakta olduğunuz içerik için ihtiyacınız olan videonun ilgi çekici bölümlerine doğrudan sahip olabilirsiniz.
* *Erişilebilirlik*: içeriğinizi Engelli kişiler için kullanılabilir hale getirmek isteyip istemediğinizi veya içeriğinizi farklı diller kullanılarak farklı bölgelere dağıtmak istiyorsanız, video tarafından sağlanan dökümü ve çeviriyi kullanabilirsiniz birden çok dilde Dizin Oluşturucu.
* *Para kazanma*: Video Indexer videoların değerini artırmaya yardımcı olabilir. Örnek olarak, ad gelirine (örneğin, haber medyası, sosyal medya vb.) bağlı olan endüstriler, ayıklanan öngörüleri ad sunucusuna ek sinyaller olarak kullanarak, ilgili reklamları sunabilir.
* *İçerik denetleme*: kullanıcılarınızın uygunsuz içeriklerden güvenli kalmasını sağlamak ve yayımladığınız içeriğin kuruluşunuzun değerleriyle eşleştiğinden emin olmak için metin ve görsel içerik denetleme modellerini kullanın. Belirli videoları otomatik olarak engelleyebilir veya içeriklerle ilgili olarak kullanıcılarınıza uyarı verebilirsiniz. 
* *Öneriler*: Video öngörüleri, kullanıcılara ilgili video dakikanızı ayırarak kullanıcı etkileşimini artırmak için kullanılabilir. Her videoyu ek meta verilerle etiketleyerek, kullanıcılara en uygun videoları önererek, videonun ihtiyaçlarını karşılayacak bir kısmını vurgulayacaksınız. 

## <a name="features"></a>Özellikler

Video ve ses modellerini Video Indexer kullanarak videolarınızdan alabileceğiniz öngörülerin listesi aşağıda verilmiştir:

### <a name="video-insights"></a>Video öngörüleri

* **Yüz algılama**: Videoda görünen yüzleri algılar ve gruplandırır.
* **Ünlüylik kimliği**: Video Indexer dünyanın dört bir yanında dünya öncüleri, aktörler, actresler, atlet, araştırmacılar, iş ve Teknoloji liderleriyle ilgili 1.000.000 ünlüler üzerinden otomatik olarak tanımlar. Bu ünlülerle ilgili bilgilere IMDB ve Wikipedia gibi birçok ünlü siteden de ulaşılabilir.
* **Hesap tabanlı yüz tanıma kimliği**: Belirli bir hesap için model Video Indexer. Daha sonra eğitilen modele göre videodaki yüzeyleri tanır. Daha fazla bilgi için, [video Indexer Web sitesinden kişi modelini özelleştirme](customize-person-model-with-website.md) ve [video Indexer API 'Si ile bir kişi modelini özelleştirme](customize-person-model-with-api.md)bölümüne bakın.
* **Yüzler Için küçük resim ayıklama** ("en iyi yüz"): Her bir yüzey grubundaki en iyi yakalanan yüzeyi (kalite, boyut ve en yüksek konuma göre) otomatik olarak belirler ve bunu bir görüntü varlığı olarak ayıklar.
* **Görsel metin tanıma** (OCR): Videoda görsel olarak görüntülenen metni ayıklar.
* **Görsel içerik denetleme**: Yetişkin ve/veya süslü görselleri algılar.
* **Etiket kimliği**: Görünen görsel nesneleri ve eylemleri tanımlar.
* **Sahne segmentleri**: görsel ipuçları temelinde videoda bir sahnenin ne zaman değişiklik olduğunu belirler. Bir sahne, tek bir olay gösterir ve bu, anlam ile ilgili olan bir dizi ardışık görünüm tarafından oluşur.
* **Görüntü algılama**: görsel ipuçları temelinde videoda bir çekneye ne zaman değişiklik olduğunu belirler. Bir görüntü, aynı hareket resmi kamerasından alınan bir çerçeve dizisidir. Daha fazla bilgi için bkz. sahneler, görüntüleri ve ana kareler.
* **Siyah çerçeve algılama**: Videoda sunulan siyah kareleri tanımlar.
* **Ana kare ayıklama**: Videodaki kararlı ana kareleri algılar.
* **Jenerik**KREDISI: TV programları ve filminin sonunda, hareketli kredilerin başlangıcını ve bitişini belirler.

### <a name="audio-insights"></a>Audio Insights

* **Otomatik dil algılama**: , Baskın konuşulan dili otomatik olarak tanımlar. Desteklenen diller arasında Ingilizce, Ispanyolca, Fransızca, Almanca, Italyanca, Çince (Basitleştirilmiş), Japonca, Rusça ve Brezilya Portekizcesi. Dil güvenle tanımlanamıyorsa, Video Indexer konuşulan dilin Ingilizce olduğunu varsayar. Daha fazla bilgi için bkz. [dil tanımlama modeli](language-identification-model.md).
* **Ses dökümü**: Konuşmayı 12 dilde metne dönüştürür ve uzantılara izin verir. Desteklenen diller arasında Ingilizce, Ispanyolca, Fransızca, Almanca, Italyanca, Çince (Basitleştirilmiş), Japonca, Arapça, Rusça, Brezilya Portekizcesi, Hintçe ve Korece bulunur.
* **Kapalı açıklamalı altyazı**: Kapalı açıklamalı alt yazıları üç biçimde oluşturur: VTT, TTML, SRT.
* **İki kanal işleme**: Otomatik olarak algılar, dökümü ve birleştirmeleri tek bir zaman çizelgesine ayırır.
* **Gürültü azaltma**: Telefon sesini veya gürültülü kayıtları temizler (Skype filtrelerine göre).
* Döküm **özelleştirmesi** (CRSıS): Sektöre özgü döküm oluşturmak için özel konuşmayı metin modellerine kadar eğitme. Daha fazla bilgi için bkz. [video Indexer Web sitesinden dil modelini özelleştirme](customize-language-model-with-website.md) ve [video Indexer API 'leriyle bir dil modelini özelleştirme](customize-language-model-with-api.md).
* **Konuşmacı numaralandırması**: Hangi konuşmacının hangi sözcüklere ve ne zaman bağlı olduğunu eşleştirir ve anlamıştır.
* **Konuşmacı istatistikleri**: Hoparlörlerin konuşma oranları için istatistikler sağlar.
* **Metinsel içerik denetlemesi**: Ses dökümünü betiğinde açık metin algılar.
* **Ses efektleri**: Parmaklıklar, konuşma ve sessizlik gibi ses efektlerini tanımlar.
* **Duygu algılama**: Konuşmayı (söylemekte olan) ve ses tonlarını (nasıl söylüreceğini) temel alan özellikleri tanımlar. Duygu tanıma, Joy, sadyeti, Anger veya korku olabilir.
* **Çeviri**: Ses dökümü 54 farklı dillerde çeviri oluşturur.

### <a name="audio-and-video-insights-multi-channels"></a>Ses ve video öngörüleri (çoklu kanallar)

Bu modeller için bir kanal kısmi sonucu tarafından dizin oluşturma kullanılabilir olacaktır

* **Anahtar sözcük ayıklama**: Konuşma ve görsel metinden anahtar sözcükleri ayıklar.
* **Markalar ayıklama**: Konuşma ve görsel metinden markalar ayıklar.
* **Konu çıkarımı**: Döküm dosyalarından Ana konuların çıkarımını oluşturur. 1\. düzey ıPTC taksonomi dahil edilir.
* **Yapıtlar**: Modellerin her biri için zengin "sonraki ayrıntı düzeyi" yapıtları kümesini ayıklar.
* Yaklaşım **Analizi**: Konuşma ve görsel metinden olumlu, olumsuz ve nötr düşünceleri tanımlar.

## <a name="how-can-i-get-started-with-video-indexer"></a>Video Indexer nasıl başlamalıyım?

Video Indexer özelliklerine üç farklı yol kullanarak erişebilirsiniz: 

* Video Indexer Portal-ürünü değerlendirmenize, hesabı yönetmenize ve modelleri özelleştirmenize olanak tanıyan kullanımı kolay bir çözümdür. 

    Portal hakkında daha fazla bilgi için bkz. [video Indexer Web sitesi ile çalışmaya başlama](video-indexer-get-started.md).  
* API Tümleştirmesi-çözümü uygulamalarınıza ve altyapıya tümleştirmenize olanak tanımak için Video Indexer tüm özellikleri REST API aracılığıyla kullanılabilir. 

    Geliştirici olarak başlamak için bkz. [Use Video Indexer REST API](video-indexer-use-apis.md). 
* Emendable pencere öğesi-video Indexer Insights, Player ve düzenleyici deneyimlerini uygulamanıza eklemenize olanak sağlar. 

    Daha fazla bilgi için bkz. [uygulamanıza görsel pencere öğeleri ekleme](video-indexer-embed-widgets.md). 

Web sitesini kullanıyorsanız, Öngörüler meta veriler olarak eklenir ve portalda görünür. API 'Leri kullanıyorsanız, Öngörüler bir JSON dosyası olarak kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar

Video Indexer hizmetini kullanmaya başlamaya hazırsınız. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Video Indexer web sitesini kullanmaya başlama](video-indexer-get-started.md)
- [Video Indexer REST API'si ile içerik işleme](video-indexer-use-apis.md)
- [Görsel pencere öğelerini uygulamanıza ekleme](video-indexer-embed-widgets.md)
