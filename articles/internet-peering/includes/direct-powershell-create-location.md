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
ms.openlocfilehash: 86d768db7a31c634bdaca6c93f633c7bbaf10a65
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774295"
---
**Get-AzPeeringLocation** PowerShell cmdlet 'i, sonraki adımlarda kullanacağınız zorunlu parametre `Kind`bir eşleme konumları listesi döndürür:

```powershell
Get-AzPeeringLocation -Kind Direct
```

Doğrudan eşleme konumları aşağıdaki alanları içerir:
* PeeringLocation 
* Ülke
* PeeringDBFacilityId
* PeeringDBFacilityLink
* BandwidthOffers

[Peeringdb](https://w www.peeringdb.com)'ye başvurarak istenen eşleme özelliğinde mevcut olduğunu doğrulayın.

Bir doğrudan eşleme oluşturmak için Seattle 'ın eşleme konumu olarak nasıl kullanılacağını gösteren bir örnek aşağıda verilmiştir:

```powershell
$peeringLocations = Get-AzPeeringLocation -Kind Direct
$peeringLocation = $peeringLocations | where {$_.PeeringLocation -contains "Seattle"}
$peeringLocation

PeeringLocation       : Seattle
Address               : 2001 Sixth Avenue
Country               : US
PeeringDBFacilityId   : 71
PeeringDBFacilityLink : https://www.peeringdb.com/fac/71
BandwidthOffers       : {10Gbps, 100Gbps}
```