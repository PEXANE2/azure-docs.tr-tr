---
title: Azure PowerShell örnekleri-Azure Cloud Services sıfırlama (genişletilmiş destek)
description: Bir Azure bulut hizmeti (genişletilmiş destek) dağıtımını sıfırlamaya yönelik örnek betikler
ms.topic: sample
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 40b44fd277eac14a5bf2c15f58fccfd9d5b156c4
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98881493"
---
# <a name="reset-an-azure-cloud-service-extended-support"></a>Azure bulut hizmetini sıfırlama (genişletilmiş destek) 
Bu örnekler, var olan bir Azure bulut hizmeti (genişletilmiş destek) dağıtımını sıfırlamanın çeşitli yollarını kapsar.

## <a name="reimage-role-instances-of-cloud-service"></a>Bulut hizmetinin rol örnekleri yeniden görüntüsü
```powershell
$roleInstances = @("ContosoFrontEnd_IN_0", "ContosoBackEnd_IN_1")
Reset-AzCloudService -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstance $roleInstances -Reimage
```
Bu komut 2 rol örneklerinin ContosoFrontEnd ve ContosOrg adlı kaynak grubuna ait ContosoCS adlı bir bulut hizmetinde **\_ \_ 1 ' de contosoarka** ucunu yeniden görüntüler. **\_ \_**

## <a name="reimage-all-roles-of-cloud-service"></a>Tüm bulut hizmeti rollerini yeniden görüntü oluştur
```powershell
Reset-AzCloudService -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstance "*" -Reimage
```

## <a name="reimage-a-single-role-instance-of-a-cloud-service"></a>Bulut hizmetinin tek bir rol örneğini yeniden görüntü oluşturma
```powershell
Reset-AzCloudServiceRoleInstance -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstanceName "ContosoFrontEnd_IN_0" -Reimage
```

## <a name="restart-a-single-role-instance-of-a-cloud-service"></a>Bulut hizmetinin tek bir rol örneğini yeniden başlatma
```powershell
Reset-AzCloudService -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstance "*" -Restart
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure Cloud Services (genişletilmiş destek) hakkında daha fazla bilgi için bkz. [azure Cloud Services (genişletilmiş destek) genel bakış](overview.md).
- [Cloud Services (genişletilmiş destek) örnekleri deposunu](https://github.com/Azure-Samples/cloud-services-extended-support) ziyaret edin