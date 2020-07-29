---
title: Azure portal VM için FQDN oluşturma
description: Azure portal Kaynak Yöneticisi tabanlı bir sanal makine için tam etki alanı adı veya FQDN oluşturma hakkında bilgi edinin.
author: cynthn
ms.service: virtual-machines
ms.subservice: networking
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9ac44c8d157102a39df9c580938589862dafd0a4
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87371898"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Linux VM için Azure portal tam etki alanı adı oluşturma

[Azure Portal](https://portal.azure.com)bir sanal makıne (VM) oluşturduğunuzda, sanal makine için genel bir IP kaynağı otomatik olarak oluşturulur. Bu IP adresini kullanarak VM 'ye uzaktan erişin. Portal [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)veya FQDN OLUŞTURMASA da VM oluşturulduktan sonra bir tane ekleyebilirsiniz. Bu makalede bir DNS adı veya FQDN oluşturma adımları gösterilmektedir.

## <a name="create-a-fqdn"></a>FQDN oluşturma
Bu makalede, zaten bir VM oluşturmuş olduğunuz varsayılmaktadır. Gerekirse, portalda veya [Azure CLI ile](quick-create-cli.md) [bir VM oluşturabilirsiniz](quick-create-portal.md) . VM 'niz çalışır duruma getirildikten sonra aşağıdaki adımları izleyin:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Artık, ile gibi bu DNS adını kullanarak VM 'ye uzaktan bağlanabilirsiniz `ssh azureuser@mydns.westus.cloudapp.azure.com` .

## <a name="next-steps"></a>Sonraki adımlar
Artık VM 'nizin genel IP ve DNS adı olduğuna göre, yaygın uygulama çerçevelerini veya NGINX, MongoDB, Docker gibi Hizmetleri dağıtabilirsiniz.

Ayrıca, Azure dağıtımlarınızı oluşturmaya yönelik ipuçları için [Kaynak Yöneticisi kullanma](../../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinebilirsiniz.

