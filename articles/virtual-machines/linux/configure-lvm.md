---
title: Linux çalıştıran bir sanal makinede LVM 'yi yapılandırma
description: Azure 'da Linux üzerinde LVM yapılandırma hakkında bilgi edinin.
author: gbowerman
ms.service: virtual-machines-linux
ms.topic: how-to
ms.date: 09/27/2018
ms.author: guybo
ms.subservice: disks
ms.openlocfilehash: 9a3498939ddf57e2520a140ff693a30de913fae0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84658286"
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>Azure 'da bir Linux VM 'de LVM 'yi yapılandırma
Bu belgede, Azure sanal makinenizde mantıksal birim Yöneticisi 'nin (LVM) nasıl yapılandırılacağı açıklanmaktadır. LVM, Azure VM 'lerde işletim sistemi diskinde veya veri disklerinde kullanılabilir, ancak varsayılan olarak çoğu bulut görüntüsü, işletim sistemi diskinde yapılandırılmış LVM 'ye sahip olmayacaktır. Aşağıdaki adımlar, veri diskleriniz için LVM yapılandırmaya odaklanacaktır.

## <a name="linear-vs-striped-logical-volumes"></a>Doğrusal ve şeritli mantıksal birimler
LVM, bir dizi fiziksel diski tek bir depolama biriminde birleştirmek için kullanılabilir. Varsayılan olarak LVM genellikle doğrusal mantıksal birimler oluşturur, bu da fiziksel depolamanın birlikte bitiştirildiği anlamına gelir. Bu durumda, okuma/yazma işlemleri genellikle yalnızca tek bir diske gönderilir. Buna karşılık, okuma ve yazma işlemlerinin birim grubundaki birden çok diske dağıtıldığı (RAID0 benzer) dizili mantıksal birimler de oluşturabilirsiniz. Performans nedenleriyle, büyük olasılıkla mantıksal birimlerinizi, okuma ve yazma işlemlerinin tüm ekli veri disklerinizi kullanmasını sağlayacak şekilde eklemek isteyeceksiniz.

Bu belge, çeşitli veri disklerinin tek bir birim grubunda nasıl birleştirileceğini betimler ve sonra şeritli bir mantıksal birim oluşturur. Aşağıdaki adımlar, çoğu dağıtımda çalışacak şekilde genelleştirilir. Çoğu durumda, Azure 'da LVM 'yi yönetmeye yönelik yardımcı programlar ve iş akışları diğer ortamlarından temelde farklı değildir. Her zamanki gibi, belirli bir dağıtıma sahip LVM kullanmaya yönelik belgeler ve en iyi uygulamalar için de Linux satıcınıza başvurun.

## <a name="attaching-data-disks"></a>Veri diskleri iliştirme
Bir tane, LVM kullanırken genellikle iki veya daha fazla boş veri diski ile başlamak ister. G/ç gereksinimlerinize göre disk başına en fazla 500 GÇ/PS veya disk başına en fazla 5000 GÇ/PS olan Premium depolama alanı ile standart depomızda depolanan diskleri eklemeyi tercih edebilirsiniz. Bu makale, bir Linux sanal makinesine veri diskleri sağlama ve iliştirme konusunda ayrıntıya geçmeyecektir. Azure 'da bir Linux sanal makinesine boş veri diski iliştirme hakkında ayrıntılı yönergeler için Microsoft Azure makalesine [disk iliştirme](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bölümüne bakın.

## <a name="install-the-lvm-utilities"></a>LVM yardımcı programlarını yükler
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

    SLES11 'de, `/etc/sysconfig/lvm` `LVM_ACTIVATED_ON_DISCOVERED` "etkinleştir" olarak da düzenlemeniz ve ayarlamanız gerekir:

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a>LVM'yi yapılandırma
Bu kılavuzda, ve olarak adlandırdığımız üç veri diskine sahip olduğunuz varsayılır `/dev/sdc` `/dev/sdd` `/dev/sde` . Bu yollar, sanal makinenizin disk yolu adlarıyla eşleşmeyebilir. `sudo fdisk -l`Kullanılabilir disklerinizi listelemek için ' ' veya benzer komutu çalıştırabilirsiniz.

1. Fiziksel birimleri hazırlama:

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. Birim grubu oluşturun. Bu örnekte birim grubunu arıyoruz `data-vg01` :

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. Mantıksal birim (ler) i oluşturun. Aşağıdaki komut, `data-lv01` tüm birim grubunu yaymak için adlı tek bir mantıksal birim oluşturacağız, ancak birim grubunda birden çok mantıksal birim oluşturmak için de uygun olduğunu unutmayın.

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. Mantıksal birimi biçimlendirme

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > Ext4 yerine SLES11 kullanımı vardır `-t ext3` . SLES11 yalnızca ext4 filesystems için salt okuma erişimini destekler.

## <a name="add-the-new-file-system-to-etcfstab"></a>Yeni dosya sistemini/etc/fstab 'e ekleme
> [!IMPORTANT]
> `/etc/fstab` dosyasının hatalı düzenlenmesi sonucunda önyüklemesi yapılamayan bir sistem ortaya çıkar. Emin değilseniz, bu dosyayı doğru düzenleme hakkındaki bilgiler için dağıtımın belgelerine bakın. Ayrıca, `/etc/fstab` düzenlemeden önce dosyanın bir yedeğinin oluşturulması önerilir.

1. Yeni dosya sisteminiz için istenen bağlama noktasını oluşturun, örneğin:

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

3. `/etc/fstab`Bir metin düzenleyicisinde açın ve yeni dosya sistemi için bir giriş ekleyin, örneğin:

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    Sonra kaydedin ve kapatın `/etc/fstab` .

4. Girişin doğru olduğunu test edin `/etc/fstab` :

    ```bash    
    sudo mount -a
    ```

    Bu komut bir hata iletisiyle sonuçlanırsa dosyadaki sözdizimini denetleyin `/etc/fstab` .
   
    Sonra, `mount` dosya sisteminin takılı olduğundan emin olmak için komutunu çalıştırın:

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. Seçim ' De Failsafe önyükleme parametreleri`/etc/fstab`
   
    Birçok dağıtım, `nobootwait` `nofail` dosyaya eklenebilen veya bağlama parametrelerini içerir `/etc/fstab` . Bu parametreler, belirli bir dosya sistemini bağladığınızda ve Linux sisteminin, RAID dosya sistemini düzgün bir şekilde bağlamasa bile önyüklemeye devam etmesine izin veren hatalara izin verir. Bu parametrelerle ilgili daha fazla bilgi için, dağıtım belgelerine bakın.
   
    Örnek (Ubuntu):

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a>KESME/eşlemeyi kaldır desteği
Bazı Linux çekirdekler, diskteki kullanılmayan blokları atmak için kesme/eşlemeyi Kaldır işlemlerini destekler. Bu işlemler, Azure 'un silinen sayfaların artık geçerli olmadığını ve yoksayılabilir olduğunu bildirmek için öncelikle standart depolamada yararlı olur. Sayfaların atılması, büyük dosyalar oluşturup bunları silerseniz maliyeti kaydedebilir.

Linux sanal makinenizde KıRPMA desteğini etkinleştirmenin iki yolu vardır. Her zamanki gibi, önerilen yaklaşım için dağıtıma başvurun:

- `discard`İçindeki bağlama seçeneğini kullanın `/etc/fstab` , örneğin:

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- Bazı durumlarda, `discard` seçeneğinde performans olumsuz etkileri olabilir. Alternatif olarak, komut `fstrim` satırından komutu el ile çalıştırabilir veya bunları düzenli olarak çalıştırmak için crontab 'ize ekleyebilirsiniz:

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
