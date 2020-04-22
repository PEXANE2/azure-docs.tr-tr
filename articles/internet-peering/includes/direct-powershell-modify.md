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
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680749"
---
Bu bölümde, Doğrudan eşleme için aşağıdaki değişiklik işlemleri nasıl gerçekleştirilireaçıklanmaktadır:

* Doğrudan bakan bağlantılar ekleyin.
* Doğrudan bakan bağlantıları kaldırın.
* Etkin bağlantılarda bant genişliğini yükseltin veya düşürün.
* Etkin bağlantılara IPv4 veya IPv6 oturumları ekleyin.
* Etkin bağlantılardaki IPv4 veya IPv6 oturumlarını kaldırın.

### <a name="add-direct-peering-connections"></a>Doğrudan eşleme bağlantıları ekleme

Bu örnekte, varolan Doğrudan eşlemelere bağlantı nasıl eklenilen açıklanmaktadır.

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

### <a name="remove-direct-peering-connections"></a>Doğrudan bakan bağlantıları kaldırma

Bağlantıyı kaldırmak şu anda PowerShell'de desteklenmez. Daha fazla bilgi için [Microsoft'a başvurun.](mailto:peeringexperience@microsoft.com)

<!--
```powershell
$directPeering.Connections.Remove($directPeering.Connections[0])

$directPeering | Update-AzPeering
```
-->

### <a name="upgrade-or-downgrade-bandwidth-on-active-connections"></a>Etkin bağlantılarda bant genişliğini yükseltme veya düşürme

Bu örnekte, varolan bir Doğrudan bağlantıya 10 Gbps nasıl ekleyeceğiniz açıklanmaktadır.

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"
$directPeering.Connections[0].BandwidthInMbps  = 20000
$directPeering | Update-AzPeering

```

### <a name="add-ipv4-or-ipv6-sessions-on-active-connections"></a>Etkin bağlantılarda IPv4 veya IPv6 oturumları ekleme

Bu örnekte, yalnızca bir IPv4 oturumu olan varolan bir Doğrudan bağlantıya IPv6 oturumunun nasıl eklenilen bir şekilde eklenilen açıklanmaktadır. 

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"
$directPeering.Connections[0].BGPSession.SessionPrefixv6 = "fe01::3e:0/127"
$directPeering | Update-AzPeering

```

### <a name="remove-ipv4-or-ipv6-sessions-on-active-connections"></a>Etkin bağlantılardaki IPv4 veya IPv6 oturumlarını kaldırma

Varolan bir bağlantıdan bir IPv4 veya IPv6 oturumunun kaldırılması şu anda PowerShell'de desteklenmez. Daha fazla bilgi için [Microsoft'a başvurun.](mailto:peeringexperience@microsoft.com)