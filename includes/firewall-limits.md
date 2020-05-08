---
title: include dosyası
description: include dosyası
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 05/06/2020
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 5a5f831e72da6bedaf12d3ed82e79f0250a09062
ms.sourcegitcommit: 602e6db62069d568a91981a1117244ffd757f1c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82876000"
---
| Kaynak | Sınır |
| --- | --- |
| Veri aktarım hızı |30 Gbps<sup>1</sup> |
|Kurallar|10.000. Tüm kural türleri birleştirildi.|
|Maksimum DNAT kuralları|298<br>Bir kuralın protokolü TCP ve UDP için yapılandırılırsa, iki kural olarak sayılır.|
|Minimum AzureFirewallSubnet boyutu |/26|
|Ağ ve uygulama kurallarında bağlantı noktası aralığı|1 - 65535|
|Genel IP adresleri|DNAT ve SNAT bağlantı noktaları için en fazla 250.|
|IP grupları IP adresleri|50 IP grubu veya daha az: her güvenlik duvarı örneği için en fazla 5000 ayrı IP adresi.<br>51-100 IP grupları 500: her bir güvenlik duvarı örneği için her biri ayrı IP adresi.<br><br>Daha fazla bilgi için bkz. [Azure Güvenlik duvarında IP grupları (Önizleme)](../articles/firewall/ip-groups.md#ip-address-limits)
|Yol tablosu|Varsayılan olarak, AzureFirewallSubnet, NextHopType değeri **Internet**olarak ayarlanmış bir 0.0.0.0/0 yolu içerir.<br><br>Azure Güvenlik duvarının doğrudan Internet bağlantısı olmalıdır. AzureFirewallSubnet, BGP aracılığıyla şirket içi ağınıza varsayılan bir yol öğrenirse, doğrudan Internet bağlantısını sürdürmek için **Nexthoptype** değeri **Internet** olarak ayarlanmış bir 0.0.0.0/0 UDR ile geçersiz kılmalısınız. Azure Güvenlik Duvarı, varsayılan olarak şirket içi bir ağa Zorlamalı tünel oluşturmayı desteklemez.<br><br>Ancak, yapılandırmanız şirket içi bir ağa Zorlamalı tünel gerektiriyorsa, Microsoft bu servis talebi büyük bir durum temelinde destekleyecektir. Büyük/küçük harf bilgilerinizi gözden geçirebilmemiz için desteğe başvurun. Kabul edilirse, aboneliğinize izin vereceğiz ve gerekli güvenlik duvarı Internet bağlantısının korunduğundan emin olacaksınız.|

<sup>1</sup> Bu sınırları artırmanız gerekiyorsa, Azure desteği 'ne başvurun.
