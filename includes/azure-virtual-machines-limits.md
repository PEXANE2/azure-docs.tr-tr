---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: cynthn
ms.openlocfilehash: 2cfd376f595ae70daf9ab468d464dd9c8ff13d74
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80335012"
---
| Kaynak | Sınır |
| --- | --- |
| Bulut hizmeti başına [sanal makine](../articles/virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) sayısı<sup>1</sup> |50 |
| Bulut hizmeti başına giriş uç noktaları<sup>2</sup> |150 |

<sup>1</sup> Azure Resource Manager yerine klasik dağıtım modeli kullanılarak oluşturulan sanal makineler otomatik olarak bir bulut hizmetinde depolanır. Yük Dengeleme ve kullanılabilirlik için bu bulut hizmetine daha fazla sanal makine ekleyebilirsiniz. 

<sup>2</sup> Giriş uç noktaları, sanal makinenin bulut hizmeti dışından bir sanal makineye yönelik iletişimlere izin verir. Aynı bulut hizmeti veya sanal ağ içindeki sanal makineler birbirleriyle otomatik olarak iletişim kurabilir. Daha fazla bilgi için bkz. [sanal makineye uç noktaları ayarlama](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). 
