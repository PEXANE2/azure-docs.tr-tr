---
title: GRUB ve tek kullanıcı modu için Azure seri konsolu
description: Azure sanal makineler 'de grub için seri konsol kullanma.
services: virtual-machines-linux
author: asinn826
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.date: 08/14/2018
ms.author: alsin
ms.openlocfilehash: d594f4d8019a7c23da79506cd702adbe9f25038d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87028950"
---
# <a name="use-serial-console-to-access-grub-and-single-user-mode"></a>GRUB ve tek kullanıcı moduna erişmek için seri konsol kullanma
GRUB, genel olarak birleştirilmiş önyükleme yükleyicisinden. GRUB 'den önyükleme yapılandırmanızı, diğer şeyler arasında tek kullanıcı modunda önyüklenecek şekilde değiştirebilirsiniz.

Tek Kullanıcı modu, en az işlevselliğe sahip en az bir ortamdır. Önyükleme sorunlarını, dosya sistemi sorunlarını veya ağ sorunlarını araştırmak için yararlı olabilir. Arka planda daha az hizmet çalışabilir ve Runlevel 'a bağlı olarak, bir dosya sistemi otomatik olarak takılmayabilir.

Tek Kullanıcı modu, sanal makinenizin yalnızca oturum açmak için SSH anahtarlarını kabul edecek şekilde yapılandırılabileceği durumlarda da yararlıdır. Bu durumda, parola kimlik doğrulamasıyla bir hesap oluşturmak için tek kullanıcı modunu kullanabilirsiniz.

Tek Kullanıcı modunu girmek için, VM 'niz önyüklenirken GRUB girmeniz ve GRUB 'de önyükleme yapılandırmasını değiştirmeniz gerekir. Bu işlem, VM seri konsolu ile yapılabilir.

## <a name="general-grub-access"></a>Genel GRUB erişimi
GRUB 'ye erişmek için, seri konsol dikey penceresini açık tutarken sanal makinenizi yeniden başlatmanız gerekir. Bazı destekler, GRUB 'yi göstermek için klavye girişi gerektirecektir, diğerleri bir kaç saniye için otomatik olarak GRUB 'yi gösterecek ve Kullanıcı klavye girişinin zaman aşımını iptal edebilmesini sağlayacak. 

Tek kullanıcı moduna erişebilmek için sanal makinenizde GRUB 'nin etkinleştirildiğinden emin olmanız gerekir. Disillerinize bağlı olarak, GRUB 'nin etkinleştirildiğinden emin olmak için bazı kurulum işleri olabilir. Aşağıdaki bilgileri aşağıda bulabilirsiniz.

### <a name="reboot-your-vm-to-access-grub-in-serial-console"></a>Seri konsolundaki GRUB 'ye erişmek için sanal makinenizi yeniden başlatın
VM 'nizi seri konsol dikey penceresinde `'b'` yeniden başlatmak, [SySRq](./serial-console-nmi-sysrq.md) etkin olduğunda bir SySRq komutuyla veya genel bakış dikey penceresinde (seri konsol dikey penceresini kapatmadan YENIDEN başlatmak için VM 'yi yeni bir tarayıcı sekmesinde aç) tıklatarak çalıştırabilirsiniz. ' İ yeniden başlattığınızda, GRUB 'den ne beklendiğini öğrenmek için aşağıdaki özel yönergeleri izleyin.

## <a name="general-single-user-mode-access"></a>Genel tek kullanıcı modu erişimi
Parola kimlik doğrulamasıyla bir hesap yapılandırmadığınız durumlarda, tek kullanıcı moduna el ile erişim gerekli olabilir. Tek kullanıcı moduna el ile girmek için GRUB yapılandırmasını değiştirmeniz gerekir. Bunu yaptıktan sonra, daha fazla yönerge için bir parolayı sıfırlamak veya bir parola eklemek için bkz. tek kullanıcı modunu kullanma.

VM 'nin önyüklenebildiği durumlarda, distro 'lara genellikle sizi otomatik olarak tek kullanıcı moduna veya acil durum moduna geçirecektir. Ancak diğerleri, sizi otomatik olarak tek kullanıcı veya Acil moduna bırakmadan önce ek kurulum gerektirir (bir kök parola ayarlama gibi).

### <a name="use-single-user-mode-to-reset-or-add-a-password"></a>Bir parolayı sıfırlamak veya eklemek için tek kullanıcı modunu kullanın
Tek Kullanıcı modundayken, sudo ayrıcalıklarına sahip yeni bir kullanıcı eklemek için aşağıdakileri yapın:
1. `useradd <username>`Kullanıcı eklemek için Çalıştır
1. `sudo usermod -a -G sudo <username>`Yeni Kullanıcı kök ayrıcalıklarına izin vermek için Çalıştır
1. `passwd <username>`Yeni kullanıcının parolasını ayarlamak için kullanın. Daha sonra yeni kullanıcı olarak oturum açabilirsiniz

## <a name="access-for-red-hat-enterprise-linux-rhel"></a>Red Hat Enterprise Linux erişimi (RHEL)
RHEL, normal şekilde önyükleme yapamıyor, sizi otomatik olarak tek kullanıcı moduna bırakacak. Ancak, tek kullanıcı modu için kök erişimi ayarlanmamışsa, kök parolaya sahip olmayacaktır ve oturum açabilirsiniz. Geçici bir çözüm vardır (bkz. ' El Ile tek kullanıcı moduna girme '), ancak öneri öncelikle kök erişimi ayarlamadır.

### <a name="grub-access-in-rhel"></a>RHEL 'de GRUB erişimi
RHEL, kutudan çıkar seçeneğiyle birlikte gelir. GRUB girmek için VM 'nizi ile yeniden başlatın `sudo reboot` ve herhangi bir tuşa basın. GRUB ekranının gösterilmesini görürsünüz.

> Note: Red hat, kurtarma modu, acil durum modu, hata ayıklama modu ve kök parolayı sıfırlama için de belgeler sağlar. [Erişmek için buraya tıklayın](https://aka.ms/rhel7grubterminal).

### <a name="set-up-root-access-for-single-user-mode-in-rhel"></a>RHEL 'de tek kullanıcı modu için kök erişimi ayarlama
RHEL 'deki tek kullanıcılı mod, kök kullanıcının etkinleştirilmesini gerektirir, bu varsayılan olarak devre dışıdır. Tek Kullanıcı modunu etkinleştirmeniz gerekiyorsa aşağıdaki yönergeleri kullanın:

1. SSH aracılığıyla Red Hat sisteminde oturum açma
1. Köke geç
1. Kök kullanıcı için parolayı etkinleştir 
    * `passwd root`(güçlü bir kök parolası ayarla)
1. Kök kullanıcının yalnızca ttyS0 aracılığıyla oturum açabildiğinden emin olun
    * `edit /etc/ssh/sshd_config`ve Permitrootlogın 'in Hayır olarak ayarlandığından emin olun
    * `edit /etc/securetty file`yalnızca ttyS0 aracılığıyla oturum açmaya izin vermek için 

Artık sistem tek kullanıcı modunda önyükleniyorsa kök parolasıyla oturum açabilirsiniz.

Alternatif olarak, RHEL 7.4 + veya 6.9 + için alternatif olarak, GRUB istemlerinde tek kullanıcı modunu etkinleştirebilirsiniz, [buradaki](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single) yönergelere bakın

### <a name="manually-enter-single-user-mode-in-rhel"></a>RHEL 'ye el ile tek kullanıcı modu girin
Yukarıdaki yönergelerle bir GRUB ve kök erişim ayarladıysanız, aşağıdaki yönergelerle tek bir Kullanıcı modu girebilirsiniz:

1. GRUB girmek için VM 'yi yeniden başlatırken ' Esc ' tuşuna basın
1. GRUB 'de, önyüklemek istediğiniz işletim sistemini düzenlemek için ' e ' düğmesine basın (genellikle ilk satır)
1. Çekirdek satırını bulma-Azure 'da bu işlem ile başlar`linux16`
1. Satırın sonuna gitmek için CTRL + E tuşlarına basın
1. Aşağıdaki satırı sonuna ekleyin:`systemd.unit=rescue.target`
    * Bu, sizi tek kullanıcı modunda önyükler. Acil durum modunu kullanmak istiyorsanız, `systemd.unit=emergency.target` bunun yerine satırın sonuna ekleyin`systemd.unit=rescue.target`
1. Çıkmak için CTRL + X tuşlarına basın ve uygulanan ayarlarla yeniden başlatın
1. Tek kullanıcı moduna girebilmek için önce yönetici parolasını girmeniz istenir-bu, yukarıdaki yönergelerde oluşturduğunuz parolayla aynı olur    

    ![Bir komut satırı arabirimini gösteren animasyonlu resim. Kullanıcı bir sunucu seçer, çekirdek çizginin sonunu bulur ve sonra belirtilen metni girer.](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-enter-emergency-shell.gif)

### <a name="enter-single-user-mode-without-root-account-enabled-in-rhel"></a>RHEL 'de kök hesabı etkin olmadan tek kullanıcı modu girin
Kök kullanıcıyı etkinleştirmek için yukarıdaki adımlara gitmediyseniz, yine de kök parolanızı sıfırlamayı seçebilirsiniz. Aşağıdaki yönergeleri kullanın:

> Note: SELinux kullanıyorsanız, kök parolayı sıfırlarken [buradaki](https://aka.ms/rhel7grubterminal) Red Hat belgelerinde açıklanan ek adımları gerçekleştirdiğinizden emin olun.

1. GRUB girmek için VM 'yi yeniden başlatırken ' Esc ' tuşuna basın
1. GRUB 'de, önyüklemek istediğiniz işletim sistemini düzenlemek için ' e ' düğmesine basın (genellikle ilk satır)
1. Çekirdek satırını bulma-Azure 'da bu işlem ile başlar`linux16`
1. `rd.break`Daha önce bir boşluk olduğundan emin olmak için satırın sonuna ekleyin `rd.break` (aşağıdaki örneğe bakın)
    - Bu, `initramfs` `systemd` [burada](https://aka.ms/rhel7rootpassword)Red Hat belgelerinde açıklandığı gibi, denetim öğesinden öğesine geçirilmeden önce önyükleme işlemini kesintiye uğratır.
1. Çıkmak için CTRL + X tuşlarına basın ve uygulanan ayarlarla yeniden başlatın
1. Önyükleme yaptıktan sonra, salt bir dosya sistemi ile acil durum moduna alınacaktır. `mount -o remount,rw /sysroot`Kök dosya sistemini okuma/yazma izinleriyle yeniden bağlamak için kabuğa girin
1. Tek Kullanıcı modunda önyükleme yaptıktan sonra, `chroot /sysroot` jailbreak uygulanmış 'e geçiş yapmak için yazın `sysroot`
1. Artık köküdür. Kök parolanızı ile sıfırlayabilir `passwd` ve sonra tek bir Kullanıcı modu girmek için yukarıdaki yönergeleri kullanabilirsiniz. `reboot -f`İşiniz bittiğinde yeniden başlatmak için yazın.

![Bir komut satırı arabirimini gösteren animasyonlu resim. Kullanıcı bir sunucu seçer, çekirdek çizginin sonunu bulur ve belirtilen komutları girer.](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-emergency-mount-no-root.gif)

> Göz atın: Yukarıdaki yönergelerden çalıştırılması sizi acil durum kabuğuna bırakacak, bu sayede de Düzenle gibi görevler gerçekleştirebilirsiniz `fstab` . Bununla birlikte, genellikle kabul edilen öneri, kök parolanızı sıfırlayıp tek kullanıcı moduna girmek için bunu kullanmaktır. 


## <a name="access-for-centos"></a>CentOS erişimi
Red Hat Enterprise Linux benzer şekilde, CentOS 'daki tek kullanıcılı mod, GRUB ve kök kullanıcının etkinleştirilmesini gerektirir. 

### <a name="grub-access-in-centos"></a>CentOS 'da GRUB erişimi
CentOS, kutudan çıkan GRUB ile birlikte gelir. GRUB girmek için VM 'nizi ile yeniden başlatın `sudo reboot` ve herhangi bir tuşa basın. GRUB ekranının gösterilmesini görürsünüz.

### <a name="single-user-mode-in-centos"></a>CentOS 'da tek kullanıcılı mod
CentOS 'da tek kullanıcı modunu etkinleştirmek için yukarıdaki RHEL yönergelerini izleyin.

## <a name="access-for-ubuntu"></a>Ubuntu erişimi 
Ubuntu görüntüleri kök parolası gerektirmez. Sistem tek kullanıcı modunda önyükleniyorsa, ek kimlik bilgileri olmadan kullanabilirsiniz. 

### <a name="grub-access-in-ubuntu"></a>Ubuntu 'da GRUB erişimi
GRUB 'ye erişmek için VM 'nin önyüklemesi sırasında ' Esc ' tuşuna basın ve basılı tutun. 

Varsayılan olarak, Ubuntu görüntüleri, GRUB ekranını otomatik olarak göstermez. Bu, aşağıdaki yönergelerle değiştirilebilir:
1. `/etc/default/grub.d/50-cloudimg-settings.cfg`Seçtiğiniz bir metin düzenleyicisinde açın
1. `GRUB_TIMEOUT`Değeri sıfır olmayan bir değerle değiştirin
1. `/etc/default/grub`Seçtiğiniz bir metin düzenleyicisinde açın
1. `GRUB_HIDDEN_TIMEOUT=1`Satırı açıklama
1. `sudo update-grub` komutunu çalıştırın

### <a name="single-user-mode-in-ubuntu"></a>Ubuntu 'da tek kullanıcı modu
Ubuntu, normal şekilde önyükleme yapmam durumunda sizi otomatik olarak tek kullanıcı moduna bırakacak. Tek kullanıcı moduna el ile girmek için aşağıdaki yönergeleri kullanın:

1. GRUB 'den, önyükleme girdinizi (Ubuntu girişi) düzenlemek için ' e ' düğmesine basın
1. İle başlayan satırı bulun `linux` ve şuna bakın`ro`
1. `single`Sonra `ro` ve sonra bir boşluk olduğundan emin olmak için daha sonra ekleyin`single`
1. Bu ayarlarla yeniden başlatmak için CTRL + X tuşlarına basın ve tek kullanıcı modunu girin

## <a name="access-for-coreos"></a>CoreOS erişimi
CoreOS 'daki tek kullanıcılı mod, GRUB 'nin etkinleştirilmesini gerektirir. 

### <a name="grub-access-in-coreos"></a>CoreOS 'ta GRUB erişimi
GRUB 'ye erişmek için VM 'niz önyüklenirken herhangi bir tuşa basın.

### <a name="single-user-mode-in-coreos"></a>CoreOS 'de tek kullanıcı modu
CoreOS, normal olarak önyüklenemediğinde sizi otomatik olarak tek kullanıcı moduna yükler. Tek kullanıcı moduna el ile girmek için aşağıdaki yönergeleri kullanın:
1. GRUB 'den, önyükleme girdinizi düzenlemek için ' e ' düğmesine basın
1. İle başlayan satırı bulun `linux$` . Farklı if/else yan tümcelerinde kapsüllenmiş 2 olmalıdır
1. `coreos.autologin=ttyS0`Her iki satırın sonuna ekle `linux$`
1. Bu ayarlarla yeniden başlatmak için CTRL + X tuşlarına basın ve tek kullanıcı modunu girin

## <a name="access-for-suse-sles"></a>SUSE SLES için erişim
SLES 12 SP3 + ' un daha yeni görüntüleri, sistemin acil durum modunda önyüklemesinde seri konsol aracılığıyla erişime izin verir. 

### <a name="grub-access-in-suse-sles"></a>SUSE SLES 'de GRUB erişimi
SLES 'deki GRUB erişimi, YaST aracılığıyla önyükleme yükleyicisi yapılandırmasını gerektirir. Bunu yapmak için şu yönergeleri izleyin:

1. SLES sanal makinenize SSH ekleyin ve çalıştırın `sudo yast bootloader` . `tab` `enter` Menüde gezinmek için anahtar, anahtar ve ok tuşlarını kullanın. 
1. Gidin `Kernel Parameters` ve denetleyin `Use serial console` . 
1. `serial --unit=0 --speed=9600 --parity=no`Konsol bağımsız değişkenlerine ekleme

1. Ayarlarınızı kaydetmek ve çıkmak için F10 tuşuna basın
1. GRUB girmek için sanal makinenizi yeniden başlatın ve önyükleme sırası sırasında herhangi bir tuşa basarak GRUB 'nin ekranda kalmasını sağlayın
    - GRUB için varsayılan zaman aşımı 1 ' dir. Bunu, içindeki değişkenini değiştirerek değiştirebilirsiniz. `GRUB_TIMEOUT``/etc/default/grub`

![Bir komut satırı arabirimini gösteren animasyonlu resim. Kullanıcı belirtilen metni girer, belirtilen seçeneği seçer ve ayarları kaydeder.](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-sles-yast-grub-config.gif)

### <a name="single-user-mode-in-suse-sles"></a>SUSE SLES 'de tek kullanıcı modu
SLES normal şekilde önyüklenemediğinde, otomatik olarak acil durum kabuğu 'na bırakılır. Acil durum kabuğunu el ile girmek için aşağıdaki yönergeleri kullanın:

1. GRUB 'den, önyükleme girdinizi (SLES girişi) düzenlemek için ' e ' düğmesine basın
1. Başlamak için kullanılacak çekirdek hattını arayın`linux`
1. `systemd.unit=emergency.target`Satırın sonuna ekle
1. Bu ayarlarla yeniden başlatmak için CTRL + X tuşlarına basın ve acil durum kabuğu girin
   > _Salt okunabilir_ bir dosya sistemiyle acil durum kabuğu 'na bırakılacağını unutmayın. Herhangi bir dosyada herhangi bir düzenleme yapmak istiyorsanız, dosya sistemini okuma-yazma izinleriyle yeniden bağlayın. Bunu yapmak için, `mount -o remount,rw /` kabuğa girin

## <a name="access-for-oracle-linux"></a>Oracle Linux için erişim
Red Hat Enterprise Linux benzer şekilde, Oracle Linux tek bir Kullanıcı modu, GRUB ve kök kullanıcının etkinleştirilmesini gerektirir. 

### <a name="grub-access-in-oracle-linux"></a>Oracle Linux erişim
Oracle Linux, kutudan çıkan GRUB ile birlikte gelir. GRUB girmek için VM 'nizi ile yeniden başlatın `sudo reboot` ve ' Esc ' tuşuna basın. GRUB ekranının gösterilmesini görürsünüz.

### <a name="single-user-mode-in-oracle-linux"></a>Oracle Linux 'de tek kullanıcı modu
Oracle Linux ' de tek kullanıcı modunu etkinleştirmek için yukarıdaki RHEL yönergelerini izleyin.

## <a name="next-steps"></a>Sonraki adımlar
* Ana seri konsol Linux belge sayfası [burada](../troubleshooting/serial-console-linux.md)bulunur.
* [NMI ve SysRq çağrıları](serial-console-nmi-sysrq.md) Için seri konsol kullan
* Seri konsol [Windows](../troubleshooting/serial-console-windows.md) VM 'leri için de kullanılabilir
* [Önyükleme tanılaması](../troubleshooting/boot-diagnostics.md) hakkında daha fazla bilgi
