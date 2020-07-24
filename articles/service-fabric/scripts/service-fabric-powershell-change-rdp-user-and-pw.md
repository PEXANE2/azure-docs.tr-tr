---
title: PowerShell 'de RDP Kullanıcı adını ve parolasını güncelleştirme
description: Azure PowerShell Betiği Örneği - Belirli bir düğüm türünün tüm Service Fabric küme düğümleri için RDP kullanıcı adını ve parolasını güncelleştirin.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 03/19/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: bcf619e2251f5c1b641190549da45f721835ce0e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87076161"
---
# <a name="update-the-admin-username-and-password-of-the-vms-in-a-cluster"></a>Bir kümedeki sanal makinelerin yönetici kullanıcı adını ve parolasını güncelleştirme

Service Fabric kümesindeki her [düğüm türü](../service-fabric-cluster-nodetypes.md) bir sanal makine ölçek kümesidir. Bu örnek betik, belirli bir düğüm türündeki küme sanal makineleri için yönetici kullanıcı adı ve parolasını güncelleştirir.  Yönetici parolası, değiştirilebilir bir ölçek kümesi özelliği olmadığından, ölçek kümesine VMAccessAgent uzantısını ekleyin.  Kullanıcı adı ve parola değişiklikleri, ölçek kümesindeki tüm düğümlere uygulanır. Parametreleri gereken şekilde özelleştirin.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/) bulunan yönergeyi kullanarak Azure PowerShell’i yükleyin. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-user-and-pw/change-rdp-user-and-pw.ps1 "Updates a RDP username and password for cluster nodes")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzVmss](/powershell/module/az.compute/get-azvmss) | Küme düğümü türünün özelliklerini alır (sanal makine ölçek kümesi).   |
| [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension)| Sanal makine ölçek kümesine bir uzantı ekler.|
| [Güncelleştirme-AzVmss](/powershell/module/az.compute/update-azvmss)|Sanal makine ölçek kümesinin durumunu, yerel bir VMSS nesnesinin durumuna güncelleştirir.|

## <a name="duration"></a>Süre

Beş düğüm içeren tek düğümlü bir tür, örneğin, Kullanıcı adının veya parolanın değiştirilmesi için 60 45 dakikalık bir süreye sahiptir. 

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Azure Service Fabric için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md) bölümünde bulabilirsiniz.
