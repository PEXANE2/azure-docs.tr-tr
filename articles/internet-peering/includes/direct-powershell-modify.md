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
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774269"
---
Bu bölümde, doğrudan eşleme için aşağıdaki değişiklik işlemlerinin nasıl gerçekleştirileceği açıklanmaktadır:

* Doğrudan eşleme bağlantıları Ekle
* Doğrudan eşleme bağlantılarını kaldır
* Etkin bağlantılarda bant genişliğini yükseltin veya alçaltma.
* Etkin bağlantılara IPv4/IPv6 oturumu ekleyin.
* Etkin bağlantılarda IPv4/IPv6 oturumunu kaldırın.

### <a name="add-direct-peering-connections"></a>Doğrudan eşleme bağlantıları Ekle

Aşağıdaki örnek, mevcut doğrudan eşlemeye nasıl bağlantı ekleneceğini açıklar

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

Bir bağlantının kaldırılması, PowerShell 'de Şu anda desteklenmiyor. [Microsoft eşlemesiyle](mailto:peeringexperience@microsoft.com)iletişim kurun.

<!--
```powershell
$directPeering.Connections.Remove($directPeering.Connections[0])

$directPeering | Update-AzPeering
```
-->

### <a name="upgrade-or-downgrade-bandwidth-on-active-connections"></a>Etkin bağlantılarda bant genişliğini yükseltme veya düşürme

Aşağıdaki örnek, mevcut doğrudan bağlantıya 10Gbps ekleme işlemini açıklar.

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"
$directPeering.Connections[0].BandwidthInMbps  = 20000
$directPeering | Update-AzPeering

```

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılara IPv4/IPv6 oturumu ekleyin.

Aşağıdaki örnek, yalnızca IPv4 oturumuna sahip mevcut bir doğrudan bağlantı üzerinde IPv6 oturumunun nasıl ekleneceğini açıklar. 

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"
$directPeering.Connections[0].BGPSession.SessionPrefixv6 = "fe01::3e:0/127"
$directPeering | Update-AzPeering

```

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Etkin bağlantılarda IPv4/IPv6 oturumunu kaldırın.

Mevcut bir bağlantıdan IPv4/IPv6 oturumunun kaldırılması, PowerShell 'de Şu anda desteklenmiyor. [Microsoft eşlemesiyle](mailto:peeringexperience@microsoft.com)iletişim kurun.