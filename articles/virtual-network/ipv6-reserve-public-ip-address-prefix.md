---
title: Azure sanal ağında herkese açık IPv6 adreslerini ve adres aralıklarını rezerve edin
titlesuffix: Azure Virtual Network
description: Azure sanal ağında genel IPv6 adreslerini ve adres aralıklarını nasıl ayırılamayı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/15/2019
ms.author: kumud
ms.openlocfilehash: 76d1ba2717ac3c8ac8e86687ef1754a8790f3e4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72595080"
---
# <a name="reserve-public-ipv6-address-prefix"></a>Rezerv genel IPv6 adresi öneki
Azure Sanal Ağ için IPv6 (VNet), Hem sanal ağ içinde hem de Internet'ten IPv6 ve IPv4 bağlantısıyla Azure'daki uygulamaları barındırmanızı sağlar. Tek tek IPv6 adreslerini ayırmanın yanı sıra, kullanımınız için bitişik Azure IPv6 adresleri (IP Öneki olarak bilinir) aralıklarını ayırabilirsiniz. Bu makalede, Azure PowerShell ve CLI kullanarak IPv6 genel IP adresleri ve adres aralıkları nasıl oluşturulacak açıklanmaktadır.

> [!Important]
> Azure Sanal Ağı için IPv6 şu anda genel önizlemededir. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="create-a-single-reserved-ipv6-public-ip"></a>Tek bir ayrılmış IPv6 genel IP'si oluşturma

### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

Azure PowerShell'i [kullanarak Yeni-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) ile tek bir ayrılmış (statik) IPv6 Public IP adresi oluşturabilirsiniz:

```azurepowershell  
 $myOwnIPv6Address = New-AzPublicIpAddress `
 -name PIPv6_WestUS `
 -ResourceGroup MyRG `
 -Location "West US" `
 -Sku Standard `
 -allocationMethod static `
 -IpAddressVersion IPv6
 ```

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

 Az [network public-ip oluşturma](/cli/azure/network/public-ip) ile tek bir ayrılmış (statik) IPv6 Genel IP adresi Azure CLI oluşturabilirsiniz aşağıdaki gibi:
  
```azurecli
 az network public-ip create \
 --name dsPublicIP_v6 \
 --resource-group UpgradeInPlace_CLI_RG1 \
 --location WestUS \
 --sku Standard  \
 --allocation-method static  \
 --version IPv6
```

## <a name="create-a-reserved-ipv6-prefix-range"></a>Ayrılmış bir IPv6 öneki oluşturma (aralık)

Bir IPv6 öneki ayırmak için, IPv4 önekleri oluşturmak için kullanılan komuta IPv6 IP adresi ailesini ekleyin. Aşağıdaki komutlar boyut /125 (8 IPv6 adresleri) bir önek oluşturur.  

### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

Aşağıdaki gibi [az ağ public-ip oluşturma](/powershell/module/az.network/new-azpublicipprefix) ile Azure CLI kullanarak ortak bir IPv6 adresi oluşturabilirsiniz:
```azurepowershell  
 $myOwnIPv6Prefix = New-AzPublicIpPrefix `
 -name IPv6PrefixWestUS `
 -ResourceGroupName MyRG `
 -Location "West US" `
 -Sku Standard `
 -IpAddressVersion IPv6 `
 -PrefixLength 125
```

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

Azure CLI'yi kullanarak genel bir IPv6 adresi oluşturabilirsiniz:

```azurecli  
az network public-ip prefix create \
--name IPv6PrefixWestUS \
--resource-group MyRG \
--location WestUS \
--version IPv6 \
--length 125
```

## <a name="allocate-a-public-ip-address-from-a-reserved-ipv6-prefix"></a>Ayrılmış bir IPv6 Öneki'nden genel bir IP adresi ayırma

### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

 Azure PowerShell kullanarak ortak IP oluştururken `-PublicIpPrefix` bağımsız değişkeni ekleyerek ayrılmış bir önek için statik bir IPv6 Public IP oluşturursunuz. Aşağıdaki örnek, bir önek oluşturulduğunu ve PowerShell değişkeninde depolandığını varsayıyor: *$myOwnIPv6Prefix*.

```azurepowershell:  
 $MyIPv6PublicIPFromMyReservedPrefix = New-AzPublicIpAddress \
 -name PIPv6_fromPrefix `
 -ResourceGroup DsStdLb04 `
 -Location "West Central US" `
 -Sku Standard `
 -allocationMethod static `
 -IpAddressVersion IPv6 `
 -PublicIpPrefix $myOwnIPv6Prefix
```

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma
 
Aşağıdaki örnek, bir önek oluşturulduğunu ve CLI değişkeninde depolandığını varsayar: *IPv6PrefixWestUS*.

```azurecli 
az network public-ip create \
--name dsPublicIP_v6 \
--resource-group UpgradeInPlace_CLI_RG1 \
--location WestUS \
--sku Standard \
--allocation-method static \
--version IPv6 \
--public-ip-prefix  IPv6PrefixWestUS
```

## <a name="next-steps"></a>Sonraki adımlar
- [IPv6 adres öneki](ipv6-public-ip-address-prefix.md)hakkında daha fazla bilgi edinin.
- [IPv6 adresleri](ipv6-overview.md)hakkında daha fazla bilgi edinin.
