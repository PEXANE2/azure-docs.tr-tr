---
title: Azure Linux Sanal Makineleri 'de önyükleme hatalarını giderme | Microsoft Docs
description: Bu makale, Azure Linux Sanal Makineleri 'da önyükleme hatalarını gidermek için sizi makalelere bağlamanıza yardımcı olur.
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
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74408748"
---
# <a name="troubleshoot-azure-linux-virtual-machines-boot-errors"></a>Azure Linux Sanal Makineleri önyükleme hatalarında sorun giderme

Bu makalede, Microsoft Azure ' de bir Linux sanal makinesi (VM) başlattığınızda alabileceğiniz yaygın önyükleme hataları listelenmektedir. Hatalar hakkında daha fazla bilgi için **önyükleme hataları ve çözümleri** bölümündeki makalelere bakın.

## <a name="boot-errors-and-solutions"></a>Önyükleme hataları ve çözümleri

* [GRUB kurtarma](troubleshoot-vm-boot-error.md)

## <a name="next-steps"></a>Sonraki adımlar

- [VM seri konsolu](serial-console-linux.md)

Azure kullanarak bir kurtarma VM 'sine işletim sistemi diski ekleyerek Linux VM sorunlarını giderme:

- [Azure VM onarımı](repair-linux-vm-using-azure-virtual-machine-repair-commands.md)

 Disk takas: Bu, aşağıdakilerden biri kullanılarak otomatikleştirilebilir:
- [Power Shell kurtarma betikleri](https://github.com/Azure/azure-support-scripts/tree/master/VMRecovery/ResourceManager)
- [Bash kurtarma betikleri](https://github.com/sribs/azure-support-scripts)

- [CLI](troubleshoot-recovery-disks-linux.md)
- [Azure Portal](troubleshoot-recovery-disks-portal-linux.md)


## <a name="disk-swap-video"></a>Disk takas videosu:

GRUB 'ye erişiminiz yoksa, [Bu](https://youtu.be/m5t0GZ5oGAc) videoyu IZLEYIN ve VM 'nizi kurtarmak için disk takas yordamını nasıl kolayca otomatikleştirebileceğinizi öğrenin

## <a name="unofficial-solution"></a>Resmi olmayan çözüm

VM kurtarma, desteklenmeyen [Beta betiği ile](https://github.com/malachma/azure-auto-recover) de denenmeye çalışılabilir