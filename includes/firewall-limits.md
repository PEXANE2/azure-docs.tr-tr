---
title: include dosyası
description: include dosyası
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 01/22/2020
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 0df38533afe97f010d1050c3ee2a4a69a54d4cc7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335094"
---
| Kaynak | Sınır |
| --- | --- |
| Veri aktarım hızı |30 Gbps<sup>1</sup> |
|Kurallar|10,000. Tüm kural türleri birleştirildi.|
|Maksimum DNAT kuralları|299|
|Minimum AzureFirewallSubnet boyutu |/26|
|Ağ ve uygulama kurallarında bağlantı noktası aralığı|0-64,000. Bu sınırlamayı gevşetmek için çalışmalar devam ediyor.|
|Genel IP adresleri|100 maksimum (Şu anda, SNAT bağlantı noktaları yalnızca ilk beş genel IP adresi için eklenir.)|
|Yol tablosu|Varsayılan olarak, AzureFirewallSubnet'in 0.0.0.0/0 rotası vardır ve NextHopType değeri **Internet**olarak ayarlanır.<br><br>Azure Güvenlik Duvarı'nın doğrudan Internet bağlantısı olması gerekir. AzureFirewallSubnet'iniz BGP üzerinden şirket içi ağınıza varsayılan bir rota öğrenirse, doğrudan Internet bağlantısını korumak için **Internet** olarak ayarlanmış **NextHopType** değeriyle 0.0.0.0/0 UDR ile bunu geçersiz kılmanız gerekir. Varsayılan olarak, Azure Güvenlik Duvarı şirket içi ağa zorla tünel kazmayı desteklemez.<br><br>Ancak, yapılandırmanız şirket içi bir ağa zorunlu tünel oluşturma gerektiriyorsa, Microsoft bunu servis talebi bazında destekler. Davanızı inceleyebilmemiz için Destek'e başvurun. Kabul edilirse, aboneliğinize izin verir izninizle gerekli güvenlik duvarı Internet bağlantısının korunmasını sağlarız.|

<sup>1.1.2</sup> Bu sınırları artırmanız gerekiyorsa Azure Desteği'ne başvurun.
