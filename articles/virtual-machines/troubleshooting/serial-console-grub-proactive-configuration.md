---
title: Azure Seri Konsol proaktif GRUB yapılandırması| Microsoft Dokümanlar
description: Azure sanal makinelerinde tek kullanıcı ve kurtarma modu erişimine izin veren çeşitli dağıtımlarda GRUB'u yapılandırın.
services: virtual-machines-linux
documentationcenter: ''
author: vilibert
manager: spogge
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/10/2019
ms.author: vilibert
ms.openlocfilehash: a154ab4742f0d0d7acae0376bcf894bc2b62b4cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74186935"
---
# <a name="proactively-ensuring-you-have-access-to-grub-and-sysrq-could-save-you-lots-of-down-time"></a>Proaktif GRUB ve sysrq erişimi nizi sağlamak size çok fazla zaman kazandırabilir

Seri Konsol ve GRUB erişimi olması çoğu durumda IaaS Linux Sanal Makine kurtarma süreleri artıracaktır. GRUB, aksi takdirde VM'nizi kurtarmanın daha uzun süreceğini kurtarma seçenekleri sunar. 


VM kurtarma gerçekleştirmek için nedenler çoktur ve aşağıdaki gibi senaryolara atfedilebilir:

   - Bozuk dosya sistemleri/çekirdek/MBR (Ana Önyükleme Kaydı)
   - Başarısız çekirdek yükseltmeleri
   - Yanlış GRUB çekirdeği parametreleri
   - Yanlış fstab yapılandırmaları
   - Güvenlik duvarı yapılandırmaları
   - Kayıp parola
   - Mangled sshd yapılandırmaları dosyaları
   - Ağ yapılandırmaları

 [Burada](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux#common-scenarios-for-accessing-the-serial-console) ayrıntılı olarak birçok diğer senaryolar

Azure'da dağıtılan VM'lerinizde GRUB'a ve Seri konsoluna erişebileceğinizi doğrulayın. 

Seri Konsol'da yeniyseniz, [bu bağlantıya](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux/)bakın.

> [!TIP]
> Değişiklik yapmadan önce dosyaların yedeklerini aldığınızdan emin olun

GRUB'a eriştinmibir kez hızlı bir şekilde nasıl kurtarabileceğinizi görmek için aşağıdaki videoyu izleyin

[Linux VM Videosunu Kurtar](https://youtu.be/KevOc3d_SG4)

Linux VM'lerinin kurtarılmasına yardımcı olmak için bir dizi yöntem vardır. Bulut ortamında, bu süreç zorlu olmuştur.
Hizmetlerin hızlı bir şekilde kurtarılmasını sağlamak için araç ve özelliklerde sürekli ilerleme kaydedilmektedir.

Azure Seri Konsolu ile Linux VM'inizle bir sistemin konsolundaymuş gibi etkileşim kurabilirsiniz.

Çekirdeğin nasıl önyükleme yapacağı da dahil olmak üzere birçok yapılandırma dosyasını işleyebilirsiniz. 

Daha deneyimli Linux/Unix sys yöneticileri, Azure Seri Konsolu üzerinden erişilebilen **tek kullanıcı** ve acil **durum modlarını** takdir edecektir ve birçok kurtarma senaryosu için Disk Değiştirme ve VM silme işlemini gereksiz hale getirir.

Kurtarma yöntemi, karşılaşılan soruna bağlıdır, örneğin kaybolan veya yanlış yerleştirilen bir parola Azure portal seçenekleri ile sıfırlanabilir -> **Parolayı Sıfırla**. Parolayı **Sıfırla** özelliği Uzantı olarak bilinir ve Linux Guest aracısıyla iletişim kurar.

Custom Script gibi diğer uzantıları ancak bu seçenekler Linux **waagent** yukarı ve her zaman böyle değildir sağlıklı bir durumda olmasını gerektirir.

![ajan durumu](./media/virtual-machines-serial-console/agent-status.png)


Azure Seri Konsolu ve GRUB'a erişebildiğinizi sağlamak, parola değişikliğinin veya yanlış yapılandırmanın saatler yerine birkaç dakika içinde düzeltilebileceği anlamına gelir. Birincil çekirdeğinizin bozulduğu senaryoda diskte birden çok çekirdek olması durumunda VM'yi alternatif bir çekirdekten önyüklemeye bile zorlayabilirsiniz.

![çok çekirdek](./media/virtual-machines-serial-console/more-kernel.png)

## <a name="suggested-order-of-recovery-methods"></a>Önerilen kurtarma yöntemleri sırası:

- Azure Seri Konsolu

- Disk Swap – aşağıdakileri kullanarak otomatikhale alınabilir:

   - [Güç Kabuğu Kurtarma Komut Dosyaları](https://github.com/Azure/azure-support-scripts/tree/master/VMRecovery/ResourceManager)
   - [bash Kurtarma Scriptler](https://github.com/sribs/azure-support-scripts)

- Eski Yöntem

## <a name="disk-swap-video"></a>Disk Değiştirme Videosu:

GRUB erişiminiz yoksa [bu](https://youtu.be/m5t0GZ5oGAc) videoyu izleyin ve VM'nizi kurtarmak için disk değiştirme yordamını nasıl otomatikleştirebileceğinizi görün

## <a name="challenges"></a>Zorluklar:

Tüm Linux Azure VM'leri GRUB erişimi için varsayılan olarak yapılandırılmamıştır ve bunların tümü sysrq komutlarıyla kesintiye uğratılacak şekilde yapılandırılmamıştır. SLES 11 gibi bazı eski dağıtımlar Azure Seri Konsolunda Giriş istemini görüntülemek üzere yapılandırılmamıştır

Bu makalede, GRUB'un kullanıma sunulması konusunda çeşitli Linux dağıtımlarını ve belge yapılandırmalarını gözden geçireceğiz.




## <a name="how-to-configure-linux-vm-to-accept-sysrq-keys"></a>Linux VM'yi SysRq tuşlarını kabul etmek için yapılandırma
Sysrq tuşu varsayılan olarak bazı yeni Linux dağıtımlarında etkindir, ancak diğerlerinde yalnızca belirli SysRq işlevleri için değerleri kabul etmek üzere yapılandırılabilir.
Eski dağıtımlarda tamamen devre dışı bırakılmış olabilir.

SysRq özelliği, kilitlenmiş veya asılı bir VM'yi doğrudan Azure Seri Konsolu'ndan yeniden başlatmak için yararlıdır, Ayrıca GRUB menüsüne erişmede yardımcı olur, alternatif olarak başka bir portal penceresinden vm başlatmak veya ssh oturumu geçerli konsol bağlantınızı bırakabilir böylece GRUB menüsünü görüntülemek için kullanılan GRUB Zaman Aşımı süresi doluyor.
VM, çekirdek parametresi için 1 değerini kabul etmek üzere yapılandırılmalıdır, bu da sysrq veya 128'in tüm işlevlerini sağlar, bu da yeniden başlatma/poweroff


[Sysrq videosunu etkinleştirme](https://youtu.be/0doqFRrHz_Mc)


VM'yi Azure portalındaki SysRq komutları aracılığıyla yeniden başlatmayı kabul edecek şekilde yapılandırmak için çekirdek parametresi çekirdeği için 1 değeri ayarlamanız gerekir, çekirdek parametresi çekirdek.sysrq

Bu yapılandırmanın yeniden başlatmayı devam etmesi için **sysctl.conf** dosyasına bir giriş ekleyin

`echo kernel.sysrq = 1 >> /etc/sysctl.conf`

Çekirdek parametresini dinamik olarak yapılandırmak için

`sysctl -w kernel.sysrq=1`

**Kök** erişiminiz yoksa veya sudo bozulduysa, kabuk isteminden sysrq'u yapılandırmak mümkün olmayacaktır.

Azure portalını kullanarak bu senaryoda sysrq'u etkinleştirebilirsiniz. **Sudoers.d/waagent** dosyası kırıldıysa veya silinmişse bu yöntem yararlı olabilir.

Azure portalı İşlemleri -> Run Command -> RunShellScript özelliğini kullanarak, waagent işleminin sağlıklı olmasını gerektirir ve daha sonra sysrq'u etkinleştirmek için bu komutu enjekte edebilirsiniz

`sysctl -w kernel.sysrq=1 ; echo kernel.sysrq = 1 >> /etc/sysctl.conf`

Burada gösterildiği ![gibi: sysrq2 etkinleştirin](./media/virtual-machines-serial-console/enabling-sysrq-2.png)

Tamamlandıktan sonra, daha sonra **sysrq** erişerek deneyebilirsiniz ve bir yeniden başlatma mümkün olduğunu görmelisiniz.

![sysrq3 etkinleştirmek](./media/virtual-machines-serial-console/enabling-sysrq-3.png)

**Yeniden Başlat'ı** seçin ve **SysRq** Komutunu Gönderin

![sysrq4 etkinleştirin](./media/virtual-machines-serial-console/enabling-sysrq-4.png)

Sistem bu gibi bir sıfırlama iletisi oturum açmalıdır

![sysrq5 etkinleştirmek](./media/virtual-machines-serial-console/enabling-sysrq-5.png)


## <a name="ubuntu-grub-configuration"></a>Ubuntu GRUB yapılandırması

Varsayılan olarak VM önyükleme sırasında **Esc** tuşunu basılı tutarak GRUB'a erişebilmelisiniz, GRUB menüsü sunulmuyorsa, bu seçeneklerden birini kullanarak GRUB menüsünü Azure Seri Konsolu'nda ekranda tutabilirsiniz.

**Seçenek 1** - GRUB'u Ekranda Görüntülenecek Kuvvetler 

Belirtilen TIMEOUT için Ekranda GRUB menüsünü tutmak için /etc/default/grub.d/50-cloudimg-settings.cfg dosyasını güncelleyin.
GRUB hemen görüntüleneceği için **Esc** tuşuna basmanız gerekmez

```
GRUB_TIMEOUT=0

change to

GRUB_TIMEOUT=5
```

**Seçenek 2** - Önyüklemeden önce **Esc** tuşuna basılmasına izin verir

Benzer davranış dosya / etc/default/grub değişiklikler yaparak yaşanabilir ve **Esc** vurmak için 3 saniyelik bir zaman dilimi gözlemlemek


Bu iki satır dışarı yorum:

```
#GRUB_HIDDEN_TIMEOUT=0
#GRUB_HIDDEN_TIMEOUT_QUIET=true
```
ve bu satırı ekleyin:

```
GRUB_TIMEOUT_STYLE=countdown
```


## <a name="ubuntu-1204"></a>Ubuntu 12\.04

Ubuntu 12.04 seri konsola erişim sağlar, ancak etkileşim yeteneği sunmaz. **Giriş:** komut istemi görünmüyor


12.04 için bir giriş elde etmek **için:** istemi:
1. /etc/init/ttyS0.conf adlı bir dosya oluşturarak aşağıdaki metni içeren:

    ```
    # ttyS0 - getty
    #
    # This service maintains a getty on ttyS0 from the point the system is
    # started until it is shut down again.
    start on stopped rc RUNLEVEL=[12345]
    stop on runlevel [!12345]
    
    respawn
    exec /sbin/getty -L 115200 ttyS0 vt102
    ```    

2. Getty başlatmak için upstart isteyin     
    ```
    sudo start ttyS0
    ```
 
Ubuntu sürümleri için seri konsolu yapılandırmak için gereken ayarları [burada](https://help.ubuntu.com/community/SerialConsoleHowto) bulabilirsiniz

## <a name="ubuntu-recovery-mode"></a>Ubuntu Kurtarma Modu

GRUB üzerinden Ubuntu için ek kurtarma ve temizleme seçenekleri mevcuttur, ancak bu ayarlara yalnızca çekirdek parametrelerini buna göre yapılandırırsanız erişilebilir.
Bu çekirdek önyükleme parametresinin yapıya konulmaması, Kurtarma menüsünün Azure Seri Konsoluna değil, Azure Tanılama'ya gönderilmesini zorlar.
Aşağıdaki adımları izleyerek Ubuntu Kurtarma Menüsüne erişebilirsiniz:

BOOT İşlemini böl ve GRUB menüsüne eriş

Ubuntu için Gelişmiş Seçenekler'i seçin ve enter tuşuna basın

![ubunturec1](./media/virtual-machines-serial-console/ubunturec1.png)

Görüntüleyen satırı *seçin (kurtarma modu)* enter tuşuna basmayın ama "e" tuşuna basın

![ubunturec2](./media/virtual-machines-serial-console/ubunturec2.png)

Çekirdeği yükleyecek satırı bulun ve son parametre **nomodesetini** **konsol=ttyS0** olarak hedefle değiştirin

```
linux /boot/vmlinuz-4.15.0-1023-azure root=UUID=21b294f1-25bd-4265-9c4e-d6e4aeb57e97 ro recovery nomodeset

change to

linux /boot/vmlinuz-4.15.0-1023-azure root=UUID=21b294f1-25bd-4265-9c4e-d6e4aeb57e97 ro recovery console=ttyS0
```

![ubunturec3](./media/virtual-machines-serial-console/ubunturec3.png)

Çekirdeği başlatmak ve yüklemek için **Ctrl-x** tuşuna basın.
Her şey yolunda giderse, diğer kurtarma seçeneklerini gerçekleştirmenize yardımcı olabilecek bu ek Seçenekleri göreceksiniz

![ubunturec4](./media/virtual-machines-serial-console/ubunturec4.png)


## <a name="red-hat-grub-configuration"></a>Red Hat GRUB yapılandırması

## <a name="red-hat-74-grub-configuration"></a>Red Hat\.\+ 7 4 GRUB yapılandırması
Bu sürümlerde varsayılan /etc/default/grub yapılandırması yeterince yapılandırılmıştır

```
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL="serial console"
GRUB_SERIAL_COMMAND="serial"
GRUB_CMDLINE_LINUX="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"
GRUB_DISABLE_RECOVERY="true"
```

SysRq tuşunu etkinleştirme

```
sysctl -w kernel.sysrq=1;echo kernel.sysrq = 1 >> /etc/sysctl.conf;sysctl -a | grep -i sysrq
```

## <a name="red-hat-72-and-73-grub-configuration"></a>Red Hat\.7\.2 ve 7 3 GRUB yapılandırması
Değiştirilen dosya /etc/default/grub'dur – varsayılan bir config aşağıdaki örneğe benzer:

```
GRUB_TIMEOUT=1
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"
GRUB_DISABLE_RECOVERY="true"
```

/etc/default/grub'da aşağıdaki satırları değiştirme

```
GRUB_TIMEOUT=1 

to

GRUB_TIMEOUT=5
```


```
GRUB_TERMINAL_OUTPUT="console"

to

GRUB_TERMINAL="serial console"
```

Ayrıca bu satırı ekleyin:

```
GRUB_SERIAL_COMMAND=”serial –speed=115200 –unit=0 –word=8 –parity=no –stop=1″
```

/etc/default/grub şimdi bu örneğe benzer olmalıdır:

```
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL="serial console"
GRUB_CMDLINE_LINUX="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"
GRUB_DISABLE_RECOVERY="true"
```
 
Grub yapılandırmalarını kullanarak tamamlayın ve güncelleştirin

`grub2-mkconfig -o /boot/grub2/grub.cfg`

SysRq çekirdek parametresini ayarlayın:

`sysctl -w kernel.sysrq = 1;echo kernel.sysrq = 1 >> /etc/sysctl.conf;sysctl -a | grep -i sysrq`

Alternatif olarak, Kabukta veya Run Komutu aracılığıyla tek bir satır kullanarak GRUB ve SysRq'u yapılandırabilirsiniz. Bu komutu çalıştırmadan önce dosyalarınızı yedekleme:


`cp /etc/default/grub /etc/default/grub.bak; sed -i 's/GRUB_TIMEOUT=1/GRUB_TIMEOUT=5/g' /etc/default/grub; sed -i 's/GRUB_TERMINAL_OUTPUT="console"/GRUB_TERMINAL="serial console"/g' /etc/default/grub; echo "GRUB_SERIAL_COMMAND=\"serial --speed=115200 --unit=0 --word=8 --parity=no --stop=1\"" >> /etc/default/grub;grub2-mkconfig -o /boot/grub2/grub.cfg;sysctl -w kernel.sysrq=1;echo kernel.sysrq = 1 /etc/sysctl.conf;sysctl -a | grep -i sysrq`


## <a name="red-hat-6x-grub-configuration"></a>Red Hat\.6 x GRUB yapılandırması
Değiştirilen dosya /boot/grub/grub.conf'tır. Değer, GRUB'un `timeout` ne kadar süreyle gösterildiğini belirler.

```
#boot=/dev/vda
default=0
timeout=15
splashimage=(hd0,0)/grub/splash.xpm.gz
#hiddenmenu
serial --unit=0 --speed=9600
terminal serial
terminal --timeout=5 serial console
```


Son hat *terminali –-timeout=5 seri konsolu,* devam etmek için herhangi bir **tuşa basın** 5 saniyelik bir istem ekleyerek **GRUB** zaman dışarısını daha da artıracaktır.

![rh6-1](./media/virtual-machines-serial-console/rh6-1.png)

GRUB menüsü, Yapılandırılan zaman aşması=15 için Esc tuşuna basmaya gerek kalmadan ekranda görünmelidir. Menüyü aktif hale getirmek için Tarayıcı'daki Konsol'a tıkladığınızdan emin olun ve gerekli çekirdeği seçin

![rh6-2](./media/virtual-machines-serial-console/rh6-2.png)

## <a name="suse"></a>Suse

## <a name="sles-12-sp1"></a>SLES 12 sp1
Ya resmi [dokümanlar](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-grub-single-user-mode#grub-access-in-suse-sles) uyarınca yast bootloader kullanın

Veya ekle/değiştirin /etc/default/grub aşağıdaki parametreleri:

```
GRUB_TERMINAL=serial
GRUB_TIMEOUT=5
GRUB_SERIAL_COMMAND="serial --unit=0 --speed=9600 --parity=no"

```
ttys0'nin GRUB_CMDLINE_LINUX veya GRUB_CMDLINE_LINUX_DEFAULT

```
GRUB_CMDLINE_LINUX_DEFAULT="console=ttyS0,9600n"
```

Grub.cfg yeniden oluşturma

`grub2-mkconfig -o /boot/grub2/grub.cfg`


## <a name="sles-11-sp4"></a>SLES 11 SP4 
Seri Konsol görünür ve önyükleme iletileri görüntüler, ancak bir **giriş görüntülemez:** istemi

VM'ye bir ssh oturumu açın ve bu satırı un-yorum yaparak dosya **/etc/inittab'ı** güncelleştirin:

```
#S0:12345:respawn:/sbin/agetty -L 9600 ttyS0 vt102
```

Sonraki komutu çalıştırın 

`telinit q`

GRUB'u etkinleştirmek için ,boot/grub/menu.lst adresinde aşağıdaki değişiklikler yapılmalıdır. 

```
timeout 5
serial --unit=0 --speed=9600 --parity=no
terminal --timeout=5 serial console

root (hd0,0)
kernel /boot/vmlinuz-3.0.101-108.74-default root=/dev/disk/by-uuid/ab6b62bb--
1a8c-45eb-96b1-1fbc535b9265 disk=/dev/sda  USE_BY_UUID_DEVICE_NAMES=1 earlyprinttk=ttyS0 console=ttyS0 rootdelay=300  showopts vga=0x314
```

 Bu yapılandırma, konsolda 5 saniye görünmeye **devam etmek için herhangi bir tuşa basın** iletiyi etkinleştirir 

Daha sonra ek bir 5 saniye için GRUB menüsünü görüntüler - aşağı ok tuşuna basarak sayacı kesecek ve ya kök şifre ayarlanması gereken tek bir kullanıcı modu için anahtar kelime **tek** eklemek istiyorum bir çekirdek seçin.

Komut **init=/bin/bash'ı** ekleyerek çekirdeği yükler, ancak init programının bir bash kabuğu ile değiştirilmesini sağlar.

Parola girmenize gerek kalmadan bir kabuk erişimi elde eve ceksiniz. Daha sonra Linux hesapları için parolagüncelleme veya diğer yapılandırma değişiklikleri yapmaya devam edebilirsiniz.


## <a name="force-the-kernel-to-a-bash-prompt"></a>Çekirdeği bir bash istemine zorla
GRUB erişimi olması, bu etkileşim birçok kurtarma yordamları için yararlıdır başlatma işlemini kesmenize olanak sağlar.
Kök parolanız yoksa ve tek kullanıcı kök parolanız olmasını gerektiriyorsa, init programını bir bash istemiyle değiştirerek çekirdeği önyüklemeyapabilirsiniz – bu kesme, çekirdek önyükleme satırına init=/bin/bash ekleyerek elde edilebilir

![bash1](./media/virtual-machines-serial-console/bash1.png)

Komutunu kullanarak / (root) dosya sistemi RW yeniden

`mount -o remount,rw /`

![bash2](./media/virtual-machines-serial-console/bash2.png)


Artık kök parola değişikliği veya diğer birçok Linux yapılandırma değişikliği gerçekleştirebilirsiniz

![bash3](./media/virtual-machines-serial-console/bash3.png)

VM'yi yeniden başlatın 

`/sbin/reboot -f`




## <a name="single-user-mode"></a>Tek Kullanıcı modu

Alternatif olarak, VM'ye tek kullanıcı veya acil durum modunda erişmeniz gerekebilir. Ok tuşlarını kullanarak önyükleme yapmak veya bölmek istediğiniz çekirdeği seçin.
Çekirdek önyükleme satırına **tek** veya **1** anahtar sözcük ekleyerek istediğiniz modu girin. RHEL sistemlerinde **rd.break'i**de ekleyebilirsiniz.

Tek kullanıcı moduna nasıl erişilir hakkında daha fazla bilgi için [bu dokümana](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-grub-single-user-mode#general-single-user-mode-access) bakın 


![single_user_ubuntu](./media/virtual-machines-serial-console/single-user-ubuntu.png)


## <a name="next-steps"></a>Sonraki adımlar
[Azure Seri Konsolu]( https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux) hakkında daha fazla bilgi edinin