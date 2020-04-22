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
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680817"
---
PowerShell cmdlet **Get-AzPeeringLocation,** daha sonraki adımlarda kullanacağınız `Kind`zorunlu parametreye sahip görünen konumların listesini döndürür.

```powershell
Get-AzPeeringLocation -Kind Direct
```

Doğrudan eşleme konumları aşağıdaki alanları içerir:
* PeeringLocation 
* Ülke
* PeeringDBFacilityId
* PeeringDBFacilityLink
* Bant GenişliğiTeklifler

[PeeringDB'ye](https://wwww.peeringdb.com)atıfta bulunarak istediğiniz eşleme tesisinde bulunduğunuzu doğrulayın.

Bu örnek, Doğrudan bir bakış oluşturmak için Seattle'ın eşleme konumu olarak nasıl kullanılacağını gösterir.

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