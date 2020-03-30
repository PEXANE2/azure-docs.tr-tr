---
title: Azure CLI'yi kullanarak Linux VM'ye veri diski ekleme
description: Azure CLI ile Linux VM'nize kalıcı bir veri diski eklemeyi öğrenin
author: roygara
manager: twooley
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 06/13/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: a80a1fe21ba0b40aebf9e426e3d49f499c2d2a21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250419"
---
# <a name="add-a-disk-to-a-linux-vm"></a>Linux VM'ye disk ekleme
Bu makalede, VM'niz bakım veya yeniden boyutlandırma nedeniyle yeniden sağlanmış olsa bile, verilerinizi koruyabilmeniz için VM'nize kalıcı bir disk nasıl eklenebilir.


## <a name="attach-a-new-disk-to-a-vm"></a>VM'ye yeni bir disk ekleme

VM'nize yeni, boş bir veri diski eklemek istiyorsanız, parametreli [az vm disk ekleme](/cli/azure/vm/disk?view=azure-cli-latest) komutunu `--new` kullanın. VM'niz Kullanılabilirlik Bölgesindeyse, disk otomatik olarak VM ile aynı bölgede oluşturulur. Daha fazla bilgi için, [Kullanılabilirlik Bölgelerine Genel Bakış](../../availability-zones/az-overview.md)bölümüne bakın. Aşağıdaki örnek, *myDataDisk* adında 50 Gb boyutunda bir disk oluşturur:

```azurecli
az vm disk attach \
   -g myResourceGroup \
   --vm-name myVM \
   --name myDataDisk \
   --new \
   --size-gb 50
```

## <a name="attach-an-existing-disk"></a>Var olan bir diski ekleme

Varolan bir disk eklemek için disk kimliğini bulun ve kimliği [az vm disk ekleme](/cli/azure/vm/disk?view=azure-cli-latest) komutuna geçirin. *MyResourceGroup'ta* *myDataDisk* adlı bir disk için aşağıdaki örnek sorgular, daha sonra *myVM*adlı VM'ye iliştirilir:

```azurecli
diskId=$(az disk show -g myResourceGroup -n myDataDisk --query 'id' -o tsv)

az vm disk attach -g myResourceGroup --vm-name myVM --name $diskId
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Yeni diski takmak için Linux VM'ye bağlanın

Linux VM'nizin kullanabilmesi için yeni diskinizi bölmek, biçimlendirmek ve takmak için SSH'i VM'nize dönüştürün. Daha fazla bilgi için bkz. [Azure’da Linux ile SSH kullanma](mac-create-ssh-keys.md). Aşağıdaki örnek, *azureuser*kullanıcı adı ile *mypublicdns.westus.cloudapp.azure.com* ortak DNS girişi ile bir VM bağlanır:

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

VM'nize bağlandıktan sonra bir disk takmaya hazırsınız. İlk olarak, diski kullanarak `dmesg` bulun (yeni diskinizi keşfetmek için kullandığınız yöntem değişebilir). Aşağıdaki örnek, *SCSI* disklerine filtre sağlamak için dmesg kullanır:

```bash
dmesg | grep SCSI
```

Çıktı aşağıdaki örneğe benzer:

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

> [!NOTE]
> Fdisk'in veya dağıtımIçin kullanılabilen ayrılmış en son sürümlerini kullanmanız önerilir.

Burada, *SDC* istediğimiz disktir. Diski `parted`, disk boyutu 2 tebibayt (TiB) veya daha büyükse, 2TiB'nin altındaysa GPT bölümleme kullanmanız gerekir, o zaman MBR veya GPT bölümleme kullanabilirsiniz. MBR bölümleme kullanıyorsanız, kullanabilirsiniz. `fdisk` Bölüm 1'de birincil disk yapın ve diğer varsayılanları kabul edin. Aşağıdaki örnek `fdisk` */dev/sdc*üzerinde süreci başlatır:

```bash
sudo fdisk /dev/sdc
```

Yeni bölüm eklemek için `n` komutunu kullanın. Bu örnekte, birincil `p` bölüm için de seçim ve varsayılan değerlerin geri kalanını kabul. Çıkış aşağıdaki örneğe benzeyecektir:

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Bölüm tablosunu yazarak `p` yazdırın ve ardından tabloyu diske ve çıkışa yazmak için kullanın. `w` Çıktı aşağıdaki örneğe benzer olmalıdır:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```
Çekirdeği güncelleştirmek için aşağıdaki komutu kullanın:
```
partprobe 
```

Şimdi, `mkfs` komutu ile bölüme bir dosya sistemi yazın. Dosya sistem türünü ve aygıt adını belirtin. Aşağıdaki örnek, */dev/sdc1* bölümü üzerinde önceki adımlarda oluşturulan bir *ext4* dosya sistemi oluşturur:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Çıktı aşağıdaki örneğe benzer:

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Şimdi, dosya sistemini kullanarak `mkdir`monte etmek için bir dizin oluşturun. Aşağıdaki örnek */datadrive'da*bir dizin oluşturur:

```bash
sudo mkdir /datadrive
```

Daha `mount` sonra dosya sistemini takmak için kullanın. Aşağıdaki örnek / *dev/sdc1* bölümü */ datadrive* montaj noktasına bağlar:

```bash
sudo mount /dev/sdc1 /datadrive
```

Sürücünün yeniden başlatıldıktan sonra otomatik olarak yeniden monte edilmesini sağlamak için */etc/fstab* dosyasına eklenmelidir. UUID 'nin (Evrensel Olarak Benzersiz Tanımlayıcı) sürücüye başvurmak için */etc/fstab'da* kullanılması da tavsiye edilir (örneğin, */dev/sdc1).* Önyükleme sırasında işletim sistemi bir disk hatası algılarsa, UUID'nin kullanılması belirlenen konuma yanlış diskin bağlanmasını önler. Bundan sonra kalan veri diskleri aynı cihaz kimliklerine atanabilir. Yeni sürücünün UUID'sini bulmak için `blkid` yardımcı programını kullanın:

```bash
sudo blkid
```

Çıktı aşağıdaki örneğe benzer:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

> [!NOTE]
> **/etc/fstab** dosyasının yanlış düzenlenmesi başlatılamaz bir sisteme neden olabilir. Emin değilseniz, bu dosyayı doğru düzenleme hakkındaki bilgiler için dağıtımın belgelerine bakın. Düzenlemeden önce /etc/fstab dosyasının bir yedeklemesinin oluşturulması da önerilir.

Ardından, bir metin düzenleyicisi */etc/fstab* dosyasını aşağıdaki gibi açın:

```bash
sudo vi /etc/fstab
```

Bu örnekte, önceki adımlarda oluşturulan */dev/sdc1* aygıtı nın UUID değerini ve */datadrive'ın*montaj noktasını kullanın. */etc/fstab* dosyasının sonuna aşağıdaki satırı ekleyin:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> Daha sonra fstab düzenleme olmadan bir veri diski kaldırma VM önyükleme başarısız neden olabilir. Çoğu dağıtım *nofail* ve/veya *nobootwait* fstab seçeneklerini sağlar. Bu seçenekler, disk önyükleme zamanında monte başarısız olsa bile bir sistemin önyükleme sağlar. Bu parametreler hakkında daha fazla bilgi için dağıtımınızın belgelerine başvurun.
>
> *Nofail* seçeneği, dosya sistemi bozuk olsa veya önyükleme zamanında disk yok olsa bile VM'nin başlatılmasını sağlar. Bu seçenek olmadan, [FSTAB hataları nedeniyle Can SSH'den Linux VM'ye](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/) açıklanan davranışlarla karşılaşabilirsiniz
>
> Azure VM Seri Konsolu, fstab'ı değiştirmenin önyükleme hatasına yol açması durumunda VM'nize konsol erişimi için kullanılabilir. [Seri Konsol belgelerinde](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux)daha fazla ayrıntı mevcuttur.

### <a name="trimunmap-support-for-linux-in-azure"></a>Azure'da Linux için TRIM/UNMAP desteği
Bazı Linux çekirdekleri, diskteki kullanılmayan blokları atmak için TRIM/UNMAP işlemlerini destekler. Bu özellik, azure'a silinen sayfaların artık geçerli olmadığını ve atılabildiği konusunda azure'a bilgi vermek için öncelikle standart depolama alanında kullanışlıdır ve büyük dosyalar oluşturup silerseniz paradan tasarruf edebilirsiniz.

Linux VM'nizde TRIM desteğini etkinleştirmenin iki yolu vardır. Her zamanki gibi, önerilen yaklaşım için dağıtımınıza danışın:

* `discard` */etc/fstab'daki*montaj seçeneğini kullanın, örneğin:

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```
* Bazı durumlarda, `discard` seçeneğin performans etkileri olabilir. Alternatif olarak, komutu `fstrim` komut satırından el ile çalıştırabilir veya düzenli olarak çalıştırmak için crontab'ınıza ekleyebilirsiniz:

    **Ubuntu**

    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```

    **RHEL/CentOS**

    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a>Sorun giderme

[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Linux VM'nizin doğru şekilde yapılandırıldığından emin olmak [için, Linux makine performans önerilerinizi optimize edin.](optimization.md)
* Ek diskler ekleyerek depolama kapasitenizi genişletin ve ek performans için [RAID'i yapılandırın.](configure-raid.md)
