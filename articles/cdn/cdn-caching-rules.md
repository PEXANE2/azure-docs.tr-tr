---
title: Önbelleğe alma kurallarıyla denetim Azure CDN önbelleğe alma davranışı | Microsoft Docs
description: CDN önbelleğe alma kurallarını, hem genel olarak hem de bir URL yolu ve dosya uzantıları gibi koşullara sahip olan varsayılan önbellek süre sonu davranışını ayarlamak veya değiştirmek için kullanabilirsiniz.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 03/19/2019
ms.author: allensu
ms.openlocfilehash: 1f30943eb0cc72f677785d1228b47b65764c1e7d
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84887865"
---
# <a name="control-azure-cdn-caching-behavior-with-caching-rules"></a>Azure CDN önbelleğe alma davranışını önbelleğe alma kurallarıyla denetleme

> [!NOTE] 
> Önbelleğe alma kuralları yalnızca Verizon **Azure CDN ve Akamai** profillerindeki standart **Azure CDN Standart** için kullanılabilir. **Microsoft profillerinin Azure CDN** Için, Verizon profiller **'den Azure CDN Premium** için [standart kurallar altyapısını](cdn-standard-rules-engine-reference.md) kullanmanız gerekir, benzer Işlevler için **Manage** Portal 'daki [Verizon Premium Rules altyapısını](cdn-rules-engine.md) kullanmanız gerekir.
 
Azure Content Delivery Network (CDN), dosyalarınızın nasıl önbelleğe alınacağını denetlemek için iki yol sunar: 

- Önbelleğe alma kuralları: Bu makalede, hem genel olarak hem de bir URL yolu ve dosya uzantısı gibi özel koşullara sahip varsayılan önbellek süre sonu davranışını ayarlamak veya değiştirmek için Content Delivery Network (CDN) önbelleğe alma kurallarını nasıl kullanabileceğiniz açıklanır. Azure CDN iki tür önbelleğe alma kuralı sağlar:

   - Genel önbelleğe alma kuralları: Profilinizdeki her uç nokta için bir genel önbelleğe alma kuralı ayarlayabilirsiniz. Bu işlem uç noktaya yönelik tüm istekleri etkiler. Genel önbelleğe alma kuralı ayarlandığında tüm HTTP önbellek yönergesi üst bilgilerini geçersiz kılar.

   - Özel önbelleğe alma kuralları: Profilinizdeki her uç nokta için bir veya daha fazla özel önbelleğe alma kuralı ayarlayabilirsiniz. Özel önbelleğe alma kuralları ayarlandığında belirli yollar ve dosya uzantılarıyla eşleşir, sırasıyla işlenir ve genel önbelleğe alma kuralını geçersiz kılar. 

- Sorgu dizesi önbelleğe alma: Azure CDN sorgu dizelerine sahip istekler için önbelleğe almayı nasıl değerlendirip ayarlayabelirtebilirsiniz. Bilgi için bkz. [sorgu dizeleri Ile denetim Azure CDN önbelleğe alma davranışı](cdn-query-string.md). Dosya önbelleklenebilir değilse, sorgu dizesi önbelleğe alma ayarının önbelleğe alma kuralları ve CDN varsayılan davranışlarına göre hiçbir etkisi yoktur.

Varsayılan önbelleğe alma davranışı ve önbelleğe alma yönergesi üstbilgileri hakkında daha fazla bilgi için bkz. [önbelleğe alma nasıl kullanılır](cdn-how-caching-works.md). 


## <a name="accessing-azure-cdn-caching-rules"></a>Azure CDN önbelleğe alma kurallarına erişme

1. Azure portal açın, bir CDN profili seçin ve ardından bir uç nokta seçin.

2. Ayarların altındaki sol bölmede **Önbelleğe alma kuralları**’nı seçin.

   ![CDN Önbelleğe alma kuralları düğmesi](./media/cdn-caching-rules/cdn-caching-rules-btn.png)

   **Önbelleğe alma kuralları** sayfası görüntülenir.

   ![CDN Önbelleğe alma kuralları sayfası](./media/cdn-caching-rules/cdn-caching-rules-page.png)


## <a name="caching-behavior-settings"></a>Önbelleğe alma davranışı ayarları
Genel ve özel önbelleğe alma kuralları için aşağıdaki **önbelleğe alma davranışı** ayarlarını belirtebilirsiniz:

- **Atlama önbelleği**: Origin tarafından belirtilen Cache-Directive üst bilgilerini önbelleğe almayın ve yoksayın.

- **Geçersiz kıl**: Origin tarafından belirtilen önbellek süresini yoksay; Bunun yerine, belirtilen önbellek süresini kullanın. Bu, Cache-Control: No-Cache ' i geçersiz kılmaz.

- **Eksik Ise ayarla**: varsa, kaynak tarafından sağlanmış Cache-Directive üst bilgilerini kabul edin. Aksi takdirde, belirtilen önbellek süresini kullanın.

![Genel önbelleğe alma kuralları](./media/cdn-caching-rules/cdn-global-caching-rules.png)

![Özel önbelleğe alma kuralları](./media/cdn-caching-rules/cdn-custom-caching-rules.png)

## <a name="cache-expiration-duration"></a>Önbellek sona erme süresi
Genel ve özel önbelleğe alma kuralları için önbelleğin süre sonu süresini gün, saat, dakika ve saniye cinsinden belirtebilirsiniz:

- **Geçersiz kılma** için, **eksik** **önbelleğe alma davranışı** ayarları varsa, geçerli önbellek süreleri 0 saniye ile 366 gün arasında değişir. 0 saniyelik bir değer için CDN, içeriği önbelleğe alır, ancak her isteği kaynak sunucu ile yeniden doğrulaması gerekir.

- Önbelleği **atlama** ayarı için önbellek süresi otomatik olarak 0 saniyeye ayarlanır ve değiştirilemez.

## <a name="custom-caching-rules-match-conditions"></a>Özel önbelleğe alma kuralları koşullara uymuyor

Özel önbellek kuralları için, iki eşleşme koşulu vardır:
 
- **Yol**: Bu koşul, etki alanı adı hariç olmak üzere URL 'nin yoluyla eşleşir ve joker karakter simgesini () destekler \* . Örneğin, _/myfile.html_, _/My/Folder/*_ ve _/My/images/*. jpg_. En fazla 260 karakter uzunluğunda olur.

- **Uzantı**: Bu koşul, istenen dosyanın dosya uzantısıyla eşleşiyor. Eşleştirilecek virgülle ayrılmış dosya uzantılarının bir listesini sağlayabilirsiniz. Örneğin, _. jpg_, _. mp3_veya _. png_. En fazla uzantı sayısı 50, uzantı başına en fazla karakter sayısı 16 ' dır. 

## <a name="global-and-custom-rule-processing-order"></a>Genel ve özel kural işleme sırası
Genel ve özel önbelleğe alma kuralları aşağıdaki sırayla işlenir:

- Genel önbelleğe alma kuralları varsayılan CDN önbelleğe alma davranışına (HTTP Cache-Directive üstbilgi ayarları) göre önceliklidir. 

- Özel önbelleğe alma kuralları, uygulanan genel önbelleğe alma kurallarına göre önceliklidir. Özel önbelleğe alma kuralları yukarıdan aşağıya doğru sırayla işlenir. Diğer bir deyişle, bir istek her iki koşuldan de eşleşiyorsa, listenin altındaki kurallar listenin en üstündeki kurallara göre önceliklidir. Bu nedenle, listede daha az belirli kurallar yerleştirmeniz gerekir.

**Örnek**:
- Genel önbelleğe alma kuralı: 
   - Önbelleğe alma davranışı: **geçersiz kıl**
   - Önbellek sona erme süresi: 1 gün

- Özel önbelleğe alma kuralı #1:
   - Eşleşme koşulu: **yol**
   - Eşleşme değeri: _/Home/*_
   - Önbelleğe alma davranışı: **geçersiz kıl**
   - Önbellek sona erme süresi: 2 gün

- Özel önbelleğe alma kuralı #2:
   - Eşleşme koşulu: **uzantı**
   - Eşleşme değeri: _. html_
   - Önbelleğe alma davranışı: **eksikse ayarla**
   - Önbellek sona erme süresi: 3 gün

Bu kurallar ayarlandığında, _ &lt; uç nokta ana bilgisayar adı &gt; _. azureedge.net/Home/index.html için bir istek özel önbelleğe alma kuralı #2 tetikler: eksik ve 3 gün olarak **ayarlanır** . Bu nedenle, *index.html* dosyası `Cache-Control` veya `Expires` http üstbilgileri varsa, bunlar kabul edilir; Aksi takdirde, bu üst bilgiler ayarlanmamışsa, dosya 3 gün boyunca önbelleğe alınır.

> [!NOTE] 
> Bir kural değişikliğinden önce önbelleğe alınan dosyalar, kaynak önbellek süresi ayarını korur. Önbellek sürelerini sıfırlamak için [dosyayı temizlemeniz](cdn-purge-endpoint.md)gerekir. 
>
> Azure CDN yapılandırma değişikliklerinin ağ üzerinden yayılması biraz zaman alabilir: 
> - **Akamai’den Azure CDN Standart** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. 
> - **Verizon profillerinden Azure CDN Standart** için, yayma genellikle 10 dakika içinde tamamlanır.  
>

## <a name="see-also"></a>Ayrıca bkz.

- [Önbelleğe alma nasıl işler?](cdn-how-caching-works.md)
- [Öğretici: Azure CDN önbelleğe alma kurallarını ayarlama](cdn-caching-rules-tutorial.md)
