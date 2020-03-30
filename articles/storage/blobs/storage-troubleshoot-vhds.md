---
title: Azure VM'lere bağlı sorun giderme diskleri | Microsoft Dokümanlar
description: Azure sanal makine sanal sabit diskleri (VHD'ler) için sorun giderme kaynaklarına bağlantılar sağlar.
services: storage
author: roygara
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: rogarana
ms.reviewer: wmgries
ms.openlocfilehash: b81c0947327dc8e84c5d3fbbecde3aa31313cc70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061193"
---
# <a name="troubleshoot-disks-attached-to-azure-vms"></a>Azure VM'lerine bağlı sorun giderme diskleri 

Azure Sanal Makineler (VM'ler), işletim sistemi diski ve ekli tüm veri diskleri için Sanal Sabit Disklere (VHD' ler) güvenir. VHD'ler bir veya daha fazla Azure Depolama hesabında sayfa blob'ları olarak depolanır. Bu makalede, VHD'lerle ortaya çıkabilecek sık karşılaşılan sorunlar için içerik giderme sorunu işaret eder. 

## <a name="troubleshoot-storage-deletion-errors-for-a-vm"></a>VM için depolama silme hatalarını giderme

Bazı durumlarda, Kaynak Yöneticisi dağıtımındaki bir VM ekli VHD'ler içerdiğinde depolama kaynağını silerken bir hatayla karşılaşabilirsiniz. Bu sorunun çözümünde yardım için aşağıdaki makalelerden birine bakın: 

  * Linux [VM'lerde: Kaynak Yöneticisi dağıtımında depolama silme hataları](../../virtual-machines/linux/storage-resource-deletion-errors.md)  
  * Windows [VM'lerde: Kaynak Yöneticisi dağıtımında depolama silme hataları](../../virtual-machines/windows/storage-resource-deletion-errors.md)  

## <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Bağlı VHD'lerle VM'lerin beklenmeyen yeniden başlatmalarını giderme

Çok sayıda bağlı VHD'si olan bir VM'nin beklenmeyen yeniden başlatmalarıyla karşılaşırsanız, aşağıdaki makalelerden birine bakın:

  * Linux [VM'lerde: Bağlı VHD'lerle VM'lerin beklenmeyen yeniden başlatılması](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
  * Windows [VM'lerde: Bağlı VHD'lerle VM'lerin beklenmeyen yeniden başlatılması](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
