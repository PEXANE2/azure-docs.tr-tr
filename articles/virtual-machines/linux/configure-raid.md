---
title: Linux VM'de yazılım RAID'i yapılandırma
description: AZURE'da RAID'i Linux'ta yapılandırmak için mdadm'ı nasıl kullanacağınızı öğrenin.
author: rickstercdn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 02/02/2017
ms.author: rclaus
ms.subservice: disks
ms.openlocfilehash: 122abda51b907491b322908c3c2c689bc1723e87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250263"
---
# <a name="configure-software-raid-on-linux"></a>Linux'ta Yazılım RAID'i yapılandırın
Birden çok bağlı veri diskini tek bir RAID aygıtı olarak sunmak için Azure'daki Linux sanal makinelerinde RAID yazılımlarını kullanmak yaygın bir senaryo. Genellikle bu, performansı artırmak ve yalnızca tek bir disk kullanarak karşılaştırıldığında gelişmiş iş elde edilmesine olanak sağlamak için kullanılabilir.

## <a name="attaching-data-disks"></a>Veri diskleri ekleme
RAID aygıtını yapılandırmak için iki veya daha fazla boş veri diski gerekir.  RAID aygıtı oluşturmanın birincil nedeni diskInizin IO performansını artırmaktır.  IO ihtiyaçlarınıza bağlı olarak, Standart Depolama alanımızda depolanan diskleri disk başına 500'e kadar IO/ps veya disk başına 5000 IO/ps'ye kadar Premium depolama alanımızla eklemeyi seçebilirsiniz. Bu makalede, bir Linux sanal makineye veri disklerinin nasıl sağlanıp eklendirilen hakkında ayrıntılı bilgi verilmeyecektir.  Azure'daki bir Linux sanal makinesine boş bir veri diskinin nasıl eklenecek lerine ilişkin ayrıntılı talimatlar [için](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Microsoft Azure makalesine bakın.

> [!IMPORTANT]
>Farklı boyutlardaki diskleri karıştırmayın, bunu yapmak raidset performansının en yavaş diskinperformansıyla sınırlı olmasını sağlar. 

## <a name="install-the-mdadm-utility"></a>mdadm yardımcı programını yükleyin
* **Ubuntu**
  ```bash
  sudo apt-get update
  sudo apt-get install mdadm
  ```

* **Oracle Linux& CentOS**
  ```bash
  sudo yum install mdadm
  ```

* **SLES ve openSUSE**
  ```bash  
  zypper install mdadm
  ```

## <a name="create-the-disk-partitions"></a>Disk bölümlerini oluşturma
Bu örnekte, /dev/sdc üzerinde tek bir disk bölümü oluştururuz. Yeni disk bölümü /dev/sdc1 olarak adlandırılır.

1. Bölüm `fdisk` oluşturmaya başlamaya başla

    ```bash
    sudo fdisk /dev/sdc
    Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
    Building a new DOS disklabel with disk identifier 0xa34cb70c.
    Changes will remain in memory only, until you decide to write them.
    After that, of course, the previous content won't be recoverable.

    WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
                    switch off the mode (command 'c') and change display units to
                    sectors (command 'u').
    ```

1. **N**ew bölümü oluşturmak için komut isteminde 'n' tuşuna basın:

    ```bash
    Command (m for help): n
    ```

1. Ardından, **p**rimary bölümü oluşturmak için 'p' tuşuna basın:

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

1. Bölüm numarası 1'i seçmek için '1' tuşuna basın:

    ```bash
    Partition number (1-4): 1
    ```

1. Yeni bölümün başlangıç noktasını seçin veya `<enter>` bölümü sürücüdeki boş alanın başına yerleştirmek için varsayılanı kabul etmek için basın:

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

1. 10 gigabaytlık bir bölüm oluşturmak için bölüm boyutunu seçin, örneğin '+10G' yazın. Veya, `<enter>` tüm sürücüyü kapsayan tek bir bölüm oluşturun' tuşuna basın:

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

1. Ardından, varsayılan kimlik '83' (Linux) 'fd' (Linux raid auto) için bölümün kimliğini ve **t**ype'ini değiştirin:

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L to list codes): fd
    ```

1. Son olarak, sürücüve çıkış fdisk için bölüm tablosu yazın:

    ```bash   
    Command (m for help): w
    The partition table has been altered!
    ```

## <a name="create-the-raid-array"></a>RAID dizisini oluşturma
1. Aşağıdaki örnekte üç ayrı veri diskinde (sdc1, sdd1, sde1) bulunan üç bölüm "şerit" (RAID düzeyi 0) olacaktır.  Bu komutu çalıştırdıktan sonra **/dev/md127** adında yeni bir RAID aygıtı oluşturulur. Ayrıca, bu veri diskleri daha önce başka bir geçersiz RAID dizisinin parçası `--force` olsaydı, `mdadm` komuta parametre eklemek gerekebilir unutmayın:

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

1. Yeni RAID aygıtında dosya sistemini oluşturma
   
    **CentOS, Oracle Linux, SLES 12, openSUSE ve Ubuntu**

    ```bash   
    sudo mkfs -t ext4 /dev/md127
    ```
   
    **SLES 11**

    ```bash
    sudo mkfs -t ext3 /dev/md127
    ```
   
    **SLES 11** - boot.md etkinleştirin ve mdadm.conf oluşturmak

    ```bash
    sudo -i chkconfig --add boot.md
    sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf
    ```
   
   > [!NOTE]
   > SUSE sistemlerinde bu değişiklikleri yaptıktan sonra yeniden başlatma gerekebilir. Bu adım SLES 12 için gerekli *değildir.*
   > 
   

## <a name="add-the-new-file-system-to-etcfstab"></a>Yeni dosya sistemini /etc/fstab'a ekleme
> [!IMPORTANT]
> /etc/fstab dosyasının yanlış düzenlenmesi başlatılamaz bir sisteme neden olabilir. Emin değilseniz, bu dosyayı doğru düzenleme hakkındaki bilgiler için dağıtımın belgelerine bakın. Düzenlemeden önce /etc/fstab dosyasının bir yedeklemesinin oluşturulması da önerilir.

1. Örneğin, yeni dosya sisteminiz için istediğiniz montaj noktasını oluşturun:

    ```bash
    sudo mkdir /data
    ```
1. /etc/fstab düzenlerken, **UUID** aygıt adı yerine dosya sistemine başvurmak için kullanılmalıdır.  Yeni `blkid` dosya sistemi için UUID'yi belirlemek için yardımcı programı kullanın:

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

1. Bir metin düzenleyicisinde /etc/fstab'ı açın ve örneğin yeni dosya sistemi için bir giriş ekleyin:

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    Veya **SLES 11'de**:

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    Sonra, kaydedin ve kapatın /etc/fstab.

1. /etc/fstab girişinin doğru olduğunu test edin:

    ```bash  
    sudo mount -a
    ```

    Bu komut bir hata iletisi ile sonuçlanırsa, lütfen /etc/fstab dosyasındaki sözdizimini kontrol edin.
   
    Dosya sisteminin `mount` monte edilmesini sağlamak için komutu sonraki çalıştırın:

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

1. (İsteğe bağlı) Failsafe Önyükleme Parametreleri
   
    **fstab yapılandırma**
   
    Birçok dağıtım , `nobootwait` /etc/fstab dosyasına eklenebilecek veya `nofail` monte parametrelerini içerir. Bu parametreler, belirli bir dosya sistemi monte ederken hatalara izin verir ve RAID dosya sistemini düzgün bir şekilde monte edemese bile Linux sisteminin önyüklemeye devam etmesine olanak sağlar. Bu parametreler hakkında daha fazla bilgi için dağıtımınızın belgelerine bakın.
   
    Örnek (Ubuntu):

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    **Linux önyükleme parametreleri**
   
    Yukarıdaki parametrelere ek olarak, ""`bootdegraded=true`çekirdek parametresi, RAID hasarlı veya bozulmuş olarak algılasa bile( örneğin bir veri sürücüsü yanlışlıkla sanal makineden kaldırılırsa) sistemin önyüklemesine izin verebilir. Varsayılan olarak bu da önyükleme olmayan bir sistem neden olabilir.
   
    Çekirdek parametrelerinin düzgün bir şekilde nasıl dinlenir? Örneğin, birçok dağıtımda (CentOS, Oracle Linux, SLES 11) bu parametreler`/boot/grub/menu.lst`" " dosyasına el ile eklenebilir.  Ubuntu'da bu parametre "/etc/default/grub" değişkenine `GRUB_CMDLINE_LINUX_DEFAULT` eklenebilir.


## <a name="trimunmap-support"></a>TRIM/UNMAP desteği
Bazı Linux çekirdekleri, diskteki kullanılmayan blokları atmak için TRIM/UNMAP işlemlerini destekler. Bu işlemler, azure'a silinen sayfaların artık geçerli olmadığını ve atılabilir olduğunu bildirmek için öncelikle standart depolama alanında yararlıdır. Sayfaları atmak, büyük dosyalar oluşturup silerseniz maliyet tasarrufu sağlayabilir.

> [!NOTE]
> Dizinin öbek boyutu varsayılandan (512KB) daha az ayarlanmışsa RAID, komutları atamayabilir. Bunun nedeni, Ana Bilgisayar'daki harita olmayan parçalılığın da 512KB olmasıdır. Dizinin yığın boyutunu mdadm'ın `--chunk=` parametresi ile değiştirdiyseniz, TRIM/unmap istekleri çekirdek tarafından yoksayılabilir.

Linux VM'nizde TRIM desteğini etkinleştirmenin iki yolu vardır. Her zamanki gibi, önerilen yaklaşım için dağıtımınıza danışın:

- `discard` Montaj seçeneğini `/etc/fstab`kullanın, örneğin:

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- Bazı durumlarda `discard` bu seçeneğin performans etkileri olabilir. Alternatif olarak, komutu `fstrim` komut satırından el ile çalıştırabilir veya düzenli olarak çalıştırmak için crontab'ınıza ekleyebilirsiniz:

    **Ubuntu**

    ```bash
    # sudo apt-get install util-linux
    # sudo fstrim /data
    ```

    **RHEL/CentOS**
    ```bash
    # sudo yum install util-linux
    # sudo fstrim /data
    ```
