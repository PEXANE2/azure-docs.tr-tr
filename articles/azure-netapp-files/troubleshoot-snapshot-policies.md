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
ms.openlocfilehash: 1d7a91de8fa505fe4c2b06eea59f3acc2ae1f7e8
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91345500"
---
# <a name="troubleshoot-snapshot-policies"></a>Anlık görüntü ilkelerine sorun giderme

Bu makalede Azure NetApp Files anlık görüntü ilkelerini yönetirken karşılaşabileceğiniz hata senaryoları açıklanmaktadır. Ayrıca, sorunları gidermenize yardımcı olabilecek çözümler de sağlar.

## <a name="snapshot-policy-creation-failing-with-invalid-snapshot-policy-name"></a>Anlık görüntü ilkesi oluşturma geçersiz anlık görüntü ilkesi adıyla başarısız oluyor

Anlık görüntü ilkesi adı geçersizse anlık görüntü ilkesi oluşturma sırasında bir hata oluştu. Anlık görüntü ilkesi adları için aşağıdaki yönergeler geçerlidir:  

* Anlık görüntü ilkesi adı ASCII olmayan veya özel karakterler içeremez.
* Anlık görüntü ilkesi adı bir harf veya sayı ile başlamalıdır ve yalnızca harf, rakam, alt çizgi (' _ ') ve kısa çizgi ('-') içerebilir.
* Anlık görüntü ilkesi adı 1 ile 64 karakter arasında olmalıdır.  

Anlık görüntü ilkesi adını yönergelere göre düzeltmeniz gerekir.  

## <a name="snapshot-policy-creation-failing-with-invalid-values"></a>Anlık görüntü ilkesi oluşturma geçersiz değerlerle başarısız oluyor 

Veya gibi bir alan için geçersiz bir değer girerseniz, Azure NetApp Files bir anlık görüntü ilkesi oluşturamaz `Number of snapshots to keep` `Minute on the hour to take snapshot` .  
 
Geçerli değerler aşağıdaki gibidir:   

* Değer geçerli bir sayı olmalıdır.
* Değer 0 ile 59 arasında olmalıdır.

Alanlar için geçerli bir değer sağlandığından emin olun.

## <a name="snapshot-policy-creation-failing-with-total-number-of-snapshots-to-keep-exceeds-255-error"></a>Anlık görüntü ilkesi oluşturma, "tutulacak toplam anlık görüntü sayısı 255 ' i aşıyor" hatası ile başarısız oluyor 

Her birimde [en fazla 255 anlık görüntü](azure-netapp-files-resource-limits.md)olabilir. En büyük değer, saatlik, günlük, haftalık ve aylık anlık görüntülerin toplamını içerir. Değeri azaltmalı `Snapshots to keep` ve yeniden denemeniz gerekir.

## <a name="assigning-policy-to-a-volume-failing-with-total-snapshot-policy-is-over-the-max-255-error"></a>"Toplam Snapshot Policy en fazla ' 255 ' hatası ile başarısız olan bir birime ilke atama

Her birimde [en fazla 255 anlık görüntü](azure-netapp-files-resource-limits.md)olabilir. Tüm isteğe bağlı, saatlik, günlük, haftalık ve aylık anlık görüntülerin toplamı en büyük değeri aşarsa bir hata oluşur. Değeri azaltın `snapshots to keep` veya bazı isteğe bağlı anlık görüntüleri silip yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar  

* [Anlık görüntü ilkelerini yönetme](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies)
