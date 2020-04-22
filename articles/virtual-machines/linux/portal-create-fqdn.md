---
title: Azure portalında bir VM için FQDN oluşturma
description: Azure portalında Kaynak Yöneticisi tabanlı bir sanal makine için Tam Nitelikli Alan Adı veya FQDN nasıl oluşturulacak öğrenin.
author: cynthn
ms.service: virtual-machines
ms.subservice: networking
ms.topic: article
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f51b9ecf8e300af6b1e3f11d8431de7a282ab342
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759359"
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

