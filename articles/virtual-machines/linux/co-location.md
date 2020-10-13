---
title: Linux VM 'lerini birlikte bulun
description: Linux için Azure VM kaynaklarının nasıl iyileştireolabileceğini öğrenin.
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: 822d67211982526bdd7a36cb5922f33b448eb7c7
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91973385"
---
# <a name="co-locate-resources-for-improved-latency"></a>Gecikme süresini geliştirmek için kaynakları birlikte konumlandırma

Uygulamanızı Azure 'da dağıttığınızda, örnekleri bölgeler veya kullanılabilirlik alanları arasında yaymak, uygulamanızın genel performansını etkileyebilecek ağ gecikmesi oluşturur. 

## <a name="proximity-placement-groups"></a>Yakınlık yerleştirme grupları

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure CLı kullanarak bir [yakınlık yerleşimi grubuna](proximity-placement-groups.md) VM dağıtın.

[Ağ gecikmesini test](../../virtual-network/virtual-network-test-latency.md?toc=%252fazure%252fvirtual-machines%252flinux%252ftoc.json)etme hakkında bilgi edinin.

[Ağ aktarım hızını en uygun hale getirmeyi](../../virtual-network/virtual-network-optimize-network-bandwidth.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)öğrenin.  

[SAP uygulamalarıyla yakınlık yerleştirme gruplarını nasıl kullanacağınızı](../workloads/sap/sap-proximity-placement-scenarios.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)öğrenin.