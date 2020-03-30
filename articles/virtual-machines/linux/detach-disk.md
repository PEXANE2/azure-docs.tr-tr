---
title: Bir veri diskini Linux VM'den ayırma - Azure
description: Azure CLI veya Azure portalını kullanarak bir veri diskini Azure'daki sanal bir makineden ayırmayı öğrenin.
author: roygara
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: f8a0790169b17ad7755386f9bdd4f9372efc83e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74036380"
---
# <a name="how-to-detach-a-data-disk-from-a-linux-virtual-machine"></a>Bir Linux sanal makinesindeki veri diskini ayırma

Sanal makineye bağlı bir veri diskine ihtiyacınız olmadığında bunu kolayca ayırabilirsiniz. Bu, diski sanal makineden kaldırır, ancak depolama dan kaldırmaz. Bu makalede, bir Ubuntu LTS 16.04 dağılımı ile çalışıyoruz. Farklı bir dağıtım kullanıyorsanız, diski sökme yönergeleri farklı olabilir.

> [!WARNING]
> Bir diski ayırın, otomatik olarak silinmez. Premium depolama alanına abone olduysanız, disk için depolama ücreti ödemeye devam erecektir. Daha fazla bilgi [için, Premium Depolama'yı kullanırken Fiyatlandırma ve Faturalandırma'ya](https://azure.microsoft.com/pricing/details/storage/page-blobs/)bakın.

Disk üzerinde var olan verileri yeniden kullanmak isterseniz bu verileri aynı sanal makineye veya başka birine yeniden ekleyebilirsiniz.  


## <a name="connect-to-the-vm-to-unmount-the-disk"></a>Diski sökmek için VM'ye bağlanın

CLI veya portalı kullanarak diski ayırmadan önce diski sökmeniz ve fstab dosyanızdan varsa için başvuruları kaldırmanız gerekir.

VM’ye bağlanın. Bu örnekte, VM'nin ortak IP adresi *azureuser*kullanıcı adı ile *10.0.1.4'tür:* 

```bash
ssh azureuser@10.0.1.4
```

İlk olarak, ayırmak istediğiniz veri diskini bulun. Aşağıdaki örnek, SCSI disklerine filtre sağlamak için dmesg kullanır:

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

Burada, *sdc* ayırmak istediğimiz disktir. Ayrıca diskin UUID kapmak gerekir.

```bash
sudo -i blkid
```

Çıktı aşağıdaki örneğe benzer:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```


Diske yapılan başvuruları kaldırmak için */etc/fstab* dosyasını edin. 

> [!NOTE]
> **/etc/fstab** dosyasının yanlış düzenlenmesi başlatılamaz bir sisteme neden olabilir. Emin değilseniz, bu dosyayı doğru düzenleme hakkındaki bilgiler için dağıtımın belgelerine bakın. Düzenlemeden önce /etc/fstab dosyasının bir yedeklemesinin oluşturulması da önerilir.

*/etc/fstab* dosyasını bir metin düzenleyicisinde aşağıdaki gibi açın:

```bash
sudo vi /etc/fstab
```

Bu örnekte, */etc/fstab* dosyasından aşağıdaki satırın silinmesi gerekir:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

Diski sökmek için kullanın. `umount` Aşağıdaki örnek , */datadrive* montaj noktasından */dev/sdc1* tümcesini ayırır:

```bash
sudo umount /dev/sdc1 /datadrive
```


## <a name="detach-a-data-disk-using-azure-cli"></a>Azure CLI'yi kullanarak veri diskini ayırma 

Bu örnek, *myResourceGroup'taki* *MyVM* adlı MyDataDisk diskini VM'den ayırır. *myDataDisk*

```azurecli
az vm disk detach \
    -g myResourceGroup \
    --vm-name myVm \
    -n myDataDisk
```

Disk depolamada kalır, ancak artık sanal bir makineye bağlı değildir.


## <a name="detach-a-data-disk-using-the-portal"></a>Portalı kullanarak veri diski çıkarma

1. Sol menüde **Sanal Makineler'i**seçin.
2. Ayırmak istediğiniz veri diskine sahip sanal makineyi seçin ve VM'yi bulmak için **Durdur'u** tıklatın.
3. Sanal makine bölmesinde **Diskler'i**seçin.
4. **Diskler** bölmesinin üst **kısmında, Edit'i**seçin.
5. **Diskler** bölmesinde, ayırmak istediğiniz veri diskinin en sağında, ayır düğmesi ![görüntü](./media/detach-disk/detach.png) ayırma düğmesini tıklatın.
5. Disk kaldırıldıktan sonra bölmenin üst kısmında Kaydet'i tıklatın.
6. Sanal makine bölmesinde **Genel Bakış'ı** tıklatın ve ardından VM'yi yeniden başlatmak için bölmenin üst kısmındaki **Başlat** düğmesini tıklatın.

Disk depolamada kalır, ancak artık sanal bir makineye bağlı değildir.



## <a name="next-steps"></a>Sonraki adımlar
Veri diskini yeniden kullanmak istiyorsanız, [başka bir VM'ye ekleyebilirsiniz.](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

