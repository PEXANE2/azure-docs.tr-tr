---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 091e4dc0759ce1b227349d5d1b6b08b9eda66ee0
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2020
ms.locfileid: "84237727"
---
* Dönüştürme işlemi VM’nin yeniden başlatılmasını gerektirir, bu nedenle VM'lerinizin geçişini önceden var olan bir bakım penceresi sırasında zamanlayın. 

* Bu dönüştürme geri alınamaz. 

* [Sanal makine katılımcısı](../articles/role-based-access-control/built-in-roles.md#virtual-machine-contributor) rolüne sahip tüm kullanıcıların VM boyutunu (ön dönüştürme yapabilecekleri gibi) değiştiremeyeceği göz önünde bulundurun. Bunun nedeni, yönetilen diskleri olan VM 'Lerin, kullanıcının işletim sistemi disklerinde Microsoft. COMPUTE/diskler/Write iznine sahip olmasını gerektirir.

* Dönüştürmeyi test ettiğinizden emin olun. Üretimde geçişi gerçekleştirmeden önce bir sınama sanal makinesini geçirin.

* Dönüştürme sırasında VM’yi serbest bırakın. VM, dönüştürmeden sonra başlatıldığında yeni bir IP adresi alır. Gerekirse VM’ye [statik bir IP adresi atayabilirsiniz](../articles/virtual-network/public-ip-addresses.md).

* Dönüştürme işlemini desteklemek için gereken en düşük Azure VM Aracısı sürümünü gözden geçirin. Aracı sürümünüzü denetleme ve güncelleştirme hakkında daha fazla bilgi için bkz. [Azure 'DA VM aracıları Için en düşük sürüm desteği](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)