---
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 11/09/2018
ms.author: genli
ms.openlocfilehash: 3df4108907a4e1e65a444faf1049163966b7accf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188293"
---
## <a name="scenario"></a>Senaryo
Tek bir NIC'li bir VM oluşturulur ve sanal ağa bağlanır. VM üç farklı *özel* IP adresi ve iki *genel* IP adresi gerektirir. IP adresleri aşağıdaki IP yapılandırmalarına atanır:

* **IPConfig-1:** *Statik* özel IP adresi ve *statik* genel IP adresi atar.
* **IPConfig-2:** *Statik* özel IP adresi ve *statik* genel IP adresi atar.
* **IPConfig-3:** *Statik* bir özel IP adresi atar ve genel IP adresi atamaz.
  
    ![Birden çok IP adresi](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

NIC oluşturulduğunda IP yapılandırmaları NIC ile ilişkilidir ve VM oluşturulduğunda NIC VM'ye bağlanır. Senaryo için kullanılan IP adreslerinin türleri illüstrasyon içindir. İstediğiniz IP adresini ve atama türlerini atayabilirsiniz.

> [!NOTE]
> Bu makaledeki adımlar tüm IP yapılandırmalarını tek bir NIC'e atasa da, birden çok NIC VM'de herhangi bir NIC'e birden çok IP yapılandırması atayabilirsiniz. Birden çok NIC'li bir VM nasıl oluşturulacak öğrenmek için, [birden çok NIC'li VM](../articles/virtual-machines/windows/multiple-nics.md) makalesini okuyun.