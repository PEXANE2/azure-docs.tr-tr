---
title: GRUB ve Tek Kullanıcı Modu için Azure Seri Konsolu
description: Azure sanal makinelerinde grub için Seri Konsol'u kullanma.
services: virtual-machines-linux
author: asinn826
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.date: 08/14/2018
ms.author: alsin
ms.openlocfilehash: 2aa7110ab4e52fdc5c3804bd27be5f41081fb435
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758497"
---
# <a name="use-serial-console-to-access-grub-and-single-user-mode"></a>GRUB ve Tek Kullanıcı Moduna erişmek için Seri Konsol'u kullanın
GRUB GRand Birleşik Bootloader olduğunu. GRUB itibaren diğer şeylerin yanı sıra, tek bir kullanıcı moduna önyükleme için önyükleme yapılandırmasını değiştirmek edebiliyoruz.

Tek kullanıcı modu en az işlevsellik ile en az bir ortamdır. Önyükleme sorunlarını, dosya sistemi sorunlarını veya ağ sorunlarını araştırmak için yararlı olabilir. Arka planda daha az hizmet çalışabilir ve runlevel'e bağlı olarak bir dosya sistemi otomatik olarak monte edilemeyebilir.

Tek kullanıcı modu, VM'nizin yalnızca oturum açmak için SSH anahtarlarını kabul etmek üzere yapılandırılabileceği durumlarda da yararlıdır. Bu durumda, parola kimlik doğrulaması içeren bir hesap oluşturmak için tek kullanıcı modunu kullanabilirsiniz.

Tek kullanıcı moduna girmek için, VM'niz başlatılırken GRUB'a girmeniz ve GRUB'daki önyükleme yapılandırmasını değiştirmeniz gerekir. Bu, VM seri konsolu ile yapılabilir.

## <a name="general-grub-access"></a>Genel GRUB erişimi
GRUB'a erişmek için seri konsol bıçağını açık tutarken VM'nizi yeniden başlatmanız gerekir. Bazı dağıtımlar GRUB'u göstermek için klavye girişi gerektirirken, diğerleri grub'u otomatik olarak birkaç saniye gösterir ve kullanıcı klavyegirişinin zaman alacaktır. 

Tek kullanıcı moduna erişebilmek için VM'nizde GRUB'un etkin olduğundan emin olmak isteyeceksiniz. Dağıtıma bağlı olarak, GRUB'un etkin olduğundan emin olmak için bazı kurulum çalışmaları olabilir. Dağıtıma özel bilgilere aşağıdan ulaşabilirsiniz.

### <a name="reboot-your-vm-to-access-grub-in-serial-console"></a>Seri Konsol'da GRUB'a erişmek için VM'nizi yeniden başlatın
SysRq etkinse vm'inizi seri konsol bıçağı açık `'b'` olarak yeniden başlatmanız [sysRq](./serial-console-nmi-sysrq.md) komutuyla veya Genel Bakış bıçağındaki Yeniden Başlat düğmesine tıklayarak yapılabilir (Seri konsol bıçağını kapatmadan yeniden başlatmak için vm'yi yeni bir tarayıcı sekmesinde açın). Yeniden başlatırken GRUB'dan ne beklemeniz gerektiğini öğrenmek için aşağıdaki dağıtıma özgü yönergeleri izleyin.

## <a name="general-single-user-mode-access"></a>Genel Tek Kullanıcı Modu erişimi
Parola kimlik doğrulaması içeren bir hesabı yapılandırmadığınız durumlarda tek kullanıcı moduna manuel erişim gerekebilir. Tek kullanım moduna el ile girmek için GRUB yapılandırmasını değiştirmeniz gerekir. Bunu yaptıktan sonra, sıfırlamak veya daha fazla talimat için parola eklemek için Tek Kullanıcı Modunu Kullan'a bakın.

VM'nin önyükleme yapamadığı durumlarda, dağıtımlar genellikle sizi otomatik olarak tek kullanıcı moduna veya acil durum moduna düşürür. Ancak diğerleri, sizi otomatik olarak tek kullanıcılı veya acil durum moduna alabilmeleri için ek kurulum gerektirir (örneğin, kök parola ayarlama).

### <a name="use-single-user-mode-to-reset-or-add-a-password"></a>Parolayı sıfırlamak veya eklemek için Tek Kullanıcı Modunu kullanma
Tek kullanıcı moduna geçtikten sonra sudo ayrıcalıklarına sahip yeni bir kullanıcı eklemek için aşağıdakileri yapın:
1. Kullanıcı `useradd <username>` eklemek için çalıştırın
1. Yeni `sudo usermod -a -G sudo <username>` kullanıcı kök ayrıcalıkları vermek için çalıştırın
1. Yeni `passwd <username>` kullanıcının parolasını ayarlamak için kullanın. Daha sonra yeni kullanıcı olarak oturum açabilirsiniz

## <a name="access-for-red-hat-enterprise-linux-rhel"></a>Red Hat Enterprise Linux (RHEL) için erişim
RHEL, normal olarak önyükleme yapamıyorsa sizi otomatik olarak tek kullanıcı moduna düşürür. Ancak, tek kullanıcı modu için kök erişimi ayarlamadıysanız, kök parolanız olmaz ve oturum açamaz. Bir geçici çözüm vardır (Bkz. 'El ile tek kullanıcı modu' giriyor), ancak öneri başlangıçta kök erişimi kurmaktır.

### <a name="grub-access-in-rhel"></a>RHEL'de GRUB erişimi
RHEL, Kutunun dışında grub özellikli olarak gelir. GRUB'a girmek için VM'nizi yeniden başlatın `sudo reboot` ve herhangi bir tuşa basın. GRUB ekranının ortaya çıkarken göreceksiniz.

> Not: Red Hat ayrıca Kurtarma Modu, Acil Durum Modu, Hata Ayıklama Modu ve kök parolayı sıfırlama için belgeler sağlar. [Erişmek için buraya tıklayın.](https://aka.ms/rhel7grubterminal)

### <a name="set-up-root-access-for-single-user-mode-in-rhel"></a>RHEL'de tek kullanıcı modu için kök erişimi ayarlama
RHEL'deki tek kullanıcılı mod, varsayılan olarak devre dışı bırakılan kök kullanıcının etkinleştirilmesini gerektirir. Tek kullanıcı modunu etkinleştirme niz gerekiyorsa, aşağıdaki yönergeleri kullanın:

1. SSH üzerinden Red Hat sistemine giriş yapın
1. Köke geçme
1. Kök kullanıcı için parolayı etkinleştirme 
    * `passwd root`(güçlü bir kök parola ayarlayın)
1. Kök kullanıcının yalnızca ttyS0 üzerinden oturum açabilmesini sağlayın
    * `edit /etc/ssh/sshd_config`ve PermitRootLogIn'in
    * `edit /etc/securetty file`sadece ttyS0 üzerinden giriş yapmak için 

Şimdi sistem tek kullanıcı moduna önlerse kök şifre ile giriş yapabilirsiniz.

RHEL 7.4+ veya 6.9+ için alternatif olarak GRUB istemlerinde tek kullanıcı modunu etkinleştirebilirsiniz, [burada](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single) talimatlara bakın

### <a name="manually-enter-single-user-mode-in-rhel"></a>RHEL'de tek kullanım moduna el ile girin
Yukarıdaki talimatlarla GRUB ve kök erişimi ayarladıysanız, aşağıdaki yönergeleri içeren tek kullanıcı moduna girebilirsiniz:

1. GRUB girmek için VM'yi yeniden başlatırken 'Esc' tuşuna basın
1. GRUB'da, önyükleme yapmak istediğiniz seçili işletim sistemi (genellikle ilk satır) için 'e' tuşuna basın
1. Çekirdek çizgisini bulun - Azure'da, bu işlem`linux16`
1. Satırın sonuna gitmek için Ctrl + E tuşuna basın
1. Satırın sonuna aşağıdakileri ekleyin:`systemd.unit=rescue.target`
    * Bu, sizi tek kullanıcı moduna başlatacaktır. Acil durum modunu kullanmak `systemd.unit=emergency.target` istiyorsanız, satırın sonuna`systemd.unit=rescue.target`
1. Uygulanan ayarlarla çıkmak ve yeniden başlatmak için Ctrl + X tuşuna basın
1. Tek kullanıcı moduna girmeden önce yönetici parolası istenir - bu yukarıdaki talimatlarda oluşturduğunuz parolanın aynısI    

    ![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-enter-emergency-shell.gif)

### <a name="enter-single-user-mode-without-root-account-enabled-in-rhel"></a>RHEL'de kök hesabı etkin olmadan tek kullanıcı moduna girin
Kök kullanıcıyı etkinleştirmek için yukarıdaki adımları atamadıysanız, kök parolanızı yine de sıfırlayabilirsiniz. Aşağıdaki talimatları kullanın:

> Not: SELinux kullanıyorsanız, lütfen kök parolayı sıfırlarken [red](https://aka.ms/rhel7grubterminal) hat belgelerinde açıklanan ek adımları attığınıza emin olun.

1. GRUB girmek için VM'yi yeniden başlatırken 'Esc' tuşuna basın
1. GRUB'da, önyükleme yapmak istediğiniz seçili işletim sistemi (genellikle ilk satır) için 'e' tuşuna basın
1. Çekirdek çizgisini bulun - Azure'da, bu işlem`linux16`
1. Satırın sonuna ekle, `rd.break` daha önce `rd.break` bir boşluk olmasını sağlar (aşağıdaki örneğe bakın)
    - Bu, denetim buradan , [Red](https://aka.ms/rhel7rootpassword) `initramfs` Hat `systemd`belgelerinde açıklandığı gibi, önyükleme işlemini kesintiye uğratacaktır.
1. Uygulanan ayarlarla çıkmak ve yeniden başlatmak için Ctrl + X tuşuna basın
1. Önyükleme yi başlatTınktan sonra, salt okunur dosya sistemiyle acil durum moduna düşersiniz. Okuma/yazma izinleri ile kök dosya sistemini yeniden monte etmek için kabuğun içine girin `mount -o remount,rw /sysroot`
1. Tek kullanıcı moduna önyükleme `chroot /sysroot` yaptıktan sonra, `sysroot` hapishaneye geçmek için yazın
1. Artık köksün. Kök parolanızı sıfırlayabilir `passwd` ve ardından tek kullanıcı moduna girmek için yukarıdaki yönergeleri kullanabilirsiniz. İşlem `reboot -f` bittikten sonra yeniden başlatmak için yazın.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-emergency-mount-no-root.gif)

> Not: Yukarıdaki talimatlar anına kadar koşmak sizi acil durum kabuğuna `fstab`düşürür, böylece düzenleme gibi görevleri de gerçekleştirebilirsiniz. Ancak, genel olarak kabul edilen öneri kök parolanızı sıfırlamak ve bunu tek kullanıcı moduna girmek için kullanmaktır. 


## <a name="access-for-centos"></a>CentOS için erişim
Red Hat Enterprise Linux gibi CentOS'taki tek kullanıcı modu da GRUB'un ve kök kullanıcının etkin olmasını gerektirir. 

### <a name="grub-access-in-centos"></a>CentOS'ta GRUB erişimi
CentOS, GRUB özellikli kutusundan çıktı. GRUB'a girmek için VM'nizi yeniden başlatın `sudo reboot` ve herhangi bir tuşa basın. GRUB ekranının ortaya çıkarken göreceksiniz.

### <a name="single-user-mode-in-centos"></a>CentOS'ta tek kullanıcı modu
CentOS'ta tek kullanıcı modunu etkinleştirmek için yukarıdaki RHEL yönergelerini izleyin.

## <a name="access-for-ubuntu"></a>Ubuntu'ya erişim 
Ubuntu görüntüleri kök parola gerektirmez. Sistem tek kullanıcı moduna giriyorsa, ek kimlik bilgileri olmadan kullanabilirsiniz. 

### <a name="grub-access-in-ubuntu"></a>Ubuntu'da GRUB erişimi
GRUB'a erişmek için VM başlatılırken 'Esc' tuşuna basın ve basılı tutun. 

Varsayılan olarak, Ubuntu görüntüleri GRUB ekranını otomatik olarak göstermez. Bu, aşağıdaki talimatlarla değiştirilebilir:
1. Seçtiğiniz `/etc/default/grub.d/50-cloudimg-settings.cfg` bir metin düzenleyicisinde açın
1. `GRUB_TIMEOUT` Değeri sıfır olmayan bir değerle değiştirme
1. Seçtiğiniz `/etc/default/grub` bir metin düzenleyicisinde açın
1. Satırı yorumla `GRUB_HIDDEN_TIMEOUT=1`
1. `sudo update-grub` öğesini çalıştırın

### <a name="single-user-mode-in-ubuntu"></a>Ubuntu'da tek kullanıcı modu
Ubuntu, normal olarak önyükleme yapamıyorsa sizi otomatik olarak tek kullanıcı moduna alacaktır. Tek kullanımlık moduel olarak girmek için aşağıdaki yönergeleri kullanın:

1. GRUB'dan önyükleme girişinizi (Ubuntu girişi) (Ubuntu girişi) için 'e' tuşuna basın
1. Ile `linux`başlayan satırı arayın , sonra arayın`ro`
1. Sonra `single` `ro`ekle , önce ve sonra bir boşluk olmasını sağlamak`single`
1. Bu ayarlarla yeniden başlatmak için Ctrl + X tuşuna basın ve tek kullanıcı moduna girin

## <a name="access-for-coreos"></a>CoreOS için erişim
CoreOS'taki tek kullanıcı modu, GRUB'un etkinleştirilmesini gerektirir. 

### <a name="grub-access-in-coreos"></a>CoreOS'ta GRUB erişimi
GRUB'a erişmek için, VM'niz açılırken herhangi bir tuşa basın.

### <a name="single-user-mode-in-coreos"></a>CoreOS'ta tek kullanıcı modu
CoreOS, normal olarak önyükleme yapamıyorsa sizi otomatik olarak tek kullanıcı moduna düşürür. Tek kullanımlık moduel olarak girmek için aşağıdaki yönergeleri kullanın:
1. GRUB'dan önyükleme girişinizi yapmak için 'e' tuşuna basın
1. 'ile `linux$`başlayan satırı arayın. Farklı if/else yan tümcelerinde kapsüllenmiş 2 tane olmalıdır
1. Her `coreos.autologin=ttyS0` iki `linux$` satırın sonuna ek
1. Bu ayarlarla yeniden başlatmak için Ctrl + X tuşuna basın ve tek kullanıcı moduna girin

## <a name="access-for-suse-sles"></a>SUSE SLES için erişim
SLES 12 SP3+'ın yeni görüntüleri, sistemin acil durum moduna girmesi durumunda seri konsol üzerinden erişime olanak sağlar. 

### <a name="grub-access-in-suse-sles"></a>SUSE SLES'te GRUB erişimi
SLES'teki GRUB erişimi, YaST üzerinden bootloader yapılandırması gerektirir. Bunu yapmak için aşağıdaki yönergeleri izleyin:

1. ssh içine SLES VM `sudo yast bootloader`ve çalıştırın. Menüde `tab` gezinmek için tuş, `enter` tuş ve ok tuşlarını kullanın. 
1. `Kernel Parameters`Gidin ve denetleyin. `Use serial console` 
1. Konsol `serial --unit=0 --speed=9600 --parity=no` bağımsız değişkenlerine ekleme

1. Ayarlarınızı kaydetmek ve çıkmak için F10 tuşuna basın
1. GRUB'a girmek için VM'nizi yeniden başlatın ve Önyükleme sırasında herhangi bir tuşa basın ve GRUB'un ekranda kalmasını sağlamak için
    - GRUB için varsayılan zaman ası 1'dir. Değişkeni `GRUB_TIMEOUT` değiştirerek bunu değiştirebilirsiniz`/etc/default/grub`

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-sles-yast-grub-config.gif)

### <a name="single-user-mode-in-suse-sles"></a>SUSE SLES'te tek kullanıcı modu
SLES normal olarak önyükleme yapamıyorsa otomatik olarak acil durum kabuğuna bırakılırsınız. Acil durum kabuğunu el ile girmek için aşağıdaki talimatları kullanın:

1. GRUB'dan önyükleme girişinizi (SLES girişi) (SLES girişi) için 'e' tuşuna basın
1. Ile başlayacak çekirdek hattı arayın`linux`
1. Satırın `systemd.unit=emergency.target` sonuna ek
1. Bu ayarlarla yeniden başlatmak için Ctrl + X tuşuna basın ve acil durum kabuğunu girin
   > _Salt okunur_ dosya sistemiyle acil durum kabuğuna bırakılacağınızı unutmayın. Herhangi bir dosyada herhangi bir düzenleme yapmak istiyorsanız, dosya sistemini okuma yazma izinleriyle yeniden monte etmeniz gerekir. Bunu yapmak için, kabuk girin `mount -o remount,rw /`

## <a name="access-for-oracle-linux"></a>Oracle Linux'a erişim
Red Hat Enterprise Linux gibi Oracle Linux'taki tek kullanıcı modu da GRUB'un ve kök kullanıcının etkin olmasını gerektirir. 

### <a name="grub-access-in-oracle-linux"></a>Oracle Linux'ta GRUB erişimi
Oracle Linux grub kutunun dışında etkin ile birlikte geliyor. GRUB'a girmek için VM'nizi yeniden başlatın `sudo reboot` ve 'Esc' tuşuna basın. GRUB ekranının ortaya çıkarken göreceksiniz.

### <a name="single-user-mode-in-oracle-linux"></a>Oracle Linux'ta tek kullanıcı modu
Oracle Linux'ta tek kullanıcı modunu etkinleştirmek için yukarıdaki RHEL yönergelerini izleyin.

## <a name="next-steps"></a>Sonraki adımlar
* Ana seri konsol Linux dokümantasyon sayfası [burada](serial-console.md)yer almaktadır.
* [NMI ve SysRq aramaları](serial-console-nmi-sysrq.md) için Seri Konsol kullanın
* Seri Konsol, [Windows](../windows/serial-console.md) VM'ler için de kullanılabilir
* [Önyükleme tanılama](boot-diagnostics.md) hakkında daha fazla bilgi edinin
