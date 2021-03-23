---
title: Azure BareMetal altyapısının koşullarını öğrenin
description: Azure BareMetal altyapısının koşullarını öğrenin.
ms.topic: conceptual
ms.date: 1/4/2021
ms.openlocfilehash: b22a6cecb2647df3878cb8fd4ade93d9a7d963fd
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2021
ms.locfileid: "104770895"
---
# <a name="know-the-terms-for-baremetal-infrastructure"></a>BareMetal altyapısının koşullarını öğrenin

Bu makalede bazı önemli BareMetal terimleri ele alacağız.

- **Düzeltme**: düzeltme 3 (Rev 3) olarak bilinen bir özgün damga düzeltmesi ve BareMetal örnek damgaları için iki farklı damga düzeltmesi vardır. Her damga mimaride ve Azure sanal makine konaklarına yakınlığa göre farklılık gösterir:
    - **Düzeltme 4** (Rev 4): Azure sanal MAKINESI (VM) konaklarına daha yakın bir yakınlık sağlayan ve Azure VM 'Leri Ile BareMetal örnek birimleri arasındaki gecikmeyi alçalkan daha yeni bir tasarım. 
    - **Düzeltme 4,2** (Rev 4,2): var olan Rev 4 mimarisini kullanan en son yeniden markalı BareMetal altyapısı. Rev 4, Azure sanal makinesi (VM) konaklarına daha yakın bir yakınlık sağlar. Bu, Azure VM 'Ler ile karemetal örnek birimleri arasında, Rev 4 Damgalarında veya satırlarda dağıtılan ağ gecikmesi açısından önemli geliştirmeler sunar. BareMetal örneklerinizi Azure portal aracılığıyla erişebilir ve yönetebilirsiniz.    

- **Damga**: BareMetal örneklerinin Microsoft iç dağıtım boyutunu tanımlar. Örnek birimlerinin dağıtılması için, işlem, ağ ve depolama raflarından oluşan bir BareMetal örnek damgası, bir veri merkezi konumuna dağıtılmalıdır. Bu tür bir dağıtıma BareMetal örnek damgası veya düzeltme 4,2 ' den denir.

- **Kiracı**: BareMetal örnek damgasında dağıtılan bir müşteri bir *kiracıya yalıtılmış.* Bir kiracı, diğer kiracılardan ağ, depolama ve işlem katmanında yalıtılmıştır. Farklı kiracılara atanan depolama ve işlem birimleri, her birinin veya BareMetal örnek damgası düzeyinde birbirleriyle iletişim kuramazlar. Müşteri, farklı kiracılarda dağıtımları olmasını seçebilir. Daha sonra bile, BareMetal örnek damgası düzeyinde kiracılar arasında iletişim yoktur.

## <a name="next-steps"></a>Sonraki adımlar
[Baremetal altyapısı](concepts-baremetal-infrastructure-overview.md) hakkında daha fazla bilgi edinin veya [Baremetal örnek birimlerinin nasıl tanımlanacağına ve etkileşime gireceğini](connect-baremetal-infrastructure.md)öğrenin. 