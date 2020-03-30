---
title: Bir Debian Linux VHD hazırlayın
description: Azure'daki VM dağıtımları için Debian VHD görüntüleri nin nasıl oluşturulabildiğini öğrenin.
author: gbowerman
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 11/13/2018
ms.author: guybo
ms.openlocfilehash: d54f7a11d929c31fee29a788eb3a2ae2cc8f2703
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066707"
---
# <a name="prepare-a-debian-vhd-for-azure"></a>Azure için Bir Debian VHD hazırlama
## <a name="prerequisites"></a>Ön koşullar
Bu bölüm, [Debian web sitesinden](https://www.debian.org/distrib/) sanal bir sabit diske indirilen bir .iso dosyasından bir Debian Linux işletim sistemi yüklediğinizi varsayar. .vhd dosyaları oluşturmak için birden çok araç vardır; Hyper-V sadece bir örnektir. Hyper-V'yi kullanan talimatlar için [Hyper-V Rolünü Yükleyin ve Sanal Makineyi Yapılandırın'](https://technet.microsoft.com/library/hh846766.aspx)a bakın.

## <a name="installation-notes"></a>Yükleme notları
* Linux'u Azure'a hazırlama hakkında daha fazla ipucu için [Genel Linux Yükleme Notları'na](create-upload-generic.md#general-linux-installation-notes) da bakın.
* Azure'da yeni VHDX biçimi desteklenmez. Hyper-V Manager veya **convert-vhd** cmdlet kullanarak diski VHD formatına dönüştürebilirsiniz.
* Linux sistemini yüklerken, LVM yerine standart bölümler kullanmanız önerilir (genellikle birçok yükleme için varsayılan). Bu, özellikle bir işletim sistemi diskinin sorun giderme için başka bir VM'ye eklenmesi gerekiyorsa, klonlanmış VM'lerle LVM adı çakışmalarını önler. Tercih edilirse veri disklerinde [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kullanılabilir.
* OS diskinde bir takas bölümü yapılandırmayın. Azure Linux aracısı, geçici kaynak diskinde bir takas dosyası oluşturacak şekilde yapılandırılabilir. Daha fazla bilgiyi aşağıdaki adımlarda bulabilirsiniz.
* Azure'daki tüm VHD'lerin 1MB'ye hizalanmış bir sanal boyutu olmalıdır. Ham diskten VHD'ye dönüştürürken, dönüştürmeden önce ham disk boyutunun 1MB'nin katı olduğundan emin olmalısınız. Daha fazla bilgi için [Linux Yükleme Notları'na](create-upload-generic.md#general-linux-installation-notes)bakın.

## <a name="use-azure-manage-to-create-debian-vhds"></a>Debian VHD'leri oluşturmak için Azure-Yönet'i kullanma
Azure için Debian VHD'leri oluşturmak için Kullanılabilir araçlar vardır [(örneğin, Credativ'in](https://www.credativ.com/) [azure yönetimi](https://github.com/credativ/azure-manage) komut dosyaları. Bu sıfırdan bir görüntü oluşturmaya karşı önerilen bir yaklaşımdır. Örneğin, bir Debian 8 VHD oluşturmak için `azure-manage` yardımcı programı (ve bağımlılıkları) `azure_build_image` indirmek ve komut dosyasını çalıştırmak için aşağıdaki komutları çalıştırın:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>El ile bir Debian VHD hazırlamak
1. Hyper-V Manager'da sanal makineyi seçin.
2. Sanal makine için bir konsol penceresi açmak için **Bağlan'ı** tıklatın.
3. Bir ISO kullanarak işletim sistemi yüklü, sonra " "`deb cdrom`ile `/etc/apt/source.list`ilgili herhangi bir satır dışarı yorum .

4. Dosyayı `/etc/default/grub` edin ve Azure için ek çekirdek parametreleri içerecek şekilde **GRUB_CMDLINE_LINUX** parametresini aşağıdaki gibi değiştirin.
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200n8 earlyprintk=ttyS0,115200"

5. Grub'u yeniden oluştur ve çalıştır:

        # sudo update-grub

6. Debian'ın Azure depolarını /etc/apt/sources.list'e Debian 8 veya 9 için ekleyin:

    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie main
        deb-src http://debian-archive.trafficmanager.net/debian jessie main
        deb http://debian-archive.trafficmanager.net/debian-security jessie/updates main
        deb-src http://debian-archive.trafficmanager.net/debian-security jessie/updates
        deb http://debian-archive.trafficmanager.net/debian jessie-updates main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-updates main
        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main

    **Debian 9.x "Stretch"**

        deb http://debian-archive.trafficmanager.net/debian stretch main
        deb-src http://debian-archive.trafficmanager.net/debian stretch main
        deb http://debian-archive.trafficmanager.net/debian-security stretch/updates main
        deb-src http://debian-archive.trafficmanager.net/debian-security stretch/updates main
        deb http://debian-archive.trafficmanager.net/debian stretch-updates main
        deb-src http://debian-archive.trafficmanager.net/debian stretch-updates main
        deb http://debian-archive.trafficmanager.net/debian stretch-backports main
        deb-src http://debian-archive.trafficmanager.net/debian stretch-backports main


7. Azure Linux Aracısını yükleyin:
   
        # sudo apt-get update
        # sudo apt-get install waagent

8. Debian 9+ için, Azure'da VM'lerle kullanılmak üzere yeni Debian Cloud çekirdeğini kullanmanız önerilir. Bu yeni çekirdeği yüklemek için öncelikle /etc/apt/preferences.d/linux.pref adlı bir dosya oluşturun:
   
        Package: linux-* initramfs-tools
        Pin: release n=stretch-backports
        Pin-Priority: 500
   
    Sonra yeni Debian Cloud çekirdek yüklemek için "sudo apt-get install linux-image-cloud-amd64" çalıştırın.

9. Sanal makineyi deprovision ve Azure'da sağlama için hazırlayın ve çalıştırın:
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

10. Hyper-V Manager'da **Eylem** -> Kapat'ı tıklatın. Linux VHD'niz artık Azure'a yüklenmeye hazır.

## <a name="next-steps"></a>Sonraki adımlar
Azure'da yeni sanal makineler oluşturmak için Debian sanal sabit diskinizi kullanmaya hazırsınız. .vhd dosyasını Azure'a ilk kez bu kez yüklediğinizde, [bkz.](upload-vhd.md#option-1-upload-a-vhd)

