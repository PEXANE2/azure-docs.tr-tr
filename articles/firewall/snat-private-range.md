---
title: Azure Güvenlik Duvarı SNAT özel IP adresi aralıkları
description: IP adresi özel aralıklarını, güvenlik duvarının bu IP adreslerine gelen trafiği SNAT olmayacak şekilde yapılandırabilirsiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 01/09/2020
ms.author: victorh
ms.openlocfilehash: b190d07ceadea43ca572f5eb5be3eeeafa616971
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77444465"
---
# <a name="azure-firewall-snat-private-ip-address-ranges"></a>Azure Güvenlik Duvarı SNAT özel IP adresi aralıkları

Hedef IP adresi, [ıANA RFC 1918](https://tools.ietf.org/html/rfc1918)başına özel bir IP adresi aralığında olduğunda Azure GÜVENLIK duvarı SNAT değildir. 

Kuruluşunuz özel ağlar için genel bir IP adresi aralığı kullanıyorsa, Azure Güvenlik Duvarı, trafiği AzureFirewallSubnet içindeki güvenlik duvarı özel IP adreslerinden birine karşı bir şekilde SNAT olarak kullanır. Ancak, Azure Güvenlik duvarını genel IP adresi **aralığınızı SNAT olarak** yapılandırmak için yapılandırabilirsiniz.

## <a name="configure-snat-private-ip-address-ranges"></a>SNAT özel IP adresi aralıklarını yapılandırma

Güvenlik duvarının SNAT olmayacak bir IP adresi aralığı belirtmek için Azure PowerShell kullanabilirsiniz.

### <a name="new-firewall"></a>Yeni güvenlik duvarı

Yeni bir güvenlik duvarı için Azure PowerShell komutu şu şekilde olur:

`New-AzFirewall -Name $GatewayName -ResourceGroupName $RG -Location $Location -VirtualNetworkName $vnet.Name -PublicIpName $LBPip.Name -PrivateRange @("IANAPrivateRanges","IPRange1", "IPRange2")`

> [!NOTE]
> IANAPrivateRanges, diğer aralıklar buna eklenirken Azure Güvenlik duvarında geçerli varsayılan değerlere genişletilir.

Daha fazla bilgi için bkz. [New-AzFirewall](https://docs.microsoft.com/powershell/module/az.network/new-azfirewall?view=azps-3.3.0).

### <a name="existing-firewall"></a>Mevcut güvenlik duvarı

Mevcut bir güvenlik duvarını yapılandırmak için aşağıdaki Azure PowerShell komutları kullanın:

```azurepowershell
$azfw = Get-AzFirewall -ResourceGroupName "Firewall Resource Group name"
$azfw.PrivateRange = @(“IANAPrivateRanges”,“IPRange1”, “IPRange2”)
Set-AzFirewall -AzureFirewall $azfw
```

### <a name="templates"></a>Şablonlar

`additionalProperties` bölümüne aşağıdakileri ekleyebilirsiniz:

```
"additionalProperties": {
                    "Network.SNAT.PrivateRanges": "IANAPrivateRanges , IPRange1, IPRange2"
                },
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik duvarını dağıtmayı ve yapılandırmayı](tutorial-firewall-deploy-portal.md)öğrenin.