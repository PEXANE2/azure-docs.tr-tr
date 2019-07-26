---
title: Azure Güvenlik Merkezi 'nde Azure güvenlik ürünleri tümleştirmesi | Microsoft Docs
description: Bu konularda Azure Güvenlik Merkezi ile tümleştirilmiş Azure güvenlik ürünleri sunulmaktadır.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: ad4b0373-08ee-46ca-a573-638ed93a647c
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: 847748d1c56221119d8f74a2aee716ee08448e28
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335823"
---
# <a name="integration-of-azure-security-products-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde Azure güvenlik ürünlerini tümleştirme

Güvenlik Merkezi, müşteri bulgularını Güvenlik Merkezi 'ne eklemek ve bunları birleştirilmiş bir şekilde görüntülemek için ek Microsoft lisansları sunar.

* [Azure WAF](#azure-waf)
* [Azure DDoS](#azure-ddos)

## Azure WAF<a name="azure-waf"></a>

Azure Application Gateway, web uygulamalarınız için açıklardan yararlanmaya ve güvenlik açıklarına karşı merkezi koruma sağlayan bir web uygulaması güvenlik duvarı (WAF) sunar.

Web uygulamaları, yaygın olarak bilinen güvenlik açıklarından yararlanan kötü amaçlı saldırılara karşı giderek daha da yöneliktir. WAF Application Gateway, açık Web uygulaması güvenlik projesinden (OWASP) çekirdek kural kümesi (sp_configure) 3,0 veya 2.2.9 tabanlıdır. WAF, ek yapılandırma gerekmeden yeni güvenlik açıklarına karşı korunmak üzere otomatik olarak güncelleştirilir. WAF tarafından oluşturulan uyarılar Güvenlik Merkezi 'ne akışlardır. WAF tarafından oluşturulan uyarılar hakkında daha fazla bilgi için bu [makaleye](https://docs.microsoft.com/azure/application-gateway/application-gateway-crs-rulegroups-rules?tabs=owasp3#crs911)bakın.

## Azure DDoS<a name="azure-ddos"></a>

Dağıtılmış hizmet reddi (DDoS) saldırılarına kolayca çalıştırılabilir. Bu nedenle, uygulamalarını buluta taşıyan müşteriler için harika bir güvenlik sorunu haline gelmiştir. 

DDoS saldırıları, bir uygulamanın kaynaklarını tüketerek uygulamanın geçerli kullanıcılar için kullanılamaz duruma gelmesini amaçlar. DDoS saldırıları Internet üzerinden erişilebilecek tüm uç noktaları hedefleyebilir.

Azure DDoS koruması, uygulama tasarımı en iyi uygulamaları ile birlikte, DDoS saldırılarına karşı bir savunma sağlar. Azure DDoS koruması farklı hizmet katmanları sağlar. Daha fazla bilgi için bkz. [Azure DDoS korumasına genel bakış](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

DDoS koruma standardı aşağıdaki tür saldırıları hafifletmenize yardımcı olabilir:

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Volumetric saldırısı algılandı**|Bu saldırının amacı, ağ katmanını önemli miktarda meşru trafik ile taşmaktır. Bu, UDP floods, yükseltme floods ve diğer sahte paket floods içerir. DDoS koruma standardı, bu potansiyel çok gigabaytlık saldırıları, Azure 'un küresel ağ ölçeklendirilmesi ve bunları otomatik olarak yaparak, bu olası çok kiracılı saldırıları azaltır.|
|**Protokol saldırısı algılandı**|Bu saldırılar, katman 3 ve katman 4 protokol yığınındaki bir zayıflığın kötüye kullanımı ile bir hedefi işlemez. Bu, SYN taşma saldırıları, yansıma saldırıları ve diğer protokol saldırıları içerir. DDoS koruması standardı, bu saldırıları azaltır, istemciyle etkileşime girerek ve kötü amaçlı trafiği engelleyerek kötü amaçlı ve meşru trafik arasında ayrım gerçekleştirebilir.|
|**Kaynak (uygulama) katmanı saldırısı algılandı**|Bu saldırılar, ana bilgisayarlar arasında veri aktarımını kesintiye uğratan Web uygulaması paketlerini hedefleyebilir. Saldırılara HTTP protokol ihlalleri, SQL ekleme, siteler arası komut dosyası oluşturma ve diğer katman 7 saldırıları dahildir. Bu saldırılara karşı savunmak için DDoS koruma standardı ile Azure Application Gateway Web uygulaması güvenlik duvarı 'nı kullanın. Ayrıca, Azure Marketi 'nde bulunan üçüncü taraf Web uygulaması güvenlik duvarı teklifleri de mevcuttur.|
