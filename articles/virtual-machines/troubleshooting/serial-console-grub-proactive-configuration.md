---
title: Azure seri konsol progrub yapılandırması | Microsoft Docs
description: Azure sanal makinelerinde tek kullanıcı ve kurtarma modu erişimine izin veren çeşitli dağıtımlar genelinde GRUB 'yi yapılandırın.
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
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74186935"
---
# <a name="proactively-ensuring-you-have-access-to-grub-and-sysrq-could-save-you-lots-of-down-time"></a>GRUB ve SySRq erişimine sahip olmanızı sağlamak, size büyük bir süre aşağı kaydetme

Seri konsoluna ve GRUB erişimine sahip olmak, çoğu durumda IaaS Linux sanal makinenizin kurtarma sürelerini iyileştirir. GRUB, aksi takdirde VM 'nizi kurtarmak için daha uzun sürebilecek kurtarma seçenekleri sunar. 


VM kurtarması gerçekleştirme nedenleri çok daha vardır ve şu gibi senaryolar için kullanılabilir:

   - Bozuk dosya sistemleri/çekirdek/MBR (ana önyükleme kaydı)
   - Başarısız çekirdek yükseltmeleri
   - Yanlış GRUB çekirdek parametreleri
   - Yanlış fstab yapılandırması
   - Güvenlik duvarı yapılandırması
   - Parola kayboldu
   - Karıştırılmış SSHD yapılandırma dosyaları
   - Ağ yapılandırması

 [Burada](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux#common-scenarios-for-accessing-the-serial-console) ayrıntılı olarak daha fazla sayıda senaryo

GRUB 'ye ve Azure 'da dağıtılan sanal makinelerinizdeki Seri konsol erişebildiğinizi doğrulayın. 

Seri konsol ' i yeni kullanıyorsanız [Bu bağlantıya](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux/)başvurun.

> [!TIP]
> Değişiklik yapmadan önce dosyaların yedeklerini aldığınızdan emin olun

GRUB 'ye eriştiğinizde Linux VM 'nizi nasıl hızlı bir şekilde kurtarabileceğinizi görmek için aşağıdaki videoyu izleyin

[Linux VM videosunu kurtarma](https://youtu.be/KevOc3d_SG4)

Linux VM 'lerinin kurtarılmasına yardımcı olacak çeşitli yöntemler vardır. Bu işlem, bir bulut ortamında zorlayıcı bir işlemdir.
Hizmetler hızlı bir şekilde kurtarılırken emin olmak için, araç ve özelliklere sürekli olarak ilerleme yapılıyor.

Azure seri konsolu ile Linux VM 'niz ile bir sistem konsolundan olduğu gibi etkileşim kurabilirsiniz.

Çekirdeğin nasıl önyükleneceðini de içeren birçok yapılandırma dosyasını yönetebilirsiniz. 

Daha fazla deneyimli Linux/UNIX sys yöneticileri, Azure seri konsolu aracılığıyla erişilebilen **tek bir kullanıcıyı** ve **acil durum modlarını** , çok sayıda kurtarma senaryosunda gereksiz bir şekılde disk takas ve VM silme işlemi yapmaya çalışır.

Kurtarma yöntemi, karşılaşmakta olan soruna bağlıdır. Örneğin, kayıp veya yanlış yerleştirilmiş bir parola Azure portal seçenekler aracılığıyla sıfırlanabilir > **sıfırlama parolası**. **Parola sıfırlama** özelliği, bir uzantı olarak bilinir ve Linux konuk aracısıyla iletişim kurar.

Özel Betik gibi diğer uzantılar kullanılabilir ancak bu seçenekler, Linux **waagent** 'ın, her zaman durum olmayan sağlıklı bir durumda olmasını gerektirir.

![Aracı durumu](./media/virtual-machines-serial-console/agent-status.png)


Azure seri konsoluna erişiminizin olmasını sağlamak ve GRUB, parola değişikliğinin veya hatalı yapılandırmanın saat yerine birkaç dakika içinde geri alabileceği anlamına gelir. VM 'nin alternatif bir çekirdekten önyükleme yapılmasını bile, birincil çekirdeğin bozulmuş olduğu senaryoda diskte birden çok kerıls olması gerekir.

![Çoklu çekirdek](./media/virtual-machines-serial-console/more-kernel.png)

## <a name="suggested-order-of-recovery-methods"></a>Kurtarma yöntemlerinin önerilen sırası:

- Azure Seri Konsol

- Disk takas: Şu iki kullanılarak otomatik olabilir:

   - [Power Shell kurtarma betikleri](https://github.com/Azure/azure-support-scripts/tree/master/VMRecovery/ResourceManager)
   - [Bash kurtarma betikleri](https://github.com/sribs/azure-support-scripts)

- Eski Yöntem

## <a name="disk-swap-video"></a>Disk takas videosu:

GRUB 'ye erişiminiz yoksa, [Bu](https://youtu.be/m5t0GZ5oGAc) videoyu IZLEYIN ve VM 'nizi kurtarmak için disk takas yordamını nasıl kolayca otomatikleştirebileceğinizi öğrenin

## <a name="challenges"></a>Sorunlarını

Tüm Linux Azure VM 'Leri, GRUB erişimi için varsayılan olarak yapılandırılmaz ve hepsi SySRq komutlarıyla kesintiye uğratılmaz. SLES 11 gibi bazı eski kaldırmalar, oturum açma isteğini Azure seri konsolunda görüntüleyecek şekilde yapılandırılmamış

Bu makalede, farklı Linux dağıtımlarını ve belge yapılandırmalarının, GRUB 'yi nasıl kullanılabilir hale geçitireceğiz.




## <a name="how-to-configure-linux-vm-to-accept-sysrq-keys"></a>Linux VM 'yi SysRq anahtarlarını kabul etmek için yapılandırma
SySRq anahtarı, varsayılan olarak bazı yeni Linux yedeklerindeki etkin hale gelir, ancak diğer bir deyişle, yalnızca belirli SysRq işlevlerine değer kabul etmek için yapılandırılmış olabilir.
Daha eski bir işlem, tamamen devre dışı bırakılabilir.

SysRq özelliği, kilitlenmiş veya askıda bir VM 'yi doğrudan Azure seri konsolundan yeniden başlatmak için yararlıdır. Ayrıca, GRUB menüsüne erişim elde etmek de yardımcı olur, alternatif olarak başka bir portal penceresinden veya SSH oturumundan bir VM 'nin yeniden başlatılması geçerli konsol bağlantınızı bırakabilir Bu nedenle, GRUB menüsünü göstermek için kullanılan zaman aşımı süresi dolar.
VM, çekirdek parametresi için 1 değerini kabul edecek şekilde yapılandırılmalıdır. Bu, yeniden başlatma/kapatma izni veren tüm SySRq veya 128 işlevlerini sağlar


[SySRq videosunu etkinleştir](https://youtu.be/0doqFRrHz_Mc)


VM 'yi Azure portal, SysRq komutları aracılığıyla bir yeniden başlatmayı kabul edecek şekilde yapılandırmak için çekirdek parametresi çekirdek parametresi için 1 değerini ayarlamanız gerekecektir. SySRq

Bu yapılandırmanın yeniden başlatılmasını kalıcı hale getirmek için **sysctl. conf** dosyasına bir giriş ekleyin

`echo kernel.sysrq = 1 >> /etc/sysctl.conf`

Çekirdek parametresini dinamik olarak yapılandırmak için

`sysctl -w kernel.sysrq=1`

**Kök** erişiminiz yoksa veya sudo bozulur, bir kabuk isteminden SySRq yapılandırması mümkün olmayacaktır.

Bu senaryoda Azure portal kullanarak SySRq 'ı etkinleştirebilirsiniz. Bu yöntem, **sudoers. d/waagent** dosyası kopuk hale gelirse veya silinmişse yararlı olabilir.

Azure portal Işlemler-> Çalıştır komutu-> RunShellScript özelliğini kullanarak, waagent işleminin sağlıklı olması gerekir ve ardından SySRq 'yı etkinleştirmek için bu komutu ekleyebilirsiniz

`sysctl -w kernel.sysrq=1 ; echo kernel.sysrq = 1 >> /etc/sysctl.conf`

Burada gösterildiği gibi: sysrq2 Enable ![](./media/virtual-machines-serial-console/enabling-sysrq-2.png)

Tamamlandıktan sonra, **SySRq** erişimini deneyebilirsiniz ve yeniden başlatmanın mümkün olduğunu görmeniz gerekir.

![sysrq3 etkinleştir](./media/virtual-machines-serial-console/enabling-sysrq-3.png)

**Yeniden başlatma** ve **SySRq gönder** komutunu seçin

![sysrq4 etkinleştir](./media/virtual-machines-serial-console/enabling-sysrq-4.png)

Sistem bunun gibi bir sıfırlama iletisini günlüğe kaydeder

![sysrq5 etkinleştir](./media/virtual-machines-serial-console/enabling-sysrq-5.png)


## <a name="ubuntu-grub-configuration"></a>Ubuntu GRUB yapılandırması

Varsayılan olarak, bu seçeneklerden birini kullanarak, sanal makine önyüklemesi sırasında **ESC** tuşunu basılı tutarak, GRUB ' yi, Azure seri konsolundaki ekran üzerinde Önyükle ve devam edebilir.

**Seçenek 1** -cihazı ekranda görüntülenecek şekilde zorlar 

Belirtilen zaman aşımı süresi boyunca GRUB menüsünü ekranda tutmak için/etc/default/grub.d/50-cloudimg-Settings.cfg dosyasını güncelleştirin.
GRUB hemen görüntülenecağından **ESC** 'ye vurmaya gerek yoktur

```
GRUB_TIMEOUT=0

change to

GRUB_TIMEOUT=5
```

**Seçenek 2** -önyüklemeden önce **ESC** 'ye basıldığında izin verir

/Etc/default/grub dosyasında değişiklik yapılarak benzer davranışa ve **ESC** tuşuna basarak 3 saniyelik bir zaman aşımını gözlemleyebilirsiniz


Bu iki satırı Açıklama:

```
#GRUB_HIDDEN_TIMEOUT=0
#GRUB_HIDDEN_TIMEOUT_QUIET=true
```
ve şu satırı ekleyin:

```
GRUB_TIMEOUT_STYLE=countdown
```


## <a name="ubuntu-1204"></a>Ubuntu 12\.04

Ubuntu 12,04, seri konsoluna erişime izin verir, ancak etkileşim özelliği sunmaz. **Oturum açma:** istem görülmedi


12,04 için **oturum açma:** komut istemi:
1. Aşağıdaki metni içeren/etc/Init/ttyS0.conf adlı bir dosya oluşturun:

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

2. Getty 'ı başlatmaya sorma     
    ```
    sudo start ttyS0
    ```
 
Ubuntu sürümleri için seri konsolunu yapılandırmak için gereken ayarlar [burada](https://help.ubuntu.com/community/SerialConsoleHowto) bulunabilir

## <a name="ubuntu-recovery-mode"></a>Ubuntu kurtarma modu

Ek kurtarma ve temizleme seçenekleri, GRUB aracılığıyla Ubuntu için kullanılabilir, ancak bu ayarlar yalnızca çekirdek parametrelerini uygun şekilde yapılandırırsanız erişilebilir.
Bu çekirdek önyükleme parametresini yapılandırma hatası, Kurtarma menüsünü Azure seri konsoluna değil Azure Tanılama gönderilmesine zorlayabilir.
Aşağıdaki adımları izleyerek Ubuntu kurtarma menüsüne erişim elde edebilirsiniz:

ÖNYÜKLEME Işlemini kesme ve erişim GRUB menüsü

Ubuntu için Gelişmiş Seçenekler ' i seçin ve ENTER tuşuna basın

![ubunturec1](./media/virtual-machines-serial-console/ubunturec1.png)

Görüntülenen çizgiyi seçin *(kurtarma modu)* ENTER tuşuna basmayın, ancak "e" düğmesine basın

![ubunturec2](./media/virtual-machines-serial-console/ubunturec2.png)

Çekirdek yükleyecek satırı bulun ve **son parametre olarak** Destination parametresini **Console = ttyS0** olarak değiştirin

```
linux /boot/vmlinuz-4.15.0-1023-azure root=UUID=21b294f1-25bd-4265-9c4e-d6e4aeb57e97 ro recovery nomodeset

change to

linux /boot/vmlinuz-4.15.0-1023-azure root=UUID=21b294f1-25bd-4265-9c4e-d6e4aeb57e97 ro recovery console=ttyS0
```

![ubunturec3](./media/virtual-machines-serial-console/ubunturec3.png)

Çekirdeği başlatmak ve yüklemek için **Ctrl + x** tuşlarına basın.
Her şey iyi gitse, diğer kurtarma seçeneklerini gerçekleştirmenize yardımcı olabilecek bu ek seçenekleri de görürsünüz

![ubunturec4](./media/virtual-machines-serial-console/ubunturec4.png)


## <a name="red-hat-grub-configuration"></a>Red Hat GRUB yapılandırması

## <a name="red-hat-74-grub-configuration"></a>Red hat 7\.4\+ GRUB yapılandırması
Bu sürümlerdeki varsayılan/etc/default/grub yapılandırması yeterli şekilde yapılandırıldı

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

SysRq anahtarını etkinleştir

```
sysctl -w kernel.sysrq=1;echo kernel.sysrq = 1 >> /etc/sysctl.conf;sysctl -a | grep -i sysrq
```

## <a name="red-hat-72-and-73-grub-configuration"></a>Red hat 7\.2 ve 7\.3 GRUB yapılandırması
Değiştirilecek dosya/etc/default/GRUB ' dir; varsayılan bir yapılandırma şöyle görünür:

```
GRUB_TIMEOUT=1
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"
GRUB_DISABLE_RECOVERY="true"
```

/Etc/default/grub içinde aşağıdaki satırları değiştirin

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

Ayrıca şu satırı ekleyin:

```
GRUB_SERIAL_COMMAND=”serial –speed=115200 –unit=0 –word=8 –parity=no –stop=1″
```

/etc/default/grub Şu örneğe benzer şekilde görünmelidir:

```
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL="serial console"
GRUB_CMDLINE_LINUX="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"
GRUB_DISABLE_RECOVERY="true"
```
 
Kullanarak grub yapılandırmasını doldurun ve güncelleştirin

`grub2-mkconfig -o /boot/grub2/grub.cfg`

SysRq çekirdek parametresini ayarlayın:

`sysctl -w kernel.sysrq = 1;echo kernel.sysrq = 1 >> /etc/sysctl.conf;sysctl -a | grep -i sysrq`

Alternatif olarak, kabuğa veya Çalıştır komutuyla tek bir satır kullanarak GRUB ve SysRq 'ı yapılandırabilirsiniz. Bu komutu çalıştırmadan önce dosyalarınızı yedekleyin:


`cp /etc/default/grub /etc/default/grub.bak; sed -i 's/GRUB_TIMEOUT=1/GRUB_TIMEOUT=5/g' /etc/default/grub; sed -i 's/GRUB_TERMINAL_OUTPUT="console"/GRUB_TERMINAL="serial console"/g' /etc/default/grub; echo "GRUB_SERIAL_COMMAND=\"serial --speed=115200 --unit=0 --word=8 --parity=no --stop=1\"" >> /etc/default/grub;grub2-mkconfig -o /boot/grub2/grub.cfg;sysctl -w kernel.sysrq=1;echo kernel.sysrq = 1 /etc/sysctl.conf;sysctl -a | grep -i sysrq`


## <a name="red-hat-6x-grub-configuration"></a>Red hat 6\.x GRUB yapılandırması
Değiştirilecek dosya/boot/grub/grub.conf. `timeout` değeri, ' nin ne kadar süreceğine ilişkin olacağını belirlemektir.

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


Son satır *terminali –-Timeout = 5 seri konsol* , **devam etmek için herhangi bir tuşa basarak** 5 saniyelik bir istem ekleyerek **grub** zaman aşımını artırır.

![RH6-1](./media/virtual-machines-serial-console/rh6-1.png)

GRUB menüsü, yapılandırılmış zaman aşımı = 15 ' in ESC tuşuna basması gerekmeden ekran üzerinde görünmelidir. Etkin menü oluşturmak için tarayıcıda konsolunda tıkladığınızdan emin olun ve gerekli çekirdeği seçin

![RH6-2](./media/virtual-machines-serial-console/rh6-2.png)

## <a name="suse"></a>SuSE

## <a name="sles-12-sp1"></a>SLES 12 SP1
Her resmi [Belgeler](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-grub-single-user-mode#grub-access-in-suse-sles) için yast önyükleme yükleyicisine kullanın

Ya da/etc/default/grub öğesine Ekle/Değiştir aşağıdaki parametreleri:

```
GRUB_TERMINAL=serial
GRUB_TIMEOUT=5
GRUB_SERIAL_COMMAND="serial --unit=0 --speed=9600 --parity=no"

```
TtyS0 GRUB_CMDLINE_LINUX veya GRUB_CMDLINE_LINUX_DEFAULT kullanıldığını doğrulayın

```
GRUB_CMDLINE_LINUX_DEFAULT="console=ttyS0,9600n"
```

Grub. cfg 'yi yeniden oluşturma

`grub2-mkconfig -o /boot/grub2/grub.cfg`


## <a name="sles-11-sp4"></a>SLES 11 SP4 
Seri konsol görünür ve önyükleme iletilerini görüntüler, ancak **oturum açma** iletisi görüntülemez: Prompt

VM 'de bir SSH oturumu açın ve bu satırı açıklama ekleyerek **/etc/ınittab** dosyasını güncelleştirin:

```
#S0:12345:respawn:/sbin/agetty -L 9600 ttyS0 vt102
```

Sonra komutu çalıştırın 

`telinit q`

GRUB 'yi etkinleştirmek için,/boot/grub/menu.lst 'de aşağıdaki değişiklikler yapılmalıdır 

```
timeout 5
serial --unit=0 --speed=9600 --parity=no
terminal --timeout=5 serial console

root (hd0,0)
kernel /boot/vmlinuz-3.0.101-108.74-default root=/dev/disk/by-uuid/ab6b62bb--
1a8c-45eb-96b1-1fbc535b9265 disk=/dev/sda  USE_BY_UUID_DEVICE_NAMES=1 earlyprinttk=ttyS0 console=ttyS0 rootdelay=300  showopts vga=0x314
```

 Bu yapılandırma, konsolda 5 saniye boyunca **görünmeye devam etmek için herhangi bir tuşa basarak** iletiyi etkinleştirir 

Daha sonra bu, ek 5 saniye boyunca bir x işareti görüntüler ve sonra da sayacı kesintiye uğratacak ve önyüklemek istediğiniz bir çekirdek seçtiğinizde, kök parolanın ayarlanmasını gerektiren tek bir Kullanıcı modu için **tek** bir anahtar sözcüğü ekleyin.

**İnit =/bin/Bash** komutunu eklemek çekirdeği yükler, ancak init programının bir bash kabuğu ile değiştirilmesini sağlar.

Bir parola girmek zorunda kalmadan bir Shell 'e erişim elde edersiniz. Daha sonra, Linux hesaplarının parolasını güncelleştirebilir veya başka yapılandırma değişiklikleri yapabilirsiniz.


## <a name="force-the-kernel-to-a-bash-prompt"></a>Çekirdeği bir bash istemine zorla
GRUB 'ye erişimi olması, başlatma işlemini kesintiye uğratmak için bu etkileşim birçok kurtarma yordamı için yararlıdır.
Kök parolanız yoksa ve tek bir kullanıcı için bir kök parolanız olması gerekiyorsa, bir bash istemiyle init programını değiştirme çekirdeğini önyükleyebilirsiniz. bu kesme, çekirdek önyükleme satırına init =/bin/Bash eklenerek elde edilebilir

![bash1](./media/virtual-machines-serial-console/bash1.png)

Komutunu kullanarak/(kök) dosya sistemi RW 'nizi uzaktan bağlayın

`mount -o remount,rw /`

![bash2](./media/virtual-machines-serial-console/bash2.png)


Artık kök parola değişikliği veya diğer birçok Linux yapılandırma değişikliğini gerçekleştirebilirsiniz

![bash3](./media/virtual-machines-serial-console/bash3.png)

VM 'yi yeniden başlatma 

`/sbin/reboot -f`




## <a name="single-user-mode"></a>Tek Kullanıcı modu

Alternatif olarak, VM 'ye tek bir kullanıcı veya acil durum modunda erişmeniz gerekebilir. Önyüklemek veya ok tuşlarını kullanarak kesintiye uğratmak istediğiniz çekirdeği seçin.
Çekirdek önyükleme satırına **tek** veya **1** anahtar sözcüğünü ekleyerek istediğiniz modu girin. RHEL sistemlerinde **RD. Break**' i de ekleyebilirsiniz.

Tek kullanıcı moduna erişme hakkında daha fazla bilgi için [Bu belgeye](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-grub-single-user-mode#general-single-user-mode-access) bakın 


![single_user_ubuntu](./media/virtual-machines-serial-console/single-user-ubuntu.png)


## <a name="next-steps"></a>Sonraki adımlar
[Azure seri konsolu]( https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux) hakkında daha fazla bilgi edinin