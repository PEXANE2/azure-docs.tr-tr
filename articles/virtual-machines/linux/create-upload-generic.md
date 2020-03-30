---
title: Linux VHD oluşturma ve yükleme
description: Linux işletim sistemi içeren bir Azure sanal sabit diski (VHD) oluşturmayı ve yüklemeyi öğrenin.
author: gbowerman
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 10/08/2018
ms.author: guybo
ms.openlocfilehash: f700dec6486bad9e7024d7c908a70dd0ff2b342c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066769"
---
# <a name="information-for-non-endorsed-distributions"></a>Onaylanmayan Dağıtımlar için Bilgiler

Azure platformu SLA, Linux işletim sistemi çalıştıran sanal makineler için yalnızca [onaylanan dağıtımlardan](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) biri kullanıldığında geçerlidir. Onaylanan bu dağıtımlar için Azure Marketi'nde önceden yapılandırılmış Linux görüntüleri sağlanır.

* [Azure'da Linux - Onaylanan Dağıtımlar](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Microsoft Azure'da Linux görüntüleri desteği](https://support.microsoft.com/kb/2941892)

Azure'da çalışan tüm dağıtımların bir dizi ön koşulu vardır. Her dağıtım farklı olduğundan, bu makale kapsamlı olamaz. Aşağıdaki tüm kriterleri karşılasanız bile, linux sisteminizin düzgün çalışması için önemli ölçüde değişiklik yapmanız gerekebilir.

[Azure Onaylı Dağıtımlarda Linux'tan](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)biriyle başlamanızı öneririz. Aşağıdaki makaleler, Azure'da desteklenen çeşitli onaylanan Linux dağıtımlarını nasıl hazırlayacağınızı gösterir:

* **[CentOS Tabanlı Dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Kırmızı Şapka Kurumsal Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Bu makale, Linux dağıtımınızı Azure'da çalıştırmak için genel kılavuza odaklanır.

## <a name="general-linux-installation-notes"></a>Genel Linux Kurulum Notları
* Hyper-V sanal sabit disk (VHDX) biçimi Azure'da desteklenmez, yalnızca *Sabit VHD'de.*  Hyper-V Manager veya [Convert-VHD](https://docs.microsoft.com/powershell/module/hyper-v/convert-vhd) cmdlet kullanarak diski VHD biçimine dönüştürebilirsiniz. VirtualBox kullanıyorsanız, diski oluştururken varsayılan (dinamik olarak ayrılmış) yerine **Sabit boyutu** seçin.
* Azure, Gen1 (BIOS önyükleme) & Gen2 (UEFI önyükleme) Sanal makineleri destekler.
* VHD için izin verilen maksimum boyut 1.023 GB'dır.
* Linux sistemini yüklerken, birçok yükleme için varsayılan olan Mantıksal Birim Yöneticisi (LVM) yerine standart bölümler kullanmanızı öneririz. Standart bölümlerin kullanılması, özellikle sorun giderme için başka bir özdeş VM'ye bir işletim sistemi diski bağlıysa, klonlanmış VM'lerle LVM adı çakışmalarını önler. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri disklerinde kullanılabilir.
* UDF dosya sistemlerinin montajı için çekirdek desteği gereklidir. Azure'da ilk önyüklemede sağlama yapılandırması, misafire iliştirilen UDF biçimli ortam kullanılarak Linux VM'ye aktarılır. Azure Linux aracısı, yapılandırmasını okumak ve VM'yi sağlamak için UDF dosya sistemini monte etmelidir.
* 2.6.37'den önceki Linux çekirdek sürümleri, Hyper-V'de daha büyük VM boyutlarına sahip NUMA'yı desteklemez. Bu sorun öncelikle upstream Red Hat 2.6.32 çekirdeğini kullanarak eski dağılımları etkiler ve Red Hat Enterprise Linux (RHEL) 6.6 (çekirdek-2.6.32-504) ile düzeltilmiştir. 2.6.37'den eski özel çekirdekler veya 2.6.32-504'ten büyük RHEL tabanlı çekirdekler çalıştıran `numa=off` sistemler, önyükleme parametresini grub.conf'taki çekirdek komut satırında ayarlamalıdır. Daha fazla bilgi için Red [Hat KB 436883'e](https://access.redhat.com/solutions/436883)bakın.
* Os diskinde bir takas bölümü yapılandırmayın. Linux aracısı, aşağıdaki adımlarda açıklandığı gibi geçici kaynak diskinde bir takas dosyası oluşturacak şekilde yapılandırılabilir.
* Azure'daki tüm VHD'lerin 1 MB'a hizalanmış bir sanal boyutu olmalıdır. Ham diskten VHD'ye dönüştürürken, ham disk boyutunun dönüşümden önce aşağıdaki adımlarda açıklandığı gibi 1 MB'ın katları olduğundan emin olmalısınız.

### <a name="installing-kernel-modules-without-hyper-v"></a>Hyper-V olmadan çekirdek modülleri yükleme
Azure Hyper-V hypervisor üzerinde çalışır, bu nedenle Linux azure'da çalıştırmak için belirli çekirdek modülleri gerektirir. Hyper-V dışında oluşturulan bir VM'iniz varsa, VM Hyper-V ortamında çalıştığını algılamıyorsa, Linux yükleyiciler ilk ramdiskte Hyper-V sürücülerini (initrd veya initramfs) içermeyebilir. Linux resminizi hazırlamak için farklı bir sanallaştırma sistemi (VirtualBox, KVM vb. gibi) kullanırken, en azından hv_vmbus ve hv_storvsc çekirdek modüllerinin ilk ramdiskte kullanılabilmesi için initrd'i yeniden oluşturmanız gerekebilir.  Bu bilinen sorun upstream Red Hat dağıtım dayalı sistemler için, ve muhtemelen diğerleri.

Initrd veya initramfs görüntü yeniden oluşturma mekanizması dağılımına bağlı olarak değişebilir. Uygun prosedür için dağıtımınızın belgelerine veya desteğine başvurun.  Aşağıda `mkinitrd` yardımcı programı kullanarak initrd yeniden oluşturmak için bir örnektir:

1. Varolan initrd görüntüsünü yedekleyin:

    ```
    cd /boot
    sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak
    ```

2. hv_vmbus ve çekirdek modülleri hv_storvsc ile initrd yeniden oluşturma:

    ```
    sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`
    ```

### <a name="resizing-vhds"></a>VHD'leri yeniden boyutlandırma
Azure'daki VHD görüntülerinin 1 MB'a hizalanmış bir sanal boyutu olmalıdır.  Genellikle, Hyper-V kullanılarak oluşturulan VHD'ler doğru hizalanır.  VHD doğru hizalanmazsa, VHD'nizden bir görüntü oluşturmaya çalıştığınızda aşağıdakilere benzer bir hata iletisi alabilirsiniz.

* VHD http:\//\<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd 21475270656 bayt desteklenmeyen bir sanal boyutu vardır. Boyutu tam sayı (MBs) olmalıdır.

Bu durumda, Hyper-V Manager konsolu veya Resize-VHD PowerShell cmdlet kullanarak VM'yi yeniden [boyutlandırın.](https://technet.microsoft.com/library/hh848535.aspx)  Windows ortamında çalışmıyorsanız, dönüştürmeyi (gerekirse) `qemu-img` ve VHD'yi yeniden boyutlandırmayı kullanmanızı öneririz.

> [!NOTE]
> [Qemu-img sürümlerinde bilinen](https://bugs.launchpad.net/qemu/+bug/1490611) bir hata var >=2.2.1 yanlış biçimlendirilmiş bir VHD ile sonuçlanır. Sorun QEMU 2.6'da giderilmiştir. `qemu-img` 2.2.0 veya daha düşük veya 2.6 veya daha yüksek kullanmanızı öneririz.
> 

1. VHD'yi doğrudan gibi araçlarla `qemu-img` `vbox-manage` yeniden boyutlandırmak veya başlatılamaz bir VHD ile sonuçlanabilir.  Önce VHD'yi RAW disk görüntüsüne dönüştürmenizi öneririz.  VM görüntüsü RAW disk görüntüsü (KVM gibi bazı hipervizörler için varsayılan) olarak oluşturulduysa, bu adımı atlayabilirsiniz.
 
    ```
    qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw
    ```

1. Disk görüntüsünün gerekli boyutunu sanal boyutun 1 MB'a hizalanmış olması için hesaplayın.  Aşağıdaki bash kabuk `qemu-img info` komut dosyası disk görüntüsünün sanal boyutunu belirlemek için kullanır ve sonra sonraki 1 MB boyutunu hesaplar.

    ```bash
    rawdisk="MyLinuxVM.raw"
    vhddisk="MyLinuxVM.vhd"

    MB=$((1024*1024))
    size=$(qemu-img info -f raw --output json "$rawdisk" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

    rounded_size=$(((($size+$MB-1)/$MB)*$MB))
    
    echo "Rounded Size = $rounded_size"
    ```

3. Yukarıdaki olarak ayarlanan `$rounded_size` ham diski yeniden boyutlandırın.

    ```bash
    qemu-img resize MyLinuxVM.raw $rounded_size
    ```

4. Şimdi, RAW diski sabit boyutlu bir VHD'ye dönüştürün.

    ```bash
    qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd
    ```

   Veya, qemu sürüm 2.6+ `force_size` ile, seçeneği ekleyin.

    ```bash
    qemu-img convert -f raw -o subformat=fixed,force_size -O vpc MyLinuxVM.raw MyLinuxVM.vhd
    ```

## <a name="linux-kernel-requirements"></a>Linux Çekirdek Gereksinimleri

Hyper-V ve Azure için Linux Entegrasyon Hizmetleri (LIS) sürücüleri doğrudan yukarı linux çekirdeğine katkıda bulunulmaktadır. Yeni bir Linux çekirdeği sürümü (3.x gibi) içeren birçok dağıtım, bu sürücüleri zaten kullanılabilir veya başka bir şekilde çekirdekleri ile bu sürücülerin backported sürümlerini sağlar.  Bu sürücüler sürekli olarak yeni düzeltmeler ve özelliklerle yukarı akış çekirdeğinde güncellenmektedir, bu nedenle mümkün olduğunda bu düzeltmeleri ve güncelleştirmeleri içeren onaylı bir [dağıtım](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) çalıştırmanızı öneririz.

Red Hat Enterprise Linux sürümleri6.0 ile 6.3'ün bir çeşidini çalıştırıyorsanız, [Hyper-V için en son LIS sürücülerini](https://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409)yüklemeniz gerekir. RHEL 6.4+ (ve türevleri) ile başlayan LIS sürücüleri çekirdekle birlikte verilir ve bu nedenle ek kurulum paketlerine gerek yoktur.

Özel bir çekirdek gerekiyorsa, yeni bir çekirdek sürümü (3,8+ gibi) öneririz. Kendi çekirdeğini koruyan dağıtımlar veya satıcılar için, LIS sürücülerini yukarı çekirdekten özel çekirdeğinize düzenli olarak geri yüklemeniz gerekir.  Zaten nispeten yeni bir çekirdek sürümünü çalıştırıyor olsanız bile, LIS sürücülerindeki yukarı akım düzeltmelerini izlemenizi ve gerektiğinde geri portlamanızı öneririz. LIS sürücü kaynak dosyalarının konumları Linux çekirdeği kaynak ağacındaki [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) dosyasında belirtilir:
```
    F:    arch/x86/include/asm/mshyperv.h
    F:    arch/x86/include/uapi/asm/hyperv.h
    F:    arch/x86/kernel/cpu/mshyperv.c
    F:    drivers/hid/hid-hyperv.c
    F:    drivers/hv/
    F:    drivers/input/serio/hyperv-keyboard.c
    F:    drivers/net/hyperv/
    F:    drivers/scsi/storvsc_drv.c
    F:    drivers/video/fbdev/hyperv_fb.c
    F:    include/linux/hyperv.h
    F:    tools/hv/
```
Aşağıdaki düzeltme emlemi çekirdeklere eklenmelidir. Bu liste tüm dağıtımlar için tamamlanamıyor.

* [ata_piix: diskleri varsayılan olarak Hyper-V sürücülerine erteler](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
* [storvsc: RESET yolundaki transit paketler için hesap](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
* [storvsc: WRITE_SAME kullanımından kaçının](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
* [storvsc: RAID ve sanal ana bilgisayar adaptörü sürücüleri için AYNı YAZ'ı devre dışı](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
* [storvsc: NULL işaretçi dereference düzeltme](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
* [storvsc: halka arabellek hataları G/Ç'nin donmasına neden olabilir](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
* [scsi_sysfs: __scsi_remove_device çifte infazkarşı korumak](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)

## <a name="the-azure-linux-agent"></a>Azure Linux Aracısı
[Azure Linux Aracısı,](../extensions/agent-linux.md) `waagent` Azure'da bir Linux sanal makinesini karşılar. [Linux Agent GitHub repo'sunda](https://github.com/Azure/WALinuxAgent)en son sürümü, dosya sorunlarını alabilir veya çekme istekleri gönderebilirsiniz.

* Linux aracısı Apache 2.0 lisansı altında piyasaya sürüldü. Birçok dağıtım zaten aracı için RPM veya .deb paketleri sağlar ve bu paketler kolayca yüklenebilir ve güncellenebilir.
* Azure Linux Aracısı Python v2.6+ gerektirir.
* Aracı ayrıca python-pyasn1 modülgerektirir. Çoğu dağıtım bu modülü kurulacak ayrı bir paket olarak sağlar.
* Bazı durumlarda, Azure Linux Aracısı NetworkManager ile uyumlu olmayabilir. Dağıtımlar tarafından sağlanan RPM/deb paketlerinin çoğu NetworkManager'ı waagent paketine çakışma olarak yapılandırır. Bu gibi durumlarda, Linux aracı paketi yüklediğinizde NetworkManager kaldıracaktır.
* Azure Linux Aracısı, desteklenen [minimum sürümde](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)veya üstünde olmalıdır.

## <a name="general-linux-system-requirements"></a>Genel Linux Sistem Gereksinimleri

1. Tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesi için GRUB veya GRUB2'deki çekirdek önyükleme satırını aşağıdaki parametreleri içerecek şekilde değiştirin. Bu iletiler, Azure desteğine herhangi bir sorunu hata ayıklamakonusunda yardımcı olabilir.
    ```  
    console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300
    ```
    Ayrıca, varsa aşağıdaki parametreleri *kaldırmanızı* öneririz.
    ```  
    rhgb quiet crashkernel=auto
    ```
    Grafiksel ve sessiz önyükleme, tüm günlüklerin seri bağlantı noktasına gönderilmesini istediğimiz bulut ortamında kullanışlı değildir. Seçenek `crashkernel` gerekirse yapılandırılmış olarak bırakılabilir, ancak bu parametrenin VM'deki kullanılabilir bellek miktarını en az 128 MB azalttığını ve bu da daha küçük VM boyutları için sorunlu olabileceğini unutmayın.

1. Azure Linux Aracısını yükleyin.
  
    Azure Linux Aracısı, Azure'da bir Linux görüntüsü sağlamaiçin gereklidir.  Birçok dağıtım aracıyı RPM veya .deb paketi olarak sağlar (paket genellikle WALinuxAgent veya walinuxagent olarak adlandırılır).  Aracı, [Linux Agent Guide'daki](../extensions/agent-linux.md)adımları izleyerek el ile de yüklenebilir.

1. SSH sunucusunun yüklendiğinden ve önyükleme zamanında başlayacak şekilde yapılandırıldığından emin olun.  Bu yapılandırma genellikle varsayılandır.

1. İşletim sistemi diskinde takas alanı oluşturmayın.
  
    Azure Linux Aracısı, Azure'da sağlama yaptıktan sonra VM'ye bağlı yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diski *geçici* bir disktir ve VM deprovisioned olduğunda boşaltılabilir. Azure Linux Aracısını yükledikten sonra (yukarıdaki adım 2), gerektiğinde /etc/waagent.conf'ta aşağıdaki parametreleri değiştirin.
    ```  
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: Set this to your desired size.
    ```
1. Sanal makineyi etkisiz hale getirmek için aşağıdaki komutları çalıştırın.
  
     ```
     sudo waagent -force -deprovision
     export HISTSIZE=0
     logout
     ```  
   > [!NOTE]
   > Virtualbox'ta aşağıdaki hatayı görebilirsiniz. `waagent -force -deprovision` `[Errno 5] Input/output error` Bu hata iletisi kritik değildir ve yoksayılabilir.

* Sanal makineyi kapatın ve VHD'yi Azure'a yükleyin.

