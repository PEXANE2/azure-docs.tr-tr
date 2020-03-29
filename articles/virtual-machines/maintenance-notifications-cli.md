---
title: CLI'yi kullanarak bakım bildirimleri alın
description: Azure'da çalışan sanal makineler için bakım bildirimlerini görüntüleyin ve Azure CLI'yi kullanarak self servis bakıma başlayın.
author: shants123
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/19/2019
ms.author: shants
ms.openlocfilehash: 4ad57c1c71a51f948bd405a5487a1e27e36bfff7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920901"
---
# <a name="handling-planned-maintenance-notifications-using-the-azure-cli"></a>Azure CLI'yi kullanarak planlanan bakım bildirimlerini işleme

**Bu makale, hem Linux hem de Windows çalıştıran sanal makineler için geçerlidir.**

VM'lerin [bakım](maintenance-notifications.md)için ne zaman zamanlandığını görmek için CLI'yi kullanabilirsiniz. Planlanan bakım bilgileri [az vm get-instance-view](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-get-instance-view)kullanılabilir.
 
Bakım bilgileri yalnızca planlanmış bir bakım varsa döndürülür. 

```azurecli-interactive
az vm get-instance-view -n myVM -g myResourceGroup --query instanceView.maintenanceRedeployStatus
```

## <a name="start-maintenance"></a>Bakımı başlatın

Aşağıdaki arama, doğru ayarlanmışsa `IsCustomerInitiatedMaintenanceAllowed` vm'de bakıma başlar.

```azurecli-interactive
az vm perform-maintenance -g myResourceGroup -n myVM 
```

## <a name="classic-deployments"></a>Klasik dağıtımlar

[!INCLUDE [classic-vm-deprecation](../../includes/classic-vm-deprecation.md)]

Klasik dağıtım modelini kullanarak dağıtılan eski VM'leriniz hala varsa, VM'leri sorgulamak ve bakımı başlatmak için Azure klasik CLI'yi kullanabilirsiniz.

Yazarak klasik VM ile çalışmak için doğru modda olduğundan emin olun:

```
azure config mode asm
```

*myVM*adlı bir VM'nin bakım durumunu almak için, yazın:

```
azure vm show myVM 
``` 

*MyService* hizmetinde ve *myDeployment* dağıtımında *myVM* adlı klasik VM'nizde bakım başlatmak için şunları yazın:

```
azure compute virtual-machine initiate-maintenance --service-name myService --name myDeployment --virtual-machine-name myVM
```

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, [Azure PowerShell](maintenance-notifications-powershell.md) veya [portalını](maintenance-notifications-portal.md)kullanarak planlı bakımı da işleyebilirsiniz.
