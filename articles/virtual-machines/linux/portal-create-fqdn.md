---
title: Azure portalında bir Linux VM için FQDN oluşturma
description: Azure portalında Kaynak Yöneticisi tabanlı bir sanal makine için Tam Nitelikli Alan Adı veya FQDN nasıl oluşturulacak öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2cd6c249-a737-4a0a-b5ba-e1c09e551b30
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 256e1e60ec8f50df2faea64f31e88d00370b33c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458710"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Linux VM için Azure portalında tam nitelikli bir etki alanı adı oluşturma

[Azure portalında](https://portal.azure.com)sanal bir makine (VM) oluşturduğunuzda, sanal makine için genel bir IP kaynağı otomatik olarak oluşturulur. VM'ye uzaktan erişmek için bu IP adresini kullanırsınız. Portal [tam nitelikli bir etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)veya FQDN oluşturmasa da, VM oluşturulduktan sonra bir tane ekleyebilirsiniz. Bu makalede, bir DNS adı veya FQDN oluşturmak için adımları gösterir.

## <a name="create-a-fqdn"></a>FQDN oluşturma
Bu makalede, zaten bir VM oluşturduğunuz varsayar. Gerekirse, portalda veya [Azure CLI ile](quick-create-cli.md)bir [VM oluşturabilirsiniz.](quick-create-portal.md) VM'niz çalışmaya başladığında aşağıdaki adımları izleyin:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Artık VM'ye bu DNS adını kullanarak uzaktan `ssh azureuser@mydns.westus.cloudapp.azure.com`bağlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Artık VM'nizin ortak bir IP ve DNS adı olduğuna göre, nginx, MongoDB, Docker, vb. gibi yaygın uygulama çerçevelerini veya hizmetleri dağıtabilirsiniz.

Azure dağıtımlarınızı oluşturma yla ilgili ipuçları için [Kaynak Yöneticisi'ni kullanma](../../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinebilirsiniz.

