---
title: Azure Güvenlik Merkezi 'nde Azure güvenlik ürünleri için tehdit algılama
description: Bu konuda, Azure Güvenlik Merkezi 'nin tehdit algılama sağlayabileceği Azure güvenlik ürünleri sunulmaktadır
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ad4b0373-08ee-46ca-a573-638ed93a647c
ms.service: security-center
ms.topic: conceptual
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: 1ea207f0ba09e0637a08632d5c56591fd1335b22
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75665713"
---
# <a name="threat-detection-for-azure-waf-and-azure-ddos-protection"></a>Azure WAF ve Azure DDoS Koruması için tehdit algılama

Azure Güvenlik Merkezi, aşağıdaki Azure güvenlik ürünleri için tehdit algılama sağlayabilir (her bir ürün için ayrı bir lisans gerekir):

* [Azure WAF](#azure-waf)
* [Azure DDoS Koruması](#azure-ddos)

## Azure WAF<a name="azure-waf"></a>

Azure Application Gateway, web uygulamalarınız için açıklardan yararlanmaya ve güvenlik açıklarına karşı merkezi koruma sağlayan bir web uygulaması güvenlik duvarı (WAF) sunar.

Web uygulamaları, yaygın olarak bilinen güvenlik açıklarından yararlanan kötü amaçlı saldırılara karşı giderek daha da yöneliktir. WAF Application Gateway, açık Web uygulaması güvenlik projesinden 3,0 veya 2.2.9 temel kural kümesini temel alır. WAF, ek yapılandırma gerekmeden yeni güvenlik açıklarına karşı korunmak üzere otomatik olarak güncelleştirilir. WAF uyarıları Güvenlik Merkezi 'ne akışla kaydedilir. WAF tarafından oluşturulan uyarılar hakkında daha fazla bilgi için bkz. [Web uygulaması güvenlik duvarı Ayrıntılar kural grupları ve kuralları](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md?tabs=owasp31#crs911-31).

## Azure DDoS koruması<a name="azure-ddos"></a>

Dağıtılmış hizmet reddi (DDoS) saldırılarına kolayca çalıştırılabilir. Özellikle uygulamalarınızı buluta taşıyorsanız harika bir güvenlik sorunu haline gelmiştir. 

DDoS saldırıları, bir uygulamanın kaynaklarını tüketerek uygulamanın geçerli kullanıcılar için kullanılamaz duruma gelmesini amaçlar. DDoS saldırıları Internet üzerinden erişilebilecek tüm uç noktaları hedefleyebilir.

Azure DDoS koruması, uygulama tasarımı en iyi uygulamaları ile birlikte, DDoS saldırılarına karşı bir savunma sağlar. DDoS koruması farklı hizmet katmanları sağlar. Daha fazla bilgi için bkz. [Azure DDoS korumasına genel bakış](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

Azure DDoS koruması uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azureddos)bakın.