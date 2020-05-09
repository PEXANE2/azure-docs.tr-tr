---
title: Sanal makineler için Azure NetApp Files birimleri bağlama
description: Azure 'da Windows sanal makineleri veya Linux sanal makineleri için bir birimi bağlama veya çıkarma hakkında bilgi edinin.
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: conceptual
ms.date: 04/30/2020
ms.openlocfilehash: d02ceda9dc2c6a822d45c2a31fe91a976610292b
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82610862"
---
# <a name="mount-or-unmount-a-volume-for-windows-or-linux-virtual-machines"></a>Windows veya Linux sanal makineleri için birimi bağlama veya ayırma 

Gerektiğinde Windows veya Linux sanal makineleri için bir birimi bağlayabilir veya kaldırabilirsiniz.  Linux sanal makineleri için bağlama yönergeleri Azure NetApp Files kullanılabilir.  

> [!IMPORTANT] 
> Bir NFS birimine erişebilmek için en az bir dışarı aktarma ilkeniz olması gerekir.

1. **Birimler** dikey penceresine tıklayın ve sonra bağlamak istediğiniz birimi seçin. 
2. Seçili birimdeki **bağlama yönergeleri** ' ne tıklayın ve ardından birimi bağlamak için yönergeleri izleyin. 

    ![Bağlama yönergeleri NFS](../media/azure-netapp-files/azure-netapp-files-mount-instructions-nfs.png)

    ![Bağlama yönergeleri SMB](../media/azure-netapp-files/azure-netapp-files-mount-instructions-smb.png)
    
    NFSv 4.1 kullanıyorsanız, dosya sisteminizi bağlamak için aşağıdaki komutu kullanın:`sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys $MOUNTTARGETIPADDRESS:/$VOLUMENAME $MOUNTPOINT`  

3. Bir Azure VM başlatıldığında veya yeniden başlatıldığında bir NFS biriminin otomatik olarak bağlanmasını istiyorsanız, konaktaki `/etc/fstab` dosyasına bir giriş ekleyin. 

    Örneğin:`$ANFIP:/$FILEPATH        /$MOUNTPOINT    nfs bg,rw,hard,noatime,nolock,rsize=65536,wsize=65536,vers=3,tcp,_netdev 0 0`

    * `$ANFIP`, birim özellikleri dikey penceresinde bulunan Azure NetApp Files biriminin IP adresidir.
    * `$FILEPATH`Azure NetApp Files biriminin dışarı aktarma yoludur.
    * `$MOUNTPOINT`, NFS dışarı aktarmayı bağlamak için kullanılan Linux ana bilgisayarında oluşturulan dizindir.

4. Birimini NFS kullanarak Windows 'a bağlamak istiyorsanız:

    a. Öncelikle birimi bir UNIX veya Linux VM 'ye bağlayın.  
    b. Birimde bir `chmod 777` veya `chmod 775` komutunu çalıştırın.  
    c. Windows üzerinde NFS istemcisi aracılığıyla birimi bağlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure NetApp Files için NFSv 4.1 varsayılan etki alanını yapılandırma](azure-netapp-files-configure-nfsv41-domain.md)
* [NFS SSS](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#nfs-faqs)
* [Ağ dosya sistemine genel bakış](https://docs.microsoft.com/windows-server/storage/nfs/nfs-overview)
