---
title: Windows veya Linux sanal makineleri için Azure NetApp Files birimi bağlama veya çıkarma | Microsoft Docs
description: Sanal makineler veya Linux sanal makineleri için bir birimi bağlama veya çıkarma işlemini açıklar.
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
ms.date: 12/05/2019
ms.author: b-juche
ms.openlocfilehash: 7f1e9500a9268e0fba054f7065e858cd801aca7b
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74894105"
---
# <a name="mount-or-unmount-a-volume-for-windows-or-linux-virtual-machines"></a>Windows veya Linux sanal makineleri için birimi bağlama veya ayırma 

Gerektiğinde Windows veya Linux sanal makineleri için bir birimi bağlayabilir veya kaldırabilirsiniz.  Linux sanal makineleri için bağlama yönergeleri Azure NetApp Files kullanılabilir.  

1. **Birimler** dikey penceresine tıklayın ve sonra bağlamak istediğiniz birimi seçin. 
2. Seçili birimdeki **bağlama yönergeleri** ' ne tıklayın ve ardından birimi bağlamak için yönergeleri izleyin. 

    ![Bağlama yönergeleri NFS](../media/azure-netapp-files/azure-netapp-files-mount-instructions-nfs.png)

    ![Bağlama yönergeleri SMB](../media/azure-netapp-files/azure-netapp-files-mount-instructions-smb.png)
    
    NFSv 4.1 kullanıyorsanız, dosya sisteminizi bağlamak için şu komutu kullanın: `sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys $MOUNTTARGETIPADDRESS:/$VOLUMENAME $MOUNTPOINT`  

3. Bir Azure VM başlatıldığında veya yeniden başlatıldığında bir NFS biriminin otomatik olarak bağlanmasını istiyorsanız konaktaki `/etc/fstab` dosyasına bir giriş ekleyin. 

    Örneğin: `$ANFIP:/$FILEPATH        /$MOUNTPOINT    nfs bg,rw,hard,noatime,nolock,rsize=65536,wsize=65536,vers=3,tcp,_netdev 0 0`

    * `$ANFIP`, birim özellikleri dikey penceresinde bulunan Azure NetApp Files biriminin IP adresidir.
    * `$FILEPATH`, Azure NetApp Files biriminin dışarı aktarma yoludur.
    * `$MOUNTPOINT`, NFS dışarı aktarmayı bağlamak için kullanılan Linux ana bilgisayarında oluşturulan dizindir.

4. Birimini NFS kullanarak Windows 'a bağlamak istiyorsanız:

    a. Öncelikle birimi bir UNIX veya Linux VM 'ye bağlayın.  
    b. Birimde bir `chmod 777` veya `chmod 775` komutu çalıştırın.  
    c. Windows üzerinde NFS istemcisi aracılığıyla birimi bağlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure NetApp Files için NFSv 4.1 varsayılan etki alanını yapılandırın](azure-netapp-files-configure-nfsv41-domain.md)
* [NFS SSS](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#nfs-faqs)
