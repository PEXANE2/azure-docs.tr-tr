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
ms.openlocfilehash: 6f5d2dc30ac0f6316587fa0836b87cbd4efc0a8b
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774399"
---
**Get-AzPeeringLocation** PowerShell cmdlet 'i, sonraki adımlarda kullanacağınız zorunlu parametre `Kind`bir eşleme konumları listesi döndürür:

```powershell
Get-AzPeeringLocation -Kind "Exchange"
```

Exchange eşleme konumları aşağıdaki alanları içerir:
* ExchangeName
* PeeringLocation
* Ülke
* PeeringDBFacilityId
* PeeringDBFacilityLink
* MicrosoftIPv4Address
* MicrosoftIPv6Address

[Peeringdb](https://w www.peeringdb.com)'ye başvurarak istenen eşleme özelliğinde mevcut olduğunu doğrulayın.

Bir eşleme oluşturmak için Seattle 'ın eşleme konumu olarak nasıl kullanılacağını gösteren bir örnek aşağıda verilmiştir:

```powershell
$exchangeLocations = Get-AzPeeringLocation -Kind Exchange
$exchangeLocation = $exchangeLocations | where {$_.PeeringLocation -eq "Seattle"}

#check the location metadata
$exchangeLocation

ExchangeName          : Columbia IX
PeeringLocation       : Seattle
Country               : US
PeeringDBFacilityId   : 99999
PeeringDBFacilityLink : https://www.peeringdb.com/ix/99999
MicrosoftIPv4Address  : 10.12.97.129
MicrosoftIPv6Address  :

ExchangeName          : Equinix Seattle
PeeringLocation       : Seattle
Country               : US
PeeringDBFacilityId   : 11
PeeringDBFacilityLink : https://www.peeringdb.com/ix/11
MicrosoftIPv4Address  : 198.32.134.152
MicrosoftIPv6Address  : 2001:504:12::15

...

```