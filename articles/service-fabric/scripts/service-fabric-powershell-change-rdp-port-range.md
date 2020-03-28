---
title: Azure PowerShell Betiği Örneği - RDP bağlantı noktası aralığını değiştirme | Microsoft Docs
description: Azure PowerShell Betiği Örneği - Dağıtılan bir kümenin RDP bağlantı noktası aralığını değiştirir.
services: service-fabric
tags: azure-service-management
author: athinanthny
ms.author: atsenthi
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 03/19/2018
ms.custom: sfrev
ms.openlocfilehash: b3d922cb5a7cdf34e63731ab32f21c94d7b6bca0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76025011"
---
# <a name="update-the-rdp-port-range-values"></a>RDP bağlantı noktası aralığı değerlerini güncelleştirme

Bu örnek betik, küme dağıtıldıktan sonra küme düğümü sanal makinelerinde RDP bağlantı noktası aralığı değerlerini değiştirir.  Temel sanal makinelerin döngüye girmemesi için Azure PowerShell kullanılır.  Komut dosyası, `Microsoft.Network/loadBalancers` kümenin kaynak grubundaki kaynağı `inboundNatPools.frontendPortRangeStart` alır `inboundNatPools.frontendPortRangeEnd` ve ve değerleri güncelleştirir. Parametreleri gereken şekilde özelleştirin.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeyi kullanarak Azure PowerShell’i yükleyin.

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Al-AzResource](/powershell/module/az.resources/get-azresource) | `Microsoft.Network/loadBalancers` kaynağını alır. |
|[Set-AzResource](/powershell/module/az.resources/set-azresource)|`Microsoft.Network/loadBalancers` kaynağını güncelleştirir.|

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure Service Fabric için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md) bölümünde bulabilirsiniz.
