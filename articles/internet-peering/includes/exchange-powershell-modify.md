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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81678619"
---
Exchange eşlemesi için aşağıdaki değişiklik işlemleri desteklenir:
* Exchange eşleme bağlantıları ekleyin.
* Exchange eşleme bağlantılarını kaldırın.
* Etkin bağlantılara IPv4 veya IPv6 oturumu ekleyin.
* Etkin bağlantılardaki IPv4 veya IPv6 oturumunu kaldırın.


### <a name="add-exchange-peering-connections"></a>Exchange eşleme bağlantıları Ekle

Bu örnek, mevcut bir Exchange eşlemesine bağlantıların nasıl ekleneceğini açıklar.

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

Bu örnek, mevcut bir Exchange eşlemesinin bağlantılarının nasıl kaldırılacağını açıklar.

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

### <a name="add-an-ipv4-or-ipv6-session-on-active-connections"></a>Etkin bağlantılara IPv4 veya IPv6 oturumu ekleme

Bu örnek, mevcut bir Exchange bağlantısına bir IPv6 oturumunun nasıl ekleneceğini açıklar.

```powershell

$exchangePeering = Get-AzPeering -Name "SeattleExchangePeering" -ResourceGroupName "PeeringResourceGroup"

$exchangePeering.Connections[0].BgpSession.PeerSessionIPv6Address = "2001:504:12::34"

$exchangePeering | Update-AzPeering

```

### <a name="remove-an-ipv4-or-ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4 veya IPv6 oturumu kaldırma

Mevcut bir bağlantıdan IPv4 veya IPv6 oturumunun kaldırılması Şu anda PowerShell 'de desteklenmemektedir. Daha fazla bilgi için [Microsoft eşlemesi](mailto:peeringexperience@microsoft.com)'ne başvurun.