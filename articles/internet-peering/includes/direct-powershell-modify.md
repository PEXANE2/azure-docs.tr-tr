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
ms.openlocfilehash: b2609a069872ec55ac9068fadcbb3f312d68a630
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81680749"
---
Bu bölümde, doğrudan eşleme için aşağıdaki değişiklik işlemlerinin nasıl gerçekleştirileceği açıklanmaktadır:

* Doğrudan eşleme bağlantıları ekleyin.
* Doğrudan eşleme bağlantılarını kaldırın.
* Etkin bağlantılarda bant genişliğini yükseltin veya alçaltma.
* Etkin bağlantılara IPv4 veya IPv6 oturumları ekleyin.
* Etkin bağlantılardaki IPv4 veya IPv6 oturumlarını kaldırın.

### <a name="add-direct-peering-connections"></a>Doğrudan eşleme bağlantıları Ekle

Bu örnek, mevcut doğrudan eşlemeye bağlantıların nasıl ekleneceğini açıklar.

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"

$connection = New-AzPeeringDirectConnection `
    -PeeringDBFacilityId $peeringLocation.PeeringDBFacilityId `
    -SessionPrefixV4 "10.22.31.0/31" `
    -SessionPrefixV6 "fe02::3e:0/127" `
    -MaxPrefixesAdvertisedIPv4 1000 `
    -MaxPrefixesAdvertisedIPv6 100 `
    -BandwidthInMbps 10000

$directPeering.Connections.Add($connection)

$directPeering | Update-AzPeering
```

### <a name="remove-direct-peering-connections"></a>Doğrudan eşleme bağlantılarını kaldır

Bağlantı kaldırma, şu anda PowerShell 'de desteklenmemektedir. Daha fazla bilgi için [Microsoft eşlemesi](mailto:peeringexperience@microsoft.com)'ne başvurun.

<!--
```powershell
$directPeering.Connections.Remove($directPeering.Connections[0])

$directPeering | Update-AzPeering
```
-->

### <a name="upgrade-or-downgrade-bandwidth-on-active-connections"></a>Etkin bağlantılarda bant genişliğini yükseltme veya düşürme

Bu örnek, mevcut bir doğrudan bağlantıya 10 Gbps 'nin nasıl ekleneceğini açıklar.

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"
$directPeering.Connections[0].BandwidthInMbps  = 20000
$directPeering | Update-AzPeering

```

### <a name="add-ipv4-or-ipv6-sessions-on-active-connections"></a>Etkin bağlantılara IPv4 veya IPv6 oturumları ekleme

Bu örnek, mevcut bir doğrudan bağlantı üzerinde yalnızca bir IPv4 oturumuyla bir IPv6 oturumunun nasıl ekleneceğini açıklar. 

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"
$directPeering.Connections[0].BGPSession.SessionPrefixv6 = "fe01::3e:0/127"
$directPeering | Update-AzPeering

```

### <a name="remove-ipv4-or-ipv6-sessions-on-active-connections"></a>Etkin bağlantılardaki IPv4 veya IPv6 oturumlarını kaldırma

Mevcut bir bağlantıdan IPv4 veya IPv6 oturumunun kaldırılması Şu anda PowerShell 'de desteklenmemektedir. Daha fazla bilgi için [Microsoft eşlemesi](mailto:peeringexperience@microsoft.com)'ne başvurun.