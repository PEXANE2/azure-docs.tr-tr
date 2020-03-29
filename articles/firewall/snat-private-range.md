---
title: Azure Güvenlik Duvarı SNAT özel IP adresi aralıkları
description: Güvenlik duvarının bu IP adreslerine SNAT trafiği yapmaması için IP adresi özel aralıklarını yapılandırabilirsiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 03/20/2020
ms.author: victorh
ms.openlocfilehash: ed8cef00b7de67458c607373c724a3717f14a7cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064804"
---
# <a name="azure-firewall-snat-private-ip-address-ranges"></a>Azure Güvenlik Duvarı SNAT özel IP adresi aralıkları

Azure Güvenlik Duvarı, hedef IP adresi [IANA RFC 1918](https://tools.ietf.org/html/rfc1918)başına özel bir IP adresi aralığında olduğunda Ağ kurallarıyla SNAT yapmaz. Uygulama kuralları her zaman hedef IP adresi ne olursa olsun saydam bir [proxy](https://wikipedia.org/wiki/Proxy_server#Transparent_proxy) kullanılarak uygulanır.

Kuruluşunuz özel ağlar için genel bir IP adresi aralığı kullanıyorsa, Azure Güvenlik Duvarı trafiği AzureFirewallSubnet'teki güvenlik duvarı özel IP adreslerinden birine gider. Ancak, Azure Güvenlik Duvarı'nı ortak IP adres aralığınızı SNAT **olarak değil** olarak yapılandırabilirsiniz.

## <a name="configure-snat-private-ip-address-ranges"></a>SNAT özel IP adresi aralıklarını yapılandırma

Güvenlik duvarının SNAT'a girmeyeceği bir IP adresi aralığı nı belirtmek için Azure PowerShell'i kullanabilirsiniz.

### <a name="new-firewall"></a>Yeni güvenlik duvarı

Yeni bir güvenlik duvarı için Azure PowerShell komutu şu şekildedir:

`New-AzFirewall -Name $GatewayName -ResourceGroupName $RG -Location $Location -VirtualNetworkName $vnet.Name -PublicIpName $LBPip.Name -PrivateRange @("IANAPrivateRanges","IPRange1", "IPRange2")`

> [!NOTE]
> IANAPrivateRanges, Azure Güvenlik Duvarı'ndaki geçerli varsayılan lara genişletilirken, diğer aralıklar da eklenir.

Daha fazla bilgi için [New-AzFirewall'a](https://docs.microsoft.com/powershell/module/az.network/new-azfirewall?view=azps-3.3.0)bakın.

### <a name="existing-firewall"></a>Varolan güvenlik duvarı

Varolan bir güvenlik duvarını yapılandırmak için aşağıdaki Azure PowerShell komutlarını kullanın:

```azurepowershell
$azfw = Get-AzFirewall -ResourceGroupName "Firewall Resource Group name"
$azfw.PrivateRange = @("IANAPrivateRanges","IPRange1", "IPRange2")
Set-AzFirewall -AzureFirewall $azfw
```

### <a name="templates"></a>Şablonlar

`additionalProperties` Bölüme aşağıdakileri ekleyebilirsiniz:

```
"additionalProperties": {
                    "Network.SNAT.PrivateRanges": "IANAPrivateRanges , IPRange1, IPRange2"
                },
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı'nı nasıl dağıtıp yapılandırılamayı](tutorial-firewall-deploy-portal.md)öğrenin.