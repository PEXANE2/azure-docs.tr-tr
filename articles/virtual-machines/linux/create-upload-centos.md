---
title: CentOS tabanlı Linux VHD oluşturma ve yükleme
description: CentOS tabanlı Bir Linux işletim sistemi içeren bir Azure sanal sabit disk (VHD) oluşturmayı ve yüklemeyi öğrenin.
author: gbowerman
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 11/25/2019
ms.author: guybo
ms.openlocfilehash: 8899249fd284f69fa26bab8cd70aaf6a67fbb83c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066777"
---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Azure'da CentOS tabanlı bir sanal makine hazırlama

CentOS tabanlı Bir Linux işletim sistemi içeren bir Azure sanal sabit disk (VHD) oluşturmayı ve yüklemeyi öğrenin.

* [Azure için centOS 6.x sanal makine hazırlayın](#centos-6x)
* [Azure için CentOS 7.0+ sanal makine hazırlayın](#centos-70)


## <a name="prerequisites"></a>Ön koşullar

Bu makalede, sanal bir sabit diske zaten bir CentOS (veya benzer türev) Linux işletim sistemi yüklediğinizi varsayar. .vhd dosyaları oluşturmak için birden çok araç vardır, örneğin Hyper-V gibi bir sanallaştırma çözümü. Talimatlar için [hyper-v rolünü yükleyin ve Sanal Makineyi Yapılandırın'](https://technet.microsoft.com/library/hh846766.aspx)a bakın.

**CentOS yükleme notları**

* Linux'u Azure'a hazırlama hakkında daha fazla ipucu için lütfen [Genel Linux Yükleme Notları'na](create-upload-generic.md#general-linux-installation-notes) bakın.
* VHDX biçimi Azure'da desteklenmez, yalnızca **Sabit VHD'de**dir.  Hyper-V Manager veya convert-vhd cmdlet kullanarak diski VHD formatına dönüştürebilirsiniz. VirtualBox kullanıyorsanız, bu, diski oluştururken dinamik olarak ayrılan varsayılan boyutun aksine **Sabit boyutu** seçmek anlamına gelir.
* Linux sistemini yüklerken LVM yerine standart bölümler kullanmanız *önerilir* (genellikle birçok kurulum için varsayılan). Bu, özellikle bir işletim sistemi diskinin sorun giderme için başka bir özdeş VM'ye eklenmesi gerekiyorsa, LVM adının klonlanmış VM'lerle çakışmasını önler. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri disklerinde kullanılabilir.
* UDF dosya sistemlerinin montajı için çekirdek desteği gereklidir. Azure'da ilk önyüklemede sağlama yapılandırması, misafire iliştirilen UDF biçimli medya aracılığıyla Linux VM'ye aktarılır. Azure Linux aracısı, yapılandırmasını okumak ve VM'yi sağlamak için UDF dosya sistemini monte edebilmelidir.
* 2.6.37 altındaki Linux çekirdek sürümleri, Hyper-V'de daha büyük VM boyutlarına sahip NUMA'yı desteklemez. Bu sorun öncelikle yukarı Red Hat 2.6.32 çekirdeğini kullanarak eski dağılımları etkiler ve RHEL 6.6'da (çekirdek-2.6.32-504) düzeltilmiştir. 2.6.37'den eski özel çekirdekler veya 2.6.32-504'ten büyük RHEL tabanlı çekirdekler çalıştıran `numa=off` sistemler, önyükleme parametresini grub.conf'taki çekirdek komut satırında ayarlamalıdır. Daha fazla bilgi için Red Hat [KB 436883](https://access.redhat.com/solutions/436883)bakın.
* OS diskinde bir takas bölümü yapılandırmayın. Linux aracısı geçici kaynak diskinde bir takas dosyası oluşturmak için yapılandırılabilir.  Bu konuda daha fazla bilgiyi aşağıdaki adımlarda bulabilirsiniz.
* Azure'daki tüm VHD'lerin 1MB'ye hizalanmış bir sanal boyutu olmalıdır. Ham diskten VHD'ye dönüştürürken, dönüştürmeden önce ham disk boyutunun 1MB'nin katı olduğundan emin olmalısınız. Daha fazla bilgi için [Linux Yükleme Notları'na](create-upload-generic.md#general-linux-installation-notes) bakın.

## <a name="centos-6x"></a>CentOS 6.x

1. Hyper-V Manager'da sanal makineyi seçin.

2. Sanal makine için bir konsol penceresi açmak için **Bağlan'ı** tıklatın.

3. CentOS 6'da NetworkManager, Azure Linux aracısını etkileyebilir. Aşağıdaki komutu çalıştırarak bu paketi kaldırın:

    ```bash
    sudo rpm -e --nodeps NetworkManager
    ```

4. Dosyayı `/etc/sysconfig/network` oluşturun veya edin ve aşağıdaki metni ekleyin:

    ```console
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

5. Dosyayı `/etc/sysconfig/network-scripts/ifcfg-eth0` oluşturun veya edin ve aşağıdaki metni ekleyin:

    ```console
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    ```

6. Ethernet arabirimi(ler) için statik kurallar oluşturmamak için udev kurallarını değiştirin. Bu kurallar, Microsoft Azure veya Hyper-V'de sanal bir makineklonlarken sorunlara neden olabilir:

    ```bash
    sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
    sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
    ```

7. Ağ hizmetinin önyükleme zamanında aşağıdaki komutu çalıştırarak başlayacağından emin olun:

    ```bash
    sudo chkconfig network on
    ```

8. Azure veri merkezleri içinde barındırılan OpenLogic aynalarını kullanmak istiyorsanız, `/etc/yum.repos.d/CentOS-Base.repo` dosyayı aşağıdaki depolarla değiştirin.  Bu, Azure Linux aracısı gibi ek paketleri içeren **[openlogic]** deposunu da ekler:

   ```console
   [openlogic]
   name=CentOS-$releasever - openlogic packages for $basearch
   baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
   enabled=1
   gpgcheck=0

   [base]
   name=CentOS-$releasever - Base
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

   #released updates
   [updates]
   name=CentOS-$releasever - Updates
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

   #additional packages that may be useful
   [extras]
   name=CentOS-$releasever - Extras
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

   #additional packages that extend functionality of existing packages
   [centosplus]
   name=CentOS-$releasever - Plus
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
   gpgcheck=1
   enabled=0
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

   #contrib - packages by Centos Users
   [contrib]
   name=CentOS-$releasever - Contrib
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
   gpgcheck=1
   enabled=0
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
   ```

    > [!Note]
    > Bu kılavuzun geri kalanı, aşağıdaki Azure `[openlogic]` Linux aracısını yüklemek için kullanılacak olan en azından repo'yu kullandığınızı varsayacaktır.

9. /etc/yum.conf için aşağıdaki satırı ekleyin:

    ```console
    http_caching=packages
    ```

10. Geçerli yum meta verileri temizlemek ve en son paketleri ile sistemi güncellemek için aşağıdaki komutu çalıştırın:

    ```bash
    yum clean all
    ```

    CentOS'un eski bir sürümü için bir resim oluşturmadığınız sürece, tüm paketleri en son güncelleştirmeniz önerilir:

    ```bash
    sudo yum -y update
    ```

    Bu komutu çalıştırdıktan sonra yeniden başlatma gerekebilir.

11. (İsteğe bağlı) Linux Entegrasyon Hizmetleri (LIS) için sürücüleri yükleyin.

    > [!IMPORTANT]
    > Bu adım CentOS 6.3 ve daha önceki sürümler için **gereklidir** ve daha sonraki sürümler için isteğe bağlıdır.

    ```bash
    sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
    sudo yum install microsoft-hyper-v
    ```

    Alternatif olarak, RPM'yi VM'nize yüklemek için [LIS indirme sayfasındaki](https://www.microsoft.com/download/details.aspx?id=51612) manuel yükleme yönergelerini takip edebilirsiniz.

12. Azure Linux Aracısını ve bağımlılıklarını yükleyin. Waagent hizmetini başlatın ve etkinleştirin:

    ```bash
    sudo yum install python-pyasn1 WALinuxAgent
    sudo service waagent start
    sudo chkconfig waagent on
    ```


    WALinuxAgent paketi, 3.

13. Grub yapılandırmanızdaki çekirdek önyükleme satırını Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bunu yapmak için `/boot/grub/menu.lst` bir metin düzenleyicisinde açın ve varsayılan çekirdeğin aşağıdaki parametreleri içerdiğinden emin olun:

    ```console
    console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    ```

    Bu, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini de sağlar ve bu da hata ayıklama sorunlarında Azure desteğine yardımcı olabilir.

    Yukarıdakilere ek olarak, aşağıdaki parametrelerin *kaldırılması* önerilir:

    ```console
    rhgb quiet crashkernel=auto
    ```

    Grafiksel ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderilmesini istediğimiz bir bulut ortamında kullanışlı değildir.  İstenirse `crashkernel` seçenek yapılandırılmış olarak bırakılabilir, ancak bu parametrenin VM'deki kullanılabilir bellek miktarını 128MB veya daha fazla azaltacağını ve bunun da küçük VM boyutlarında sorunlu olabileceğini unutmayın.

    > [!Important]
    > CentOS 6.5 ve daha önceki çekirdek parametresini `numa=off`de ayarlamalıdır. Bkz. Kırmızı Şapka [KB 436883](https://access.redhat.com/solutions/436883).

14. SSH sunucusunun yükleme zamanında başlayacak şekilde yüklendiğinden ve yapılandırıldığından emin olun.  Bu genellikle varsayılan dır.

15. İşletim sistemi diskinde takas alanı oluşturmayın.

    Azure Linux Aracısı, Azure'da sağlama yaptıktan sonra VM'ye bağlı yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diskinin *geçici* bir disk olduğunu ve VM deprovisioned olduğunda boşalınabileceğini unutmayın. Azure Linux Aracısını yükledikten sonra (önceki adıma bakın), aşağıdaki parametreleri uygun şekilde değiştirin: `/etc/waagent.conf`

    ```console
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048 ## NOTE: set this to whatever you need it to be.
    ```

16. Sanal makineyi sağlamanın kaldırılması ve Azure'da sağlama için hazırlanması için aşağıdaki komutları çalıştırın:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

17. Hyper-V Manager'da **Eylem -> Kapat'ı** tıklatın. Linux VHD'niz artık Azure'a yüklenmeye hazır.



## <a name="centos-70"></a>CentOS 7.0+

**CentOS 7'deki değişiklikler (ve benzer türevler)**

Azure için CentOS 7 sanal makine hazırlamak CentOS 6'ya çok benzer, ancak kayda değer birkaç önemli fark vardır:

* NetworkManager paketi artık Azure Linux aracısıyla çakışmıyor. Bu paket varsayılan olarak yüklenir ve kaldırılmadığını öneririz.
* GRUB2 artık varsayılan bootloader olarak kullanılır, bu nedenle çekirdek parametrelerini düzenleme yordamı değişti (aşağıya bakın).
* XFS artık varsayılan dosya sistemidir. İstenirse ext4 dosya sistemi kullanılabilir.

**Yapılandırma Adımları**

1. Hyper-V Manager'da sanal makineyi seçin.

2. Sanal makine için bir konsol penceresi açmak için **Bağlan'ı** tıklatın.

3. Dosyayı `/etc/sysconfig/network` oluşturun veya edin ve aşağıdaki metni ekleyin:

    ```console
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

4. Dosyayı `/etc/sysconfig/network-scripts/ifcfg-eth0` oluşturun veya edin ve aşağıdaki metni ekleyin:

    ```console
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    NM_CONTROLLED=no
    ```

5. Ethernet arabirimi(ler) için statik kurallar oluşturmamak için udev kurallarını değiştirin. Bu kurallar, Microsoft Azure veya Hyper-V'de sanal bir makineklonlarken sorunlara neden olabilir:

    ```bash
    sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
    ```

6. Azure veri merkezleri içinde barındırılan OpenLogic aynalarını kullanmak istiyorsanız, `/etc/yum.repos.d/CentOS-Base.repo` dosyayı aşağıdaki depolarla değiştirin.  Bu, Azure Linux aracısı için paketleri içeren **[openlogic]** deposunu da ekler:

   ```console
   [openlogic]
   name=CentOS-$releasever - openlogic packages for $basearch
   baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
   enabled=1
   gpgcheck=0
    
   [base]
   name=CentOS-$releasever - Base
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    
   #released updates
   [updates]
   name=CentOS-$releasever - Updates
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    
   #additional packages that may be useful
   [extras]
   name=CentOS-$releasever - Extras
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    
   #additional packages that extend functionality of existing packages
   [centosplus]
   name=CentOS-$releasever - Plus
   #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
   baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
   gpgcheck=1
   enabled=0
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
   ```
    
   > [!Note]
   > Bu kılavuzun geri kalanı, aşağıdaki Azure `[openlogic]` Linux aracısını yüklemek için kullanılacak olan en azından repo'yu kullandığınızı varsayacaktır.

7. Geçerli yum meta verileri temizlemek ve güncellemeleri yüklemek için aşağıdaki komutu çalıştırın:

    ```bash
    sudo yum clean all
    ```

    CentOS'un eski bir sürümü için bir resim oluşturmadığınız sürece, tüm paketleri en son güncelleştirmeniz önerilir:

    ```bash
    sudo yum -y update
    ```

    Bu komutu çalıştırdıktan sonra yeniden başlatma gerekebilir.

8. Grub yapılandırmanızdaki çekirdek önyükleme satırını Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bunu yapmak için, bir metin `/etc/default/grub` düzenleyicisi açık ve `GRUB_CMDLINE_LINUX` parametre, örneğin:

    ```console
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Bu, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini de sağlar ve bu da hata ayıklama sorunlarında Azure desteğine yardımcı olabilir. Ayrıca NIC'ler için yeni CentOS 7 adlandırma kurallarını kapatır. Yukarıdakilere ek olarak, aşağıdaki parametrelerin *kaldırılması* önerilir:

    ```console
    rhgb quiet crashkernel=auto
    ```

    Grafiksel ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderilmesini istediğimiz bir bulut ortamında kullanışlı değildir. İstenirse `crashkernel` seçenek yapılandırılmış olarak bırakılabilir, ancak bu parametrenin VM'deki kullanılabilir bellek miktarını 128MB veya daha fazla azaltacağını ve bunun da küçük VM boyutlarında sorunlu olabileceğini unutmayın.

9. Yukarıdaki başına düzenleme `/etc/default/grub` yapıldıktan sonra, grub yapılandırmasını yeniden oluşturmak için aşağıdaki komutu çalıştırın:

    ```bash
    sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

10. **Görüntüyü VMware, VirtualBox veya KVM'den oluşturuyorsanız:** Hyper-V sürücülerinin initramfs'a dahil olduğundan emin olun:

    Edit `/etc/dracut.conf`, içerik eklemek:

    ```console
    add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "
    ```

    Initramfs yeniden:

    ```bash
    sudo dracut -f -v
    ```

11. Azure Linux Aracısını ve bağımlılıkları yükleyin:

    ```bash
    sudo yum install python-pyasn1 WALinuxAgent
    sudo systemctl enable waagent
    ```

12. İşletim sistemi diskinde takas alanı oluşturmayın.

    Azure Linux Aracısı, Azure'da sağlama yaptıktan sonra VM'ye bağlı yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diskinin *geçici* bir disk olduğunu ve VM deprovisioned olduğunda boşalınabileceğini unutmayın. Azure Linux Aracısını yükledikten sonra (önceki adıma bakın), aşağıdaki parametreleri uygun şekilde değiştirin: `/etc/waagent.conf`

    ```console
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
    ```

13. Sanal makineyi sağlamanın kaldırılması ve Azure'da sağlama için hazırlanması için aşağıdaki komutları çalıştırın:

    ```bash
    sudo waagent -force -deprovision
    export HISTSIZE=0
    logout
    ```

14. Hyper-V Manager'da **Eylem -> Kapat'ı** tıklatın. Linux VHD'niz artık Azure'a yüklenmeye hazır.

## <a name="next-steps"></a>Sonraki adımlar

Azure'da yeni sanal makineler oluşturmak için CentOS Linux sanal sabit diskinizi kullanmaya hazırsınız. .vhd dosyasını Azure'a ilk kez bu kez yüklediğinizde, [bkz.](upload-vhd.md#option-1-upload-a-vhd)
