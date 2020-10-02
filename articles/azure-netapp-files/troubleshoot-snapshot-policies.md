---
title: Azure NetApp Files için anlık görüntü ilkelerine sorun giderme | Microsoft Docs
description: Azure NetApp Files için anlık görüntü ilkesi yönetimi sorunlarını gidermenize yardımcı olabilecek hata iletileri ve çözümleri açıklanmaktadır.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/23/2020
ms.author: b-juche
ms.openlocfilehash: 6ba8b18876bdae2754a6a772ce3909ff2f5a71b7
ms.sourcegitcommit: b4f303f59bb04e3bae0739761a0eb7e974745bb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91651013"
---
# <a name="troubleshoot-snapshot-policies"></a>Anlık görüntü ilkelerinde sorun giderme

Bu makalede Azure NetApp Files anlık görüntü ilkelerini yönetirken karşılaşabileceğiniz hata senaryoları açıklanmaktadır. Ayrıca, sorunları gidermenize yardımcı olabilecek çözümler de sağlar.

## <a name="error-conditions-and-resolutions"></a>Hata koşulları ve çözümleri 

|     Hata koşulu    |     Çözüm    |
|-|-|
| Anlık görüntü ilkesi oluşturma geçersiz anlık görüntü ilkesi adıyla başarısız oluyor. | Anlık görüntü ilkesi adı geçersizse anlık görüntü ilkesi oluşturma sırasında bir hata oluştu. Anlık görüntü ilkesi adları için aşağıdaki yönergeler geçerlidir:  <ul><li> Anlık görüntü ilkesi adı ASCII olmayan veya özel karakterler içeremez. </li> <li> Anlık görüntü ilkesi adı bir harf veya sayı ile başlamalıdır ve yalnızca harf, rakam, alt çizgi (' _ ') ve kısa çizgi ('-') içerebilir. </li> <li> Anlık görüntü ilkesi adı 1 ile 64 karakter arasında olmalıdır.  </li></ul> Anlık görüntü ilkesi adını yönergelere göre gözden geçirin.  |
| Anlık görüntü ilkesi oluşturma geçersiz değerlerle başarısız oluyor. | Veya gibi bir alan için geçersiz bir değer girerseniz, Azure NetApp Files bir anlık görüntü ilkesi oluşturamaz `Number of snapshots to keep` `Minute on the hour to take snapshot` . Geçerli değerler aşağıdaki gibidir:  <ul><li>Değer geçerli bir sayı olmalıdır.</li> <li>Değer 0 ile 59 arasında olmalıdır.</li></ul> Alanlar için geçerli bir değer sağlandığından emin olun. | 
| Anlık görüntü ilkesi oluşturma hatayla başarısız oluyor `Total number of snapshots to keep exceeds 255` . | Her birimde [en fazla 255 anlık görüntü](azure-netapp-files-resource-limits.md)olabilir. En büyük değer, saatlik, günlük, haftalık ve aylık anlık görüntülerin toplamını içerir. <br> Değeri azaltın `Snapshots to keep` ve yeniden deneyin. |
| İlke bir birime atanırken hata ile başarısız olur `Total snapshot policy is over the max '255'` . | Her birimde [en fazla 255 anlık görüntü](azure-netapp-files-resource-limits.md)olabilir. Tüm isteğe bağlı, saatlik, günlük, haftalık ve aylık anlık görüntülerin toplamı en büyük değeri aşarsa bir hata oluşur. <br> Değeri azaltın `snapshots to keep` veya bazı isteğe bağlı anlık görüntüleri silip yeniden deneyin. | 

## <a name="next-steps"></a>Sonraki adımlar  

* [Anlık görüntü ilkelerini yönetme](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies)
