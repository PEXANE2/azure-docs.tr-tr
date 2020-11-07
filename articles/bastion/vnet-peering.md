---
title: VNet eşlemesi ve Azure savunma mimarisi
description: Bu makalede, sanal makinelere bağlanmak için VNet eşlemesi ve Azure savunma 'nin birlikte nasıl kullanılabileceğini öğrenin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 11/05/2020
ms.author: cherylmc
ms.openlocfilehash: ad3cf33dacffc8bcda9376857206784afedf7139
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2020
ms.locfileid: "94362002"
---
# <a name="vnet-peering-and-azure-bastion-preview"></a>VNet eşlemesi ve Azure savunma (Önizleme)

Azure savunma ve VNet eşlemesi birlikte kullanılabilir. VNet eşlemesi yapılandırıldığında, eşlenen her VNet 'te Azure savunma 'yı dağıtmanız gerekmez. Yani, bir sanal ağ (VNet) içinde yapılandırılmış bir Azure savunma ana bilgisayarı varsa, ek bir savunma ana bilgisayarı dağıtmaya gerek kalmadan, eşlenmiş bir VNet 'te dağıtılan VM 'lere bağlanmak için kullanılabilir. VNet eşlemesi hakkında daha fazla bilgi için bkz. [sanal ağ eşlemesi hakkında](../virtual-network/virtual-network-peering-overview.md).

Azure savunma, aşağıdaki eşleme türleriyle birlikte çalışmaktadır:

* **Sanal ağ eşlemesi:** Sanal ağları aynı Azure bölgesi içinde bağlayın.
* **Genel sanal ağ eşlemesi:** Azure bölgeleri arasında sanal ağları bağlama.

## <a name="architecture"></a>Mimari

VNet eşlemesi yapılandırıldığında, Azure savunma, hub ve bağlı bileşen ya da tam ağ topolojilerinde dağıtılabilir. Azure savunma dağıtımı, abonelik/hesap veya sanal makine başına değil, sanal ağ başına değildir.

Sanal ağınızda Azure savunma hizmetini sağladığınızda, RDP/SSH deneyimi aynı VNet 'teki tüm sanal makinelerinizdeki ve eşlenmiş sanal ağlarda kullanılabilir. Bu, savunma dağıtımını tek VNet 'e birleştirebilmeniz ve bir eşlenen VNet 'te dağıtılan VM 'Lere ulaşmaya devam etmek için genel dağıtımı merkezileştirmeniz anlamına gelir.

:::image type="content" source="./media/vnet-peering/design.png" alt-text="Tasarım ve mimari diyagramı":::

Bu şekilde, bir hub ve bağlı bileşen modelinde bir Azure savunma dağıtımının mimarisi gösterilmektedir. Bu diyagramda aşağıdaki yapılandırmaya bakabilirsiniz:

* Savunma ana bilgisayarı merkezi hub sanal ağında dağıtılır.
* Merkezi ağ güvenlik grubu (NSG) dağıtılır.
* Azure VM 'de genel IP gerekli değildir.

**Olanları**

1. Herhangi bir HTML5 tarayıcısı kullanarak Azure portal bağlayın.
1. Bağlanılacak sanal makineyi seçin.
1. Azure savunma, eşlenmiş VNet genelinde sorunsuz bir şekilde algılanır.
1. Tek bir tıklama ile, RDP/SSH oturumu tarayıcıda açılır. RDP ve SSH eşzamanlı oturum sınırları için bkz. [RDP ve ssh oturumları](bastion-faq.md#limits).

   :::image type="content" source="../../includes/media/bastion-vm-rdp/connect-vm.png" alt-text="Bağlan":::

   Azure savunma aracılığıyla bir sanal makineye bağlanma hakkında daha fazla bilgi için bkz.:

   * [VM-RDP ' y e bağlanın](bastion-connect-vm-rdp.md).
   * [VM-SSH ' y e bağlanın](bastion-connect-vm-ssh.md).

## <a name="faq"></a>SSS

[!INCLUDE [FAQ for VNet peering](../../includes/bastion-faq-peering-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Savunma [hakkında SSS](bastion-faq.md)makalesini okuyun.