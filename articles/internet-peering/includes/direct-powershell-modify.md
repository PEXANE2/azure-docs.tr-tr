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
ms.openlocfilehash: 03c67ccf88a8c73fe04f062c6af9520115c185a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774269"
---
Bu bölümde, Doğrudan eşleme için aşağıdaki değişiklik işlemleri nasıl gerçekleştirilireaçıklanmaktadır:

* Doğrudan eşleme bağlantıları ekleme
* Doğrudan bakan bağlantıları kaldırma
* Etkin bağlantılarda bant genişliğini yükseltin veya düşürün.
* Etkin bağlantılarda IPv4/IPv6 oturumu ekleyin.
* Etkin bağlantılarda IPv4/IPv6 oturumunu kaldırın.

### <a name="add-direct-peering-connections"></a>Doğrudan eşleme bağlantıları ekleme

Aşağıdaki örnekte, varolan Doğrudan eşlemelere nasıl bağlantı eklendirilir

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

Bağlantıyı kaldırmak şu anda PowerShell'de desteklenmez. [Microsoft'a bakan kişi.](mailto:peeringexperience@microsoft.com)

<!--
```powershell
$directPeering.Connections.Remove($directPeering.Connections[0])

$directPeering | Update-AzPeering
```
-->

### <a name="upgrade-or-downgrade-bandwidth-on-active-connections"></a>Etkin bağlantılarda bant genişliğini yükseltme veya düşürme

Aşağıdaki örnekte, varolan doğrudan bağlantıya 10Gbps nasıl ekleyeceğiniz açıklanmaktadır.

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"
$directPeering.Connections[0].BandwidthInMbps  = 20000
$directPeering | Update-AzPeering

```

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4/IPv6 oturumu ekleyin.

Aşağıdaki örnekte, yalnızca IPv4 oturumuyla varolan doğrudan bağlantıya IPv6 oturumunun nasıl eklenilen rilmiştir. 

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"
$directPeering.Connections[0].BGPSession.SessionPrefixv6 = "fe01::3e:0/127"
$directPeering | Update-AzPeering

```

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4/IPv6 oturumunu kaldırın.

Varolan bir bağlantıdan bir IPv4/IPv6 oturumunun kaldırılması şu anda PowerShell'de desteklenmez. [Microsoft'a bakan kişi.](mailto:peeringexperience@microsoft.com)