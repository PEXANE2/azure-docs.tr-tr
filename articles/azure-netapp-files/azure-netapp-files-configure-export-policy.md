---
title: NFS için dışarı aktarma ilkesini yapılandırma birim-Azure NetApp Files
description: Azure NetApp Files kullanarak bir NFS birimine erişimi denetlemek için, dışa aktarma ilkesinin nasıl yapılandırılacağını açıklar.
services: azure-netapp-files
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: how-to
ms.date: 07/27/2020
ms.openlocfilehash: 4a20a223932f82c80ad5831ef3a02bad803e26e6
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87533231"
---
# <a name="configure-export-policy-for-an-nfs-volume"></a>NFS birimine yönelik dışarı aktarma ilkesini yapılandırma

Azure NetApp Files birime erişimi denetlemek için, dışa aktarma ilkesini yapılandırabilirsiniz. Azure NetApp Files dışa aktarma ilkesi, NFS protokolünü (hem NFSv3 hem de NFSv 4.1) ve ikili Protokolü (NFSv3 ve SMB) kullanan birimleri destekler. 

En fazla beş dışarı aktarma ilkesi kuralı oluşturabilirsiniz.

## <a name="steps"></a>Adımlar 

1.  Birimler sayfasında, dışarı aktarma ilkesini yapılandırmak istediğiniz birimi seçin ve **Ilkeyi dışarı aktar**' a tıklayın. 

    Ayrıca, birim oluşturma sırasında dışarı aktarma ilkesini yapılandırabilirsiniz.

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

    * **Salt okunurdur** ve **okuma/yazma**  
        NFSv 4.1 ile Kerberos Şifrelemesi kullanırsanız, [NFSv 4.1 Kerberos şifrelemesini yapılandırma](configure-kerberos-encryption.md)bölümündeki yönergeleri izleyin.  Kerberos 'un performans etkisi için bkz. [NFSv 4.1 üzerinde Kerberos 'un performans etkisi](configure-kerberos-encryption.md#kerberos_performance). 

        ![Kerberos güvenlik seçenekleri](../media/azure-netapp-files/kerberos-security-options.png) 

    * **Kök erişim**  
        `root`Hesabın birime erişip erişemeyeceğini belirtin.  Varsayılan olarak, kök erişim **Açık**olarak ayarlanır ve `root` hesabın birime erişimi vardır.

![İlkeyi dışarı aktarma](../media/azure-netapp-files/azure-netapp-files-export-policy.png) 



## <a name="next-steps"></a>Sonraki adımlar 
* [Sanal makineler için birimi bağlama veya ayırma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Anlık görüntüleri yönetme](azure-netapp-files-manage-snapshots.md)
