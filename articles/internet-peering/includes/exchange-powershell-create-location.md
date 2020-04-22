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
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678597"
---
PowerShell cmdlet **Get-AzPeeringLocation,** daha sonraki adımlarda kullanacağınız `Kind`zorunlu parametreye sahip görünen konumların listesini döndürür.

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

[PeeringDB'ye](https://wwww.peeringdb.com)atıfta bulunarak istediğiniz eşleme tesisinde bulunduğunuzu doğrulayın.

Bu örnek, bir eşoluşturmak için Seattle'ın eşleme konumu olarak nasıl kullanılacağını gösterir.

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