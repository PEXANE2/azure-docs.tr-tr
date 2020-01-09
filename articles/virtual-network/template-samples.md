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
ms.openlocfilehash: 675c222bfbacf8207c7b3f906452251bedd64a66
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75350179"
---
# <a name="azure-resource-manager-template-samples-for-virtual-network"></a>Sanal ağ için Azure Resource Manager şablonu örnekleri

Aşağıdaki tablo, Azure Resource Manager şablonu örneklerinin bağlantılarını içerir. Azure [portalını](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json), Azure [CLI](../azure-resource-manager/resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya Azure [PowerShell](../azure-resource-manager/resource-group-template-deploy.md?toc=%2fazure%2fvirtual-network%2ftoc.json) kullanarak şablonları dağıtabilirsiniz. Kendi şablonlarınızın nasıl yazılacağı hakkında bilgi edinmek için bkz. [İlk şablonunuzu oluşturma](../azure-resource-manager/resource-manager-create-first-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Azure Resource Manager şablonlarının yapısını ve söz dizimini anlama](../azure-resource-manager/templates/template-syntax.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Şablonlarda kullanılacak JSON sözdizimi ve özellikler için bkz. [Microsoft. Network kaynak türleri](/azure/templates/microsoft.network/allversions).


| Görev | Açıklama |
|----|----|
|[İki alt ağa sahip bir sanal ağ oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)| İki alt ağa sahip bir sanal ağ oluşturur.|
|[Bir ağ sanal gereci yoluyla trafiği yönlendirme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-userdefined-routes-appliance)| Üç alt ağa sahip bir sanal ağ oluşturur. Bir sanal makineyi alt ağların her birine dağıtır. Üçüncü bir alt ağdaki sanal makine aracılığıyla bir alt ağdan diğerine trafiği yönlendirme rotalarını içeren bir rota tablosu oluşturur. Rota tablosunu alt ağların biriyle ilişkilendirir.|
|[Azure Depolama için sanal ağ hizmet uç noktaları oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-2subnets-service-endpoints-storage-integration)|İki alt ağ içeren yeni bir sanal ağ ve her alt ağ içinde bir ağ arabirimi oluşturur. Alt ağlardan biri için Azure Depolama’ya yönelik hizmet uç noktasını etkinleştirir ve o alt ağa yönelik yeni bir depolama hesabının güvenliğini sağlar.|
|[İki sanal ağı bağlama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-to-vnet-peering)| İki sanal ağ ve bunlar arasında bir sanal ağ eşlemesi oluşturur.|
|[Birden çok IP adresi ile sanal makine oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-multiple-ipconfig)| Birden çok IP adresi ile bir Windows veya Linux sanal makinesi oluşturur.|
|[IPv4 + IPv6 çift yığın sanal ağını yapılandırma](https://github.com/Azure/azure-quickstart-templates/tree/master/ipv6-in-vnet)|İki VM ile çift yığın (IPv4 + IPv6) sanal ağını ve IPv4 ve IPv6 genel IP adreslerine sahip bir Azure temel Load Balancer dağıtır. |
