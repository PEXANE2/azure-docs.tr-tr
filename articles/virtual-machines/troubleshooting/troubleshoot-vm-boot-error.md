---
title: Linux VM, grub kurtarma için önyüklenir
description: Sanal makine bir kurtarma konsolu girdiği için sanal makine önyükleme yapamadı
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
ms.openlocfilehash: 5353bad3ff631fd7baa5d9892d38449ba622ea46
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87284685"
---
# <a name="linux-vm-boots-to-grub-rescue"></a>Linux VM, grub kurtarma için önyüklenir

Sanal makinenize (VM) bir kurtarma konsolu girdiğinizi belirledik. Bu sorun, Linux VM 'niz çekirdek yükseltmesi gibi son zamanlarda uygulanan çekirdek değişiklikleri olduğunda ve önyükleme işlemi sırasında çekirdek hataları nedeniyle artık düzgün şekilde başlatılamıyorsa oluşur. Önyükleme işlemi sırasında, önyükleme yükleyicisi Linux çekirdeğini bulmaya çalışır ve önyükleme denetimini devre dışı bırakır. iletim başarısız olduğunda VM bir kurtarma konsolu girer.

Gelecekte bir VM 'ye bağlanamadıysanız, Azure portal için önyükleme tanılama dikey penceresini kullanarak sanal makinenizin ekran görüntüsünü görüntüleyebilirsiniz. Bu görüntü sorunu tanılamanıza ve nedeninin benzer bir önyükleme hatası olup olmadığını belirlemenize yardımcı olabilir.

## <a name="recommended-steps"></a>Önerilen adımlar

Aldığınız hataya bağlı olarak aşağıdaki azaltma adımlarını izleyin:

### <a name="error---unknown-filesystem"></a>Hata-bilinmeyen dosya sistemi

* **Bilinmeyen**hata dosyalarını alıyorsanız, bu hata önyükleme bölümünde bir dosya sistemi Bozulması veya yanlış bir çekirdek yapılandırması elde edebilir.

   * Dosya sistemi sorunları için [Linux Kurtarma: dosya sistemi hataları nedeniyle LINUX VM 'ye SSH (fsck, ınomdes)](/archive/blogs/linuxonazure/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes)makalesindeki adımları izleyin.
   * Çekirdek sorunları için Linux Kurtarma makalesindeki adımları izleyin [: çekirdek sorunları ile ilgili önyükleme olmayan sorunları el ile düzeltme](http://linuxonazure.azurewebsites.net/linux-recovery-fixing-non-boot-issues-related-to-kernel-problems-using-chroot/)veya [Linux Kurtarma: chroot kullanarak çekirdek sorunlarıyla ilgili önyükleme dışı sorunları çözme](http://linuxonazure.azurewebsites.net/linux-recovery-fixing-non-boot-issues-related-to-kernel-problems-using-chroot/).
   
### <a name="error---file-not-found"></a>Hata-dosya bulunamadı

* Hata 15 hata alıyorsanız **: dosya bulunamadı veya Ilk RAM diski** ya da **ınitrd/ınitramfs dosyası bulunamadı**, aşağıdaki adımları izleyin.

    * Eksik dosya için `/boot/grub2/grub.cfg` `initrd/initramfs` aşağıdaki işleme devam edin:

    1. `/etc/default/grub`Var olduğundan ve doğru/istenen ayarlara sahip olduğundan emin olun. Varsayılan ayarları olduğunu bilmiyorsanız, çalışan bir VM ile kontrol edebilirsiniz.

    2. Sonra, yapılandırmasını yeniden oluşturmak için aşağıdaki komutu çalıştırın:`grub2-mkconfig -o /boot/grub2/grub.cfg`

   * Eksik dosya varsa `/boot/grub/menu.lst` , bu hata eski işletim sistemi sürümleri (**RHEL 6. x**, **CentOS 6. x** ve **Ubuntu 14,04**) için, komutların farklı olması için kullanılır. Doğru komutların sağlandığından emin olmak için, eski bir sunucu ve test etmeniz gerekir.

### <a name="error---no-such-partition"></a>Hata-böyle bir bölüm yok

* **Böyle bir bölüm olmadığında**hata alıyorsanız, [işletim sistemi sürücüsünü GENIŞLETMEYE çalıştıktan sonra VM başlatılmaya çalışılırken "böyle bir bölüm yok" hatası](/archive/blogs/shwetanayak/case-scenario-no-such-partition-error-while-trying-to-start-the-vm-after-attempting-to-extend-the-os-drive)bölümüne bakın.

### <a name="error---grubcfg-file-not-found"></a>Hata-grub. cfg dosyası bulunamadı

* **/Boot/GRUB2/grub.cfg dosyası bulunamadı**hatasını alıyorsanız aşağıdaki adımları izleyin.

    * Eksik dosya için `/boot/grub2/grub.cfg` `initrd/initramfs` aşağıdaki işleme devam edin:

    1. `/etc/default/grub`Var olduğundan ve doğru/istenen ayarlara sahip olduğundan emin olun. Varsayılan ayarları olduğunu bilmiyorsanız, çalışan bir VM ile kontrol edebilirsiniz.

    2. Sonra, yapılandırmasını yeniden oluşturmak için aşağıdaki komutu çalıştırın: `grub2-mkconfig -o /boot/grub2/grub.cfg` .

   * Eksik dosya varsa `/boot/grub/menu.lst` , bu hata eski işletim sistemi sürümleri için (**RHEL 6. x**, **CentOS 6. x** ve **Ubuntu 14,04**), böylece komutların ertelenmesi gerekir. Eski bir sunucu başlatın ve doğru komutların sağlandığından emin olmak için test edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal makine aracısına genel bakış](../extensions/agent-windows.md)
* [Windows için sanal makine uzantıları ve özellikleri](../extensions/features-windows.md)
