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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128572"
---
## <a name="limitations"></a>Sınırlamalar

- Sanal makine ölçek kümeleri şu anda özel ana bilgisayarlarda desteklenmez.
- Özel ana bilgisayarlar için kullanılabilen boyutlar ve donanım türleri bölgeye göre değişir. Daha fazla bilgi edinmek için ana bilgisayar [fiyatlandırma sayfasına](https://aka.ms/ADHPricing) bakın.

## <a name="create-a-host-group"></a>Ana bilgisayar grubu oluşturma

**Ana bilgisayar grubu,** özel ana bilgisayar koleksiyonunu temsil eden yeni bir kaynaktır. Bir bölgede ve kullanılabilirlik bölgesinde bir ana bilgisayar grubu oluşturur ve ona ana bilgisayarlar eklersiniz. Yüksek kullanılabilirlik için planlama yaparken, ek seçenekler vardır. İlgili ana bilgisayarlarınızla aşağıdaki seçeneklerden birini veya her ikisini de kullanabilirsiniz: 
- Birden çok kullanılabilirlik bölgesine yayılma. Bu durumda, kullanmak istediğiniz bölgelerin her birinde bir ana bilgisayar grubuna sahip olması gerekir.
- Fiziksel raflara eşlenen birden çok hata etki alanına yayılma alanı. 
 
Her iki durumda da, ana bilgisayar grubunuz için hata etki alanı sayısını sağlamanız gerekir. Grubunuzdaki hata etki alanlarını yayılımını istemiyorsanız, 1 hata etki alanı sayısı kullanın. 

Ayrıca, hem kullanılabilirlik bölgelerini hem de hata etki alanlarını kullanmaya karar verebilirsiniz. 

Bu örnekte, 1 kullanılabilirlik bölgesi ve 2 hata etki alanı kullanarak bir ana bilgisayar grubu oluştururuz. 


1. Azure [portalını](https://portal.azure.com)açın.
1. Sol üst köşede **kaynak oluştur'u** seçin.
1. Ana **Bilgisayar grubunu** arayın ve ardından sonuçlardan **Ana Bilgisayar Grupları'nı** seçin.
1. Ana **Bilgisayar Grupları** sayfasında **Oluştur'u**seçin.
1. Kullanmak istediğiniz aboneliği seçin ve ardından yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** seçin.
1. **Ad** olarak *myDedicatedHostsRG* yazın ve ardından **Tamam'ı**seçin.
1. **Ana Bilgisayar grubu adı için** *myHostGroup*yazın.
1. **Konum**için Doğu **ABD'yi**seçin.
1. **Kullanılabilirlik Bölgesi**için **1'** i seçin.
1. **Hata etki alanı sayısı**için **2'yi**seçin.
1. **Gözden Geçir + oluştur'u** seçin ve ardından doğrulamayı bekleyin.
1. **Doğrulama geçti** iletisini gördüğünüzde, ana bilgisayar grubu oluşturmak için **Oluştur'u** seçin.

Ana bilgisayar grubunu oluşturmak yalnızca birkaç dakika sürer.

## <a name="create-a-dedicated-host"></a>Özel bir ana bilgisayar oluşturma

Şimdi ev sahibi grubunda özel bir ana bilgisayar oluşturun. Ev sahibi nin adına ek olarak, ev sahibi için SKU sağlamanız gerekir. Ana bilgisayar SKU, desteklenen VM serisinin yanı sıra özel ana bilgisayarınız için donanım üretimini de yakalar.

Ana bilgisayar SK'leri ve fiyatlandırması hakkında daha fazla bilgi için [Azure Özel Ana Bilgisayar fiyatlandırması'na](https://aka.ms/ADHPricing)bakın.

Ana bilgisayar grubunuz için bir hata etki alanı sayısı ayarlarsanız, ev sahibiniz için hata etki alanını belirtmeniz istenir.  

1. Sol üst köşede **kaynak oluştur'u** seçin.
1. Özel **ana bilgisayar** arama ve ardından sonuçlardan **Özel ana bilgisayarları** seçin.
1. Özel **Ana Bilgisayarlar** sayfasında **Oluştur'u**seçin.
1. Kullanmak istediğiniz aboneliği seçin.
1. **Kaynak grubu**olarak *myDedicatedHostsRG'yi* seçin.
1. **Örnek ayrıntıları,** **Ad** için *myHost* yazın ve konum için *Doğu ABD* seçin.
1. **Donanım profilinde,** Standart Es3 ailesini seçin - **Boyut ailesi**için Type *1,* **Ana Bilgisayar grubu** için *myHostGroup'u* seçin ve ardından **Hata etki alanı**için *1'i* seçin. Varsayılanları alanların geri kalanı için bırakın.
1. İşi bittiğinde, **Gözden Geçir + oluştur'u** seçin ve doğrulamayı bekleyin.
1. **Doğrulama geçti** iletisini gördüğünüzde, ana bilgisayarı oluşturmak için **Oluştur'u** seçin.


