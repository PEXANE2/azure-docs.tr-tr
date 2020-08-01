---
title: Ağ kurallarında Azure Güvenlik Duvarı FQDN filtrelemesi (Önizleme)
description: Ağ kurallarında Azure Güvenlik Duvarı FQDN filtrelemesi kullanma
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 07/30/2020
ms.author: victorh
ms.openlocfilehash: 6e90a8bc0998b43a84658958215e4b7977f8fdd0
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87461315"
---
# <a name="use-fqdn-filtering-in-network-rules-preview"></a>Ağ kurallarında FQDN filtrelemesi kullanma (Önizleme)

> [!IMPORTANT]
> Ağ kurallarında FQDN filtrelemesi Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Tam etki alanı adı (FQDN), bir konağın veya IP adresinin etki alanı adını temsil eder. Azure Güvenlik Duvarı ve güvenlik duvarı ilkesinde DNS çözümüne bağlı olarak ağ kurallarında FQDN 'leri kullanabilirsiniz. Bu özellik, giden trafiği herhangi bir TCP/UDP protokolüyle (NTP, SSH, RDP ve daha fazlası dahil) filtrelemenizi sağlar. Ağ kurallarınızın FQDN 'lerini kullanması için DNS proxy 'yi etkinleştirmelisiniz. Daha fazla bilgi için bkz. [Azure Güvenlik DUVARı DNS ayarları (Önizleme)](dns-settings.md).

## <a name="how-it-works"></a>Nasıl çalışır?

Kuruluşunuzun ihtiyaç duyacağı DNS sunucusunu (Azure DNS veya kendi özel DNS) tanımladıktan sonra Azure Güvenlik Duvarı, FQDN 'yi seçili DNS sunucusuna bağlı olarak bir IP adresine çevirir. Bu çeviri hem uygulama hem de ağ kuralı işleme için gerçekleşir.

Uygulama kurallarında, ağ kurallarından karşılaştırılan etki alanı adları kullanma arasındaki fark nedir? 

- HTTP/S ve MSSQL için uygulama kurallarında FQDN filtrelemesi, bir uygulama düzeyi saydam proxy ve SNı üst bilgisini temel alır. Bu nedenle, aynı IP adresine çözümlenen iki FQDN arasında bir ayırt edilebilir. Ağ kurallarında FQDN filtrelemede bu durum böyle değildir. Mümkün olduğunda her zaman uygulama kurallarını kullanın.
- Uygulama kuralları ' nda, seçili protokollerinizin bulunduğu HTTP/S ve MSSQL ' yi kullanabilirsiniz. Ağ kuralları ' nda, hedef FQDN 'lerinizle herhangi bir TCP/UDP protokolünü kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Güvenlik duvarı DNS ayarları](dns-settings.md)
