---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 2/14/2020
ms.author: rohink
ms.openlocfilehash: 3fbbb9d491eef202309c9e64f9a5a839a5d4516d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80335068"
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