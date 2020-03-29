---
title: Azure CDN gelişmiş HTTP raporlarıyla kullanım istatistiklerini analiz edin | Microsoft Dokümanlar
description: Microsoft Azure CDN'de gelişmiş HTTP raporlarının nasıl oluşturulabildiğini öğrenin. Bu raporlar CDN etkinliği hakkında ayrıntılı bilgi sağlar.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: ef90adc1-580e-4955-8ff1-bde3f3cafc5d
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 0b0eec2425f8a1663eb7a09c83a6bad037d1d79c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67594103"
---
# <a name="analyze-usage-statistics-with-azure-cdn-advanced-http-reports"></a>Azure CDN gelişmiş HTTP raporlarıyla kullanım istatistiklerini analiz edin
## <a name="overview"></a>Genel Bakış
Bu belge, Microsoft Azure CDN'deki gelişmiş HTTP raporlamalarını açıklar. Bu raporlar CDN etkinliği hakkında ayrıntılı bilgi sağlar.

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Gelişmiş HTTP raporlarına erişim
1. CDN profil bıçağından **Yönet** düğmesini tıklatın.
   
    ![CDN profil blade yönetme düğmesi](./media/cdn-advanced-http-reports/cdn-manage-btn.png)
   
    CDN yönetim portalı açılır.
2. **Analytics** sekmesinin üzerine gel, ardından **Gelişmiş HTTP Raporları** uçuşunun üzerinde gezin.  HTTP **Büyük Platformu'na**tıklayın.
   
    ![CDN yönetim portalı - Gelişmiş Raporlar menüsü](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)
   
    Rapor seçenekleri görüntülenir.

## <a name="geography-reports-map-based"></a>Coğrafya Raporları (Harita Tabanlı)
İçeriğinizin istendiği bölgeleri belirtmek için bir haritadan yararlanan beş rapor vardır. Bu raporlar Dünya Haritası, Amerika Birleşik Devletleri Haritası, Kanada Haritası, Avrupa Haritası ve Asya Pasifik Haritası'dır.

Harita tabanlı her rapor coğrafi varlıkları sıralar (örneğin, ülkeler/bölgeler, içeriğinizin istendiği konumları görselleştirmenize yardımcı olmak için bir harita sağlanır). Bunu, o bölgede yaşanan talep miktarına göre her bölgeyi renk lendirerek yapabilir. Daha açık gölgeli bölgeler içeriğinize olan talebin daha düşük olduğunu gösterirken, daha koyu bölgeler içeriğinize daha yüksek talep düzeyleri gösterir.

Her bölge için ayrıntılı trafik ve bant genişliği bilgileri haritanın hemen altında sağlanır. Bu, toplam isabet sayısını, isabet yüzdesini, aktarılan toplam veri miktarını (gigabaytlarda) ve her bölge için aktarılan verilerin yüzdesini görüntülemenize olanak tanır. Bu ölçümlerin her biri için bir açıklama görüntüleyin. Son olarak, bir bölgenin (örn. ülke/bölge, eyalet veya bölge) üzerinde gezinirken, bölgede meydana gelen isabetlerin adı ve yüzdesi araç ipucu olarak görüntülenir.

Harita tabanlı coğrafya raporunun her türü için aşağıda kısa bir açıklama verilmiştir.

| Rapor Adı | Açıklama |
| --- | --- |
| Dünya Haritası |Bu rapor, CDN içeriğiniz için dünya çapındaki talebi görüntülemenize olanak tanır. Her ülke/bölge, o bölgeden kaynaklanan isabet yüzdesini belirtmek için dünya haritasında renk kodludur. |
| Amerika Birleşik Devletleri Haritası |Bu rapor, CDN içeriğiniz için abd'deki talebi görüntülemenize olanak tanır. Her durum, bu haritada, o bölgeden kaynaklanan isabet yüzdesini belirtmek için renk kodludur. |
| Kanada Haritası |Bu rapor, Kanada'daki CDN içeriğinize olan talebi görüntülemenizi sağlar. Her bölge, bu bölgeden kaynaklanan isabet yüzdesini belirtmek için bu haritada renk kodludur. |
| Avrupa Haritası |Bu rapor, Avrupa'daki CDN içeriğinize olan talebi görüntülemenizi sağlar. Her ülke/bölge, bu haritada, o bölgeden kaynaklanan isabet yüzdesini belirtmek için renk kodludur. |
| Asya Pasifik Haritası |Bu rapor, Asya'daki CDN içeriğinize olan talebi görüntülemenizi sağlar. Her ülke/bölge, bu haritada, o bölgeden kaynaklanan isabet yüzdesini belirtmek için renk kodludur. |

## <a name="geography-reports-bar-charts"></a>Coğrafya Raporları (Çubuk Grafikler)
En İyi Şehirler ve En İyi Ülkeler olan coğrafyaya göre istatistiksel bilgi sağlayan iki ek rapor daha bulunmaktadır. Bu raporlar, şehirleri ve ülkeleri/bölgeleri sırasıyla, bu ülkelerden/bölgelerden kaynaklanan isabet sayısına göre sıralar. Bu tür bir rapor üretildikten sonra, bir çubuk grafik, belirli bir platform üzerinden içerik isteyen en iyi 10 şehri veya ülkeyi/bölgeyi gösterir. Bu çubuk grafik, içeriğiniz için en yüksek sayıda istek oluşturan bölgeleri hızla değerlendirmenize olanak tanır.

Grafiğin sol tarafı (y ekseni) belirtilen bölgede kaç isabet oluştuğunu gösterir. Grafiğin (x ekseni) hemen altında, en iyi 10 bölgenin her biri için bir etiket bulacaksınız.

### <a name="using-the-bar-charts"></a>Çubuk grafiklerini kullanma
* Bir çubuğun üzerinde gezinirseniz, bölgede oluşan ad ve toplam isabet sayısı araç ucu olarak görüntülenir.
* Top Cities raporunun araç ipucu, bir şehri ada, eyalet/eyalete ve ülke/bölge kısaltmasına göre tanımlar.
* Bir talebin kaynaklandığı şehir veya bölge (yani eyalet/eyalet) belirlenemediyse, bu onların bilinmediğini gösterir. Ülke/bölge bilinmiyorsa, iki soru işareti (örn.
* Bir rapor "Avrupa" veya "Asya/Pasifik Bölgesi" ölçümlerini içerebilir. Bu öğeler, bu bölgelerdeki tüm IP adresleri hakkında istatistiksel bilgi sağlamak için değildir. Bunun yerine, yalnızca belirli bir şehir veya ülke/bölge yerine Avrupa veya Asya/Pasifik'e yayılan IP adreslerinden kaynaklanan istekler için geçerlidir.

Çubuk grafiği oluşturmak için kullanılan veriler aşağıda görüntülenebilir. Burada toplam isabet sayısını, isabet yüzdesini, aktarılan veri miktarını (gigabaytlarda) ve en iyi 250 bölge için aktarılan verilerin yüzdesini bulacaksınız. Bu ölçümlerin her biri için bir açıklama görüntüleyin.

Aşağıdaki her iki rapor türü için de kısa bir açıklama sağlanmıştır.

| Rapor Adı | Açıklama |
| --- | --- |
| En İyi Şehirler |Bu rapor, şehirleri o bölgeden gelen isabet sayısına göre sıralar. |
| En İyi Ülkeler |Bu rapor, ülkeleri/bölgeleri, o ülkeden/bölgeden kaynaklanan isabet sayısına göre sıralar. |

## <a name="daily-summary"></a>Günlük Özet
Günlük Özet raporu, belirli bir platform üzerinden aktarılan toplam isabet ve veri sayısını günlük olarak görüntülemenize olanak tanır. Bu bilgiler CDN etkinlik desenlerini hızla ayırt etmek için kullanılabilir. Örneğin, bu rapor, beklenenden daha yüksek veya daha düşük trafikle hangi günlerin yaşandığını tespit nize yardımcı olabilir.

Bu tür bir rapor oluşturduktan sonra, bir çubuk grafik, raporun kapsadığı süre boyunca günlük olarak karşılaşılan platforma özgü talep miktarı hakkında görsel bir gösterge sağlayacaktır. Bunu raporda her gün için bir çubuk görüntüleyerek yapacaktır. Örneğin, "Geçen Hafta" adlı zaman dilimini seçmek, yedi çubuklu bir çubuk grafik oluşturur. Her çubuk, o gün yaşanan toplam isabet sayısını gösterir.

Grafiğin sol tarafı (y ekseni) belirtilen tarihte kaç isabet oluştuğunu gösterir. Grafiğin (x ekseni) hemen altında, raporda yer alan her gün için tarihi (Format: YYYY-MM-DD) gösteren bir etiket bulursunuz.

> [!TIP]
> Bir çubuğun üzerinde gezinirseniz, o tarihte gerçekleşen toplam isabet sayısı araç ucu olarak görüntülenir.
> 
> 

Çubuk grafiği oluşturmak için kullanılan veriler aşağıda görüntülenebilir. Burada, raporun kapsadığı her gün için toplam isabet sayısını ve aktarılan veri miktarını (gigabaytolarak) bulacaksınız.

## <a name="by-hour"></a>Saate Göre
By Hour raporu, belirli bir platform üzerinden aktarılan toplam isabet ve veri sayısını saatlik olarak görüntülemenize olanak tanır. Bu bilgiler CDN etkinlik desenlerini hızla ayırt etmek için kullanılabilir. Örneğin, bu rapor, gün içinde beklenenden daha yüksek veya daha düşük trafik yaşayan zaman dönemlerini algılamanıza yardımcı olabilir.

Bu tür bir rapor oluşturduktan sonra, bir çubuk grafik, raporun kapsadığı süre boyunca saatlik olarak karşılaşılan platforma özgü talep miktarı hakkında görsel bir gösterge sağlar. Bunu, raporun kapsadığı her saat için bir çubuk göstererek yapar. Örneğin, 24 saatlik bir zaman dilimi seçmek yirmi dört çubuklu bir çubuk grafik oluşturur. Her çubuk, o saat içinde yaşanan toplam isabet sayısını gösterir.

Grafiğin sol tarafı (y ekseni) belirtilen saatte kaç isabet oluştuğunu gösterir. Grafiğin (x ekseni) hemen altında, raporda yer alan her saat için tarih/saati (Format: YYYY-MM-DD hh:mm) gösteren bir etiket bulacaksınız. Saat 24 saat biçimi kullanılarak bildirilir ve UTC/GMT saat dilimi kullanılarak belirtilir.

> [!TIP]
> Bir çubuğun üzerinde gezinirseniz, o saat içinde oluşan toplam isabet sayısı araç ucu olarak görüntülenir.
> 
> 

Çubuk grafiği oluşturmak için kullanılan veriler aşağıda görüntülenebilir. Burada, raporun kapsadığı her saat için toplam isabet sayısını ve aktarılan veri miktarını (gigabaytlarda) bulacaksınız.

## <a name="by-file"></a>Dosyaya Göre
Dosya Tarafından raporu, en çok istenen varlıklar için belirli bir platform üzerinden tahakkuk eden talep miktarını ve trafiği görüntülemenize olanak tanır. Bu rapor türünü oluşturduktan sonra, belirtilen süre içinde en çok istenen 10 varlık üzerinde bir çubuk grafik oluşturulur.

> [!NOTE]
> Bu raporun amaçları doğrultusunda, kenar CNAME URL'leri eşdeğer CDN URL'lerine dönüştürülür. Bu, bir varlıkla ilişkili toplam isabet sayısının, istemek için kullanılan CDN veya kenar CNAME URL'sinden bağımsız olarak doğru bir sayım alabına olanak tanır.
> 
> 

Grafiğin sol tarafı (y ekseni), belirtilen zaman dilimi içinde her kıymet için istek sayısını gösterir. Grafiğin (x ekseni) hemen altında, istenen en iyi 10 varlığın her biri için dosya adını gösteren bir etiket bulacaksınız.

Çubuk grafiği oluşturmak için kullanılan veriler aşağıda görüntülenebilir. Burada en çok istenen 250 varlığın her biri için aşağıdaki bilgileri bulacaksınız: göreli yol, toplam isabet sayısı, isabet yüzdesi, aktarılan veri miktarı (gigabayt olarak) ve aktarılan verilerin yüzdesi.

## <a name="by-file-detail"></a>Dosya Detayına Göre
Dosya Ayrıntısı tarafından rapor, belirli bir varlık için belirli bir platform üzerinden tahakkuk eden talep miktarını ve trafiği görüntülemenize olanak tanır. Bu raporun en üstünde, Dosya Ayrıntıları seçeneği yer alıyor. Bu seçenek, seçili platformda en çok istediğiniz varlıkların listesini sağlar. Dosya Ayrıntısına Göre raporu oluşturmak için, Dosya Ayrıntıları seçeneğinden istenen varlığı seçmeniz gerekir. Bundan sonra, bir çubuk grafik, belirtilen zaman dilimi içinde oluşturulan günlük talep miktarını gösterir.

Grafiğin sol tarafı (y ekseni), bir varlığın belirli bir günde karşılaştığı toplam istek sayısını gösterir. Grafiğin (x ekseni) hemen altında, kıymet için CDN talebinin raporlandığı tarihi (Biçim: YYYY-MM-DD) gösteren bir etiket bulursunuz.

Çubuk grafiği oluşturmak için kullanılan veriler aşağıda görüntülenebilir. Burada, raporun kapsadığı her gün için toplam isabet sayısını ve aktarılan veri miktarını (gigabaytolarak) bulacaksınız.

## <a name="by-file-type"></a>Dosya Türüne Göre
Dosya Türüne Göre raporu, talep miktarını ve dosya türünden kaynaklanan trafiği görüntülemenize olanak tanır. Bu tür bir rapor oluşturulduktan sonra, donut grafiği en iyi 10 dosya türü tarafından oluşturulan isabet yüzdesini gösterir.

> [!TIP]
> Donut grafiğindeki bir dilimin üzerinde gezinirseniz, bu dosya türünün Internet ortam türü araç ipucu olarak görüntülenir.
> 
> 

Donut grafiğioluşturmak için kullanılan veriler aşağıda görüntülenebilir. Burada dosya adı uzantısı/Internet ortam türü, toplam isabet sayısı, isabet yüzdesi, aktarılan veri miktarı (gigabaytolarak) ve en iyi 250 dosya türünün her biri için aktarılan verilerin yüzdesi bulacaksınız.

## <a name="by-directory"></a>Yazar: Directory
By Directory raporu, belirli bir dizindeki içerik için belirli bir platform üzerinden tahakkuk eden talep miktarını ve trafiği görüntülemenize olanak tanır. Bu tür bir rapor oluşturulduktan sonra, çubuk grafik, ilk 10 dizindeki içerik tarafından oluşturulan toplam isabet sayısını gösterir.

### <a name="using-the-bar-chart"></a>Çubuk grafiği kullanma
* İlgili dizine giden göreli yolu görüntülemek için çubuğun üzerine titreyin.
* Dizin alt klasöründe depolanan içerik, dizin tarafından talep hesaplanırken sayılmaz. Bu hesaplama yalnızca gerçek dizinde depolanan içerik için oluşturulan istek sayısına dayanır.
* Bu raporun amaçları doğrultusunda, kenar CNAME URL'leri eşdeğer CDN URL'lerine dönüştürülür. Bu, bir varlıkla ilişkili tüm istatistikler için, istemek için kullanılan CDN veya kenar CNAME URL'sinden bağımsız olarak doğru bir sayım sağlar.

Grafiğin sol tarafı (y ekseni), en iyi 10 dizininizde depolanan içerik için toplam istek sayısını gösterir. Grafikteki her çubuk bir dizin temsil eder. Bir çubuğu En İyi 250 Tam Dizinler bölümünde listelenen bir dizine eşleştirmek için renk kodlama düzenini kullanın.

Çubuk grafiği oluşturmak için kullanılan veriler aşağıda görüntülenebilir. Burada en iyi 250 dizinlerin her biri için aşağıdaki bilgileri bulacaksınız: göreli yol, toplam isabet sayısı, isabet yüzdesi, aktarılan veri miktarı (gigabayt olarak) ve aktarılan verilerin yüzdesi.

## <a name="by-browser"></a>Tarayıcıya Göre
Tarayıcı tarafından rapor, içerik istemek için hangi tarayıcıların kullanıldığını görüntülemenize olanak tanır. Bu tür bir rapor üretilmesi üzerine, bir pasta grafiği en iyi 10 tarayıcı tarafından işlenen isteklerin yüzdesini gösterir.

### <a name="using-the-pie-chart"></a>Pasta grafiğini kullanma
* Tarayıcının adını ve sürümünü görüntülemek için pasta grafiğindeki bir dilimin üzerine titreyin.
* Bu raporun amaçları için, her benzersiz tarayıcı/sürüm kombinasyonu farklı bir tarayıcı olarak kabul edilir.
* "Diğer" adı verilen dilim, diğer tüm tarayıcılar ve sürümler tarafından işlenen isteklerin yüzdesini gösterir.

Pasta grafiği oluşturmak için kullanılan veriler aşağıda görüntülenebilir. Burada tarayıcı türü/sürüm numarası, toplam isabet sayısı ve en iyi 250 tarayıcının her biri için isabet yüzdesi bulacaksınız.

## <a name="by-referrer"></a>Yazar: Referrer
By Referrer raporu, seçili platformdaki içeriğe en üstten yönlendirenleri görüntülemenize olanak tanır. Başvuran, istek oluşturulan ana bilgisayar adını gösterir. Bu tür bir rapor oluşturulduktan sonra, bir çubuk grafik, en iyi 10 başvurucu tarafından oluşturulan talep miktarını (yani, isabet) gösterir.

Grafiğin sol tarafı (y ekseni), bir varlığın her başvuru için karşılaştığı toplam istek sayısını gösterir. Grafikteki her çubuk bir başvurucucuyu temsil eder. Bir çubuğu En İyi 250 Yönlendiren bölümünde listelenen bir başvurucucuyla eşleştirmek için renk kodlama düzenini kullanın.

Çubuk grafiği oluşturmak için kullanılan veriler aşağıda görüntülenebilir. Burada URL'yi, toplam isabet sayısını ve en iyi 250 başvurucunun her birinden oluşturulan isabet yüzdesini bulacaksınız.

## <a name="by-download"></a>İndirerek
By Download raporu, en çok istediğiniz içeriğiniz için indirme modellerini çözümlemenize olanak tanır. Raporun üst kısmında, istenen en iyi 10 varlık için tamamlanan indirmelerle deneme denemelerini karşılaştıran bir çubuk grafik içerir. Her çubuk, indirme denemesi (mavi) veya tamamlanmış bir indirme (yeşil) olup olmadığına göre renk kodludur.

> [!NOTE]
> Bu raporun amaçları doğrultusunda, kenar CNAME URL'leri eşdeğer CDN URL'lerine dönüştürülür. Bu, bir varlıkla ilişkili tüm istatistikler için, istemek için kullanılan CDN veya kenar CNAME URL'sinden bağımsız olarak doğru bir sayım sağlar.
> 
> 

Grafiğin sol tarafı (y ekseni), istenen en iyi 10 varlığın her biri için dosya adını gösterir. Grafiğin (x ekseni) hemen altında, toplam denenmiş/tamamlanmış indirme sayısını gösteren etiketler bulacaksınız.

Çubuk grafiğin hemen altında, aşağıdaki bilgiler en çok istenen 250 varlık için listelenir: göreli yol (dosya adı dahil), tamamlanmak üzere indirilme sayısı, talep edilen kaç kez ve tam bir indirme ile sonuçlanan istekler.

> [!TIP]
> CdN'miz, bir varlık tamamen indirildiğinde bir HTTP istemcisi (yani tarayıcı) tarafından bilgilendirilmez. Sonuç olarak, bir varlığın durum kodlarına ve bayt aralığı isteklerine göre tamamen indirilip indirilmediğini hesaplamamız gerekir. Bu hesaplamayaparken aradığımız ilk şey, isteğin 200 Ok durum koduyla sonuçlanıp sonuçlamayacağıdır. Eğer öyleyse, o zaman tüm varlığı kapsayacak şekilde bayt aralığı istekleribakmak. Son olarak, istenen varlığın boyutuna aktarılan veri miktarını karşılaştırıyoruz. Aktarılan veriler dosya boyutuna eşit veya daha büyükse ve bayt aralığı istekleri bu varlık için uygunsa, isabet tam bir karşıdan yükleme olarak sayılır.
> 
> Bu raporun yorumlayıcı yapısı nedeniyle, bu raporun tutarlılığını ve doğruluğunu değiştirebilecek aşağıdaki noktaları göz önünde bulundurmalısınız.
> 
> * Kullanıcı aracıları farklı davrandığında trafik desenleri doğru tahmin edilemez. Bu, %100'den büyük tamamlanmış indirme sonuçları üretebilir.
> * HTTP Aşamalı İndirme'den yararlanan varlıklar bu raporla doğru şekilde temsil edilemeyebilir. Bunun nedeni, bir videoda farklı konumlara gitmek isteyen kullanıcılardır.
> 
> 

## <a name="by-404-errors"></a>Tarafından 404 Hatalar
By 404 Errors raporu, en çok 404 Bulunamadı durum kodu üreten içerik türünü belirlemenize olanak tanır. Raporun üst kısmında, 404 Bulunamadı durum kodunun döndürüldİğİ en iyi 10 varlık için bir çubuk grafik bulunur. Bu çubuk grafik, bu varlıklar için 404 Bulunamadı durum koduyla sonuçlanan isteklerle toplam istek sayısını karşılaştırır. Her çubuk renk kodludur. Sarı çubuk, isteğin 404 Bulunamadı durum koduyla sonuçlandığını belirtmek için kullanılır. Kıymet için toplam istek sayısını belirtmek için kırmızı çubuk kullanılır.

> [!NOTE]
> Bu raporun amaçları için aşağıdakileri unutmayın:
> 
> * Isabet, durum kodundan bağımsız olarak bir varlık için herhangi bir isteği temsil eder.
> * Kenar CNAME URL'leri eşdeğer CDN URL'lerine dönüştürülür. Bu, bir varlıkla ilişkili tüm istatistikler için, istemek için kullanılan CDN veya kenar CNAME URL'sinden bağımsız olarak doğru bir sayım sağlar.
> 
> 

Grafiğin sol tarafı (y ekseni), 404 Bulunamadı durum koduyla sonuçlanan en iyi 10 varlığın her biri için dosya adını gösterir. Grafiğin (x ekseni) hemen altında, toplam istek sayısını ve 404 Bulunamadı durum koduyla sonuçlanan istek sayısını gösteren etiketler bulacaksınız.

Çubuk grafiğin hemen altında, aşağıdaki bilgiler en çok istenen 250 varlık için listelenir: göreli yol (dosya adı dahil), 404 Bulunamadı durum koduyla sonuçlanan istek sayısı, varlığın talep edildiği toplam sayı ve 404 Bulunamadı durum koduyla sonuçlanan isteklerin yüzdesi.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure CDN'ye Genel Bakış](cdn-overview.md)
* [Microsoft Azure CDN'de gerçek zamanlı istatistikler](cdn-real-time-stats.md)
* [Kurallar altyapısını kullanarak varsayılan HTTP davranışını geçersiz kılma](cdn-rules-engine.md)
* [Kenar Performansını Analiz Et](cdn-edge-performance.md)

