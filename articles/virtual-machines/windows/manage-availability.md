---
title: Azure'da Windows VM'lerinin kullanılabilirliğini yönetme
description: Azure'daki Windows uygulamanız için yüksek kullanılabilirlik sağlamak için birden çok sanal makineyi nasıl kullanacağınızı öğrenin
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
ms.openlocfilehash: ae5d60f77319a6590807ae0b18a0c07c116e128b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267449"
---
# <a name="manage-the-availability-of-windows-virtual-machines-in-azure"></a>Azure’da Windows sanal makinelerinin kullanılabilirliğini yönetme 

Azure'daki Windows uygulamanız için yüksek kullanılabilirlik sağlamak için birden çok sanal makine ayarlamanın ve yönetmenin yollarını öğrenin. Ayrıca [Linux sanal makinelerin kullanılabilirliğini yönetebilirsiniz.](../linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[!INCLUDE [virtual-machines-common-manage-availability](../../../includes/virtual-machines-common-manage-availability.md)]

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinelerinizi yük dengeleme hakkında daha fazla bilgi edinmek [için, Yük Dengeleme sanal makineleri](tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)ne bıyiklü.

IaaS'da SQL Server'da N katmanlı uygulamaları çalıştırmak için Başvuru Mimarilerini görüntüleyin

* [SQL Server ile Azure'da Windows N katmanlı uygulama](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/n-tier-sql-server)
* [Yüksek kullanılabilirlik için birden çok Azure bölgesinde N katmanlı bir uygulama çalıştırma](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)
