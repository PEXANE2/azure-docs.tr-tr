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
ms.date: 03/07/2019
ms.author: b-juche
ms.openlocfilehash: 76e01055043932f2c7e7d57bd7eed6265d666a8c
ms.sourcegitcommit: bd4198a3f2a028f0ce0a63e5f479242f6a98cc04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2019
ms.locfileid: "72302779"
---
# <a name="mount-or-unmount-a-volume-for-windows-or-linux-virtual-machines"></a>Windows veya Linux sanal makineleri için birimi bağlama veya ayırma 

Gerektiğinde Windows veya Linux sanal makineleri için bir birimi bağlayabilir veya kaldırabilirsiniz.  Linux sanal makineleri için bağlama yönergeleri Azure NetApp Files kullanılabilir.  

1. **Birimler** dikey penceresine tıklayın ve sonra bağlamak istediğiniz birimi seçin. 
2. Seçili birimdeki **bağlama yönergeleri** ' ne tıklayın ve ardından birimi bağlamak için yönergeleri izleyin. 

    ![Bağlama yönergeleri NFS](../media/azure-netapp-files/azure-netapp-files-mount-instructions-nfs.png)

    ![Bağlama yönergeleri SMB](../media/azure-netapp-files/azure-netapp-files-mount-instructions-smb.png)
    
NFSv 4.1 kullanıyorsanız, dosya sisteminizi bağlamak için şu komutu kullanın: `sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys $MOUNTTARGETIPADDRESS:/$VOLUMENAME $MOUNTPOINT`
