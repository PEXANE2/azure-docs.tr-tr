---
title: Azure NetApp Files ölçümleri | Microsoft Docs
description: Azure NetApp Files, ayrılan depolama, gerçek depolama alanı kullanımı, hacim ıOPS ve gecikme hakkında ölçümler sağlar. Kullanım ve performansı anlamak için bu ölçümleri kullanın.
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
ms.date: 09/10/2020
ms.author: b-juche
ms.openlocfilehash: 1690a844ff700a2975be8e972fd90ba71eeb937c
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90707790"
---
# <a name="metrics-for-azure-netapp-files"></a>Azure NetApp Files için ölçümler

Azure NetApp Files, ayrılan depolama, gerçek depolama alanı kullanımı, hacim ıOPS ve gecikme hakkında ölçümler sağlar. Bu ölçümleri çözümleyerek, NetApp hesaplarınızın kullanım deseninin ve birim performansının daha iyi şekilde öğrenilmesine sahip olabilirsiniz.  

## <a name="usage-metrics-for-capacity-pools"></a><a name="capacity_pools"></a>Kapasite havuzları için kullanım ölçümleri

- *Havuza ayrılan boyut*   
    Havuzun sağlanan boyutu.

- *Birim boyutuna ayrılan havuz*  
    Belirli bir kapasite havuzundaki birim kotasının (GiB) toplamı (diğer bir deyişle, kapasite havuzundaki birimlerin sağlanan boyutlarının toplamı).  
    Bu boyut, birim oluşturma sırasında seçtiğiniz boyutudur.  

- *Kullanılan havuz boyutu*  
    Bir kapasite havuzundaki birimlerde kullanılan mantıksal alan toplamı (GiB).  

- *Havuzun toplam anlık görüntü boyutu*    
    Havuzdaki tüm birimlerin anlık görüntü boyutu toplamı.

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

- *Ortalama okuma gecikmesi*   
    Birimdeki ortalama okuma süresi (milisaniye).
- *Ortalama yazma gecikmesi*   
    Birimdeki ortalama yazma süresi (milisaniye).
- *IOPS 'yi oku*   
    Birime saniye başına okuma sayısı.
- *IOPS yaz*   
    Birime saniye başına yazma sayısı.

## <a name="volume-replication-metrics"></a><a name="replication"></a>Birim çoğaltma ölçümleri

- *Birim çoğaltma durumu sağlıklı*   
    Çoğaltma ilişkisinin koşulu. 

- *Birim çoğaltma aktarılıyor*    
    Birim çoğaltmasının durumunun ' aktarma ' olup olmadığı. 
 
- *Birim çoğaltma gecikme süresi*   
    Yansıtmadaki verilerin kaynağın arkasında olduğu sürenin saniye cinsinden miktarı. 

- *Birim çoğaltma son aktarım süresi*   
    Son aktarımın tamamlanması için geçen saniye cinsinden süre. 

- *Birim çoğaltma son aktarım boyutu*    
    Son aktarımın bir parçası olarak aktarılan toplam bayt sayısı. 

- *Birim çoğaltma ilerleme durumu*    
    Geçerli aktarım işlemi için aktarılan toplam veri miktarı. 

- *Birim çoğaltma toplam aktarımı*   
    İlişki için aktarılan birikmeli baytlar. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure NetApp Files’ın depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md)
* [Kapasite havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
* [Azure NetApp Files için birim oluşturma](azure-netapp-files-create-volumes.md)
