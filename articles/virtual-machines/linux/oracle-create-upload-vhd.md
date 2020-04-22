---
title: Oracle Linux VHD oluşturma ve yükleme
description: Oracle Linux işletim sistemi içeren bir Azure sanal sabit diski (VHD) oluşturmayı ve yüklemeyi öğrenin.
author: gbowerman
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: article
ms.date: 12/10/2019
ms.author: guybo
ms.openlocfilehash: fd6d17709cc3e5e9f6bb89ed7480fcd9ee80fd97
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759394"
---
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Azure için bir Oracle Linux sanal makine hazırlayın

Bu makalede, zaten sanal bir sabit diske bir Oracle Linux işletim sistemi yüklediğinizvarsayar. .vhd dosyaları oluşturmak için birden çok araç vardır, örneğin Hyper-V gibi bir sanallaştırma çözümü. Talimatlar için [hyper-v rolünü yükleyin ve Sanal Makineyi Yapılandırın'](https://technet.microsoft.com/library/hh846766.aspx)a bakın.

## <a name="oracle-linux-installation-notes"></a>Oracle Linux yükleme notları
* Linux'u Azure'a hazırlama hakkında daha fazla ipucu için lütfen [Genel Linux Yükleme Notları'na](create-upload-generic.md#general-linux-installation-notes) bakın.
* Hyper-V ve Azure, Oracle Linux'u Kırılmaz Kurumsal Çekirdek (UEK) veya Red Hat Uyumlu Çekirdek ile destekler.
* Oracle'ın UEK2'si Hyper-V ve Azure'da desteklenmez, çünkü gerekli sürücüleri içermez.
* VHDX biçimi Azure'da desteklenmez, yalnızca **Sabit VHD'de**dir.  Hyper-V Manager veya convert-vhd cmdlet kullanarak diski VHD formatına dönüştürebilirsiniz.
* Linux sistemini yüklerken LVM yerine standart bölümler kullanmanız önerilir (genellikle birçok kurulum için varsayılan). Bu, özellikle bir işletim sistemi diskinin sorun giderme için başka bir VM'ye eklenmesi gerekiyorsa, klonlanmış VM'lerle LVM adı çakışmalarını önler. Tercih edilirse veri disklerinde [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kullanılabilir.
* 2.6.37'den önceki Linux çekirdek sürümleri, Hyper-V'de daha büyük VM boyutlarına sahip NUMA'yı desteklemez. Bu sorun öncelikle upstream Red Hat 2.6.32 çekirdeğini kullanarak eski dağılımları etkiler ve Oracle Linux 6.6 ve daha sonra
* OS diskinde bir takas bölümü yapılandırmayın. Linux aracısı geçici kaynak diskinde bir takas dosyası oluşturmak için yapılandırılabilir.  Bu konuda daha fazla bilgiyi aşağıdaki adımlarda bulabilirsiniz.
* Azure'daki tüm VHD'lerin 1MB'ye hizalanmış bir sanal boyutu olmalıdır. Ham diskten VHD'ye dönüştürürken, dönüştürmeden önce ham disk boyutunun 1MB'nin katı olduğundan emin olmalısınız. Daha fazla bilgi için [Linux Yükleme Notları'na](create-upload-generic.md#general-linux-installation-notes) bakın.
* Deponun `Addons` etkin olduğundan emin olun. `/etc/yum.repos.d/public-yum-ol6.repo`Dosyayı (Oracle Linux 6) `/etc/yum.repos.d/public-yum-ol7.repo`veya (Oracle Linux 7) düzenle ve `enabled=0` satırı bu dosyadaki **[ol6_addons]** veya `enabled=1` **[ol7_addons]** altında değiştirin.

## <a name="oracle-linux-64-and-later"></a>Oracle Linux 6.4 ve sonrası
Sanal makinenin Azure'da çalışması için işletim sisteminde belirli yapılandırma adımlarını tamamlamanız gerekir.

1. Hyper-V Manager'ın orta bölmesinde sanal makineyi seçin.
2. Sanal makinenin penceresini açmak için **Bağlan'ı** tıklatın.
3. Aşağıdaki komutu çalıştırarak NetworkManager'ı kaldırın:
   
        # sudo rpm -e --nodeps NetworkManager
   
    **Not:** Paket zaten yüklenmediyse, bu komut bir hata iletisiyle başarısız olur. Bu beklenen bir durumdur.
4. Dizinde `/etc/sysconfig/` aşağıdaki metni içeren **ağ** adlı bir dosya oluşturun:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. Dizinde `/etc/sysconfig/network-scripts/` aşağıdaki metni içeren **ifcfg-eth0** adlı bir dosya oluşturun:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
6. Ethernet arabirimi(ler) için statik kurallar oluşturmamak için udev kurallarını değiştirin. Bu kurallar, Microsoft Azure veya Hyper-V'de sanal bir makineklonlarken sorunlara neden olabilir:
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
7. Ağ hizmetinin önyükleme zamanında aşağıdaki komutu çalıştırarak başlayacağından emin olun:
   
        # chkconfig network on
8. Python-pyasn1'i aşağıdaki komutu çalıştırarak yükleyin:
   
        # sudo yum install python-pyasn1
9. Grub yapılandırmanızdaki çekirdek önyükleme satırını Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bunu bir metin düzenleyicisinde "/boot/grub/menu.lst" olarak açın ve çekirdeğin aşağıdaki parametreleri içerdiğinden emin olun:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
   Bu, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini sağlar ve bu da hata ayıklama sorunlarında Azure desteğine yardımcı olabilir.
   
   Yukarıdakilere ek olarak, aşağıdaki parametrelerin *kaldırılması* önerilir:
   
        rhgb quiet crashkernel=auto
   
   Grafiksel ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderilmesini istediğimiz bir bulut ortamında kullanışlı değildir.
   
   İstenirse `crashkernel` seçenek yapılandırılmış olarak bırakılabilir, ancak bu parametrenin VM'deki kullanılabilir bellek miktarını 128MB veya daha fazla azaltacağını ve bunun da küçük VM boyutlarında sorunlu olabileceğini unutmayın.
10. SSH sunucusunun yükleme zamanında başlayacak şekilde yüklendiğinden ve yapılandırıldığından emin olun.  Bu genellikle varsayılan dır.
11. Aşağıdaki komutu çalıştırarak Azure Linux Aracısını yükleyin. En son sürümü 2.0.15 olduğunu.
    
        # sudo yum install WALinuxAgent
    
    WALinuxAgent paketinin yüklenmesi, adım 2'de açıklandığı gibi zaten kaldırılmadıysa NetworkManager ve NetworkManager-gnome paketlerini kaldıracaktır.
12. İşletim sistemi diskinde takas alanı oluşturmayın.
    
    Azure Linux Aracısı, Azure'da sağlama yaptıktan sonra VM'ye bağlı yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diskinin *geçici* bir disk olduğunu ve VM deprovisioned olduğunda boşalınabileceğini unutmayın. Azure Linux Aracısını yükledikten sonra (önceki adıma bakın), /etc/waagent.conf'ta aşağıdaki parametreleri uygun şekilde değiştirin:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
13. Sanal makineyi sağlamanın kaldırılması ve Azure'da sağlama için hazırlanması için aşağıdaki komutları çalıştırın:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
14. Hyper-V Manager'da **Eylem -> Kapat'ı** tıklatın. Linux VHD'niz artık Azure'a yüklenmeye hazır.

---
## <a name="oracle-linux-70-and-later"></a>Oracle Linux 7.0 ve sonrası
**Oracle Linux 7'deki değişiklikler**

Azure için Oracle Linux 7 sanal makine hazırlamak Oracle Linux 6'ya çok benzer, ancak kayda değer birkaç önemli fark vardır:

* Azure, Oracle Linux'u Kırılmaz Kurumsal Çekirdek (UEK) veya Red Hat Uyumlu Çekirdek ile destekler. UEK ile Oracle Linux önerilir.
* NetworkManager paketi artık Azure Linux aracısıyla çakışmıyor. Bu paket varsayılan olarak yüklenir ve kaldırılmadığını öneririz.
* GRUB2 artık varsayılan bootloader olarak kullanılır, bu nedenle çekirdek parametrelerini düzenleme yordamı değişti (aşağıya bakın).
* XFS artık varsayılan dosya sistemidir. İstenirse ext4 dosya sistemi kullanılabilir.

**Yapılandırma adımları**

1. Hyper-V Manager'da sanal makineyi seçin.
2. Sanal makine için bir konsol penceresi açmak için **Bağlan'ı** tıklatın.
3. Dizinde `/etc/sysconfig/` aşağıdaki metni içeren **ağ** adlı bir dosya oluşturun:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. Dizinde `/etc/sysconfig/network-scripts/` aşağıdaki metni içeren **ifcfg-eth0** adlı bir dosya oluşturun:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. Ethernet arabirimi(ler) için statik kurallar oluşturmamak için udev kurallarını değiştirin. Bu kurallar, Microsoft Azure veya Hyper-V'de sanal bir makineklonlarken sorunlara neden olabilir:
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. Ağ hizmetinin önyükleme zamanında aşağıdaki komutu çalıştırarak başlayacağından emin olun:
   
        # sudo chkconfig network on
7. Python-pyasn1 paketini aşağıdaki komutu çalıştırarak yükleyin:
   
        # sudo yum install python-pyasn1
8. Geçerli yum meta verileri temizlemek ve güncellemeleri yüklemek için aşağıdaki komutu çalıştırın:
   
        # sudo yum clean all
        # sudo yum -y update
9. Grub yapılandırmanızdaki çekirdek önyükleme satırını Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bu açık "/etc/default/grub" bir metin düzenleyicisi `GRUB_CMDLINE_LINUX` yapmak ve parametre, örneğin de) edin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Bu, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini de sağlar ve bu da hata ayıklama sorunlarında Azure desteğine yardımcı olabilir. Ayrıca, Unbreakable Enterprise Kernel ile Oracle Linux 7'deki NIC'lerin adlandırma kurallarını da kapatır. Yukarıdakilere ek olarak, aşağıdaki parametrelerin *kaldırılması* önerilir:
   
       rhgb quiet crashkernel=auto
   
   Grafiksel ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderilmesini istediğimiz bir bulut ortamında kullanışlı değildir.
   
   İstenirse `crashkernel` seçenek yapılandırılmış olarak bırakılabilir, ancak bu parametrenin VM'deki kullanılabilir bellek miktarını 128MB veya daha fazla azaltacağını ve bunun da küçük VM boyutlarında sorunlu olabileceğini unutmayın.
10. Yukarıdaki başına "/etc/default/grub" düzenlemesini yaptıktan sonra, grub yapılandırmasını yeniden oluşturmak için aşağıdaki komutu çalıştırın:
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. SSH sunucusunun yükleme zamanında başlayacak şekilde yüklendiğinden ve yapılandırıldığından emin olun.  Bu genellikle varsayılan dır.
12. Aşağıdaki komutu çalıştırarak Azure Linux Aracısını yükleyin:
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. İşletim sistemi diskinde takas alanı oluşturmayın.
    
    Azure Linux Aracısı, Azure'da sağlama yaptıktan sonra VM'ye bağlı yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diskinin *geçici* bir disk olduğunu ve VM deprovisioned olduğunda boşalınabileceğini unutmayın. Azure Linux Aracısını yükledikten sonra (önceki adıma bakın), /etc/waagent.conf'ta aşağıdaki parametreleri uygun şekilde değiştirin:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
14. Sanal makineyi sağlamanın kaldırılması ve Azure'da sağlama için hazırlanması için aşağıdaki komutları çalıştırın:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. Hyper-V Manager'da **Eylem -> Kapat'ı** tıklatın. Linux VHD'niz artık Azure'a yüklenmeye hazır.

## <a name="next-steps"></a>Sonraki adımlar
Azure'da yeni sanal makineler oluşturmak için Oracle Linux .vhd'inizi kullanmaya hazırsınız. .vhd dosyasını Azure'a ilk kez bu kez yüklediğinizde, [bkz.](upload-vhd.md#option-1-upload-a-vhd)

