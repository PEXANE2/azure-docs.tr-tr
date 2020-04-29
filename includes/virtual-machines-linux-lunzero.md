---
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 09fa612e7e5c681da16bf19e94c626ae14a3b8a1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77590710"
---
Bir Linux sanal makinesine veri diski eklerken, LUN 0 ' da bir disk yoksa hatalarla karşılaşabilirsiniz. `az vm disk attach -new` Komutunu kullanarak bir diski el ile ekliyorsanız ve Azure platformunun uygun LUN 'u belirlemesine izin vermek`--lun`yerine bir LUN () belirtirseniz, bir diskin zaten var olduğunu veya LUN 0 ' da var olacağını unutmayın. 

Aşağıdaki örnekte çıktının bir parçacığını gösteren bir örnek göz önünde bulundurun `lsscsi`:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

İki veri diski LUN 0 ve LUN 1 ' de ( `lsscsi` çıkış ayrıntılarında `[host:channel:target:lun]`ilk sütun) bulunur. Her iki diskin de VM içinden erişilebilir olması gerekir. LUN 1 ' e ve LUN 2 ' deki ikinci diske eklenecek ilk diski el ile belirttiyseniz, diskleri sanal makinenizin içinden doğru şekilde göremeyebilirsiniz.

> [!NOTE]
> Bu örneklerde `host` Azure değeri 5 ' tir, ancak seçtiğiniz depolama türüne göre değişiklik gösterebilir.
> 
> 

Bu disk davranışı bir Azure sorunu değildir, ancak Linux çekirdeğinin SCSI belirtimlerini takip eden yoludur. Linux çekirdeği bağlı cihazlar için SCSI veri yolunu taradığında, sistemin ek cihazları taramaya devam edebilmesi için LUN 0 ' da bir cihazın bulunması gerekir. Şöyle:

* LUN 0 ' da `lsscsi` bir diskiniz olduğunu doğrulamak için bir veri diski ekledikten sonraki çıktıyı gözden geçirin.
* Diskiniz VM 'niz içinde doğru görünmüyorsa, LUN 0 ' da bir disk olduğunu doğrulayın.

