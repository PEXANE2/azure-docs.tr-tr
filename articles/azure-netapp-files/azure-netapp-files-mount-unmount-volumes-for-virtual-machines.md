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
ms.date: 11/08/2019
ms.author: b-juche
ms.openlocfilehash: 4cd3bc7e4f95869d3efd2d92a7cdf1addc7ce5b2
ms.sourcegitcommit: 44c2a964fb8521f9961928f6f7457ae3ed362694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73953108"
---
# <a name="mount-or-unmount-a-volume-for-windows-or-linux-virtual-machines"></a>Windows veya Linux sanal makineleri için birimi bağlama veya ayırma 

Gerektiğinde Windows veya Linux sanal makineleri için bir birimi bağlayabilir veya kaldırabilirsiniz.  Linux sanal makineleri için bağlama yönergeleri Azure NetApp Files kullanılabilir.  

1. **Birimler** dikey penceresine tıklayın ve sonra bağlamak istediğiniz birimi seçin. 
2. Seçili birimdeki **bağlama yönergeleri** ' ne tıklayın ve ardından birimi bağlamak için yönergeleri izleyin. 

    ![Bağlama yönergeleri NFS](../media/azure-netapp-files/azure-netapp-files-mount-instructions-nfs.png)

    ![Bağlama yönergeleri SMB](../media/azure-netapp-files/azure-netapp-files-mount-instructions-smb.png)
    
NFSv 4.1 kullanıyorsanız, dosya sisteminizi bağlamak için aşağıdaki komutu kullanın:  

`sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys $MOUNTTARGETIPADDRESS:/$VOLUMENAME $MOUNTPOINT`  

## <a name="next-steps"></a>Sonraki adımlar

* [Azure NetApp Files için NFSv 4.1 varsayılan etki alanını yapılandırın](azure-netapp-files-configure-nfsv41-domain.md)
* Bir Azure VM başlatıldığında veya yeniden başlatıldığında NFS biriminin otomatik olarak bağlanması hakkında bilgi için bkz. [NFS SSS](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#nfs-faqs) .
