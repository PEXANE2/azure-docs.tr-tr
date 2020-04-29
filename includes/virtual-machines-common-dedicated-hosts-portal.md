---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/10/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 427117fe47294a1db1fa8d3fa1e46ee1efb91b4d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79128572"
---
## <a name="limitations"></a>Sınırlamalar

- Sanal Makine Ölçek Kümeleri Şu anda adanmış konaklarda desteklenmiyor.
- Adanmış konaklar için kullanılabilen Boyutlar ve donanım türleri bölgelere göre farklılık gösterir. Daha fazla bilgi edinmek için konak [fiyatlandırma sayfasına](https://aka.ms/ADHPricing) bakın.

## <a name="create-a-host-group"></a>Konak grubu oluştur

**Konak grubu** , adanmış Konakları koleksiyonunu temsil eden yeni bir kaynaktır. Bir bölge ve kullanılabilirlik bölgesinde bir konak grubu oluşturur ve bu gruba ana bilgisayar ekleyebilirsiniz. Yüksek kullanılabilirlik için planlama yaparken ek seçenekler vardır. Aşağıdaki seçeneklerden birini veya her ikisini de adanmış konaklarınız ile kullanabilirsiniz: 
- Birden çok kullanılabilirlik alanı arasında yayılır. Bu durumda, kullanmak istediğiniz her bölgede bir konak grubunuz olması gerekir.
- Fiziksel raflarla eşlenen birden çok hata etki alanı arasında yayılma. 
 
Her iki durumda da, konak grubunuz için hata etki alanı sayısını sağlamanız gerekir. Grubunuzdaki hata etki alanlarına yayılması istemiyorsanız, hata etki alanı sayısı 1 ' i kullanın. 

Hem kullanılabilirlik bölgelerini hem de hata etki alanlarını kullanmaya karar verebilirsiniz. 

Bu örnekte, 1 kullanılabilirlik alanı ve 2 hata etki alanı kullanarak bir konak grubu oluşturacağız. 


1. Azure [portalını](https://portal.azure.com)açın.
1. Sol üst köşedeki **kaynak oluştur** ' u seçin.
1. **Konak grubunu** arayın ve sonra sonuçlardan **konak grupları** ' nı seçin.
1. **Konak grupları** sayfasında **Oluştur**' u seçin.
1. Kullanmak istediğiniz aboneliği seçin ve yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' u seçin.
1. **Ad** olarak *Myayrılmış hostsrg* yazın ve ardından **Tamam**' ı seçin.
1. **Konak grubu adı**Için *myhostgroup*yazın.
1. **Konum**için **Doğu ABD**' yi seçin.
1. **Kullanılabilirlik bölgesi**için **1**' i seçin.
1. **Hata etki alanı sayısı**için **2**' yi seçin.
1. **Gözden geçir + oluştur** ' u seçin ve doğrulama için bekleyin.
1. **Doğrulama başarılı** iletisini gördüğünüzde, konak grubunu oluşturmak için **Oluştur** ' u seçin.

Konak grubunun oluşturulması yalnızca birkaç dakika sürer.

## <a name="create-a-dedicated-host"></a>Adanmış konak oluşturma

Şimdi konak grubunda ayrılmış bir konak oluşturun. Konak için bir ada ek olarak, ana bilgisayar için SKU sağlamanız gerekir. Ana bilgisayar SKU 'SU, desteklenen VM serisini ve adanmış ana bilgisayarınız için donanım oluşturmayı yakalar.

Konak SKU 'Ları ve fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure ayrılmış ana bilgisayar fiyatlandırması](https://aka.ms/ADHPricing).

Konak grubunuz için bir hata etki alanı sayısı ayarlarsanız, ana bilgisayarınız için hata etki alanını belirtmeniz istenir.  

1. Sol üst köşedeki **kaynak oluştur** ' u seçin.
1. **Adanmış ana bilgisayar** için arama yapın ve sonra sonuçlardan **adanmış konaklar** ' ı seçin.
1. **Adanmış konaklar** sayfasında **Oluştur**' u seçin.
1. Kullanmak istediğiniz aboneliği seçin.
1. **Kaynak grubu**olarak, *Myayrılmış hostsrg* öğesini seçin.
1. **Örnek ayrıntıları**' nda **ad** için *myhost* yazın ve konum için *Doğu ABD* seçin.
1. **Donanım profili**' nde, **Boyut ailesi**için *Standart Es3 Family-Type 1* ' i seçin, **konak grubu** için *myhostgroup* ' u seçin ve ardından **hata etki alanı**için *1* ' i seçin. Diğer alanları için varsayılan değerleri bırakın.
1. İşiniz bittiğinde, **gözden geçir + oluştur** ' u seçin ve doğrulama için bekleyin.
1. **Doğrulama başarılı** iletisini gördüğünüzde, konağı oluşturmak için **Oluştur** ' u seçin.


