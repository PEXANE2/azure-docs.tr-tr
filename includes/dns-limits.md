---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 2/14/2020
ms.author: rohink
ms.openlocfilehash: 3fbbb9d491eef202309c9e64f9a5a839a5d4516d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335068"
---
**Genel DNS bölgeleri**

| Kaynak | Sınır |
| --- | --- |
| Abonelik başına genel DNS Bölgeleri |250 <sup>1</sup> |
| Ortak DNS bölgesi başına kayıt kümeleri |10.000 <sup>1</sup> |
| Genel DNS bölgesinde ayarlanan kayıt başına kayıtlar |20 |
| Tek bir Azure kaynağıiçin Diğer Ad kayıtlarının sayısı |20|
| Abonelik başına özel DNS bölgeleri |1000|
| Özel DNS bölgesi başına kayıt kümeleri |25000|
| Özel DNS bölgeleri için ayarlanan kayıt başına rekorlar |20|
| Özel DNS bölgesi başına Sanal Ağ Bağlantıları |1000|
| Otomatik kayıt etkin özel DNS bölgeleri başına Sanal Ağlar Bağlantılar |100|
| Bir sanal ağın otomatik kayıt etkinleştirilmiş ile bağlanabileceği özel DNS bölgelerinin sayısı |1|
| Sanal bir ağın bağlanabileceği özel DNS bölgeleri sayısı |1000|
| Sanal bir makinenin Azure DNS çözümleyicisine saniyede gönderebileceği DNS sorgusu sayısı |500 <sup>2</sup> |
| Sanal makine başına sıraya alınan (bekleyen yanıt) maksimum DNS sorgusu sayısı |200 <sup>200 2</sup> |

<sup>1.1.2</sup> Bu sınırları artırmanız gerekiyorsa Azure Desteği'ne başvurun.

<sup>2.000</sup> Bu sınırlar sanal ağ düzeyinde değil, her bir sanal makineye uygulanır. Bu sınırları aşan DNS sorguları bırakılır.