---
title: Azure CDN ile medya akışı iyileştirmesi
description: Akıcı medya dosyalarını kesintisiz teslim için iyileştirin
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: allensu
ms.openlocfilehash: 2931dffaaab2d06b2c06f03770a66d78d6466787
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81260488"
---
# <a name="media-streaming-optimization-with-azure-cdn"></a>Azure CDN ile medya akışı iyileştirmesi 
 
Yüksek tanımlı video kullanımı Internet üzerinde artıyor ve bu da büyük dosyaların verimli bir şekilde teslim edilmesi için zorluklar oluşturuyor. Müşteriler, dünyanın her yerindeki çeşitli ağlar ve istemciler üzerinde isteğe bağlı video ve canlı video varlıkları için sorunsuz kayıttan yürütmeyi bekler. Medya akışı dosyaları için hızlı ve verimli bir teslim mekanizması, sorunsuz ve keyifli bir tüketici deneyimi sağlamak açısından önemlidir.  

Canlı akış medyası, büyük boyutlarda ve eşzamanlı görüntüleyicilerin sayısı nedeniyle teslim edilmesi özellikle zordur. Uzun gecikmeler kullanıcıların kalmasına neden olur. Canlı akışlar zaman içinde önbelleğe alınamadığından ve büyük gecikmeleri görüntüleyiciler için kabul edilemediğinden, video parçalarının zamanında teslim edilmesi gerekir. 

Akış için istek desenleri de bazı yeni güçlükler sağlar. İsteğe bağlı video için popüler bir canlı akış veya yeni bir seri olduğunda, binlerce veya milyonlarca izleyici akış için aynı anda istek gösterebilir. Bu durumda, varlıklar henüz önbelleğe alınmamışsa, kaynak sunucuların olmaması için akıllı istek birleştirme önemlidir.
 

## <a name="media-streaming-optimizations-for-azure-cdn-from-microsoft"></a>Microsoft 'tan Azure CDN için medya akışı iyileştirmeleri

**Microsoft uç noktalarından alınan Azure CDN Standart** , genel Web teslim iyileştirme türünü kullanarak doğrudan akış medya varlıkları sağlar. 

**Microsoft 'tan Azure CDN Standard** için medya akışı iyileştirmesi, teslimin tek tek medya parçalarını kullanan canlı veya isteğe bağlı video temelli akış medyası için geçerlidir. Bu işlem, aşamalı indirme yoluyla veya bayt aralığı istekleri kullanılarak aktarılan tek bir büyük varlıktan farklıdır. Bu medya teslimi stili hakkında daha fazla bilgi için, [Azure CDN Ile büyük dosya indirme iyileştirmesi](cdn-large-file-optimization.md)konusuna bakın.

Genel medya teslimi veya isteğe bağlı video, medya varlıklarını daha hızlı sunmak için arka uç iyileştirmeleri olan Azure Content Delivery Network (CDN) kullanır. Ayrıca, zaman içinde öğrenilen en iyi yöntemlere göre medya varlıkları için yapılandırma kullanırlar.

### <a name="partial-cache-sharing"></a>Kısmi önbellek paylaşımı
Kısmi önbellek paylaşımı, CDN 'nin kısmen önbelleğe alınmış içeriği yeni isteklere almasına olanak tanır. Örneğin, CDN 'e yapılan ilk istek önbellek isabetsizliği ile sonuçlanırsa, istek kaynağa gönderilir. Bu tamamlanmamış içerik CDN önbelleğine yüklense de, CDN 'ye yapılan diğer istekler bu verileri almaya başlayabilir. 


## <a name="media-streaming-optimizations-for-azure-cdn-from-verizon"></a>Verizon 'ten Azure CDN için medya akışı iyileştirmeleri

Verizon uç noktalarından Verizon ve **Azure CDN Premium** **'dan Azure CDN Standart** , genel web teslimi iyileştirme türünü kullanarak doğrudan akış medya varlıkları sağlar. CDN 'deki bazı özellikler, varsayılan olarak medya varlıklarını doğrudan sunmaya yardımcı olur.

### <a name="partial-cache-sharing"></a>Kısmi önbellek paylaşımı

Kısmi önbellek paylaşımı, CDN 'nin kısmen önbelleğe alınmış içeriği yeni isteklere almasına olanak tanır. Örneğin, CDN 'e yapılan ilk istek önbellek isabetsizliği ile sonuçlanırsa, istek kaynağa gönderilir. Bu tamamlanmamış içerik CDN önbelleğine yüklense de, CDN 'ye yapılan diğer istekler bu verileri almaya başlayabilir. 

### <a name="cache-fill-wait-time"></a>Önbellek dolgusu bekleme süresi

 Önbellek dolgusu bekleme süresi özelliği, uç sunucusunu, HTTP yanıt üstbilgileri kaynak sunucudan alınana kadar aynı kaynak için sonraki istekleri tutmaya zorlar. Başlangıç noktasından gelen HTTP yanıt üst bilgileri, Zamanlayıcının süresi dolmadan önce geldiğinde, beklemeye alınan tüm istekler büyüyen önbellekten sunulur. Aynı zamanda, önbellek, kaynaktan alınan verilerle doldurulur. Varsayılan olarak, önbellek dolgusu bekleme süresi 3.000 milisaniyeye ayarlanır. 

 
## <a name="media-streaming-optimizations-for-azure-cdn-from-akamai"></a>Akamai 'ten Azure CDN için medya akışı iyileştirmeleri
 
**Akamai ' den Azure CDN Standard** , akış ortamı varlıklarını dünyanın her yerindeki kullanıcılara verimli bir şekilde sunan bir özellik sunar. Bu özellik, kaynak sunuculardaki yükü azalttığından gecikme sürelerini azaltır. Bu özellik standart Akamai fiyatlandırma katmanıyla birlikte kullanılabilir. 

**Akamai ' dan Azure CDN Standard** için medya akışı iyileştirmesi, teslimin tek tek medya parçalarını kullanan canlı veya isteğe bağlı video içi akış medyası için geçerlidir. Bu işlem, aşamalı indirme yoluyla veya bayt aralığı istekleri kullanılarak aktarılan tek bir büyük varlıktan farklıdır. Bu medya teslimi stili hakkında daha fazla bilgi için bkz. [büyük dosya iyileştirmesi](cdn-large-file-optimization.md).

Genel medya teslimi veya video isteğe bağlı medya teslimi iyileştirme türleri, medya varlıklarını daha hızlı sunmak için arka uç iyileştirmeleri olan bir CDN kullanır. Ayrıca, zaman içinde öğrenilen en iyi yöntemlere göre medya varlıkları için yapılandırma kullanırlar.

### <a name="configure-an-akamai-cdn-endpoint-to-optimize-media-streaming"></a>Medya akışını iyileştirmek için bir Akamai CDN uç noktası yapılandırma
 
İçerik teslim ağı (CDN) uç noktanızı, büyük dosyalar için Azure portal aracılığıyla teslimi iyileştirmek üzere yapılandırabilirsiniz. Bunu yapmak için REST API 'Lerini veya istemci SDK 'Larını de kullanabilirsiniz. Aşağıdaki adımlarda, Akamai profilinden bir **Azure CDN Standart** için Azure Portal işlem gösterilmektedir:

1. Yeni bir uç nokta eklemek için, bir Akamai **CDN profili** sayfasında **uç nokta**' ı seçin.
  
    ![Yeni uç nokta](./media/cdn-media-streaming-optimization/cdn-new-akamai-endpoint.png)

2. **En iyileştirilmiş for** açılan listesinde, isteğe bağlı video varlıkları için isteğe bağlı video **akışı** ' nı seçin. Canlı ve isteğe bağlı video akışı birleşimi yaparsanız, **genel medya akışı**' nı seçin.

    ![Akış seçildi](./media/cdn-media-streaming-optimization/02_Creating.png) 
 
Uç noktasını oluşturduktan sonra, belirli ölçütlerle eşleşen tüm dosyalar için iyileştirme uygular. Aşağıdaki bölümde bu işlem açıklanmaktadır. 

### <a name="caching"></a>Önbelleğe alma

**Akamai 'den Azure CDN Standard** , varlığın bir akış bildirimi veya parçası olduğunu algılarsa, genel Web tesliminden farklı önbelleğe alma sona erme süreleri kullanır. (Aşağıdaki tablodaki tam listeye bakın.) Her zaman olduğu gibi, kaynaktan gönderilen Cache-Control veya Expires üstbilgileri kabul edilir. Varlık bir medya varlığı değilse, genel Web tesliminin sona erme zamanlarını kullanarak önbelleğe alınır.

Çok sayıda kullanıcı henüz mevcut olmayan bir parça istemesi durumunda, kaynak boşaltması için kısa negatif önbellek süresi faydalıdır. Örnek, paketin kaynaktan kullanılamadığı canlı bir akıştır. Daha uzun önbelleğe alma aralığı, video içeriği genellikle değiştirilmediğinden, isteklerin kaynaktan oluşturulmasına da yardımcı olur.
 

|   | Genel web teslimi | Genel medya akışı | İsteğe bağlı video medya akışı  
--- | --- | --- | ---
Önbelleğe alma: pozitif <br> HTTP 200, 203, 300, <br> 301, 302 ve 410 | 7 gün |365 gün | 365 gün   
Önbelleğe alma: negatif <br> HTTP 204, 305, 404, <br> ve 405 | Hiçbiri | 1 saniye | 1 saniye
 
### <a name="deal-with-origin-failure"></a>Kaynak hatasıyla uğraşın  

Genel medya teslimi ve video isteğe bağlı medya teslimi, tipik istek desenleri için en iyi yöntemlere dayalı olarak kaynak zaman aşımları ve bir yeniden deneme günlüğü de vardır. Örneğin, genel medya teslimi canlı ve isteğe bağlı video temelli medya teslimi için olduğundan, canlı akışın zamana duyarlı yapısı nedeniyle daha kısa bir bağlantı zaman aşımı kullanır.

Bir bağlantı zaman aşımına uğrarsa, CDN istemciye bir "504-Gateway timeout" hatası göndermeden önce birkaç kez yeniden dener. 

Dosya türü ve boyut koşulları listesiyle eşleşen CDN, medya akışı davranışını kullanır. Aksi takdirde, genel Web teslimini kullanır.
   
### <a name="conditions-for-media-streaming-optimization"></a>Medya akışı iyileştirmesi için koşullar 

Aşağıdaki tabloda, medya akışı iyileştirmesi için karşılanması için ölçüt kümesi listelenmektedir: 
 
Desteklenen akış türleri | Dosya uzantıları  
--- | ---  
Apple HLS | M3U8, M3U, m3ub, anahtar, TS, AAC
Adobe HDS | f4m, F4X, drmmeta, Bootstrap, F4F,<br>SEG-Frag URL yapısı <br> (Regex ile eşleşen: ^ (/. *) SEQ (\d +)-Frag (\d +)
TILDE | MPD, Dash, DivX, ismv, M4S, M4V, MP4, MP4V, <br> sıdx, WebM, MP4A, M4A, Wed
Kesintisiz Akış | /manifest/,/QualityLevels/Fragments/
  
 
