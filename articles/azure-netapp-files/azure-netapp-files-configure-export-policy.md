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
ms.openlocfilehash: d3f268a4ce2660350055367770e987a06828e2d7
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101740129"
---
# <a name="configure-export-policy-for-an-nfs-volume"></a>NFS birimine yönelik dışarı aktarma ilkesini yapılandırma

Azure NetApp Files birime erişimi denetlemek için, dışa aktarma ilkesini yapılandırabilirsiniz. Azure NetApp Files dışa aktarma ilkesi, NFS protokolünü (hem NFSv3 hem de NFSv 4.1) ve ikili Protokolü (NFSv3 ve SMB) kullanan birimleri destekler. 

En fazla beş dışarı aktarma ilkesi kuralı oluşturabilirsiniz.

## <a name="configure-the-policy"></a>İlkeyi yapılandırma 

1.  **Birimler** sayfasında, dışarı aktarma ilkesini yapılandırmak istediğiniz birimi seçin ve ardından **ilkeyi dışarı aktar**' ı seçin. Ayrıca, birim oluşturma sırasında dışarı aktarma ilkesini yapılandırabilirsiniz.

2.  Dışarı aktarma ilkesi kuralı oluşturmak için aşağıdaki bilgileri belirtin:   
    * **Dizin**: kuralın Dizin numarasını belirtin.  
      
      Dışarı aktarma ilkesi en fazla beş kural içerir. Kurallar, dizin numaraları listesindeki sıraya göre değerlendirilir. Dizin numarası en düşük olanlar ilk değerlendirilir. Örneğin dizin numarası 1 olan kural dizin numarası 2 olan kuraldan önce değerlendirilir. 

    * **Izin verilen istemciler**: değeri aşağıdaki biçimlerden birinde belirtin:  
      * IPv4 adresinizi arayın. Örnek: `10.1.12.24`
      * Bir alt ağ maskesi olan IPv4 adresi bir bit sayısı olarak ifade edilir. Örnek: `10.1.12.10/4`
      * Virgülle ayrılmış IP adresleri. Birden çok konak IP 'yi virgülle ayırarak tek bir kurala girebilirsiniz. Örnek: `10.1.12.25,10.1.12.28,10.1.12.29`

    * **Erişim**: aşağıdaki erişim türlerinden birini seçin:  
      * Erişim Yok 
      * Okuma ve Yazma
      * Salt Okunur

    * **Salt okunurdur** ve **okuma/yazma**: nfsv 4.1 ile Kerberos Şifrelemesi kullanırsanız, [Nfsv 4.1 Kerberos şifrelemesini yapılandırma](configure-kerberos-encryption.md)bölümündeki yönergeleri izleyin.  Kerberos 'un performans etkisi için bkz. [NFSv 4.1 birimlerinde Kerberos 'un performans etkisi](performance-impact-kerberos.md). 

      ![Kerberos güvenlik seçenekleri](../media/azure-netapp-files/kerberos-security-options.png) 

    * **Kök erişimi**: `root` hesabın birime erişip erişemeyeceğini belirtin.  Varsayılan olarak, kök erişim **Açık** olarak ayarlanır ve `root` hesabın birime erişimi vardır.

      ![İlkeyi dışarı aktarma](../media/azure-netapp-files/azure-netapp-files-export-policy.png) 

## <a name="next-steps"></a>Sonraki adımlar 
* [Sanal makineler için birimi bağlama veya ayırma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Anlık görüntüleri yönetme](azure-netapp-files-manage-snapshots.md)
