---
title: Azure BareMetal altyapısının koşullarını öğrenin
description: Azure BareMetal altyapısının koşullarını öğrenin.
ms.topic: conceptual
ms.subservice: workloads
ms.date: 04/06/2021
ms.openlocfilehash: 53a601cc4556198479d8ca5d7495942d4dc2762c
ms.sourcegitcommit: d63f15674f74d908f4017176f8eddf0283f3fac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2021
ms.locfileid: "106580134"
---
# <a name="know-the-terms-for-baremetal-infrastructure"></a>BareMetal altyapısının koşullarını öğrenin

Bu makalede, BareMetal altyapısıyla ilgili bazı önemli koşullar ele alınacaktır.

- **Düzeltme**: düzeltme 3 (Rev 3) olarak bilinen bir özgün damga düzeltmesi ve BareMetal örnek damgaları için iki ek damga düzeltmesi vardır. Her damga mimaride ve Azure sanal makine konaklarına yakınlığa göre farklılık gösterir:
    - **Düzeltme 4** (Rev 4): Azure sanal MAKINESI (VM) konaklarına daha yakından yakınlık sağlayan ve Azure VM 'ler ile SAP HANA örnekleri arasındaki gecikmeyi düşürür. 
    - **Düzeltme 4,2** (Rev 4,2): var olan Rev 4 mimarisini kullanan en son yeniden markalı BareMetal altyapısı. Rev 4, Azure sanal makinesi (VM) konaklarına daha yakın bir yakınlık sağlar. Bu, Rev 4 Damgalarında veya satırlarda dağıtılan Azure VM 'Leri ile BareMetal örnekleri arasındaki ağ gecikmesi açısından önemli geliştirmelere sahiptir. BareMetal örneklerinizi Azure portal aracılığıyla erişebilir ve yönetebilirsiniz.    

- **Damga**: BareMetal örneklerinin Microsoft iç dağıtım boyutunu tanımlar. Örneklerin dağıtılması için işlem, ağ ve depolama raflarından oluşan bir BareMetal örnek damgası, bir veri merkezi konumuna dağıtılmalıdır. Böyle bir dağıtıma BareMetal örnek damgası denir.

- **Kiracı**: bir BareMetal örnek damgası dağıtan bir müşteri, kiracı olarak yalıtılır *.* Bir kiracı, diğer kiracılardan ağ, depolama ve işlem katmanında yalıtılmıştır. Farklı kiracılara atanan depolama ve işlem birimleri, her birinin veya BareMetal örnek damgası düzeyinde birbirleriyle iletişim kuramazlar. Müşteri, farklı kiracılarda dağıtımları olmasını seçebilir. Daha sonra bile, BareMetal örnek damgası düzeyinde kiracılar arasında iletişim yoktur.

## <a name="next-steps"></a>Sonraki adımlar
BareMetal altyapısının önemli terimlerinin tanıtıldığına göre, şu bilgileri öğrenmek isteyebilirsiniz:
- [Baremetal altyapısının](concepts-baremetal-infrastructure-overview.md)daha fazla ayrıntısı.
- [Azure 'Da BareMetal altyapı örneklerine bağlanma](connect-baremetal-infrastructure.md).

