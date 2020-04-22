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
ms.openlocfilehash: 12d169697a35af446392843eb57e6ec3a5508e45
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678619"
---
Exchange eşleme için aşağıdaki değişiklik işlemleri desteklenir:
* Exchange bakan bağlantıları ekleyin.
* Exchange bakan bağlantılarını kaldırın.
* Etkin bağlantılara bir IPv4 veya IPv6 oturumu ekleyin.
* Etkin bağlantılardaki Bir IPv4 veya IPv6 oturumunu kaldırın.


### <a name="add-exchange-peering-connections"></a>Exchange eşleme bağlantıları ekleme

Bu örnekte, varolan bir Exchange eşlenesine nasıl bağlantı ekleyeceğiniz açıklanmaktadır.

```powershell

$exchangePeering = Get-AzPeering -Name "SeattleExchangePeering" -ResourceGroupName "PeeringResourceGroup"

$connection = New-AzPeeringExchangeConnectionObject `
    -PeeringDBFacilityId $exchangeLocation[1].PeeringDBFacilityId `
    -PeerSessionIPv4Address 198.32.134.22 `
    -PeerSessionIPv6Address  2001:504:12::22 `
    -MaxPrefixesAdvertisedIPv4 2000 `
    -MaxPrefixesAdvertisedIPv6 2000 `

$exchangePeering.Connections.Add($connection)

$exchangePeering | Update-AzPeering

```

### <a name="remove-exchange-peering-connections"></a>Exchange eşleme bağlantılarını kaldırma

Bu örnekte, varolan bir Exchange eşlenesine bağlantıların nasıl kaldırılılabildiğini açıklanmaktadır.

```powershell

$exchangePeering = Get-AzPeering -Name "SeattleExchangePeering" -ResourceGroupName "PeeringResourceGroup"

```

Tüm bağlantıları görüntüleyin ve kaldırmak istediğiniz bağlantıyı seçin. 

```powershell

$exchangePeering

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

Aşağıdaki komutta, kaldırmak istediğiniz bağlantı için dizin numarasını 0 yerine girin.

```powershell

$connection = $exchangePeering.Connections[0]

$exchangePeering.Connections.Remove($connection)

$exchangePeering | Update-AzPeering

```

### <a name="add-an-ipv4-or-ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4 veya IPv6 oturumu ekleme

Bu örnekte, varolan bir Exchange bağlantısına IPv6 oturumu nasıl ekleyeceğiniz açıklanmaktadır.

```powershell

$exchangePeering = Get-AzPeering -Name "SeattleExchangePeering" -ResourceGroupName "PeeringResourceGroup"

$exchangePeering.Connections[0].BgpSession.PeerSessionIPv6Address = "2001:504:12::34"

$exchangePeering | Update-AzPeering

```

### <a name="remove-an-ipv4-or-ipv6-session-on-active-connections"></a>Etkin bağlantılarda Bir IPv4 veya IPv6 oturumunu kaldırma

Varolan bir bağlantıdan bir IPv4 veya IPv6 oturumunun kaldırılması şu anda PowerShell'de desteklenmez. Daha fazla bilgi için [Microsoft'a başvurun.](mailto:peeringexperience@microsoft.com)