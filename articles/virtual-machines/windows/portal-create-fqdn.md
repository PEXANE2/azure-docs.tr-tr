---
title: Azure portal bir Windows sanal makinesi için FQDN oluşturma
description: Azure portal Kaynak Yöneticisi tabanlı bir sanal makine için tam etki alanı adı veya FQDN oluşturma hakkında bilgi edinin.
author: cynthn
ms.service: virtual-machines-windows
ms.subservice: networking
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7708e2ad7e84c4449d425e8bd6eb7d4d1f6a27eb
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288240"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a>Windows VM için Azure portal tam etki alanı adı oluşturma

[Azure Portal](https://portal.azure.com)bir sanal makıne (VM) oluşturduğunuzda, sanal makine için genel bir IP kaynağı otomatik olarak oluşturulur. Bu IP adresini kullanarak VM 'ye uzaktan erişin. Portal [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)veya FQDN oluşturmaz, ancak VM oluşturulduktan sonra bir tane oluşturabilirsiniz. Bu makalede bir DNS adı veya FQDN oluşturma adımları gösterilmektedir.

## <a name="create-a-fqdn"></a>FQDN oluşturma
Bu makalede, zaten bir VM oluşturmuş olduğunuz varsayılmaktadır. Gerekirse, portalda veya [Azure PowerShell](quick-create-powershell.md) [bir VM oluşturabilirsiniz](quick-create-portal.md) . VM 'niz çalışır duruma getirildikten sonra aşağıdaki adımları izleyin:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Artık Uzak Masaüstü Protokolü (RDP) gibi bu DNS adını kullanarak VM 'ye uzaktan bağlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Artık VM 'nizin genel IP ve DNS adı olduğuna göre, genel uygulama çerçeveleri veya IIS, SQL veya SharePoint gibi Hizmetleri dağıtabilirsiniz.

Ayrıca, Azure dağıtımlarınızı oluşturmaya yönelik ipuçları için [Kaynak Yöneticisi kullanma](../../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinebilirsiniz.

