---
title: Azure NetApp Dosyaları için Ölçümler | Microsoft Dokümanlar
description: Azure NetApp Dosyaları ölçümlerini açıklar.
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
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: b-juche
ms.openlocfilehash: c8e3b616dee1ab4e6bb6e77c6a8bab5661d4e20b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460441"
---
# <a name="metrics-for-azure-netapp-files"></a>Azure NetApp Files için ölçümler

Azure NetApp Files, ayrılan depolama, gerçek depolama kullanımı, birim IOPS ve gecikme gecikmesi ölçümleri sağlar. Bu ölçümleri analiz ederek, NetApp hesaplarınızın kullanım deseni ve ses performansı hakkında daha iyi bir anlayış elde edebilirsiniz.  

## <a name="usage-metrics-for-capacity-pools"></a><a name="capacity_pools"></a>Kapasite havuzları için kullanım ölçümleri

<!-- 
- *Pool Provisioned Size*  
    The logical space (GiB) the capacity pool is provisioned with.  
    This size is the size you selected during capacity pool creation. 
--> 
- *Hacim Boyutuna Ayrılan Havuz*  
    Belirli bir kapasite havuzundaki hacim kotasının (GiB) toplamı (diğer bir deyişle, birimlerin kapasite havuzundaki sağlanan boyutların toplamı).  
    Bu boyut, birim oluşturma sırasında seçtiğiniz boyut.  
- *Havuz Tüketilen Boyut*  
    Kapasite havuzunda birimler arasında kullanılan mantıksal alanın (GiB) toplamı.  
<!-- 
- *Pool Consumed Snapshot Size*  
    The total of logical space (GiB) used by snapshots across all volumes in a capacity pool. 
-->

## <a name="usage-metrics-for-volumes"></a><a name="volumes"></a>Birimler için kullanım ölçümleri

<!--
- *Volume Quota Size*    
    The quota size (GiB) the volume is provisioned with.   
    This size is the size you selected during capacity pool creation. 
-->
- *Hacim Tüketilen Boyut*   
    Bir birimde (GiB) kullanılan toplam mantıksal alan.  
    Bu boyut, etkin dosya sistemleri ve anlık görüntüler tarafından kullanılan mantıksal alanı içerir.  
- *Birim Anlık Görüntü Boyutu*   
   Bir birimdeki anlık görüntüler tarafından kullanılan artımlı mantıksal alan.  

## <a name="performance-metrics-for-volumes"></a>Birimler için performans ölçümleri

- *AverageReadLatency*   
    Birimden milisaniye cinsinden okumaların ortalama süresi.
- *AverageWriteLatency*   
    Birimden milisaniye cinsinden yazmanın ortalama süresi.
- *ReadIops*   
    Saniyede ses egöre okuma sayısı.
- *Yazma*   
    Saniyede hacim için yazma sayısı.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure NetApp Files’ın depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md)
* [Kapasitesi havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
* [Azure NetApp Files için birim oluşturma](azure-netapp-files-create-volumes.md)
