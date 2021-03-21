---
title: Ağ kurallarında Azure Güvenlik Duvarı FQDN filtrelemesi
description: Ağ kurallarında Azure Güvenlik Duvarı FQDN filtrelemesi kullanma
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/06/2020
ms.author: victorh
ms.openlocfilehash: f7196c7715ad5d2c02759040b780b96218e1655e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94695954"
---
# <a name="use-fqdn-filtering-in-network-rules"></a>Ağ kurallarında FQDN filtrelemesi kullanma

Tam etki alanı adı (FQDN), bir konağın veya IP adresinin etki alanı adını temsil eder. Azure Güvenlik Duvarı ve güvenlik duvarı ilkesinde DNS çözümüne bağlı olarak ağ kurallarında FQDN 'leri kullanabilirsiniz. Bu özellik, giden trafiği herhangi bir TCP/UDP protokolüyle (NTP, SSH, RDP ve daha fazlası dahil) filtrelemenizi sağlar. Ağ kurallarınızın FQDN 'lerini kullanması için DNS proxy 'yi etkinleştirmelisiniz. Daha fazla bilgi için bkz. [Azure Güvenlik DUVARı DNS ayarları](dns-settings.md).

> [!NOTE]
> Tasarıma göre, FQDN filtrelemesi joker karakterleri desteklemez.

## <a name="how-it-works"></a>Nasıl çalışır?

Kuruluşunuzun ihtiyaç duyacağı DNS sunucusunu (Azure DNS veya kendi özel DNS) tanımladıktan sonra Azure Güvenlik Duvarı, FQDN 'yi seçili DNS sunucusuna bağlı olarak bir IP adresine çevirir. Bu çeviri hem uygulama hem de ağ kuralı işleme için gerçekleşir.

Yeni bir DNS çözümlemesi gerçekleştiğinde, güvenlik duvarı kurallarına yeni IP adresleri eklenir. Artık DNS sunucusu tarafından döndürülen eski IP adreslerinin süresi 15 dakika içinde doluyor. Azure Güvenlik duvarı kuralları, ağ kurallarında FQDN 'lerin DNS çözümünden 15 saniyede bir güncelleştirilir.

### <a name="differences-in-application-rules-vs-network-rules"></a>Uygulama kuralları ile ağ kuralları arasındaki farklılıklar

- HTTP/S ve MSSQL için uygulama kurallarında FQDN filtrelemesi, bir uygulama düzeyi saydam proxy ve SNı üst bilgisini temel alır. Bu nedenle, aynı IP adresine çözümlenen iki FQDN arasında bir ayırt edilebilir. Ağ kurallarında FQDN filtrelemede bu durum böyle değildir. 

   Mümkün olduğunda her zaman uygulama kurallarını kullan:
     - Protokol HTTP/S veya MSSQL ise, FQDN filtrelemesi için uygulama kurallarını kullanın.
   - HTTP/S veya MSSQL 'nin yanı sıra diğer protokoller için, FQDN filtrelemesi için uygulama veya ağ kurallarını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Güvenlik duvarı DNS ayarları](dns-settings.md)
