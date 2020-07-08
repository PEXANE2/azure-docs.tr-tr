---
author: rothja
ms.service: azure-resource-manager
ms.topic: include
ms.date: 2/14/2020
ms.author: rohink
ms.openlocfilehash: 434b79a2b178defd9543e1d3ad087bb5282cb287
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85805524"
---
**Genel DNS bölgeleri**

| Kaynak | Sınır |
| --- | --- |
| Abonelik başına genel DNS Bölgeleri |250 <sup>1</sup> |
| Genel DNS bölgesi başına kayıt kümeleri |10.000 <sup>1</sup> |
| Genel DNS bölgesinde kayıt kümesi başına kayıt sayısı |20 |
| Tek bir Azure kaynağı için diğer ad kaydı sayısı |20|
| Abonelik başına Özel DNS bölgeleri |1000|
| Özel DNS bölgesi başına kayıt kümeleri |25000|
| Özel DNS bölgeleri için kayıt kümesi başına kayıt sayısı |20|
| Özel DNS bölgesi başına sanal ağ bağlantıları |1000|
| Otomatik kayıt özellikli özel DNS bölgeleri başına sanal ağ bağlantıları |100|
| Bir sanal ağın otomatik kayıt etkin ile bağlantılandırılan özel DNS bölgesi sayısı |1|
| Bir sanal ağın bağlı olduğu özel DNS bölgesi sayısı |1000|
| Bir sanal makinenin Azure DNS çözümleyici 'ye gönderebileceği DNS sorgularının sayısı (saniye başına) |500 <sup>2</sup> |
| Sanal makine başına sıraya alınan en fazla DNS sorgusu sayısı (bekleyen yanıt) |200 <sup>2</sup> |

<sup>1</sup> Bu sınırları artırmanız gerekiyorsa, Azure desteği 'ne başvurun.

<sup>2</sup> Bu sınırlar, sanal ağ düzeyinde değil, her bir sanal makineye uygulanır. Bu sınırları aşan DNS sorguları bırakılır.
