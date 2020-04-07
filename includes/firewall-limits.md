---
title: include dosyası
description: include dosyası
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 04/03/2020
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 4972a80ec7c481ac7bc8860c005c8576ce37a090
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80758127"
---
| Kaynak | Sınır |
| --- | --- |
| Veri aktarım hızı |30 Gbps<sup>1</sup> |
|Kurallar|10,000. Tüm kural türleri birleştirildi.|
|Maksimum DNAT kuralları|298<br>Bir kuralın protokolü hem TCP hem de UDP için yapılandırılırsa, iki kural olarak sayılır.|
|Minimum AzureFirewallSubnet boyutu |/26|
|Ağ ve uygulama kurallarında bağlantı noktası aralığı|0-64,000. Bu sınırlamayı gevşetmek için çalışmalar devam ediyor.|
|Genel IP adresleri|100 maksimum (Şu anda, SNAT bağlantı noktaları yalnızca ilk beş genel IP adresi için eklenir.)|
|IP Grupları IP adresleri|50 IP Grubu veya daha azı: güvenlik duvarı örneği başına her biri maksimum 5000 tek TEK IP adresi.<br>51 - 100 IP Grubu: Güvenlik duvarı örneği başına her biri 500 ayrı IP adresi.<br><br>Daha fazla bilgi için [Azure Güvenlik Duvarı'ndaki IP Grupları 'na (önizleme)](../articles/firewall/ip-groups.md#ip-address-limits) bakın
|Yol tablosu|Varsayılan olarak, AzureFirewallSubnet'in 0.0.0.0/0 rotası vardır ve NextHopType değeri **Internet**olarak ayarlanır.<br><br>Azure Güvenlik Duvarı'nın doğrudan Internet bağlantısı olması gerekir. AzureFirewallSubnet'iniz BGP üzerinden şirket içi ağınıza varsayılan bir rota öğrenirse, doğrudan Internet bağlantısını korumak için **Internet** olarak ayarlanmış **NextHopType** değeriyle 0.0.0.0/0 UDR ile bunu geçersiz kılmanız gerekir. Varsayılan olarak, Azure Güvenlik Duvarı şirket içi ağa zorla tünel kazmayı desteklemez.<br><br>Ancak, yapılandırmanız şirket içi bir ağa zorunlu tünel oluşturma gerektiriyorsa, Microsoft bunu servis talebi bazında destekler. Davanızı inceleyebilmemiz için Destek'e başvurun. Kabul edilirse, aboneliğinize izin verir izninizle gerekli güvenlik duvarı Internet bağlantısının korunmasını sağlarız.|

<sup>1.1.2</sup> Bu sınırları artırmanız gerekiyorsa Azure Desteği'ne başvurun.
