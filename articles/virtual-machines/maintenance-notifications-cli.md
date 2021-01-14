---
title: CLı kullanarak bakım bildirimleri alın
description: Azure 'da çalışan sanal makineler için bakım bildirimlerini görüntüleyin ve Azure CLı kullanarak Self servis bakımı ' nı başlatın.
author: shants123
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 11/19/2019
ms.author: shants
ms.openlocfilehash: 4bd5c8ae7b4f2ba2d057f61712ce799814e19050
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98202156"
---
# <a name="handling-planned-maintenance-notifications-using-the-azure-cli"></a>Azure CLı kullanarak planlı bakım bildirimlerini işleme

**Bu makale hem Linux hem de Windows çalıştıran sanal makineler için geçerlidir.**

Sanal makinelerin [bakım](maintenance-notifications.md)için ne zaman zamanlandığını görmek için CLI 'yi kullanabilirsiniz. Planlı bakım bilgilerine [az VM Get-instance-View](/cli/azure/vm#az-vm-get-instance-view)üzerinden ulaşabilirsiniz.
 
Bakım bilgileri yalnızca bakım planlandı durumunda döndürülür. 

```azurecli-interactive
az vm get-instance-view -n myVM -g myResourceGroup --query instanceView.maintenanceRedeployStatus
```

Çıktı
```
      "maintenanceRedeployStatus": {
      "additionalProperties": {},
      "isCustomerInitiatedMaintenanceAllowed": true,
      "lastOperationMessage": null,
      "lastOperationResultCode": "None",
      "maintenanceWindowEndTime": "2018-06-04T16:30:00+00:00",
      "maintenanceWindowStartTime": "2018-05-21T16:30:00+00:00",
      "preMaintenanceWindowEndTime": "2018-05-19T12:30:00+00:00",
      "preMaintenanceWindowStartTime": "2018-05-14T12:30:00+00:00"
```

## <a name="start-maintenance"></a>Bakımı Başlat

Aşağıdaki çağrı, true olarak ayarlandıysa bir VM 'de bakım başlatır `IsCustomerInitiatedMaintenanceAllowed` .

```azurecli-interactive
az vm perform-maintenance -g myResourceGroup -n myVM 
```

## <a name="classic-deployments"></a>Klasik dağıtımlar

[!INCLUDE [classic-vm-deprecation](../../includes/classic-vm-deprecation.md)]

Klasik dağıtım modeli kullanılarak dağıtılmış eski sanal makinelerseniz, VM 'Leri sorgulamak ve bakım başlatmak için klasik Azure CLı 'yi kullanabilirsiniz.

Klasik VM ile çalışmak için doğru modda olduğunuzdan emin olun, şunu yazın:

```
azure config mode asm
```

*Myvm* ADLı bir VM 'nin bakım durumunu almak için şunu yazın:

```
azure vm show myVM 
``` 

*Hizmetim* hizmeti ve *Mydeployment* dağıtımında *myvm* adlı klasik sanal makinenizde bakım başlatmak için şunu yazın:

```
azure compute virtual-machine initiate-maintenance --service-name myService --name myDeployment --virtual-machine-name myVM
```

## <a name="next-steps"></a>Sonraki adımlar

Planlı bakım [Azure PowerShell](maintenance-notifications-powershell.md) veya [portalını](maintenance-notifications-portal.md)kullanarak da işleyebilirsiniz.
