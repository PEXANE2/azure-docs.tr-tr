---
title: Azure NetApp Files ölçümleri | Microsoft Docs
description: Azure NetApp Files için ölçümleri açıklar.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79460441"
---
# <a name="metrics-for-azure-netapp-files"></a>Azure NetApp Files için ölçümler

Azure NetApp Files, ayrılan depolama, gerçek depolama alanı kullanımı, hacim ıOPS ve gecikme hakkında ölçümler sağlar. Bu ölçümleri çözümleyerek, NetApp hesaplarınızın kullanım deseninin ve birim performansının daha iyi şekilde öğrenilmesine sahip olabilirsiniz.  

## <a name="usage-metrics-for-capacity-pools"></a><a name="capacity_pools"></a>Kapasite havuzları için kullanım ölçümleri

<!-- 
- *Pool Provisioned Size*  
    The logical space (GiB) the capacity pool is provisioned with.  
    This size is the size you selected during capacity pool creation. 
--> 
- *Birim boyutuna ayrılan havuz*  
    Belirli bir kapasite havuzundaki birim kotasının (GiB) toplamı (diğer bir deyişle, kapasite havuzundaki birimlerin sağlanan boyutlarının toplamı).  
    Bu boyut, birim oluşturma sırasında seçtiğiniz boyutudur.  
- *Kullanılan havuz boyutu*  
    Bir kapasite havuzundaki birimlerde kullanılan mantıksal alan toplamı (GiB).  
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
- *Tüketilen birim boyutu*   
    Bir birimde (GiB) kullanılan toplam mantıksal alan.  
    Bu boyut, etkin dosya sistemleri ve anlık görüntüleri tarafından kullanılan mantıksal alanı içerir.  
- *Birim anlık görüntü boyutu*   
   Bir birimdeki anlık görüntüler tarafından kullanılan artımlı mantıksal alan.  

## <a name="performance-metrics-for-volumes"></a>Birimler için performans ölçümleri

- *AverageReadLatency*   
    Birimdeki ortalama okuma süresi (milisaniye).
- *AverageWriteLatency*   
    Birimdeki ortalama yazma süresi (milisaniye).
- *ReadIops*   
    Birime saniye başına okuma sayısı.
- *Writeıops*   
    Birime saniye başına yazma sayısı.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure NetApp Files’ın depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md)
* [Kapasite havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
* [Azure NetApp Files için birim oluşturma](azure-netapp-files-create-volumes.md)
