---
title: Azure Web uygulaması güvenlik duvarı 'na giriş
description: Bu makalede, Azure Web uygulaması güvenlik duvarı 'na (WAF) genel bir bakış sunulmaktadır
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 03/06/2020
ms.author: victorh
ms.topic: overview
ms.openlocfilehash: 44bc8db5d8ada0378f8f9d0911ed398ba491d289
ms.sourcegitcommit: f5e4d0466b417fa511b942fd3bd206aeae0055bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78851194"
---
# <a name="what-is-azure-web-application-firewall"></a>Azure Web Uygulaması Güvenlik Duvarı nedir?

Web Uygulaması Güvenlik Duvarı (WAF) web uygulamalarınız için yaygın güvenlik açıklarına ve açıklardan yararlanma işlemlerine karşı merkezi bir koruma sağlar. Web uygulamaları, yaygın olarak bilinen güvenlik açıklarından yararlanan kötü amaçlı saldırılara karşı giderek daha da yöneliktir. SQL ekleme ve siteler arası komut dosyası oluşturma en yaygın saldırı arasındadır.

![WAF 'ye Genel Bakış](media/overview/wafoverview.png)

Uygulama kodundaki saldırıların önlenmesi zor olur. Uygulama topolojisinin birden çok katmanında kapsamlı bakım, düzeltme eki uygulama ve izleme gerektirebilir. Merkezi bir Web uygulaması güvenlik duvarı, güvenlik yönetiminin daha basit olmasına yardımcı olur. Ayrıca, bir WAF, uygulama yöneticilerine tehditler ve yetkisiz erişimlere karşı daha iyi koruma güvence altına almanızı sağlar.

Bir WAF çözümü, her bir Web uygulamasının güvenliğini sağlamak yerine, bilinen bir güvenlik açığına merkezi olarak yama yaparak bir güvenlik tehdidi üzerinde daha hızlı yanıt verebilir.

## <a name="supported-services"></a>Desteklenen hizmetler

WAF, [azure Application Gateway](../application-gateway/overview.md) ve [Azure ön kapılı hizmeti](../frontdoor/front-door-overview.md)ile dağıtılabilir. Her iki hizmet de katman 7 (HTTP/S) yük dengeleyicilerine sahiptir, ancak Application Gateway bölgesel bir hizmettir ve ön kapıdır küresel bir hizmettir. WAF, her belirli hizmet için özelleştirilmiş özelliklere sahiptir.

Daha fazla bilgi için her hizmet için WAF genel bakış bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Application Gateway Web uygulaması güvenlik duvarı hakkında daha fazla bilgi için bkz. [Azure Application Gateway Web uygulaması güvenlik duvarı](./ag/ag-overview.md).
- Azure ön kapısının Web uygulaması güvenlik duvarı hakkında daha fazla bilgi için bkz. [Azure ön kapısı hizmetinde Web uygulaması güvenlik duvarı](./afds/afds-overview.md).
