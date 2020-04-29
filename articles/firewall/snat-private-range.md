---
title: Azure Güvenlik Duvarı SNAT özel IP adresi aralıkları
description: IP adresi özel aralıklarını, güvenlik duvarının bu IP adreslerine gelen trafiği SNAT olmayacak şekilde yapılandırabilirsiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 03/20/2020
ms.author: victorh
ms.openlocfilehash: ed8cef00b7de67458c607373c724a3717f14a7cb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80064804"
---
# <a name="azure-firewall-snat-private-ip-address-ranges"></a>Azure Güvenlik Duvarı SNAT özel IP adresi aralıkları

Hedef IP adresi, [ıANA RFC 1918](https://tools.ietf.org/html/rfc1918)başına özel bir IP adresi aralığında olduğunda Azure Güvenlik Duvarı ağ kurallarıyla SNAT yapmaz. Uygulama kuralları, hedef IP adresinden bağımsız olarak her zaman [saydam bir proxy](https://wikipedia.org/wiki/Proxy_server#Transparent_proxy) kullanılarak uygulanır.

Kuruluşunuz özel ağlar için genel bir IP adresi aralığı kullanıyorsa, Azure Güvenlik Duvarı AzureFirewallSubnet 'deki güvenlik duvarı özel IP adreslerinden birine giden trafiği yeniden çıkarır. Ancak, Azure Güvenlik duvarını genel IP adresi **aralığınızı SNAT olarak** yapılandırmak için yapılandırabilirsiniz.

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
$azfw.PrivateRange = @("IANAPrivateRanges","IPRange1", "IPRange2")
Set-AzFirewall -AzureFirewall $azfw
```

### <a name="templates"></a>Şablonlar

`additionalProperties` Bölümüne aşağıdakileri ekleyebilirsiniz:

```
"additionalProperties": {
                    "Network.SNAT.PrivateRanges": "IANAPrivateRanges , IPRange1, IPRange2"
                },
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik duvarını dağıtmayı ve yapılandırmayı](tutorial-firewall-deploy-portal.md)öğrenin.