---
author: rothja
ms.service: azure-resource-manager
ms.topic: include
ms.date: 11/05/2020
ms.author: rohink
ms.openlocfilehash: 7011b92485c56187021c9043ba84bc85e448a98f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94329525"
---
**Genel DNS bölgeleri**

| Kaynak | Sınır |
| --- | --- |
| Abonelik başına genel DNS Bölgeleri |250 <sup>1</sup> |
| Genel DNS bölgesi başına kayıt kümeleri |10.000 <sup>1</sup> |
| Genel DNS bölgesinde kayıt kümesi başına kayıt sayısı |20 |
| Tek bir Azure kaynağı için diğer ad kaydı sayısı |20|

<sup>1</sup> Bu sınırları artırmanız gerekiyorsa, Azure desteği 'ne başvurun.

**Özel DNS bölgeler**

| Kaynak | Sınır |
| --- | --- |
| Abonelik başına Özel DNS bölgeleri |1000|
| Özel DNS bölgesi başına kayıt kümeleri |25000|
| Özel DNS bölgeleri için kayıt kümesi başına kayıt sayısı |20|
| Özel DNS bölgesi başına sanal ağ bağlantıları |1000|
| Otomatik kayıt özellikli özel DNS bölgeleri başına sanal ağ bağlantıları |100|
| Bir sanal ağın otomatik kayıt etkin ile bağlantılandırılan özel DNS bölgesi sayısı |1|
| Bir sanal ağın bağlı olduğu özel DNS bölgesi sayısı |1000|
| Bir sanal makinenin Azure DNS çözümleyici 'ye gönderebileceği DNS sorgularının sayısı (saniye başına) |1000 <sup>1</sup> |
| Sanal makine başına sıraya alınan en fazla DNS sorgusu sayısı (bekleyen yanıt) |200 <sup>1</sup> |

<sup>1</sup> Bu sınırlar, sanal ağ düzeyinde değil, her bir sanal makineye uygulanır. Bu sınırları aşan DNS sorguları bırakılır.
