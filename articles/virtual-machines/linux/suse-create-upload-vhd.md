---
title: Azure'da Bir SUSE Linux VHD oluşturma ve yükleme
description: SUSE Linux işletim sistemi içeren bir Azure sanal sabit diski (VHD) oluşturmayı ve yüklemeyi öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: gbowerman
manager: gwallace
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 066d01a6-2a54-4718-bcd0-90fe7a5303a1
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 03/12/2018
ms.author: guybo
ms.openlocfilehash: 7bfe0aabbf2318643b59d57519be1475648a12ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066564"
---
# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Azure'da SLES veya openSUSE sanal makinesi hazırlama


Bu makalede, sanal bir sabit diske zaten bir SUSE veya openSUSE Linux işletim sistemi yüklediğinizi varsayar. .vhd dosyaları oluşturmak için birden çok araç vardır, örneğin Hyper-V gibi bir sanallaştırma çözümü. Talimatlar için [hyper-v rolünü yükleyin ve Sanal Makineyi Yapılandırın'](https://technet.microsoft.com/library/hh846766.aspx)a bakın.

## <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE kurulum notları
* Linux'u Azure'a hazırlama hakkında daha fazla ipucu için lütfen [Genel Linux Yükleme Notları'na](create-upload-generic.md#general-linux-installation-notes) bakın.
* VHDX biçimi Azure'da desteklenmez, yalnızca **Sabit VHD'de**dir.  Hyper-V Manager veya convert-vhd cmdlet kullanarak diski VHD formatına dönüştürebilirsiniz.
* Linux sistemini yüklerken LVM yerine standart bölümler kullanmanız önerilir (genellikle birçok kurulum için varsayılan). Bu, özellikle bir işletim sistemi diskinin sorun giderme için başka bir VM'ye eklenmesi gerekiyorsa, klonlanmış VM'lerle LVM adı çakışmalarını önler. Tercih edilirse veri disklerinde [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kullanılabilir.
* OS diskinde bir takas bölümü yapılandırmayın. Linux aracısı geçici kaynak diskinde bir takas dosyası oluşturmak için yapılandırılabilir.  Bu konuda daha fazla bilgiyi aşağıdaki adımlarda bulabilirsiniz.
* Azure'daki tüm VHD'lerin 1MB'ye hizalanmış bir sanal boyutu olmalıdır. Ham diskten VHD'ye dönüştürürken, dönüştürmeden önce ham disk boyutunun 1MB'nin katı olduğundan emin olmalısınız. Daha fazla bilgi için [Linux Yükleme Notları'na](create-upload-generic.md#general-linux-installation-notes) bakın.

## <a name="use-suse-studio"></a>SUSE Studio'u kullanma
[SUSE Studio,](http://www.susestudio.com) Azure ve Hyper-V için SLES'inizi kolayca oluşturabilir ve yönetebilir ve SUSE görüntülerini açabilir. Bu, kendi SLES ve openSUSE görüntüleri özelleştirmek için önerilen bir yaklaşımdır.

Kendi VHD oluşturmaalternatif olarak, SUSE da ByOS yayınlar (Kendi Abonelik getir) [VMDepot](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/04/using-and-contributing-vms-to-vm-depot.pdf)de SLES için görüntüleri.

## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>Hazırlamak SUSE Linux Enterprise Server 11 SP4
1. Hyper-V Manager'ın orta bölmesinde sanal makineyi seçin.
2. Sanal makinenin penceresini açmak için **Bağlan'ı** tıklatın.
3. Güncellemeleri karşıdan yükleyip paketleri yüklemesine izin vermek için SUSE Linux Enterprise sisteminizi kaydedin.
4. Sistemi en son düzeltme ekleriyle güncelleyin:
   
        # sudo zypper update
5. Azure Linux Aracısını SLES deposundan yükleyin:
   
        # sudo zypper install python-azure-agent
6. Waagent'ın chkconfig'de "açık" olarak ayarlanıp ayarlanmadığını denetleyin ve değilse otomatik başlatma için etkinleştirin:
   
        # sudo chkconfig waagent on
7. Waagent hizmetinin çalışır durumda olup olmadığını denetleyin ve çalıştırın: 
   
        # sudo service waagent start
8. Grub yapılandırmanızdaki çekirdek önyükleme satırını Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bunu bir metin düzenleyicisinde "/boot/grub/menu.lst" olarak açın ve varsayılan çekirdeğin aşağıdaki parametreleri içerdiğinden emin olun:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
    Bu, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini sağlar ve bu da hata ayıklama sorunlarında Azure desteğine yardımcı olabilir.
9. /boot/grub/menu.lst ve /etc/fstab her iki başvuru disk kimliği (by-id) yerine UUID (by-uuid) kullanarak disk onaylayın. 
   
    Disk UUID'yi alın
   
        # ls /dev/disk/by-uuid/
   
    /dev/disk/by-id/ kullanılırsa, hem /boot/grub/menu.lst ve /etc/fstab'ı uygun by-uuid değeriyle güncelleyin
   
    Değişikliköncesi
   
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1
   
    Değişiklikten sonra
   
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
10. Ethernet arabirimi(ler) için statik kurallar oluşturmamak için udev kurallarını değiştirin. Bu kurallar, Microsoft Azure veya Hyper-V'de sanal bir makineklonlarken sorunlara neden olabilir:
    
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
11. "/etc/sysconfig/network/dhcp" dosyasını ve parametreyi `DHCLIENT_SET_HOSTNAME` aşağıdakilerle değiştirmen önerilir:
    
     DHCLIENT_SET_HOSTNAME="hayır"
12. "/etc/sudoers" olarak, varsa aşağıdaki satırları yorum yapın veya kaldırın:
    
    ```
     Defaults targetpw   # ask for the password of the target user i.e. root
     ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!
     ```
13. SSH sunucusunun yükleme zamanında başlayacak şekilde yüklendiğinden ve yapılandırıldığından emin olun.  Bu genellikle varsayılan dır.
14. İşletim sistemi diskinde takas alanı oluşturmayın.
    
    Azure Linux Aracısı, Azure'da sağlama yaptıktan sonra VM'ye bağlı yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diskinin *geçici* bir disk olduğunu ve VM deprovisioned olduğunda boşalınabileceğini unutmayın. Azure Linux Aracısını yükledikten sonra (önceki adıma bakın), /etc/waagent.conf'ta aşağıdaki parametreleri uygun şekilde değiştirin:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## NOT: Bunu neye ihtiyacınız varsa ayarlayın.
15. Sanal makineyi sağlamanın kaldırılması ve Azure'da sağlama için hazırlanması için aşağıdaki komutları çalıştırın:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
16. Hyper-V Manager'da **Eylem -> Kapat'ı** tıklatın. Linux VHD'niz artık Azure'a yüklenmeye hazır.

---
## <a name="prepare-opensuse-131"></a>Açık hazırlayınSUSE 13.1+
1. Hyper-V Manager'ın orta bölmesinde sanal makineyi seçin.
2. Sanal makinenin penceresini açmak için **Bağlan'ı** tıklatın.
3. Kabuğun üzerinde , '`zypper lr`' 'komutu çalıştırın. Bu komut aşağıdakine benzer çıktı döndürürse, depolar beklendiği gibi yapılandırılır;
   
        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes
   
    Komut "Tanımlanmamış depolar..." komutunu döndürürse. sonra bu depoları eklemek için aşağıdaki komutları kullanın:
   
        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f https://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates
   
    Daha sonra ' ' '`zypper lr`komutu yeniden çalıştırılarak depoların eklendiğini doğrulayabilirsiniz. İlgili güncelleştirme depolarından birinin etkin olmaması durumunda, aşağıdaki komutla etkinleştirin:
   
        # sudo zypper mr -e [NUMBER OF REPOSITORY]
4. Çekirdeği en son kullanılabilir sürüme güncelleştirin:
   
        # sudo zypper up kernel-default
   
    Veya en son yamalar ile sistemi güncellemek için:
   
        # sudo zypper update
5. Azure Linux Aracısını yükleyin.
   
        # sudo zypper install WALinuxAgent
6. Grub yapılandırmanızdaki çekirdek önyükleme satırını Azure için ek çekirdek parametreleri içerecek şekilde değiştirin. Bunu yapmak için metin düzenleyicisinde "/boot/grub/menu.lst" adlı kitabı açın ve varsayılan çekirdeğin aşağıdaki parametreleri içerdiğinden emin olun:
   
     konsol=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
   Bu, tüm konsol iletilerinin ilk seri bağlantı noktasına gönderilmesini sağlar ve bu da hata ayıklama sorunlarında Azure desteğine yardımcı olabilir. Ayrıca, varsa çekirdek önyükleme satırından aşağıdaki parametreleri kaldırın:
   
     libata.atapi_enabled=0 rezerv=0x1f0,0x8
7. "/etc/sysconfig/network/dhcp" dosyasını ve parametreyi `DHCLIENT_SET_HOSTNAME` aşağıdakilerle değiştirmen önerilir:
   
     DHCLIENT_SET_HOSTNAME="hayır"
8. **Önemli:** "/etc/sudoers" olarak, varsa aşağıdaki satırları yorum yapın veya kaldırın:
     
     ```
     Defaults targetpw   # ask for the password of the target user i.e. root
     ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!
     ```

9. SSH sunucusunun yükleme zamanında başlayacak şekilde yüklendiğinden ve yapılandırıldığından emin olun.  Bu genellikle varsayılan dır.
10. İşletim sistemi diskinde takas alanı oluşturmayın.
    
    Azure Linux Aracısı, Azure'da sağlama yaptıktan sonra VM'ye bağlı yerel kaynak diskini kullanarak takas alanını otomatik olarak yapılandırabilir. Yerel kaynak diskinin *geçici* bir disk olduğunu ve VM deprovisioned olduğunda boşalınabileceğini unutmayın. Azure Linux Aracısını yükledikten sonra (önceki adıma bakın), /etc/waagent.conf'ta aşağıdaki parametreleri uygun şekilde değiştirin:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## NOT: Bunu neye ihtiyacınız varsa ayarlayın.
11. Sanal makineyi sağlamanın kaldırılması ve Azure'da sağlama için hazırlanması için aşağıdaki komutları çalıştırın:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
12. Azure Linux Aracısı'nın başlangıçta çalıştığından emin olun:
    
        # sudo systemctl enable waagent.service
13. Hyper-V Manager'da **Eylem -> Kapat'ı** tıklatın. Linux VHD'niz artık Azure'a yüklenmeye hazır.

## <a name="next-steps"></a>Sonraki adımlar
Azure'da yeni sanal makineler oluşturmak için SUSE Linux sanal sabit diskinizi kullanmaya hazırsınız. .vhd dosyasını Azure'a ilk kez bu kez yüklediğinizde, [bkz.](upload-vhd.md#option-1-upload-a-vhd)
