---
title: GRUB ve tek kullanıcı modu için Azure Seri Konsolu | Microsoft Dokümanlar
description: Bu makalede, Azure sanal makinelerde GRUB için Seri Konsol'un nasıl kullanılacağı açıklanmaktadır.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/06/2019
ms.author: alsin
ms.openlocfilehash: 06cb3fe5d551ddfc95fcbd37cd9620adebd825c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70883935"
---
# <a name="use-serial-console-to-access-grub-and-single-user-mode"></a>GRUB ve tek kullanıcı moduna erişmek için Seri Konsol'u kullanma
GRand Birleşik Bootloader (GRUB) büyük olasılıkla bir sanal makine (VM) önyükleme gördüğünüz ilk şeydir. İşletim sistemi başlamadan önce görüntülendiğinden, GRUB'a SSH üzerinden erişilemez. GRUB'da, önyükleme yapılandırmanızı diğer şeylerin yanı sıra tek kullanıcı moduna önyükleme yapmak için değiştirebilirsiniz.

Tek kullanıcılı mod, en az işlevsellik ile en az bir ortamdır. Önyükleme sorunlarını, dosya sistemi sorunlarını veya ağ sorunlarını araştırmak için yararlı olabilir. Arka planda daha az hizmet çalışabilir ve runlevel'e bağlı olarak bir dosya sistemi otomatik olarak monte edilemeyebilir.

Tek kullanıcılı mod, VM'nizin oturum açma için yalnızca SSH anahtarlarını kabul etmek üzere yapılandırılabileceği durumlarda da yararlıdır. Bu durumda, parola kimlik doğrulaması içeren bir hesap oluşturmak için tek kullanıcılı modu kullanabilirsiniz. 

> [!NOTE]
> Seri Konsol hizmeti, yalnızca *katılımcı* düzeyine veya daha yüksek izinlere sahip kullanıcıların bir VM'nin seri konsoluna erişmesine izin verir.

Tek kullanıcımoduna girmek için, VM'niz önyükleme yaparken GRUB'u girin ve GRUB'da önyükleme yapılandırmasını değiştirin. Sonraki bölümde GRUB girmek için ayrıntılı talimatlara bakın. Genel olarak, VM'iniz GRUB'u görüntülemek üzere yapılandırıldıysa, VM'nizi yeniden başlatmak ve GRUB görüntülemek için VM'nizin seri konsolundaki yeniden başlatma düğmesini kullanabilirsiniz.

![Linux Seri Konsol yeniden başlatma düğmesi](./media/virtual-machines-serial-console/virtual-machine-serial-console-restart-button-bar.png)

## <a name="general-grub-access"></a>Genel GRUB erişimi
GRUB'a erişmek için Seri Konsol bölmesi açıkken VM'nizi yeniden başlatın. Bazı dağıtımlar GRUB'u göstermek için klavye girişi gerektirir ve diğerleri kullanıcı klavyegirişinin zaman anına iptal etmesine izin vermek için grub'u otomatik olarak birkaç saniye liğine gösterir.

Tek kullanıcı moduna erişebilmek için VM'nizde GRUB'un etkin olduğundan emin olmak istersiniz. Dağıtımınıza bağlı olarak, GRUB'un etkin olduğundan emin olmak için bazı kurulum çalışmaları gerekebilir. Dağıtıma özel bilgiler için, Azure sayfasında bir sonraki bölüme ve [Linux desteğimize](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/) bakın.

### <a name="restart-your-vm-to-access-grub-in-serial-console"></a>Seri Konsol'da GRUB'a erişmek için VM'nizi yeniden başlatın
VM'nizi Seri Konsol içinde **Yeniden Başlat** düğmesinin üzerine gidip **VM'yi yeniden başlat'ı**seçerek yeniden başlatabilirsiniz. Bölmenin alt kısmında yeniden başlatma yla ilgili bir bildirim görüntülenir.

[Ayrıca,](./serial-console-nmi-sysrq.md) SysRq etkinse bir SysRq "b" komutu çalıştırarak VM'nizi yeniden başlatabilirsiniz. Yeniden başlatırken GRUB'dan ne beklemeniz gerektiğini öğrenmek için sonraki bölümlerdeki dağıtıma özel yönergeleri görün.

![Linux Seri Konsol yeniden başlatma](./media/virtual-machines-serial-console/virtual-machine-serial-console-restart-button-ubuntu.gif)

## <a name="general-single-user-mode-access"></a>Genel tek kullanıcımodu erişimi
Parola kimlik doğrulaması içeren bir hesabı yapılandırmadığınızda, tek kullanıcı moduna manuel erişim eki gerekebilir. Tek kullanımlık modu el ile girmek için GRUB yapılandırmasını değiştirin. Bunu yaptıktan sonra, daha fazla talimat için "Sıfırlamak veya parola eklemek için tek kullanıcı modunu kullanın" bölümüne bakın.

VM önyükleme yapamıyorsa, dağıtımlar genellikle sizi otomatik olarak tek kullanıcı moduna veya acil durum moduna düşürür. Ancak diğer dağıtımlar, sizi otomatik olarak tek kullanıcılı veya acil durum moduna bırakmadan önce kök parola ayarlama gibi ek kurulum gerektirir.

### <a name="use-single-user-mode-to-reset-or-add-a-password"></a>Parolayı sıfırlamak veya eklemek için tek kullanıcılı modu kullanma
Tek kullanıcı modunda olduktan sonra, sudo ayrıcalıklarına sahip yeni bir kullanıcı ekleyin:
1. Kullanıcı `useradd <username>` eklemek için çalıştırın.
1. Yeni `sudo usermod -a -G sudo <username>` kullanıcı kök ayrıcalıkları vermek için çalıştırın.
1. Yeni `passwd <username>` kullanıcının parolasını ayarlamak için kullanın. Daha sonra yeni kullanıcı olarak oturum açabilirsiniz.


## <a name="access-for-red-hat-enterprise-linux-rhel"></a>Red Hat Enterprise Linux (RHEL) için erişim
RHEL normal olarak önyükleme yapamazsa, sizi otomatik olarak tek kullanıcı moduna düşürür. Ancak, tek kullanıcı modu için kök erişimi ayarlamadıysanız, kök parolanız yoktur ve oturum açamaz. Bir geçici çözüm vardır (bkz: "RHEL'de el ile tek kullanıcı moduna girin" bölümüne bakın), ancak başlangıçta kök erişimi ayarlamanızı öneririz.

### <a name="grub-access-in-rhel"></a>RHEL'de GRUB erişimi
RHEL, Kutunun dışında grub özellikli olarak gelir. GRUB'a girmek için VM'nizi çalıştırarak `sudo reboot`yeniden başlatın ve ardından herhangi bir tuşa basın. GRUB bölmesi görüntülenmelidir. Değilse, GRUB dosyanızda aşağıdaki satırların bulunduğundan emin`/etc/default/grub`olun ( ):

**RHEL 8 için**

```
GRUB_TIMEOUT=5
GRUB_TERMINAL="serial console"
GRUB_CMDLINE_LINUX="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"
```

**RHEL 7 için**

```
GRUB_TIMEOUT=5
GRUB_TERMINAL_OUTPUT="serial console"
GRUB_CMDLINE_LINUX="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300 net.ifnames=0"
```

> [!NOTE]
> Red Hat ayrıca Kurtarma Modu, Acil Durum Modu veya Hata Ayıklama Modu'na önyükleme ve kök parolayı sıfırlama için belgeler sağlar. Talimatlar [için, önyükleme sırasında Terminal menüsü düzenlemesine](https://aka.ms/rhel7grubterminal)bakın.

### <a name="set-up-root-access-for-single-user-mode-in-rhel"></a>RHEL'de tek kullanıcı modu için kök erişimi ayarlama
Kök kullanıcı varsayılan olarak devre dışı bırakılır. RHEL'deki tek kullanıcı lı mod, kök kullanıcının etkinleştirilmesini gerektirir. Tek kullanıcılı modu etkinleştirmeniz gerekiyorsa, aşağıdaki yönergeleri kullanın:

1. SSH üzerinden Red Hat sistemine oturum açın.
1. Köke geçin.
1. Aşağıdakileri yaparak kök kullanıcının parolasını etkinleştirin:
    * Çalıştır `passwd root` (güçlü bir kök parola ayarlayın).
1. Kök kullanıcının yalnızca ttyS0 üzerinden aşağıdakileri yaparak oturum açabilmesini sağlayın:  
    a. Çalıştırın `edit /etc/ssh/sshd_config`ve PermitRootLogIn'in `no`.  
    b. Yalnızca `edit /etc/securetty file` ttyS0 üzerinden oturum açmaya izin vermek için çalıştırın.

Şimdi, sistem tek kullanıcı moduna geçerse, kök parolayla oturum açabilirsiniz.

Alternatif olarak, RHEL 7.4+ veya 6.9+için, GRUB istemlerinde tek kullanıcılı modu etkinleştirmek için [önyükleme'yi tek kullanıcı moduna](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single)bakın.

### <a name="manually-enter-single-user-mode-in-rhel"></a>RHEL'de tek kullanımlık modu el ile girin
Yukarıdaki yönergeleri kullanarak GRUB ve kök erişimi ayarladıysanız, aşağıdakileri yaparak tek kullanıcılı moduna girebilirsiniz:

1. GRUB'a girmek için VM'yi yeniden başlatırken Esc tuşuna basın.
1. GRUB'da, önyükleme yapmak istediğiniz işletim sistemi nizi yeniden yüklemek için E tuşuna basın. İşletim sistemi genellikle ilk satırda listelenir.
1. Çekirdek çizgisini bul. Azure'da *linux16*ile başlar.
1. Satırın sonuna gitmek için Ctrl+E tuşuna basın.
1. Hattın sonunda, *systemd.unit=rescue.target*ekleyin.
    
    Bu eylem sizi tek kullanıcı moduna getirir. Acil durum modunu kullanmak istiyorsanız, linenin sonuna *systemd.unit=emergency.target* ekleyin *(systemd.unit=rescue.target*yerine).

1. Ctrl+X tuşuna basın ve uygulanan ayarlarla çıkış ve yeniden başlatın.

   Tek kullanıcılı modu girmeden önce yönetici parolası için istenirsiniz. Bu parola, önceki yönergelerde oluşturduğunuz paroladır.

    ![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-enter-emergency-shell.gif)

### <a name="enter-single-user-mode-without-root-account-enabled-in-rhel"></a>RHEL'de kök hesabı etkin olmadan tek kullanıcılı modu girin
Önceki yönergeleri izleyerek kök kullanıcıyı etkinleştirmediyseniz, aşağıdakileri yaparak kök parolanızı sıfırlayabilirsiniz:

> [!NOTE]
> SELinux kullanıyorsanız, kök parolayı sıfırlarken, [Red Hat belgelerinde](https://aka.ms/rhel7grubterminal)açıklanan ek adımları izlediğinden emin olun.

1. GRUB'a girmek için VM'yi yeniden başlatırken Esc tuşuna basın.

1. GRUB'da, önyükleme yapmak istediğiniz işletim sistemi nizi yeniden yüklemek için E tuşuna basın. İşletim sistemi genellikle ilk satırda listelenir.
1. Çekirdek çizgisini bul. Azure'da *linux16*ile başlar.
1. Satırın sonunda, satırın sonuna *rd.break* ekleyin. Çekirdek çizgisi ve *rd.break*arasında bir boşluk bırakın.

    Bu eylem, denetim [Red Hat belgelerinde](https://aka.ms/rhel7rootpassword)açıklandığı gibi `initramfs` `systemd`, 'dan geçirilmeden önce önyükleme işlemini keser.
1. Ctrl+X tuşuna basın ve uygulanan ayarlarla çıkış ve yeniden başlatın.

   Yeniden başlattıktan sonra, salt okunur dosya sistemiyle acil durum moduna düşersiniz. 
   
1. Kabukta, okuma/yazma izinleriyle kök dosya sistemini yeniden monte etmek için girin. `mount -o remount,rw /sysroot`
1. Tek kullanıcı moduna önyükleme `chroot /sysroot` sonra, `sysroot` hapishaneye geçmek için girin.
1. Artık kökün sende. Tek kullanıcılı modu girmek için `passwd` önceki yönergeleri girerek kök parolanızı sıfırlayabilir ve ardından kullanabilirsiniz. 
1. İşiniz bittikten `reboot -f` sonra yeniden başlatmak için girin.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-emergency-mount-no-root.gif)

> [!NOTE]
> Yukarıdaki yönergeleri çalıştırmak sizi acil durum kabuğuna düşürür, böylece düzenleme `fstab`gibi görevleri de gerçekleştirebilirsiniz. Ancak, normalde kök parolanızı sıfırlamanızı ve tek kullanıcılı modu girmek için kullanmanızı öneririz.

## <a name="access-for-centos"></a>CentOS için erişim
Red Hat Enterprise Linux gibi CentOS'taki tek kullanıcılı mod da GRUB'un ve kök kullanıcının etkin olmasını gerektirir.

### <a name="grub-access-in-centos"></a>CentOS'ta GRUB erişimi
CentOS, GRUB özellikli kutusundan çıktı. GRUB'a girmek için VM'nizi `sudo reboot`girerek yeniden başlatın ve ardından herhangi bir tuşa basın. Bu eylem GRUB bölmesini görüntüler.

### <a name="single-user-mode-in-centos"></a>CentOS'ta tek kullanıcı modu
CentOS'ta tek kullanıcı modunu etkinleştirmek için RHEL için önceki yönergeleri izleyin.

## <a name="access-for-ubuntu"></a>Ubuntu'ya erişim
Ubuntu görüntüleri kök parola gerektirmez. Sistem tek kullanıcı moduna giriyorsa, ek kimlik bilgileri olmadan kullanabilirsiniz.

### <a name="grub-access-in-ubuntu"></a>Ubuntu'da GRUB erişimi
GRUB'a erişmek için VM önyükleme yaparken Esc tuşuna basın ve basılı tutun.

Varsayılan olarak, Ubuntu görüntüleri GRUB bölmesini otomatik olarak görüntülemeyebilir. Aşağıdakileri yaparak ayarı değiştirebilirsiniz:
1. Bir metin düzenleyicisinde */etc/default/grub.d/50-cloudimg-settings.cfg* dosyasını açın.

1. Değeri `GRUB_TIMEOUT` sıfır olmayan bir değerle değiştirin.
1. Metin düzenleyicisinde, açık */etc/default/grub*.
1. `GRUB_HIDDEN_TIMEOUT=1` Satırı yorumla.
1. Bir `GRUB_TIMEOUT_STYLE=menu` çizgi olduğundan emin olun.
1. `sudo update-grub` öğesini çalıştırın.

### <a name="single-user-mode-in-ubuntu"></a>Ubuntu'da tek kullanıcı modu
Ubuntu normal olarak önyükleme yapamazsa, sizi otomatik olarak tek kullanıcı moduna düşürür. Tek kullanımlık modu el ile girmek için aşağıdakileri yapın:

1. GRUB'da, önyükleme girişinizi (Ubuntu girişi) yeniden yüklemek için E tuşuna basın.
1. *Linux*ile başlayan satırı arayın ve sonra *ro*arayın .
1. *Ro*sonra *tek* ekleyin , *tek*önce ve sonra bir boşluk olmasını sağlamak .
1. Bu ayarlarla yeniden başlatmak için Ctrl+X tuşuna basın ve tek kullanıcılı modu girin.

### <a name="use-grub-to-invoke-bash-in-ubuntu"></a>Ubuntu bash çağırmak için GRUB kullanın
Önceki yönergeleri denedikten sonra, Ubuntu VM'nizde tek kullanıcı moduna hala erişemediğiniz bir durum (unutulmuş kök parola gibi) olabilir. Çekirdekten sistem init yerine `/bin/bash` init olarak çalışmasını da söyleyebilirsiniz. Bu eylem size bir bash kabuk verir ve sistem bakımı için izin verir. Aşağıdaki talimatları kullanın:

1. GRUB'da, önyükleme girişinizi (Ubuntu girişi) yeniden yüklemek için E tuşuna basın.

1. *Linux*ile başlayan satırı arayın ve sonra *ro*arayın .
1. *ro'yu* *rw init=/bin/bash*ile değiştirin.

    Bu eylem, dosya sisteminizi okuma yazma `/bin/bash` olarak bağlar ve init işlemi olarak kullanır.
1. Bu ayarlarla yeniden başlatmak için Ctrl+X tuşuna basın.

## <a name="access-for-coreos"></a>CoreOS için erişim
CoreOS'ta tek kullanıcı modu, GRUB'un etkinleştirilmesini gerektirir.

### <a name="grub-access-in-coreos"></a>CoreOS'ta GRUB erişimi
GRUB'a erişmek için, VM'niz önyükleme yaparken herhangi bir tuşa basın.

### <a name="single-user-mode-in-coreos"></a>CoreOS'ta tek kullanıcı modu
CoreOS normal önyükleme yapamazsa, otomatik olarak tek kullanıcı moduna düşer. Tek kullanımlık modu el ile girmek için aşağıdakileri yapın:

1. GRUB'da, önyükleme girişinizi yapmak için E tuşuna basın.

1. *Linux$* ile başlayan satırı arayın. Satırın iki örneği olmalı, her biri farklı bir şekilde kapsüllenmiş *sa... başka* bir madde.
1. Her *linux$* satırının sonuna *coreos.autologin=ttyS0* ekin.
1. Bu ayarlarla yeniden başlatmak için Ctrl+X tuşuna basın ve tek kullanıcılı modu girin.

## <a name="access-for-suse-sles"></a>SUSE SLES için erişim
Sistem acil durum moduna giriyorsa SLES 12 SP3+'ın yeni görüntüleri seri konsol üzerinden erişime izin verir.

### <a name="grub-access-in-suse-sles"></a>SUSE SLES'te GRUB erişimi
SLES GRUB erişim YaST üzerinden bir bootloader yapılandırma gerektirir. Yapılandırmayı oluşturmak için aşağıdakileri yapın:

1. SLES VM'nizde oturum açıp çalıştırmak için `sudo yast bootloader`SSH'yi kullanın. Sekme tuşuna basın, Enter tuşuna basın ve menüde gezinmek için ok tuşlarını kullanın.

1. Çekirdek **Parametreleri'ne**gidin ve ardından **Seri Konsolu Kullan** onay kutusunu seçin.
1. Konsol `serial --unit=0 --speed=9600 --parity=no` bağımsız **Console** değişkenlerine ekleyin.
1. Ayarlarınızı kaydetmek ve çıkmak için F10 tuşuna basın.
1. GRUB'a girmek için VM'nizi yeniden başlatın ve GRUB bölmesinin görüntülenmesini sağlamak için önyükleme sırasında herhangi bir tuşa basın.

    GRUB için varsayılan zaman ası **1'dir.** */etc/default/grub* `GRUB_TIMEOUT` dosyasındaki değişkeni değiştirerek bu ayarı değiştirebilirsiniz.

![Bootloader yapılandırmasını başlatma](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-sles-yast-grub-config.gif)

### <a name="single-user-mode-in-suse-sles"></a>SUSE SLES'te tek kullanıcı modu
SLES normal bir şekilde önyükleme yapamazsa, otomatik olarak acil durum kabuğuna bırakılırsınız. Acil durum kabuğunu el ile girmek için aşağıdakileri yapın:

1. GRUB'da, önyükleme girişinizi (SLES girişi) yeniden yüklemek için E tuşuna basın.

1. *Linux*ile başlayan çekirdek çizgisine bakın.
1. Ek *systemd.unit=emergency.target* çekirdek hattının sonuna kadar.
1. Bu ayarlarla yeniden başlatmak için Ctrl+X tuşuna basın ve acil durum kabuğunu girin.

   > [!NOTE]
   > Bu eylem, salt okunur dosya sistemiyle sizi acil durum kabuğuna düşürür. Herhangi bir dosyayı düzenlemek için, dosya sistemini okuma yazma izinleriyle yeniden monte edin. Bunu yapmak için, kabuk girin. `mount -o remount,rw /`

## <a name="access-for-oracle-linux"></a>Oracle Linux'a erişim
Red Hat Enterprise Linux gibi Oracle Linux'taki tek kullanıcılı mod da GRUB'un ve kök kullanıcının etkin olmasını gerektirir.

### <a name="grub-access-in-oracle-linux"></a>Oracle Linux'ta GRUB erişimi
Oracle Linux grub kutunun dışında etkin ile birlikte geliyor. GRUB'a girmek için VM'nizi çalıştırarak `sudo reboot`yeniden başlatın ve ardından Esc tuşuna basın. Bu eylem GRUB bölmesini görüntüler. GRUB bölmesi görüntülenmiyorsa, satırın değerinin `GRUB_TERMINAL` *seri konsol* içerdiğinden `GRUB_TERMINAL="serial console"`emin olun (diğer bir şekilde). GRUB'u `grub2-mkconfig -o /boot/grub/grub.cfg`yeniden inşa edin.

### <a name="single-user-mode-in-oracle-linux"></a>Oracle Linux'ta tek kullanıcı modu
Oracle Linux'ta tek kullanıcı modunu etkinleştirmek için RHEL için önceki yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar
Seri Konsol hakkında daha fazla bilgi edinmek için bkz:
* [Linux Seri Konsol belgeleri](serial-console-linux.md)
* [Çeşitli dağıtımlarda GRUB'u etkinleştirmek için Seri Konsol'u kullanma](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/)
* [NMI ve SysRq aramaları için Seri Konsol kullanın](serial-console-nmi-sysrq.md)
* [Windows VM'ler için Seri Konsol](serial-console-windows.md)
* [Önyükleme tanılamaları](boot-diagnostics.md)
