---
title: Azure Linux Sanal Makinelerde sorun giderme önyükleme hataları | Microsoft Dokümanlar
description: Bu makale, Azure Linux Sanal Makinelerdeki önyükleme hatalarını gidermek için makalelere bağlanmanıza yardımcı olur.
services: virtual-machines-linux
documentationCenter: ''
author: vilibert
manager: spogge
editor: ''
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/20/2019
ms.author: vilibert
ms.openlocfilehash: 37cb201751f72918838efe5837aa0e357d483f24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74408748"
---
# <a name="troubleshoot-azure-linux-virtual-machines-boot-errors"></a>Azure Linux Sanal Makineler önyükleme hatalarını giderme

Bu makalede, Microsoft Azure'da bir Linux sanal makine (VM) başlattığınızda alabileceğiniz yaygın önyükleme hataları listelenebilmiştir. Hatalar hakkında daha fazla bilgi için **Önyükleme hataları ve çözümleri** bölümündeki makalelere bakın.

## <a name="boot-errors-and-solutions"></a>Önyükleme hataları ve çözümleri

* [GRUB kurtarma](troubleshoot-vm-boot-error.md)

## <a name="next-steps"></a>Sonraki adımlar

- [VM Seri Konsol](serial-console-linux.md)

Azure kullanarak işletim sistemi diskini kurtarma VM'ine takarak bir Linux VM sorun giderme:

- [Azure VM onarımı](repair-linux-vm-using-azure-virtual-machine-repair-commands.md)

 Disk Takas - bu ya kullanarak otomatik olabilir:
- [Güç Kabuğu Kurtarma Komut Dosyaları](https://github.com/Azure/azure-support-scripts/tree/master/VMRecovery/ResourceManager)
- [bash Kurtarma Scriptler](https://github.com/sribs/azure-support-scripts)

- [CLI](troubleshoot-recovery-disks-linux.md)
- [Azure portalında](troubleshoot-recovery-disks-portal-linux.md)


## <a name="disk-swap-video"></a>Disk Değiştirme Videosu:

GRUB erişiminiz yoksa [bu](https://youtu.be/m5t0GZ5oGAc) videoyu izleyin ve VM'nizi kurtarmak için disk değiştirme yordamını nasıl otomatikleştirebileceğinizi görün

## <a name="unofficial-solution"></a>Gayri resmi çözüm

VM'nin geri kazanımı, desteklenmeyen BETA komut dosyası [ALAR](https://github.com/malachma/azure-auto-recover) ile de denenebilir