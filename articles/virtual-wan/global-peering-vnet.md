---
title: Azure sanal WAN için küresel VNet eşlemesini yapılandırma | Microsoft Docs
description: Farklı bir bölgedeki VNet 'i sanal WAN hub 'ınıza bağlayın.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: 78c06ec1d93dcda5d171099943c287a9e4f43bc1
ms.sourcegitcommit: 4ac596f284a239a9b3d8ed42f89ed546290f4128
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84750555"
---
# <a name="configure-global-vnet-peering-cross-region-vnet-for-virtual-wan"></a>Sanal WAN için genel VNet eşlemesini (çapraz bölge VNet) yapılandırma

Farklı bir bölgedeki VNet 'i sanal WAN hub 'ınıza bağlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Bölgeler arası VNet (bağlı bileşen) başka bir sanal WAN hub 'ına bağlı değil. Bir bağlı bileşen yalnızca bir sanal hub 'a bağlanabilir.
* VNet (bağlı bileşen) bir sanal ağ geçidi içermez (örneğin, bir Azure VPN Gateway veya ExpressRoute sanal ağ geçidi). VNet bir sanal ağ geçidi içeriyorsa, bağlı olan VNet 'i hub 'a bağlamadan önce ağ geçidini kaldırmanız gerekir.

## <a name="register-this-feature"></a><a name="register"></a>Bu özelliği kaydedin

PowerShell kullanarak bu özelliğe kaydolabilirsiniz. Aşağıdaki örnekteki "deneyin" seçeneğini belirlerseniz, Azure Cloud-Shell açılır ve PowerShell cmdlet 'lerini bilgisayarınıza yerel olarak yüklemeniz gerekmez. Gerekirse, ' Select-AzSubscription-SubscriptionID <subid> ' cmdlet 'ini kullanarak abonelikleri değiştirebilirsiniz.

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName AllowCortexGlobalVnetPeering -ProviderNamespace Microsoft.Network
Register-AzResourceProvider -ProviderNamespace 'Microsoft.Network'
```

## <a name="verify-registration"></a><a name="verify"></a>Kaydı doğrula

```azurepowershell-interactive
Get-AzProviderFeature -FeatureName AllowCortexGlobalVnetPeering -ProviderNamespace Microsoft.Network
```

## <a name="connect-a-vnet-to-the-hub"></a><a name="hub"></a>Sanal ağı hub 'a bağlama

Bu adımda, hub 'ınız ve bölgeler arası VNet arasındaki eşleme bağlantısını oluşturursunuz. Bu adımları bağlanmak istediğiniz tüm sanal ağlar için tekrarlayın.

1. Sanal WAN'ınızın sayfasında **Sanal ağ bağlantıları**'na tıklayın.
2. Sanal ağ bağlantısı sayfasında **+Bağlantı ekle**'ye tıklayın.
3. **Bağlantı ekle** sayfasında aşağıdaki alanları doldurun:

    * **Bağlantı adı**: Bağlantınıza bir ad verin.
    * **Hub'lar**: Bu bağlantıyla ilişkilendirmek istediğiniz hub'ı seçin.
    * **Abonelik**: Aboneliği doğrulayın.
    * **Sanal ağ**: Bu hub'a bağlamak istediğiniz sanal ağı seçin. Sanal ağda önceden var olan bir sanal ağ geçidi bulunamaz.
4. Eşleme bağlantısını oluşturmak için **Tamam**’a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi edinmek için bkz. [sanal WAN genel bakış](virtual-wan-about.md).