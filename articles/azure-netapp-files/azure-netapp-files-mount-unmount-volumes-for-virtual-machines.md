---
title: Sanal makineler için Azure NetApp Dosyaları ciltlerini dağıla
description: Azure'daki Windows sanal makineleri veya Linux sanal makineleri için bir ses düzeyini nasıl monte edinveya atmayı öğrenin.
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: conceptual
ms.date: 04/22/2020
ms.openlocfilehash: c439ff8df95d759e96d2fc82356bda8551507e8d
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82084949"
---
# <a name="mount-or-unmount-a-volume-for-windows-or-linux-virtual-machines"></a>Windows veya Linux sanal makineleri için birimi bağlama veya ayırma 

Gerektiğinde Windows veya Linux sanal makineleri için bir birim monte edebilir veya atlayabilirsiniz.  Linux sanal makineleri için montaj talimatları Azure NetApp Files'da mevcuttur.  

1. **Birimler** bıçağını tıklatın ve sonra monte etmek istediğiniz birimi seçin. 
2. Seçili birimden **Yönergeleri Dağı'nı** tıklatın ve sonra sesi takmak için yönergeleri izleyin. 

    ![Montaj talimatları NFS](../media/azure-netapp-files/azure-netapp-files-mount-instructions-nfs.png)

    ![Montaj talimatları SMB](../media/azure-netapp-files/azure-netapp-files-mount-instructions-smb.png)
    
    NFSv4.1 kullanıyorsanız, dosya sisteminizi takmak için aşağıdaki komutu kullanın:`sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys $MOUNTTARGETIPADDRESS:/$VOLUMENAME $MOUNTPOINT`  

3. Azure VM başlatıldığında veya yeniden başlatıldığında bir NFS biriminin otomatik olarak monte `/etc/fstab` edilmesini istiyorsanız, ana bilgisayardaki dosyaya bir giriş ekleyin. 

    Örneğin:`$ANFIP:/$FILEPATH        /$MOUNTPOINT    nfs bg,rw,hard,noatime,nolock,rsize=65536,wsize=65536,vers=3,tcp,_netdev 0 0`

    * `$ANFIP`ses özellikleri bıçak bulunan Azure NetApp Dosyaları hacminin IP adresidir.
    * `$FILEPATH`Azure NetApp Dosyaları hacminin dışa aktarma yoludur.
    * `$MOUNTPOINT`NFS dışa aktarmayı monte etmek için kullanılan Linux ana bilgisayarda oluşturulan dizindir.

4. NFS kullanarak ses düzeyini Windows'a monte etmek istiyorsanız:

    a. Önce bir Unix veya Linux VM'ye ses seviyesini monte edin.  
    b. Ses `chmod 777` düzeyine karşı a veya `chmod 775` komut çalıştırın.  
    c. Ses düzeyini Windows'daki NFS istemcisi üzerinden monte edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure NetApp Files için NFSv 4.1 varsayılan etki alanını yapılandırma](azure-netapp-files-configure-nfsv41-domain.md)
* [NFS Sıkça Sorulan Soru](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#nfs-faqs)
* [Ağ Dosya Sistemi'ne genel bakış](https://docs.microsoft.com/windows-server/storage/nfs/nfs-overview)
