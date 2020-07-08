---
title: Azure VM 'yi önyüklerken dosya sistemi denetleniyor | Microsoft Docs
description: Önyükleme sırasında VM 'nin dosya sistemini denetleme ile ilgili sorunu çözmeyi öğrenin | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/31/2018
ms.author: genli
ms.openlocfilehash: 86938c582745cb0759eda9cd0693f407471a0529
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77921496"
---
# <a name="windows-shows-checking-file-system-when-booting-an-azure-vm"></a>Windows, bir Azure VM 'yi önyüklerken "dosya sistemini denetleme" işlemini gösterir

Bu makalede, Microsoft Azure ' de bir Windows sanal makinesini (VM) önyüklediğinizde karşılaşabileceğiniz "dosya sistemi denetleniyor" hatası açıklanır.


## <a name="symptom"></a>Belirti 

Bir Windows VM 'si başlamıyor. Önyükleme [tanılamalarında](boot-diagnostics.md)önyükleme ekran görüntülerini denetlediğinizde, disk denetleme işleminin (chkdsk.exe) aşağıdaki iletilerden biriyle çalıştığını görürsünüz:

- Sürücüyü tarama ve onarma (C:)
- C üzerinde dosya sistemi denetleniyor:

## <a name="cause"></a>Nedeni

Dosya sisteminde bir NTFS hatası bulunursa, Windows bir sonraki yeniden başlatmada diskin tutarlılığını denetler ve onarır. Genellikle bu durum, VM 'nin beklenmeyen bir yeniden başlatmaya sahip olması veya VM kapatılma işleminin aniden kesintiye uğramasından kaynaklanır.

## <a name="solution"></a>Çözüm 

Disk denetleme işlemi tamamlandıktan sonra Windows normal olarak önyüklenir. VM, diski denetle işleminde takılı kalırsa, sanal makinede denetim diskini çevrimdışı çalıştırmayı deneyin:
1.  Etkilenen VM 'nin işletim sistemi diskinin anlık görüntüsünü bir yedekleme olarak alın. Daha fazla bilgi için bkz. [disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md).
2.  [İşletim sistemi diskini bir kurtarma sanal makinesine ekleyin](troubleshoot-recovery-disks-portal-windows.md).  
3.  Kurtarma VM 'sinde, bağlı işletim sistemi diskinde diski denetle ' yi çalıştırın. Aşağıdaki örnekte, bağlı işletim sistemi diskinin sürücü harfi E: 
        
        chkdsk E: /f
4.  Disk denetimi tamamlandıktan sonra, diski kurtarma VM 'sinden ayırın ve ardından diski etkilenen VM 'ye bir işletim sistemi diski olarak yeniden ekleyin. Daha fazla bilgi için bkz. [bir kurtarma VM 'sine işletim sistemi diski ekleyerek bir Windows sanal makinesi sorunlarını giderme](troubleshoot-recovery-disks-portal-windows.md).
