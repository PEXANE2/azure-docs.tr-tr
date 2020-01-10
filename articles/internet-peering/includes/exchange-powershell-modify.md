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
ms.openlocfilehash: 86e1a9cce1864ce259fe07b6949be2e32be242a8
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774425"
---
Exchange eşlemesi için aşağıdaki değişiklik işlemi destekleniyor
1. Exchange eşleme bağlantıları Ekle
1. Exchange eşleme bağlantılarını kaldır
1. Etkin bağlantılara IPv4/IPv6 oturumu ekleyin.
1. Etkin bağlantılarda IPv4/IPv6 oturumunu kaldırın.


### <a name="add-exchange-peering-connections"></a>Exchange eşleme bağlantıları Ekle

Aşağıdaki örnek, mevcut Exchange eşlemesine nasıl bağlantı ekleneceğini açıklar

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

### <a name="remove-exchange-peering-connections"></a>Exchange eşleme bağlantılarını kaldır

Aşağıdaki örnek, var olan Exchange eşlemesinin bağlantılarının nasıl kaldırılacağını açıklar

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

Aşağıdaki komutta, 0 yerine, kaldırmak istediğiniz bağlantının Dizin numarasını girin.

```powershell

$connection = $exchangePeering.Connections[0]

$exchangePeering.Connections.Remove($connection)

$exchangePeering | Update-AzPeering

```

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılara IPv4/IPv6 oturumu ekleme

Aşağıdaki örnek, mevcut Exchange bağlantısına IPv6 oturumunun nasıl ekleneceğini açıklar.

```powershell

$exchangePeering = Get-AzPeering -Name "SeattleExchangePeering" -ResourceGroupName "PeeringResourceGroup"

$exchangePeering.Connections[0].BgpSession.PeerSessionIPv6Address = "2001:504:12::34"

$exchangePeering | Update-AzPeering

```

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4/IPv6 oturumunu kaldır

Mevcut bir bağlantıdan IPv4/IPv6 oturumunun kaldırılması, PowerShell 'de Şu anda desteklenmiyor. [Microsoft eşlemesiyle](mailto:peeringexperience@microsoft.com)iletişim kurun.