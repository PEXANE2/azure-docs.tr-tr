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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774308"
---
Aşağıdaki örnek, Seattle'da 10Gbps Direct'in nasıl oluşturulabildiğini gösterir.

PowerShell cmdlet **New-AzPeeringDirectConnectionObject'i** kullanarak yeni eşleme isteğinde kullanılacak DirectConnections'ı kullanın.

Aşağıda directconnection oluşturmak için bir örnek:

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
> <index> Yukarıdaki örnekte<index>$peeringLocation için seçtiğiniz emsal konumuna karşılık olmalıdır

Belirli bir eşleme konumunda artıklığa ihtiyaç duymanız durumunda başka bir bağlantı oluşturun:

```powershell
$connection2 = New-AzPeeringDirectConnectionObject `
    -PeeringDBFacilityId $peeringLocation[0].PeeringDBFacilityId `
    -SessionPrefixV4 10.21.33.0/31 `
    -SessionPrefixV6 fe01::3f:0/127 `
    -MaxPrefixesAdvertisedIPv4 1000 `
    -MaxPrefixesAdvertisedIPv6 100 `
    -BandwidthInMbps 10000
```

Yeni bir Doğrudan eşleme oluşturmak için PowerShell cmdlet **New-AzPeering'i** kullanın. Bu komut, aşağıda gösterildiği gibi alınabilir ASN kaynak kimliği gerektirir.


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

İstek başarılı bir şekilde işlendiğinde örnek bir yanıt aşağıda verilmiştir:

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
Yukarıdaki çıktıda **{subscriptionId}** yerine gerçek abonelik kimliğinin görüntüleneceğini unutmayın.
