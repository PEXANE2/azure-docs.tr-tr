---
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 09fa612e7e5c681da16bf19e94c626ae14a3b8a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77590710"
---
Linux VM'ye veri diskleri eklerken, LUN 0'da bir disk yoksa hatalarla karşılaşabilirsiniz. `az vm disk attach -new` Komutu kullanarak el ile bir disk ekliyorsanız ve`--lun`Azure platformunun uygun LUN'u belirlemesine izin vermek yerine bir LUN ( ) belirtirseniz, lun 0'da bir diskin zaten var olmasına dikkat edin. 

Çıktının bir snippet gösteren aşağıdaki `lsscsi`örneği göz önünde bulundurun:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

İki veri diski LUN 0 ve LUN 1'de `lsscsi` (çıktı ayrıntılarındaki `[host:channel:target:lun]`ilk sütun) bulunur. Her iki diske de VM içinden erişilebilir olmalıdır. LUN 1'e ve ikinci diski LUN 2'ye eklenecek ilk diski el ile belirtmiş olsaydınız, diskleri VM'nizin içinden doğru şekilde göremeyebilirsiniz.

> [!NOTE]
> Azure `host` değeri bu örneklerde 5'tir, ancak seçtiğiniz depolama türüne bağlı olarak bu değişiklik gösterebilir.
> 
> 

Bu disk davranışı bir Azure sorunu değil, Linux çekirdeğinin SCSI belirtimlerini izlemesidir. Linux çekirdeği bağlı aygıtlar için SCSI veri toplarını taradığında, sistemin ek aygıtları taramaya devam edebilmesi için LUN 0'da bir aygıt bulunması gerekir. Bu nedenle:

* LUN 0'da bir diskiniz olduğunu doğrulamak için bir veri diski `lsscsi` ekledikten sonra çıktısını gözden geçirin.
* Diskiniz VM'nizde doğru görünmüyorsa, LUN 0'da bir diskin var olduğunu doğrulayın.

