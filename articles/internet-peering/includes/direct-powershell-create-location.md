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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774295"
---
PowerShell cmdlet **Get-AzPeeringLocation,** daha sonraki adımlarda kullanacağınız zorunlu parametreye `Kind`sahip görünen konumların listesini döndürür:

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

Aşağıda, Doğrudan bir eşleme oluşturmak için Seattle'ın eşleme konumu olarak nasıl kullanılacağını gösteren bir örnek verilmiştir:

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