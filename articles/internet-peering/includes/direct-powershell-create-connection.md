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
ms.openlocfilehash: e1cb1bec6273fa79315b9439bec1412622ebfe28
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774308"
---
Aşağıdaki örnekte Seattle 'da 10 Gbps doğrudan eşlemesinin nasıl oluşturulacağı gösterilmektedir.

Yeni eşleme isteğinde kullanılacak DirectConnections oluşturmak için **New-AzPeeringDirectConnectionObject** PowerShell cmdlet 'ini kullanın.

Aşağıda, bir DirectConnection oluşturma örneği verilmiştir:

```powershell
$connection1 = New-AzPeeringDirectConnectionObject `
    -PeeringDBFacilityId $peeringLocation[0].PeeringDBFacilityId `
    -SessionPrefixV4 10.21.31.0/31 `
    -SessionPrefixV6 fe01::3e:0/127 `
    -MaxPrefixesAdvertisedIPv4 1000 `
    -MaxPrefixesAdvertisedIPv6 100 `
    -BandwidthInMbps 10000
```

> [!NOTE]
> Yukarıdaki örnekteki $peeringLocation [<index>] için <index>, tercih ettiğiniz eşleme konumuna karşılık gelmelidir

Belirtilen eşleme konumunda artıklık gerekli olduğunda başka bir bağlantı oluşturun:

```powershell
$connection2 = New-AzPeeringDirectConnectionObject `
    -PeeringDBFacilityId $peeringLocation[0].PeeringDBFacilityId `
    -SessionPrefixV4 10.21.33.0/31 `
    -SessionPrefixV6 fe01::3f:0/127 `
    -MaxPrefixesAdvertisedIPv4 1000 `
    -MaxPrefixesAdvertisedIPv6 100 `
    -BandwidthInMbps 10000
```

Yeni bir doğrudan eşleme oluşturmak için **New-Azeşleme** PowerShell cmdlet 'ini kullanın. Bu komut, aşağıda gösterildiği gibi alınabilecek ASN kaynak KIMLIĞI gerektirir.


```powershell
$asn = Get-AzPeerAsn
New-AzPeering `
    -Name "SeattleDirectPeering" `
    -ResourceGroupName "PeeringResourceGroup" `
    -PeeringLocation  $peeringLocation[0].PeeringLocation `
    -PeerAsnResourceId $asn.Id `
    -DirectConnection $connection1 [, $connection2]
```
&nbsp;

İstek başarıyla işlendiğinde örnek bir yanıt aşağıda verilmiştir:

```powershell

    Name                 : SeattleDirectPeering
    Sku.Name             : Basic_Direct_Free
    Kind                 : Direct
    Connections          : 71
    PeerAsn.Id           : /subscriptions/{subscriptionId}/providers/Microsoft.Peering/peerAsns/SeattleDirectPeering
    UseForPeeringService : False
    PeeringLocation      : Seattle
    ProvisioningState    : Succeeded
    Location             : centralus
    Id                   : /subscriptions/{subscriptionId}/resourceGroups/PeeringResourceGroup/providers/Microsoft.Peering/peerings/SeattleDirectPeering
    Type                 : Microsoft.Peering/peerings
    Tags                 : {}

```
Yukarıdaki çıktıda **{SubscriptionID}** yerine gerçek abonelik kimliği görüntülenir.
