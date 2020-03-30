---
title: include dosyası
titleSuffix: Azure
description: include dosyası
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 0014b67443797f45de51ec1bc459f71bde55cdc9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774438"
---
Aşağıdaki örnek, Seattle'daki Equinix Internet Exchange'de exchange bağlantısının nasıl oluşturulabildiğini gösterir. Farklı bir sağlayıcı ve farklı ayarlar kullanıyorsanız, isteğinizi yaparken bu bilgileri değiştirin.

PowerShell cmdlet **New-AzPeeringExchangeConnectionObject'i** kullanarak powershell bağlantı nesnelerini yeni bir eşleme isteği oluşturmak için kullanılacaktır:

Aşağıda bir Exchange Bağlantısı oluşturmak için bir örnek:

```powershell
$connection1 = New-AzPeeringExchangeConnectionObject `
    -PeeringDBFacilityId $exchangeLocation[1].PeeringDBFacilityId `
    -PeerSessionIPv4Address 198.32.134.22 `
    -PeerSessionIPv6Address  2001:504:12::22 `
    -MaxPrefixesAdvertisedIPv4 2000 `
    -MaxPrefixesAdvertisedIPv6 2000 `
```

Belirli bir eşleme konumunda artıklığa ihtiyaç duymanız durumunda başka bir bağlantı oluşturun:

```powershell
$connection2 = New-AzPeeringExchangeConnectionObject `
    -PeeringDBFacilityId $exchangeLocation[1].PeeringDBFacilityId `
    -PeerSessionIPv4Address 198.32.134.23 `
    -PeerSessionIPv6Address  2001:504:12::23 `
    -MaxPrefixesAdvertisedIPv4 2000 `
    -MaxPrefixesAdvertisedIPv6 2000 `
```

PowerShell cmdlet **New-AzPeering** yeni bir Exchange bakan oluşturmak için kullanılabilir:

```powershell
$asn = Get-AzPeerAsn
New-AzPeering `
    -Name "SeattleExchangePeering" `
    -ResourceGroupName "PeeringResourceGroup" `
    -PeerAsnResourceId $asn.Id `
    -PeeringLocation  $exchangeLocation[1].PeeringLocation `
    -ExchangeConnection $connection1[, $connection2]
```
&nbsp;

Aşağıda, istek tek bir bağlantı kullanılarak yürütüldüğünde örnek bir yanıt verilmiştir:

```powershell

Name              : SeattleExchangePeering
Sku.Name          : Basic_Exchange_Free
Kind              : Exchange
Connections       : {11}
PeerAsn.Id        : /subscriptions/{subscriptionId}/providers/Microsoft.Peering/peerAsns/{peerAsnName}
PeeringLocation   : Seattle
ProvisioningState : Succeeded
Location          : West US 2
Id                : /subscriptions/{subscriptionId}/resourceGroups/PeeringResourceGroup/providers/Microsoft.Peering/peerings/SeattleExchangePeering
Type              : Microsoft.Peering/peerings
Tags              : {}

```

> [!IMPORTANT]
> Microsoft, istenen eşlemi sağlamaya `ConnectionState` başlar ve bu ilerlemeyi yansıtır.
> İlgili adımları sağlamak için [Exchange eşleme gözden geçirme](../walkthrough-exchange-all.md) belgesine bakın.

Aşağıda gösterildiği gibi ConnectionState'i kontrol edebilirsiniz:

```powershell

$peering = Get-AzPeering -Name "SeattleExchangePeering" -ResourceGroupName "PeeringResourceGroup"
$peering.Connections

PeeringDBFacilityId         : 11
PeerSessionIPv4Address      : 198.32.134.22
PeerSessionIPv6Address      : 2001:504:12::22
SessionStateV4              : PendingAdd
SessionStateV6              : PendingAdd
MaxPrefixesAdvertisedV4     : 2000
MaxPrefixesAdvertisedV6     : 2000
MicrosoftSessionIPv4Address : 198.32.134.152
MicrosoftSessionIPv4Address : 2001:504:12::15
Md5AuthenticationKey        :

```