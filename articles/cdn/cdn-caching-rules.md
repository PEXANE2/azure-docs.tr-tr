---
title: Önbelleğe alma kurallarıyla Azure CDN önbelleğe alma davranışını denetleme | Microsoft Dokümanlar
description: Varsayılan önbellek sona erme davranışını hem genel olarak hem de URL yolu ve dosya uzantıları gibi koşullarla ayarlamak veya değiştirmek için CDN önbelleğe alma kurallarını kullanabilirsiniz.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: allensu
ms.openlocfilehash: 874ec75fb9173b6cee50bf8880510464fa13e9d2
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81254249"
---
# <a name="control-azure-cdn-caching-behavior-with-caching-rules"></a>Azure CDN önbelleğe alma davranışını önbelleğe alma kurallarıyla denetleme

> [!NOTE] 
> Önbelleğe alma kuralları yalnızca **Akamai profillerinden** Verizon ve Azure **CDN Standard'dan Azure CDN Standardı** için kullanılabilir. **Microsoft profillerinden Azure CDN için,** **Verizon profillerinden Azure CDN Premium** için Standart kurallar [altyapısını](cdn-standard-rules-engine-reference.md) kullanmanız gerekir, benzer işlevler için **Yönet** portalındaki Verizon Premium [kuralları altyapısını](cdn-rules-engine.md) kullanmanız gerekir.
 
Azure İçerik Teslim Ağı (CDN), dosyalarınızın önbelleğe nasıl önbelleğe alındığından denetlemenin iki yolunu sunar: 

- Önbelleğe alma kuralları: Bu makalede, hem genel olarak hem de URL yolu ve dosya uzantısı gibi özel koşullarla varsayılan önbellek sona erme davranışını ayarlamak veya değiştirmek için içerik teslim ağı (CDN) önbelleğe alma kurallarını nasıl kullanabileceğiniz açıklanmaktadır. Azure CDN iki tür önbelleğe alma kuralı sağlar:

   - Genel önbelleğe alma kuralları: Profilinizdeki her uç nokta için bir genel önbelleğe alma kuralı ayarlayabilirsiniz. Bu işlem uç noktaya yönelik tüm istekleri etkiler. Genel önbelleğe alma kuralı ayarlandığında tüm HTTP önbellek yönergesi üst bilgilerini geçersiz kılar.

   - Özel önbelleğe alma kuralları: Profilinizdeki her uç nokta için bir veya daha fazla özel önbelleğe alma kuralı ayarlayabilirsiniz. Özel önbelleğe alma kuralları ayarlandığında belirli yollar ve dosya uzantılarıyla eşleşir, sırasıyla işlenir ve genel önbelleğe alma kuralını geçersiz kılar. 

- Sorgu dize önbelleğe alma: Azure CDN'nin sorgu dizeleri olan istekler için önbelleğe alma şeklini ayarlayabilirsiniz. Bilgi için bkz. [Sorgu dizeleriyle Azure CDN önbelleğe alma davranışını denetle.](cdn-query-string.md) Dosya önbelleğe alınamazsa, önbelleğe alma kuralları ve CDN varsayılan davranışlarıtemel alınarak sorgu dize önbelleğe alma ayarı hiçbir etkisi yoktur.

Varsayılan önbelleğe alma davranışı ve önbelleğe alma üstbilgileri hakkında bilgi için [önbelleğe alma nın nasıl çalıştığını](cdn-how-caching-works.md)görün. 


## <a name="accessing-azure-cdn-caching-rules"></a>Azure CDN önbelleğe alma kurallarına erişim

1. Azure portalını açın, CDN profilini seçin ve ardından bir bitiş noktası seçin.

2. Ayarların altındaki sol bölmede **Önbelleğe alma kuralları**’nı seçin.

   ![CDN Önbelleğe alma kuralları düğmesi](./media/cdn-caching-rules/cdn-caching-rules-btn.png)

   **Önbelleğe alma kuralları** sayfası görüntülenir.

   ![CDN Önbelleğe alma kuralları sayfası](./media/cdn-caching-rules/cdn-caching-rules-page.png)


## <a name="caching-behavior-settings"></a>Önbelleğe alma davranış ayarları
Genel ve özel önbelleğe alma kuralları için aşağıdaki **Önbelleğe Alma davranış** ayarlarını belirtebilirsiniz:

- **Atlama önbelleği**: Önbellek ve kaynak sağlanan önbellek yönergesi üstbilgi yok saymak.

- **Geçersiz Kılma**: Kaynak tarafından sağlanan önbellek süresini yoksay; bunun yerine sağlanan önbellek süresini kullanın. Bu önbellek denetimini geçersiz kılmaz: önbellek yok.

- **Varsa ayarlayın**: Varsa, kaynak tarafından sağlanan önbellek yönergesi üstbilgilerini onurlandırın; aksi takdirde, sağlanan önbellek süresini kullanın.

![Genel önbelleğe alma kuralları](./media/cdn-caching-rules/cdn-global-caching-rules.png)

![Özel önbelleğe alma kuralları](./media/cdn-caching-rules/cdn-custom-caching-rules.png)

## <a name="cache-expiration-duration"></a>Önbellek son kullanma süresi
Genel ve özel önbelleğe alma kuralları için önbellek son kullanma süresini gün, saat, dakika ve saniye cinsinden belirtebilirsiniz:

- Geçersiz **Kılma** ve Ayarla için **Önbelleğe Alma davranış** ayarları **eksikse,** geçerli önbellek süreleri 0 saniye ile 366 gün arasında değişir. 0 saniyelik bir değer için, CDN içeriği önbelleğe alır, ancak her isteği kaynak sunucuyla yeniden onaylamalıdır.

- **Bypass önbellek** ayarı için önbellek süresi otomatik olarak 0 saniye olarak ayarlanır ve değiştirilemez.

## <a name="custom-caching-rules-match-conditions"></a>Özel önbelleğe alma kuralları koşulları eşer

Özel önbellek kuralları için iki eşleşme koşulu kullanılabilir:
 
- **Yol**: Bu durum, etki alanı adı hariç URL'nin yolu ile\*eşleşir ve joker karakter simgesini destekler ( ). Örneğin, _/myfile.html_, _/my/folder/*_ ve _/my/images/*.jpg_. Maksimum uzunluk 260 karakterdir.

- **Uzantı**: Bu koşul, istenen dosyanın dosya uzantısı ile eşleşir. Eşleşecek virgülle ayrılmış dosya uzantılarının bir listesini sağlayabilirsiniz. Örneğin, _.jpg_, _.mp3_, veya _.png_. En fazla uzantı sayısı 50 ve uzantı başına maksimum karakter sayısı 16'dır. 

## <a name="global-and-custom-rule-processing-order"></a>Genel ve özel kural işleme sırası
Genel ve özel önbelleğe alma kuralları aşağıdaki sırada işlenir:

- Genel önbelleğe alma kuralları varsayılan CDN önbelleğe alma davranışından (HTTP önbellek yönergesi üstbilgi ayarları) öncegelir. 

- Özel önbelleğe alma kuralları, geçerli oldukları genel önbelleğe alma kurallarından önce gelir. Özel önbelleğe alma kuralları yukarıdan aşağıya doğru sırayla işlenir. Diğer bir zamanda, bir istek her iki koşulla da eşleşirse, listenin altındaki kurallar listenin en üstündeki kurallardan önce gelir. Bu nedenle, listeye daha düşük belirli kurallar yerleştirmeniz gerekir.

**Örnek**:
- Genel önbelleğe alma kuralı: 
   - Önbelleğe alma davranışı: **Geçersiz kılma**
   - Önbellek son kullanma süresi: 1 gün

- Özel önbelleğe alma kuralı #1:
   - Maç durumu: **Yol**
   - Maç değeri: _/home/*_
   - Önbelleğe alma davranışı: **Geçersiz kılma**
   - Önbellek son kullanma süresi: 2 gün

- Özel önbelleğe alma kuralı #2:
   - Maç koşulu: **Uzatma**
   - Maç değeri: _.html_
   - Önbelleğe alma davranışı: **Eksikse ayarla**
   - Önbellek son kullanma süresi: 3 gün

Bu kurallar ayarlandığında, _ &lt;bitiş noktası ana&gt;bilgisayar adı_.azureedge.net/home/index.html için bir istek özel önbelleğe alma kuralı #2 tetikler, hangi ayarlanır: Eksik ve 3 **gün.** Bu nedenle, *index.html* `Cache-Control` dosyasıveya `Expires` HTTP üstbilgi varsa, bunlar onurlandırıldı; aksi takdirde, bu üstbilgi ayarlanmazsa, dosya 3 gün önbelleğe alınır.

> [!NOTE] 
> Kural değişikliğinden önce önbelleğe alınan dosyalar, başlangıç önbelleğe alınan süre ayarını korur. Önbellek sürelerini sıfırlamak için [dosyayı temizlemeniz](cdn-purge-endpoint.md)gerekir. 
>
> Azure CDN yapılandırma değişikliklerinin ağ da yayılması biraz zaman alabilir: 
> - **Akamai’den Azure CDN Standart** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. 
> - **Verizon profillerinden Azure CDN Standardı** için yayılma genellikle 10 dakika içinde tamamlar.  
>

## <a name="see-also"></a>Ayrıca bkz.

- [Önbelleğe alma nasıl işler?](cdn-how-caching-works.md)
- [Öğretici: Azure CDN önbelleğe alma kurallarını ayarlama](cdn-caching-rules-tutorial.md)
