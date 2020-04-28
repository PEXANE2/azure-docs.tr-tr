---
title: NFS için dışarı aktarma ilkesini yapılandırma birim-Azure NetApp Files
description: Azure NetApp Files kullanarak bir NFS birimine erişimi denetlemek için, dışa aktarma ilkesinin nasıl yapılandırılacağını açıklar.
services: azure-netapp-files
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: conceptual
ms.date: 10/18/2019
ms.openlocfilehash: b96fca3a5627a1c6c96c8db5c1c209a51c5e102a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75551567"
---
# <a name="configure-export-policy-for-an-nfs-volume"></a>NFS birimine yönelik dışarı aktarma ilkesini yapılandırma

İsteğe bağlı olarak Azure NetApp Files birimine erişimi denetlemek için dışarı aktarma ilkesi yapılandırabilirsiniz. Azure NetApp Files dışarı aktarma ilkesi yalnızca NFS birimlerini destekler.  Hem NFSv3 hem de NFSv4 desteklenir. 

## <a name="steps"></a>Adımlar 

1.  Azure NetApp Files gezinti bölmesinden **Ilkeyi dışarı aktar** ' a tıklayın. 

2.  İlke kuralını oluşturmak veya dışarı aktarmak için aşağıdaki alanların bilgilerini belirtin:   
    *  **İndeks**   
        Kuralın dizin numarasını belirtin.  
        Dışarı aktarma ilkesi en fazla beş kural içerir. Kurallar, dizin numaraları listesindeki sıraya göre değerlendirilir. Dizin numarası en düşük olanlar ilk değerlendirilir. Örneğin dizin numarası 1 olan kural dizin numarası 2 olan kuraldan önce değerlendirilir. 

    * **İzin verilen Istemciler**   
        Değeri belirtirken aşağıdaki biçimlerden birini kullanın:  
        * IPv4 adresi, örneğin `10.1.12.24` 
        * Bit numaraları olarak belirtilen alt ağ maskesine sahip IPv4 adresi, örneğin `10.1.12.10/4`

    * **Erişim**  
        Aşağıdaki erişim türlerinden birini seçin:  
        * Erişim Yok 
        * Okuma ve Yazma
        * Salt Okunur

    ![İlkeyi dışarı aktarma](../media/azure-netapp-files/azure-netapp-files-export-policy.png) 


## <a name="next-steps"></a>Sonraki adımlar 
* [Birimleri yönetme](azure-netapp-files-manage-volumes.md)
* [Sanal makineler için birimi bağlama veya ayırma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Anlık görüntüleri yönetme](azure-netapp-files-manage-snapshots.md)
