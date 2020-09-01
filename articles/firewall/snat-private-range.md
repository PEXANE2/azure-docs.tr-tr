---
title: Azure Güvenlik Duvarı SNAT özel IP adresi aralıkları
description: SNAT için IP adresi aralıklarını yapılandırabilirsiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 08/31/2020
ms.author: victorh
ms.openlocfilehash: 272f5b747efbc3776b1b2ba7c3546ade717c2452
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89231376"
---
# <a name="azure-firewall-snat-private-ip-address-ranges"></a>Azure Güvenlik Duvarı SNAT özel IP adresi aralıkları

Azure Güvenlik Duvarı, genel IP adreslerine giden tüm trafik için otomatik SNAT sağlar. Varsayılan olarak, hedef IP adresi [ıANA RFC 1918](https://tools.ietf.org/html/rfc1918)başına özel bir IP adresi aralığında olduğunda Azure Güvenlik Duvarı ağ kurallarıyla SNAT yapmaz. Uygulama kuralları, hedef IP adresinden bağımsız olarak her zaman [saydam bir proxy](https://wikipedia.org/wiki/Proxy_server#Transparent_proxy) kullanılarak uygulanır.

Bu mantık, trafiği doğrudan Internet 'e yönlendirdiğinizde iyi sonuç verir. Bununla birlikte, [Zorlamalı tünel](forced-tunneling.md)seçeneğini etkinleştirdiyseniz, Internet 'e bağlı trafik AzureFirewallSubnet içindeki güvenlik DUVARı özel IP adreslerinden birine karşı, kaynağı şirket içi güvenlik duvarından gizleyerek gizler.

Kuruluşunuz özel ağlar için genel bir IP adresi aralığı kullanıyorsa, Azure Güvenlik Duvarı AzureFirewallSubnet 'deki güvenlik duvarı özel IP adreslerinden birine giden trafiği yeniden çıkarır. Ancak, Azure Güvenlik duvarını genel IP adresi **aralığınızı SNAT olarak** yapılandırmak için yapılandırabilirsiniz. Örneğin, tek bir IP adresi belirtmek için bunu şöyle belirtebilirsiniz: `192.168.1.10` . IP adresi aralığını belirtmek için bunu şöyle belirtebilirsiniz: `192.168.1.0/24` .

Azure Güvenlik duvarını hedef IP adresinden bağımsız olarak hiçbir şekilde hiçbir şekilde hiçbir şekilde hiçbir şekilde SNAT olarak yapılandırmak için, özel IP adresi aralığınız olarak **0.0.0.0/0** kullanın Bu yapılandırmayla, Azure Güvenlik Duvarı trafiği hiçbir şekilde doğrudan Internet 'e yönlendirmez. Güvenlik duvarını, hedef adresten bağımsız olarak her zaman SNAT olarak yapılandırmak için, özel IP adresi aralığınızdan **255.255.255.255/32** kullanın.

> [!IMPORTANT]
> Kendi özel IP adresi aralıklarını belirtmek ve varsayılan ıANA RFC 1918 adres aralıklarını korumak istiyorsanız, özel listenizin ıANA RFC 1918 aralığını hala içerdiğinden emin olun. 

## <a name="configure-snat-private-ip-address-ranges---azure-powershell"></a>SNAT özel IP adresi aralıklarını Yapılandırma-Azure PowerShell

Güvenlik Duvarı için özel IP adresi aralıklarını belirtmek üzere Azure PowerShell kullanabilirsiniz.

### <a name="new-firewall"></a>Yeni güvenlik duvarı

Yeni bir güvenlik duvarı için Azure PowerShell komutu şu şekilde olur:

`New-AzFirewall -Name $GatewayName -ResourceGroupName $RG -Location $Location -VirtualNetworkName $vnet.Name -PublicIpName $LBPip.Name -PrivateRange @("IANAPrivateRanges","IPRange1", "IPRange2")`

> [!NOTE]
> IANAPrivateRanges, diğer aralıklar buna eklenirken Azure Güvenlik duvarında geçerli varsayılan değerlere genişletilir. Özel Aralık belirtimde IANAPrivateRanges varsayılan kalmasını sağlamak için, `PrivateRange` Aşağıdaki örneklerde gösterildiği gibi belirtimde kalması gerekir.

Daha fazla bilgi için bkz. [New-AzFirewall](https://docs.microsoft.com/powershell/module/az.network/new-azfirewall?view=azps-3.3.0).

### <a name="existing-firewall"></a>Mevcut güvenlik duvarı

Mevcut bir güvenlik duvarını yapılandırmak için aşağıdaki Azure PowerShell komutları kullanın:

```azurepowershell
$azfw = Get-AzFirewall -ResourceGroupName "Firewall Resource Group name"
$azfw.PrivateRange = @("IANAPrivateRanges","IPRange1", "IPRange2")
Set-AzFirewall -AzureFirewall $azfw
```

### <a name="templates"></a>Şablonlar

Bölümüne aşağıdakileri ekleyebilirsiniz `additionalProperties` :

```
"additionalProperties": {
                    "Network.SNAT.PrivateRanges": "IANAPrivateRanges , IPRange1, IPRange2"
                },
```

## <a name="configure-snat-private-ip-address-ranges---azure-portal"></a>SNAT özel IP adresi aralıklarını Yapılandırma-Azure portal

Güvenlik Duvarı için özel IP adresi aralıklarını belirtmek üzere Azure portal kullanabilirsiniz.

1. Kaynak grubunuzu seçin ve ardından güvenlik duvarınızı seçin.
2. **Genel bakış** SAYFASıNDA **özel IP aralıkları**' nı seçerek **IANA RFC 1918**varsayılan değerini seçin.

   **Özel IP öneklerini Düzenle** sayfası açılır:

   :::image type="content" source="media/snat-private-range/private-ip.png" alt-text="Özel IP öneklerini Düzenle":::

1. Varsayılan olarak, **IANAPrivateRanges** yapılandırılır.
2. Ortamınız için özel IP adresi aralıklarını düzenleyin ve ardından **Kaydet**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı Zorlamalı tünel oluşturma](forced-tunneling.md)hakkında bilgi edinin.