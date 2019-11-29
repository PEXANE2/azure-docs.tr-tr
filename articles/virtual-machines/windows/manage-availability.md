---
title: Azure 'da Windows VM 'lerinin kullanılabilirliğini yönetme
description: Azure 'da Windows uygulamanız için yüksek kullanılabilirlik sağlamak üzere birden çok sanal makine kullanmayı öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 02351953-7b6a-4657-b9e1-de2ea8f6aa05
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 11/27/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1f9914b84b63f271c7dd7d1b8f7dbc3b69511605
ms.sourcegitcommit: c31dbf646682c0f9d731f8df8cfd43d36a041f85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74561182"
---
# <a name="manage-the-availability-of-windows-virtual-machines-in-azure"></a>Azure 'da Windows sanal makinelerinin kullanılabilirliğini yönetme 

Azure 'da Windows uygulamanız için yüksek kullanılabilirlik sağlamak üzere birden çok sanal makine ayarlama ve yönetme yolları hakkında bilgi edinin. [Linux sanal makinelerinin kullanılabilirliğini de yönetebilirsiniz](../linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Klasik dağıtım modelini kullanırken kullanılabilirlik kümeleri oluşturma ve kullanma hakkında yönergeler için bkz. [bir kullanılabilirlik kümesi yapılandırma](classic/configure-availability-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

[!INCLUDE [virtual-machines-common-manage-availability](../../../includes/virtual-machines-common-manage-availability.md)]

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinelerinizin Yük Dengelemesi hakkında daha fazla bilgi edinmek için bkz. [Yük Dengeleme sanal makineleri](tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

IaaS 'de SQL Server N katmanlı uygulamaları çalıştırmaya yönelik başvuru mimarilerini görüntüleyin

* [SQL Server ile Azure 'da Windows N katmanlı uygulama](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/n-tier-sql-server)
* [Yüksek kullanılabilirlik için birden fazla Azure bölgesinde N katmanlı bir uygulama çalıştırma](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)
