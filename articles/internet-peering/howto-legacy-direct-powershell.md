---
title: PowerShell'i kullanarak eski doğrudan bir Azure kaynağına doğrudan bakış dönüştürme
titleSuffix: Azure
description: PowerShell'i kullanarak eski doğrudan bir Azure kaynağına doğrudan bakış dönüştürme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 5d2a8c910c9e384e137785bc1cd491bc85c7e7a8
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678469"
---
# <a name="convert-a-legacy-direct-peering-to-an-azure-resource-by-using-powershell"></a>PowerShell'i kullanarak eski doğrudan bir Azure kaynağına doğrudan bakış dönüştürme

Bu makalede, PowerShell cmdlets kullanarak varolan bir eski Doğrudan bir Azure kaynağına bakan dönüştürmek için nasıl açıklanmaktadır.

İsterseniz, Azure [portalını](howto-legacy-direct-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [ön koşulları](prerequisites.md) ve Doğrudan bakan [gözden geçirin.](walkthrough-direct-all.md)

### <a name="work-with-azure-powershell"></a>Azure PowerShell ile çalışın
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="convert-a-legacy-direct-peering-to-an-azure-resource"></a>Eski bir Azure kaynağına doğrudan bakış dönüştürme

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-a-legacy-direct-peering-for-conversion"></a><a name= get></a>Dönüşüm için bir eski Doğrudan bakış alın
Bu örnek, Seattle'a bakan konuma doğrudan bakan bir eskinin nasıl alındığını gösterir.

```powershell
$legacyPeering = Get-AzLegacyPeering `
    -Kind Direct -PeeringLocation "Seattle"
$legacyPeering
```

Aşağıda bir yanıt örneği yer alır:
```powershell
Name                       :
Sku                        : Basic_Direct_Free
Kind                       : Direct
PeeringLocation            : Seattle
UseForPeeringService       : False
PeerAsn.Id                 :
Connection                 : ------------------------
PeeringDBFacilityId        : 71
SessionPrefixIPv4          : 4.71.156.72/30
PeerSessionIPv4Address     : 4.71.156.73
MicrosoftIPv4Address       : 4.71.156.74
SessionStateV4             : Established
MaxPrefixesAdvertisedV4    : 20000
SessionPrefixIPv6          : 2001:1900:2100::1e10/126
MaxPrefixesAdvertisedV6    : 2000
ConnectionState            : Active
BandwidthInMbps            : 0
ProvisionedBandwidthInMbps : 20000
Connection                 : ------------------------
PeeringDBFacilityId        : 71
SessionPrefixIPv4          : 4.68.70.140/30
PeerSessionIPv4Address     : 4.68.70.141
MicrosoftIPv4Address       : 4.68.70.142
SessionStateV4             : Established
MaxPrefixesAdvertisedV4    : 20000
SessionPrefixIPv6          : 2001:1900:4:3::cc/126
PeerSessionIPv6Address     : 2001:1900:4:3::cd
MicrosoftIPv6Address       : 2001:1900:4:3::ce
SessionStateV6             : Established
MaxPrefixesAdvertisedV6    : 2000
ConnectionState            : Active
BandwidthInMbps            : 0
ProvisionedBandwidthInMbps : 20000
ProvisioningState          : Succeeded
```

### <a name="convert-a-legacy-direct-peering"></a>Eskiye doğrudan bakmayı dönüştürme

&nbsp;
> [!IMPORTANT]
> Eski bir azure kaynağına baktığınızda, değişiklikler desteklenmez. &nbsp;

Eski bir Azure kaynağına doğrudan bakış dönüştürmeyapmak için bu komutu kullanın:

```powershell
$legacyPeering[0] | New-AzPeering `
    -Name "SeattleDirectPeering" `
    -ResourceGroupName "PeeringResourceGroup" `

```

Aşağıda bir yanıt örneği yer alır:

```powershell
Name                 : SeattleDirectPeering
Sku.Name             : Basic_Direct_Free
Kind                 : Direct
Connections          : {11, 11}
PeerAsn.Id           : /subscriptions/{subscriptionId}/providers/Microsoft.Peering/peerAsns/{asnNumber}
UseForPeeringService : False
PeeringLocation      : Seattle
ProvisioningState    : Succeeded
Location             : centralus
Id                   : /subscriptions/{subscriptionId}/resourceGroups/PeeringResourceGroup/providers/Microsoft.Peering/peerings/SeattleDirectPeering
Type                 : Microsoft.Peering/peerings
Tags                 : {}
```

## <a name="additional-resources"></a>Ek kaynaklar
Bu komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```powershell
Get-Help Get-AzPeering -detailed
```

Daha fazla bilgi için [Internet'e bakan SSS'lere](faqs.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell'i kullanarak Doğrudan bir eşleme oluşturma veya değiştirme](howto-direct-powershell.md)
