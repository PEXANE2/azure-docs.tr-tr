---
title: Azure PowerShell kullanarak birden çok genel IP adresi ile Azure Güvenlik Duvarı dağıtma
description: Sanal hub 'ı korumak için birden çok genel IP adresi içeren bir güvenlik duvarı dağıtın
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: how-to
ms.date: 07/09/2020
ms.author: victorh
ms.openlocfilehash: 652c7cbfbe63ef2ae9a0d54e05407152ea300f1d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87007019"
---
# <a name="deploy-an-azure-firewall-with-multiple-public-ip-addresses"></a>Birden çok genel IP adresi ile bir Azure Güvenlik Duvarı dağıtın

Azure Güvenlik Duvarı 'nı kullanarak bir sanal hub 'ı korumak istiyorsanız, Azure PowerShell kullanarak birden çok genel IP adresi ile güvenlik duvarını dağıtabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-the-firewall"></a>Güvenlik duvarını dağıtma

Bir sanal hub 'ı korumak için birden çok genel IP adresi ile bir Azure Güvenlik Duvarı dağıtmak üzere aşağıdaki Azure PowerShell örneğini kullanın.

```azurepowershell
Select-AzSubscription -SubscriptionId <subscription ID> 

$rgName = <resource group name> 
$vHub = Get-AzVirtualHub -Name <hub name> 
$vHubId = $vHub.Id 
$fwpips = New-AzFirewallHubPublicIpAddress -Count 3
$hubIpAddresses = New-AzFirewallHubIpAddress -PublicIPs $fwpips 
$fw = New-AzFirewall -Name <firewall name> -ResourceGroupName $rgName `
     -Location <location> -Sku AZFW_Hub -HubIPAddresses $hubIpAddresses `
     -VirtualHubId $vHubId 
```

### <a name="update-a-public-ip-address"></a>Genel IP adresini güncelleştirme

Azure Güvenlik duvarının genel IP adresini güncelleştirmek için Azure PowerShell kullanabilirsiniz. Aşağıdaki örnek bir güvenlik duvarındaki genel IP adresini siler. Üç genel IP adresi ile başlar.

```azurepowershell
Select-AzSubscription -SubscriptionId <subscription ID>

$azfw = get-azfirewall -Name <firewall name> -ResourceGroupName <resource group name>
$ip1 = New-AzFirewallPublicIpAddress -Address <first ip address to keep>
$ip2 = New-AzFirewallPublicIpAddress -Address <second ip address to keep>
$ipAddresses = $ip1,$ip2
$azfw.HubIPAddresses.publicIPs.Addresses = $ipAddresses
$azfw.HubIPAddresses.publicIPs.count = 2
Set-AzFirewall -AzureFirewall $azfw
```

## <a name="next-steps"></a>Sonraki adımlar

[Güvenli sanal merkez nedir?](secured-virtual-hub.md)