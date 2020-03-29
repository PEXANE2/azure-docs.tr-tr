---
title: Azure portalında Windows VM için FQDN oluşturma
description: Azure portalında Kaynak Yöneticisi tabanlı bir sanal makine için Tam Nitelikli Alan Adı veya FQDN nasıl oluşturulacak öğrenin.
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
ms.openlocfilehash: fa4e4bd7561a7a745e6eb0b70016144b9fb57998
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75371284"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a>Windows VM için Azure portalında tam nitelikli bir etki alanı adı oluşturma

[Azure portalında](https://portal.azure.com)sanal bir makine (VM) oluşturduğunuzda, sanal makine için genel bir IP kaynağı otomatik olarak oluşturulur. VM'ye uzaktan erişmek için bu IP adresini kullanırsınız. Portal [tam nitelikli bir etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)veya FQDN oluşturmasa da, VM oluşturulduktan sonra bir tane oluşturabilirsiniz. Bu makalede, bir DNS adı veya FQDN oluşturmak için adımları gösterir.

## <a name="create-a-fqdn"></a>FQDN oluşturma
Bu makalede, zaten bir VM oluşturduğunuz varsayar. Gerekirse, portalda veya [Azure PowerShell ile](quick-create-powershell.md)bir [VM oluşturabilirsiniz.](quick-create-portal.md) VM'niz çalışmaya başladığında aşağıdaki adımları izleyin:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Artık Uzak Masaüstü Protokolü (RDP) gibi bu DNS adını kullanarak VM'ye uzaktan bağlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
VM'nizin ortak bir IP ve DNS adı olduğuna göre, IIS, SQL veya SharePoint gibi yaygın uygulama çerçevelerini veya hizmetlerini dağıtabilirsiniz.

Azure dağıtımlarınızı oluşturma yla ilgili ipuçları için [Kaynak Yöneticisi'ni kullanma](../../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinebilirsiniz.

