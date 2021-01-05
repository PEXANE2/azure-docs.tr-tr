---
title: Azure BareMetal altyapısının koşullarını öğrenin
description: Azure BareMetal altyapısının koşullarını öğrenin.
ms.topic: conceptual
ms.date: 12/31/2020
ms.openlocfilehash: f11bc4cfdd463623010ed7b6677235344ec7cd62
ms.sourcegitcommit: 42922af070f7edf3639a79b1a60565d90bb801c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97829219"
---
# <a name="know-the-terms-for-baremetal-infrastructure"></a>BareMetal altyapısının koşullarını öğrenin

Bu makalede bazı önemli BareMetal terimleri ele alacağız.

- **Düzeltme**: BareMetal örnek damgaları için iki farklı damga düzeltmesi vardır. Her sürüm mimaride ve Azure sanal makine konaklarına yakınlığa göre farklılık gösterir:
    - **Düzeltme 3** (Rev 3): özgün tasarımdır.
    - **Düzeltme 4** (Rev 4): Azure sanal MAKINESI (VM) konaklarına daha yakından yakınlık sağlayan ve Azure VM 'Leri Ile BareMetal örnek birimleri arasındaki gecikmeyi azaltan yeni bir tasarımdır. 
    - **Düzeltme 4,2** (Rev 4,2): var olan Rev 4 mimarisini kullanan en son yeniden markalı BareMetal altyapısıdır. BareMetal örneklerinizi Azure portal aracılığıyla erişebilir ve yönetebilirsiniz.  

- **Damga**: BareMetal örneklerinin Microsoft iç dağıtım boyutunu tanımlar. Örnek birimlerinin dağıtılması için, işlem, ağ ve depolama raflarından oluşan bir BareMetal örnek damgası, bir veri merkezi konumunda dağıtılmalıdır. Bu tür bir dağıtıma BareMetal örnek damgası veya düzeltme 4,2 ' den denir.

- **Kiracı**: BareMetal örnek damgasında dağıtılan bir müşteri bir *kiracıya yalıtılmış.* Bir kiracı, diğer kiracılardan ağ, depolama ve işlem katmanında yalıtılmıştır. Farklı kiracılara atanan depolama ve işlem birimleri, her birinin veya BareMetal örnek damgası düzeyinde birbirleriyle iletişim kuramazlar. Müşteri, farklı kiracılarda dağıtımları olmasını seçebilir. Daha sonra bile, BareMetal örnek damgası düzeyinde kiracılar arasında iletişim yoktur.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Portal](workloads/sap/baremetal-infrastructure-portal.md)aracılığıyla BareMetal örnek birimlerinin nasıl tanımlanacağına ve etkileşime gireceğini öğrenin.


 