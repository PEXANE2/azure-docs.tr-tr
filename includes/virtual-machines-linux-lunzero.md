---
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 09fa612e7e5c681da16bf19e94c626ae14a3b8a1
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77590710"
---
Bir Linux sanal makinesine veri diski eklerken, LUN 0 ' da bir disk yoksa hatalarla karşılaşabilirsiniz. `az vm disk attach -new` komutunu kullanarak el ile bir disk ekliyorsanız ve Azure platformunun uygun LUN 'u belirlemesine izin vermek yerine bir LUN (`--lun`) belirtirseniz, bir diskin zaten var olduğunu veya LUN 0 ' da mevcut olacağını unutmayın. 

`lsscsi`çıktının bir parçacığını gösteren aşağıdaki örneği göz önünde bulundurun:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

LUN 0 ve LUN 1 ' de iki veri diski bulunur (`lsscsi` çıkış ayrıntılarında `[host:channel:target:lun]`ilk sütun). Her iki diskin de VM içinden erişilebilir olması gerekir. LUN 1 ' e ve LUN 2 ' deki ikinci diske eklenecek ilk diski el ile belirttiyseniz, diskleri sanal makinenizin içinden doğru şekilde göremeyebilirsiniz.

> [!NOTE]
> Azure `host` değeri bu örneklerde 5 ' tir, ancak seçtiğiniz depolama türüne göre değişiklik gösterebilir.
> 
> 

Bu disk davranışı bir Azure sorunu değildir, ancak Linux çekirdeğinin SCSI belirtimlerini takip eden yoludur. Linux çekirdeği bağlı cihazlar için SCSI veri yolunu taradığında, sistemin ek cihazları taramaya devam edebilmesi için LUN 0 ' da bir cihazın bulunması gerekir. Şöyle:

* LUN 0 ' da bir diskiniz olduğunu doğrulamak üzere bir veri diski ekledikten sonra `lsscsi` çıkışını gözden geçirin.
* Diskiniz VM 'niz içinde doğru görünmüyorsa, LUN 0 ' da bir disk olduğunu doğrulayın.

