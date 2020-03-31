---
title: Azure'da Bir Ubuntu Linux VHD oluşturma ve yükleme
description: Ubuntu Linux işletim sistemi içeren bir Azure sanal sabit diski (VHD) oluşturmayı ve yüklemeyi öğrenin.
author: gbowerman
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 06/24/2019
ms.author: guybo
ms.openlocfilehash: 5fa3415d8663f358bf0ae48be46ac52b8f8b4b06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066723"
---
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Azure’da Ubuntu sanal makinesi hazırlama


Ubuntu şimdi indirmek için resmi Azure VHD'leri [https://cloud-images.ubuntu.com/](https://cloud-images.ubuntu.com/)yayınlar. Azure için kendi özel Ubuntu resminizi oluşturmanız gerekiyorsa, aşağıdaki manuel yordamı kullanmak yerine, bu bilinen çalışan VHD'lerle başlayıp gerektiğinde özelleştirmeniz önerilir. En son görüntü sürümleri her zaman aşağıdaki konumlarda bulunabilir:

* Ubuntu 12.04/Hassas: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 14.04/Güvenilir: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vmdk](https://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vmdk)
* Ubuntu 18.04/Bionic: [biyonik-sunucu-cloudimg-amd64.vmdk](https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.vmdk)
* Ubuntu 18.10/Kozmik: [kozmik-sunucu-cloudimg-amd64.vhd.zip](http://cloud-images.ubuntu.com/releases/cosmic/release/ubuntu-18.10-server-cloudimg-amd64.vhd.zip)

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, sanal bir sabit diske zaten bir Ubuntu Linux işletim sistemi yüklediğinizi varsayar. .vhd dosyaları oluşturmak için birden çok araç vardır, örneğin Hyper-V gibi bir sanallaştırma çözümü. Talimatlar için [hyper-v rolünü yükleyin ve Sanal Makineyi Yapılandırın'](https://technet.microsoft.com/library/hh846766.aspx)a bakın.

**Ubuntu yükleme notları**

* Linux'u Azure'a hazırlama hakkında daha fazla ipucu için lütfen [Genel Linux Yükleme Notları'na](create-upload-generic.md#general-linux-installation-notes) bakın.
* VHDX biçimi Azure'da desteklenmez, yalnızca **Sabit VHD'de**dir.  Hyper-V Manager veya convert-vhd cmdlet kullanarak diski VHD formatına dönüştürebilirsiniz.
* Linux sistemini yüklerken LVM yerine standart bölümler kullanmanız önerilir (genellikle birçok kurulum için varsayılan). Bu, özellikle bir işletim sistemi diskinin sorun giderme için başka bir VM'ye eklenmesi gerekiyorsa, klonlanmış VM'lerle LVM adı çakışmalarını önler. Tercih edilirse veri disklerinde [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kullanılabilir.
* OS diskinde bir takas bölümü yapılandırmayın. Linux aracısı geçici kaynak diskinde bir takas dosyası oluşturmak için yapılandırılabilir.  Bu konuda daha fazla bilgiyi aşağıdaki adımlarda bulabilirsiniz.
* Azure'daki tüm VHD'lerin 1MB'ye hizalanmış bir sanal boyutu olmalıdır. Ham diskten VHD'ye dönüştürürken, dönüştürmeden önce ham disk boyutunun 1MB'nin katı olduğundan emin olmalısınız. Daha fazla bilgi için [Linux Yükleme Notları'na](create-upload-generic.md#general-linux-installation-notes) bakın.

## <a name="manual-steps"></a>Manuel adımlar
> [!NOTE]
> Azure için kendi özel Ubuntu resminizi oluşturmaya çalışmadan [https://cloud-images.ubuntu.com/](https://cloud-images.ubuntu.com/) önce, bunun yerine önceden oluşturulmuş ve test edilmiş görüntüleri kullanmayı düşünün.
> 
> 

1. Hyper-V Manager'ın orta bölmesinde sanal makineyi seçin.

2. Sanal makinenin penceresini açmak için **Bağlan'ı** tıklatın.

3. Ubuntu'nun Azure deposunu kullanmak için görüntüdeki geçerli depoları değiştirin. Adımlar Ubuntu sürümüne bağlı olarak biraz değişir.
   
    Düzenlemeden `/etc/apt/sources.list`önce, yedekleme yapmak önerilir:
   
        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 16.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

4. Ubuntu Azure görüntüleri artık *donanım etkinleştirme* (HWE) çekirdeğini takip ediyor. Aşağıdaki komutları çalıştırarak işletim sistemini en son çekirdek olarak güncelleştirin:

    Ubuntu 12.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot
   
    Ubuntu 14.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot

    Ubuntu 16.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-generic-hwe-16.04 linux-cloud-tools-generic-hwe-16.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot
    
    Ubuntu 18.04.04:
    
        # sudo apt-get update
        # sudo apt-get install --install-recommends linux-generic-hwe-18.04 xserver-xorg-hwe-18.04
        # sudo apt-get install --install-recommends linux-cloud-tools-generic-hwe-18.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot
    
    **Ayrıca bakınız:**
    - [https://wiki.ubuntu.com/Kernel/LTSEnablementStack](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. Grub için çekirdek önyükleme satırını Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bunu bir `/etc/default/grub` metin düzenleyicisinde açmak için, `GRUB_CMDLINE_LINUX_DEFAULT` adlı değişkeni bulun (veya gerekirse ekleyin) bulun ve aşağıdaki parametreleri içerecek şekilde edin:
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Bu dosyayı kaydedip kapatın `sudo update-grub`ve sonra çalıştırın. Bu, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini sağlar ve bu da hata ayıklama sorunlarıyla Azure teknik desteğine yardımcı olabilir.

6. SSH sunucusunun yükleme zamanında başlayacak şekilde yüklendiğinden ve yapılandırıldığından emin olun.  Bu genellikle varsayılan dır.

7. Azure Linux Aracısını yükleyin:
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

   > [!Note]
   >  Paket, `walinuxagent` yüklenirse `NetworkManager` paketleri ve `NetworkManager-gnome` paketleri kaldırabilir.


1. Sanal makineyi sağlamanın kaldırılması ve Azure'da sağlama için hazırlanması için aşağıdaki komutları çalıştırın:
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

1. Hyper-V Manager'da **Eylem -> Kapat'ı** tıklatın. Linux VHD'niz artık Azure'a yüklenmeye hazır.

## <a name="references"></a>Başvurular
[Ubuntu donanım etkinleştirme (HWE) çekirdeği](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)

## <a name="next-steps"></a>Sonraki adımlar
Azure'da yeni sanal makineler oluşturmak için Ubuntu Linux sanal sabit diskinizi kullanmaya hazırsınız. .vhd dosyasını Azure'a ilk kez bu kez yüklediğinizde, [bkz.](upload-vhd.md#option-1-upload-a-vhd)

