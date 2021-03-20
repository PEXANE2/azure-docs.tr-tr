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
ms.openlocfilehash: dbaa0b5fc87cb5393b323b8a9b7a38b72efe9518
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "81680817"
---
**Get-AzPeeringLocation** PowerShell cmdlet 'i, sonraki adımlarda kullanacağınız zorunlu parametreye sahip eşleme konumları listesini döndürür `Kind` .

```powershell
Get-AzPeeringLocation -Kind Direct
```

Doğrudan eşleme konumları aşağıdaki alanları içerir:
* PeeringLocation 
* Ülke
* PeeringDBFacilityId
* PeeringDBFacilityLink
* BandwidthOffers

[Peeringdb](https://wwww.peeringdb.com)'ye başvurarak istenen eşleme özelliğinde mevcut olduğunuzu doğrulayın.

Bu örnek, doğrudan eşleme oluşturmak için Seattle 'ın eşleme konumu olarak nasıl kullanılacağını gösterir.

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