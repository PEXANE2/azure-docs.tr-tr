---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 9f7e2760ef8bf06a2e680dce90c323672ca9d491
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "66416073"
---
* Dönüştürme işlemi VM’nin yeniden başlatılmasını gerektirir, bu nedenle VM'lerinizin geçişini önceden var olan bir bakım penceresi sırasında zamanlayın. 

* Bu dönüştürme geri alınamaz. 

* [Sanal Makine Katılımcısı](../articles/role-based-access-control/built-in-roles.md#virtual-machine-contributor) rolüne sahip kullanıcıların VM boyutunu değiştiremeyeceğini unutmayın (dönüşüm öncesi olabilecekleri gibi). Bunun nedeni, yönetilen disklere sahip VM'lerin kullanıcının işletim sistemi disklerinde Microsoft.Compute/disks/write iznine sahip olmasını gerektirmesidir.

* Dönüştürmeyi test ettiğinizden emin olun. Üretimde geçişi gerçekleştirmeden önce bir sınama sanal makinesini geçirin.

* Dönüştürme sırasında VM’yi serbest bırakın. VM, dönüştürmeden sonra başlatıldığında yeni bir IP adresi alır. Gerekirse VM’ye [statik bir IP adresi atayabilirsiniz](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md).

* Dönüştürme işlemini desteklemek için gereken Azure VM aracısının minimum sürümünü gözden geçirin. Aracı sürümünüzü nasıl kontrol edip güncelleştireceğimiz hakkında bilgi için [Azure'daki VM aracıları için Minimum sürüm desteğine](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) bakın