---
title: Azure'da kullanmak için Red Hat Enterprise Linux VHD oluşturma ve yükleme
description: Red Hat Linux işletim sistemi içeren bir Azure sanal sabit diski (VHD) oluşturmayı ve yüklemeyi öğrenin.
author: gbowerman
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/17/2019
ms.author: guybo
ms.openlocfilehash: 625dd1c8051d7949987d0dd19ee8a08347eedf14
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758646"
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Azure'da Red Hat tabanlı bir sanal makine hazırlama
Bu makalede, Azure'da kullanılmak üzere red hat enterprise Linux (RHEL) sanal makinesini nasıl hazırlayacağınızı öğreneceksiniz. Bu makalede ele alınan RHEL sürümleri 6.7+ ve 7.1+ şeklindedir. Bu makalede ele alınan hazırlık için hipervizörler Hyper-V, çekirdek tabanlı sanal makine (KVM) ve VMware'dir. Red Hat'in Bulut Erişimi programına katılmak için uygunluk gereksinimleri hakkında daha fazla bilgi için [Red Hat'in Bulut Erişimi web sitesine](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) ve [Azure'da RHEL'i çalıştırmaya](https://access.redhat.com/ecosystem/ccsp/microsoft-azure)bakın. Bina RHEL görüntülerini otomatikleştirmenin yolları için [Azure Resim Oluşturucusu'na](https://docs.microsoft.com/azure/virtual-machines/linux/image-builder-overview)bakın.

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Hyper-V Manager'dan Red Hat tabanlı sanal makine hazırlayın

### <a name="prerequisites"></a>Ön koşullar
Bu bölüm, Red Hat web sitesinden bir ISO dosyası aldığınızı ve RHEL görüntüsünü sanal bir sabit diske (VHD) yüklediğinizi varsayar. İşletim sistemi görüntüsünü yüklemek için Hyper-V Manager'ın nasıl kullanılacağı hakkında daha fazla bilgi için [Hyper-V Rolünü Yükleyin ve Sanal Makineyi Yapılandırın'](https://technet.microsoft.com/library/hh846766.aspx)a bakın.

**RHEL kurulum notları**

* Azure VHDX biçimini desteklemez. Azure yalnızca sabit VHD'yi destekler. Diski VHD biçimine dönüştürmek için Hyper-V Manager'ı veya convert-vhd cmdlet'i kullanabilirsiniz. VirtualBox kullanıyorsanız, **diski** oluştururken varsayılan dinamik olarak ayrılan seçeneğin aksine Sabit boyutu seçin.
* Azure, Gen1 (BIOS önyükleme) & Gen2 (UEFI önyükleme) Sanal makineleri destekler.
* VHD için izin verilen maksimum boyut 1.023 GB'dır.
* Mantıksal Birim Yöneticisi (LVM) desteklenir ve Azure sanal makinelerinde işletim sistemi diskinde veya veri disklerinde kullanılabilir. Ancak, genel olarak LVM yerine işletim sistemi diskinde standart bölümler kullanılması önerilir. Bu uygulama, özellikle sorun giderme için başka bir özdeş sanal makineye bir işletim sistemi diski takmanız gerekirse, klonlanmış sanal makinelerle LVM adı çakışmalarını önler. Ayrıca [bkz.](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Evrensel Disk Formatı (UDF) dosya sistemlerinin montajı için çekirdek desteği gereklidir. Azure'da ilk önyüklemede, konuk adabına iliştirilen UDF biçimli ortam, sağlama yapılandırmasını Linux sanal makinesine geçirir. Azure Linux Aracısı, yapılandırmasını okumak ve sanal makineyi sağlamak için UDF dosya sistemini monte edebilmelidir.
* İşletim sistemi diskinde bir takas bölümü yapılandırmayın. Linux Aracısı, geçici kaynak diskinde bir takas dosyası oluşturmak üzere yapılandırılabilir.  Bu konuda daha fazla bilgi aşağıdaki adımları bulunabilir.
* Azure'daki tüm VHD'lerin 1MB'ye hizalanmış bir sanal boyutu olmalıdır. Ham diskten VHD'ye dönüştürürken, dönüştürmeden önce ham disk boyutunun 1MB'nin katı olduğundan emin olmalısınız. Daha fazla bilgi aşağıdaki adımlarda bulunabilir. Ayrıca daha fazla bilgi için [Linux Yükleme Notları'na](create-upload-generic.md#general-linux-installation-notes) bakın.

### <a name="prepare-a-rhel-6-virtual-machine-from-hyper-v-manager"></a>Hyper-V Manager'dan BIR RHEL 6 sanal makine hazırlayın

1. Hyper-V Manager'da sanal makineyi seçin.

1. Sanal makine için bir konsol penceresi açmak için **Bağlan'ı** tıklatın.

1. RHEL 6'da NetworkManager, Azure Linux aracısını etkileyebilir. Aşağıdaki komutu çalıştırarak bu paketi kaldırın:
   
        # sudo rpm -e --nodeps NetworkManager

1. Dosyayı oluşturun `/etc/sysconfig/network` veya edin ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Dosyayı oluşturun `/etc/sysconfig/network-scripts/ifcfg-eth0` veya edin ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

1. Ethernet arabirimi için statik kurallar oluşturmamak için udev kurallarını taşıyın (veya kaldırın). Bu kurallar, Microsoft Azure veya Hyper-V'de sanal bir makineklonladığınızda sorunlara neden olur:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

1. Ağ hizmetinin önyükleme zamanında aşağıdaki komutu çalıştırarak başlayacağından emin olun:

        # sudo chkconfig network on

1. Aşağıdaki komutu çalıştırarak RHEL deposundan paketlerin yüklenmesini sağlamak için Red Hat aboneliğinizi kaydedin:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

1. WALinuxAgent paketi, `WALinuxAgent-<version>`Red Hat ekstralar deposuna itildi. Aşağıdaki komutu çalıştırarak ekstralar deposunu etkinleştirin:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

1. Grub yapılandırmanızdaki çekirdek önyükleme satırını Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bu değişikliği yapmak `/boot/grub/menu.lst` için metin düzenleyicisinde açın ve varsayılan çekirdeğin aşağıdaki parametreleri içerdiğinden emin olun:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Bu, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini de sağlar ve bu da hata ayıklama sorunlarında Azure desteğine yardımcı olabilir.
    
    Buna ek olarak, aşağıdaki parametreleri kaldırmanızı tavsiye ettik:
    
        rhgb quiet crashkernel=auto
    
    Grafiksel ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderilmesini istediğimiz bir bulut ortamında kullanışlı değildir.  İstenirseniz `crashkernel` seçeneği yapılandırabilirsiniz. Bu parametrenin sanal makinedeki kullanılabilir bellek miktarını 128 MB veya daha fazla azalttığını unutmayın. Bu yapılandırma, daha küçük sanal makine boyutlarında sorunlu olabilir.


1. Güvenli kabuk (SSH) sunucusunun yükleme zamanında başlayacak şekilde yüklendiğinden ve yapılandırıldığından emin olun, ki bu genellikle varsayılan değerdir. /etc/ssh/sshd_config aşağıdaki satırı içerecek şekilde değiştirin:

        ClientAliveInterval 180

1. Aşağıdaki komutu çalıştırarak Azure Linux Aracısını yükleyin:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

    WALinuxAgent paketinin yüklenmesi, 3.

1. İşletim sistemi diskinde takas alanı oluşturmayın.

    Azure Linux Aracısı, sanal makine Azure'da sağlandıktan sonra sanal makineye bağlı olan yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diskinin geçici bir disk olduğunu ve sanal makine nin kullanılabilirliği iptal edilirse boşaltılabilir olduğunu unutmayın. Azure Linux Aracısını önceki adımda yükledikten sonra /etc/waagent.conf'ta aşağıdaki parametreleri uygun şekilde değiştirin:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Aşağıdaki komutu çalıştırarak aboneliğin kaydını (gerekirse) unregister:

        # sudo subscription-manager unregister

1. Sanal makineyi sağlamanın kaldırılması ve Azure'da sağlama için hazırlanması için aşağıdaki komutları çalıştırın:

        # Note: if you are migrating a specific virtual machine and do not wish to create a generalized image,
        # skip the deprovision step
        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. Hyper-V Manager'da **Eylem** > **Kapatma'yı** tıklatın. Linux VHD'niz artık Azure'a yüklenmeye hazır.


### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a>Hyper-V Manager'dan BIR RHEL 7 sanal makine hazırlayın

1. Hyper-V Manager'da sanal makineyi seçin.

1. Sanal makine için bir konsol penceresi açmak için **Bağlan'ı** tıklatın.

1. Dosyayı oluşturun `/etc/sysconfig/network` veya edin ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Dosyayı oluşturun `/etc/sysconfig/network-scripts/ifcfg-eth0` veya edin ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
    PERSISTENT_DHCLIENT=evet NM_CONTROLLED=evet

1. Ağ hizmetinin önyükleme zamanında aşağıdaki komutu çalıştırarak başlayacağından emin olun:

        # sudo systemctl enable network

1. Aşağıdaki komutu çalıştırarak RHEL deposundan paketlerin yüklenmesini sağlamak için Red Hat aboneliğinizi kaydedin:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

1. Grub yapılandırmanızdaki çekirdek önyükleme satırını Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bu değişikliği yapmak `/etc/default/grub` için metin düzenleyicisinde açın `GRUB_CMDLINE_LINUX` ve parametreyi edin. Örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Bu, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini de sağlar ve bu da hata ayıklama sorunlarında Azure desteğine yardımcı olabilir. Bu yapılandırma, NIC'ler için yeni RHEL 7 adlandırma kurallarını da kapatır. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
   
        rhgb quiet crashkernel=auto
   
    Grafiksel ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderilmesini istediğimiz bir bulut ortamında kullanışlı değildir. İstenirseniz `crashkernel` seçeneği yapılandırabilirsiniz. Bu parametrenin sanal makinedeki kullanılabilir bellek miktarını 128 MB veya daha fazla azalttığını ve bunun da daha küçük sanal makine boyutlarında sorun olabileceğini unutmayın.

1. Düzenleme `/etc/default/grub`bittikten sonra, grub yapılandırmasını yeniden oluşturmak için aşağıdaki komutu çalıştırın:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

1. SSH sunucusunun genellikle varsayılan olan önyükleme zamanında başlayacak şekilde yüklendiğinden ve yapılandırıldığından emin olun. Aşağıdaki `/etc/ssh/sshd_config` satırı içerecek şekilde değiştirin:

        ClientAliveInterval 180

1. WALinuxAgent paketi, `WALinuxAgent-<version>`Red Hat ekstralar deposuna itildi. Aşağıdaki komutu çalıştırarak ekstralar deposunu etkinleştirin:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

1. Aşağıdaki komutu çalıştırarak Azure Linux Aracısını yükleyin:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

1. İşletim sistemi diskinde takas alanı oluşturmayın.

    Azure Linux Aracısı, sanal makine Azure'da sağlandıktan sonra sanal makineye bağlı olan yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diskinin geçici bir disk olduğunu ve sanal makine nin kullanılabilirliği iptal edilirse boşalınabileceğini unutmayın. Önceki adımda Azure Linux Aracısını yükledikten sonra `/etc/waagent.conf` aşağıdaki parametreleri uygun şekilde değiştirin:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Aboneliğin kaydını açmak istiyorsanız, aşağıdaki komutu çalıştırın:

        # sudo subscription-manager unregister

1. Sanal makineyi sağlamanın kaldırılması ve Azure'da sağlama için hazırlanması için aşağıdaki komutları çalıştırın:

        # Mote: if you are migrating a specific virtual machine and do not wish to create a generalized image,
        # skip the deprovision step
        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. Hyper-V Manager'da **Eylem** > **Kapatma'yı** tıklatın. Linux VHD'niz artık Azure'a yüklenmeye hazır.


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>KVM'den Red Hat tabanlı sanal makine hazırlayın
### <a name="prepare-a-rhel-6-virtual-machine-from-kvm"></a>KVM'den BIR RHEL 6 sanal makine hazırlayın

1. RHEL 6'nın KVM görüntüsünü Red Hat web sitesinden indirin.

1. Kök parola ayarlayın.

    Şifreli bir parola oluşturun ve komutun çıktısını kopyalayın:

        # openssl passwd -1 changeme

    Konuk balığıyla kök parola ayarlayın:
        
        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Kök kullanıcının ikinci alanını "!!" dan değiştirin şifrelenmiş parolaya.

1. Qcow2 görüntüden KVM sanal bir makine oluşturun. Disk türünü **qcow2**olarak ayarlayın ve sanal ağ arabirimi aygıtı **modelini virtio**olarak ayarlayın. Ardından, sanal makineyi başlatın ve kök olarak oturum açın.

1. Dosyayı oluşturun `/etc/sysconfig/network` veya edin ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Dosyayı oluşturun `/etc/sysconfig/network-scripts/ifcfg-eth0` veya edin ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

1. Ethernet arabirimi için statik kurallar oluşturmamak için udev kurallarını taşıyın (veya kaldırın). Bu kurallar, Azure veya Hyper-V'de sanal bir makineyi klonladığınızda sorunlara neden olur:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

1. Ağ hizmetinin önyükleme zamanında aşağıdaki komutu çalıştırarak başlayacağından emin olun:

        # chkconfig network on

1. Aşağıdaki komutu çalıştırarak RHEL deposundan paketlerin yüklenmesini sağlamak için Red Hat aboneliğinizi kaydedin:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

1. Grub yapılandırmanızdaki çekirdek önyükleme satırını Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bu yapılandırmayı yapmak `/boot/grub/menu.lst` için metin düzenleyicisinde açın ve varsayılan çekirdeğin aşağıdaki parametreleri içerdiğinden emin olun:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    Bu, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini de sağlar ve bu da hata ayıklama sorunlarında Azure desteğine yardımcı olabilir.
    
    Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
    
        rhgb quiet crashkernel=auto
    
    Grafiksel ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderilmesini istediğimiz bir bulut ortamında kullanışlı değildir. İstenirseniz `crashkernel` seçeneği yapılandırabilirsiniz. Bu parametrenin sanal makinedeki kullanılabilir bellek miktarını 128 MB veya daha fazla azalttığını ve bunun da daha küçük sanal makine boyutlarında sorun olabileceğini unutmayın.


1. Initramfs hyper-V modülleri ekleyin:  

    Edin, `/etc/dracut.conf`ve aşağıdaki içeriği ekleyin:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

    Initramfs yeniden:

        # dracut -f -v

1. Bulut-init'i kaldırın:

        # yum remove cloud-init

1. SSH sunucusunun yükleme zamanında başlayacak şekilde yüklendiğinden ve yapılandırıldığından emin olun:

        # chkconfig sshd on

    /etc/ssh/sshd_config aşağıdaki satırları içerecek şekilde değiştirin:

        PasswordAuthentication yes
        ClientAliveInterval 180

1. WALinuxAgent paketi, `WALinuxAgent-<version>`Red Hat ekstralar deposuna itildi. Aşağıdaki komutu çalıştırarak ekstralar deposunu etkinleştirin:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

1. Aşağıdaki komutu çalıştırarak Azure Linux Aracısını yükleyin:

        # yum install WALinuxAgent

        # chkconfig waagent on

1. Azure Linux Aracısı, sanal makine Azure'da sağlandıktan sonra sanal makineye bağlı olan yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diskinin geçici bir disk olduğunu ve sanal makine nin kullanılabilirliği iptal edilirse boşalınabileceğini unutmayın. Azure Linux Aracısını önceki adımda yükledikten sonra **/etc/waagent.conf'ta** aşağıdaki parametreleri uygun şekilde değiştirin:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Aşağıdaki komutu çalıştırarak aboneliğin kaydını (gerekirse) unregister:

        # subscription-manager unregister

1. Sanal makineyi sağlamanın kaldırılması ve Azure'da sağlama için hazırlanması için aşağıdaki komutları çalıştırın:

        # Mote: if you are migrating a specific virtual machine and do not wish to create a generalized image,
        # skip the deprovision step
        # waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. KVM'deki sanal makineyi kapatın.

1. Qcow2 görüntüsünü VHD biçimine dönüştürün.

> [!NOTE]
> Qemu-img sürümlerinde bilinen bir hata >=2.2.1 yanlış biçimlendirilmiş bir VHD ile sonuçlanır. Sorun QEMU 2.6'da giderilmiştir. Qemu-img 2.2.0 veya daha düşük kullanılması veya 2.6 veya daha yüksek bir güncelleştirme kullanılması önerilir. Referans: https://bugs.launchpad.net/qemu/+bug/1490611.
>


    First convert the image to raw format:

        # qemu-img convert -f qcow2 -O raw rhel-6.9.qcow2 rhel-6.9.raw

    Make sure that the size of the raw image is aligned with 1 MB. Otherwise, round up the size to align with 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.9.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.9.raw $rounded_size

    Convert the raw disk to a fixed-sized VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.9.raw rhel-6.9.vhd

    Or, with qemu version **2.6+** include the `force_size` option:

        # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-6.9.raw rhel-6.9.vhd

        
### <a name="prepare-a-rhel-7-virtual-machine-from-kvm"></a>KVM'den BIR RHEL 7 sanal makine hazırlayın

1. RHEL 7'nin KVM görüntüsünü Red Hat web sitesinden indirin. Bu yordam, örnek olarak RHEL 7'yi kullanır.

1. Kök parola ayarlayın.

    Şifreli bir parola oluşturun ve komutun çıktısını kopyalayın:

        # openssl passwd -1 changeme

    Konuk balığıyla kök parola ayarlayın:

        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Kök kullanıcının ikinci alanını "!!" dan değiştirin şifrelenmiş parolaya.

1. Qcow2 görüntüden KVM sanal bir makine oluşturun. Disk türünü **qcow2**olarak ayarlayın ve sanal ağ arabirimi aygıtı **modelini virtio**olarak ayarlayın. Ardından, sanal makineyi başlatın ve kök olarak oturum açın.

1. Dosyayı oluşturun `/etc/sysconfig/network` veya edin ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Dosyayı oluşturun `/etc/sysconfig/network-scripts/ifcfg-eth0` veya edin ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
    PERSISTENT_DHCLIENT=evet NM_CONTROLLED=evet

1. Ağ hizmetinin önyükleme zamanında aşağıdaki komutu çalıştırarak başlayacağından emin olun:

        # sudo systemctl enable network

1. Aşağıdaki komutu çalıştırarak RHEL deposundan paketlerin yüklenmesini sağlamak için Red Hat aboneliğinizi kaydedin:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

1. Grub yapılandırmanızdaki çekirdek önyükleme satırını Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bu yapılandırmayı yapmak `/etc/default/grub` için metin düzenleyicisinde açın `GRUB_CMDLINE_LINUX` ve parametreyi düzenlemeyi. Örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Bu komut, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini de sağlar ve bu da hata ayıklama sorunlarında Azure desteğine yardımcı olabilir. Komut ayrıca NIC'ler için yeni RHEL 7 adlandırma kurallarını da kapatır. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
   
        rhgb quiet crashkernel=auto
   
    Grafiksel ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderilmesini istediğimiz bir bulut ortamında kullanışlı değildir. İstenirseniz `crashkernel` seçeneği yapılandırabilirsiniz. Bu parametrenin sanal makinedeki kullanılabilir bellek miktarını 128 MB veya daha fazla azalttığını ve bunun da daha küçük sanal makine boyutlarında sorun olabileceğini unutmayın.

1. Düzenleme `/etc/default/grub`bittikten sonra, grub yapılandırmasını yeniden oluşturmak için aşağıdaki komutu çalıştırın:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

1. Initramfs içine Hyper-V modülleri ekleyin.

    İçeriği `/etc/dracut.conf` edin ve ekleyin:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

    Initramfs yeniden:

        # dracut -f -v

1. Bulut-init'i kaldırın:

        # yum remove cloud-init

1. SSH sunucusunun yükleme zamanında başlayacak şekilde yüklendiğinden ve yapılandırıldığından emin olun:

        # systemctl enable sshd

    /etc/ssh/sshd_config aşağıdaki satırları içerecek şekilde değiştirin:

        PasswordAuthentication yes
        ClientAliveInterval 180

1. WALinuxAgent paketi, `WALinuxAgent-<version>`Red Hat ekstralar deposuna itildi. Aşağıdaki komutu çalıştırarak ekstralar deposunu etkinleştirin:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

1. Aşağıdaki komutu çalıştırarak Azure Linux Aracısını yükleyin:

        # yum install WALinuxAgent

    Waagent hizmetini etkinleştirin:

        # systemctl enable waagent.service

1. İşletim sistemi diskinde takas alanı oluşturmayın.

    Azure Linux Aracısı, sanal makine Azure'da sağlandıktan sonra sanal makineye bağlı olan yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diskinin geçici bir disk olduğunu ve sanal makine nin kullanılabilirliği iptal edilirse boşalınabileceğini unutmayın. Önceki adımda Azure Linux Aracısını yükledikten sonra `/etc/waagent.conf` aşağıdaki parametreleri uygun şekilde değiştirin:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Aşağıdaki komutu çalıştırarak aboneliğin kaydını (gerekirse) unregister:

        # subscription-manager unregister

1. Sanal makineyi sağlamanın kaldırılması ve Azure'da sağlama için hazırlanması için aşağıdaki komutları çalıştırın:

        # Mote: if you are migrating a specific virtual machine and do not wish to create a generalized image,
        # skip the deprovision step
        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. KVM'deki sanal makineyi kapatın.

1. Qcow2 görüntüsünü VHD biçimine dönüştürün.

> [!NOTE]
> Qemu-img sürümlerinde bilinen bir hata >=2.2.1 yanlış biçimlendirilmiş bir VHD ile sonuçlanır. Sorun QEMU 2.6'da giderilmiştir. Qemu-img 2.2.0 veya daha düşük kullanılması veya 2.6 veya daha yüksek bir güncelleştirme kullanılması önerilir. Referans: https://bugs.launchpad.net/qemu/+bug/1490611.
>


    First convert the image to raw format:

        # qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw

    Make sure that the size of the raw image is aligned with 1 MB. Otherwise, round up the size to align with 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.4.raw $rounded_size

    Convert the raw disk to a fixed-sized VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd

    Or, with qemu version **2.6+** include the `force_size` option:

        # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>VMware'den Red Hat tabanlı sanal makine hazırlayın
### <a name="prerequisites"></a>Ön koşullar
Bu bölüm, VMware'de zaten bir RHEL sanal makine yüklediğinizi varsayar. VMware'de işletim sistemi nasıl kurulabilir hakkında ayrıntılı bilgi için [VMware Guest İşletim Sistemi Kurulum Kılavuzu'na](https://partnerweb.vmware.com/GOSIG/home.html)bakın.

* Linux işletim sistemini yüklediğinizde, çoğu zaman birçok yükleme için varsayılan olan LVM yerine standart bölümler kullanmanızı öneririz. Bu, özellikle bir işletim sistemi diskinin sorun giderme için başka bir sanal makineye eklenmesi gerekiyorsa, LVM adının klonlanmış sanal makineyle çakışmasını önler. LVM veya RAID tercih edilirse veri disklerinde kullanılabilir.
* İşletim sistemi diskinde bir takas bölümü yapılandırmayın. Geçici kaynak diskinde bir takas dosyası oluşturmak için Linux aracısını yapılandırabilirsiniz. Bu konuda daha fazla bilgiyi takip eden adımlarda bulabilirsiniz.
* Sanal sabit diski oluşturduğunuzda, **sanal diski tek bir dosya olarak**depola'yı seçin.

### <a name="prepare-a-rhel-6-virtual-machine-from-vmware"></a>VMware'den bir RHEL 6 sanal makine hazırlayın
1. RHEL 6'da NetworkManager, Azure Linux aracısını etkileyebilir. Aşağıdaki komutu çalıştırarak bu paketi kaldırın:
   
        # sudo rpm -e --nodeps NetworkManager

1. /etc/sysconfig/ **dizininde** ağ adlı ve aşağıdaki metni içeren bir dosya oluşturun:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Dosyayı oluşturun `/etc/sysconfig/network-scripts/ifcfg-eth0` veya edin ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

1. Ethernet arabirimi için statik kurallar oluşturmamak için udev kurallarını taşıyın (veya kaldırın). Bu kurallar, Azure veya Hyper-V'de sanal bir makineyi klonladığınızda sorunlara neden olur:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

1. Ağ hizmetinin önyükleme zamanında aşağıdaki komutu çalıştırarak başlayacağından emin olun:

        # sudo chkconfig network on

1. Aşağıdaki komutu çalıştırarak RHEL deposundan paketlerin yüklenmesini sağlamak için Red Hat aboneliğinizi kaydedin:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

1. WALinuxAgent paketi, `WALinuxAgent-<version>`Red Hat ekstralar deposuna itildi. Aşağıdaki komutu çalıştırarak ekstralar deposunu etkinleştirin:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

1. Grub yapılandırmanızdaki çekirdek önyükleme satırını Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bunu yapmak için `/etc/default/grub` metin düzenleyicisinde açın ve `GRUB_CMDLINE_LINUX` parametreyi tıklatın. Örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"
   
   Bu, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini de sağlar ve bu da hata ayıklama sorunlarında Azure desteğine yardımcı olabilir. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
   
        rhgb quiet crashkernel=auto
   
    Grafiksel ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderilmesini istediğimiz bir bulut ortamında kullanışlı değildir. İstenirseniz `crashkernel` seçeneği yapılandırabilirsiniz. Bu parametrenin sanal makinedeki kullanılabilir bellek miktarını 128 MB veya daha fazla azalttığını ve bunun da daha küçük sanal makine boyutlarında sorun olabileceğini unutmayın.

1. Initramfs hyper-V modülleri ekleyin:

    Edin, `/etc/dracut.conf`ve aşağıdaki içeriği ekleyin:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

    Initramfs yeniden:

        # dracut -f -v

1. SSH sunucusunun genellikle varsayılan olan önyükleme zamanında başlayacak şekilde yüklendiğinden ve yapılandırıldığından emin olun. Aşağıdaki `/etc/ssh/sshd_config` satırı içerecek şekilde değiştirin:

    MüşteriAliveInterval 180

1. Aşağıdaki komutu çalıştırarak Azure Linux Aracısını yükleyin:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

1. İşletim sistemi diskinde takas alanı oluşturmayın.

    Azure Linux Aracısı, sanal makine Azure'da sağlandıktan sonra sanal makineye bağlı olan yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diskinin geçici bir disk olduğunu ve sanal makine nin kullanılabilirliği iptal edilirse boşalınabileceğini unutmayın. Önceki adımda Azure Linux Aracısını yükledikten sonra `/etc/waagent.conf` aşağıdaki parametreleri uygun şekilde değiştirin:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Aşağıdaki komutu çalıştırarak aboneliğin kaydını (gerekirse) unregister:

        # sudo subscription-manager unregister

1. Sanal makineyi sağlamanın kaldırılması ve Azure'da sağlama için hazırlanması için aşağıdaki komutları çalıştırın:

        # Mote: if you are migrating a specific virtual machine and do not wish to create a generalized image,
        # skip the deprovision step
        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. Sanal makineyi kapatın ve VMDK dosyasını .vhd dosyasına dönüştürün.

> [!NOTE]
> Qemu-img sürümlerinde bilinen bir hata >=2.2.1 yanlış biçimlendirilmiş bir VHD ile sonuçlanır. Sorun QEMU 2.6'da giderilmiştir. Qemu-img 2.2.0 veya daha düşük kullanılması veya 2.6 veya daha yüksek bir güncelleştirme kullanılması önerilir. Referans: https://bugs.launchpad.net/qemu/+bug/1490611.
>


    First convert the image to raw format:

        # qemu-img convert -f vmdk -O raw rhel-6.9.vmdk rhel-6.9.raw

    Make sure that the size of the raw image is aligned with 1 MB. Otherwise, round up the size to align with 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.9.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.9.raw $rounded_size

    Convert the raw disk to a fixed-sized VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.9.raw rhel-6.9.vhd

    Or, with qemu version **2.6+** include the `force_size` option:

        # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-6.9.raw rhel-6.9.vhd


### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a>VMware'den BIR RHEL 7 sanal makine hazırlayın
1. Dosyayı oluşturun `/etc/sysconfig/network` veya edin ve aşağıdaki metni ekleyin:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

1. Dosyayı oluşturun `/etc/sysconfig/network-scripts/ifcfg-eth0` veya edin ve aşağıdaki metni ekleyin:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
    PERSISTENT_DHCLIENT=evet NM_CONTROLLED=evet

1. Ağ hizmetinin önyükleme zamanında aşağıdaki komutu çalıştırarak başlayacağından emin olun:

        # sudo systemctl enable network

1. Aşağıdaki komutu çalıştırarak RHEL deposundan paketlerin yüklenmesini sağlamak için Red Hat aboneliğinizi kaydedin:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

1. Grub yapılandırmanızdaki çekirdek önyükleme satırını Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bu değişikliği yapmak `/etc/default/grub` için metin düzenleyicisinde açın `GRUB_CMDLINE_LINUX` ve parametreyi edin. Örneğin:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Bu yapılandırma, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini de sağlar ve bu da hata ayıklama sorunlarında Azure desteğine yardımcı olabilir. Ayrıca NIC'ler için yeni RHEL 7 adlandırma kurallarını kapatır. Ayrıca, aşağıdaki parametreleri kaldırmanızı öneririz:
   
        rhgb quiet crashkernel=auto
   
    Grafiksel ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderilmesini istediğimiz bir bulut ortamında kullanışlı değildir. İstenirseniz `crashkernel` seçeneği yapılandırabilirsiniz. Bu parametrenin sanal makinedeki kullanılabilir bellek miktarını 128 MB veya daha fazla azalttığını ve bunun da daha küçük sanal makine boyutlarında sorun olabileceğini unutmayın.

1. Düzenleme `/etc/default/grub`bittikten sonra, grub yapılandırmasını yeniden oluşturmak için aşağıdaki komutu çalıştırın:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

1. Initramfs Hyper-V modülleri ekleyin.

    Edit `/etc/dracut.conf`, içerik eklemek:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

    Initramfs yeniden:

        # dracut -f -v

1. SSH sunucusunun yükleme zamanında başlayacak şekilde yüklendiğinden ve yapılandırıldığından emin olun. Bu ayar genellikle varsayılandır. Aşağıdaki `/etc/ssh/sshd_config` satırı içerecek şekilde değiştirin:

        ClientAliveInterval 180

1. WALinuxAgent paketi, `WALinuxAgent-<version>`Red Hat ekstralar deposuna itildi. Aşağıdaki komutu çalıştırarak ekstralar deposunu etkinleştirin:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

1. Aşağıdaki komutu çalıştırarak Azure Linux Aracısını yükleyin:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

1. İşletim sistemi diskinde takas alanı oluşturmayın.

    Azure Linux Aracısı, sanal makine Azure'da sağlandıktan sonra sanal makineye bağlı olan yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diskinin geçici bir disk olduğunu ve sanal makine nin kullanılabilirliği iptal edilirse boşalınabileceğini unutmayın. Önceki adımda Azure Linux Aracısını yükledikten sonra `/etc/waagent.conf` aşağıdaki parametreleri uygun şekilde değiştirin:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

1. Aboneliğin kaydını açmak istiyorsanız, aşağıdaki komutu çalıştırın:

        # sudo subscription-manager unregister

1. Sanal makineyi sağlamanın kaldırılması ve Azure'da sağlama için hazırlanması için aşağıdaki komutları çalıştırın:

        # Mote: if you are migrating a specific virtual machine and do not wish to create a generalized image,
        # skip the deprovision step
        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

1. Sanal makineyi kapatın ve VMDK dosyasını VHD biçimine dönüştürün.

> [!NOTE]
> Qemu-img sürümlerinde bilinen bir hata >=2.2.1 yanlış biçimlendirilmiş bir VHD ile sonuçlanır. Sorun QEMU 2.6'da giderilmiştir. Qemu-img 2.2.0 veya daha düşük kullanılması veya 2.6 veya daha yüksek bir güncelleştirme kullanılması önerilir. Referans: https://bugs.launchpad.net/qemu/+bug/1490611.
>


    First convert the image to raw format:

        # qemu-img convert -f vmdk -O raw rhel-7.4.vmdk rhel-7.4.raw

    Make sure that the size of the raw image is aligned with 1 MB. Otherwise, round up the size to align with 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.4.raw $rounded_size

    Convert the raw disk to a fixed-sized VHD:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd

    Or, with qemu version **2.6+** include the `force_size` option:

        # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Otomatik olarak kickstart dosyakullanarak BIR ISO'dan Red Hat tabanlı sanal makine hazırlama
### <a name="prepare-a-rhel-7-virtual-machine-from-a-kickstart-file"></a>Kickstart dosyasından BIR RHEL 7 sanal makine hazırlama

1.  Aşağıdaki içeriği içeren bir kickstart dosyası oluşturun ve dosyayı kaydedin. Kickstart yüklemesi hakkında ayrıntılı bilgi için [Kickstart Yükleme Kılavuzu'na](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html)bakın.

        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
          auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff

        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
    PERSISTENT_DHCLIENT=evet NM_CONTROLLED=evet EOF

        # Deprovision and prepare for Azure if you are creating a generalized image
        waagent -force -deprovision

        %end

1. Kickstart dosyasını yükleme sisteminin erişebileceği bir yere yerleştirin.

1. Hyper-V Manager'da yeni bir sanal makine oluşturun. Sanal **Sabit Disk'i Bağla** sayfasında, **daha sonra sanal sabit disk ekle'yi**seçin ve Yeni Sanal Makine Sihirbazı'nı tamamlayın.

1. Sanal makine ayarlarını açın:

    a.  Sanal makineye yeni bir sanal sabit disk takın. **VHD Biçimi** ve **Sabit Boyut'u**seçtiğinizden emin olun.

    b.  Yükleme ISO'yu DVD sürücüsüne takın.

    c.  BIOS'u CD'den önyüklemeye ayarlayın.

1. Sanal makineyi başlatın. Yükleme kılavuzu göründüğünde, önyükleme seçeneklerini yapılandırmak için **Sekme'ye** basın.

1. Önyükleme seçeneklerinin sonuna girin `inst.ks=<the location of the kickstart file>` ve **Enter**tuşuna basın.

1. Yüklemenin tamamlanmasını bekleyin. Bittiğinde, sanal makine otomatik olarak kapatılır. Linux VHD'niz artık Azure'a yüklenmeye hazır.

## <a name="known-issues"></a>Bilinen sorunlar
### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Hyper-V sürücüsü, Hyper-V olmayan bir hipervizör kullanırken ilk RAM diskine dahil edilemedi

Bazı durumlarda, Linux yükleyiciler, Linux'un Hyper-V ortamında çalıştığını algılamadığı sürece, hyper-v sürücülerini ilk RAM diskine (initrd veya initramfs) dahil olmayabilir.

Linux resminizi hazırlamak için farklı bir sanallaştırma sistemi (yani VirtualBox, Xen, vb.) kullanıyorsanız, en azından hv_vmbus ve hv_storvsc çekirdek modüllerinin ilk RAM diskinde mevcut olduğundan emin olmak için initrd'i yeniden oluşturmanız gerekebilir. Bu, en azından yukarı doğru Red Hat dağılımına dayanan sistemlerde bilinen bir sorundur.

Bu sorunu gidermek için, initramfs Hyper-V modülleri ekleyin ve yeniden:

Edin, `/etc/dracut.conf`ve aşağıdaki içeriği ekleyin:

        add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "

Initramfs yeniden:

        # dracut -f -v

Daha fazla bilgi için, [initramfs yeniden hakkında](https://access.redhat.com/solutions/1958)bilgi bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Azure'da yeni sanal makineler oluşturmak için Red Hat Enterprise Linux sanal sabit diskinizi kullanmaya hazırsınız. .vhd dosyasını Azure'a ilk kez bu kez yüklediğinizde, [bkz.](upload-vhd.md#option-1-upload-a-vhd)
* Red Hat Enterprise Linux çalıştırmak için sertifikalı hipervizörler hakkında daha fazla bilgi için, [Red Hat web sitesine](https://access.redhat.com/certified-hypervisors)bakın.
* Üretime hazır RHEL BYOS görüntülerini kullanma hakkında daha fazla bilgi edinmek için BYOS için dokümantasyon [sayfasına](../workloads/redhat/byos.md)gidin.
