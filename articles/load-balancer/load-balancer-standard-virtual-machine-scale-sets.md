---
title: Azure Standart Load Balancer ve sanal makine ölçek kümeleri
titleSuffix: Azure Standard Load Balancer and Virtual Machine Scale Sets
description: Bu öğrenme yoluyla Azure Standart Load Balancer ve sanal makine ölçek kümelerini kullanmaya başlayın.
services: load-balancer
documentationcenter: na
author: irenehua
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2020
ms.author: irenehua
ms.openlocfilehash: f5a8453f84854108facb4da4616a7d362e0fb38c
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86532276"
---
# <a name="azure-load-balancer-with-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümeleri ile Azure Load Balancer

Sanal Makine Ölçek Kümeleri ve yük dengeleyici ile çalışırken aşağıdaki yönergeler göz önünde bulundurulmalıdır:

## <a name="multiple-virtual-machine-scale-sets-cant-use-the-same-load-balancer"></a>Birden çok sanal makine ölçek kümesi aynı yük dengeleyiciyi kullanamaz
## <a name="port-forwarding-and-inbound-nat-rules"></a>Bağlantı noktası Iletme ve gelen NAT kuralları:
  * Ölçek kümesi oluşturulduktan sonra, yük dengeleyicinin bir sistem durumu araştırması tarafından kullanılan bir yük dengeleme kuralı için arka uç bağlantı noktası değiştirilemez. Bağlantı noktasını değiştirmek için Azure sanal makine ölçek kümesini güncelleştirerek sistem durumu araştırmasını kaldırabilir, bağlantı noktasını güncelleştirebilir ve ardından sistem durumu araştırmasını yeniden yapılandırabilirsiniz.
  * Yük dengeleyicinin arka uç havuzunda sanal makine ölçek kümesi kullanılırken, varsayılan gelen NAT kuralları otomatik olarak oluşturulur.
## <a name="inbound-nat-pool"></a>Gelen NAT havuzu:
  * Her sanal makine ölçek kümesinin en az bir gelen NAT havuzu olmalıdır. 
  * Gelen NAT havuzu, gelen NAT kurallarından oluşan bir koleksiyondur. Bir gelen NAT havuzu birden çok sanal makine ölçek kümesini destekleyemez.
  
## <a name="load-balancing-rules"></a>Yük Dengeleme kuralları:
  * Yük dengeleyicinin arka uç havuzunda sanal makine ölçek kümesi kullanılırken, varsayılan Yük Dengeleme kuralı otomatik olarak oluşturulur.
## <a name="outbound-rules"></a>Giden kuralları:
  *  Zaten bir yük dengeleme kuralı tarafından başvurulan bir arka uç havuzu için giden kuralı oluşturmak üzere, gelen yük dengeleme kuralı oluşturulduğunda öncelikle portalda **Hayır** olarak **"örtük giden kuralları oluştur"** seçeneğini işaretlemeniz gerekir.

  :::image type="content" source="./media/vm-scale-sets/load-balancer-and-vm-scale-sets.png" alt-text="Yük Dengeleme kuralı oluşturma" border="true":::

Aşağıdaki yöntemler, mevcut bir Azure yük dengeleyiciye sahip bir sanal makine ölçek kümesi dağıtmak için kullanılabilir.

* [Azure Portal kullanarak bir sanal makine ölçek kümesini mevcut bir Azure Load Balancer yapılandırın](https://docs.microsoft.com/azure/load-balancer/configure-vm-scale-set-portal).
* [Azure PowerShell kullanarak bir sanal makine ölçek kümesini mevcut bir Azure Load Balancer yapılandırın](https://docs.microsoft.com/azure/load-balancer/configure-vm-scale-set-powershell).
* [Azure CLI kullanarak mevcut bir Azure Load Balancer bir sanal makine ölçek kümesi yapılandırın](https://docs.microsoft.com/azure/load-balancer/configure-vm-scale-set-cli).
