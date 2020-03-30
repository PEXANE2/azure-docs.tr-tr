---
title: LVM'yi Linux çalıştıran sanal bir makinede yapılandırma
description: Azure'da LVM'yi Linux'ta nasıl yapılandırıştırmayı öğrenin.
author: gbowerman
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 09/27/2018
ms.author: guybo
ms.subservice: disks
ms.openlocfilehash: 7f560a1e6266b5f2452bf9442d2d4c983de1236e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066804"
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>AZURE'da BIR Linux VM üzerinde LVM yapılandırma
Bu belgede, Azure sanal makinenizde Mantıksal Birim Yöneticisi'nin (LVM) nasıl yapılandırılabildiğini açıklanır. LVM, Azure VM'lerde işletim sistemi diskinde veya veri disklerinde kullanılabilir, ancak varsayılan olarak çoğu bulut görüntüsünün çoğu, OS diskinde LVM yapılandırılmamıştır. Aşağıdaki adımlar, veri diskleriniz için LVM yapılandırılamaya odaklanacaktır.

## <a name="linear-vs-striped-logical-volumes"></a>Doğrusal ve çizgili mantıksal birimler
LVM, bir dizi fiziksel diski tek bir depolama biriminde birleştirmek için kullanılabilir. Varsayılan olarak LVM genellikle doğrusal mantıksal birimler oluşturur, bu da fiziksel depolamanın birlikte kısıtlandığı anlamına gelir. Bu durumda okuma/yazma işlemleri genellikle yalnızca tek bir diske gönderilir. Buna karşılık, okuma ve yazmaların ses grubunda bulunan birden çok diske dağıtıldığı çizgili mantıksal birimler de oluşturabiliriz (RAID0'a benzer). Performans nedenleriyle, mantıksal birimlerinizi şeritlemek isteyebilirsiniz, böylece okuma ve yazma tüm bağlı veri disklerinizi kullanır.

Bu belge, birden çok veri diskini tek bir birim grubunda nasıl birleştireceğini açıklar ve sonra çizgili mantıksal birim oluşturur. Aşağıdaki adımlar, çoğu dağıtımla çalışmak üzere genelleştirilmiştir. Çoğu durumda, Azure'da LVM'yi yönetmek için yardımcı programlar ve iş akışları diğer ortamlardan temelde farklı değildir. Her zamanki gibi, aynı zamanda belgeler ve belirli dağıtım ile LVM kullanmak için en iyi uygulamalar için Linux satıcınıza danışın.

## <a name="attaching-data-disks"></a>Veri diskleri ekleme
Bir genellikle LVM kullanırken iki veya daha fazla boş veri diskleri ile başlamak isteyecektir. IO ihtiyaçlarınıza bağlı olarak, Standart Depolama alanımızda depolanan diskleri disk başına 500'e kadar IO/ps veya disk başına 5000 IO/ps'ye kadar Premium depolama alanımızla eklemeyi seçebilirsiniz. Bu makalede, bir Linux sanal makineye veri diskleri sağlama ve ekleme hakkında ayrıntılı bilgi verilmeyecektir. Azure'daki bir Linux sanal makinesine boş bir veri diskinin nasıl eklenecek lerine ilişkin ayrıntılı talimatlar [için](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Microsoft Azure makalesine bakın.

## <a name="install-the-lvm-utilities"></a>LVM yardımcı hizmetlerini yükleyin
* **Ubuntu**

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* **RHEL, CentOS & Oracle Linux**

    ```bash   
    sudo yum install lvm2
    ```

* **SLES 12 ve openSUSE**

    ```bash   
    sudo zypper install lvm2
    ```

* **SLES 11**

    ```bash   
    sudo zypper install lvm2
    ```

    SLES11'de, "etkinleştirmek" için de ayarlamanız `/etc/sysconfig/lvm` ve ayarlamanız `LVM_ACTIVATED_ON_DISCOVERED` gerekir:

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a>LVM'yi yapılandırma
Bu kılavuzda, "ve `/dev/sdc` `/dev/sdd` `/dev/sde`. " olarak adlandırılacağız üç veri diski iliştirdiğinizi varsayacağız. Bu yollar VM'nizdeki disk yolu adlarıyla eşleşmeyebilir. Kullanılabilir disklerinizi`sudo fdisk -l`listelemek için ' ' veya benzer komutu çalıştırabilirsiniz.

1. Fiziksel hacimleri hazırlayın:

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. Bir birim grubu oluşturun. Bu örnekte biz hacim `data-vg01`grubu çağırıyoruz:

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. Mantıksal birim(ler) oluşturun. Aşağıdaki komut, tüm birim grubunu `data-lv01` kapamak için çağrılan tek bir mantıksal birim oluşturur, ancak birim grubunda birden çok mantıksal birim oluşturmanın da mümkün olduğunu unutmayın.

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. Mantıksal birimi biçimlendirme

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > Ext4 yerine `-t ext3` SLES11 kullanımı ile. SLES11 yalnızca ext4 dosya sistemlerine salt okunur erişimi destekler.

## <a name="add-the-new-file-system-to-etcfstab"></a>Yeni dosya sistemini /etc/fstab'a ekleme
> [!IMPORTANT]
> `/etc/fstab` dosyasının hatalı düzenlenmesi sonucunda önyüklemesi yapılamayan bir sistem ortaya çıkar. Emin değilseniz, bu dosyayı doğru düzenleme hakkındaki bilgiler için dağıtımın belgelerine bakın. Ayrıca, düzenlemeden önce dosyanın `/etc/fstab` bir yedeklemesinin oluşturulması önerilir.

1. Örneğin, yeni dosya sisteminiz için istediğiniz montaj noktasını oluşturun:

    ```bash  
    sudo mkdir /data
    ```

2. Mantıksal birim yolunu bulma

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. Metin `/etc/fstab` düzenleyicisinde açın ve örneğin yeni dosya sistemi için bir giriş ekleyin:

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    Sonra, kaydet `/etc/fstab`ve kapatın.

4. Girişin `/etc/fstab` doğru olduğunu test edin:

    ```bash    
    sudo mount -a
    ```

    Bu komut bir hata iletisi ile `/etc/fstab` sonuçlanırsa dosyadaki sözdizimini denetleyin.
   
    Dosya sisteminin `mount` monte edilmesini sağlamak için komutu sonraki çalıştırın:

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. (İsteğe bağlı) Failsafe önyükleme parametreleri`/etc/fstab`
   
    Birçok dağıtım, dosyaya eklenebilecek `nobootwait` veya `nofail` monte `/etc/fstab` parametrelerini içerir. Bu parametreler, belirli bir dosya sistemi monte ederken hatalara izin verir ve RAID dosya sistemini düzgün bir şekilde monte edemese bile Linux sisteminin önyüklemeye devam etmesine olanak sağlar. Bu parametreler hakkında daha fazla bilgi için dağıtımınızın belgelerine bakın.
   
    Örnek (Ubuntu):

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a>TRIM/UNMAP desteği
Bazı Linux çekirdekleri, diskteki kullanılmayan blokları atmak için TRIM/UNMAP işlemlerini destekler. Bu işlemler, azure'a silinen sayfaların artık geçerli olmadığını ve atılabilir olduğunu bildirmek için öncelikle standart depolama alanında yararlıdır. Sayfaları atmak, büyük dosyalar oluşturup silerseniz maliyet tasarrufu sağlayabilir.

Linux VM'nizde TRIM desteğini etkinleştirmenin iki yolu vardır. Her zamanki gibi, önerilen yaklaşım için dağıtımınıza danışın:

- `discard` Montaj seçeneğini `/etc/fstab`kullanın, örneğin:

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- Bazı durumlarda `discard` bu seçeneğin performans etkileri olabilir. Alternatif olarak, komutu `fstrim` komut satırından el ile çalıştırabilir veya düzenli olarak çalıştırmak için crontab'ınıza ekleyebilirsiniz:

    **Ubuntu**

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    **RHEL, CentOS & Oracle Linux**

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```
