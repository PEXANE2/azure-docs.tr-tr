---
title: Azure Sanal Makine önyükleme hatası
description: Sanal makine bir kurtarma konsolu girdiği için sanal makine önyükleme yapamadı
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: ''
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/28/2019
ms.author: tiag
ms.openlocfilehash: 9995b9049378a0ab4f3450ec577d034598d171e9
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70984836"
---
# <a name="vm-boot-error"></a>Sanal makine önyükleme hatası

Sanal makinenize (VM) bir kurtarma konsolu girdiğinizi belirledik. Bu sorun, Linux VM 'niz çekirdek yükseltmesi gibi son zamanlarda uygulanan çekirdek değişiklikleri olduğunda ve önyükleme işlemi sırasında çekirdek hataları nedeniyle artık düzgün şekilde başlatılamıyorsa oluşur. Önyükleme işlemi sırasında, önyükleme yükleyicisi Linux çekirdeğini bulmaya çalışır ve önyükleme denetimini devre dışı bırakır. iletim başarısız olduğunda VM bir kurtarma konsolu girer.

Gelecekte bir VM 'ye bağlanamadıysanız, Azure portal için önyükleme tanılama dikey penceresini kullanarak sanal makinenizin ekran görüntüsünü görüntüleyebilirsiniz. Bu görüntü sorunu tanılamanıza ve nedeninin benzer bir önyükleme hatası olup olmadığını belirlemenize yardımcı olabilir.

## <a name="recommended-steps"></a>Önerilen adımlar

Aldığınız hataya bağlı olarak aşağıdaki azaltma adımlarını izleyin:

### <a name="error---unknown-filesystem"></a>Hata-bilinmeyen dosya sistemi

* **Bilinmeyen**hata dosyalarını alıyorsanız, bu hata önyükleme bölümünde bir dosya sistemi Bozulması veya yanlış bir çekirdek yapılandırması elde edebilir.

   * Dosya sistemi sorunları için Linux Kurtarma makalesindeki [adımları izleyin: Dosya sistemi hataları nedeniyle Linux VM 'ye SSH uygulanamıyor (fsck, ınomdes)](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/).
   * Çekirdek sorunları için Linux Kurtarma makalesindeki [adımları izleyin: Çekirdek sorunlarıyla](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)veya [Linux kurtarmayla ilgili önyükleme olmayan sorunları el ile Düzeltme: Chroot](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-fixing-non-boot-issues-related-to-kernel-problems-using-chroot/)kullanarak çekirdek sorunlarıyla ilgili önyükleme dışı sorunlar düzeltiliyor.
   
### <a name="error---file-not-found"></a>Hata-dosya bulunamadı

* **Hata 15 hata alıyorsanız: Dosya bulunamadı veya ilk RAM diski** ya da **ınitrd/ınitramfs dosyası bulunamadı**, aşağıdaki adımları izleyin.

    * Eksik dosya `/boot/grub2/grub.cfg` `initrd/initramfs` için aşağıdaki işleme devam edin:

    1. Var `/etc/default/grub` olduğundan ve doğru/istenen ayarlara sahip olduğundan emin olun. Varsayılan ayarları olduğunu bilmiyorsanız, çalışan bir VM ile kontrol edebilirsiniz.

    2. Sonra, yapılandırmasını yeniden oluşturmak için aşağıdaki komutu çalıştırın:`grub2-mkconfig -o /boot/grub2/grub.cfg`

   * `/boot/grub/menu.lst`Eksik dosya varsa, bu hata eski işletim sistemi sürümleri (**RHEL 6. x**, **CentOS 6. x** ve **Ubuntu 14,04**) için, komutların farklı olması için kullanılır. Doğru komutların sağlandığından emin olmak için, eski bir sunucu ve test etmeniz gerekir.

### <a name="error---no-such-partition"></a>Hata-böyle bir bölüm yok

* **Böyle bir bölüm olmadığında**hata alıyorsanız, [işletim sistemi sürücüsünü GENIŞLETMEYE çalıştıktan sonra VM başlatılmaya çalışılırken "böyle bir bölüm yok" hatası](https://blogs.technet.microsoft.com/shwetanayak/2017/03/12/case-scenario-no-such-partition-error-while-trying-to-start-the-vm-after-attempting-to-extend-the-os-drive/)bölümüne bakın.

### <a name="error---grubcfg-file-not-found"></a>Hata-grub. cfg dosyası bulunamadı

* **/Boot/GRUB2/grub.cfg dosyası bulunamadı**hatasını alıyorsanız aşağıdaki adımları izleyin.

    * Eksik dosya `/boot/grub2/grub.cfg` `initrd/initramfs` için aşağıdaki işleme devam edin:

    1. Var `/etc/default/grub` olduğundan ve doğru/istenen ayarlara sahip olduğundan emin olun. Varsayılan ayarları olduğunu bilmiyorsanız, çalışan bir VM ile kontrol edebilirsiniz.

    2. Sonra, yapılandırmasını yeniden oluşturmak için aşağıdaki komutu çalıştırın: `grub2-mkconfig -o /boot/grub2/grub.cfg`.

   * `/boot/grub/menu.lst`Eksik dosya varsa, bu hata eski işletim sistemi sürümleri için (**RHEL 6. x**, **CentOS 6. x** ve **Ubuntu 14,04**), böylece komutların ertelenmesi gerekir. Eski bir sunucu başlatın ve doğru komutların sağlandığından emin olmak için test edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal makine aracısına genel bakış](../extensions/agent-windows.md)
* [Windows için sanal makine uzantıları ve özellikleri](../extensions/features-windows.md)

