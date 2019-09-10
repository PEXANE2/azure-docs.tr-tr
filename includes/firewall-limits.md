---
title: include dosyası
description: include dosyası
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 7/16/2019
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: a42284765a46f4a000dc5b7fcf2867ef17d69570
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "68229345"
---
| Resource | Varsayılan limit |
| --- | --- |
| Veri işleme |30 Gbps<sup>1</sup> |
|Kurallar|10.000, tüm kural türleri birleştirildi.|
|Minimum AzureFirewallSubnet boyutu |/26|
|Ağ ve uygulama kurallarında bağlantı noktası aralığı|0-64000. Bu kısıtlamayı rahat hale getirme işi devam ediyor.|
|Genel IP adresleri|100 en fazla|
|Rota tablosu|Varsayılan olarak, AzureFirewallSubnet, NextHopType değeri **Internet**olarak ayarlanmış bir 0.0.0.0/0 yolu içerir.<br><br>Azure Güvenlik duvarının doğrudan Internet bağlantısı olmalıdır. AzureFirewallSubnet, BGP aracılığıyla şirket içi ağınıza varsayılan bir yol öğrenirse, doğrudan Internet bağlantısını sürdürmek için **Nexthoptype** değeri **Internet** olarak ayarlanmış bir 0.0.0.0/0 UDR ile geçersiz kılmalısınız. Azure Güvenlik Duvarı, varsayılan olarak şirket içi bir ağa Zorlamalı tünel oluşturmayı desteklemez.<br><br>Ancak, yapılandırmanız şirket içi bir ağa Zorlamalı tünel gerektiriyorsa, Microsoft bu servis talebi büyük bir durum temelinde destekleyecektir. Büyük/küçük harf bilgilerinizi gözden geçirebilmemiz için desteğe başvurun. Kabul edilirse, aboneliğinize izin vereceğiz ve gerekli güvenlik duvarı Internet bağlantısının korunduğundan emin olacaksınız.|

<sup>1</sup> Bu sınırları artırmanız gerekiyorsa, Azure desteği 'ne başvurun.
