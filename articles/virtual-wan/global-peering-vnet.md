---
title: Azure Virtual WAN için Global VNet eşlemeyapılandırma | Microsoft Dokümanlar
description: Farklı bir bölgedeki bir VNet'i Sanal WAN hub'ınıza bağlayın.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: 340472f84d2dd2c4f46d180992745a57e8ad1884
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73588232"
---
# <a name="configure-global-vnet-peering-cross-region-vnet-for-virtual-wan"></a>Virtual WAN için Global VNet eşleme (bölgeler arası VNet) yapılandırma

Farklı bir bölgedeki bir VNet'i Virtual WAN hub'ınıza bağlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Bölgeler arası VNet (spoke) başka bir Virtual WAN hub'ına bağlı değildir. Bir spoke yalnızca bir sanal hub'a bağlanabilir.
* VNet (spoke) sanal ağ ağ geçidi (örneğin, bir Azure VPN Ağ Geçidi veya ExpressRoute sanal ağ ağ ağ geçidi) içermez. VNet sanal ağ ağ geçidi içeriyorsa, bağlı VNet'i hub'a bağlamadan önce ağ geçidini kaldırmanız gerekir.

## <a name="register-this-feature"></a><a name="register"></a>Bu özelliği kaydedin

PowerShell'i kullanarak bu özellik için kaydolabilirsiniz. Aşağıdaki örnekten "Try It" seçeneğini belirlerseniz, Azure Cloud-Shell açılır ve PowerShell cmdletlerini bilgisayarınıza yerel olarak yüklemeniz gerekmez. Gerekirse, 'Select-AzSubscription -SubscriptionId <subid>' cmdlet'i kullanarak abonelikleri değiştirebilirsiniz.

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName AllowCortexGlobalVnetPeering -ProviderNamespace Microsoft.Network
Register-AzResourceProvider -ProviderNamespace 'Microsoft.Network'
```

## <a name="verify-registration"></a><a name="verify"></a>Kaydı doğrulama

```azurepowershell-interactive
Get-AzProviderFeature -FeatureName AllowCortexGlobalVnetPeering -ProviderNamespace Microsoft.Network
```

## <a name="connect-a-vnet-to-the-hub"></a><a name="hub"></a>Hub'a bir VNet bağlama

Bu adımda, hub'ınızla bölgeler arası VNet arasındaki eşleme bağlantısını oluşturursunuz. Bu adımları bağlanmak istediğiniz tüm sanal ağlar için tekrarlayın.

1. Sanal WAN'ınızın sayfasında **Sanal ağ bağlantıları**'na tıklayın.
2. Sanal ağ bağlantısı sayfasında **+Bağlantı ekle**'ye tıklayın.
3. **Bağlantı ekle** sayfasında aşağıdaki alanları doldurun:

    * **Bağlantı adı**: Bağlantınıza bir ad verin.
    * **Hub'lar**: Bu bağlantıyla ilişkilendirmek istediğiniz hub'ı seçin.
    * **Abonelik**: Aboneliği doğrulayın.
    * **Sanal ağ**: Bu hub'a bağlamak istediğiniz sanal ağı seçin. Sanal ağda önceden var olan bir sanal ağ geçidi bulunamaz.
4. Eşleme bağlantısını oluşturmak için **Tamam**’a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Virtual WAN hakkında daha fazla bilgi edinmek için [Virtual WAN Genel Bakış'a](virtual-wan-about.md)bakın.