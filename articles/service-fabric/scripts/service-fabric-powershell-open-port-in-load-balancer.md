---
title: PowerShell 'de yük dengeleyicide uygulama bağlantı noktasını aç
description: Azure PowerShell Betiği Örneği - Service Fabric uygulaması için Azure Load Balancer’da bir bağlantı noktasını açın.
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
ms.date: 05/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 3e5e1df77b8bc701bf330d98f264db26a01ea748
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75614766"
---
# <a name="open-an-application-port-in-the-azure-load-balancer"></a>Azure Load Balancer’da bir uygulama bağlantı noktasını açma

Azure’da çalıştırılan bir Service Fabric uygulaması, Azure Load Balancer’ın ardında yer alır. Bu örnek betik, Service Fabric uygulamasının dış istemcilerle iletişim kurabilmesi için Azure Load Balancer’da bir bağlantı noktasını açar. Parametreleri gereken şekilde özelleştirin. Kümeniz bir ağ güvenliği grubundaysa, gelen trafiğe izin vermek için [bir gelen ağ güvenliği grubu kuralı ekleyin](service-fabric-powershell-add-nsg-rule.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Gerekirse, [Service Fabric SDK’sı](../service-fabric-get-started.md) ile Service Fabric PowerShell modülünü yükleyin. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in the load balancer")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Bir Azure kaynağını alır.  |
| [Get-AzLoadBalancer](/powershell/module/az.network/get-azloadbalancer) | Azure Load Balancer’ı alır. |
| [Add-AzLoadBalancerProbeConfig](/powershell/module/az.network/add-azloadbalancerprobeconfig) | Yük dengeleyiciye bir araştırma yapılandırması ekler.|
| [Get-AzLoadBalancerProbeConfig](/powershell/module/az.network/get-azloadbalancerprobeconfig) | Yük dengeleyici için bir araştırma yapılandırması alır. |
| [Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig) | Yük dengeleyiciye bir kural yapılandırması ekler. |
| [Set-AzLoadBalancer](/powershell/module/az.network/set-azloadbalancer) | Yük dengeleyici için hedef durumunu ayarlar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure Service Fabric için ek Powershell örneklerine [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md) içinden erişilebilir.
