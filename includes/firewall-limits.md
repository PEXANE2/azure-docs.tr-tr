---
title: dosya dahil etme
description: dosya dahil etme
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 06/22/2020
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: fe223030b98f95036f01cf69babdeb8a9a84dc2d
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85242185"
---
| Kaynak | Sınır |
| --- | --- |
| Veri işleme |30 Gbps<sup>1</sup> |
|Kurallar|10.000. Tüm kural türleri birleştirildi.|
|Maksimum DNAT kuralları|tek bir genel IP adresi için 298.<br>Ek genel IP adresleri kullanılabilir SNAT bağlantı noktalarına katkıda bulunur, ancak kullanılabilir DNAT kurallarının sayısını azaltır. Örneğin, 297 DNAT kurallarına izin veren iki genel IP adresi vardır. Bir kuralın protokolü TCP ve UDP için yapılandırılırsa, iki kural olarak sayılır.|
|Minimum AzureFirewallSubnet boyutu |/26|
|Ağ ve uygulama kurallarında bağlantı noktası aralığı|1 - 65535|
|Genel IP adresleri|250 en fazla. Tüm genel IP adresleri DNAT kurallarında kullanılabilir ve hepsi, kullanılabilir SNAT bağlantı noktalarına katkıda bulunur.|
|IP gruplarındaki IP adresleri|50 IP grubu veya daha az: her güvenlik duvarı örneği için en fazla 5000 ayrı IP adresi.<br>51-100 IP grupları 500: her bir güvenlik duvarı örneği için her biri ayrı IP adresi.<br><br>Daha fazla bilgi için bkz. [Azure Güvenlik duvarında IP grupları (Önizleme)](../articles/firewall/ip-groups.md#ip-address-limits)
|Yol tablosu|Varsayılan olarak, AzureFirewallSubnet, NextHopType değeri **Internet**olarak ayarlanmış bir 0.0.0.0/0 yolu içerir.<br><br>Azure Güvenlik duvarının doğrudan Internet bağlantısı olmalıdır. AzureFirewallSubnet, BGP aracılığıyla şirket içi ağınıza varsayılan bir yol öğrenirse, doğrudan Internet bağlantısını sürdürmek için **Nexthoptype** değeri **Internet** olarak ayarlanmış bir 0.0.0.0/0 UDR ile geçersiz kılmalısınız. Azure Güvenlik Duvarı, varsayılan olarak şirket içi bir ağa Zorlamalı tünel oluşturmayı desteklemez.<br><br>Ancak, yapılandırmanız şirket içi bir ağa Zorlamalı tünel gerektiriyorsa, Microsoft bu servis talebi büyük bir durum temelinde destekleyecektir. Büyük/küçük harf bilgilerinizi gözden geçirebilmemiz için desteğe başvurun. Kabul edilirse, aboneliğinize izin vereceğiz ve gerekli güvenlik duvarı Internet bağlantısının korunduğundan emin olacaksınız.|

<sup>1</sup> Bu sınırları artırmanız gerekiyorsa, Azure desteği 'ne başvurun.
