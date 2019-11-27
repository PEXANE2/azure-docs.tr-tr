---
title: CLı kullanarak Azure VM 'Leri için bakım bildirimleri alın
description: Azure 'da çalışan sanal makineler için bakım bildirimlerini görüntüleyin ve Azure CLı kullanarak Self servis bakımı ' nı başlatın.
services: virtual-machines
author: shants123
tags: azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/19/2019
ms.author: shants
ms.openlocfilehash: 3c75adc2bf5974c1bce9f05306342068396ae62b
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74535866"
---
# <a name="handling-planned-maintenance-notifications-using-the-azure-cli"></a>Azure CLı kullanarak planlı bakım bildirimlerini işleme

**Bu makale hem Linux hem de Windows çalıştıran sanal makineler için geçerlidir.**

Sanal makinelerin [bakım](maintenance-notifications.md)için ne zaman zamanlandığını görmek için CLI 'yi kullanabilirsiniz. Planlı bakım bilgilerine [az VM Get-instance-View](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-get-instance-view)üzerinden ulaşabilirsiniz.
 
Bakım bilgileri yalnızca bakım planlandı durumunda döndürülür. 

```azurecli-interactive
az vm get-instance-view -n myVM -g myResourceGroup --query instanceView.maintenanceRedeployStatus
```

## <a name="start-maintenance"></a>Bakımı Başlat

Aşağıdaki çağrı, `IsCustomerInitiatedMaintenanceAllowed` true olarak ayarlandıysa bir VM üzerinde bakım başlatır.

```azurecli-interactive
az vm perform-maintenance -g myResourceGroup -n myVM 
```

## <a name="classic-deployments"></a>Klasik dağıtımlar


Klasik dağıtım modeli kullanılarak dağıtılmış eski sanal makinelerseniz, VM 'Leri sorgulamak ve bakım başlatmak için klasik Azure CLı 'yi kullanabilirsiniz.

Klasik VM ile çalışmak için doğru modda olduğunuzdan emin olun, şunu yazın:

```
azure config mode asm
```

*Myvm*ADLı bir VM 'nin bakım durumunu almak için şunu yazın:

```
azure vm show myVM 
``` 

*Hizmetim* hizmeti ve *Mydeployment* dağıtımında *myvm* adlı klasik sanal makinenizde bakım başlatmak için şunu yazın:

```
azure compute virtual-machine initiate-maintenance --service-name myService --name myDeployment --virtual-machine-name myVM
```

## <a name="next-steps"></a>Sonraki adımlar

Planlı bakım [Azure PowerShell](maintenance-notifications-powershell.md) veya [portalını](maintenance-notifications-portal.md)kullanarak da işleyebilirsiniz.
