---
title: WAF üzerinde Azure Application Gateway bot koruma genel bakış
titleSuffix: Azure Web Application Firewall
description: Bu makale, Uygulama Ağ Geçidi bot koruması web uygulama güvenlik duvarı (WAF) genel bir bakış sağlar
services: web-application-firewall
author: winthrop28
ms.service: web-application-firewall
ms.date: 02/04/2020
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 3bc481cfc35ac94699d2795862f1fe8e4decf875
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77027101"
---
# <a name="azure-web-application-firewall-on-azure-application-gateway-bot-protection-overview"></a>Azure Uygulama Ağ Geçidi bot koruma genel bakış Azure Web Uygulama Güvenlik Duvarı

Tüm Internet trafiğinin kabaca% 20 kötü botlar geliyor. Web uygulamanızda kazıma, tarama ve güvenlik açıklarını arama gibi şeyler yaparlar. Bu botlar Web Uygulaması Güvenlik Duvarı'nda (WAF) durdurulduğunda, size saldıramaz. Ayrıca, arka uçlarınız ve diğer temel altyapınız gibi kaynaklarınızı ve hizmetlerinizi kullanamazlar.

WAF'Nizin bilinen kötü amaçlı IP adreslerinden gelen istekleri engellemek veya günlüğe kaydetmek için yönetilen bir bot koruma kuralını etkinleştirebilirsiniz. IP adresleri Microsoft Threat Intelligence akışından kaynaklanmaktadır. Akıllı Güvenlik Grafiği Microsoft tehdit zekasına güç sağlar ve Azure Güvenlik Merkezi de dahil olmak üzere birden çok hizmet tarafından kullanılır.

> [!IMPORTANT]
> Bot koruma kuralı kümesi şu anda genel önizlemededir ve bir önizleme hizmeti düzeyi sözleşmesiyle sağlanır. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar [için Microsoft Azure Önizlemeleri](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) için Ek Kullanım Koşulları'na bakın.

## <a name="use-with-owasp-rulesets"></a>OWASP kural kümeleri ile kullanın

Bot Koruma kural kümesini, OWASP kural kümelerinden herhangi birinin (2.2.9, 3.0 ve 3.1) yanında kullanabilirsiniz. Herhangi bir zamanda yalnızca bir OWASP kural kümesi kullanılabilir. Bot koruma kural kümesi, kendi kural kümesinde görünen ek bir kural içerir. Bu **Microsoft_BotManagerRuleSet_0.1**başlıklı ve etkinleştirmek veya diğer OWASP kuralları gibi devre dışı kullanabilirsiniz.

![Bot kural seti](../media/bot-protection-overview/bot-ruleset.png)

## <a name="ruleset-update"></a>Kural kümesi güncelleştirmesi

Bilinen kötü IP adreslerinin bot azaltma kural kümesi listesi, botlarla eşit kalmak için Microsoft Threat Intelligence akışından günde birden çok kez güncellenir. Bot saldırı vektörleri değişse bile web uygulamalarınız sürekli olarak korunur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Uygulama Ağ Geçidi'nde Web Uygulaması Güvenlik Duvarı için bot koruma yapılandırma (Önizleme)](bot-protection.md)
