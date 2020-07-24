---
title: Ağ kurallarında Azure Güvenlik duvarı yöneticisi filtreleme (Önizleme)
description: Ağ kurallarında FQDN filtrelemesi kullanma
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: article
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 599620c5fcc3ad1802527bd66e2dbead1b97d11d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079023"
---
# <a name="fqdn-filtering-in-network-rules-preview"></a>Ağ kurallarında FQDN filtrelemesi (Önizleme)

> [!IMPORTANT]
> Ağ kurallarında FQDN filtrelemesi Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Tam etki alanı adı (FQDN), bir konağın etki alanı adını temsil eder. Bir etki alanı adı, tek veya birden çok IP adresi ile ilişkilendirilir. Uygulama kurallarında FQDN ve FQDN etiketlerine izin verebilir veya onları engelleyebilirsiniz. Özel DNS ve DNS proxy ayarlarını kullanarak, ağ kurallarında FQDN filtrelemeyi de kullanabilirsiniz.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure Güvenlik Duvarı, DNS ayarlarını kullanarak FQDN 'yi bir IP adresine çevirir ve Azure DNS veya özel bir DNS yapılandırmasına göre kural işleme yapar.

Ağ kurallarında FQDN 'leri kullanmak için DNS proxy 'yi etkinleştirmelisiniz. DNS proxy 'yi etkinleştirmezseniz güvenilir kural işleme riskidir. Etkin olduğunda DNS trafiği, özel DNS sunucunuzu yapılandırabileceğiniz Azure Güvenlik Duvarı 'na yönlendirilir. Daha sonra güvenlik duvarı ve istemciler aynı yapılandırılmış DNS sunucusunu kullanır. DNS proxy etkinleştirilmemişse, istemci ve güvenlik duvarı ad çözümlemesi için farklı sunucular kullanabileceğinden Azure Güvenlik Duvarı farklı bir yanıt üretebilir. İstemci ve güvenlik duvarı farklı DNS yanıtları alıyorsa ağ kurallarında FQDN filtrelemesi hatalı veya tutarsız olabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Güvenlik duvarı DNS ayarları](dns-settings.md)
