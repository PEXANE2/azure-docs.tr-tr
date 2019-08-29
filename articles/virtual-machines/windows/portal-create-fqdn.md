---
title: Azure portal bir Windows sanal makinesi için FQDN oluşturun | Microsoft Docs
description: Azure portal Kaynak Yöneticisi tabanlı bir sanal makine için tam etki alanı adı veya FQDN oluşturma hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: a2ae5887-76df-485e-ae19-0efd96df8600
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c86e2739efd951ad9274f2fa0829b9fd7e13e3e5
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70102606"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a>Windows VM için Azure portal tam etki alanı adı oluşturma

[Azure Portal](https://portal.azure.com)bir sanal makıne (VM) oluşturduğunuzda, sanal makine için genel bir IP kaynağı otomatik olarak oluşturulur. Bu IP adresini kullanarak VM 'ye uzaktan erişin. Portal [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)veya FQDN oluşturmaz, ancak VM oluşturulduktan sonra bir tane oluşturabilirsiniz. Bu makalede bir DNS adı veya FQDN oluşturma adımları gösterilmektedir.

## <a name="create-a-fqdn"></a>FQDN oluşturma
Bu makalede, zaten bir VM oluşturmuş olduğunuz varsayılmaktadır. Gerekirse, portalda veya [Azure PowerShell](quick-create-powershell.md) [bir VM oluşturabilirsiniz](quick-create-portal.md) . VM 'niz çalışır duruma getirildikten sonra aşağıdaki adımları izleyin:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Artık Uzak Masaüstü Protokolü (RDP) gibi bu DNS adını kullanarak VM 'ye uzaktan bağlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Artık VM 'nizin genel IP ve DNS adı olduğuna göre, genel uygulama çerçeveleri veya IIS, SQL veya SharePoint gibi Hizmetleri dağıtabilirsiniz.

Ayrıca, Azure dağıtımlarınızı oluşturmaya yönelik ipuçları için [Kaynak Yöneticisi kullanma](../../azure-resource-manager/resource-group-overview.md) hakkında daha fazla bilgi edinebilirsiniz.

