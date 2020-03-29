---
title: Azure VM önyükleme yaparken dosya sistemini denetleme| Microsoft Dokümanlar
description: Önyükleme yaparken VM'nin dosya sistemini denetlemeyi gösterdiği sorununu nasıl çözeceğinizi öğrenin| Microsoft Dokümanlar
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921496"
---
# <a name="windows-shows-checking-file-system-when-booting-an-azure-vm"></a>Windows, Azure VM önyükleme yaparken "dosya sistemini denetleme" gösterir

Bu makalede, Microsoft Azure'da bir Windows Sanal Makine (VM) önyükleme yaptığınızda karşılaşabileceğiniz "Dosya sistemini denetleme" hatası açıklanmaktadır.


## <a name="symptom"></a>Belirti 

Windows VM başlatılamıyor. [Önyükleme tanılamasında](boot-diagnostics.md)önyükleme ekran görüntülerini kontrol ettiğinizde, Denetim Diski işleminin (chkdsk.exe) aşağıdaki iletilerden biriyle çalıştığını görürsünüz:

- Tarama ve onarım sürücü (C:)
- C'de dosya sistemini denetleme:

## <a name="cause"></a>Nedeni

Dosya sisteminde bir NTFS hatası bulunursa, Windows bir sonraki yeniden başlatmada diskin tutarlılığını denetler ve onarır. Genellikle bu durum, VM'de beklenmeyen bir yeniden başlatma olduysa veya VM kapatma işlemi aniden kesintiye uğradıysa olur.

## <a name="solution"></a>Çözüm 

Windows, Denetim Diski işlemi tamamlandıktan sonra normal olarak önyükleme yapacaktır. VM Denetim Diski işleminde sıkışmışsa, VM'de Denetim Diskini çevrimdışı çalıştırmayı deneyin:
1.  Yedek olarak etkilenen VM'nin işletim sistemi diskinin anlık görüntüsünü alın. Daha fazla bilgi için [bir diskanlık anlık görüntüsüne](../windows/snapshot-copy-managed-disk.md)bakın.
2.  [Os diskini kurtarma VM'sine takın.](troubleshoot-recovery-disks-portal-windows.md)  
3.  Kurtarma VM'inde, ekli işletim sistemi diskinde Denetim Diski çalıştırın. Aşağıdaki örnekte, bağlı işletim sistemi diskinin sürücü harfi E'dir: 
        
        chkdsk E: /f
4.  Denetim Diski tamamlandıktan sonra, diski kurtarma VM'sinden ayırın ve diski işletim sistemi diski olarak etkilenen VM'ye yeniden takın. Daha fazla bilgi için, [işletim sistemi diskini kurtarma VM'ine takarak Windows VM Sorun Giderme'ye](troubleshoot-recovery-disks-portal-windows.md)bakın.
