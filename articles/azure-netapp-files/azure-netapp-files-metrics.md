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
ms.date: 08/06/2019
ms.author: b-juche
ms.openlocfilehash: 9a56fb27fdf9e196291942041d68b249d7f16648
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68839242"
---
# <a name="metrics-for-azure-netapp-files"></a>Azure NetApp Files için ölçümler

Azure NetApp Files, ayrılan depolama, gerçek depolama alanı kullanımı, birim üretimi, ıOPS ve gecikme hakkında ölçümler sağlar. Bu ölçümleri çözümleyerek, NetApp hesaplarınızın kullanım deseninin ve birim performansının daha iyi şekilde öğrenilmesine sahip olabilirsiniz.  

## <a name="capacity_pools"></a>Kapasite havuzları için kullanım ölçümleri

<!-- 
- *Volume pool allocated size*  
    This is the size (GiB) of the provisioned capacity pool.  
--> 
- *Ayrılan birim havuzu*  
    Bu, belirli bir kapasite havuzundaki birim kotasının (GiB) toplamıdır (diğer bir deyişle, kapasite havuzundaki birimlerin sağlanan boyutlarının toplamıdır). Bu, birim oluşturma sırasında seçtiğiniz boyutudur.  
- *Birim havuzu toplam mantıksal boyut*  
    Bu, bir kapasite havuzundaki birimler genelinde kullanılan mantıksal alanın (GiB) toplamıdır.  
<!-- 
- *Volume pool total snapshot size*  
    This is the total of incremental logical space used by the snapshots.  
-->

## <a name="volumes"></a>Birimler için kullanım ölçümleri

<!-- 
- *Volume allocated size*   
    This is the volume size (quota) provisioned in GiB.  
--> 
- *Birim mantıksal boyutu*   
    Bu, bir birimde (GiB) kullanılan toplam mantıksal alandır. Bu boyut, etkin dosya sistemleri ve anlık görüntüleri tarafından kullanılan mantıksal alanı içerir.  
- *Birim anlık görüntü boyutu*   
    Bu, bir birimdeki anlık görüntüler tarafından kullanılan artımlı mantıksal alandır.  

## <a name="next-steps"></a>Sonraki adımlar

* [Azure NetApp Files depolama hiyerarşisini anlayın](azure-netapp-files-understand-storage-hierarchy.md)
* [Kapasitesi havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
* [Azure NetApp Files için birim oluşturma](azure-netapp-files-create-volumes.md)
