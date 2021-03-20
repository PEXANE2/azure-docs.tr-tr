---
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 11/09/2018
ms.author: genli
ms.openlocfilehash: 3df4108907a4e1e65a444faf1049163966b7accf
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "67188293"
---
## <a name="scenario"></a>Senaryo
Tek bir NIC içeren bir VM oluşturulur ve bir sanal ağa bağlanır. VM üç farklı *özel* IP adresi ve iki *genel* IP adresi gerektirir. IP adresleri aşağıdaki IP yapılandırmalarına atanır:

* **Ipconfig-1:** *Statik* BIR özel IP adresi ve *STATIK* bir genel IP adresi atar.
* **Ipconfig-2:** *Statik* BIR özel IP adresi ve *STATIK* bir genel IP adresi atar.
* **Ipconfig-3:** *Statik* BIR özel IP adresi atar ve genel IP adresi içermez.
  
    ![Birden çok IP adresi](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

IP yapılandırması NIC oluşturulduğunda NIC ile ilişkilendirilir ve sanal makine oluşturulduğunda NIC sanal makineye eklenir. Senaryo için kullanılan IP adresi türleri illüstrasyon içindir. İhtiyaç duyduğunuz her IP adresini ve atama türünü atayabilirsiniz.

> [!NOTE]
> Bu makaledeki adımlar, tüm IP yapılandırmalarının tek bir NIC 'ye atamasını de sağlar. Ayrıca, çok kiracılı bir VM 'deki herhangi bir NIC 'ye birden çok IP yapılandırması atayabilirsiniz. Birden çok NIC ile VM oluşturma hakkında bilgi edinmek için [birden çok NIC Ile VM oluşturma](../articles/virtual-machines/windows/multiple-nics.md) makalesini okuyun.