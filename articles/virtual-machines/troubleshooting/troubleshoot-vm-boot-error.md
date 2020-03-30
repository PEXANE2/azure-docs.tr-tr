---
title: Linux VM botları Grub Rescue için
description: Sanal makine bir kurtarma konsoluna girdiği için sanal makine önyükleme de başarısız oldu
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 08/28/2019
ms.author: tiag
ms.openlocfilehash: c24a840716841d04537ac5b77bcaf26fca4b78cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77561958"
---
# <a name="linux-vm-boots-to-grub-rescue"></a>Linux VM botları Grub Rescue için

Sanal Makinenizin (VM) bir kurtarma konsoluna girdiğini belirledik. Sorun, Linux VM'nizin çekirdek yükseltmesi gibi son zamanlarda uygulanan çekirdek değişiklikleri olduğunda ve önyükleme işlemi sırasında çekirdek hataları nedeniyle artık düzgün başlatılamadığında sorun ortaya çıkar. Önyükleme işlemi sırasında, önyükleme yükleyicisi Linux çekirdeğini bulmaya ve önyükleme denetimini ona teslim etmeye çalıştığında, teslim başarısız olduğunda VM bir kurtarma konsoluna girer.

Gelecekte bir VM'ye bağlanamayacağınızı fark ederseniz, Azure portalındaki önyükleme tanılama bıçağını kullanarak VM'nizin ekran görüntüsünü görüntüleyebilirsiniz. Bu görüntü sorunu tanılamanıza ve nedeninin benzer bir önyükleme hatası olup olmadığını belirlemenize yardımcı olabilir.

## <a name="recommended-steps"></a>Önerilen adımlar

Aldığınız hataya bağlı olarak aşağıdaki azaltma adımlarını izleyin:

### <a name="error---unknown-filesystem"></a>Hata - Bilinmeyen dosya sistemi

* Hata **Bilinmiyor dosya sistemi**alıyorsanız, bu hata önyükleme bölümü üzerinde bir dosya sistemi bozulması veya yanlış çekirdek yapılandırma kaynaklanabilir.

   * Dosya sistemi sorunları için, makalelinux kurtarma adımları [izleyin: Dosya sistemi hataları (fsck, inodes) nedeniyle Linux VM SSH olamaz.](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/)
   * Çekirdek sorunları için, makalelinux Kurtarma adımları [izleyin: El ile çekirdek sorunları ile ilgili olmayan önyükleme sorunları giderme,](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)veya [Linux Kurtarma: Chroot kullanarak Çekirdek sorunları ile ilgili olmayan önyükleme sorunları giderme](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-fixing-non-boot-issues-related-to-kernel-problems-using-chroot/).
   
### <a name="error---file-not-found"></a>Hata - Dosya bulunamadı

* **Hata hata 15 alıyorsanız: Dosya bulunamadı veya ilk RAM disk** veya **initrd/initramfs dosyası bulunamadı,** aşağıdaki adımları izleyin.

    * Eksik dosya `/boot/grub2/grub.cfg` için `initrd/initramfs` veya aşağıdaki işlem ile devam edin:

    1. Var `/etc/default/grub` olduğundan ve doğru/istenilen ayarlara sahip olduğundan emin olun. Varsayılan ayarların hangisi olduğunu bilmiyorsanız, çalışan bir VM ile kontrol edebilirsiniz.

    2. Ardından, yapılandırmasını yeniden oluşturmak için aşağıdaki komutu çalıştırın:`grub2-mkconfig -o /boot/grub2/grub.cfg`

   * Eksik `/boot/grub/menu.lst`dosya ise, bu hata eski işletim sistemi sürümleri için **(RHEL 6.x**, **Centos 6.x** ve **Ubuntu 14.04**) böylece komutları farklı olabilir. Doğru komutların sağlandığından emin olmak için eski bir sunucuyu döndürmeniz ve test etmeniz gerekir.

### <a name="error---no-such-partition"></a>Hata - Böyle bir bölüm yok

* Hata alıyorsanız Böyle **bir bölüm yok**, [Case Scenario bakın: "böyle bir bölüm" hata işletim sistemi sürücüsü genişletmeye çalıştıktan sonra VM başlatmak için çalışırken .](https://blogs.technet.microsoft.com/shwetanayak/2017/03/12/case-scenario-no-such-partition-error-while-trying-to-start-the-vm-after-attempting-to-extend-the-os-drive/)

### <a name="error---grubcfg-file-not-found"></a>Hata - grub.cfg dosyası bulunamadı

* Hata **/boot/grub2/grub.cfg dosyası bulunamadı**alıyorsanız, aşağıdaki adımları izleyin.

    * Eksik dosya `/boot/grub2/grub.cfg` için `initrd/initramfs` veya aşağıdaki işlem ile devam edin:

    1. Var `/etc/default/grub` olduğundan ve doğru/istenilen ayarlara sahip olduğundan emin olun. Varsayılan ayarların hangisi olduğunu bilmiyorsanız, çalışan bir VM ile kontrol edebilirsiniz.

    2. Ardından, yapılandırmasını yeniden oluşturmak için `grub2-mkconfig -o /boot/grub2/grub.cfg`aşağıdaki komutu çalıştırın: .

   * Eksik `/boot/grub/menu.lst`dosya ise, bu hata eski işletim sistemi sürümleri için **(RHEL 6.x**, **Centos 6.x** ve **Ubuntu 14.04**) böylece komutları erteleyebilir. Eski bir sunucuyu döndürün ve doğru komutların sağlandığından emin olmak için onu test edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Sanal Makine Aracısı'na genel bakış](../extensions/agent-windows.md)
* [Windows için sanal makine uzantıları ve özellikleri](../extensions/features-windows.md)

