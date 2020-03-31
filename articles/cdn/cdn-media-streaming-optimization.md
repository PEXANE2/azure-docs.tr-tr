---
title: Azure CDN ile medya akışı optimizasyonu
description: Sorunsuz teslim için akışlı ortam dosyalarını optimize edin
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: magattus
ms.openlocfilehash: c6ed546735058e330368151adb0df7323f943050
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67593664"
---
# <a name="media-streaming-optimization-with-azure-cdn"></a>Azure CDN ile medya akışı optimizasyonu 
 
Yüksek tanımlı video kullanımı büyük dosyaların verimli teslim için zorluklar oluşturur internet üzerinde artmaktadır. Müşteriler, talep üzerine videonun veya tüm dünyadaki çeşitli ağlarda ve istemcilerden canlı video varlıklarının sorunsuz bir şekilde oynatmasını bekler. Sorunsuz ve keyifli bir tüketici deneyimi sağlamak için medya akış dosyaları için hızlı ve verimli bir dağıtım mekanizması çok önemlidir.  

Canlı akış lı ortam, büyük boyutlar ve eşzamanlı görüntüleyen sayısı nedeniyle özellikle teslim etmek zordur. Uzun gecikmeler kullanıcıların ayrılmasına neden olur. Canlı akışlar önceden önbelleğe alınamadığından ve büyük gecikmeler görüntüleyenler tarafından kabul edilemediği için, video parçaları zamanında teslim edilmelidir. 

Akış istek kalıpları da bazı yeni zorluklar sağlar. Popüler bir canlı akış veya yeni bir dizi isteğe bağlı video için yayımlandığında, binlerce ve milyonlarca izleyici aynı anda akışı isteyebilir. Bu durumda, varlıklar henüz önbelleğe alınmadığında, akıllı istek birleştirme, kaynak sunucularını bastırmamak için hayati önem taşır.
 

## <a name="media-streaming-optimizations-for-azure-cdn-from-microsoft"></a>Microsoft'tan Azure CDN için ortam akışı optimizasyonları

Microsoft uç **noktalarından Gelen Azure CDN Standardı,** genel web teslim optimizasyonu türünü kullanarak doğrudan akışlı ortam varlıklarını sunar. 

**Microsoft'tan Azure CDN Standard** için medya akışı optimizasyonu, teslimat için tek tek ortam parçalarını kullanan canlı veya isteğe bağlı video akış ortamları için etkilidir. Bu işlem, aşamalı karşıdan yükleme yoluyla veya bayt aralığı istekleri kullanılarak aktarılan tek bir büyük varlıktan farklıdır. Bu ortam teslim tarzı hakkında bilgi için [Azure CDN ile Büyük dosya indirme optimizasyonu](cdn-large-file-optimization.md)bölümüne bakın.

Genel ortam teslimi veya isteğe bağlı video medya teslimi optimizasyon uyruk türleri, ortam varlıklarını daha hızlı sunmak için arka uç optimizasyonlarıyla Azure İçerik Teslim Ağı'nı (CDN) kullanır. Ayrıca, zaman içinde öğrenilen en iyi uygulamalara dayalı medya varlıkları için yapılandırmalar da kullanırlar.

### <a name="partial-cache-sharing"></a>Kısmi önbellek paylaşımı
Kısmi önbellek paylaşımı, CDN'nin kısmen önbelleğe alınmış içeriği yeni isteklere sunmasını sağlar. Örneğin, CDN'ye yapılan ilk istek önbellek kaçırmayla sonuçlanırsa, istek menşeine gönderilir. Bu tamamlanmamış içerik CDN önbelleğine yüklenmiş olsa da, CDN'ye gelen diğer istekler bu verileri almaya başlayabilir. 


## <a name="media-streaming-optimizations-for-azure-cdn-from-verizon"></a>Verizon'dan Azure CDN için ortam akışı optimizasyonları

**Verizon'dan Azure CDN Standardı** ve Verizon uç **noktalarından Azure CDN Premium,** genel web teslim optimizasyonu türünü kullanarak doğrudan akışlı ortam varlıklarını sunar. CDN'deki birkaç özellik, varsayılan olarak medya varlıklarının teslimine doğrudan yardımcı olur.

### <a name="partial-cache-sharing"></a>Kısmi önbellek paylaşımı

Kısmi önbellek paylaşımı, CDN'nin kısmen önbelleğe alınmış içeriği yeni isteklere sunmasını sağlar. Örneğin, CDN'ye yapılan ilk istek önbellek kaçırmayla sonuçlanırsa, istek menşeine gönderilir. Bu tamamlanmamış içerik CDN önbelleğine yüklenmiş olsa da, CDN'ye gelen diğer istekler bu verileri almaya başlayabilir. 

### <a name="cache-fill-wait-time"></a>Önbellek doldurma bekleme süresi

 Önbellek doldurma bekleme süresi özelliği, kenar sunucusunu, http yanıt üstbilgilerinin kaynağı sunucudan gelene kadar aynı kaynak için sonraki isteklerini tutmaya zorlar. Kaynaktan gelen HTTP yanıt üstbilgisi zamanlayıcı süresi dolmadan önce gelirse, beklemeye alınan tüm istekler büyüyen önbellekten sunulur. Aynı zamanda, önbellek kaynaktan gelen verilerle doldurulur. Varsayılan olarak, önbellek doldurma bekleme süresi 3.000 milisaniye olarak ayarlanır. 

 
## <a name="media-streaming-optimizations-for-azure-cdn-from-akamai"></a>Akamai'den Azure CDN için ortam akışı optimizasyonları
 
**Akamai'deki Azure CDN Standardı,** akışlı medya varlıklarını ölçekolarak dünya çapındaki kullanıcılara verimli bir şekilde sunan bir özellik sunar. Özellik, kaynak sunuculardaki yükü azalttığından gecikme gevellerini azaltır. Bu özellik standart Akamai fiyatlandırma katmanı ile kullanılabilir. 

**Akamai'den Azure CDN Standard** için medya akışı optimizasyonu, teslimat için tek tek ortam parçaları kullanan canlı veya isteğe bağlı video akış ortamları için etkilidir. Bu işlem, aşamalı karşıdan yükleme yoluyla veya bayt aralığı istekleri kullanılarak aktarılan tek bir büyük varlıktan farklıdır. Bu ortam teslim için bu şiçin büyük [dosya optimizasyonu](cdn-large-file-optimization.md)na bakın.

Genel ortam teslimi veya isteğe bağlı video medya teslimi optimizasyon uyruk türleri, ortam varlıklarını daha hızlı sunmak için arka uç optimizasyonları içeren bir CDN kullanır. Ayrıca, zaman içinde öğrenilen en iyi uygulamalara dayalı medya varlıkları için yapılandırmalar da kullanırlar.

### <a name="configure-an-akamai-cdn-endpoint-to-optimize-media-streaming"></a>Medya akışını optimize etmek için akamai CDN bitiş noktasını yapılandırma
 
Azure portalı üzerinden büyük dosyalar için teslimatı optimize etmek için içerik teslim ağınızı (CDN) bitiş noktasıolarak yapılandırabilirsiniz. Bunu yapmak için REST API'lerini veya istemci SDK'larından herhangi birini de kullanabilirsiniz. Aşağıdaki adımlar, **Akamai profilinden** bir Azure CDN Standardı için Azure portalı üzerinden işlemi gösterir:

1. Akamai **CDN profil** sayfasında yeni bir bitiş noktası eklemek için **Bitiş Noktası'nı**seçin.
  
    ![Yeni bitiş noktası](./media/cdn-media-streaming-optimization/cdn-new-akamai-endpoint.png)

2. Açılan için **Optimize Edilmiş** listede, isteğe bağlı video varlıkları için **isteğe bağlı medya akışında Video'yu** seçin. Canlı ve isteğe bağlı video akışı nın bir birleşimini yaparsanız, **Genel medya akışını**seçin.

    ![Seçili akış](./media/cdn-media-streaming-optimization/02_Creating.png) 
 
Bitiş noktasını oluşturduktan sonra, belirli ölçütlerile eşleşen tüm dosyalar için en iyi duruma getirilmesi ni uygular. Aşağıdaki bölümde bu işlem açıklanmaktadır. 

### <a name="caching"></a>Önbelleğe alma

**Akamai'deki Azure CDN Standardı** varlığın bir akış bildirimi veya parçası olduğunu algılarsa, genel web tesliminden farklı önbelleğe alma son kullanma süreleri kullanır. (Aşağıdaki tablodaki tam listeye bakın.) Her zaman olduğu gibi, önbellek denetimi veya kaynaktan gönderilen Sona Erer üstbellekleri onurlandırılır. Varlık bir ortam varlığı değilse, genel web teslimatı için son kullanma sürelerini kullanarak önbelleğe gelir.

Kısa negatif önbelleğe alma süresi, birçok kullanıcı henüz var olmayan bir parça istediğinde kaynak boşaltma için yararlıdır. Buna örnek olarak, paketlerin o anda kaynağından kullanılamadığı canlı akış adave verilmiştir. Video içeriği genellikle değiştirilmediği için, daha uzun önbelleğe alma aralığı da kaynaktan gelen istekleri nitreyi boşaltmaya yardımcı olur.
 

|   | Genel web teslimatı | Genel medya akışı | İsteğe bağlı video medya akışı  
--- | --- | --- | ---
Önbelleğe alma: Pozitif <br> HTTP 200, 203, 300, <br> 301, 302 ve 410 | 7 gün |365 gün | 365 gün   
Önbelleğe alma: Negatif <br> HTTP 204, 305, 404, <br> ve 405 | None | 1 saniye | 1 saniye
 
### <a name="deal-with-origin-failure"></a>Menşe hatasıyla başa çık  

Genel ortam teslimi ve isteğe bağlı video ortam teslimi, başlangıç zaman ayarı ve tipik istek desenleri için en iyi uygulamalara dayalı bir yeniden deneme günlüğüne de sahiptir. Örneğin, genel ortam teslimi canlı ve isteğe bağlı video ortam teslimi için olduğundan, canlı akışın zamana duyarlı yapısı nedeniyle daha kısa bir bağlantı zaman ayarı kullanır.

Bağlantı zaman ları bittiğinde, CDN istemciye "504 - Ağ Geçidi Zaman Kaçaması" hatası göndermeden önce birkaç kez yeniden çalışır. 

Bir dosya dosya türü ve boyut koşulları listesiyle eşleştiğinde, CDN ortam akışı davranışını kullanır. Aksi takdirde, genel web dağıtım kullanır.
   
### <a name="conditions-for-media-streaming-optimization"></a>Medya akışı optimizasyonu için koşullar 

Aşağıdaki tabloda, ortam akışı optimizasyonu için karşılanacak ölçütler kümesi listelenebilmelidir: 
 
Desteklenen akış türleri | Dosya uzantıları  
--- | ---  
Elma HLS | m3u8, m3u, m3ub, anahtar, ts, aac
Adobe HDS | f4m, f4x, drmmeta, bootstrap, f4f,<br>Seg-Frag URL yapısı <br> (eşleşen regex: ^(/.*)Seq(\d+)-Frag(\d+)
Demir | mpd, çizgi, divx, ismv, m4s, m4v, mp4, mp4v, <br> sidx, webm, mp4a, m4a, isma
Düzgün akış | /manifest/, /QualityLevels/Fragments/
  
 
