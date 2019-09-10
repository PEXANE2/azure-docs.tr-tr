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
ms.date: 09/06/2019
ms.author: juliako
ms.openlocfilehash: e3f60b5fb0693e40c9db040f7b14f487fce8f68e
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860248"
---
# <a name="what-is-video-indexer"></a>Video Indexer nedir?

Azure Media Services Video Indexer ürünü Azure Media Analytics, Azure Search, Bilişsel Hizmetler (Yüz Tanıma API'si, Microsoft Translator, Görüntü İşleme API'si ve Özel Konuşma Tanıma Hizmeti gibi) temel alınarak oluşturulmuş bir bulut uygulamasıdır. Aşağıda açıklanan Video Indexer video ve ses modellerini kullanarak videolarınızdan öngörüleri ayıklamanızı sağlar:
  
## <a name="video-insights"></a>Video öngörüleri

- **Yüz algılama**: Videoda görünen yüzleri algılar ve gruplandırır.
- **Ünlüylik kimliği**: Video Indexer dünyanın dört bir yanında dünya öncüleri, aktörler, actresler, atlet, araştırmacılar, iş ve Teknoloji liderleriyle ilgili 1.000.000 ünlüler üzerinden otomatik olarak tanımlar. Bu ünlülerle ilgili bilgilere IMDB ve Wikipedia gibi birçok ünlü siteden de ulaşılabilir.
- **Hesap tabanlı yüz tanıma kimliği**: Belirli bir hesap için model Video Indexer. Daha sonra eğitilen modele göre videodaki yüzeyleri tanır. Daha fazla bilgi için, [video Indexer Web sitesinden kişi modelini özelleştirme](customize-person-model-with-website.md) ve [video Indexer API 'Si ile bir kişi modelini özelleştirme](customize-person-model-with-api.md)bölümüne bakın.
- **Yüzler Için küçük resim ayıklama** ("en iyi yüz"): Her bir yüzey grubundaki en iyi yakalanan yüzeyi (kalite, boyut ve en yüksek konuma göre) otomatik olarak belirler ve bunu bir görüntü varlığı olarak ayıklar.
- **Görsel metin tanıma** (OCR): Videoda görsel olarak görüntülenen metni ayıklar.
- **Görsel içerik denetleme**: Yetişkin ve/veya süslü görselleri algılar.
- **Etiket kimliği**: Görünen görsel nesneleri ve eylemleri tanımlar.
- **Sahne segmentleri**: görsel ipuçları temelinde videoda bir sahnenin ne zaman değişiklik olduğunu belirler. Bir sahne, tek bir olay gösterir ve bu, anlam ile ilgili olan bir dizi ardışık görünüm tarafından oluşur. 
- **Görüntü algılama**: görsel ipuçları temelinde videoda bir çekneye ne zaman değişiklik olduğunu belirler. Bir görüntü, aynı hareket resmi kamerasından alınan bir çerçeve dizisidir. Daha fazla bilgi için bkz. [sahneler, görüntüleri ve ana kareler](scenes-shots-keyframes.md).
- **Siyah çerçeve algılama**: Videoda sunulan siyah kareleri tanımlar.
- **Ana kare ayıklama**: Videodaki kararlı ana kareleri algılar.
- **Jenerik**KREDISI: TV programları ve filminin sonunda, hareketli kredilerin başlangıcını ve bitişini belirler.
- **Animasyonlu karakter algılama** (Önizleme): bilişsel [Hizmetler özel vizyonu](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/)ile tümleştirme aracılığıyla animasyonlu içerikte karakterlerin algılanması, gruplandırılması ve tanınması. Daha fazla bilgi için bkz. [animasyonlu karakter algılama](animated-characters-recognition.md).
- **Düzenleme görüntüsü türü algılama**: görüntülerini türlerine göre etiketleme (geniş görüntü, orta görüntü, yakın, büyük bir kapanış, iki görüntü, birden fazla kişi, dış mekve inkapıda vb.). Daha fazla bilgi için bkz. [düzenleme görüntüsü türü algılama](scenes-shots-keyframes.md#editorial-shot-type-detection).

## <a name="audio-insights"></a>Audio Insights

- **Otomatik dil algılama**: , Baskın konuşulan dili otomatik olarak tanımlar. Desteklenen diller arasında Ingilizce, Ispanyolca, Fransızca, Almanca, Italyanca, Çince (Basitleştirilmiş), Japonca, Rusça ve Brezilya Portekizcesi. Dil güvenle tanımlanamıyorsa, Video Indexer konuşulan dilin Ingilizce olduğunu varsayar. Daha fazla bilgi için bkz. [dil tanımlama modeli](language-identification-model.md).
- **Çok dilli konuşma tanımlama ve dökümü** (Önizleme): , Bir araya getirilebilmesi için medya dosyasının her bir kesimini göndererek ve geri dönerek tek bir Birleşik döküm ile birleştirmek için, sesli olarak konuşulan dili farklı kesimlerde otomatik olarak tanımlar. Daha fazla bilgi için bkz. [Çoklu dil Içeriğini otomatik olarak tanımla ve yeniden tanımla](multi-language-identification-transcription.md).
- **Ses dökümü**: Konuşmayı 12 dilde metne dönüştürür ve uzantılara izin verir. Desteklenen diller arasında Ingilizce, Ispanyolca, Fransızca, Almanca, Italyanca, Çince (Basitleştirilmiş), Japonca, Arapça, Rusça, Brezilya Portekizcesi, Hintçe ve Korece bulunur.
- **Kapalı açıklamalı altyazı**: Kapalı açıklamalı alt yazıları üç biçimde oluşturur: VTT, TTML, SRT.
- **İki kanal işleme**: Otomatik olarak algılar, dökümü ve birleştirmeleri tek bir zaman çizelgesine ayırır.
- **Gürültü azaltma**: Telefon sesini veya gürültülü kayıtları temizler (Skype filtrelerine göre).
- Döküm **özelleştirmesi** (CRSıS): Sektöre özgü döküm oluşturmak için özel konuşmayı metin modellerine kadar eğitme. Daha fazla bilgi için bkz. [video Indexer Web sitesinden dil modelini özelleştirme](customize-language-model-with-website.md) ve [video Indexer API 'leriyle bir dil modelini özelleştirme](customize-language-model-with-api.md).
- **Konuşmacı numaralandırması**: Hangi konuşmacının hangi sözcüklere ve ne zaman bağlı olduğunu eşleştirir ve anlamıştır.
- **Konuşmacı istatistikleri**: Hoparlörler konuşma oranları için istatistikler sağlar.
- **Metinsel içerik denetlemesi**: Ses dökümünü betiğinde açık metin algılar.
- **Ses efektleri**: Parmaklıklar, konuşma ve sessizlik gibi ses efektlerini tanımlar.
- **Duygu algılama**: Konuşmayı (söylemekte olan) ve ses tonlarını (nasıl söylüreceğini) temel alan özellikleri tanımlar.  Şu duygular belirlenebilir: sevinç, üzüntü, kızgınlık ve korku.
- **Çeviri**: Ses dökümü 54 farklı dillerde çeviri oluşturur.

## <a name="audio-and-video-insights-multi-channels"></a>Ses ve video öngörüleri (çoklu kanallar)

Bu modeller için bir kanal kısmi sonucu tarafından dizin oluşturma kullanılabilir olacaktır

- **Anahtar sözcük ayıklama**: Konuşma ve görsel metinden anahtar sözcükleri ayıklar.
- **Adlandırılmış varlıkların ayıklanması**: Marka, konum ve kişileri, doğal dil işleme (NLP) aracılığıyla konuşma ve görsel metinden ayıklar.
- **Konu çıkarımı**: Döküm dosyalarından Ana konuların çıkarımını oluşturur. 1\. düzey ıPTC taksonomi dahil edilir.
- **Yapıtlar**: Modellerin her biri için zengin "sonraki ayrıntı düzeyi" yapıtları kümesini ayıklar.
- Yaklaşım **Analizi**: Konuşma ve görsel metinden olumlu, olumsuz ve nötr düşünceleri tanımlar.
 
Video Indexer işlemi ve analizi tamamladıktan sonra video içgörülerini gözden geçirebilir, düzenleyebilir, içerisinde arama yapabilir ve yayımlayabilirsiniz.

Video Indexer hizmeti, hem içerik yöneticilerinin hem de geliştiricilerin ihtiyaçlarını karşılayacak şekilde tasarlanmıştır. İçerik yöneticileri Video Indexer web portalını kullanarak tek bir kod satırı yazmadan hizmetten faydalanabilir, bkz. [Video Indexer web sitesini kullanmaya başlama](video-indexer-get-started.md). Geliştiriciler, büyük ölçekli verileri işlemek için API'leri kullanabilirler, bkz. [Video Indexer REST API'sini kullanma](video-indexer-use-apis.md). Hizmet ayrıca müşterilerin pencere öğelerini kullanarak video akışlarını ve ayıklanan içgörüleri kendi uygulamalarında yayımlamalarını sağlar, bkz. [Görsel pencere öğelerini uygulamanıza ekleme](video-indexer-embed-widgets.md).

Bu hizmete var olan AAD, LinkedIn, Facebook, Google veya MSA hesabınızla kaydolabilirsiniz. Daha fazla bilgi için [başlarken](video-indexer-get-started.md) bölümüne bakın.

## <a name="scenarios"></a>Senaryolar

Aşağıda Video Indexer hizmetinin yararlı olabileceği senaryolar verilmiştir

- Arama – Videodan ayıklanan içgörüler, bir video kitaplığındaki arama deneyimini geliştirmek için kullanılabilir. Örneğin konuşmaları ve yüzleri dizine eklemek, videoda belirli bir kişinin belirli bir sözcüğü söylediği veya iki kişinin bir arada göründüğü anları bulmayı sağlayacak arama deneyimini mümkün kılabilir. Videolardan elde edilen bu içgörülerle yapılan aramalar haber ajansları, eğitim kurumları, yayıncılar, eğlence içeriği sahipleri, kurumsal iş kolu uygulamaları ve kullanıcıların arama yapması gereken bir video kitaplığına sahip olan tüm sektörleri hedeflemektedir.
- İçerik oluşturma – videolardan ayıklanan Öngörüler ve yeni içerik oluşturma, sosyal medya içeriği, haber klipleri, kuruluş arşivindeki mevcut içerikten 
- Paraya dönüştürme – Video Indexer, videoların değerinin artırılmasına yardımcı olabilir. Örneğin reklam gelirine bağlı olan sektörler (haber, sosyal medya vb.), ayıklanan içgörüleri reklam sunucusuna ek sinyal olarak ekleyerek daha ilgili reklamların görüntülenmesini sağlayabilir (örneğin bir spor ayakkabısı reklamının, yüzme yarışması yerine futbol maçının ortasında gösterilmesi daha uygun olacaktır).
- Kullanıcı etkileşimi – Video içgörüleri, kullanıcılara ilgili video anlarının sunularak kullanıcı etkileşiminin geliştirilmesini sağlayabilir. Örneğin ilk 30 dakika boyunca kürelerin, ikinci 30 dakika boyunca da piramitlerin anlatıldığı bir video olduğunu düşünün. Videonun 30. dakikadan başlatılması, piramitlerle ilgili çalışma yapan bir öğrenci için daha yararlı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Video Indexer hizmetini kullanmaya başlamaya hazırsınız. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Video Indexer web sitesini kullanmaya başlama](video-indexer-get-started.md)
- [Video Indexer REST API'si ile içerik işleme](video-indexer-use-apis.md)
- [Görsel pencere öğelerini uygulamanıza ekleme](video-indexer-embed-widgets.md)
