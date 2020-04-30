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
ms.openlocfilehash: 2ebf5d574b4d185953ab0f7984648c440d6f107e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81678597"
---
**Get-AzPeeringLocation** PowerShell cmdlet 'i, sonraki adımlarda kullanacağınız zorunlu parametreye `Kind`sahip eşleme konumları listesini döndürür.

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

[Peeringdb](https://wwww.peeringdb.com)'ye başvurarak istenen eşleme özelliğinde mevcut olduğunu doğrulayın.

Bu örnek, eşleme oluşturmak için Seattle 'ın eşleme konumu olarak nasıl kullanılacağını gösterir.

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