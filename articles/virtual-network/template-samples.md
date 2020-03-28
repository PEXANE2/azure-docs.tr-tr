---
title: Sanal ağ için Azure Resource Manager şablonu örnekleri | Microsoft Docs
description: Azure sanal ağları dağıtmanız için kullanılabilir olan farklı Azure Resource Manager şablonları hakkında bilgi edinin.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: 5971b3251812ea4193e71173943ffc0d48207567
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75975361"
---
# <a name="azure-resource-manager-template-samples-for-virtual-network"></a>Sanal ağ için Azure Resource Manager şablonu örnekleri

Aşağıdaki tablo, Azure Resource Manager şablonu örneklerinin bağlantılarını içerir. Azure [portalını](../azure-resource-manager/templates/deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json), Azure [CLI](../azure-resource-manager/templates/deploy-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya Azure [PowerShell](../azure-resource-manager/templates/deploy-powershell.md?toc=%2fazure%2fvirtual-network%2ftoc.json) kullanarak şablonları dağıtabilirsiniz. Kendi şablonlarınızın nasıl yazılacağı hakkında bilgi edinmek için bkz. [İlk şablonunuzu oluşturma](../azure-resource-manager/resource-manager-create-first-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Azure Resource Manager şablonlarının yapısını ve söz dizimini anlama](../azure-resource-manager/templates/template-syntax.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Şablonlarda kullanılacak JSON sözdizimi ve özellikleri için [Microsoft.Network kaynak türlerine](/azure/templates/microsoft.network/allversions)bakın.


| Görev | Açıklama |
|----|----|
|[İki alt ağa sahip bir sanal ağ oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)| İki alt ağa sahip bir sanal ağ oluşturur.|
|[Bir ağ sanal gereci yoluyla trafiği yönlendirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-userdefined-routes-appliance)| Üç alt ağa sahip bir sanal ağ oluşturur. Bir sanal makineyi alt ağların her birine dağıtır. Üçüncü bir alt ağdaki sanal makine aracılığıyla bir alt ağdan diğerine trafiği yönlendirme rotalarını içeren bir rota tablosu oluşturur. Rota tablosunu alt ağların biriyle ilişkilendirir.|
|[Azure Depolama için sanal ağ hizmet uç noktaları oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-2subnets-service-endpoints-storage-integration)|İki alt ağ içeren yeni bir sanal ağ ve her alt ağ içinde bir ağ arabirimi oluşturur. Alt ağlardan biri için Azure Depolama’ya yönelik hizmet uç noktasını etkinleştirir ve o alt ağa yönelik yeni bir depolama hesabının güvenliğini sağlar.|
|[İki sanal ağı bağlama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-to-vnet-peering)| İki sanal ağ ve bunlar arasında bir sanal ağ eşlemesi oluşturur.|
|[Birden çok IP adresi ile sanal makine oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-multiple-ipconfig)| Birden çok IP adresi ile bir Windows veya Linux sanal makinesi oluşturur.|
|[IPv4 + IPv6 çift yığın sanal ağ yapılandırma](https://github.com/Azure/azure-quickstart-templates/tree/master/ipv6-in-vnet)|İki VM ve IPv4 ve IPv6 ortak IP adresine sahip bir Azure Temel Yük Dengeleyicisi ile çift yığınlı (IPv4+IPv6) sanal ağ dağıtır. |
